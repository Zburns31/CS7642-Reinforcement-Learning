# Problems with Traditional RL for MARL Environments

- Unfortunately, traditional reinforcement learning approaches such as Q-Learning or policy gradient methods are poorly suited to multi-agent environments
    - One issue is that each agent’s policy is changing as training progresses, and the environment becomes non-stationary from the perspective of any individual agent (in a way that is not explainable by changes in the agent’s own policy)
        - This is because the policies for a given agent are partially dependent upon other agents and their policies will also change over time creating a non-stationary environment

# Multi-Agent Deep Deterministic Policy Gradient (MADDPG)

- The paper proposes a general-purpose multi-agent learning algorithm that:
    1. Leads to learned policies that only use local information (i.e. their own observations) at execution time
    2. Does not assume a differentiable model of the environment dynamics or any particular structure on the communication method between agents
    3. Is applicable not only to cooperative interaction but to competitive or mixed interaction involving both physical and communicative behaviour

# Framework

- Centralized training with decentralized execution, allowing the policies to use extra information to ease training, so long as this information is not used at test time
- We use a simple extension of actor-critic policy gradient methods where the critic is augmented with extra information about the policies of other agents, while the actor only has access to local information
    - After training is completed, only the local actors are used at execution phase, acting in a decentralized manner and equally applicable in cooperative and competitive settings

# Components

- **Centralized Planning:** Each agent only has direct access to local observations
    - These observations can be many things: an image of the environment, relative positions to landmarks, or even relative positions of other agents. Also, during learning, all agents are **guided by a centralized module or critic**
    - Even though each agent only has local information and local policies to train, there is an entity overlooking the *entire system of agents,* advising them on how to update their policies
        - This reduces the effect of non-stationarity
- **Decentralized Execution:** Then, during testing, the centralized module is removed, leaving only the agents, their policies, and local observations
    - This reduces the detriments of increasing state and action space because *joint* policies are never explicitly learned
    - Instead, we hope that the central module has given enough information to guide *local* policy training such that it is optimal for the entire system once test time comes around

# MADDPG

- The basic idea here is to use centralized training with decentralized execution
    - Thus, we allow the policies to use extra information to ease training, so long as this information is not used at test time

- The algorithm, called Multi-Agent Deep Deterministic Policy Gradient (MADDPG), extends the actor-critic framework to the multi-agent setting by maintaining a centralized critic that can access the states and actions of all agents, while each agent has its own local actor that selects actions based on its own observations
    - MADDPG is able to learn both cooperative and competitive behaviours in a mixed environment by using a novel approach to credit assignment, called the ***centralized training with decentralized execution (CTDE) scheme***
        - This allows the agents to learn from each other's experiences during training, while still maintaining decentralized execution during testing

![Untitled](./Multi-Agent%20Actor-Critic%20for%20Mixed%20Cooperative-Competitive%20Environments/Untitled.png)

- A primary motivation behind MADDPG is that, if we know the actions taken by all agents, the environment is stationary even as the policies change

# Overview

- **Centralized Critic, Decentralized Actor:** In MADDPG, each agent has its own local actor that selects actions based on its own observations, while a centralized critic estimates the expected return based on the joint actions and observations of all agents
    - This allows the agents to learn from each other's experiences during training, while still maintaining decentralized execution during testing
- **Experience Replay:** To increase the efficiency of learning, MADDPG uses experience replay, a technique where the agents store and sample from a buffer of previous experiences, allowing them to learn from past experiences and avoid learning from highly correlated samples
- **Target Networks:** MADDPG uses target networks, which are copies of the actor and critic networks that are periodically updated with the weights of the main networks
    - This stabilizes the learning process by preventing the targets from changing too rapidly and helps prevent the algorithm from overfitting to the training data
- **Exploration:** MADDPG uses noise added to the agents' actions to encourage exploration of the environment and prevent the agents from getting stuck in local optima
- **Centralized Training with Decentralized Execution (CTDE):** MADDPG uses a novel approach to credit assignment called the CTDE scheme, where the centralized critic is used during training to estimate the value of joint actions and observations, while each agent's local actor is used during execution to select actions based on its own observations
    - This allows the agents to learn how to coordinate with each other during training, while still maintaining decentralized execution during testing


# How it works

- Every agent has an observation space and **continuous action space**. Also, each agent has three components:
    - An actor-network that uses *local* observations for *deterministic* actions
    - A target actor-network with identical functionality for training stability
    - A critic-network that uses *joint* states action pairs to estimate Q-values
- As the critic learns the *joint* Q-value function over time, it sends appropriate Q-value approximations to the actor to help training

## Learning

- First, MADDPG uses an experience replay for efficient off-policy training. At each time step, the agent stores the following transition:

$$
(x, x', a_1, ...., a_N, r_1, ...., r_n)
$$

- where we store the joint state, next joint state, joint action, and each of the agents’ received rewards. Then, we sample a batch of these transitions from the experience replay to train our agent

## Critic Updates

- To update an agent’s centralized critic, we use a one-step lookahead TD-error:

$$
\mathcal{L}(\theta_i) = E_{x,a,r, x'} \Big [(Q_i^\mu(x,a_1, ..., a_N) - y)^2 \Big], \space y = r_i + \gamma Q_i^\mu(x', a'_1, ..., a'_N) | a'_j = \mu'_j(o_j)
$$

- where $\mu$ denotes the actor. Keep in mind that this is a *centralized critic,* meaning it uses joint information to update its parameters
    - The primary motivation is that **knowing the actions taken by all agents makes the environment stationary even when policies change**
- Notice the calculation of our target Q-value on the right. Even though we never explicitly store *the next joint actions,* we use each of the agent’s *target actor* to compute this next action during the update to help in training stability
    - The target actor’s parameters are updated periodically to match the agent’s actor parameters

## Actor Updates

- Similar to single-agent DDPG, we use the deterministic policy gradient to update each of the agent’s actor parameters

$$
\nabla_{\theta_i} J(\mu_i) = E_{x,a, \sim D} \Big [\nabla_{\theta_i} \mu_i(a_i|o_i)\nabla_{a_i}Q_i^\mu(x,a_1,...,a_N)| a_i = \mu_i(o_i)
$$

- where $\mu$ denotes an agent’s actor
- We take the gradient with respect to the *actor’s parameters* using a *central critic* to guide us
    - The most important thing to notice is that even though the actor only has local observations and actions, we use a centralized critic during training time, providing information about the optimality of its actions for the entire system
        - This reduces the effects of nonstationarity while keeping policy learning at a lower state space


## Policy Inference Policy Ensembles

- We can take decentralization one step further. In earlier critic updates, we assumed each agent *automatically* knew other agents’ actions
- To remove the assumption of knowing other agents’ policies, each agent $i$ can maintain an approximation of $\hat \mu_{\phi^j_i}$ (where $\phi$ represents the parameters of the approximation) to the true policy gradient of agent $j$, $\mu_j$
- However, MADDPG suggests **inferring other agents’ policies** to make learning even more independent. In effect, each agent adds *N-1* more networks to estimate the true policy of each of the other agents
    - We use a probabilistic network and maximize the log probability of outputting another agent’s observed action

    $$
    \mathcal{L}(\phi^j_i) = - E_{o_j, a_j} \Big [\log \hat \mu^j_i(a_j|o_j) + \lambda H(\hat \mu^j_i) \Big ]
    $$

- $H$ is the entropy of the policy distribution
- Where we show the loss function for the $i$th agent estimating the $j$th agent’s policy with an entropy regularizer
    - As a result, our Q-value target becomes a slightly different value as we replace agent actions with our predicted action

    $$
    \hat y = r_i + \gamma Q^{\mu'}_i(x', \hat \mu^{'1}_i(o_1), ...\mu^{'}_i(o_i), ..., \hat \mu^{'N}_i(o_1)(o_N))
    $$

    - So, what exactly have we done? We’ve removed any assumption that agents know each other’s policies
        - Instead, we try to train agents to **correctly predict other policies through a series of observations**
        - In effect, each agent is trained independently by *extracting global information* from the environment instead of automatically having it on hand
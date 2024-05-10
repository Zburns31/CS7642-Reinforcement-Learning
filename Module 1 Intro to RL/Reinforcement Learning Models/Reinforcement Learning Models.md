# Elements of RL

**Any reinforcement learning problem includes the following elements:**

1. **Model** **of the Environment (Optional) -** This is something that mimics the behaviour of the environment, or allows inferences to be made about how the environment will behave
2. **Rewards** – This gives us a score of how the algorithm performs with respect to the environment
    1. It’s represented as 1 or 0. ‘1’ means that the policy network made the right move, ‘0’ means wrong move. In other words, rewards represent gains and losses

    [More About Rewards (Utilities)](../More%20About%20Rewards/More%20About%20Rewards%20(Utilities).md)

3. **Policy** – The algorithm used by the agent to decide its actions. This is the part that can be model-based or model-free
    1. The policy is the core of a RL agent in the sense that it alone is sufficient to determine behaviour. In general, policies may be stochastic
4. **Value Function -** A value function specifies what is good in the long run
    1. The value of a state is the total amount of reward an agent can expect to accumulate over the future, starting from that state
        1. Whereas rewards determine the immediate, intrinsic desirability of environmental states, values indicate the long-term desirability of states after taking into account the states that are likely to follow and the rewards available in those states ****

# RL Model Types

1. **Policy-Based Approach**: In policy-based reinforcement learning, there is a policy which is needed to be optimized. The policy basically defines how the agent behaves
    1. A policy function is learned which helps in mapping each state to the best action
    2. Has no value function, uses the policy function to pick actions
    3. Getting deep into policies, we further divide policies into two types:
        1. **Deterministic**: a policy at a given state(s) will always return the same action(a)
        2. **Stochastic**: It gives a distribution of probability over different actions

2. **Value-Based Approach**: In value-based RL, the goal of the agent is to optimize the value function ***V(s)*** which is defined as a function that tells us the maximum expected future reward the agent shall get at each state
    1. The value of each state is the total amount of the reward an RL agent can expect to collect over the future, from a particular state
    2. The agent will use the above value function to select which state to choose at each step. The agent will always take the state with the biggest value
    3. Has no policy, picks actions greedily based on state values

3. **Policy Search**
    1. In some ways, policy search is the simplest of all the methods in this chapter: the idea is to keep twiddling the policy as long as its performance improves, then stop

### Model-Based Vs. Model Free

1. **Model-Based Approach**: In model-based RL, the environment is modelled. This means a model of the behaviour of the environment is created. The problem is each environment will need a different model representation
    1. Uses policy and/or value functions and has a model
    2. Examples:
        1. Value & Policy Iteration
    3. ***Can use this when the transition and reward functions are known***
    4. Model-Based methods rely on ***planning*** on their primary component
        1. [Good Resource](https://ai.stackexchange.com/questions/10615/what-is-planning-in-the-context-of-reinforcement-learning-and-how-is-it-diffe)

2. **Value Function** (**Model-Free) Based Approach**
    1. ***Algorithms that purely sample from experience***
        1. They rely on real samples from the environment and never use generated predictions of next state and next reward to alter behaviour (although they might sample from experience memory, which is close to being a model)
        2. [Reference](https://ai.stackexchange.com/questions/4456/whats-the-difference-between-model-free-and-model-based-reinforcement-learning)
    2. In these approaches the agent neither knows nor learns a transition model for the environment. Instead, it learns a more direct representation of how to behave. This comes in one of two ways:
        1. **Action-Utility Learning**:
            1. I.e. Q-Learning, where the agent learns a Q-function, or quality function, $Q(s,a)$, denoting the sum of rewards from state s onward if action a is taken in order to generate a policy
                1. This in comparison to learning either $U^\pi(s)$ or $\pi(s)$ that other algorithms do
        2. **Methods:**
            1. Q Learning
            2. SARSA
            3. Actor Critic
                1. Uses both value and policy functions
            4. Monte Carlo Control
    3. Uses policy and/or value functions but has *no* model
    4. Model-Free methods rely on ***learning*** on their primary component

- Since an RL agent does not use the true environment model, we call such RL learners "Model Free"
    - However, it is possible for an RL learner to "infer" an environment model (Transition and Rewards tables) from its observations. When an RL learner infers an internal model, we call this approach "Model-Based”

![Untitled](./Reinforcement%20Learning%20Models/Untitled.png)

# Key Concepts: RL

## On Policy Vs. Off Policy

- **Off Policy**
    - In Q-Learning, the agent learns optimal policy with the help of a greedy policy and behaves using policies of other agents
        - Q-learning is called off-policy because the updated policy is different from the behavior policy, so Q-Learning is off-policy. In other words, it estimates the reward for future actions and appends a value to the new state without actually following any greedy policy
            - The reason that Q-learning is off-policy is that it updates its Q-values using the Q-value of the next state *𝑠*′ and the *greedy action 𝑎*′.  In other words, it estimates the *return* (total discounted future reward) for state-action pairs assuming a greedy policy were followed despite the fact that it's not following a greedy policy
        - In **off-policy** learning, the *𝑄*(*𝑠*,*𝑎*) function is learned from taking different actions (for example, random actions)
- **On Policy**
    - SARSA (state-action-reward-state-action) is an on-policy reinforcement learning algorithm that estimates the value of the policy being followed
    - In this algorithm, the agent grasps the optimal policy and uses the same to act. The policy that is used for updating and the policy used for acting is the same, unlike in Q-learning
    - In **on-policy** learning, the *𝑄*(*𝑠*,*𝑎*) function is learned from actions that we took using our current policy *𝜋*(*𝑎*|*𝑠*)

## Stochastic Vs. Deterministic Policies

- A deterministic policy maps each state to a single action
    - Given the same state, a deterministic policy always chooses the same action
        - For example, a deterministic policy for playing a game of chess might always choose to move a certain piece to a specific square when a particular board configuration is encountered
- In contrast, a stochastic policy maps each state to a probability distribution over actions
    - Given the same state, a stochastic policy may choose different actions with some probability
        - For example, a stochastic policy for playing a game of poker might choose to fold, call, or raise with different probabilities, depending on the cards held and the previous actions of the other players
- The main advantage of a stochastic policy is that it can encourage exploration and enable the agent to discover new and potentially better actions
    - By choosing actions randomly or probabilistically, a stochastic policy can help the agent to explore the environment and gather information about which actions are most effective in different situations
    - On the other hand, a deterministic policy can be more effective in exploiting known good actions and achieving high performance once a good policy has been learned
    - Deterministic policies can also be simpler and more efficient to compute than stochastic policies

## Stationary Vs. Non-Stationary Environments

- A Stationary environment refers to the static model of the system. The model comprises of a Reward function and Transition probabilities
    - ***So, in a stationary environment, the reward function and transition probabilities remain constant or the changes are slow enough that the agent finds enough training time to learn the changes done in the environment***
- **Stationary:** A stationary environment is one where the probabilities of transitioning from one state to another and the rewards associated with those transitions remain constant over time
    - In other words, the environment does not change regardless of the number of times the agent interacts with it. An example of a stationary environment is a game of chess, where the rules and the outcome of every move are fixed
- **Non-Stationary:** On the other hand, a non-stationary environment is one where the probabilities of transitions and rewards can change over time
    - In other words, the environment is dynamic and can evolve over time
    - For example, a stock market can be considered a non-stationary environment as the value of the stocks changes over time due to various factors
- The difference between stationary and non-stationary environments is important in reinforcement learning because it affects the optimal strategy that the agent should follow
    - In a stationary environment, the optimal strategy can be computed once and applied indefinitely
    - However, in a non-stationary environment, the optimal strategy may need to be updated over time to adapt to the changing dynamics of the environment
- [Resource](https://ai.stackexchange.com/questions/7640/what-does-stationary-mean-in-the-context-of-reinforcement-learning)

## Experience Replay

[Experience Replay](Reinforcement%20Learning%20Models%20de856fe593b246a0b39a473f2d55f751/Experience%20Replay%20d241680545f84fbd8c3459ed97523997.md)

## Relationship Between State and Action Value in RL

- **State Value Function:** Is the expected return (cumulative reward)starting from the state s following policy $\pi$
- **Action Value Function:** The expected return (cumulative reward) starts from state s, following policy π, taking action a
- [Good explanation](https://medium.com/intro-to-artificial-intelligence/relationship-between-state-v-and-action-q-value-function-in-reinforcement-learning-bb9a988c0127)

## Exploration Vs. Exploitation Tradeoff

- *The exploration-exploitation trade-off is a fundamental dilemma whenever you learn about the world by trying things out*
- *The dilemma is between choosing what you know and getting something close to what you expect (‘exploitation’) and choosing something you aren’t sure about and possibly learning more (‘exploration’)*
- Some considerations about this tradeoff include:
    - Variance in the rewards
    - Task/Environment is ***non-stationary***
        - Meaning that the optimal action in a state will change over time
    - Desired time to find optimal policy

[Exploration - Exploitation Strategies](Reinforcement%20Learning%20Models%20de856fe593b246a0b39a473f2d55f751/Exploration%20-%20Exploitation%20Strategies%20bd6bcd5c228c42b981be1e4e135a872b.md)

## Passive Reinforcement Learning

***The goal of the agent is to evaluate how good a policy is, the agent needs to learn the expected utility $U^\pi(s)$ for each state s***

- The agent’s policy is fixed which means that it is told what to do

**This can be done in three ways:**

- [Direct Utility Evaluation](./Reinforcement%20Learning%20Models/Direct%20Utility%20Evaluation.md)

- [Adaptive Dynamic Programming (ADP)](./Reinforcement%20Learning%20Models/Adaptive%20Dynamic%20Programming%20(ADP).md)

- [Temporal Difference Learning (TD)](./Reinforcement%20Learning%20Models/Temporal%20Difference%20Learning%20(TD).md)

## Active Reinforcement Learning

***The goal of an active agent is to learn an optimal policy, the agent needs to learn the expected utility of each state and update its policy***

- The agent needs to decide what to do as there’s no fixed policy that it can act on
- The agent will need to learn a complete transition model with outcome probabilities for all actions, rather than just the model for the fixed policy

## Generalization in RL

- So far, we have assumed that utility functions and Q-functions are represented in tabular form with one output value for each state
    - This works for state spaces with up to about $10^6$ states, which is a lot lower than many real world problems

- [Generalization in RL](./Reinforcement%20Learning%20Models/Generalization%20in%20RL.md)

## Hierarchical RL

- Hierarchical reinforcement learning (HRL) decomposes a reinforcement learning problem into a hierarchy of subproblems or subtasks such that higher-level parent-tasks invoke lower-level child tasks as if they were primitive actions

- [Hierarchical RL](./Reinforcement%20Learning%20Models/Hierarchical%20RL.md)

## Apprenticeship Learning & Inverse RL

- The general field of apprenticeship learning studies the process of learning how to behave well given observations of expert behaviour
    - There are two main ways to approach this type of problem:
        1. **Imitation Learning:** Assuming the environment is observable, we apply supervised learning to the observed state-action pairs to learn a policy $\pi(s)$
            1. Suffers from brittleness where even small derivations from the training set lead to errors that grow over time
            2. Won’t exceed the teachers performance
        2. **Inverse Reinforcement Learning**
            1. Learning rewards by observing a policy, rather than learning a policy by observing rewards
            2. Observe the experts action (and resulting states) and try to work out what reward function the expert is maximizing
            3. Try to understand why it should perform any given action

# RL Model Deep Dives

![Untitled](./Reinforcement%20Learning%20Models/Untitled%201.png)

## Policy Optimization

- [Policy Search](./Reinforcement%20Learning%20Models/Policy%20Search.md)
- [Policy Gradient Methods](./Reinforcement%20Learning%20Models/Policy%20Gradient%20Methods.md)
- [Actor-Critic Methods](./Reinforcement%20Learning%20Models/Actor-Critic%20Methods.md)

## Off-Policy Learning

- [Q Learning, Double Q Learning, Dyna Q](./Reinforcement%20Learning%20Models/Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q.md)
- [Temporal Difference Q Learning](./Reinforcement%20Learning%20Models/Temporal%20Difference%20Q%20Learning.md)
- [SARSA, Expected SARSA & N-Step SARSA](./Reinforcement%20Learning%20Models/SARSA,%20Expected%20SARSA%20&%20N-Step%20SARSA.md)

## Model Based

- [Prioritized Sweeping](./Reinforcement%20Learning%20Models/Prioritized%20Sweeping.md)

## Fundamental Methods

- [Multi-Armed Bandits](./Reinforcement%20Learning%20Models/Multi-Armed%20Bandits.md)

## Multi-Agent RL (MARL)

- [Multi-Agent RL (MARL)](./Reinforcement%20Learning%20Models/Multi-Agent%20RL%20(MARL).md)

# Key Terms

- **Sparse Rewards:** A sparse reward task is typically characterized by a meagre amount of states in the state space that return a feedback signal
    - A typical situation is a situation where an agent has to reach a goal and only receives a positive reward signal when he is close enough to the target
- **Greedy in the Limit (GLIE) Schemes:** A GLIE scheme must try each action in each state an unbounded number of times to avoid having a finite probability that an optimal action is missed ****
    - $[\epsilon$ - Greedy vs. $\epsilon$-soft Greedy Policies](https://edstem.org/us/courses/33453/discussion/2597251)
        - $\epsilon$-greedy policy is a greedy policy with a small chance to take a non-greedy explorative action
        - Soft policy just means every action has a chance of being chosen, and the e-greedy is a type of soft policy where the chances are between a greedy action and a random different action
- **Absorbing State:** An absorbing state is a state that, once entered, cannot be left
- **Catastrophic Forgetting:** Catastrophic forgetting, is the tendency of an [artificial neural network](https://en.wikipedia.org/wiki/Artificial_neural_network) to abruptly and drastically forget previously learned information upon learning new information
- **Experience Replay:** [Experience Replay](https://datascience.stackexchange.com/questions/20535/what-is-experience-replay-and-what-are-its-benefits) is a replay memory technique used in reinforcement learning where we store the agent’s experiences at each time step, pooled over many episodes into a replay memory
    - We then usually sample the memory randomly for a mini-batch of experience, and use this to learn off-policy
    - Rather than regenerating new experiences each time we want to train the agent, why don’t we continue learning from experiences we already have available?
- **(Temporal)** **Credit Assignment Problem**: **Is the problem of determining the actions that lead to a certain outcome, especially when these rewards are time delayed**
    - It refers to the fact that rewards, especially in fine grained state-action spaces, can occur terribly temporally delayed
        - How do we reason about how each choice contributed to the outcome. The temporal component extends this to say that the impact of decisions at any given point may only be visible in the distant future
    - [See more](https://ai.stackexchange.com/questions/12908/what-is-the-credit-assignment-problem)
- **Boltzmann Rationality:** Boltzmann-rationality **provides a way to model a satisficing human, without implicitly adding in any other assumptions about the human's choice**
- **Tabular Vs. Non Tabular Methods**
    - Tabular methods refer to problems in which the state and actions spaces are small enough for approximate value functions to be represented as arrays and tables
- **[Bootstrap Methods](https://datascience.stackexchange.com/questions/26938/what-exactly-is-bootstrapping-in-reinforcement-learning):** bootstrapping in RL means that you **update a value based on some estimates and not on some exact values**
- **Maximization Bias:** Maximization Bias is a technical way of saying that Q-Learning algorithm overestimates the value function estimates (V) and action-value estimates (Q)
    - [Reference](https://medium.com/@ameetsd97/deep-double-q-learning-why-you-should-use-it-bedf660d5295)
    - One way to view the problem of maximizing bias is that it is due to using the same samples both to determine the maximizing action and to estimate its value
        - [Reference](https://banay.me/maximisation-bias-q-learning/)
- **Distribution Vs. Sample Models:**
    - Some models produce a description of all possibilities and their probabilities; these we call distribution models
    - Other models produce just one of the possibilities, sampled according to the probabilities; these we call sample models
- **Actor-Critic Methods:**
    - The critic is the learned value function, whereas the actor is the learned policy function

# Resources

Documentation

- [OpenAI](https://spinningup.openai.com/en/latest/spinningup/rl_intro2.html)

**Videos**

- [https://learn.udacity.com/courses/ud600](https://learn.udacity.com/courses/ud600)

**Active Vs. Passive RL**

- [https://datascience.stackexchange.com/questions/85358/what-is-the-difference-between-active-learning-and-reinforcement-learning](https://datascience.stackexchange.com/questions/85358/what-is-the-difference-between-active-learning-and-reinforcement-learning)
- [https://courses.cs.vt.edu/cs4804/Fall16/pdfs/10 Passive Reinforcement Learning.pdf](https://courses.cs.vt.edu/cs4804/Fall16/pdfs/10%20Passive%20Reinforcement%20Learning.pdf)
- [https://towardsdatascience.com/explaining-reinforcement-learning-active-vs-passive-a389f41e7195](https://towardsdatascience.com/explaining-reinforcement-learning-active-vs-passive-a389f41e7195)
- [https://towardsdatascience.com/intro-to-reinforcement-learning-temporal-difference-learning-sarsa-vs-q-learning-8b4184bb4978](https://towardsdatascience.com/intro-to-reinforcement-learning-temporal-difference-learning-sarsa-vs-q-learning-8b4184bb4978)
- [https://deeplizard.com/learn/video/mo96Nqlo1L8](https://deeplizard.com/learn/video/mo96Nqlo1L8)

**RL**

- [https://www.cs.cornell.edu/courses/cs4700/2011fa/lectures/07a_ReinforcementLearning.pdf](https://www.cs.cornell.edu/courses/cs4700/2011fa/lectures/07a_ReinforcementLearning.pdf)
- [https://notesonai.com/Reinforcement+Learning](https://notesonai.com/Reinforcement+Learning)
- [https://towardsdatascience.com/a-technical-introduction-to-experience-replay-for-off-policy-deep-reinforcement-learning-9812bc920a96](https://towardsdatascience.com/a-technical-introduction-to-experience-replay-for-off-policy-deep-reinforcement-learning-9812bc920a96)
- [https://www.cs.cornell.edu/courses/cs4700/2011fa/lectures/07a_ReinforcementLearning.pdf](https://www.cs.cornell.edu/courses/cs4700/2011fa/lectures/07a_ReinforcementLearning.pdf)
- [https://icml.cc/2015/tutorials/PolicySearch.pdf](https://icml.cc/2015/tutorials/PolicySearch.pdf)
- [https://towardsdatascience.com/policy-based-reinforcement-learning-the-easy-way-8de9a3356083](https://towardsdatascience.com/policy-based-reinforcement-learning-the-easy-way-8de9a3356083)

**Value Based Approaches**

**Policy Search Approaches**

- [https://icml.cc/2015/tutorials/PolicySearch.pdf](https://icml.cc/2015/tutorials/PolicySearch.pdf)

**Actor-Critic Based Approaches**

- [https://theaisummer.com/Actor_critics/](https://theaisummer.com/Actor_critics/)
- [https://livebook.manning.com/book/deep-learning-and-the-game-of-go/chapter-12/13](https://livebook.manning.com/book/deep-learning-and-the-game-of-go/chapter-12/13)
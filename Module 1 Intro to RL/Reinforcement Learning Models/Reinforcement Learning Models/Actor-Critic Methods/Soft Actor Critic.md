# What is it?

- SAC is an off-policy algorithm that learns a stochastic policy and a value function
    - The policy is optimized to maximize the expected sum of rewards while also maximizing the entropy of the policy, which encourages exploration
        - That is, to succeed at the task while acting as randomly as possible
    - The value function is learned using the mean squared error between the predicted and actual values
- A central feature of SAC is **entropy regularization**
    - The policy is trained to maximize a trade-off between expected return and [entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)), a measure of randomness in the policy
        - It uses a soft Q-learning approach, which means that the Q-function is regularized using the entropy of the policy
    - This has a close connection to the exploration-exploitation trade-off: increasing entropy results in more exploration, which can accelerate learning later on
        - It can also prevent the policy from prematurely converging to a bad local optimum
    - We want a high entropy in our policy to explicitly encourage exploration, to encourage the policy to assign equal probabilities to actions that have same or nearly equal Q-values, and also to ensure that it does not collapse into repeatedly selecting a particular action that could exploit some inconsistency in the approximated Q function
        - Therefore, SAC overcomes the brittleness problem by encouraging the policy network to explore and not assign a very high probability to any one part of the range of actions

# Key Components

- SAC incorporates 3 key components:
    1. Actor-Critic Architecture with separate policy and value function networks
    2. Off-Policy formulation that enables use of previously collected data for efficiency (Replay Buffer)
    3. Entropy Maximization to enable stability and exploration

# Quick Facts

- SAC is an off-policy algorithm
- The original version of SAC implemented can only be used for environments with continuous action spaces. There exists a version for [discrete action spaces](https://arxiv.org/pdf/1910.07207.pdf)
- An alternate version of SAC, which slightly changes the policy update rule, can be implemented to handle discrete action spaces

# Entropy Regularized RL

- To explain Soft Actor Critic, we first have to introduce the entropy-regularized reinforcement learning setting. In entropy-regularized RL, there are slightly-different equations for value functions
    - Maximum entropy alters the RL objective, though the original objective can be recovered using a temperature parameter
- n entropy-regularized reinforcement learning, the agent gets a bonus reward at each time step proportional to the entropy of the policy at that time step. This changes the RL problem to:

    $$
    \pi^* = \argmax_\pi E_{\tau \sim  \pi} \left [ \sum_{t=0} ^ \infin \gamma^t \left(R(s_t, a_t, s_{t+1} + \alpha H(\pi(\cdot | s_t)\right)  \right]
    $$

    - Where:
        - $H$ represents the entropy of $x$ which is a random variable. The entropy can be defined as: $H(P) = E_{x \sim P}[- \log P(x)]$
        - $\alpha > 0$ represents the trade-off coefficient
            - The trade-off coefficient in the Soft Actor-Critic (SAC) algorithm is a hyperparameter that determines the balance between maximizing the expected return and maximizing the entropy of the policy
                - Thus, it controls the stochasticity of the optimal policy

# How does it work?

- SAC concurrently learns a policy $\pi_\theta$ and two Q-functions $Q_{\phi_1}, Q_{\phi_2}$
    - There are two variants of SAC that are currently standard: one that uses a fixed entropy regularization coefficient $\alpha$, and another that enforces an entropy constraint by varying $\alpha$ over the course of training
        - Fixed entropy regularization is simpler but generally the entropy-constrained variant is preferred in practice

- **Exploration-Exploitation Trade-off**
    - The entropy term in the SAC objective function encourages exploration by penalizing policies that are too certain about their actions
        - The trade-off coefficient, denoted by alpha, is used to weight the contribution of the entropy term relative to the expected return term
            - A higher value of alpha places more emphasis on the entropy term, leading to more exploration and potentially faster learning, but also more randomness in the agent's behaviour
            - A lower value of alpha places more emphasis on the expected return term, leading to more exploitation and potentially slower learning, but also more deterministic behaviour
            - In SAC, the trade-off coefficient is typically annealed over time to balance exploration and exploitation as the agent learns. Initially, the coefficient is set to a high value to encourage exploration, and then it is gradually reduced to a lower value as the agent becomes more confident in its policy

# SAC Discrete Action Spaces (DASAC): Pseudocode

![Untitled](./Soft%20Actor%20Critic/Untitled.png)

# SAC Continuous Action Space: Pseudocode

![Untitled](./Soft%20Actor%20Critic/Untitled%201.png)

# Key Concepts & Terms

- **Sample Complexity:** A definition of the sample complexity of exploration for a reinforcement learning algorithm is the **number of time steps in which the algorithm does not select near-optimal actions**
- **Stochastic Vs. Deterministic Policies**
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
    - [Good Resource](https://ai.stackexchange.com/questions/12274/what-is-the-difference-between-a-stochastic-and-a-deterministic-policy)

# Resources

- [https://arxiv.org/pdf/1801.01290.pdf](https://arxiv.org/pdf/1801.01290.pdf)
- [https://arxiv.org/pdf/2112.02852.pdf](https://arxiv.org/pdf/2112.02852.pdf)
- [https://spinningup.openai.com/en/latest/algorithms/sac.html#id6](https://spinningup.openai.com/en/latest/algorithms/sac.html#id6)
- [https://www.mathworks.com/help/reinforcement-learning/ug/sac-agents.html](https://www.mathworks.com/help/reinforcement-learning/ug/sac-agents.html)
- [https://www.cc.gatech.edu/classes/AY2022/cs7643_spring/assets/L22_RL3.pdf](https://www.cc.gatech.edu/classes/AY2022/cs7643_spring/assets/L22_RL3.pdf)

**Discrete Action Spaces**

- [Paper](https://arxiv.org/pdf/1910.07207.pdf)
- Parameter overview: [https://github.com/yosider/ml-agents-1/blob/master/docs/Training-SAC.md](https://github.com/yosider/ml-agents-1/blob/master/docs/Training-SAC.md)
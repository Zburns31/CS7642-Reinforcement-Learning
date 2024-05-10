# What is it?

- **Multi-agent reinforcement learning** studies how multiple agents interact in a common environment
    - That is, when these agents interact with the environment and one another, can we observe them collaborate, coordinate, compete, or collectively learn to accomplish a particular task

# How is it different than normal RL

- In MARL, multiple agents are concurrently optimized to find optimal policies. Compared to the single-agent RL set tings, this introduces several important differences and possibilities
    - **Observability in MARL:** In RL, observability describes whether an agent can perceive the entire environment state fully or partially
        - MARL provides the opportunity to combine partial observations from multiple agents to gather more information
        - Many MARL methods use Recurrent Neural Networks (RNNs) to aggregate observations overtime into more complete observations
    - **Centrality:** Different MARL settings can be distinguished by how much agents can communicate with each other
        - In a fully decentralized problem, agents have no means of communication, while a fully centralized setting allows one central entity to perceive and control all agents
            - This distinction affects whether the state and action spaces for all agents are disjoint (decentralized) or joint (fully centralized)
                - Most MARL algorithms are neither decentralized nor fully centralized
    - **Heterogeneous vs. Homogeneous agents:** In general, MARL does not require all agents to be similar
        - Heterogeneous agents can have different observation and action spaces, like cars and traffic lights
        - However, in many MARL environments, the agents are homogeneous, meaning they have the same state and action space
        - For homogeneous agents, parameter sharing can be introduced to effectively jointly learn their policies
            - This is advantageous as the amount of trainable parameters is reduced, allowing for shorter training times. More efficient training is enabled as the experiences of all agents are used
            - However, different behaviour at execution time is still possible because each agent has unique observations
- **Cooperative vs. Competitive:** An important difference between MARL environments is how the goals of each agent relate to each other
    - This can be divided into fully cooperative, fully competitive, and mixed cooperative-competitive
- **Scalability:**
    - While simple MARL problems have a relatively small number of agents, many real-world mobility scenarios involve hundreds or thousands of individual traffic participants
- **Global vs. Local rewards:** Single-agent RL environments have a single reward definition specifying the objective for the agent
    - Cooperative multi-agent settings can either have a single global reward or multiple local rewards, one for each agent
        - By contrast, global rewards only depend on the global state. They are easy to compute and specify but often lead to inefficient training because agents are never explicitly rewarded or penalized for their own actions
        - Local rewards can reflect each agent’s contribution to the cooperating group. This makes it easier to learn and improves convergence, especially in decentralized settings that do not have other explicit cooperation
            - However, it is usually challenging to design and implement a reward function that correctly rewards individual agents for their contribution


# Challenges

- Learning in multi-agent settings is fundamentally more difficult than the single-agent case thanks to problems like:
    - **Non-stationarity**: If all agents are learning at the same time, the dynamics become more complicated and break many standard RL assumptions
        - Non-stationarity arises in multi-agent reinforcement learning (RL) because the environment that each agent interacts with is affected by the actions of all the other agents
            - In a single-agent RL setting, the environment is typically stationary because the agent's actions do not affect the environment's dynamics
        - However, in multi-agent RL, the actions of one agent can affect the observations and rewards of other agents, and thus the environment that each agent interacts with can change over time as the agents learn and adapt
            - This creates a non-stationary environment that can make learning more challenging, as the agents must learn to adapt to changes in the environment caused by the other agents' actions
        - Furthermore, the behaviour of other agents may also change over time as they learn and adapt, further contributing to the non-stationarity of the environment
            - This means that the agents must not only learn to adapt to the changing environment, but also to the changing behaviour of the other agents
    - **Curse of dimensionality**: An exponential growth of state-action space when a learning agent keeps track of all agent actions.
    - **Multi-agent credit assignment**: Defining how agents should deduce their contributions when learning in a team
        - For example, if agents receive a team reward but it’s one agent doing most of the work, others can become “lazy.” It’s just like real life!

# Multi-Agent Actor-Critic for Mixed Cooperative-Competitive Environments

- This paper introduces MADDPG

- [Multi-Agent Actor-Critic for Mixed Cooperative-Competitive Environments](./Multi-Agent%20RL%20(MARL)/Multi-Agent%20Actor-Critic%20for%20Mixed%20Cooperative-Competitive%20Environments.md)

# Advantage Functions

- [Advantage Functions & GAE](./Advantage%20Functions%20&%20GAE.md)

# Models

## Zero-Sum Games

- [Minimax Q](./Multi-Agent%20RL%20(MARL)/Minimax%20Q.md)
- [Nash Q-Learning](./Multi-Agent%20RL%20(MARL)/Nash%20Q-Learning.md)

## Off-Policy

## On-Policy

- [Proximal Policy Optimization](./Proximal%20Policy%20Optimization.md)

# Resources

**Papers**

- [Survey Paper on MARL](https://arxiv.org/pdf/1810.05587.pdf)
- [Overview of MARL Algorithms](https://arxiv.org/pdf/1911.10635.pdf)

**Blogs**

- [https://bair.berkeley.edu/blog/2018/12/12/rllib/](https://bair.berkeley.edu/blog/2018/12/12/rllib/)
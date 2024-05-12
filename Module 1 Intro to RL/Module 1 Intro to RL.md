# What is Reinforcement Learning (RL)?

- Reinforcement learning is learning what to do—how to map situations to actions—so as to maximize a numerical reward signal
    - The learner is not told which actions to take, but instead must discover which actions yield the most reward by trying them
- The two most important concepts in RL:
    1. Trial and error search
    2. Delayed Reward

- Challenges in RL
    - Exploration & Exploitation
        - The agent has to exploit what it has already experienced in order to obtain reward, but it also has to explore in order to make better action selections in the future
        - The dilemma is that neither exploration nor exploitation can be pursued exclusively without failing at the task
    - Interaction in an uncertain environment


# Reinforcement Learning Basics

- [Reinforcement Learning Models](./Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models.md)

# Markov Decision Processes

- [Markov Decision Processes (MDPs)](./Markov%20Decision%20Processes/Markov%20Decision%20Processes%20(MDPs).md)

### **Returns and Episodes:**

[More About Rewards (Utilities)](./More%20About%20Rewards/More%20About%20Rewards%20(Utilities).md)

- In general, we seek to maximize the expected return, where the return, denoted $G_t$, is defined as some specific function of the reward sequence

    $G_t = R_{t+1} + R_{t+2} + .... R_T$

    - where T is a final time step
- Because of *continuing tasks* (tasks that are on-going/long-lived), we need to add in the concept of discounting for rewards
    - **The agent will select actions that maximize the long-run sum of discounted rewards**


**Generally:**

- The un-discounted formulation of rewards is appropriate for episodic tasks, in which the agent-environment breaks naturally into episodes
- The discounted formulation is appropriate for continuing tasks in which the interaction does not naturally break into episodes, but continues without limit

# Policies and Value Functions

[Policies](./Markov%20Decision%20Processes/Markov%20Decision%20Processes%20(MDPs)/Policies.md)

- Basically all RL algorithms involve estimating *value functions* - functions of states (or of state-action pairs) that estimate how good it is for the agent to be in a given state
    - How good here refers to the future rewards that can be expected
        - Rewards expected can depend on what actions the agent will take
    - **Value functions are defined with respect to particular ways of acting, called policies**
        - Where a policy is defined as a mapping from states to probabilities of selecting each possible action
- The Bellman optimality equations are special consistency conditions that the optimal value functions must satisfy and that can, in principle, be solved for the optimal value functions, from which an optimal policy can be determined with relative ease

# **Optimality and Approximation**

Two of the main challenges in finding the optimal policy and value function are:

1. Computational power, in particular, the amount of computation that can be performed in a single time step
2. Memory available
    - In tasks with small, finite state sets, it is possible to form approximations using arrays or tables with one entry for each state
        - **This is called the tabular case or tabular methods**
    - In many cases, there are far more states than could possibly be entries in a table
        - In these cases the functions must be approximated, using some sort of compact parametrized function representation

# Terms and Concepts

- **Episodes:** When the agent–environment interaction breaks naturally into subsequences, which we call episodes, such as plays of a game, trips through a maze, or any sort of repeated interaction
    - Each state ends in a special state called the **terminal state**
- **Episodic Tasks:** Tasks with episodes
    - In an episodic environment, the agent faces the same problem over and over again
- **Continuing Tasks:** In many cases, the agent-environment interaction does not break into identifiable episodes, but goes on continually without limit

# Sutton and Barto Readings
- 1.1-1.4
- 1.6-1.7
- 3.1-3.8
- 16.1-16.9
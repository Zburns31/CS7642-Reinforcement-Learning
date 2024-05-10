# Readings

- Sutton 17.2
- Littman (2009)

# What makes RL hard?

- Delayed reward
    - The agent is told how it’s doing, not what it should be doing
    - The agent needs to reason over time and because there are many options at every step, there is an exponential blow up of possible paths
- Bootstrapping ~ Need for exploration
    - Making estimates based on estimates
- Computational and Memory Constraints: size of the state space, # of actions

# What is it?

- Temporal abstraction in reinforcement learning refers to the use of hierarchical reinforcement learning to learn policies that operate at multiple levels of temporal granularity
    - In other words, instead of learning a single policy that maps states directly to actions, temporal abstraction involves learning a set of policies that operate at different time scales
- By incorporating temporal abstraction into the learning process, an agent can more efficiently explore the state space and learn more effective policies for complex tasks
    - Additionally, temporal abstraction can help to reduce the problem of the "curse of dimensionality," in which the state space grows exponentially with the number of variables, by breaking down the task into smaller sub-tasks that can be solved more easily

![Untitled](./Week%2010%20Options/Untitled.png)

# How does this improve efficiency?

- Temporal abstraction can improve the efficiency of RL algorithms in several ways. First, it can speed up the learning process by enabling the agents to transfer and generalize knowledge across different situations and tasks
- Second, it can reduce the variance and bias of the reward signals by providing intermediate feedback and guidance

# How does it work?

- ***The main idea*** behind temporal abstraction is to break down a complex task into a series of smaller sub-tasks, each of which can be solved using a simpler policy
    - This approach can be more effective than trying to learn a single policy that directly solves the entire task, especially for tasks that involve long time horizons or complex decision-making

# Semi-Markov Decision Processes (SMDP’s)

- SMDPs extend the Markov decision process (MDP) framework by allowing the time intervals between actions to be random. In a standard MDP, the time intervals are assumed to be fixed and controlled by the agent
    - In contrast, in an SMDP, the time intervals can be random and depend on the state of the system
    - a SMDP has some Markovian properties but is also dependent on things such as $k$ (# of steps during an option, reward is dependent upon $k$)
        - Hence the term Semi-MDP. The state depends upon $k$
        - At the level of options, it’s not Markovian because of this dependency
- An SMDP is characterized by a set of states, actions, rewards, and transition probabilities
    - However, in addition to the standard MDP formulation, SMDPs also include a set of ***duration probabilities that specify the probability distribution of the time it takes to complete an action in a given state***
- Semi-MDPs are thus used to deal with such problems that involve actions of different levels of abstraction
    - **Hierarchical reinforcement learning** (HRL) is a generalization (or extension) of reinforcement learning where the environment is modelled as a semi-MDP
    - Given the set of options we have, we can converge to an optimal policy
- Note all of the convergence guarantees that come with MDP’s also follow for SMDP’s
    - One caveat though is that if you have the right set of options then we can be optimal
        - However, there is no guarantee that if we are just using options that we will converge upon an optimal solution

# Types of Temporal Abstraction

***There are two main types of temporal abstraction in reinforcement learning:***

1. ***Options:*** Options are sub-policies that can be executed repeatedly for a variable amount of time
    1. They allow an agent to perform a specific task or sub-task, such as moving to a specific location or avoiding an obstacle
2. **Macro-actions:** Macro-actions are high-level actions that are executed over a longer period of time than regular actions
    1. They can be thought of as a sequence of actions that are executed together to achieve a specific goal

# Options

- Options allow agents to make predictions and to operate at different levels of abstraction within an environment
    - Nevertheless, approaches based on the options framework often start with the assumption that a reasonable set of options is known beforehand
    - Options are also known as ***temporally extended actions***
        - Options can be variable amounts of time
- Proper design of options may allow us to ignore parts of the state space (MDP) that don’t matter

- [Options](./Week%2010%20Options/Options.md)

# Goal Abstraction

- **Sequential Actions Vs. Parallel Goals**
    - We may have multiple goals within focus but only one is ascendant at a time
    - All options are trying to accomplish different goals. Some of these options are more important at any given time
    - $\beta$ captures the probability that we end in some state $s$

# Modular RL (Arbitration)

- Modular RL really just represents when we divide the state space into modules, all of which have goals associated with them
    - How do we decide which module we run next?
- **Algorithms:**
    - **Greatest Mass Q-Learning:** Generate Q-functions for each option and add up the Q-value for each state action pair. Which ever one is largest we execute
        - Which action after combining (sum) the Q-functions has the largest Q-value?
            - I.e a voting mechanism and whoever has the biggest vote wins
    - **Top Q-Learning:**
        - Take whichever module has the most value associated with it
    - **Negotiated W-Learning:**
        - The agent with the most to lose gets to determine what action to choose
- **Issues:** These algorithms all have issues with them. I.e. It’s not possible to create a fair voting system
    - I.e Arrow’s impossibility theorem
        - All of the algorithms assume the agents are using exactly the same units. But if one of the agents uses a different scale, than a fair comparison is not possible
    - Goals may not be compatible

# Monte Carlo Tree Search

- Essentially, **MCTS uses Monte Carlo simulation to accumulate value estimates to guide towards highly rewarding trajectories in the search tree**
    - In other words, MCTS pays more attention to nodes that are more promising, so it avoids having to brute force all possibilities which is impractical to do
- [Monte Carlo Tree Search](./Week%2010%20Options/Monte%20Carlo%20Tree%20Search.md)

# Key Concepts & Terms

- **Separation of Concerns:** separation of concerns is a design principle for separating a computer program into distinct sections. Each section addresses a separate concern, a set of information that affects the code of a computer program

# Resources

- [https://www.deepmind.com/publications/temporal-abstraction-in-reinforcement-learning-with-the-successor-representation](https://www.deepmind.com/publications/temporal-abstraction-in-reinforcement-learning-with-the-successor-representation)
- [https://www.cs.mcgill.ca/~dprecup/courses/RL/Lectures/Options2019.pdf](https://www.cs.mcgill.ca/~dprecup/courses/RL/Lectures/Options2019.pdf)
- [https://www.linkedin.com/advice/0/how-can-temporal-abstraction-improve-efficiency](https://www.linkedin.com/advice/0/how-can-temporal-abstraction-improve-efficiency)

### Options

- [https://ai.stackexchange.com/questions/13254/what-are-options-in-reinforcement-learning](https://ai.stackexchange.com/questions/13254/what-are-options-in-reinforcement-learning)
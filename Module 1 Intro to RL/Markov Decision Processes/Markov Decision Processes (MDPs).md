# Markov Decision Processes (MDPs)

# What are Markov Decision Processes?

- A Markov Decision Process (MDP) is a mathematical framework used for modelling decision-making problems where the outcomes are partly random and partly controllable
- Markov Decision Problems (MDPs) extend Deterministic Search Problems (DSPs) by modelling the generation of successor states (given a known previous state and a given action) as a probability distribution called a Transition Probability

**Example MDP and State Transition Matrix**

![Untitled](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/Untitled.png)

[Good Example Reference](https://omkar-ranadive.github.io/posts/rl-l2-ds)

## Key Assumptions for MDP’s

- **Markov Property:**
    - The Markov Property expresses the fact that at a given time step and knowing the current state, we won’t get any additional information about the future by gathering information about the past
    - **Only the present matters!!!**
    

- **Rules of the environment are stationary**
    - Meaning they stay constant over time

# MDP Characteristics: Defining the problem space

---

- ***S* States**
    - The state defines the current situation of the agent (for eg: it can be the exact position of the Robot in the house, or the alignment of its two legs, or its current posture; it depends on how you address the problem

- ***A(s)* Actions**
    - The things you can do in a particular state
    - The choice that the agent makes at the current time step (for eg: it can move its right or left leg, or raise its arm, or lift an object, turn right or left, etc.). We know the set of actions (decisions) that the agent can perform in advance
    - State may govern the actions an agent is allowed to perform

- ***T* Transition Model:** T(state: *s*, action *a*, future state *s') ~= Pr(s'* | *s*, *a*)
    - Defines the rules of the game your playing
        - The physics of the environment we are operating in
    - The model produces a probability that you will end up transitioning to state *s'* given that you were in state *s* and took action *a*
        - **The probabilities must add up to 1**
        
- **Reward:** R(s), R(s, a), R(s, a, s')
    - A scalar value that you get for being in a state *s*
    - The rewards you receive from a state tells you the usefulness of entering into that state
    - R(s, a): The reward you receive for entering a state AND taking an action
    - R(s, a, s'): The reward you receive for entering a state, taking an action AND entering another state
    - **Minor changes to the reward function matter**
        - The rewards define the MDP and how quickly you want to reach the final (absorbing) state
        - They capture the domain knowledge
    - It is critical that the rewards are setup in such a way to truly indicate what we want accomplished
        - The reward signal is not the place to teach the agent HOW to achieve what we want or achieving subgoals
    - **Detailed Wiki on Rewards**
        
        [More About Rewards (Utilities)](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/More%20About%20Rewards%20(Utilities)%20a6ae735fef1d4927ae780ef42a0e1427.md)
        

# MDP: Defining the Solution

- Policy $\pi(s)$ —> *a*
    - A function that takes in any given state (your in) and returns a given action that you should take
    - $\pi^*$  is the optimal policy **that maximizes the long-term expected reward**
        - Of all the decisions that you could take, this policy maximizes the reward that you could receive
    - Policies may be *stochastic*, specifying probabilities for each action
    
    [Policies](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/Policies%209b4ff1610f094eba9e470925b61bd576.md)
    

# Unknown Transitions and Rewards

- We can use a **Model Based RL solution which builds a Transition and Reward function**

# How to Solve MDP’s

1. **Dynamic Programming**
    1. Dynamic Programming *uses known* probabilities to find a Markov Decision Process (MDP) that tells us what to do
        1. When we know everything about the world - all of its possible states, actions, probabilities, and rewards - we can find an equation that will tell us the optimal (i.e., best) action to take in any given situation (i.e., state)
        2. To solve this MDP, we use a type of algorithm called Dynamic Programming. Specifically, we use one of two Dynamic Programming algorithms: *Policy Iteration* or *Value Iteration*
            1. Both can be used to solve an MDP when we know the true model of the environment
        
2. **Reinforcement Learning**
    1. Reinforcement Learning *learns* probabilities to find a Markov Decision Process (MDP) that tells us what to do
    2. Reinforcement Learning (RL) essentially uses "expectations" from repeated trials to "*learn*" how to make decisions

## Algorithms for MDP’s

---

[Value Iteration](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/Value%20Iteration%20ccae45b2684545b2a238eba7c42532c0.md)

[Policy Iteration](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/Policy%20Iteration%2055a39c3b13e146b79871f389d98f5c74.md)

[Linear Programming](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/Linear%20Programming%20fbda1314dbd24f3dac55e5a3e79d8b84.md)

# Online Vs. Offline Algorithms

- Value iteration and policy iteration are offline algorithms: like the A* algorithm in Chapter 3, they generate an optimal solution for the problem, which can then be executed by a simple agent
    - For sufficiently large MDP’s, exact offline solutions, even by a polynomial time algorithm, is not possible
- **Online algorithms are where the agent does a significant amount of computation at each decision point rather than operating primarily on precomputed information**

# Bellman Equations

- Why are there several different Bellman equations (U, V, Q, C)?
    - Because the insight behind “the Bellman equation” regards the recursive nature of expected utility calculations and the different functions you mention are just expressions of different parts of that recursive relationship
    - That is, once you know the fundamental recursive relationship, there are a number of different ways you could re-express it
    - [Good Reference](https://www.quora.com/Why-are-there-several-Bellman-equations-U-V-Q-C)

## Online Algorithms

[Expectimax](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/Expectimax%20db0be3ad5c844e4baebde5ddefc094fc.md)

[Bandit Problems](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/Bandit%20Problems%20bf3c7792d5ef4a169d709841f571d1b2.md)

# Key Terms & Concepts

- **Markov Reward Process:** A Markov reward process, or MRP, is a Markov decision process without actions
    - We will often use MRPs when focusing on the prediction problem, in which there is no need to distinguish the dynamics due to the environment from those due to the agent
- **Markov Chain:** A [Markov model](https://deepai.org/machine-learning-glossary-and-terms/markov-model) is a stochastic model with the property that future states are determined only by the current state -- in other words, the model has no memory; it only knows what state it's in now, not any of the states which occurred previously
    - There is only one action

# Resources

- MDPs
    
    [MDPIntro.pdf](Markov%20Decision%20Processes%20(MDPs)%2003359ffd4f8b4c4a9befdf118ecbf55f/MDPIntro.pdf)
    
    - [https://stanford.edu/~cpiech/cs221/handouts/markovDecisions.html](https://stanford.edu/~cpiech/cs221/handouts/markovDecisions.html)
    - [https://towardsdatascience.com/understanding-the-markov-decision-process-mdp-8f838510f150](https://towardsdatascience.com/understanding-the-markov-decision-process-mdp-8f838510f150)
    - [https://www.ccs.neu.edu/home/rjw/csg220/lectures/reinforcement.pdf](https://www.ccs.neu.edu/home/rjw/csg220/lectures/reinforcement.pdf)
    - [https://web.mit.edu/6.7950/www/lectures/L4-2022fa.pdf](https://web.mit.edu/6.7950/www/lectures/L4-2022fa.pdf)
- **Bellman Equation**
    - [https://notesonai.com/Bellman+Equation+and+Value+Functions](https://notesonai.com/Bellman+Equation+and+Value+Functions)
    - [https://medium.com/analytics-vidhya/bellman-equation-and-dynamic-programming-773ce67fc6a7](https://medium.com/analytics-vidhya/bellman-equation-and-dynamic-programming-773ce67fc6a7)
    - [https://towardsdatascience.com/the-bellman-equation-59258a0d3fa7](https://towardsdatascience.com/the-bellman-equation-59258a0d3fa7)
- **Value & Policy Iteration**
    - [https://notesonai.com/Dynamic+Programming](https://notesonai.com/Dynamic+Programming)
- **Videos**
    - [https://learn.udacity.com/courses/ud600](https://learn.udacity.com/courses/ud600)
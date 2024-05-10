# What are Stochastic Games?

- Stochastic games are a popular framework for studying multi-agent reinforcement learning (MARL)
- A zero-sum two-player stochastic game with finite number of states and finite number of pure strategies at each state

# Components

- $S$: States
- $A_i$ Actions for player $i$
- $T$: Transitions $\rightarrow T(s, (a,b), s')$
- $R_i$: Rewards for player $i$ $\rightarrow R_1(a,b), R_2(a,b)$
- $\gamma$: Discount Rate

We can constrain the stochastic game model so we end up with:

1. Zero-Sum Stochastic Game: $R_1 = - R_2$
2. MDP: $T(s,(a,b), s') = T(s,(a,b'), s' \space \forall b'$
    1. $R_2(s, (a,b)) = 0$ or some constant
    2. Basically we assume there is only 1 agent. I.e. we make the other player irrelevant. So this reduces a regular MDP
3. Repeated Game: $|S| = 1$

# Zero-Sum Stochastic Games

- The idea is that the actions that the players take not only impact the rewards but also future states
    - The way we can deal with this issue is by defining a value function

$$
Q^*_i(s, (a,b)) = R_i(s,(a,b)) + \gamma \sum_{s'}T(s,(a,b),s') \max_{a',b'} Q^*_i(s', (a',b'))
$$

- Where:
    - $Q^*_i$ is defined over joint state action pairs
    - The immediate reward to player $i$ for that joint action in that state
    - Discounted expected value for that next state
        - Look at the $Q$ values in the next state and summarize them
- There is an issue with the $max$ operator in this equation
    - This equation assumes that the joint actions that are gonna be taken will benefit you the most
        - This will lead to ***delusional optimism***
- To solve this issue, we can adjust the equation to:

$$
Q^*_i(s, (a,b)) = R_i(s,(a,b)) + \gamma \sum_{s'}T(s,(a,b),s') \space minimax_{a',b'} Q^*_i(s', (a',b'))
$$

- Where:
    - When we evaluate the value of a state, we actually solve the zero-sum game in the $Q$ values

# Minimax Q

- [Minimax Q](./../../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Multi-Agent%20RL%20(MARL)/Minimax%20Q.md)

# General-Sum Games

- Minimax only works in zero-sum games because it assumes the other player is trying to minimize my score. However, this is not the concept of a Nash Equilibrium
    - So we can compute the NE instead:

    $$
    Q^*_i(s, (a,b)) = R_i(s,(a,b)) + \gamma \sum_{s'}T(s,(a,b),s') \space Nash_{a',b'} Q^*_i(s', (a',b'))
    $$


- [Nash Q-Learning](./../../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Multi-Agent%20RL%20(MARL)/Nash%20Q-Learning.md)
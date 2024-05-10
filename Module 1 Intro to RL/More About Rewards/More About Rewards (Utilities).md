# How do Rewards Impact MDP's?

![The size and direction of the reward will define how the MDP is played](./More%20About%20Rewards%20(Utilities)/Untitled.png)

The size and direction of the reward will define how the MDP is played

- For the R(s) == 2, it is objectively better to keep the game going as long as possible since the R(s) to keep playing outweighs ending the game early
- For the R(s) == -2, it is better to end the game as early as possible. In this scenario, it doesn't matter
- **How to find the expected sequence of actions?**
    - Calculate the expected values


# Sequences of Rewards: Assumptions

## Infinite Horizons (Stationarity of Policies)

- The sequence of actions an agent will take will depend on:
    1. The reward
    2. The amount of time you have
- The policy will/may change even if you're in the same state, depending on whether you have an infinite or finite time horizon
    - A finite time horizon will lead to non-stationary policies
    - $\pi(s,t) -> action$

## Utility of Sequences

- The idea of the rewards is that we would receive them through a sequence of states/actions
- **Stationary Preferences**
    - **Principle:** If I prefer the first sequence of states today over the second sequence of states, then I prefer the first sequence of states over the second tomorrow
        - This is only true for the ***infinite horizon case***
        - For the finite horizon case, the action we take in some state depends also on the time step
    - This forces adding the rewards of the sequence of states

![More%20About%20Rewards%20(Utilities)%20a6ae735fef1d4927ae780ef42a0e1427/Untitled%201.png](<./More%20About%20Rewards%20(Utilities)/Untitled 1.png>)

$U(S_0, S_1, S_2, ...) == \displaystyle\sum_{t=0}^{\infin} R(S_t) == \displaystyle\sum_{t=0}^{\infin} \gamma^t * R(S_t) :   0 \leq t \lt 1$

- The utility of a sequence of events is the sum of the rewards of all the states
    - Note that $\gamma$ will become smaller as the rewards are further out
        - **This is called discounted rewards $\gamma$**
        - E.g. If gamma ($\gamma$) is less than 1, then eventually as you raise it to a power, it will basically become 0
            - $\gamma$  is called the *discount rate*
                - a reward received k time steps in the future is only worth $\gamma^{k-1}$ times what it would be were if it were received immediately
            - If $\gamma$ < 1, the infinite sum (equation above) has a finite value as long as the Reward sequence is bounded
            - If $\gamma$ = 0, the agent is **myopic** in being concerned only with maximizing the immediate rewards
                - I.e. Maximize the reward for the next action

                **As $\gamma$ approaches 1, the return/reward objective takes future rewards into account more strongly (Agent becomes more farsighted)**

- **Utilities are really about accounting for all delayed rewards**
    - The discount factor describes the agents preference for current rewards over future rewards
    - When $\gamma$ is close to 0, rewards in the distant future are viewed as insignificant
    - When $\gamma$ is close to to 1, an agent is more willing to wait for long term rewards
    - When $\gamma$ is exactly 1, discounted rewards reduce to the special case of purely additive rewards

## Discounted Rewards

- There are several reasons why additive discounted rewards make sense:
    1. One is empirical: both humans and animals appear to value near-term rewards more highly than rewards in the distant future
    2. Another is economic due to the time value of money theory
    3. Rewards may never arrive for all sorts of reasons that are not taken into account in the transition model
    4. With discounted rewards, the utility of an infinite sequence is finite
        1. $\sum_{t=0}^\infin R(s_t, a_t, s_{t+1}) \leq \sum_{t=0}^\infin \gamma^t * R_{max} = \dfrac {R_{max}} {1-\gamma}$
            1. This is the standard definition for the sum of an infinite geometric series
    5. With proper policies, we can use additive un-discounted rewards ($\gamma$ = 1)
    6. Infinite sequences can be compared in terms of the **average reward obtained per time step**

    ## Discounted Rewards: Properties

    - *A remarkable consequence of using discounted utilities with infinite horizons is that the optimal policy is independent of the starting state*
    - For discounted infinite-horizon MDP’s, the quantity $1-\gamma$ can be viewed as the probability that the agent will cease to accrue additional reward
        - Therefore, it is as if the agent always has a constant expected number of steps remaining: $\dfrac{1} {1-\gamma}$

# Reward Shaping Theorem

- [Reward Shaping](./More%20About%20Rewards%20(Utilities)/Reward%20Shaping.md)
- [R-Max & Potential-Based Reward Shaping in Model-Based RL](./More%20About%20Rewards%20(Utilities)/R-Max%20&%20Potential-Based%20Reward%20Shaping%20in%20Model-Based%20RL.md)

# Important Concepts

## Temporal Credit Assignment Problem (CAP)?

- It refers to the fact that rewards, especially in fine grained state-action spaces, can occur terribly temporally delayed
    - **How do we reason about how each choice contributed to the outcome**
        - **The temporal component extends this to say that the impact of decisions at any given point may only be visible in the distant future**
    - For example, a robot will normally perform many moves through its state-action space where immediate rewards are (almost) zero and where more relevant events are rather distant in the future
    - As a consequence such reward signals will only very weakly affect all temporally distant states that have preceded it.
    - It is almost as if the influence of a reward gets more and more diluted over time and this can lead to bad convergence properties of the RL mechanism
- CAP is related to back-propagation in a very general sense because if we knew which neurons caused a good/bad decision, then we could leverage that information when making weight updates to the network

### Why is the CAP Problem Important?

- CAP is especially important to reinforcement learning because a good reinforcement learning method would have a strong understanding of how each action influencers the outcome
    - For a game like chess, you only receive the win/loss signal at the end of the game, which implies that you need to understand how each move contributed to the outcome, both in a positive sense ("I won because I took a key piece on the fifth turn") and a negative sense ("I won because I spotted a trap and didn't lose a rook on the sixth turn")

# Resources:

- [http://www.scholarpedia.org/article/Reinforcement_learning#.28Temporal.29_Credit_Assignment_Problem](http://www.scholarpedia.org/article/Reinforcement_learning#.28Temporal.29_Credit_Assignment_Problem)
- [https://ai.stackexchange.com/questions/12908/what-is-the-credit-assignment-problem](https://ai.stackexchange.com/questions/12908/what-is-the-credit-assignment-problem)

### Shaping Rewards

- [https://gibberblot.github.io/rl-notes/single-agent/reward-shaping.html](https://gibberblot.github.io/rl-notes/single-agent/reward-shaping.html)
- [https://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/readings/NgHaradaRussell-shaping-ICML1999.pdf](https://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/readings/NgHaradaRussell-shaping-ICML1999.pdf)
- [https://www.youtube.com/watch?v=dy2p3-iE5aY](https://www.youtube.com/watch?v=dy2p3-iE5aY)
- [https://learn.udacity.com/courses/ud600/lessons/e9b3c8f8-6d45-4a84-adb4-1fd15a3de9c1/concepts/93369d0a-31de-4835-bc2a-a16c3b668ec5](https://learn.udacity.com/courses/ud600/lessons/e9b3c8f8-6d45-4a84-adb4-1fd15a3de9c1/concepts/93369d0a-31de-4835-bc2a-a16c3b668ec5)
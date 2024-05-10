# TD Learning Intro

- [Temporal Difference Learning (TD)](../Reinforcement%20Learning%20Models/Temporal%20Difference%20Learning%20(TD).md)

# Q Learning Intro

- [Q Learning, Double Q Learning, Dyna Q](../Reinforcement%20Learning%20Models/Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q.md)

# What is TD Q Learning?

- The most obvious change compared to normal TD learning is that the agent will have to learn a transition model so that it can choose an action based on via one-step look-ahead

# How does it work?

## Q Learning TD Update Rule

- This update rule is calculated whenever action *a* is executed in state *s* leading to state *s’*

$$
Q(s,a) \leftarrow Q(s,a) + \alpha[R(s,a,s') + \gamma \max_{a'}Q(s',a') - Q(s,a)]
$$

- Where the term $R(s,a,s') + \gamma \max_{a'}Q(s',a') - Q(s,a)$ represents the error that the update is trying to minimize
- **The important part of this equation is what is does not contain: a TD Q Learning agent does not need a transition model $P(s'|s,a)$ either for learning or action selection**
    - The Q-learning agent has no means of looking into the future, so it may have difficulty when rewards are sparse and long action sequences must be constructed to reach them


# Formulas

$$
Q(state, action) = (1-\alpha)*Q(state, action) + \alpha(reward + \gamma \max_{\alpha} Q(next \space state, all \space actions)
$$

$$
Q(state, action) = (1-\alpha)*old \space Q \space value \space + \alpha * (Immediate \space Reward + All \space future \space rewards)
$$

# Algorithm Overview

![Untitled](./Temporal%20Difference%20Q%20Learning/Untitled.png)

- Notes:
    - The algorithm uses the same exploration function as ADP - hence the need to keep statistics on actions taken (table N)
        - If we instead use a simpler exploration policy - say, acting randomly on some fraction of steps, where the fraction decreases over time - then we can dispense with the statistics
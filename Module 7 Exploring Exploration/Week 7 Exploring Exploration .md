# Readings

- Sutton & Barto: 2.1-2.7, 2.10
- Asmuth, Littman, Zinkov (2008)

# Multi-Armed Bandits

- [Multi-Armed Bandits](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Multi-Armed%20Bandits.md)

# General Stochastic MDP’s

- We want to find a way to do efficient exploration in general stochastic MDP’s. We need to combine the two ideas to do this:
    - Stochastic: Hoeffding bound until estimates are accurate
    - Sequential: Unknown state-action pairs assumed optimistic
- This can be done using the $R_{max}$ algorithm which is explained below

# Potential-Based Reward Shaping in Model-Based RL

- Potential-based shaping was designed as a way of introducing background knowledge into model-free reinforcement-learning algorithms
- By identifying states that are likely to have high value, this approach can decrease experience complexity—the number of trials needed to find near-optimal behaviour

- [R-Max & Potential-Based Reward Shaping in Model-Based RL](./Week%207%20Exploring%20Exploration/R-Max%20&%20Potential-Based%20Reward%20Shaping%20in%20Model-Based%20RL.md)
- [Week 6: AAA & Messing with Rewards](../Module%206%20AAA%20&%20Messing%20with%20Rewards/Week%206%20AAA%20&%20Messing%20with%20Rewards.md)
- [More About Rewards (Utilities)](../Module%201%20Intro%20to%20RL/More%20About%20Rewards/More%20About%20Rewards%20(Utilities).md)
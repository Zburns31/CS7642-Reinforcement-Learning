# What is it?

- Inverse reinforcement learning (IRL) is a machine learning approach that aims to infer the underlying reward function of a decision-making agent based on its observed behaviour, without direct access to the true reward function
    - In other words, given a set of demonstrations or trajectories of an expert performing a task, IRL attempts to learn what goal or objective the expert was trying to achieve

# How does it work?

- To achieve this, IRL algorithms typically start by defining a parametric form for the reward function, and then use optimization techniques to fit the parameters to the observed behaviour of the expert
- The optimization process seeks to find the reward function that best explains the observed behaviour of the expert, according to some criterion, such as maximum likelihood or maximum entropy

# Algorithms

- Maximum Likelihood IRL:
    - Guess R, compute $\pi$, measure $Pr(D|\pi)$, gradient on R

# Resources

- [https://towardsdatascience.com/inverse-reinforcement-learning-6453b7cdc90d](https://towardsdatascience.com/inverse-reinforcement-learning-6453b7cdc90d)
- [https://thegradient.pub/learning-from-humans-what-is-inverse-reinforcement-learning/](https://thegradient.pub/learning-from-humans-what-is-inverse-reinforcement-learning/)
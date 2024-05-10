# How it works

- To apply value iteration to POMDPs, we use a technique called the "belief-state" approach. In this approach, we define the belief state as a probability distribution over possible states based on the agent's observations and actions so far. The belief state captures the agent's uncertainty about the true state of the environment
- The value iteration algorithm for POMDPs involves iterating between two steps:
    1. Belief update: Update the belief state based on the current observation and action using Bayes' rule
    2. Value update: Update the value function for each belief state by taking the maximum expected reward over all possible actions, weighted by their corresponding transition probabilities and the expected reward
- The key difference from the value iteration algorithm for MDPs is that in POMDPs, we need to perform the belief update step to estimate the belief state, and then use this belief state to compute the expected reward in the value update step
- The process is repeated until convergence, at which point we have the optimal policy for the POMDP. This policy maps a belief state to an action that maximizes the expected reward.

# Algorithmic Approach

- We can purge vectors by checking for domination among vectors
    - This allows us to solve the POMDP in a reasonable amount of time
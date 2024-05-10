# Adaptive Dynamic Programming (ADP)

# What is ADP?

- An adaptive dynamic programming (or ADP) agent takes advantage of the constraints among the utilities of states by learning the transition model that connects them and solving the corresponding Markov decision process using dynamic programming
- ADP is a smarter method than Direct Utility Estimation as it runs trials to learn the model of the environment by estimating the utility of a state as a sum of reward for being in that state and the expected discounted reward of being in the next state
- ***It can be solved using value-iteration algorithm***

# How does it work?

- **Idea**: ****
    - Run trials to learn model of environment (i.e. T and R)
    - Memorize R(s) for all visited states
    - Estimate fraction of times action a from state s leads to s’
    - Use PolicyEvaluation Algorithm on estimated model
- **Learning the transition model**
    - Since the environment is fully observable, the transition model is estimated directly from the counts that are accumulated in $N_{s'|s,a}$ which is a table from $P(s'|s,a)$ which maps state-action pairs to state $s'$

# Algorithm Overview

![Untitled](./Adaptive%20Dynamic%20Programming%20(ADP)/Untitled.png)

# Pros & Cons

- Can be quite costly for large state spaces
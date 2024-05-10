# Topics Summary of Week 2

- Policy and Value Iteration
    - Includes Generalized Policy Iteration
- Monte Carlo Methods
- Planning & Learning with Tabular Methods
    - I.e. Q Learning, Dyna Q
- Temporal Difference Learning

# Sutton and Barto Topics

- [Policy Iteration](../Module%201%20Intro%20to%20RL/Markov%20Decision%20Processes/Markov%20Decision%20Processes%20(MDPs)/Policy%20Iteration.md)
- [Value Iteration](../Module%201%20Intro%20to%20RL/Markov%20Decision%20Processes/Markov%20Decision%20Processes%20(MDPs)/Value%20Iteration.md)
- [Reinforcement Learning Models](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models.md)

## Asynchronous Dynamic Programming

- Asynchronous DP algorithms are in-place iterative DP algorithms that are not organized in terms of systematic sweeps of the state set
    - These algorithms update the values of states in any order whatsoever, using whatever values of other states happen to be available
        - The values of some states may be updated several times before the values of others are updated once

# Temporal Difference Learning

- [Temporal Difference Learning (TD)](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Temporal%20Difference%20Learning%20(TD).md)

# SARSA: On-Policy TD Control

- [SARSA, Expected SARSA & N-Step SARSA](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/SARSA,%20Expected%20SARSA%20&%20N-Step%20SARSA.md)

# Planning and Learning with Tabular Methods

- All State-Space planning methods share a common structure. There are two basic ideas:
    1. All State-Space planning methods involve computing value functions as a key intermediate step toward improving the policy
    2. They compute value functions by updates or backup operations applied to a simulated experience
- The common structure means that many ideas and algorithms can be transferred between planning and learning
    - In particular, in many cases a learning algorithm can be substituted for the key update step of a planning method
    - Learning methods require only experience as input, and in many cases they can be applied to simulated experience just as well as to real experience
- All kinds of state-space planning can be viewed as sequences of value updates, ***varying only in the type of update, expected or sample, large or small, and in the order in which the updates are done***

## Methods

- [Dyna Q](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q.md)
- [Prioritized Sweeping](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Prioritized%20Sweeping.md)


# Expected Vs. Sample Updates

- For one-step updates, there are three dimensions for how the value updates differ:
    1. Whether a model updates the state values or action values
    2. Whether they estimate the value for the optimal policy or for an arbitrary given policy
    - These two dimensions give rise to four classes of updates for approximating the four value functions
        1. $q_*$
        2. $v_*$
        3. $q_\pi$
        4. $v_\pi$
- The third dimension is whether the updates are ***expected*** updates, considering all possible events that might happen, or ***sample*** events, considering a single sample of what might happen

![Untitled](./Week%202%20RL%20Basics/Untitled.png)

### Are Expected updates preferred over sample updates?

- Expected updates certainly yield a better estimate because they are uncorrupted by sampling error, but they also require more computation, and computation is often the limiting resource in planning
    - Sample updates are cheaper computationally since we only considers one next state in comparison to expected updates considering all possible next states
- In practice, ***the computation required by update operations is usually dominated by the number of state–action pairs at which Q is evaluated***

# Monte Carlo Methods

- MC methods learn directly from episodes of experience
- MC learns from ***complete episodes;*** no bootstrapping
    - Only works for episodic tasks. We need to be able to terminate in order to compute a value
    - All episodes must terminate
- MC methods use the simplest possible idea: value = mean return

[Monte Carlo Methods](./Monte%20Carlo%20Methods.md)

# Evaluating A Learner

- Value of returned policy
- Computational Complexity (CPU run time / wall clock time)
- Sample Complexity
    - How much data does it need?

# Key Terms & Concepts

- **Distribution Vs. Sample Models:**
    - Some models produce a description of all possibilities and their probabilities; these we call distribution models
        - I.e Dynamic Programming Methods
    - Other models produce just one of the possibilities, sampled according to the probabilities; these we call sample models
        - I.e. Monte Carlo, Q Learning
- State-space planning, which includes the approach we take in this book, is viewed primarily as a search through the state space for an optimal policy or an optimal path to a goal
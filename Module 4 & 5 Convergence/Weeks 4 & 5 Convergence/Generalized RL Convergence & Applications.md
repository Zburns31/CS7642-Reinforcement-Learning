# Summary

- The paper provides a framework for Generalized Markov Decision Processes and how they can be solved
    - This includes ordinary MDP’s, Two-Player Games (Zero-Sum) and MDP’s under a worst-case optimality criterion as special cases
- Generalized MDP’s provide a foundation for using RL in MDP’s, Risk-Sensitive RL, Exploration-Sensitive RL and RL in simultaneous-action games
    - All these models have a notion of an optimal value function and an optimal policy, and that a general form of the value-iteration algorithm converges to the optimal value function

# The Generalized Model

- The generalized Bellman equations can be written as (1):

$$
V^*(x) = \otimes_a^x \Biggl ( R(x,a) + \gamma \oplus_y^{x,a} V^*(y) \Biggr )
$$

- Where:
    - $\otimes_a^x$ is an operator that summarizes values over actions as a function of the state
    - $\oplus_y^{x,a}$ is an operator that summarizes values over next states as a function of the state and action
    - I.e. For MDP’s:
        - $\otimes_a^x f(x,a) = \max_a f(x,a)$
        - $\oplus_y^{x,a} g(y) = \sum_y P(x,a,y)g(y)$
- The value functions defined by the generalized MDP model can be interpreted as the total value of the rewards received by an agent selection actions in a stochastic environment
    - The $\otimes_a^x$ operator defines how the value of the next state should be used in assigning value to the current state
    - The $\oplus_y^{x,a}$ operator defines how an optimal agent should choose actions
- The essence of the generalized MDP framework is that whenever the operators $\oplus$ and $\otimes$ satisfy certain non-expansion properties, then (1) us a characterization of the unique optimal value function
    - In this generalized model, the value of a state is defined by the sum of rewards along a trajectory; however, we allow other types of summaries to take the place of maximization and expectation in the MDP model

## Properties of Generalized MDP’s

- The state space $S$ can be infinite
    - To prevent this from making the transition function unwieldy, every state-action pair can only lead to a finite set of next states
    - The transition probabilities for taking an action $a$ in state $x$ are zero for all states except those in the subset $N(x,u)$

# Summary Operators

- For a summary operator $\odot$ to be a ***non-expansion,*** it must satisfy two constraints:
    - Given functions $h$ and $h'$ over a finite set $I$:

        $$
        \min_{i \in I} h(i) \le \odot_{i \in I} h(i) \le \max_{i \in I} h(i)

        \newline
        and \space \newline

        |\odot_{i \in I} h(i) - \odot_{i \in I} h'(i)| \le \max_{i \in I} |h(i) - h'(i)|
        $$

    - Notes:
        - The first condition states that the summary of a function must lie between the largest and smallest value of the function
        - The second condition states that the difference between the summaries of two different functions must be no larger than the largest difference between the summaries of two different functions

### Summary Operators Reference

- From Littman PHD Thesis (1996)

![Untitled](./Generalized%20RL%20Convergence%20&%20Applications/Untitled.png)

# Key Terms & Concepts

- **RL Scenario:** defined by the experience presented to the agent at each step and the criterion for evaluation the agents behaviour
- **$L_\infin$ Norm (Max Norm):** Represents the maximum difference (distance) between any two points in the given value functions
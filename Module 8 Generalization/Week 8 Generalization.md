# Readings

- Sutton & Barto: 9.1-9.5, 9.7, 9.12, 10.1, 10.6, 11.1-11.3, 11.10, 13.1-13.8
- Fong (1995)
- Li, Littman, Walsh (2008)

# Generalization

- ***How do we generalize from seen to unseen states??***
    - Translate the RL problem into a ML problem by using the states and extraneous information as the feature set
- **Basic Update Rule**
    - **General Form: $Q(s,a) = F(w^a, f(s))$**
        - We try to represent our Q function as a set of parameters as well as using our set of features and feed this into a function approximator

# Approximate Solution Methods: Introduction

- The problem with large state spaces is not just the memory needed for large tables, but the time and data needed to fill them accurately
- In many of our target tasks, almost every state encountered will never have been seen before
    - To make sensible decisions in such states it is necessary to generalize from previous encounters with different states that are in some sense similar to the current one
    - How can experience with a limited subset of the state space be usefully generalized to produce a good approximation over a much larger subset?
    - Approximating $v_\pi$ from experience generated using a known policy $\pi$
- Function approximation is an instance of supervised learning
    - The approximate value function is represented not as a table but as a parametrized function with weight vector $w \in \Reals^d$
        - We hope that $\hat v(s, w) \approx v_\pi(s)$
    - $v_\pi$ might be a linear function in features of the state, or a neural network, or the function computed by a decision tree
- Typically, the number of weights (the dimensionality of w) is much less than the number of states (d << |S|), and changing one weight changes the estimated value of many states
    - Consequently, when a single state is updated, the change generalizes from that state to affect the values of many other states
- Extending reinforcement learning to function approximation also makes it applicable to ***partially observable problems***, in which the full state is not available to the agent

# Value Function Approximation

- All the prediction methods covered so far involve an update to an estimated value function that shifts its value towards a backed-up value (update **target**). Let’s denote an individual update by $s \rightarrow v$
    - Monte Carlo: $S_t \rightarrow G_t$
    - TD(0): $S_t \rightarrow R_{t+1} + \gamma \hat v(S_{t+1}, w_t)$
    - N-Step TD: $S_t \rightarrow G_{t:t+n}$
    - Dynamic Programming: $S_t \rightarrow \mathbb{E_\pi}[R_{t+1} + \gamma \hat v(S_{t+1}, w_t) | S_t = s]$
- We can interpret each update as an example of the desired input-output behaviour of the value function.
    - up to now the update was trivial: table entry for $s$
    - $s$’s estimated value is shifted a fraction of the way towards the update and other states’ estimates are left unchanged
    - now updating s generalizes so that the estimated value of many other states are changed as well
- In RL, it is very important that learning is able to happen online, while the agent interacts with its environment or with a model of the environment
    - To do this, requires methods that are able to learn efficiently from incrementally acquired data
    - RL learning generally requires function approximation methods able to handle ***non-stationary target functions (target functions that change over time)***

# Baird’s Counter Example: Linear Value Function Approximation

- The counterexample demonstrates that using linear function approximation for value functions can lead to inaccurate and unstable estimates in certain scenarios
- In the counterexample, Baird considered a specific type of MDP with two states and two actions
    - The reward function is such that the value of each state is a weighted sum of the value of the other state, and the transition probabilities are such that each action transitions to the other state with equal probability
    - Baird showed that when using linear function approximation with a certain type of feature encoding, the value estimates can oscillate and fail to converge, even in the limit of infinite data
- The importance of Baird's counterexample is that it highlights the need for caution when using linear function approximation in reinforcement learning
    - While linear function approximation can be efficient and effective in many scenarios, it is not always guaranteed to provide accurate and stable estimates

# Averagers

- The value of any point is the convex combination of a set of anchor points
- **Connection to MDP’s**
    - Transitioning to the mini set of basis states results in another MDP
    - The basis states represent a set of states. We’re averaging over a set of states

![Untitled](./Week%208%20Generalization/Untitled.png)

# The Prediction Objective

- Assumptions we made in the tabular setting:
    - No need to specify an explicit objective for the prediction because the learned value could come equal with the true value
    - An update at one state does not affect any other state
- Now these two assumptions are false
    - Making one state’s estimate more accurate means making others’ less accurate so we need to know which states we care most about with a distribution $μ(s)≥ 0$ representing how much we care about the error in each state $s$
        - Error in the state means the square of the difference between the approximate value $\hat v(s,w)$ and the true value $v_\pi (s)$
        - Weighting this over the state space by $\mu$, we obtain a natural objective function, the ***Mean Squared Value Error:***

    $$
    \overline{VE}(w) = \sum_{s \in S} \mu(s)[v_\pi(s) - \hat v(s,w)]^2
    $$

    - The square root of this measure (the root $\overline{VE}$), gives a rough measure of how much the approximate values differ from the true values and is often used in plots
    - Often $\mu(s)$ is chosen to be the fraction of time spent in state $s$
    - ***Under on-policy training, this is called the on-policy distribution***

# Stochastic Gradient and Semi-Gradient (Bootstrapping) Methods

- [Gradient Based Methods](./Week%208%20Generalization/Gradient%20Based%20Methods.md)

# Function Approximation Methods

- [Linear Methods](./Week%208%20Generalization/Linear%20Methods.md)
- [Nonlinear Function Approximation](./Week%208%20Generalization/Nonlinear%20Function%20Approximation.md)

# Control Methods with Approximation

- [On-Policy Control with Approximation](./Week%208%20Generalization/On-Policy%20Control%20with%20Approximation.md)
- [Off-policy Methods with Approximation](./Week%208%20Generalization/Off-policy%20Methods%20with%20Approximation.md)

# Policy Gradient Methods

- [Policy Gradient Methods](./Week%208%20Generalization/Policy%20Gradient%20Methods.md)

# Knows What it Knows Framework (KWIK)

- [Knows What It Knowns: Self-Aware Learning](./Week%208%20Generalization/Knows%20What%20It%20Knowns%20Self-Aware%20Learning.md)

# Key Terms & Concepts

- ***State aggregation*** is a simple form of generalizing function approximation in which states are grouped together, with one estimated value (one component of the weight vector w) for each group
- **On-Policy vs. Off-Policy Distribution**
    - On-Policy Distribution: the on-policy distribution was defined as the distribution of states encountered in an MDP while following the target policy

# Resources

- **Notes**
    - [https://lcalem.github.io/blog/2018/12/22/sutton-chap09](https://lcalem.github.io/blog/2018/12/22/sutton-chap09)
    - [http://www.scholarpedia.org/article/Temporal_difference_learning](http://www.scholarpedia.org/article/Temporal_difference_learning)
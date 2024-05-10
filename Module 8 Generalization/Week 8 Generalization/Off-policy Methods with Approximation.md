# Off-Policy Value Function Approximation

- Recall that in off-policy learning we seek to learn a value function for a target policy $\pi$, given data due to a different behaviour policy $b$
    - In the prediction case, both policies are static and given, and we seek to learn either state values $\hat v \approx v_\pi$ or action values $\hat q \approx q_\pi$
    - In the control case, action values are learned, and both policies typically change during learning-$\pi$ being the greedy policy with respect to $\hat q$, and $b$ being something more exploratory such as the $\epsilon$-greedy policy with respect to $\hat q$
- The challenge of off-policy learning can be divided into two parts, one that arises in the tabular case and one that arises only with function approximation
    - The first part of the challenge has to do with the target of the update (not to be confused with the target policy), and the second part has to do with the distribution of the updates

# Semi-Gradient Methods

- Semi-gradient methods address the first challenge for off-policy function approximation, which has to do with the target of the update

# Off-Policy Divergence

- This addresses the second challenge for off-policy function approximation, which has to do with the fact that the distribution of updates does not match the on-policy distribution
- Sections to read: 11.1-11.3

# The Deadly Triad

- Our discussion so far can be summarized by saying that the danger of instability and divergence arises whenever we combine all of the following three elements, making up what we call the deadly triad:
    - **Function Approximation:** A powerful, scalable way of generalizing from a state space much larger than the memory and computational resources
    - **Bootstrapping:** Update targets that include existing estimates (as in dynamic programming or TD methods) rather than relying exclusively on actual rewards and complete returns (as in MC methods)
    - **Off-Policy Training:** Training on a distribution of transitions other than that produced by the target policy
        - Sweeping through the state space and updating all states uniformly, as in dynamic programming, does not respect the target policy and is an example of off-policy training
- If any two elements of the deadly triad are present, but not all three, then instability can be avoided
    - Can we give up any of the three?
        - **Function Approximation:**
            - We need methods that scale to large problems and to great expressive power. We need at least linear function approximation with many features and parameters
        - **Bootstrapping:**
            - Doing without bootstrapping is possible, at the cost of computational and data efficiency. Perhaps most important are the losses in computational efficiency
        - **Off-policy Learning:**
            - On-policy methods are often adequate. For model-free reinforcement learning, one can simply use Sarsa rather than Q-learning. Off-policy methods free behaviour from the target policy. This could be considered an appealing convenience but not a necessity
            - In these use cases, the agent learns not just a single value function and single policy, but large numbers of them in parallel.

# Resources

- Importance Sampling
    - [https://towardsdatascience.com/importance-sampling-introduction-e76b2c32e744](https://towardsdatascience.com/importance-sampling-introduction-e76b2c32e744)
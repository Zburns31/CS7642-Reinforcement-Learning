# What is it?

- In a simple terms, Actor-Critic is a Temporal Difference(TD) version of Policy gradient. It has two networks: Actor and Critic
- Actor-Critic methods are a combined class of methods that learn both policies and value functions
    - These methods are referred to as actor-critic because the policy, which selects actions, can be seen as an actor, and the value function, which evaluates policies, can be seen as a critic
    - Actor-critic methods often perform better than value-based or policy-gradient methods alone on many of the deep reinforcement learning benchmarks
- **Actor-Critic methods**, a hybrid architecture combining a value-based and policy-based methods that help to stabilize the training by reducing the variance:
    - *An Actor* that controls **how our agent behaves** (policy-based method): $\pi_\theta(s,a)$
    - *A Critic* that measures **how good the action taken is** (value-based method) and how it should adjust: $\hat q_w(s,a)$

# Actor & Critic Function Approximators

- To estimate the policy and value function, a SAC agent maintains the following function approximators:
    - **Stochastic actor $\pi(a|s; \theta)$** - The actor, with parameters $\theta$, outputs the mean and standard deviation of conditional Gaussian probability of taking each continuous action $A$ when in state $S$
    - **Q-Value Critics $Q_k(s, a; \phi_k)$** - The critics, each with parameters $\phi_k$, take observation $S$ and action $A$ as inputs and return the corresponding expectation of the value function, which includes both the long-term reward and entropy
    - **Target Network Critics $Q_{tk}(s, a; \phi_k)$ -** To improve the stability of the optimization, the agent periodically sets the target critic parameters $\phi_{tk}$ **to the latest corresponding critic parameter values
        - The number of target critics matches the number of critics

## Action Generation

- The actor in a SAC agent generates mean and standard deviation outputs. To select an action, the actor first randomly selects an unbounded action from a Gaussian distribution with these parameters. During training, the SAC agent uses the unbounded probability distribution to compute the entropy of the policy for the given observation
    - If the action space of the SAC agent is bounded, the actor generates bounded actions by applying $tanh$ and ***scaling*** operations to the unbounded action. This creates Discrete Action Space SAC (DASAC)

        ![Untitled](./Actor-Critic%20Methods/Untitled.png)


## Target Updates

- SAC agents update their target critic parameters using one of the following target update methods:
    - **Smoothing:** Update the target critic parameters at every time step using smoothing factor $\tau$
    - **Periodic (Fixed):** Update the target critic parameters periodically without smoothing
        - $\phi_{tk} = \phi_k$
    - **Periodic smoothing:** Update the target parameters periodically with smoothing

# How does it work?

Let's see the training process to understand how Actor and Critic are optimized:

1. At each time step $t$, we get the current state $S_t$ from the environment and **pass it as input through our Actor and Critic**
    1. Our Policy takes the state and **outputs an action** $A_t$
2. The Critic takes that action also as input and, using $S_t$ and $A_t$, **computes the value of taking that action at that state: the Q-value**
3. The action $A_t$ performed in the environment outputs a new state $S_{t+1}$ and a reward $R_{t+1}$
4. The Actor updates its policy parameters using the Q value
5. Thanks to its updated parameters, the Actor produces the next action to take at $A_{t+1}$ given the new state $S_{t+1}$
    1. The Critic then updates its value parameters

# Algorithm Overview

- In actor-critic methods, the actor and the critic are trained simultaneously using temporal-difference learning or other similar methods
- During training, the critic estimates the value or Q-function and provides feedback to the actor by calculating a gradient that guides the actor towards taking better actions
- The actor updates its policy parameters to increase the probability of selecting actions that lead to higher rewards
- This process is repeated iteratively, with the actor and critic learning from the agent's experiences in the environment until an optimal policy is learned

# Model Deep Dives

- [Soft Actor Critic](./Actor-Critic%20Methods/Soft%20Actor%20Critic.md)
- [A3C: Asynchronous Advantage Actor-Critic](./Actor-Critic%20Methods/A3C%20Asynchronous%20Advantage%20Actor-Critic.md)
- [A2C: Advantage Actor Critic](./Actor-Critic%20Methods/A2C%20Advantage%20Actor%20Critic.md)

# Resources

- [https://huggingface.co/blog/deep-rl-a2c](https://huggingface.co/blog/deep-rl-a2c)
- [https://www.mathworks.com/help/reinforcement-learning/ug/sac-agents.html](https://www.mathworks.com/help/reinforcement-learning/ug/sac-agents.html)
- [https://medium.com/analytics-vidhya/soft-actor-critic-algorithms-in-deep-reinforcement-learning-a11bedd9aa20](https://medium.com/analytics-vidhya/soft-actor-critic-algorithms-in-deep-reinforcement-learning-a11bedd9aa20)
- [https://towardsdatascience.com/soft-actor-critic-demystified-b8427df61665](https://towardsdatascience.com/soft-actor-critic-demystified-b8427df61665)
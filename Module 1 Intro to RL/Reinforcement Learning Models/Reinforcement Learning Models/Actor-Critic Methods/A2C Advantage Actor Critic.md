# What is it?

- In A3C, workers update the neural networks asynchronously. But, asynchronous workers may not be what makes A3C such a high-performance algorithm
    - Advantage actor-critic (A2C) is a synchronous version of A3C, which despite the lower numbering order, was proposed after A3C and showed to perform comparably to A3C
- **A2C**, or **Advantage Actor Critic**, is a synchronous version of the [A3C](https://paperswithcode.com/method/a3c) policy gradient method. As an alternative to the asynchronous implementation of A3C, A2C is a synchronous, deterministic implementation that waits for each actor to finish its segment of experience before updating, averaging over all of the actors
- The solution to reducing the variance of Reinforce algorithm and training our agent faster and better is to use a combination of policy-based and value-based methods: *the Actor-Critic method*
    - We can stabilize learning further by **using the Advantage function as Critic instead of the Action value function**
    - The idea is that the Advantage function calculates **how better taking that action at a state is compared to the average value of the state**
- An **advantage actor-critic** is just an actor-critic that uses the **advantage function** instead of $V_{\pi_\theta}$
    - Advantage can be estimated using any method (TD error, GAE, etc.)

        ![Untitled](./A2C%20Advantage%20Actor%20Critic/Untitled.png)


# How does it work?

## Weight Sharing Model

- A2C uses a single neural network to share the weights for the policy and value network
    - This is often referred to as a "shared network" or "shared backbone" architecture
    - The shared network has several layers that process the input observations from the environment and output both the policy and value estimates
- Sharing a model can be particularly beneficial when learning from images because feature extraction can be compute-intensive. However, model sharing can be challenging due to the potentially different scales of the policy and value function updates
- Despite sharing the network weights, the policy and value functions are still updated separately in the A2C algorithm
    - The policy is updated based on the advantages calculated using the value function, while the value function is updated based on the expected returns from the observed trajectories

![Untitled](./A2C%20Advantage%20Actor%20Critic/Untitled%201.png)

## Restoring Order in Policy Updates

- Updating the neural network in a asynchronous fashion can be chaotic, yet introducing a lockstep mechanism lowers A3C performance considerably
- In A2C, we move the workers from the agent down to the environment. Instead of having multiple actor-learners, we have multiple actors with a single learner
    - As it turns out, having workers rolling out experiences is where the gains are in policy-gradient methods

# Advantage Function

- [Advantage Functions & GAE](../Advantage%20Functions%20&%20GAE.md)

# Resources

- [https://paperswithcode.com/method/a2c](https://paperswithcode.com/method/a2c)
- [https://avandekleut.github.io/a2c/](https://avandekleut.github.io/a2c/)
# Advantage Functions

- The Advantage function is nothing but difference between Q value for a given state — action pair and value function of the state

$$
A(S_t, A_t) \approx R_t + \gamma R_{t+1} + \gamma^2 R_{t+1} + .... + \gamma^{T-1} R_{T} - v_\pi(S_t)
$$

- This can be intuitively taken as the difference of q value and the average of actions which it would have taken at that state. It tells about the extra reward that could be obtained by the agent by taking that particular action
- But the problem with this method is that both V(S) and Q(s,a) are functions which can solved using TD Error estimation
    - [Resource](https://medium.com/mindboard/advantage-function-in-deep-reinforcement-learning-39a0d809c1ff)
- In other words, the advantage function calculates **the extra reward we get if we take this action at that state compared to the mean reward we get at that state:**
- The extra reward is what's beyond the expected value of that state:
    - If $A(s,a) > 0$ our gradient is **pushed in that direction**
    - If $A(s,a) < 0$ (our action does worse than the average value of that state), **our gradient is pushed in the opposite direction**

# Generalized Advantage Estimation (GAE)

- GAE is a more robust method that combines multiple *n*-step bootstrapping targets in a single target, creating even more robust targets than a single *n*-step return: the λ-target
    - *Generalized advantage estimatio***n** (GAE) is analogous to the λ-target in TD(λ), but for advantages
- GAE is a way of estimating targets for the advantage function which most actor-critic methods can leverage
    - It uses an exponentially weighted combination of n-step action-advantage function targets, the same way the $\lambda$-target is an exponentially weighted combination of n-step state value function targets
- This type of target, which we tune in the same was as the $\lambda$-target, can substantially reduce the variance of the policy-gradient estimates at the cost of some bias

![Untitled](./Advantage%20Functions%20&%20GAE/Untitled.png)

![Untitled](./Advantage%20Functions%20&%20GAE/Untitled%201.png)

![Untitled](./Advantage%20Functions%20&%20GAE/Untitled%202.png)

# Resources

- [Paper](https://arxiv.org/pdf/1506.02438.pdf)
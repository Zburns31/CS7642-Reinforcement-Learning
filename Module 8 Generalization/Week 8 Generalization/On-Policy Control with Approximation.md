# Episodic Semi-Gradient Control

- The general gradient-descent update for action-value prediction is:

    $$
    w_{t+1} = w_t + \alpha[U_t - \hat q(S_t, A_t, w_t)] \nabla \hat q(S_t, A_t, w_t)
    $$

- The update for one-step Sarsa is:

$$
w_{t+1} = w_t + \alpha[\gamma \hat q(R_{t+1} + S_{t+1},A_{t+1}, w_t) - \hat q(S_t, A_t, w_t)] \nabla \hat q(S_t, A_t, w_t)
$$

- If the action set is discrete and not too large, then we can use the previously developed techniques for policy improvement and action selection
    - For each possible action *a* available in the next state $S_{t+1}$, we can compute $\hat q(S_{t+1}, a, w_t)$ and then find the greedy action $A^*_{t+1} = \argmax_a \hat q(S_{t+1}, a, w_t)$
        - Policy improvement is then done (in the on-policy case) by changing the estimation policy to a soft approximation of the greedy policy such as the $\epsilon$-greedy policy
        - Actions are selected according to this same policy

![Untitled](./On-Policy%20Control%20with%20Approximation/Untitled.png)

# Resources

- [https://marcinbogdanski.github.io/rl-sketchpad/RL_An_Introduction_2018/1001_Episodic_Semi_Gradient_Sarsa.html](https://marcinbogdanski.github.io/rl-sketchpad/RL_An_Introduction_2018/1001_Episodic_Semi_Gradient_Sarsa.html)
- [https://tomaxent.com/2017/07/06/On-policy-Control-with-Approximation/](https://tomaxent.com/2017/07/06/On-policy-Control-with-Approximation/)
- [https://lcalem.github.io/blog/2019/01/14/sutton-chap10](https://lcalem.github.io/blog/2019/01/14/sutton-chap10)
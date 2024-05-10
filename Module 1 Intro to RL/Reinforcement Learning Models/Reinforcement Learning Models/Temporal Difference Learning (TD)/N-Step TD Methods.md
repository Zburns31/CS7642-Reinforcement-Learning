# What is it?

- N-Step TD learning comes from the idea used in the image below, from Sutton and Barto (2020)
    - Monte Carlo methods uses ***deep backups***, where entire traces are executed and the reward back propagated
    - Methods such as Q-learning and SARSA use ***shallow backups***, only using the reward from the 1-step ahead
    - N-Step learning finds the middle ground: ***only update the Q-function after having explored ahead steps***

![Untitled](./N-Step%20TD%20Methods/Untitled.png)

## How does this differ from one-step TD methods?

- In TD(0) (or other one-step TD methods), the ***target*** is defined as the complete return for Monte Carlo methods and the first reward plus the discounted estimated value of the next state for TD(0)
    - The one-step return is defined as: $G_{t:t+1} = R_{t+1} + \gamma V_t(S_{t+1})$
        - Where $V_t$ is the estimate at time $t$ of $v_\pi$
        - The subscripts $G_{t:t+1}$ represent the truncated return for time $t$ up until time $t+1$ with the discounted estimate $\gamma V_t(S_{t+1})$ taking the place of the other terms $\gamma R_{t+2} + \gamma^2 R_{t+3} + \gamma^3 R_{t+4} + ... + \gamma^{T-t-1} R_{T}$
    - The target for the N-Step return is defined as:

$$
G_{t:t+n} = R_{t+1} + \gamma R_{t+2} + ... + \gamma^{n-1} R_{t+n} + \gamma^n V_{t+n -1}(S_{t+n})
$$

- Important Notes:
    - All N-Step returns can be considered approximations to the full return, truncated after n steps and then corrected for the remaining missing terms by $V_{t+n-1}(S_{t+n})$
    - If $t+n \ge T$ (if the n-step return extends to or beyond termination), then all the missing terms are taken as 0

# Algorithm Overview

## N-Step State-Value Learning Update

$$
V_{t+n}(S_t) = V_{t+n-1}(S_t) + \alpha [G_{t:t+n} - V_{t+n-1}(S_t)]
$$

- **N-Step Return** **Definition:** Is the sum of the first n rewards plus the estimated value of the states reached in n steps, each appropriately discounted
- The N-Step return uses the value function $V_{t+n-1}$ to correct for the missing rewards beyond $R_{t+n}$
- **Error Reduction Property:** An important property of n-step returns is that their expectation is guaranteed to be a better estimate of $v_\pi$ than $V_{t+n-1}$ is, in a worst state sense
    - That is the worst error of the expected n-step return is guaranteed to be less than or equal to $\gamma^n$ times the worst error under $V_{t+n-1}$
    - Because of the error reduction property, one can show formally that all n-step TD methods converge to the correct predictions under appropriate technical conditions

# Algorithm Pseudocode

![Untitled](./N-Step%20TD%20Methods/Untitled%201.png)

# Resources

- [https://gibberblot.github.io/rl-notes/single-agent/n-step.html](https://gibberblot.github.io/rl-notes/single-agent/n-step.html)
- [https://amreis.github.io/ml/reinf-learn/2017/11/02/reinforcement-learning-eligibility-traces.html](https://amreis.github.io/ml/reinf-learn/2017/11/02/reinforcement-learning-eligibility-traces.html)
- [https://medium.com/analytics-vidhya/reinforcement-learning-temporal-difference-learning-part-1-339fef103850](https://medium.com/analytics-vidhya/reinforcement-learning-temporal-difference-learning-part-1-339fef103850)
# How does it work?

- The main idea is to simply switch states for actions (state–action pairs) and then use an $\epsilon$-greedy policy

### **Update Rule**

$$
Q_{t+n}(S_t, A_t) = Q_{t+n-1}(S_t, A_t) + \alpha [G_{t:t+n} - Q_{t+n-1}(S_t, A_t)]
$$

- Notes:
    -

# Algorithm Overview

- While conceptually this is not so difficult, an algorithm for doing n-step learning needs to store the rewards and observed states for $n$ steps, as well as keep track of which step to update. An algorithm for n-step SARSA is shown below

![Untitled](./N-Step%20SARSA/Untitled.png)

![Untitled](./N-Step%20SARSA/Untitled%201.png)

# Resources

- [https://gibberblot.github.io/rl-notes/single-agent/n-step.html](https://gibberblot.github.io/rl-notes/single-agent/n-step.html)
- [https://medium.com/zero-equals-false/n-step-td-method-157d3875b9cb](https://medium.com/zero-equals-false/n-step-td-method-157d3875b9cb)
- [https://rl-book.com/learn/value_methods/n_step/](https://rl-book.com/learn/value_methods/n_step/)
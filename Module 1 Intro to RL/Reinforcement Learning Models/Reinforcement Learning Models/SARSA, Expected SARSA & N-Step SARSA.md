# What is SARSA?

- State-Action-Reward-State-Action is a close relative to Q learning
- ***SARSA is an on-policy algorithm: it learns Q-values that answer the question “What would this action be worth in this state, assuming I stick with my policy?”***

## How does it work?

- In previous algorithms and methods, we considered transitions from state to state and learned the values of states
    - Now we consider transitions from state-action pair to state-action pair, and learn the values of these state-action pairs
- SARSA has a very similar update rule to Q learning. The difference here is that SARSA updates with the Q value of the action $a'$ that is ***actually taken***

$$
Q(S_t,A_t) \leftarrow Q(S_t,A_t) + \alpha[R_{t+1} + \gamma Q(S_{t+1},A_{t+1}) - Q(S_t,A_t)]
$$

- Algorithm Notes:
    - This update is done after every transition from a non-terminal state $S_t$
        - If $S_{t+1}$ is a terminal state, then $Q(S_{t+1}, A_{t+1})$ is defined as 0

## Algorithm Overview

The first step is to learn an action-value function rather than a state-value function

- We must estimate $q_\pi(s,a)$ for the current behaviour policy $\pi$ and for all states *s* and actions *a*

## Steps

1. Initialize Q Table with 0’s
2. Loop for desired number of episodes or until termination happens
3. Choose action A based on the policy derived from the Q table
    1. Choose action according to $\epsilon$-greedy action selection
4. Compute SARSA TD update
5. Update state and action

![Untitled](./SARSA,%20Expected%20SARSA%20&%20N-Step%20SARSA/Untitled.png)

## How is it different than Q Learning?

- The update rule is applied at the end of each $s,a,r,s',a'$ quintuplet
- **The difference is subtle:** Whereas Q Learning backs up the Q value from the best action in s’, SARSA waits until an action is taken and backs up the Q value for that action
    - If the agent is greedy and always takes the action with the best Q value, the two algorithms are identical
- **When exploration happens the algorithms differ:** if explorations yields a negative reward, SARSA penalizes the action, while Q learning does not

## Optimality

- The convergence properties of the Sarsa algorithm depend on the nature of the policy’s dependence on Q
    - For example, one could use $\epsilon$-greedy or $\epsilon$-soft policies
        - Sarsa converges with probability 1 to an optimal policy and action-value function as long as all state–action pairs are visited an infinite number of times and the policy converges in the limit to the greedy policy

# Expected SARSA

---

- Expected SARSA is just like Q-learning except that instead of the maximum over next state–action pairs it uses the expected value, taking into account how likely each action is under the current policy

## How it works?

- Expected Sarsa is very similar to Sarsa. However, instead of state-action values being stochastically sampled using our current policy, it computes the expected value over all future state-action pairs, thereby considering how likely each action is under the current policy
-

## Expected SARSA Update Rule

$$
Q(S_t,A_t) \leftarrow Q(S_t,A_t) + \alpha[R_{t+1} + \gamma E_\pi[ Q(S_{t+1},A_{t+1} | S_{t+1}) - Q(S_t,A_t)]
$$

$$
Q(S_t,A_t) \leftarrow Q(S_t,A_t) + \alpha[R_{t+1} + \gamma \sum_a \pi(a|S_{t+1}) Q(S_{t+1},a) - Q(S_t,A_t)]
$$

# N-Step SARSA

- [N-Step SARSA](./SARSA,%20Expected%20SARSA%20&%20N-Step%20SARSA/N-Step%20SARSA.md)

# Resources

- [https://jochemsoons.medium.com/a-comparison-between-sarsa-and-expected-sarsa-66b931202c75](https://jochemsoons.medium.com/a-comparison-between-sarsa-and-expected-sarsa-66b931202c75)
- [https://mpatacchiola.github.io/blog/2017/01/29/dissecting-reinforcement-learning-3.html](https://mpatacchiola.github.io/blog/2017/01/29/dissecting-reinforcement-learning-3.html)
- [https://ai.stackexchange.com/questions/18278/what-are-the-conditions-for-the-convergence-of-sarsa-to-the-optimal-value-functi](https://ai.stackexchange.com/questions/18278/what-are-the-conditions-for-the-convergence-of-sarsa-to-the-optimal-value-functi)
- [https://towardsdatascience.com/reinforcement-learning-with-sarsa-a-good-alternative-to-q-learning-algorithm-bf35b209e1c](https://towardsdatascience.com/reinforcement-learning-with-sarsa-a-good-alternative-to-q-learning-algorithm-bf35b209e1c)
- [https://builtin.com/machine-learning/sarsa](https://builtin.com/machine-learning/sarsa)

### SARSA On-Policy TD Control

- [https://gibberblot.github.io/rl-notes/single-agent/model-free.html#sarsa-on-policy-reinforcement-learning](https://gibberblot.github.io/rl-notes/single-agent/model-free.html#sarsa-on-policy-reinforcement-learning)
- [https://mpatacchiola.github.io/blog/2017/01/29/dissecting-reinforcement-learning-3.html](https://mpatacchiola.github.io/blog/2017/01/29/dissecting-reinforcement-learning-3.html)
- [https://zoo.cs.yale.edu/classes/cs470/materials/hws/hw7/FrozenLake.html](https://zoo.cs.yale.edu/classes/cs470/materials/hws/hw7/FrozenLake.html)
# What are Monte Carlo Methods?

- Monte Carlo methods are ways of solving the reinforcement learning problem based on averaging sample returns

# Why use them?

- Monte Carlo methods require only experience—sample sequences of states, actions, and rewards from actual or simulated interaction with an environment
    - Learning from actual experience is striking because it requires no prior knowledge of the environment’s dynamics, yet can still attain optimal behaviour. Learning from simulated experience is also powerful
- In particular, note that the computational expense of estimating the value of a single state is independent of the number of states
    - This can make Monte Carlo methods particularly attractive when one requires the value of only one or a subset of states
- 3 Main advantages over DP
    1. First, they can be used to learn optimal behaviour directly from interaction with the environment, with no model of the environment’s dynamics
    2. Second, they can be used with simulation or sample models
    3. Third, it is easy and efficient to focus Monte Carlo methods on a small subset of the states


# How does it work?

- Monte Carlo methods sample and average returns for each state–action pair much like the bandit methods we explored in Chapter 2 sample and average rewards for each action
- ***Methods that go all the way until the end of a trajectory (episode) and estimate the value by looking at sample returns***
- **Goal:** Learn $v_\pi$ from episodes of experience under policy $\pi$
- **Idea:** MC policy evaluation uses empirical mean return instead of expected return
    - Only after a **complete episode**, values are updated (because of this algorithm convergence is slow and update happens after a episode is Complete)

## Incremental Mean Updates & Monte Carlo Updates

- MC methods make use of the incremental mean in order to update the expected value function

$$
u_{k-1} + \frac {1} {k}(x_k - u_{k-1})
$$

- Update $V(s)$ incrementally after episode $S_1, A_1, R_2,...., S_T$
- For each state $S_t$ with observed return $G_t$

    $$
    N(S_t) = N(S_t) + 1

    \newline

    V(S_t) = V(S_t) + \dfrac {1} {N(S_t)} (G_t -V(S_t))
    $$


# Caveats with MC Methods

- We need to visit all states infinitely many times in order for the algorithms to converge
- The tasks must be episodic (I.e. not infinite state space. We need to be able to reach the end of the sequence)

# Monte Carlo Prediction

- Rather than ***computing*** a value function from knowledge of the MDP, with Monte Carlo methods we ***learn*** the value function from sample returns with the MDP
    - An obvious way to estimate it from experience, then, is simply to average the returns observed after visits to that state
        - As more returns are observed, the average should converge to the expected value. This idea underlies all Monte Carlo methods
- To learn the value function, we can use two methods for prediction:
    1. **First-Visit MC Prediction**
        1. The first-visit MC method estimates $v_\pi(s)$ as the average of the returns following first visits to s
    2. **Every-Visit MC Prediction**
        1. The every-visit MC method averages the returns following all visits to s

## First-Visit MC Prediction Overview

**Steps:**

1. To evaluate state S
2. The first time-step t that state s is visited in an episode
3. Increment counter $N(s) = N(s) + 1$
4. Increment total return $S(s) = S(s) + G_t$
5. Value is estimated by mean return $V(s) = S(s) / N(s)$
6. By the law of large numbers, $V(s) \rightarrow v_\pi(s) \space as \space  N(s) \rightarrow \infin$

![Untitled](./Monte%20Carlo%20Methods/Untitled.png)

## Algorithm Notes

- Both first-visit MC and every-visit MC converge to $v_\pi(s)$ as the number of visits (or first visits) to s goes to infinity

# Monte Carlo Estimation of Action Values

- If a model is not available, then it is particularly useful to estimate action values (the values of state–action pairs) rather than state values
    - A state– action pair s, a is said to be visited in an episode if ever the state s is visited and action a is taken in it
- The only complication is that many state–action pairs may never be visited. If $\pi$ is a deterministic policy, then in following $\pi$ one will observe returns only for one of the actions from each state
    - With no returns to average, the Monte Carlo estimates of the other actions will not improve with experience. This is a serious problem because the purpose of learning action values is to help in choosing among the actions available in each state
    - To compare alternatives we need to estimate the value of all the actions from each state, not just the one we currently favour
- **Maintaining Exploration Problem**
    - For policy evaluation to work for action values, we must assure continual exploration
    - One way to do this is by specifying that the episodes start in a state–action pair, and that every pair has a nonzero probability of being selected as the start
        - This guarantees that all state–action pairs will be visited an infinite number of times in the limit of an infinite number of episodes

# Monte Carlo Control

![Untitled](./Monte%20Carlo%20Methods/Untitled%201.png)

![Untitled](./Monte%20Carlo%20Methods/Untitled%202.png)

# Off-Policy Prediction via Importance Sampling

# Resources

- [https://mpatacchiola.github.io/blog/2017/01/15/dissecting-reinforcement-learning-2.html](https://mpatacchiola.github.io/blog/2017/01/15/dissecting-reinforcement-learning-2.html)
- [https://towardsdatascience.com/monte-carlo-learning-b83f75233f92](https://towardsdatascience.com/monte-carlo-learning-b83f75233f92)
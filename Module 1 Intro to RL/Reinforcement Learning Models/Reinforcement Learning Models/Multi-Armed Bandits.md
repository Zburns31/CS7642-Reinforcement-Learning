# What are Multi-Armed Bandits (MAB)?

- *Multi-armed bandit* techniques are not techniques for solving MDPs, but they are used throughout a lot of reinforcement learning techniques that do solve MDPs
- The problem of multi-armed bandits can be illustrated as follows:
    - Imagine that you have N number of slot machines (or poker machines in Australia), which are sometimes called one-armed bandits
        - Over time, each bandit pays a random reward from an unknown probability distribution. Some bandits pay higher rewards than others
        - The goal is to maximize the sum of the rewards of a sequence of lever pulls of the machine
- ***The question is:*** over an infinite period of time, without knowing the probability distribution beforehand, how should we select the arms? Multi-armed bandit techniques aim to solve this problem

# How do they work?

- A **multi-armed bandit** (also known as an **N-armed bandit**) is defined by a set of *random variables* $X_{i,k}$ where:
    - $1 \le i \le N$, such that $i$ is the arm of the bandit
    - $k$ is the index of the play of arm $i$
- Successive plays $X_{i,1}, X_{j,2}, X_{k,3}$ are assumed to be independently distributed, but we do not know the probability distributions of the random variables
- The idea is that a gambler iteratively plays rounds, observing the reward from the arm after each round, and can adjust their strategy each time. The aim is to maximize the sum of the rewards collected over all rounds
- Multi-arm bandit strategies aim to learn a *policy* $\pi(k)$, where $k$ is the play

# Metrics for Bandits

1. Identify optimal arm in the limit
    1. Maximizing exploration
2. Maximize (discounted) expected reward
    1. I.e. Balance between exploitation and exploration
3.  Maximize expected reward over finite horizon
4. Identify near-optimal arm ($\epsilon$ close to the optimal arm) with high probability ($1-\delta$) in time $\Tau(k, \epsilon, \delta)$ polynomial in number of arms $k$, $\epsilon$ and $\delta$
5. Nearly maximize reward ($\epsilon$ close to the optimal arm) with high probability ($1-\delta$) in time $\Tau(k, \epsilon, \delta)$ polynomial in number of arms $k$, $\epsilon$ and $\delta$
    1. This is really saying the goal is to pick up as much reward as possible on the way to identify the optimal answer
6. Pull a non-near optimal arm ($\epsilon$ close to the optimal arm) no more than $\Tau''(k, \epsilon, \delta)$ times with high probability ($1-\delta$)
    1. I.e. the number of times we pull a non-optimal arm is small

# Action-Value Methods

- Action-Value methods are techniques that estimate the values of actions and use these estimates to make action selection decisions

### Sample-Average Method

- This method is called the sample-average method for estimating action-values because each estimate is an average of the sample of relevant rewards
- One natural way to estimate action-values is by averaging the rewards actually received:

$$
Q_t(a) = \dfrac {\text{sum of rewards when a taken prior to t}} {\text{number of times a taken prior to t}} = \dfrac {\sum_{i=1}^{t-1}R_i * 1_{A_i=a}} {\sum_{i=1}^{t-1} 1_{A_i=a}}
$$

- Where:
    - $1_{\text{predicate}}$ denotes the random variable that is 1 if predicate is true and 0 otherwise
    - If the denominator is 0, then we define $Q_t(a)$ as some default value, such as 0
- As the denominator goes to infinity, by law the large numbers, $Q_t(a)$ converges to $q_*(a)$

# Incremental Implementation

- How can these averages (mentioned above) be computed in a computationally efficient manner? Ideally with constant memory and constant-per-time-step computation?
- **Incremental Update Rule: $\text {New Estimate} \leftarrow \text {Old Estimate} + \text {Step Size}[\text {Target} - \text {Old Estimate}]$**
    - $Q_{n+1} = Q_n + \dfrac {1} {n}[R_n - Q_n]$
        - This implementation requires memory only for $Q_n$ and $n$, and only a small amount of computation for each new reward
        - The expression $\text {Target} - \text {Old Estimate}$ is an error in the estimate
            - It is reduced by taking a step toward the “Target.” The target is presumed to indicate a desirable direction in which to move, though it may be noisy
        - Good Resource:

            [Q_Learner_Derivation_Summer2022.pdf](../Reinforcement%20Learning%20Models/Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q/Q_Learner_Derivation_Summer2022.pdf)


# Non-stationary Problems

- The averaging methods discussed so far are appropriate for stationary bandit problems, that is, for bandit problems in which the reward probabilities do not change over time
    - As noted earlier, we often encounter reinforcement learning problems that are effectively non-stationary
    - In such cases it makes sense to give more weight to recent rewards than to long-past rewards
        - One of the most popular ways of doing this is to use a constant step-size parameter. We can adjust the incremental update rule above to be:

        $$
        Q_{n+1} = Q_n + \alpha [R_n - Q_n]

        \newline

        Q_{n+1} = (1-\alpha)^n Q_{1} + \sum_{i=1}^n \alpha(1-\alpha)^{n-i}R_i
        $$

        - Where:
            - $\alpha \in (0, 1]$ is a constant
        - This results in $Q_{n+1}$ being a weighted average of past rewards and the initial estimate $Q_{1}$
        - The second equation above is also known as an ***exponential-recency-weighted average***

# Exploration Vs. Exploitation: Approaches

- **Optimistic Initial Values**
- **Epsilon-Greedy**
    - Greedy action selection: $A_t = \argmax_a Q_t(a)$
    - $\epsilon$-Greedy with random action selection:$\begin{cases}
       A_t = \argmax_a Q_t(a) &\text{if } \epsilon >= \text {n} \\
       A_t = \text {random action} &\text{if } \epsilon < n
    \end{cases}$
        - Where *n* represents a random number
- **Upper Confidence Bound**
    - Upper-Confidence Bound action selection uses uncertainty in the action-value estimates for balancing exploration and exploitation
    - Since there is inherent uncertainty in the accuracy of the action-value estimates when we use a sampled set of rewards thus UCB uses uncertainty in the estimates to drive exploration
- **Thompson Sampling (Bayesian)**
- **Gittins Index**
    - Works well for Bandit problems but doesn’t seem to generalize to other RL problems

[Exploration - Exploitation Strategies](../Reinforcement%20Learning%20Models/Exploration%20-%20Exploitation%20Strategies.md)

# Optimistic Initial Values

- All the methods we have discussed so far are dependent to some extent on the initial action-value estimates, $Q_1(a)$
    - In the language of statistics, ***these methods are biased by their initial estimates***
    - For the sample-average methods, the bias disappears once all actions have been selected at least once, but for methods with constant $\alpha$, the bias is permanent, though decreasing over time
- These initial values essentially become a set of parameters that must be picked
    - The upside is that this can be a way inject domain knowledge about what level of rewards to expect
- One approach to encouraging exploration is to set rewards optimistically
    - Whichever actions are initially selected, the reward is less than the starting estimates; the learner switches to other actions, being “disappointed” with the rewards it is receiving
    - The result is that all actions are tried several times before the value estimates converge
    - This approach is not well suited to non-stationary problems because exploration is inherently temporary
        - Indeed, any method that focuses on the initial conditions in any special way is unlikely to help with the general non-stationary case

![Untitled](./Multi-Armed%20Bandits/Untitled.png)

# Upper-Confidence Bound (UCB) Action Selection

- $\epsilon$-greedy action selection forces the non-greedy actions to be tried, but indiscriminately, with no preference for those that are nearly greedy or particularly uncertain
    - It would be better to select among the non-greedy actions according to their potential for actually being optimal, taking into account both how close their estimates are to being maximal and the uncertainties in those estimates
- **The** **Basic Idea** of UCB action selection is that the square-root term is a measure of the uncertainty (or variance) in the estimate of $a$’s value
    - The quantity being max’ed over is thus a sort of upper bound on the possible true value of action $a$ with $c$ determining the confidence level
    - Each time $a$ is selected the uncertainty is presumably reduced: $N_t(a)$ increments, and, as it appears in the denominator, the uncertainty term decreases
        - On the other hand, each time an action other than a is selected, $t$ increases but $N_t(a)$ does not; because $t$ appears in the numerator, the uncertainty estimate increases
    - The use of the natural logarithm means that the increases get smaller over time, but are unbounded; all actions will eventually be selected, but actions with lower value estimates, or that have already been selected frequently, will be selected with decreasing frequency over time
- UCB provides an effective way of doing this to select actions:

$$
A_t = \argmax_a [Q_t(a) + c \sqrt{\dfrac {\ln t} {N_t(a)}}
$$

- Where
    - $\ln t$ denotes the natural logarithm of $t$
    - $N_t(a)$ denotes the number of times that action $a$ has been selected prior to time $t$
        - If $N_t(a)$ = 0, then $a$ is considered to be a ***maximizing action***
    - $c > 0$ controls the degree of exploration

    ![Untitled](./Multi-Armed%20Bandits/Untitled%201.png)

    - $Q_t(a)$ here represents the current estimate for action *a* at time *t*. We select the action that has the highest estimated action-value plus the upper-confidence bound exploration term

        ![Untitled](./Multi-Armed%20Bandits/Untitled%202.png)


# Algorithm Overviews:

- Simple Bandit

![Untitled](./Multi-Armed%20Bandits/Untitled%203.png)

# Optimality & Convergence

## Hoeffding Bounds

- Hoeffding bounds gives us a way of thinking about how many samples we need to accurately learn the value of an arm
    - Gives us PAC-style bounds for bandits
        - Helps us answer: We can get near optimal payoff after a polynomial number of pulls
- Let $X_i$ be an IID (independent and identically distributed) random variable with mean $\mu$
- $\hat \mu$ is our estimate of $M: \sum_i x_i/n$
- The following formula is a $100(1-\delta)$% confidence interval for $\mu$:

$$
[\dfrac{\hat \mu - Z \delta}{\sqrt{n}}, \dfrac{\hat \mu + Z\delta}{\sqrt{n}}]

\space \text{where} \space Z\delta = \sqrt{\frac{1}{2}\ln{\frac{2}{\delta}}}
$$

- Notes:
    - When we want to be more confident (smaller $\delta$), then the width of confidence intervals will increase
        - If we have a lot of data, then the bounds will be smaller
            - This is due to the $\sqrt{n}$ in the denominator

## How many samples do we need?

- We want to be $\epsilon/2$ accurate with probability of $1-\delta/k$

$$
\dfrac{\sqrt{\dfrac{1}{2}\ln(\dfrac{2k}{\delta}})}{\sqrt{c}} \le \epsilon/2
$$

- Where
    - c represents the number of samples
- When solving for C, we end up with:

$$
C \ge 2 \dfrac{1}{\epsilon^2} \ln(\dfrac{2k}{\delta})
$$

- The dependence on $\epsilon$ tells us that we may need to a huge amount of samples to get close to the optimal answer

# Resources

- [https://gibberblot.github.io/rl-notes/single-agent/multi-armed-bandits.html](https://gibberblot.github.io/rl-notes/single-agent/multi-armed-bandits.html)
- [https://yashbonde.github.io/blogs/bartosutton/chap2.html](https://yashbonde.github.io/blogs/bartosutton/chap2.html)
- [https://mpatacchiola.github.io/blog/2017/08/14/dissecting-reinforcement-learning-6.html](https://mpatacchiola.github.io/blog/2017/08/14/dissecting-reinforcement-learning-6.html)
- [https://towardsdatascience.com/solving-the-multi-armed-bandit-problem-b72de40db97c](https://towardsdatascience.com/solving-the-multi-armed-bandit-problem-b72de40db97c)
- [https://www.cs.ubc.ca/labs/lci/mlrg/slides/Multi_armed_bandits.pdf](https://www.cs.ubc.ca/labs/lci/mlrg/slides/Multi_armed_bandits.pdf)

**UCB Action Selection**

- [https://www.geeksforgeeks.org/upper-confidence-bound-algorithm-in-reinforcement-learning/](https://www.geeksforgeeks.org/upper-confidence-bound-algorithm-in-reinforcement-learning/)
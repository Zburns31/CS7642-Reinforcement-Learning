# What is the TD Lambda Algorithm

- The TD($\lambda$) algorithm can be understood as one particular way of averaging n-step updates
- Both TD(0) and TD(1) have updates based on differences between temporally successive predictions. The TD($\lambda$) covers both
- The TD($\lambda$) can be considered a ***K-Step estimator*** depending on which value of $\lambda$ we use. If $\lambda$ is between 0 and 1, then we have a linear combination of many possible estimators
    - This allows us to escape from the drawbacks of both the TD(0) and TD(1) algorithm
        - TD(1) is very inefficient with data since we need to wait until the end of the episode to update since it is essentially an $\infin$-Step estimator
            - There’s a lot of variance in the estimates
        - TD(0) is essentially the maximum likelihood estimate using one-step look ahead
            - However, this has higher bias since we are ***bootstrapping*** (making estimates based on other estimates)

    ![Untitled](./TD%20Lambda%20Algorithm/Untitled.png)


# How is it different than TD(0) and TD(1) Algorithms?

- [Good reference](https://medium.com/@violante.andre/simple-reinforcement-learning-temporal-difference-learning-e883ea0d65b0)

# How does it work?

- This average contains all the n-step updates, each weighted proportionally to $\lambda^{n-1}$, and is normalized by a factor of $1-\lambda$ to ensure that the weights sum to 1
    - Where $\lambda \in [0,1]$
- The resulting update is toward a return, called the $\lambda$-return, defined by:

$$
G^\lambda_t = (1-\lambda) \sum^\infin_{n=1} \lambda^{n-1} G_{t:t+n}
$$

- The diagram below illustrates the weighting on the sequence of n-step returns in the $\lambda$-return
    - The one-step return is given the largest weight, 1 - $\lambda$; the two-step return is given the next largest weight, (1 - $\lambda$); the three-step return is given the weight (1 - $\lambda$)$\lambda^2$ ; and so on
    - ***The weight fades by with each additional step***
    - After a terminal state has been reached, all subsequent n-step returns are equal to the conventional return, $G_t$

    ![Untitled](./TD%20Lambda%20Algorithm/Untitled%201.png)

    - If $\lambda$ = 0, then the overall update reduces to its first component, the one-step TD update, whereas if $\lambda$ = 1, then the overall update reduces to its last component, the Monte Carlo update

![Untitled](./TD%20Lambda%20Algorithm/Untitled%202.png)

- If we want, we can separate these post-termination terms from the main sum, yielding:

$$
G^\lambda_t = (1-\lambda) \sum^{T-t-1}_{n=1} \lambda^{n-1} G_{t:t+n} + \lambda^{T-t-1}G_t
$$

- This equation makes it clearer what happens when $\lambda$ = 1
    - In this case the sum goes to 0, and the remaining term reduces to the conventional return
    - Thus for $\lambda$ = 1, updating according to the $\lambda$-return is a Monte Carlo algorithm
    - On the other hand, if $\lambda$ = 0, then the $\lambda$-return reduces to $G_{t:t+1}$, the one step return
        - I.e. A One step TD method

## Offline Lambda-Return Algorithm

- As an offline algorithm, it makes no changes to the weight vector during the episode
    - Then, at the end of the episode, a whole sequence of off-line updates are made according to our usual semi-gradient rule, using the $\lambda$-return as the target:

    $$
    w_{t+1} = w_t + \alpha[G^\lambda_t - \hat v(S_t, w_t)]\nabla \hat v(S_t, w_t)
    $$

    - Where $t = 0, ..., T-1$
- Note that overall performance of the offline-line $\lambda$-return algorithms is comparable to that of the n-step algorithms
    - In both cases we get best performance with an intermediate value of the bootstrapping parameter, n for n-step methods and for the offline-line $\lambda$-return algorithm
- The approach that we have been taking so far is what we call the theoretical, or ***forward***, view of a learning algorithm
    - For each state visited, we look forward in time to all the future rewards and decide how best to combine them

# TD Lambda

TD($\lambda$) improves over the off-line $\lambda$-return algorithm in three ways

1. First it updates the weight vector on every step of an episode rather than only at the end, and thus its estimates may be better sooner
2. Second, its computations are equally distributed in time rather than all at the end of the episode
3. Third, it can be applied to continuing problems rather than just to episodic problems

## Forward Vs. Backward TD($\lambda)$

- **Forward (Theoretical) View:** For each state visited, we look forward in time to all the future rewards and decide how best to combine them
    - At each state, the update process looks forward to value of `G_t:t+1`, `G_t:t+2` , …, and is based on a weighted value of which to update the current state
    - ***Forward TD($\lambda$) is offline*** meaning you need all the states to calculate your update
- **Backward (Mechanistic) View:**
    - In the backward view of TD($\lambda$), there is an additional memory variable associated with each state, its *eligibility trace*
        - Backward views are more efficient because you already know the trajectory
    - At each moment we look at the current TD error and assign it backward to each prior state according to the state's eligibility trace at that time

## How it works

- With function approximation, the eligibility trace is a vector $z_t \in R^d$ with the same number of components as the weight vector $w_t$
    - Whereas the weight vector is a long-term memory, accumulating over the lifetime of the system, the eligibility trace is a short-term memory, typically lasting less time than the length of an episode
    - Eligibility traces assist in the learning process; their only consequence is that they affect the weight vector, and then the weight vector determines the estimated value

## Algorithm Overview

1. In TD($\lambda$), the eligibility trace vector is initialized to zero at the beginning of the episode, is incremented on each time step by the value gradient, and then fades away by $\gamma \lambda$

$$
z_{-1} = 0

\newline

z_t = \gamma \lambda z_{t-1} = \nabla \hat v(S_t, w_t)
$$

- Eligibility Trace Notes:
    - In the equation above $\nabla \hat v(S_t, w_t)$ is substituted with the eligibility trace
    - The ET acts to memorize all previous states’ derivative with different weigh controlled by $\lambda$

    [Eligibility Trace Breakdown](./TD%20Lambda%20Algorithm/Eligibility%20Trace%20Breakdown.md)

- Where:
    - $0 \le t \le T$
    - $\gamma$ is the discount rate
    - $\lambda$ is the trace-decay parameter
    - $z_t$ is the eligibility trace vector
        - Also called an accumulating trace
    - Notes:
        - The eligibility trace keeps track of which components of the weight vector have contributed, positively or negatively, to recent state valuations, where recent is defined in terms of $\gamma \lambda$
        - The trace is said to indicate the eligibility of each component of the weight vector for undergoing learning changes should a reinforcing event occur. The reinforcing events we are about are the moment-by-moment one-step TD errors

            $$
            \delta_t = R_{t+1} + \gamma \hat v(S_{t+1}, w_t) - \hat v(S_t, w_t)
            $$

1. In TD($\lambda$), the weight vector is updated on each step proportional to the scalar TD error and the vector eligibility trace”

    $$
    w_{t+1} = w_t + \alpha \delta_t z_t
    $$

2. TD($\lambda$) is oriented backward in time
    1. At each moment we look at the current TD error and assign it backward to each prior state according to how much that state contributed to the current eligibility trace at that time

### Additional Notes

- Values of $\lambda$
    - If $\lambda$ = 0, then the trace at $t$ is exactly the value gradient corresponding to $S_t$
        - Thus the TD($\lambda$) update reduces to the one-step semi-gradient TD update
    - For values of larger values of $\lambda$, but still $\lambda$ < 1, more of the preceding states are changed, but each more temporally distance state is changed less because the corresponding eligibility trace is smaller
        - ***We say the earlier states are given less credit for the TD error***
    - If $\lambda$ = 1, then the credit given to earlier states falls only by $\gamma$ per step
        - If $\lambda$ = 1, the algorithm is known as TD(1)
- TD(1) is a way of implementing Monte Carlo algorithms that is more general than those presented earlier and that significantly increases their range of applicability
    - Whereas the earlier Monte Carlo methods were limited to episodic tasks, TD(1) can be applied to discounted continuing tasks as well
    - TD(1) can be performed incrementally and online
    -

## Algorithm Pseudocode

**Pseudocode from Sutton and Barto**

![Untitled](./TD%20Lambda%20Algorithm/Untitled%203.png)

**Pseudocode from lectures**

```python
Loop for each episode:
	For all s, e(s) = 0 at the start of the episode, V_t(s) = V_t-1(s)
	After S_t-1 --> R_t --> S_t (step t):
			# Eligibility of the state we just left gets incremented by 1
			e(s_t-1) = e(s_t-1) + 1

	# Loop over all states
	for all states:
		V_t(s) = V_t(s) + alpha_t (r_t + gammma * V_t-1(S_t) - V_t-1(S_t-1)*e(s)
		e(s) = lambda * gamma * e(s)
```

- Notes:
    - e(s) will be 0 for all states not equal to $s_{t-1}$ and where it is equal to $s_{t-1}$

## TD($\lambda$) Implementation Notes

- Often, values of $\lambda$ between 0.3 and 0.7 perform well empirically

# N-Step Truncated $\lambda$-Return Methods

- The off-line $\lambda$-return algorithm is an important ideal, but it is of limited utility because it uses the $\lambda$-return, which is not known until the end of the episode
    - In the continuing case, the $\lambda$-return is technically never known, as it depends on n-step returns for arbitrarily large n, and thus on rewards arbitrarily far in the future
    - However, the dependence becomes weaker for longer-delayed rewards, falling by for each step of delay
- A natural approximation, then would be to truncate the sequence after some number of steps
    - Our existing notation of n-step returns provides a natural way to do this in which the ***missing rewards are replaced with estimated values***
    - The truncated $\lambda$-return for time $t$, given data only up to some later horizon h, as:

    $$
    G^\lambda_{t:h} = (1-\lambda) \sum^{h-t-1}_{n=1} \lambda^{n-1} G_{t:t+n} + \lambda^{h-t-1}G_{t:h}
    $$

- The truncated $\lambda$-return immediately gives rise to a family of n-step $\lambda$-return algorithms similar to the n-step methods of Chapter 7

    ![Untitled](./TD%20Lambda%20Algorithm/Untitled%204.png)


# Redoing Updates: Online $\lambda$-Return Algorithm

- Choosing the truncation parameter n in Truncated TD($\lambda$) involves a tradeoff
    - N should be large so that the method closely approximates the off-line $\lambda$-return algorithm, but it should also be small so that the updates can be made sooner and can influence behaviour sooner
        - Can we get the best of both? Well, yes, in principle we can, albeit at the cost of computational complexity
- **Basic Idea:** On each time step as you gather a new increment of data, you go back and redo all the updates since the beginning of the current episode
    - The new updates will be better than ones you previously made because now they can take into account the time step’s new data
    - The updates are always towards an n-step truncated $\lambda$-return, but they always use the latest horizon
    - In each pass over that episode, you can use a slightly longer horizon and obtain a slightly better estimate


# True Online TD($\lambda$)

![Untitled](./TD%20Lambda%20Algorithm/Untitled%205.png)

- Note:
    - The eligibility trace used in online TD($\lambda$) is called a dutch trace


# Resources

- [https://medium.com/analytics-vidhya/reinforcement-learning-temporal-difference-learning-part-1-339fef103850](https://medium.com/analytics-vidhya/reinforcement-learning-temporal-difference-learning-part-1-339fef103850)
- [https://towardsdatascience.com/reinforcement-learning-td-λ-introduction-686a5e4f4e60](https://towardsdatascience.com/reinforcement-learning-td-%CE%BB-introduction-686a5e4f4e60)
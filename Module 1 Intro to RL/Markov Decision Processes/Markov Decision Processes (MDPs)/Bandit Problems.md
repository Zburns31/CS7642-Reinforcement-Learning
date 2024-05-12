# What are Multi-Armed Bandit Problems?

- The term "multi-armed bandit" comes from a hypothetical experiment where a person must choose between multiple actions (i.e., slot machines, the "one-armed bandits"), each with an unknown payout
    - ***The goal is to determine the best or most profitable outcome through a series of choices***
- At the beginning of the experiment, when odds and payouts are unknown, the gambler must determine which machine to pull, in which order and how many times
    - This is the “multi-armed bandit problem”
    - This is an example of the ubiquitous tradeoff between ***exploitation*** of the current best action to obtain rewards and ***exploration*** of previously unknown states and actions to gain information, which can in some cases be
    converted into a better policy and better long-term rewards

# Definition of Bandit Problems

- Each arm $M_i$ is a Markov Reward Process (MRP) that is, an MDP wiht only one possible action $a_i$
    - It has states $S_i$, transtion model $P_i(s'|s,a_i)$ and reward $R(s, a,s')$
        - The arm defines a distribution over sequences of rewards $R_{i,0}, R_{i,1}, R_{i,2},...,$ where each $R_{i,t}$ is a random variable
- The overall bandit problem is an MDP: the state space is given by the cartesian product $S = S_1 x ... x S_n$
    - The actions are $a_1,....a_n$
    - The transition model updates the state of which ever arm $M_i$ is selected, according to its specific transition model, leaving the other arms unchanged
    - The discount factor is $\gamma$
- Each of the available ***k*** actions has an ***expected*** reward, where the term ‘expected’ refers to the mean value that would be obtained if that action was repeated multiple times
- The expected reward of an action is known as the ***value*** of that action and is denoted as $Q(a)$. This can be denoted as: $Q(a) = E[R_t|A_t = a]$
    - This can be read as: *the value of action ‘a’ is equal to the expected (mean) value of the reward, given that the action chosen at time-step ‘t’ is action ‘a’*
- ***The key property is that the arms are independent, coupled only by the fact that the agent can work on only one arm at a time***

# Metrics For Armed Bandit Problems

1. Identify Optimal Arm in the Limit
2. Maximize discounted expected reward using Gittens Index
3. Maximize expected reward over finite horizon
4. Identify near-optimal arm ($\epsilon)$ with high probability ($1- \delta)$ in time $T(k, \epsilon, \delta)$

## Metrics

- **Gittins Index**
    - The remarkable thing about the Gittins index is that it provides a very simple optimal policy for any bandit problem: pull the arm that has the highest Gittins index, then update the Gittins indices

    $$
    \lambda = \max_{T>0} \dfrac{E(\sum_{t=0}^{T-1} \gamma^t R_t)} {E(\sum_{t=0}^{T-1} \gamma^t)}
    $$

    - This equation defines a kind of “value” for in terms of its ability to deliver a stream of
    timely rewards
        - The numerator of the fraction represents a utility while the denominator can be thought of as a “discounted time,” so the value describes the maximum obtainable utility per unit of discounted time
-

# Learning From Rewards: How does it work?

### Choosing a Policy

1. **Epsilon-Greedy**
    1. This is an algorithm for continuously balancing exploration with exploitation. (In ‘greedy’ experiments, the lever with highest known payout is always pulled except when a random action is taken)
        1. A randomly chosen arm is pulled a fraction ε of the time. The other 1-ε of the time, the arm with highest known payout is pulled
2. **Upper Confidence Bound**
    1. This strategy is based on the Optimism in the Face of Uncertainty principle, and assumes that the unknown mean payoffs of each arm will be as high as possible, based on observable data
        1. The basic idea is to use the samples from each arm to establish a confidence interval for the value of the arm, that is, a range within which the value can be estimated to lie with high confidence
            1. Then choose the arm with the highest upper bound on its confidence interval

        $$
        UCB(M_i) = \hat{\mu_i} + g(N) / \sqrt{N_i}
        $$

        - **Where:**
            - $\hat{\mu_i}$ current mean value estimate
            - $N_i$ is the number of times arm $M_i$ has been sampled
            - $g(N) / \sqrt{N_i}$ represents some multiple of the standard deviation of the uncertainty in the value
            - $g(N)$ is an appropriately chosen function of N, the total number of samples drawn from all arms

    - [Upper Confidence Bound (UCB)](./Bandit%20Problems/Upper%20Confidence%20Bound%20(UCB).md)


1. **Thompson Sampling (Bayesian)**
    1. With this randomized probability matching strategy, the number of pulls for a given lever should match its actual probability of being the optimal lever
    2. Chooses chooses an arm randomly according to the probability that the arm is in fact optimal, given the samples so far

    - [Thompson Sampling](./Bandit%20Problems/Thompson%20Sampling.md)


Now that we have a policy for deciding what to do, how do we learn from our actions?

### Estimating Sample Means (Streaming Mean)

- Because we don’t know the true value of each action, we need to estimate it through calculating the ***sample mean***
    - Consequently, to keep track of which action is best, as we explore the set of possible actions, we need to make an estimate of each action’s value
        - As time progresses, this estimate should get progressively more accurate and converge upon the true reward value
    - The simplest way to form the sum of all rewards, for any action, is to store each of the rewards and then add them when required
        - However, from a practical point of view this isn’t very efficient, both in terms of storage and computing time
        - ***A better solution is to calculate the new estimated reward based on the last estimate***
    - For an action *a*, the *n*ᵗʰ estimate for the action-value, $Q_n$ is given by the sum of all previous rewards obtained for that action, divided by the number of times that action has been selected
        - $Q_n = \dfrac {1} {n-1} \sum_{i=1}^{n-1} R_i$
    - Rearranging this, we end up with a usable form for the new estimate, expressed in terms of the last estimate $Q_n$ and the new reward $R_n$: $Q_{n+1} = (1-\dfrac {1} {n}) Q_n+ \dfrac {1} {n} R_n$

# Bernoulli Bandit

- Perhaps the simplest and best-known instance of a bandit problem is the Bernoulli bandit, where each arm $M_i$produces a reward of 0 or 1 with a fixed but unknown probability $\mu_i$
    - The state of arm $M_i$ is defines by $s_i$ and $f_i$, the counts of successes (1s) and failures (0s) so far for that arm
    - The transition probability predicts the next outcome to be 1 with probability $\dfrac {s_i} {s_i + f_i}$and 0 with probability $\dfrac {f_i} {s_i + f_i}$
        - The counts are initialized to 1/2 rather than 0/0

    ![Untitled](./Bandit%20Problems/Untitled.png)


# Key Concepts

- **The Exploitation-Exploration Dilemma**
    - If we never try anything new, if we always stick to the safe bet, we don’t know what we are missing
        - Sometimes we aren’t missing much of anything, and regret not sticking with our preferred choice, yet other times we stumble upon something new that was way better than we thought
    - ***This is the exploitation-exploration dilemma***: do you go with your best choice now, or risk the less certain option with the hope of finding something better
        - Too much exploration, however, means you may end up with a sub-optimal reward once it’s time to stop
- **Regret**
    - Let’s say that we are already aware of the best arm to pull for the given bandit problem. If we keep pulling this arm repeatedly, we will get a maximum expected reward which can be represented as a horizontal line (as shown in the figure below):

        ![Untitled](./Bandit%20Problems/Untitled%201.png)

    - ***But in a real problem statement, we need to make repeated trials by pulling different arms till we am approximately sure of the arm to pull for maximum average return at a time t***
        - **The loss that we incur due to time/rounds spent due to the learning is called regret**
            - In other words, we want to maximize my reward even during the learning phase. Regret is very aptly named, as it quantifies exactly how much you regret not picking the optimal arm

                ![Untitled](./Bandit%20Problems/Untitled%202.png)


# Terms

- **Stopping Time:** If you pull the first arm T times, we say the stopping time is T

# Resources

**Multi-Armed Bandits**

- [https://www.optimizely.com/optimization-glossary/multi-armed-bandit/](https://www.optimizely.com/optimization-glossary/multi-armed-bandit/)
- [https://towardsdatascience.com/solving-the-multi-armed-bandit-problem-b72de40db97c](https://towardsdatascience.com/solving-the-multi-armed-bandit-problem-b72de40db97c)
- [https://www.analyticsvidhya.com/blog/2018/09/reinforcement-multi-armed-bandit-scratch-python/](https://www.analyticsvidhya.com/blog/2018/09/reinforcement-multi-armed-bandit-scratch-python/)
- [https://compneuro.neuromatch.io/tutorials/W3D4_ReinforcementLearning/student/W3D4_Tutorial2.html](https://compneuro.neuromatch.io/tutorials/W3D4_ReinforcementLearning/student/W3D4_Tutorial2.html)
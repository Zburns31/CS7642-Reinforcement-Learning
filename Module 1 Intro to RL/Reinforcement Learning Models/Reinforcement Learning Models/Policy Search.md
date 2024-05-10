# Policy Search

---

# What is Policy Search?

- **Idea:** Keep twiddling the policy as long as its performance improves, then stop
- Policy Search is an alternative method is to search directly in (some subset of) the policy space, in which case the problem becomes a case of [stochastic optimization](https://en.wikipedia.org/wiki/Stochastic_optimization)
    - The two approaches available are ***gradient-based and gradient-free methods***
- Policy-search methods operate directly on a representation of the policy, attempting to improve it based on observed performance
    - The variation in the performance in a stochastic domain is a serious problem; for simulated domains this can be overcome by fixing the randomness in advance

# How does it work?

- So the main idea is to be able to determine at a state **(*s*)** which action to take in order to maximize the reward
- ***The way to achieve this objective is to fine tune a vector of parameters noted 𝜽 in order to select the best action to take for policy 𝜋***
    - The policy is denoted as $\pi(a|s, \theta) = P(A_t =a | S_t=s, \theta_t = \theta)$ which means that the policy $\pi$ is the probability of taking action a when at state s and the parameters are $\theta$
- In policy approximation methods, we omit the notion of learning value functions, instead **tuning the policy directly**
    - We **parameterize the policy** with a set of parameters θ and tweak θ to improve the policy π

### Blue Print

- Find an objective function which can be used to assess the effectiveness of
the policy. In other words telling how good the result given by the
policy.
- Define the policies
    - What we mean is to list some useful policies that can be used in the learning process.
- Naive Algorithm
    - Propose an algorithm that makes direct use of the policies in order to learn the parameters.
- Improved Algorithms
    - Find algorithms that improve the objective function in order to maximize the effectiveness of the policy

# Stochastic Policies

- At first it is important to note that stochastic does not mean randomness in all states, but it can be stochastic in some states where it makes sense
- Usually maximizing reward leads to deterministic policy. But in some cases deterministic policies are not a good fit for  the problem, for example in any two player game, playing deterministically means the other player will be able to come with counter measures in order to win all the time
    - For example in Rock-Paper-Scissors game, if we play deterministically meaning the same shape every time, then the other player can easily counter our policy and wins every game
- One problem with policy representations of the kind given is that the policy is a discontinuous function of the parameters when the actions are discrete
    - That is, there will be values $\theta$  of such that an infinitesimal change in $\theta$ causes the policy to switch from one action to another
        - This means that the value of the policy may also change discontinuously, which makes gradient-based search difficult
        - ***For this reason, policy search methods often use a stochastic policy representation $\pi_\theta(s,a)$, which specifies the probability of selecting action a in state s***
- **An issue arises when the environment (or policy) is non-deterministic**
    - The problem is that the total reward for each trial may vary widely, so estimates of the policy value from a small number of trials will be very unreliable
- For nondeterministic policies $\pi_\theta(s,a)$, we can obtain an unbiased estimate of the gradient at $\theta$, $\nabla_\theta \rho(\theta)$, directly from the results of trials executed at $\theta$
    - The policy value is just the expected value of the reward:
    
    $$
    \nabla_\theta \rho(\theta) = \nabla_\theta \sum_a R(s_0, a, s_0) \pi_\theta(s_0,a) = \sum_a R(s_0, a, s_0) \nabla_\theta \pi_\theta(s_0,a)
    $$
    

# Reinforce Algorithm

- [https://towardsdatascience.com/policy-gradient-methods-104c783251e0](https://towardsdatascience.com/policy-gradient-methods-104c783251e0)
- [https://jonathan-hui.medium.com/rl-policy-gradients-explained-9b13b688b146](https://jonathan-hui.medium.com/rl-policy-gradients-explained-9b13b688b146)
- [https://towardsdatascience.com/policy-gradients-in-a-nutshell-8b72f9743c5d](https://towardsdatascience.com/policy-gradients-in-a-nutshell-8b72f9743c5d)
- [https://towardsdatascience.com/policy-gradients-in-reinforcement-learning-explained-ecec7df94245](https://towardsdatascience.com/policy-gradients-in-reinforcement-learning-explained-ecec7df94245)

# Key Concepts

- **Correlated Sampling**
    - Consider the following task: given two blackjack policies, determine which is best. The policies might have true net returns per hand of, say, % and %, so finding out which is better is very important
        - One way to do this is to have each policy play against a standard “dealer” for a certain number of hands and then to measure their respective winnings
            - The problem with this, as we have seen, is that the winnings of each policy fluctuate wildly depending on whether it receives good or bad cards
        - The same issue arises when using random sampling to compare two adjacent policies in a hill-climbing algorithm
    - **A better solution is to generate a certain number of hands in advance and have each agent play the same set of hands**
        - In this way, we eliminate measurement error due to differences in the cards recieved

# Pros & Cons

## **Advantages**

- Better convergence properties
- Effective in high-dimensional or continuous action spaces When the space is large, the usage of memory and computation consumption grows rapidly
    - The policy based RL avoids this because the objective is to learn a set of parameters that is far less than the space count
- Can learn stochastic policiesStochastic policies are better than deterministic policies, especially in 2 players game where if one player acts deterministically the other player will develop counter measures in order to win

## **Disadvantages**

- Typically converge to a local rather than global optimum
- Evaluating a policy is typically inefficient and high variancePolicy based RL has high variance, but there are techniques to reduce this variance

# Terms

- **Policy Value:** $\rho(\theta)$  represents the policy value, that is the expected reward-to-go when $\pi_\theta$ is executed
- **Policy Gradient Vector: $\nabla_\theta \rho(\theta)$**
- **Empirical Gradient:** We can follow the empirical gradient by hill climbing—that is, evaluating the change in policy value for small increments in each
# Intro to Policy Gradient Methods

- [Policy Gradient Methods](../../Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Policy%20Gradient%20Methods.md)
- [Advantage Functions & GAE](../../Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Advantage%20Functions%20&%20GAE.md)

# What is it?

- Think of PPO as an algorithm with the same underlying architecture as A2C. PPO can reuse much of the code developed for A2C
    - That is, we can roll out using multiple environments in parallel, aggregate the experiences into mini-batches, use a critic to get GAE estimates, and train the actor and critic in a way similar to training in A2C
- The critical innovation in PPO is a ***surrogate objective function*** that allows an on-policy algorithm to perform multiple gradient steps on the same mini-batch of experiences
    - In general, on-policy methods need to discard experience samples immediately after stepping the optimizer
- The main idea behind PPO is to find a policy (a mapping from states to actions) that maximizes the expected cumulative reward of an agent over time, while ensuring that the changes to the policy are not too large
    - This is done by optimizing a surrogate objective function that approximates the policy's performance
-

# How does it work?

- PPO introduces a clipped objective function that prevents the policy from getting too different after an optimization step
    - By optimizing the policy conservatively, we not only prevent performance collapse due to the innate high variance of on-policy gradient methods but also can reuse mini-batches of experiences and perform multiple optimization steps per mini-batch
    - The ability to reuse experiences makes PPO a more sample-efficient method than other on-policy methods
    - To do that, we use a ratio that will indicates the difference between our current and old policy and clip this ratio from a specific range $[1 - \epsilon, 1 + \epsilon]$

# Intuition Behind PPO

- The idea with Proximal Policy Optimization (PPO) is that we want to improve the training stability of the policy by limiting the change you make to the policy at each training epoch: **we want to avoid having too large policy updates**
    - For two reasons:
        - We know empirically that smaller policy updates during training are **more likely to converge to an optimal solution**
        - A too big step in a policy update can result in falling “off the cliff” (getting a bad policy) **and having a long time or even no possibility to recover**

## Same Architecture as A2C

- We sample parallel environments to gather the mini-batches of data and use GAE for policy targets

## Batching Experiences

- One of the features of PPO that A2C didn’t have is that with PPO, we can reuse experience samples
    - To deal with this, we could gather large trajectory batches, as in NFQ, and “fit” the model to the data, optimizing it over and over again
    - However, a better approach is to create a replay buffer and sample a large mini-batch from it on every optimization step
        - That gives the effect of stochasticity on each mini-batch because samples aren’t always the same, yet we likely reuse all samples in the long term

## Clipping Policy Updates

- The main issue with the regular policy gradient is that even a small change in parameter space can lead to a big difference in performance
    - The discrepancy between parameter space and performance is why we need to use small learning rates in policy-gradient methods, and even so, the variance of these methods can still be too large
    - The whole point of clipped PPO is to put a limit on the objective such that on each training step, the policy is only allowed to be so far away
    - Intuitively, you can think of this clipped objective as a coach preventing overreacting to outcomes
        - Did the team get a good score last night with a new tactic? Great, but don’t exaggerate. Don’t throw away a whole season of results for a new result. Instead, keep improv- ing a little bit at a time

![Untitled](./Proximal%20Policy%20Optimization/Untitled.png)

## Clipping Value Function Updates

- We can apply a similar clipping strategy to the value function with the same core concept: ***let the changes in parameter space change the Q-values only this much, but not more***
- As you can tell, this clipping technique keeps the variance of the things we care about smooth, whether changes in parameter space are smooth or not
    - We don’t necessarily need small changes in parameter space; however, we’d like level changes in performance and values

![Untitled](./Proximal%20Policy%20Optimization/Untitled%201.png)

# Advantages & Disadvantages

## Advantages

- One of the key advantages of PPO over other reinforcement learning algorithms is that it is designed to handle continuous action spaces, which are common in robotics and other real-world applications
- Additionally, PPO is relatively easy to implement and can be parallelized efficiently

# Resources

- [Paper](https://arxiv.org/pdf/1707.06347v2.pdf)
- [https://openai.com/research/openai-baselines-ppo](https://openai.com/research/openai-baselines-ppo)
- [https://huggingface.co/blog/deep-rl-ppo](https://huggingface.co/blog/deep-rl-ppo)
- [https://towardsdatascience.com/proximal-policy-optimization-ppo-explained-abed1952457b](https://towardsdatascience.com/proximal-policy-optimization-ppo-explained-abed1952457b)
- [https://spinningup.openai.com/en/latest/algorithms/ppo.html](https://spinningup.openai.com/en/latest/algorithms/ppo.html)
- [https://annhe.xyz/2021/04/12/policy-gradients/](https://annhe.xyz/2021/04/12/policy-gradients/)
- [https://jonathan-hui.medium.com/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12](https://jonathan-hui.medium.com/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12)
- [https://blog.tylertaewook.com/post/proximal-policy-optimization](https://blog.tylertaewook.com/post/proximal-policy-optimization)
- [https://towardsdatascience.com/proximal-policy-optimization-ppo-explained-abed1952457b](https://towardsdatascience.com/proximal-policy-optimization-ppo-explained-abed1952457b)
- [https://jonathan-hui.medium.com/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12](https://jonathan-hui.medium.com/rl-proximal-policy-optimization-ppo-explained-77f014ec3f12)
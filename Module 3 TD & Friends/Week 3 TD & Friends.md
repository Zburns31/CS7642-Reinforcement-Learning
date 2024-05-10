# Topics Summary

- Temporal Difference Learning Continued
    - N-Step Bootstrapping
    - Eligibility Traces —> TD($\lambda$)
- In the GPI framework, the Monte-Carlo and the TD methods are techniques for policy *evaluation,* where Q learning/Sarsa are temporal difference methods that actually implement the policy *improvement* step in addition to evaluation to make it a full model-free control learning technique

# Issues with SARSA & TD(0) Learning

- In previous sections, we looked at two fundamental temporal difference (TD) methods for reinforcement learning: Q-learning and SARSA
- These two methods have some weaknesses in this basic format:
    1. Unlike Monte-Carlo methods, which reach a reward and then back propagate this reward, TD methods use bootstrapping (they estimate the future discounted reward using), which means that for problems with sparse rewards, it can take a long time to for rewards to propagate throughout a Q-function
    2. Rewards can be sparse, meaning that there are few state/actions that lead to non-zero rewards. This is problematic because initially, reinforcement learning algorithms behave entirely randomly and will struggle to find good rewards
    3. Both methods estimate a Q-function $Q(s,a)$, and the simplest way to model this is via a Q-table. However, this requires us to maintain a table of size $|A| \space x \space |S|$, which is prohibitively large for any non-trivial problem
    4. Using a Q-table requires that we visit every reachable state many times and apply every action many times to get a good estimate of
        1. Thus, if we never visit a state $s$, we have no estimate of $Q(s,a)$, even if we have visited states that are very similar to $s$

# N-Step Learning

- To get around limitations 1 and 2, we are going to look at n-step temporal difference learning: ‘Monte Carlo’ techniques execute entire traces and then back propagate the reward, while basic TD methods only look at the reward in the next step, estimating the future wards
- n-step methods instead look steps ahead for the reward before updating the reward, and then estimate the remainder



- [Temporal Difference Learning (TD)](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Temporal%20Difference%20Learning%20(TD).md)
- [SARSA, Expected SARSA & N-Step SARSA](../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/SARSA,%20Expected%20SARSA%20&%20N-Step%20SARSA.md)

# Eligibility Traces

- Eligibility traces are one of the basic mechanisms of reinforcement learning
    - For example, in the popular TD($\lambda$) algorithm, the refers to the use of an eligibility trace
    - Almost any temporal difference (TD) method, such as Q-learning or Sarsa, can be combined with eligibility traces to obtain a more general method that may learn more efficiently
    - Eligibility Traces unify and generalize TD and Monte Carlo methods

- [Eligibility Traces & Lambda-Return](./Week%203%20TD%20&%20Friends/Eligibility%20Traces%20&%20Lambda-Return.md)

# Sutton (1988): Learning To Predict By The Methods of Temporal Differences

- [Learning to Predict by the Methods of Temporal Differences](./Week%203%20TD%20&%20Friends/Learning%20to%20Predict%20by%20the%20Methods%20of%20Temporal%20Differences.md)

# Tradeoffs: TD Learning Vs. MC Methods

- **MC has high variance, zero bias**
    - Good convergence properties
    - Not very sensitive to initial values
    - Very simple to understand and use
- **TD has low variance, some bias**
    - Usually more efficient than MC
    - TD(0) converges to $v_\pi(s)$
        - But not always with function approximation
    - More sensitive to initial value
- TD exploits the Markov Property while MC does not
    - TD is more efficient in Markov environments
    - MC is usually more efficient in non-Markov environments

# Key Terms & Concepts

- **Forward Views:** Formulations based on looking forward from the updated state
    - Can be complex to implement because the update depends on later things that are not available at that time
- **Backward Views:** It is often possible to achieve nearly the same updates (or sometimes exactly the same updates) with an algorithm that uses the current TD error, looking backward to recently visited states using an eligibility trace
- **Bootstrapping Vs. Sampling**
    - **Bootstrapping** update involves an estimate
        - MC does not bootstrap
        - DP and TD bootstraps
    - **Sampling** update samples an expectation
        - MC and TD samples
        - DP does not sample


# Resources

- TD($\lambda$):
    - [https://amreis.github.io/ml/reinf-learn/2017/11/02/reinforcement-learning-eligibility-traces.html](https://amreis.github.io/ml/reinf-learn/2017/11/02/reinforcement-learning-eligibility-traces.html)
    - [https://lcalem.github.io/blog/2019/02/25/sutton-chap12](https://lcalem.github.io/blog/2019/02/25/sutton-chap12)
    - [https://r-craft.org/r-news/suttons-temporal-difference-learning/](https://r-craft.org/r-news/suttons-temporal-difference-learning/)
    - [https://michaeloneill.github.io/RL-tutorial.html](https://michaeloneill.github.io/RL-tutorial.html)
    - [https://medium.com/@violante.andre/simple-reinforcement-learning-temporal-difference-learning-e883ea0d65b0](https://medium.com/@violante.andre/simple-reinforcement-learning-temporal-difference-learning-e883ea0d65b0)
    - [https://meatba11.medium.com/reinforcement-learning-td-λ-introduction-2-f0ea427cd395](https://meatba11.medium.com/reinforcement-learning-td-%CE%BB-introduction-2-f0ea427cd395)
    - [https://aarl-ieee-nitk.github.io/reinforcement-learning,/value-based-learning,/bootstrapped-learning,/sampled-learning/2019/12/19/Temporal-Difference-Learning.html](https://aarl-ieee-nitk.github.io/reinforcement-learning,/value-based-learning,/bootstrapped-learning,/sampled-learning/2019/12/19/Temporal-Difference-Learning.html)
    - [http://www.scholarpedia.org/article/Temporal_difference_learning](http://www.scholarpedia.org/article/Temporal_difference_learning)
# Policy Gradient Methods: Intro

- So far we’ve only looked at action-value methods which learn values of individual actions and derive a policy from them
    - This includes methods like SARSA, Q-Learning, DQN
    - The policies of these methods are completely inseparable from the learned values
    - **Can we learn the policy directly?**
        - The value function is still necessary because without it we will have no way to understand the expected future reward ****
        - Value function is still necessary but it’s not fully dependent
- The goal of reinforcement learning is to find an optimal behaviour strategy for the agent to obtain optimal rewards
    - The **policy gradient** methods target at modelling and optimizing the policy directly
    - The policy is usually modelled with a parameterized function respect to $\theta, \pi_\theta(a|s)$
        - $\theta$ represents the policy’s parameter vector
        - A value function may still be used to *learn* the policy parameter, but is not required for action selection
        - If we also want to learn a value function, then we will use $w$ to denote the parameters of that function $\hat v(s,\textbf{w})$
- In policy gradient methods, we approximate the policy from the rewards and actions received in our episodes, similar to the way we do it with Q-learning
    - We can do this provided that the policy has two properties:
        1. The policy is represented using some function that is *differentiable* with respect to its parameters. For a non-differentiable policy, we cannot calculate the gradient.
        2. Typically, we want the policy to be *stochastic*. Recall from the section on [policies](https://gibberblot.github.io/rl-notes/single-agent/MDPs.html#sec-mdps-policies) that a stochastic policy specifies a *probability distribution* over actions, defining the probability with which each action should be chosen

# Requirements

- $\pi$ must be differentiable with respect to $\theta$
    - Generally we’ll also want to ensure our policy never becomes deterministic to ensure exploration

# Policy Search

- We have our policy $\pi$ that has a parameter $\theta$. This $\pi$ outputs a probability distribution of actions:

$$
\pi_\theta(a|s) = Pr[a|s]
$$

- This represents the probability of taking action *a* in state *s* with parameters $\theta$
- ***But how do we know if our policy is good?***
    - Remember that policy can be seen as an optimization problem. We must find the best parameters ($\theta$) to maximize a score function, $J(\theta)$

    $$
    J(\theta) = E_{\pi_\theta}[\sum \gamma r]
    $$

    - ***There are 2 key steps:***
        1. Measure the quality of a $\pi$ (policy) with a policy score function $J(\theta)$
        2. Use policy gradient ascent to find the best parameter $\theta$ that improves our $\pi$
- The main idea here is that $J(\theta)$ will tell us how good our $\pi$ is. Policy gradient ascent will help us to find the best policy parameters to maximize the sample of good actions

## Policy Score Function: $J(\theta)$

- To measure how good our policy is, we use a function called the objective function (or Policy Score Function) that calculates the expected reward of policy
- Three methods work equally well for optimizing policies. The choice depends only on the environment and the objectives you have
    - First, in an episodic environment, we can use the start value. Calculate the mean of the return from the first time step (G1). This is the cumulative discounted reward for the entire episode:

        ![Untitled](./Policy%20Gradient%20Methods/Untitled.png)

        - The idea is simple. If I always start in some state s1, what’s the total reward I’ll get from that start state until the end?
            - We want to find the policy that maximizes G1, because it will be the optimal policy. This is due to the reward hypothesis [explained in the first article](https://medium.freecodecamp.org/an-introduction-to-reinforcement-learning-4339519de419)
    - In a continuous environment, we can use the average value, because we can’t rely on a specific start state
        - Each state value is now weighted (because some happen more than others) by the probability of the occurrence of the respected state

            ![Untitled](./Policy%20Gradient%20Methods/Untitled%201.png)

    - Third, we can use the average reward per time step. The idea here is that we want to get the most reward per time step

        ![Untitled](./Policy%20Gradient%20Methods/Untitled%202.png)


## Policy Gradient Ascent

- We have a Policy score function that tells us how good our policy is. Now, we want to find a parameter $\theta$ that maximizes this score function. Maximizing the score function means finding the optimal policy
- To maximize the score function $J(\theta)$, we need to do gradient ascent on policy parameters
    - Gradient ascent is the inverse of gradient descent. Remember that gradient always points to the steepest change
- ***The idea is to find the gradient to the current policy $\pi$ that updates the parameters in the direction of the greatest increase, and iterate***

    $$
    \text {Policy}:  \pi_\theta \\
    \text {Objective Function}:  J(\theta) \\
    \text {Gradient}:  \nabla_\theta J(\theta) \\

    \text {Update}:  \pi \rightarrow \theta + \alpha \nabla_\theta J(\theta)
    $$

    - We want to find the best parameters $\theta^*$, that maximize the score

        ![Untitled](./Policy%20Gradient%20Methods/Untitled%203.png)

    - Our score function can then be defined as:

        ![Untitled](./Policy%20Gradient%20Methods/Untitled%204.png)

        - Which is the total summation of expected reward given policy
    - Our score function can also be defined as:

        ![Untitled](./Policy%20Gradient%20Methods/Untitled%205.png)

        - We know that policy parameters change how actions are chosen, and as a consequence, what rewards we get and which states we will see and how often
        - So, it can be challenging to find the changes of policy in a way that ensures improvement
            - This is because the performance depends on action selections and the distribution of states in which those selections are made
        - Both of these are affected by policy parameters. ***The effect of policy parameters on the actions is simple to find, but how do we find the effect of policy on the state distribution?*** The function of the environment is unknown
            - As a consequence, we face a problem: how do we estimate the $\nabla$ (gradient) with respect to policy  ($\theta$) when the gradient depends on the unknown effect of policy changes on the state distribution?
            - ***The solution will be to use the Policy Gradient Theorem***
                - This provides an analytic expression for the gradient $\nabla$ of $J(\theta)$ (performance) with respect to policy $\theta$ that does not involve the differentiation of the state distribution

# Policy Gradient Theorem

- ***The goal of a policy gradient is to approximate the optimal policy $\pi_\theta(a|s)$ via gradient ascent on the expected return***
    - Gradient ascent will find the best parameters $\theta$  for the particular MDP
- These methods seek to ***maximize*** performance, so their updates approximate gradient descent in $J(\theta)$:

    $$
    \theta_{t+1} = \theta_t + \alpha \nabla J(\theta_t)
    $$

    - Where:
        - $J(\theta)$ is the scalar performance measure
- How do we change the policy parameters to ensure our performance improves??
    - $J(\theta) \approx v_{\pi_\theta}(s_0)$
    - $J(\theta)$ depends on both the actions selected as well as the distribution of states
        - $\theta$ obviously affects the policy and reward
        - $\theta$ also affects the states
            - Recall that given an MDP, we have a distribution over states that describes the transition probabilities
            - We don’t know these with model free algos so choice of $\theta$ impacts which states you see
- The policy gradient theorem provides us a way to obtain this gradient w.r.t $\theta$ without needing the derivative of the state distribution

$$
\nabla J(\theta) = \sum_s u(s) \sum_a q_\pi(s,a)\nabla \pi (a|s, \theta) \newline

E_{\pi}[\sum_a q_\pi(S_t,a)\nabla \pi (a|S_t, \theta)]
$$

- Where:
    - $\sum_s u(s)$ represents the state distribution

![Untitled](./Policy%20Gradient%20Methods/Untitled%206.png)

- Resources:
    - [https://gibberblot.github.io/rl-notes/single-agent/policy-gradients.html](https://gibberblot.github.io/rl-notes/single-agent/policy-gradients.html)
    - [https://sassafras13.github.io/policyMethods/](https://sassafras13.github.io/policyMethods/)

# How do Policy Gradient Methods Work?

- The basic idea of policy gradients is to estimate the gradient of the expected reward with respect to the policy parameters and use this gradient to update the policy parameters in the direction that increases the expected reward
- Here are the steps of the policy gradient algorithm:
    1. Initialize the policy parameters randomly
    2. Collect a batch of trajectories by following the current policy and recording the state, action, and reward at each step
    3. Compute the total reward for each trajectory
    4. Compute the gradient of the expected reward with respect to the policy parameters. This is typically done using the likelihood ratio trick, which involves multiplying the gradient of the log-probability of the action by the total reward
    5. Update the policy parameters using the gradient ascent rule, which increases the policy parameters in the direction of the gradient
    6. Repeat steps 2-5 until the policy converges to a good solution

- The key advantage of policy gradients is that they can learn stochastic policies, which can be more effective in complex environments where there is a high degree of uncertainty and variability
    - Policy gradients can also handle continuous action spaces more naturally than value-based methods, which require discretization or approximation
- However, policy gradients can be computationally expensive and require careful tuning of the learning rate and other hyperparameters. Additionally, they can suffer from high variance in the gradient estimates, which can lead to slow convergence and poor performance
    - Several variations of policy gradients have been proposed to address these issues, including trust region policy optimization (TRPO) and proximal policy optimization (PPO)

# General Notes

- In practice, to ensure exploration we generally require that the policy never becomes deterministic

# Policy Methods

- [Policy Search](./Policy%20Search.md)
- [Proximal Policy Optimization](Policy%20Gradient%20Methods%2022bf657f858643f4b566952601b1be70/Proximal%20Policy%20Optimization%205072d0c22f964e9ebb18b29ae0818830.md)

# Advantages & Disadvantages

### Advantages

- The main advantage of learning parameterized policies is that policies can now be any learnable function
    - Value based methods calculate the maximum value over the actions since we worked with discrete spaces. However, this can be extremely expensive. Additionally, for continuous action spaces, value-based methods are very limited
- The policy can approach a deterministic policy, whereas $\epsilon$-greedy methods always have some chance of selecting randomly
- Enables the selection of actions with arbitrary probabilities
    - This means better performance under partially observable environments. The intuition here is that because we can learn arbitrary probabilities of actions, the agent is less dependent on the markov assumption
        - If the agent can’t distinguish between a handful of states from their emitted observations, the best strategy typically is to act randomly with specific probabilities
- Learning a parameterized policy may be simpler than learning a parameterized action-value function
- The main advantage of policy gradient methods is that in some context it is much easier to estimate the policy function instead of the utility function
    - We know that the estimation of the utility function can be difficult due to the **credit assignment problem**
        - In policy gradients method we bypass this issue and we obtain a better convergence and an increased stability
- The choice of policy parameterization is sometimes a good way of injecting prior knowledge about the desired form of the policy into the reinforcement learning system
    - This is often the most important reason for using a policy-based learning method
- With continuous policy parameterization the action probabilities change smoothly as a function of the learned parameter, whereas in $\epsilon$-greedy selection the action probabilities may change dramatically for an arbitrarily small change in the estimated action values, if that change results in a different action having the maximal value
- Another advantage is that using policy gradient we can approximate a continuous action space
    - This overcomes an important limitation of **Q-Learning** where the max operator only allows us to approximate discrete action spaces

### Disadvantages

- The main disadvantage of policy gradient methods is that they are generally slower and that they often converge to local optimum
    - Having to sample entire trajectories on-policy before each gradient update is slow to begin with, and the high variance in the updates makes the search optimization highly inefficient which means more sampling which means more updates, ad infinitum
- A major problem is ***variance***
    - Evaluating the policy with an high number of trajectories leads to an high variance in the approximation of the function
    - To get a sense for why this is, first considering that in RL we're dealing with highly general problems such as teaching a car to navigate through an unpredictable environment or programming an agent to perform well across a diverse set of video games
    - Therefore, when we're sampling multiple trajectories from our untrained policy we're bound to observe highly variable behaviours
        - Without any a priori model of the system we're seeking to optimize, we begin with a policy whose distribution of actions over a given state is effectively uniform. Of course, as we train the model we hope to shape the probability density so that it's unimodal on a single action, or possibly multimodal over a few successful actions that can be taken in that state
            - However, acquiring this knowledge requires our model to observe the outcomes of many different actions taken in many different states
            - This is made exponentially worse in continuous action or state spaces as visiting even close to every state-action pair is computationally intractable
            - Due to the fact that we're using Monte Carlo estimates in policy gradient, we trade off between computational feasibility and gradient accuracy
                - It's a fine line to walk, which is why variance reduction techniques can potentially yield huge payoffs
    - [Resource](https://mcneela.github.io/machine_learning/2019/06/03/The-Problem-With-Policy-Gradient.html)
    - [https://www.reddit.com/r/reinforcementlearning/comments/d2zzbg/pg_methods_are_high_variance_can_i_measure_that/](https://www.reddit.com/r/reinforcementlearning/comments/d2zzbg/pg_methods_are_high_variance_can_i_measure_that/)

# Key Concepts & Terms

- **Types of Policies**
    - Deterministic: A deterministic policy is policy that maps state to actions. You give it a state and the function returns an action to take
    - Stochastic: A stochastic policy outputs a probability distribution over actions
        - It means that instead of being sure of taking action *a* (for instance left), there is a probability we’ll take a different one (in this case 30% that we take south)
    - [Reference](https://www.freecodecamp.org/news/an-introduction-to-policy-gradients-with-cartpole-and-doom-495b5ef2207f/)

# Resources

Papers

- [Original Paper](https://arxiv.org/pdf/1707.06347.pdf)
- [https://openreview.net/pdf?id=H1tSsb-AW](https://openreview.net/pdf?id=H1tSsb-AW)

Slides & Notes

- [https://faculty.cc.gatech.edu/~bboots3/ACRL-Spring2019/Lectures/PG_notes.pdf](https://faculty.cc.gatech.edu/~bboots3/ACRL-Spring2019/Lectures/PG_notes.pdf)
- [https://www.cc.gatech.edu/classes/AY2022/cs7643_spring/assets/L22_RL3.pdf](https://www.cc.gatech.edu/classes/AY2022/cs7643_spring/assets/L22_RL3.pdf)
- [https://spinningup.openai.com/en/latest/spinningup/rl_intro3.html](https://spinningup.openai.com/en/latest/spinningup/rl_intro3.html)
- [https://huggingface.co/deep-rl-course/unit4/what-are-policy-based-methods?fw=pt](https://huggingface.co/deep-rl-course/unit4/what-are-policy-based-methods?fw=pt)
- [https://huggingface.co/blog/deep-rl-a2c](https://huggingface.co/blog/deep-rl-a2c)
- [https://faculty.cc.gatech.edu/~bboots3/ACRL-Spring2019/Lectures/PI_notes.pdf](https://faculty.cc.gatech.edu/~bboots3/ACRL-Spring2019/Lectures/PI_notes.pdf)

Videos

- [https://www.youtube.com/watch?v=KHZVXao4qXs&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=8](https://www.youtube.com/watch?v=KHZVXao4qXs&list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ&index=8)

Articles

- [https://www.freecodecamp.org/news/an-introduction-to-policy-gradients-with-cartpole-and-doom-495b5ef2207f/](https://www.freecodecamp.org/news/an-introduction-to-policy-gradients-with-cartpole-and-doom-495b5ef2207f/)
- [https://gibberblot.github.io/rl-notes/single-agent/policy-gradients.html](https://gibberblot.github.io/rl-notes/single-agent/policy-gradients.html)
- [https://mcneela.github.io/machine_learning/2019/06/03/The-Problem-With-Policy-Gradient.html](https://mcneela.github.io/machine_learning/2019/06/03/The-Problem-With-Policy-Gradient.html)
- [https://mpatacchiola.github.io/blog/2018/12/28/dissecting-reinforcement-learning-8.html](https://mpatacchiola.github.io/blog/2018/12/28/dissecting-reinforcement-learning-8.html)
- [https://www.davidsilver.uk/wp-content/uploads/2020/03/pg.pdf](https://www.davidsilver.uk/wp-content/uploads/2020/03/pg.pdf)
- [https://lilianweng.github.io/posts/2018-04-08-policy-gradient/](https://lilianweng.github.io/posts/2018-04-08-policy-gradient/)
- [https://medium.com/nerd-for-tech/reinforcement-learning-introduction-to-policy-gradients-aa2ff134c1b](https://medium.com/nerd-for-tech/reinforcement-learning-introduction-to-policy-gradients-aa2ff134c1b)
- [https://medium.com/analytics-vidhya/policy-gradients-in-deep-reinforcement-learning-83d99575cfca](https://medium.com/analytics-vidhya/policy-gradients-in-deep-reinforcement-learning-83d99575cfca)

## Policy Gradient Variance

- [https://mcneela.github.io/machine_learning/2019/06/03/The-Problem-With-Policy-Gradient.html](https://mcneela.github.io/machine_learning/2019/06/03/The-Problem-With-Policy-Gradient.html)
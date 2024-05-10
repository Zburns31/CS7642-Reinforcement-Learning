# What is Q Learning?

- Q-learning is an off policy reinforcement learning algorithm that seeks to find the best action to take given the current state
    - It’s considered ***off-policy because the q-learning function learns from actions that are outside the current policy,*** like taking random actions, and therefore a policy isn’t needed
        - More specifically, q-learning seeks to learn a policy that maximizes the total reward
        - Another way to say this:
            - Q-learning learns Q-values that answer the question “What would this action be worth in this state, assuming that I stop using whatever policy I am using now, and start acting according to a policy that chooses the best action
    - On the other hand, an ***on-policy learner*** learns the value of the policy being carried out by the agent, including the exploration steps and it will find a policy that is optimal, taking into account the exploration inherent in the policy
- **Q learning is model-free**
- Q-learning is a ***values-based*** learning algorithm
    - Value based algorithms updates the value function based on an equation(particularly Bellman equation). Whereas the other type, ***policy-based*** estimates the value function with a greedy policy obtained from the last policy improvement
- **Success depends upon exploration**
    - **How?**
        - Choose random action with probability C

# How does it work?

- Q Learning avoids the need for a model by learning an ***action-utility*** function $Q(s,a)$ instead of a utility function U(s)
- The learned action-value function $Q$, directly approximates $q_*$, the optimal action-value function, independent of the policy being followed
    - ***The policy still has an effect in that it determines which state-action pairs are visited and updated***

# Elements of Q Learning

## ***Q Learning Update Rule***

Equations 6.8 (Sutton and Barto)

$$
Q(S_t, A_t) = Q(s,a) + \alpha * [R_{t+1} + \gamma \max_a Q(S_{t+1}, A) - Q(S_t, A)]
$$

This equation can be rewritten as:

$$
Q(state, action) = (1-\alpha)*Q(state, action) + \alpha(reward + \gamma \max_{\alpha} Q(next \space state, all \space actions)
$$

$$
Q(state, action) = (1-\alpha)*old \space Q \space value \space + \alpha * (Immediate \space Reward + All \space future \space rewards)
$$

$$
Q(state, action) = (1-\alpha)*Q[s,a] + \alpha * (R + \gamma* Q[s', \argmax_{a'} (Q[s',a'])])
$$

Note: $\argmax_{a'} (Q[s',a'])])$ represents the Q value for the best action to take in state $s'$

$$
\pi(s) = \argmax_a(Q[s,a])
$$

Where:

- $\alpha$  is the learning rate (0 < $\alpha$  ≤ 1) - Just like in supervised learning settings, $\alpha$  is the extent to which our Q-values are being updated in every iteration
    - Normally we use 0.2
- $\gamma$ is the discount factor (0 ≤ $\gamma$ ≤ 1) - determines how much importance we want to give to future rewards. A high value for the discount factor (close to **1**) captures the long-term effective award, whereas, a discount factor of **0** makes our agent consider only immediate reward, hence making it greedy

### What do these equations do?

- We are assigning, or updating, the Q-value of the agent's current *state* and *action* by first taking a weight (1-$\alpha$) of the old Q-value, then adding the learned value. The learned value is a combination of the reward for taking the current action in the current state, and the discounted maximum reward from the next state we will be in once we take the current action
- Basically, we are learning the proper action to take in the current state by looking at the reward for the current state/action combo, and the max rewards for the next state
- ***The Q-value of a state-action pair is the sum of the instant reward and the discounted future reward (of the resulting state). The way we store the Q-values for each state and action is through a Q-table***

## Q Table

- The Q-table is a matrix where we have a row for every state and a column for every action. It's first initialized to 0, and then values are updated after training. Note that the Q-table has the same dimensions as the reward table, but it has a completely different purpose

## Q Learning Algorithm Overview

1. Initialize the Q-table by all zeros
2. Start exploring actions: For each state, select any one among all possible actions for the current state (S)
3. Travel to the next state (S') as a result of that action (a)
4. For all possible actions from the state (S') select the one with the highest Q-value
5. Update Q-table values using the equation above
6. Set the next state as the current state
7. If goal state is reached, then end and repeat the process until convergence

## Q Learning Algorithm

![Untitled](./Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q/Untitled.png)

Note: equation differences

![Untitled](./Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q/Untitled%201.png)

- [https://datascience.stackexchange.com/questions/9832/what-is-the-q-function-and-what-is-the-v-function-in-reinforcement-learning](https://datascience.stackexchange.com/questions/9832/what-is-the-q-function-and-what-is-the-v-function-in-reinforcement-learning)

**For deriving the update rule formula:**

- [Q_Learner_Derivation_Summer2022.pdf](./Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q/Q_Learner_Derivation_Summer2022.pdf)

## Optimality

- **Convergence**: All that is required for correct convergence is that all pairs continue to be updated

# Double Q Learning

- The max operator in standard [Q-learning](https://paperswithcode.com/method/q-learning) and [DQN](https://paperswithcode.com/method/dqn) uses the same values both to select and to evaluate an action
    - This makes it more likely to select overestimated values, resulting in overoptimistic value estimates
    - To prevent this, we can decouple the selection from the evaluation, which is the idea behind Double Q-learning:

    $$
    Q_1(S_t, A_t) \leftarrow Q_1(S_t, A_t) + \alpha[R_{t+1} + \gamma Q_2(S_{t+1}, \argmax_a Q_1(S_{t+1}, a) - Q_1(S_t, A_t))]
    $$

    - Algorithm Notes:
        - $Q_1$ represents one action-value estimate and $Q_2$ represents another action-value estimate (network)

- [Double Q-Learning](./Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q/Double%20Q-Learning.md)

# Clipped Double Q-Learning

- In Clipped Double Q-learning, we follow the original formulation of Hasselt 2015. We have two independent estimates of the true Q value
    - Here, for computing the update targets, we take the minimum of the two next-state action values produced by our two Q networks; When the Q estimate from one is greater than the other, we reduce it to the minimum, avoiding overestimation
    - Fujimoto et al. presents another benefit of this setting: the minimum operator should provide higher value to states with lower variance estimation error
        - This means that the minimization will lead to a preference for states with low-variance value estimates, leading to safer policy updates with stable learning targets
- **Clipped Double Q-learning** is a variant on [Double Q-learning](https://paperswithcode.com/method/double-q-learning) that upper-bounds the less biased Q estimate $Q_{\theta_2}$ by the biased estimate $Q_{\theta_1}$
    - This is equivalent to taking the minimum of the two estimates, resulting in the following target update:

    $$
    y_1 = r + \gamma \min_{i=1,2} Q_{\theta'_i}(s', \pi_{\phi_1}(s'))
    $$

    - The motivation for this extension is that vanilla double [Q-learning](https://paperswithcode.com/method/q-learning) is sometimes ineffective if the target and current networks are too similar, e.g. with a slow-changing policy in an actor-critic framework
- [Paper](https://paperswithcode.com/method/clipped-double-q-learning)

# Dyna Q: Integrated Planning, Acting and Learning

- Within a planning agent, there are at least two roles for real experience:
    1. It can be used to improve the model (to make it more accurately match the real environment) and,
    2. It can be used to directly improve the value function and policy using the kinds of planning value/policy model experience model learning acting direct RL reinforcement learning methods we have discussed in previous chapters

- [Dyna Q](./Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q/Dyna%20Q.md)

# Deep Q Learning

- [Deep Q Learning](./Q%20Learning,%20Double%20Q%20Learning,%20Dyna%20Q/Deep%20Q%20Learning.md)

# Resources

- [https://rubikscode.net/2021/07/20/introduction-to-double-q-learning/](https://rubikscode.net/2021/07/20/introduction-to-double-q-learning/)

### Double Q Learning

- [https://towardsdatascience.com/double-deep-q-networks-905dd8325412](https://towardsdatascience.com/double-deep-q-networks-905dd8325412)
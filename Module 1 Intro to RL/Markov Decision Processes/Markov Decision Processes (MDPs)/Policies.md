# Policies

# What are Policies?

- To solve sequential decision problems, no fixed action sequence can solve the problem, because the agent might end up in a state other than the goal. **Therefore, a solution must specify what the agent should do for any state that the agent might reach. This is called a policy**
- The true utility of a state is just $U^{\pi ^ *} (s)$ - that is, the expected sum of discounted rewards if the agent executes an optimal policy

## Policy Characteristics & Terms

- **Stationarity:**
    - With a finite horizon, an optimal action in a given state may depend on how much time is left. ***A policy that depends on the time is called non-stationary***
    - With no fixed time limit, on the other hand, there is no reason to behave differently in the same state at different times. Hence, an optimal action depends only on the current state, and the optimal policy is stationary
    - **Note:** “infinite horizon” does not necessarily mean that all state sequences are infinite; it just means that there is no fixed deadline
- **Proper Policies**
    - A policy that is guaranteed to reach a terminal state is called a proper policy

# The Bellman Equations

The optimum policy is the policy that maximizes the long-term expected reward (utility), not just the immediate reward: 

**Optimal Policy:** $\pi^* = argmax_\pi \space E[\space \displaystyle\sum_{t=0}^{\infin} \gamma^t * R(S_t) \space | \space \pi \space ]$

Formula Notes:

- E —> Expectation due to things being potentially non-deterministic/ stochastic

$E[\space \displaystyle\sum_{t=0}^{\infin} \gamma^t * R(S_t)]$ 

- This represents the expected sequence of states where we follow the policy $\pi$
    - Multiplied by the discount factor $\gamma$
- **The optimal policy ($\pi^*$) is the one that maximizes the expected reward (from all future states) given that we follow a particular policy**

## Action-Utility Function (Q Function)

- The Q function is the expected utility of taking a given action in a given state. The Q function is related to utilities in an obvious way:

$$
U(s) = \max_a Q(s,a)
$$

- This can be rewritten using the Bellman equation for Q-functions:

$$
Q(s,a) = \sum_{s'}P(s'|s,a)[R(s,a,s') + \gamma \max_a Q(s',a')]
$$

### **But, how do we solve this....?**

We have defined utility - U(s) - in a way to help us solve this.... Since utility represents the long-term expected reward of being in a state or taking an action in a state, this will help us

## **Utility of a  Particular State: $U^\pi(s) = \space E[\space \displaystyle\sum_{t=0}^{\infin} \gamma^t * R(S_t) \space | \space \pi, \space s_0 = s ]$**

- Summary: The formula shows that the utility of a state is simply the expected set of states that we will see from that point on, given that we follow a policy
    - In other words: the utility/value at a particular state is the what happens if we start running from that state given that we follow that policy $\pi$
    - Also known as the **value function**
    - Allows us to evaluate how good it is to be in some particular state
- **How do we define the Utility of a state?**
    - The utility of a particular state is going to depend upon the policy that we are following
        - **Simply, it is the expected set of states that were going to see from that point on, given that we follow a policy**
    - **How good is it it to be in some state?**
        - It's exactly as good to be in that state as what we will expect to see from that point on, given that were following a particular policy where we started in that state
- Formula notes:
    - The formula is conditioned on the fact that we follow a particular policy starting from some state *s*

By defining utility $U(s)$, we can now define $\pi^*$  in a better way:

$\pi^* (s) = argmax_a \sum_{s'} T(s, a, s') \space U^{\pi^*}(s)$

- The optimal action to take at a state is to look over all the actions, sum up the transition probabilities (the probability we end up state $s'$) and multiply by the utility of state $s'$ (**which is not defined)**
- **This equation states that the optimal policy is the one that for every (or any particular) state, returns the action that maximizes my expected utility**
- U(s) == Utility of a state IF we follow the optimal policy
- **Note that: $U(s) == U^{\pi^*}(s)$**

# The Bellman Equation

$U(s) = R(s) + \gamma * argmax_a \sum_{s'} T(s, a, s') \space U(s')$

- The true utility/value of a state is equal to the reward of being in that state + the discounted reward that we will receive from that point on
- **This is the fundamental recursive equation of the true value of being in some particular state**
- We take a maximum over all possible actions because we want to find the value for the optimal policy
- **Formula explanation:**
    - $R(s)$ —> the reward for entering some state s
    - $argmax_a \sum_{s'} T(s, a, s') \space U(s')$ —> Return the action that maximizes the expected utility
        - We use a summation and T(s, a, s’) because there may be several different states we could end up in
    - $\gamma$  —> Discount all future rewards
    - $T(s,a, s')$ —> The probability that we end up state $s'$
    - $U(s')$ —> The true utility of being in state $s'$
    

### What is the problem with the $U(s)$  Bellman equation written above?

- The **max** operation, makes it non-linear
- We have n equations with n unknowns
    - If the dynamics p (probabilities) of the environment are known, then in principle one can solve this system of equations for $v_*$ using any one of a variety of methods for solving systems of nonlinear equations

# Finding Policies: Value-Iteration Vs. Policy Iteration

- In **policy iteration** algorithms, you start with a random policy, then find the value function of that policy (policy evaluation step), then find a new (improved) policy based on the previous value function, and so on
    - In this process, each policy is guaranteed to be a strict improvement over the previous one (unless it is already optimal). Given a policy, its value function can be obtained using the *Bellman operator*

- In **value iteration**, you start with a random value function and then find a new (improved) value function in an iterative process, until reaching the optimal value function
    - Notice that you can derive easily the optimal policy from the optimal value function. This process is based on the *optimality Bellman operator*
- [Reference](https://stackoverflow.com/questions/37370015/what-is-the-difference-between-value-iteration-and-policy-iteration)

## Finding Policies: Value Iteration

[Value Iteration](Value%20Iteration%20ccae45b2684545b2a238eba7c42532c0.md)

## **Finding Policies: Policy Iteration**

[Policy Iteration](Policy%20Iteration%2055a39c3b13e146b79871f389d98f5c74.md)

## Finding Policies: Linear Programming

## Value-Iteration Vs. Policy Iteration

---

- **Policy iteration and value iteration are both dynamic programming algorithms that find an optimal policy $\pi$ in a reinforcement learning environment.** They both employ variations of Bellman updates and exploit one-step look-ahead:
    
    
    | Policy Iteration | Value Iteration |
    | --- | --- |
    | Starts with a random policy | Starts with a random value function |
    | Algorithm is more complex | Algorithm is simpler |
    | Guaranteed to converge | Guaranteed to converge |
    | Cheaper to compute | More expensive to compute |
    | Requires fewer iterations to converge | Requires more iterations to converge |
- **In policy iteration, we start with a fixed policy. Conversely, in value iteration, we begin by selecting the value function.** Then, in both algorithms, we iteratively improve until we reach convergence
- The policy iteration algorithm updates the policy. The value iteration algorithm iterates over the value function instead
    - Still, both algorithms implicitly update the policy and state value function in each iteration
- In each iteration, the policy iteration function goes through two phases. One phase evaluates the policy, and the other one improves it
    - The value iteration function covers these two phases by taking a maximum over the utility function for all possible actions
- The value iteration algorithm is straightforward. It combines two phases of the policy iteration into a single update operation
    - However, the value iteration function runs through all possible actions at once to find the maximum action value
        - Subsequently, the value iteration algorithm is computationally heavier
- Both value-iteration and policy-iteration algorithms can be used for offline planning where the agent is assumed to have prior knowledge about the effects of its actions on the environment (they assume the MDP model is known)

# Bellman Equations

---

### What does the Bellman equation represent?

It is supposed to show the sequence of rewards received by an agent operating in environment

![Policies%209b4ff1610f094eba9e470925b61bd576/Untitled.png](Policies%209b4ff1610f094eba9e470925b61bd576/Untitled.png)

There a few different ways we can express the Bellman equation:

Bellman Equation Form 1 - $V(s) = max_a (R(s, a) + \gamma \sum_{s'} T(s, a, s') V(s'))$

- Value of a state is the max over all the actions, the reward for you taking that action in this state + the discounted value of the state you end up in weighted by the probability that you end up there
- Notation difference here:
    - R(s,a) —> Reward is a function of taking an action in a state. Mathematically equivalent as other form
    

We can rewrite Equation 1 as:

$V(s_1) = max_a (R(s_1, a_1) + \gamma \sum_{s_2} T(s_1, a_1, s_2) \space max_{a_2} (R(s_2, a_2) + \gamma \sum_{s_3} T(s_2, a_2, s_3) .... ]$

- Where ] indicates the balancing of all parentheses
- **Because this is an infinite sequence, there are other ways we can group this....leading to equation 3**

Bellman Equation Form 2 - $Q(s, a) = R(s, a) + \gamma * \sum_{s'} T(s, a, s') \space max_a * Q(s', a')$

- Since in equation 2 the $R(s_i, a_i) + \gamma \sum_{s_i} T(s_i, a_i, s_3)$ is a repeating substructure, we can replace it with the $Q(s', a')$
- **The value of an action is meaningless, except in the presence of the state where you are taking an action**
    - So the idea with $Q(s,a)$ is that we start at some state S, we take action a and then we proceed
- **Q-Values:** The value of each of the actions you could take in the state, the best of which we will take

- **Why use equation 2? What does it add?**
    - More useful in the RL world because we can take the expectations of $Q(s,a)$ using just experienced data
        - You don't need direct access to the reward function or transition function
        - **We won't know the transitions and rewards in advance**
        - Where as in Bellman equation form 1, we need direct access to link together the values of states
    - Mathematically, it doesn't really matter where you start from
        - This rewritten equation allows us to start from anywhere in the sequence

Bellman Equation Form 3 -  $C(s, a) = \gamma \sum_{s'} T(s,a,s') \space max_{a_1} (R(s', a ') + C(s', a'))$

- This form of the equation is as follows:
    1. We have our state, took an action and got our reward
    2. Get the discount towards the future (I.e. next state)
        - The summation over S' is our expectation of where we expect to end up next
    3. Take the best action
    4. Receive our reward for taking that action which puts us in the same place as before
    5. $C(s', a')$  is the continuation of the sequence
    
- The C function is basically the same as the Q function but only including the delayed/future reward, not the current reward
    - The $C$ stands for continuation
    - Useful for when the reward structure is complicated

### Differences between the 3 Bellman equations:

- By representation:
    - Equation 1 represents basically what happens when you're in a particular state
    - Equation 2 represents basically what happens when you're at an action
- By grouping:
    - Equation 1 groups by the max
    - Equation 2 groups by R(s)
- Easier intuition to start from different parts of the sequence
- **Summaries:**
    - V(s) is the expected return if you were at state s, and continued from there
    - Q(s,a) is the expected return if you were at state s, then took action a, then continued from there

# Terms

- **Policy Loss:** Is the most the an agent can lose by executing $\pi_i$ instead of the optimal policy $\pi^*_i$

### Resources

- [https://medium.com/@m.alzantot/deep-reinforcement-learning-demysitifed-episode-2-policy-iteration-value-iteration-and-q-978f9e89ddaa](https://medium.com/@m.alzantot/deep-reinforcement-learning-demysitifed-episode-2-policy-iteration-value-iteration-and-q-978f9e89ddaa)
- [https://towardsdatascience.com/the-bellman-equation-59258a0d3fa7](https://towardsdatascience.com/the-bellman-equation-59258a0d3fa7)
    
    [https://inst.eecs.berkeley.edu/~cs61a/su16/assets/slides/29-Artificial_Intelligence_1pp.pdf](https://inst.eecs.berkeley.edu/~cs61a/su16/assets/slides/29-Artificial_Intelligence_1pp.pdf)
    

**Policy & Value Iteration**

- [https://www.baeldung.com/cs/ml-value-iteration-vs-policy-iteration](https://www.baeldung.com/cs/ml-value-iteration-vs-policy-iteration)
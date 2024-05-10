# What is Policy Iteration?

- Instead of repeatedly improving the value-function estimate, **we re-define the policy at each step and compute the value according to this new policy until the policy converges**
    - Policy iteration is also guaranteed to converge to the optimal policy and it often takes less iterations to converge than the value-iteration algorithm (not necessarily less wall clock time however)
    - **Remember:** *A policy is a mapping of an action to every possible state in the system*
- **In policy iteration, we start by choosing an arbitrary policy $\pi$. Then, we iteratively evaluate and improve the policy until convergence**

# How does it work?

- **Builds on the idea that we are about finding policies, not values. Thus, the algorithm alternates between the following two steps, beginning from some initial policy $\pi_0$:**
    1. Initialization Random Policy: Start with $\pi_0 \leftarrow$  arbitrary value or guess
    2. Policy Evaluation: given $\pi_t$ calculate $U_i = U^{\pi_i} \rightarrow$ the utility of each state if $\pi_i$ were to be executed
        - Get an action for every state in the policy and evaluate the value function using **Bellmans equations**
    3. Policy Improvement: Calculate $\pi_{i+1} = argmax_a \sum T(s, a, s')\space U_t(s')$ using one-step look-ahead based on $U_i$
        - **Similar to value iteration:** update our policy to be the policy that takes the actions that maximizes the expected utility based of step 2
            - I.e. get the best action from the value function
        - **Key:** Need to figure out how to calculate $U_t(s)$
- **Termination Condition:**
    - Stop when the policy no longer changes
        - If the best action is better than the present policy action, then replace the current action by the best action

    $U_t(s) = R(s) + \gamma * \sum_{s'} T(s, \pi_t, s') \space U_t(s')$


# Algorithm Overview

- As with value iteration, we use the ***value function*** to compute the optimal action given a state
- In the beginning, we don’t care about the initial policy $\pi_0$ being optimal or not
    - During the execution, we concentrate on improving it on every iteration by repeating policy evaluation and policy improvement steps
        - Using this algorithm we produce a chain of policies, where each policy is an improvement over the previous one

         $\pi_0 \xrightarrow{Evaluation} U_{\pi_0} \xrightarrow{Improvement}    \pi_1 \xrightarrow{Evaluation} U_{\pi_1} \xrightarrow{Improvement} \pi_2 \xrightarrow{Evaluation} U_{\pi^*} \xrightarrow{Improvement} U_{\pi^*}$


### Policy Iteration Algorithm Steps

1. **Policy Evaluation:**
    - We evaluate a policy $\pi(s)$ by calculating the state value function $V(s)$

    $$
    U_{i+1}(s) = \sum_{s'} P(s'| s, \pi_i(s))[R(s, \pi_i(s), s') + \gamma U_i(s')]
    $$

    - This Policy Evaluation function can also be re-written as:

    $$
    U_{i+1}(s) = R(s) + \gamma * \sum_{s'} T(s, \pi_i(s), s') \space U_i(s')
    $$

    - The utility at time t by the following the policy at time t is equal to the above equation
    - **Note:**
        - **In the transition function, a is replaced with $\pi_i$**
        - No max function since we already know what action we will take. This is defined by the policy $\pi$ —> effectively a constant
            - The max made the function non-linear. Now, the equation above is a **set of linear equations because we are taking advantage of choosing one policy**

2. **Policy Improvement**
    - Then, we calculate the improved policy by using one-step look-ahead to replace the initial policy $\pi(s)$:

    $$
    \pi_{i+1}(s) = \argmax_a \sum_{s'} P(s'| s,a) [R(s,a, s') + \gamma U_i(s')]
    $$

    - This computes the action that is **expected** to give the highest return, given the current and fixed value function

## Algorithm Pseudocode

AI - A Modern Approach (Russel and Norvig)

![Untitled](./Policy%20Iteration/Untitled.png)

Intro to RL - (Sutton and Barto)

![Untitled](./Policy%20Iteration/Untitled%201.png)

- Note this implementation is **Iterative Policy Evaluation (In Place Operation)**

## Implementation Details

- Within the Policy Evaluation step, rather than using the old values $v_k$ to compute the new values $v_{k+1}$, we can actually use the new values which were updated in place
    - This would modify the value function $U_\pi(s)$ on the right hand side of the equation
    - It is show that generally speaking, this modification results in faster convergence
- Faster convergence can often be achieved by incorporating multiple policy evaluation steps between each policy improvement step

## Asynchronous Policy Iteration

- On each iteration, we can pick any subset of states and apply either kind of updating (policy improvement or simplified value iteration) to that subset
    - This very general algorithm is called ***asynchronous policy iteration***
- Given certain conditions on the initial policy and initial utility function, asynchronous policy iteration is guaranteed to converge to an optimal policy

## Generalized Policy Iteration

- Generalized policy iteration (GPI) refers to the general idea of letting policy-evaluation and policy-improvement processes interact, independent of the granularity and other details of the two processes

# Resources:

**Policy Iteration**

- [https://towardsdatascience.com/policy-iteration-in-rl-an-illustration-6d58bdcb87a7](https://towardsdatascience.com/policy-iteration-in-rl-an-illustration-6d58bdcb87a7)
- [https://ai.stackexchange.com/questions/12270/understanding-the-update-rule-for-the-policy-in-the-policy-iteration-algorithm](https://ai.stackexchange.com/questions/12270/understanding-the-update-rule-for-the-policy-in-the-policy-iteration-algorithm)
- [https://www.baeldung.com/cs/ml-value-iteration-vs-policy-iteration](https://www.baeldung.com/cs/ml-value-iteration-vs-policy-iteration)
- [https://mpatacchiola.github.io/blog/2016/12/09/dissecting-reinforcement-learning.html](https://mpatacchiola.github.io/blog/2016/12/09/dissecting-reinforcement-learning.html)
- [https://towardsdatascience.com/policy-iteration-in-rl-an-illustration-6d58bdcb87a7](https://towardsdatascience.com/policy-iteration-in-rl-an-illustration-6d58bdcb87a7)

**Working Examples**

- [https://towardsdatascience.com/implement-policy-iteration-in-python-a-minimal-working-example-6bf6cc156ca9](https://towardsdatascience.com/implement-policy-iteration-in-python-a-minimal-working-example-6bf6cc156ca9)
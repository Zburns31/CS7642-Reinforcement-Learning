# Research Paper Summary: R-Max & Potential-Based Reward Shaping in Model-Based RL

- The [paper](https://cdn.aaai.org/AAAI/2008/AAAI08-096.pdf) makes three main contributions
    - First, it demonstrates a concrete way of using shaping functions with model-based learning algorithms and relates it to model-free shaping and the $R_{max}$ algorithm
    - Second, it argues that admissible shaping functions are of particular interest because they do not interfere with $R_{max}$’s ability to identify near-optimal policies quickly
    - Third, it presents computational experiments that show how model-based learning, shaping, and their combination can speed up learning

# What is it?

- In training animals, shaping is the idea of directly rewarding behaviours that are needed for the main task being learned
    - Similarly, in reinforcement learning parlance, shaping means introducing “hints” about optimal behaviour into the reward function so as to accelerate the learning process

# R Max Algorithm

- The $R_{max}$ algorithm is a prototypical model-based algorithm which creates “optimistic” estimates $\hat T$ and $\hat R$ for the transition $T$ and reward $R$ functions
- The algorithm computes the following update rule for all state-action pairs:

$$
\hat Q(s,a) = \hat R(s,a) + \gamma \sum_{s'} \hat T(s,a,s') \max_a \hat Q(s',a')
$$

- The principle advantage of $R_{max}$ then, is that it makes more efficient use of the experience it gathers at the cost of increased computational complexity

## General R Max Algorithm Overview

- Steps in the algorithm:
    - Keep track of MDP $\rightarrow$ Transition Function
    - Any unknown state-action pair is $R_{max}$ $\rightarrow$  C: Unknown *s,a* if tried fewer than C times. Then use ML estimate
    - Solve the MDP
    - Take action from $\pi^*$

## Returns: Original, Shaping, $R_{max}$ and Shaped $R_{max}$

- **Original Return:** $U(s) = \sum_{i=0}^\infin \gamma^ir_i$
- **Shaped Return: $R(s,a) = R(s,a) + \gamma \phi(s') - \phi(s) = U(s') - \phi(s_0)$**
    - That is the shaped return is the unshaped return minus the shaping function’s value for the starting state
- $**R_{max}$ Return: $U_{R_{max}}(s') = \sum_{i=0}^{T-1} \gamma^i r_i + \gamma^Tv_{max}$**
    - Where $s^T, a^T$ is the first unknown state-action pair in the sequence $s'$
    - Thus the $R_{max}$ return is the truncated return plus a discounted factor of $v_{max}$ that depends only on the time step on which an unknown state is reached
        - The $R_{max}$ return for a trajectory $s'$ that does not reach an unknown state is the same as the original return $U_{R_{max}} (s') = U(s')$
- **Shaped $R_{max}$ Return:**
    - This definition of return shapes all the rewards from transitions between known states:

        $$
        U_\phi ^{R_{max}} (\bar s) = U_{R_{max}}(\bar s) - \gamma^T v_{max} - \phi(s_0) + \gamma^T \phi(s_T)
        $$

        - If $\phi(s) = v_{max}$, we have $U_\phi ^{R_{max}}(\bar s) = U_{R_{max}} (\bar s) - v_{max}$
            - This result is the same as the $R_{max}$ return shifted by the constant $v_{max}$
        - It is interesting to note that this choice of shaping function results in precisely the original $R_{max}$ algorithm
    - In its general form, the return of this rule is like that of $R_{max}$, except shifted down by the shaped value of the starting state (which has no impact on the optimal policy) and then shifted up by a discounted factor of the first state reached from which an unknown action is taken
        - This application of the shaping idea encourages the agent to explore unknown states with higher values of $\phi(s)$
        - In addition, if $\gamma < 1$, nearby states are more likely to be chosen first
        - As a result, it captures the same intuition for shaping that drives its use in the Q-learning setting when faced with an un- familiar situation, explore states with higher values of the shaping function

## Admissible Shaping Functions & Properties

- **Admissibility:**
    - In the context of $R_{max}$, a shaping function is admissible for an MPD if:

    $$
    \phi(s) \ge V(s), \text{where } V(s) = \max_a Q(s,a)
    $$

    - That is, the shaping function is an upper bound on the actual value of the state
- **Properties**
    1. If $\phi$ is admissible, then $R_{max}$ is PAC-MDP
    2. If $\phi$ is not admissible then $R_{max}$ with shaping need to be PAC-MDP

## Implementation Details

- $R_{max}$ hypothesizes:
    - State $s_{max}$ such that $\hat T(s_{\max}, a, s_{\max})$ = 1
    - $R(s_{\max}, a) = 0$ for all $a$
- Initialization Conditions:
    - $\hat T(s, a, s_{\max}) = 1$
    - $R(s, a) = v_{max}$ for all $s$ and $a$
    - $c(s,a,s')$ = 0
    - $t(s,a)$ = 0
    - Note:
        - All unmentioned transitions have probability 0
- Components
    - $c(s,a,s')$ which represents the transition count
    - $t(s,a)$ which represents the reward total
    - $m \ge 0$ which is an integer parameter
- Each experience tuple results in the following updates:
    - $c(s,a,s') \leftarrow c(s,a,s') + 1$
    - $t(s,a) \leftarrow t(s,a) + r$
    - Then if $\sum_{s'} c(s,a,s') = m$, the algorithm modifies $\hat T(s, a, s') = c(s,a,s') / m$
    - $\hat R(s, a) = t(s,a) / m$
- Notes:
    - We refer to any state-action pair $s,a$ such that $\sum_{s'} c(s,a,s') < m$ as ***unknown and otherwise known***
        - When an unknown state is reached, the agent assumes its value is $v_{max}$, that is, an upper bound on the largest possible value
    - Notice that, each time a state–action pair becomes known, $R_{max}$ performs a great deal of computation, solving an approximate MDP model

## Optimality & Convergence

- $R_{max}$ guarantees with high probability, that it will take near optimal actions on all but a polynomial number of steps if $m$ is set sufficiently high
- $R_{max}$ achieves a guarantee of PAC-MDP (probably approximately correct) for MDP’s
    - Specifically, with a probability of $1-\delta$, $R_{max}$ will execute a policy that is within $\epsilon$ of optimal on all but a polynomial number of steps


# Resources

- [http://procaccia.info/courses/15381f16/slides/781_rl_rmax.pdf](http://procaccia.info/courses/15381f16/slides/781_rl_rmax.pdf)
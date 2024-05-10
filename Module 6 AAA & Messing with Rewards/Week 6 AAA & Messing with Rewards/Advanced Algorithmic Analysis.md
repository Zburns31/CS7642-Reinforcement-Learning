## Value Iteration Convergence

- For some $t^{* < \infin}$ polynomial in $|S|, |A|, R_{max} = \max _{a,s} |R(s,a)|, \frac {1} {1-\gamma}$ bits of precision to specify the transition probabilities then (1):

    $$
    \pi(s) = \argmax_a Q_{t^*}(s,a) \space \text {is optimal}
    $$

    - For the finite case, there is some $t^*$, less than infinity, that’s polynomial in the number of states, actions, the magnitude of the rewards, number of bits of precision used to specify the transition probabilities
        - There is some greedy policy that is close enough to optimal in a finite amount of time
            - Note: The greedy policy converges, not the values
    - **Convergence Property:** Value Iteration converges in the limit to the right value function but after any finite number of steps, it need not have the optimal value function
        - But it will after some finite number of steps have the optimal policy
    - Where:
        - $Q_{t^*}$ is the $Q$ function after running $t^*$ iterations
    - Notes:
        - We know that value iteration converges in the limit
            - However (1) states that there is some time before infinity where we get a $Q$ function that is close enough
        - As $\gamma$ approaches 1, then the term $\frac {1} {1-\gamma}$ blows up
- If (1) $|V_t(s) - V_{t+1}(s)| < \epsilon \space \forall s$ then (2) $\max_s |V^{\pi_V}(s) - V^*(s)| < \dfrac {2\epsilon \gamma} {1-\gamma}$
    - **What this means:** If the value function has stopped changing, then the difference between this value function and the optimal function is small
    - Where:
        - $\dfrac {1} {1-\gamma}$ isn’t really polynomial (more like exponential)
    - Notes:
        - If the difference between the value function for some time step $t$ to the value function at time step $t+1$ is less than $\epsilon$ then the largest difference between the optimal value function and the value function we get by taking the value function with respect to the greedy policy is small
            - If we can get a good enough approximation of the value function then we can put a bound on how far off we are from the optimal policy
            - This provides us a way to tell when a decent time to stop Value Iteration would be
        - Both (1) and (2) encourage small values of $\gamma$
            - $\gamma$ really tells us what our horizon is
                - I.e. The smaller gamma is, the less we care about the future. If this is the case, it’s easier to optimize over a small horizon
                - The effective horizon is $\dfrac {1} {1-\gamma}$
                - **Basic Idea:**
                    - Smaller values of ****$\gamma$ mean more short sighted behaviour but easier problems to solve
                    - Larger values of $\gamma$ mean more look ahead into the future but more computation is required

- $||B^k Q_1 - B^kQ_2|| \le \gamma^k|| Q_1 - Q_2|| \infin$
    - The idea here is that if we start off with sum $Q$ function, $Q_1$ and we run $k$ steps of value iteration (I.e. applying the Bellman operator $k$ times), that the $k$ step Bellman operator is a contraction mapping
        - I.e. Running k steps of value iteration brings the value functions much closer together in comparison to only running value iteration once
    - This gives us a way of quantifying how long we run value iteration and how close we are to optimal after running it that far

## Linear Programming

- ***Linear Programming (LP) is the only algorithm to solve MDP’s in polynomial time***
    - This is because the term $\dfrac {1} {1-\gamma}$ isn’t really polynomial in the number of bits it takes to write down $\gamma$
- To solve a MDP via LP, the basic idea is:
    - Assume you have a complete model of the MDP (transitions, rewards, etc.)
    - For any given state, we have the assumption that the state's true value is reflected by:

    $$
    V^*(s) = r + \gamma \max_{a\in A} \sum_{s' \in S} T(s,a,s') V(s')
    $$

    - That is, the true value of the state is the reward we accrue for being in it, plus the expected future rewards of acting optimally from now until infinitely far into the future, discounted by the factor *𝛾*, which captures the idea that reward in the future is less good than reward now
    - In Linear Programming, we find the minimum or maximum value of some function, subject to a set of constraints
        - We can do this efficiently if the function can take on continuous values, but the problem becomes NP-Hard if the values are discrete
        - You would usually do this using something like the [Branch & Bound algorithm](http://web.tecnico.ulisboa.pt/mcasquilho/compute/_linpro/TaylorB_module_c.pdf)
        - These are widely available in fast implementations. GLPK is a decent free library. IBM's CPLEX is faster, but expensive
    - We can represent the problem of finding the value of a given state as:

        $$
        \text {minimize} \space V(s)
        $$

        Subject to the constraints:


    $$
    V^*(s) \ge r + \gamma \sum_{s' \in S} T(s,a,s') V(s'), \forall a \in A, s \in S
    $$

    - [Resource](https://ai.stackexchange.com/questions/11246/how-can-we-use-linear-programming-to-solve-an-mdp)
        - Slides: [https://www.cs.cmu.edu/afs/cs/academic/class/15780-s16/www/slides/mdps.pdf](https://www.cs.cmu.edu/afs/cs/academic/class/15780-s16/www/slides/mdps.pdf)

## Policy Iteration

- Steps:
    - Initialize: $\forall s, Q_0(s) = 0$
    - Policy Improvement: $\forall s \space \pi(s) = \argmax_a Q_t(s,a) \space \text {where} \space t \ge 0$
    - Policy Improvement: $Q_{t+1} = Q^{\pi_t}$
- Notes:
    - $Q_t \rightarrow Q^*$
    - Convergence is exact & complete in finite time
    - Converges at least as fast as value iteration

### Properties of Policy Iteration

- Monotonicity
- [Value Improvement](https://edstem.org/us/courses/33453/lessons/52863/slides/302079)
    - Policy iteration will not get stuck in a local optimum. It will find the optimal policy

# Domination

- **Domination:** Policy 1 dominates policy 2 if, for all states the value of policy 1 at a given state is greater than or equal to the value at that state for policy 2: $\pi_1 \ge \pi_2 \space \text {if} \space \forall_{s \in S} \space V^{\pi_1}(s) \ge V^{\pi_2}(s)$
    - It could be there are some policies where $\pi_1$ does better in some states and $\pi_2$ does better in others. But in order for $\pi_1$ to dominate $\pi_2$, it must do no worse in ***all states***
    - If policy 1 dominates policy 2 but not by strict domination, it means they have the same value everywhere
- **Strict Domination** happens when ****$\pi_1$ dominates $\pi_2$ and there is some state $s$ that it is strictly better
- **Epsilon Optimal:** A policy is $\epsilon$-optimal if its the case that the value function for that policy is epsilon close to the value function for the optimal policy for all states: $|V^\pi(s) - V^{\pi^*}(s)| \le \epsilon \space \forall s \in S$
    - This gives us a concept of **Bounding Loss or Bounding Regret**
        - So we can say that a policy is nearly optimal if the amount that it loses per time step is very small

# Resources

- [https://amiithinks.github.io/tea-time-talks/2019/2019-talk-pdfs/Paniz_Behboudian.pdf](https://amiithinks.github.io/tea-time-talks/2019/2019-talk-pdfs/Paniz_Behboudian.pdf)
- [https://www.quora.com/What-is-the-proof-that-policy-iteration-converges-in-reinforcement-learning](https://www.quora.com/What-is-the-proof-that-policy-iteration-converges-in-reinforcement-learning)
    - [https://gwern.net/doc/statistics/decision/1960-howard-dynamicprogrammingmarkovprocesses.pdf](https://gwern.net/doc/statistics/decision/1960-howard-dynamicprogrammingmarkovprocesses.pdf)
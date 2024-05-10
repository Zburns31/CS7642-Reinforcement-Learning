# Readings:

- Littman and Szepesvari (1996)
- Littman (1996) Chapter 3

# Bellman Equations: Control Vs. Prediction Problems

- ***The difference between using RL with and without control is whether there are actions that are being chosen by the learner***

    ### Bellman equations without control

    $$
    V(s) = R(s) + \gamma \sum_{s'} T(s,s')V(s')
    $$

    - Translating this to a TD update rule (1):

    $$
    V_t(s_{t-1}) = V_{t-1}(S_{t-1}) + \alpha (R_t + \gamma V_{t-1}(S_t) - V_t(S_{t-1})

    \newline

    V_t(s) = V(s_{t-1}) \space \text{otherwise}
    $$

    - The TD updates only happen when we leave a state and then update for that state we just left

    ### Bellman equations with control

    - When we need to evaluate action-values, then we have the following update rule:

        $$
        Q(s,a) = R(s,a) + \gamma \sum_{s'}T(s,a,s') \max_a Q(s',a')
        $$


    - Translating this into a TD update rule (2):

        $$
        Q_t(s_{t-1},a_{t-1}) = Q_{t-1}(S_{t-1}, a_{t-1}) + \alpha_t [R_t + \gamma \space \max_{a'} Q_{t-1}(s_t, a) - Q_{t-1}(s_{t-1}, a_{t-1})] \newline

        Q_t(s,a) = Q_{t-1}(s,a) \space \text {otherwise}
        $$

    - Using this update rule, the action-value estimates are the same everywhere, except for the state and action that we just left
    - Equation 2 is actually completing two different types of approximations all at one time:
        1. If we knew the model, synchronously update using the $Q$ function:

            $$
            Q_t(s,a) = R(s,a) + \gamma \sum_{s'}T(s,a,s') \max_a Q_{t-1}(s',a')
            $$

        2. If we knew $Q*$ then we can use a sampling synchronous update

# Bellman Operator

- To simplify notation, let $B$ be an operator, or a mapping from value function to value function
- Let the Bellman Operator be:

$$
[BQ](S,a) = R(s,a) + \gamma \sum_{s'}T(s,a,s') \max_a Q(s',a')
$$

- Here B is applied to Q and we get out a new value function

## Contraction Mappings

- In simple terms, a contraction mapping is a mathematical function that "contracts" the distance between points in a space
- An operator $B$ ****is a contraction mapping if, for all $F,G$ and some $0 \le \gamma < 1$:

    $$
    ||BF - BG|| \infin \le \gamma || F-G|| \infin
    $$

    - Where:
        - $F, G$ represent two different value functions
    - The property above states that if the distance between $B$ applied to $F$ and $B$ applied to $G$ is smaller than or equal to gamma times the original distance between $F$ and $G$, then $B$ is a contraction mapping
        - **Basic Idea:** It’s causing things to get closer to together
            - In other words, a contraction mapping takes points in a space and "pulls" them closer together. It's like squishing or shrinking the space, making the points come closer to each other
- **Infinity (Max) Norm: $||Q||{\infin} = \max_{s,a} |Q(s,a)|$**
    - This is saying what is the maximum value the $Q$ function will or currently has

### How it works

- Remember that a function is a contraction mapping if and only if the distance between any pair of points becomes *smaller* after they are both sent through the mapping
- One way to check if a function B is a contraction mapping is to let x and y be arbitrary, non-equal real numbers, and then compute B(x) and B(y). If you can prove that |B(x)-B(y)| is always smaller than |x-y|, regardless of what choice someone could make for x and y, then that means B is a contraction mapping
- On the other hand, if there is *any* example of x and y where x != y and |B(x)-B(y)| >= |x-y|, then B is not a contraction mapping

### Properties

- If $B$ is a contraction mapping:
    1. $F^* = B F^*$  has a solution and it’s unique
        1. **Fixed Point Solution:** There’s some $F^*$ that we can plug into B that causes it not to change
    2. $F_t = BF_{t-1} \rightarrow F_t \rightarrow F^*$
        1. Value iteration converges

## Bellman Operator Contracts

- $[BQ](S,a) = R(s,a) + \gamma \sum_{s'}T(s,a,s') \max_a Q(s',a')$
- Given $Q_1, Q_2$:

    $$
    ||BQ_1 - BQ_2||_\infin = \max_{a,s}|[BQ_1](S,a) - [BQ_2](S,a)| \leq \gamma \max_{s',a'} | Q_1(s',a') - Q_2(s',a') | = \gamma ||Q_1 - Q_2||_\infin
    $$

    - The max difference between all state action pairs of the absolute difference between the discounted expected value of the next state where the expected value of the next state is the max over all actions for state s prime

## Non-Expansion Operators

- **[Non-Expansion Operators:](https://ai.stackexchange.com/questions/11244/why-is-the-max-a-non-expansive-operator)** A non-expansive operator is a function that brings points closer together or at least no further apart
    - In simpler terms, if you apply a non-expansion operator to points in a metric space, the distances between the points will either stay the same or become smaller
        - The operator does not "stretch" or "expand" the distances between points
    - Example Operators: Max, Min, (Order Statistics), Fixed Convex Combinations
    - If a Boltzmann update, it is not guaranteed to converge
- Max is a non-expansion operator:

$$
 \text{For all} \space  f,g \space |\max_a f(a) - \max_a g(a) | \leq \max_a |f(a) - g(a)|
$$

# Generalized RL: Convergence & Applications

[Generalized RL: Convergence & Applications](./Weeks%204%20&%205%20Convergence/Generalized%20RL%20Convergence%20&%20Applications.md)

# Key Terms & Concepts

- **[Non-Expansion Operators:](https://ai.stackexchange.com/questions/11244/why-is-the-max-a-non-expansive-operator)** A non-expansive operator is a function that brings points closer together or at least no further apart
- **Fixed Point:** A unique fixed point, also known as a fixed point of unique attraction, is a point in mathematics that remains unchanged by a given function or transformation
    - In other words, it is a point that is mapped to itself by the function
    - More formally, let's say you have a function f that maps points from a set to itself. A fixed point of f is a point x in the set such that f(x) = x, i.e., the image of x under the function is x itself. In other words, x is unchanged by the function

# Resources

- [https://ozaner.github.io/contraction-maps/](https://ozaner.github.io/contraction-maps/)
- [https://www.youtube.com/watch?v=ZMZxjVE_4WY](https://www.youtube.com/watch?v=ZMZxjVE_4WY)
- [https://towardsdatascience.com/mathematical-analysis-of-reinforcement-learning-bellman-equation-ac9f0954e19f](https://towardsdatascience.com/mathematical-analysis-of-reinforcement-learning-bellman-equation-ac9f0954e19f)
- [https://runzhe-yang.science/2017-10-04-contraction/](https://runzhe-yang.science/2017-10-04-contraction/)
- [https://web.stanford.edu/class/cme241/lecture_slides/BellmanOperators.pdf](https://web.stanford.edu/class/cme241/lecture_slides/BellmanOperators.pdf)
- [https://ai.stackexchange.com/questions/22783/why-are-the-bellman-operators-contractions](https://ai.stackexchange.com/questions/22783/why-are-the-bellman-operators-contractions)
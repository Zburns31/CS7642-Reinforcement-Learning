# What are Eligibility Traces?

- Eligibility traces unify and generalize TD and Monte Carlo methods
    - When TD methods are augmented with eligibility traces, they produce a family of methods spanning a spectrum that has Monte Carlo methods at one end ($\lambda$= 1) and one-step TD methods at the other ($\lambda$ = 0)
    - In between are intermediate methods that are often better than either extreme method
    - Eligibility traces also provide a way of implementing Monte Carlo methods online and on continuing problems without episodes
- One way of unifying TD and Mont Carlo methods is N-Step TD methods. However, eligibility traces go beyond this and offer significant computational advantages

# How do they work?

- The mechanism is a short-term memory vector, the eligibility trace, that parallels the long-term weight vector
- **Idea:** The rough idea is that when a component of $w_t$ participates in producing an estimated value, then the corresponding component of $z_t$ is bumped up and then begins to fade away
    - Learning will then occur in that component of $w_t$ if a nonzero TD error occurs before the trace falls back to o
    - **The trace decay parameter ($\lambda \in [0,1]$) determines the rate at which the trace falls**
- Eligibility traces relates back to the credit assignment problem. [Did they bell or light cause shock?](https://youtu.be/PnHCvfgC_ZA?t=5532)
    - **Frequency Heuristic:** assign credit to most frequent states
    - **Recency Heuristic:** assign credit to most recent states
    - ***Eligibility traces combine both***

        $$
        E_0(s) = 0
        \newline
        \\

        E_t(s) = \gamma \lambda E_{t-1}(s) + 1(S_t = s)
        $$


# $\lambda$-Return

- Previously, we defined n-step return as the sum of the first n rewards plus the estimated value of the state reached in n steps, each appropriately discounted. The general form of the equation is:

    $$
    G_{t:t+n} = R_{t+1} + \gamma R_{t+2} + ... + \gamma^{n-1} R_{t+n} + \gamma^n \hat v_{t+n -1}(S_{t+n}, w_{t+n-1})
    $$

    - Where
        - $\hat v(s,w)$ is the approximate value of state s given weight vector $w$
        - T is the time of the episode termination, if any
- An update that averages simpler component updates is called a ***compound update***
    - ***Note that a compound update can only be done when the longest of its component updates is complete***
    - Averaging produces a substantial new range of algorithms
    - I.e. We could average one-step and infinite step returns to obtain another way of interrelating TD and Monte Carlo Methods
        - Could also average experience-based updates with DP updates to get a simple combination of experience-based and model-based methods
    - **Example: An update can be done toward a target that is half of a two-step return and half of a four step return:**

        $$
        \frac{1}{2}G_{t:t+2} + \frac{1}{2}G_{t:t+4}

        $$


## TD($\lambda)$ Algorithm

- [Temporal Difference Learning (TD)](../../Module%201%20Intro%20to%20RL/Reinforcement%20Learning%20Models/Reinforcement%20Learning%20Models/Temporal%20Difference%20Learning%20(TD).md)

# Advantages & Disadvantages of Eligibility Traces

### Advantages

- The primary computational advantage of eligibility traces over n-step methods is that only a single trace vector is required rather than a store of the last n feature vectors
- Learning also occurs continually and uniformly in time rather than being delayed and then catching up at the end of the episode
    - Learning can occur and affect behaviour immediately after a state is encountered rather than being delayed n steps

# Resources

- [https://amreis.github.io/ml/reinf-learn/2017/11/02/reinforcement-learning-eligibility-traces.html](https://amreis.github.io/ml/reinf-learn/2017/11/02/reinforcement-learning-eligibility-traces.html)
- [https://lcalem.github.io/blog/2019/02/25/sutton-chap12](https://lcalem.github.io/blog/2019/02/25/sutton-chap12)
- [https://meatba11.medium.com/reinforcement-learning-td-λ-introduction-2-f0ea427cd395](https://meatba11.medium.com/reinforcement-learning-td-%CE%BB-introduction-2-f0ea427cd395)
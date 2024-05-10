# Why use Options?

- Convergence & Stability guarantees
- Avoid boring parts of the MDP
- State abstraction

# Considerations

- The options need be structured in the right way, otherwise we may not be able to find the optimal policy

## Options Components

- An option consists of 3 components $O = \langle I, \pi, \beta \rangle$
    1. $I$: An initiation set of states (aka precondition)
        1. The set of states where the particular option is legal to be executed (or makes sense to execute)
    2. $\pi(a|s)$: A policy $S \times A \rightarrow [0, 1]$
        1. $\pi(a|s)$ is the probability of taking action $a$ in state $s$ when following option $O$
    3. $\beta$: A termination condition
        1. The probability the option will terminate in a particular state


## Bellman Update

$$
V_{t+1} = \max_o (R(s,o) + \sum_{s'} F(s,o,s') V_t(s'))
$$

- Where:
    - $R(s,o)$ is an abstraction over actions $\rightarrow$ options
        - We replace $a$ with $o$
    - No discount factor
        - It does exist, just hidden
    - $F$ represents the transition function

## Reward Function

$$
R = E[r_1, + \gamma r_2 + \gamma^2 r_3 + .... + \gamma^{k-1} | \gamma, \space k \space  \text{steps}]
$$

- This is the average discounted reward that we will see by executing this option in a particular state

## Transition Function

$$
F = E[\gamma^k \Delta_{s_k, s'} | s, \space k \space  \text{steps}]
$$

- F is simply the expectation of the agent ending up in state $s'$ after $k$ steps and starting in state $s$ by taking option $o$
    - The main difference now between the original transition function $T$ and $F$ is that $F$ is the accumulation of all the transitions of starting in $s$ and ending up in $s'$ after $k$ steps given that we took the actions defined by $\pi$ and the option
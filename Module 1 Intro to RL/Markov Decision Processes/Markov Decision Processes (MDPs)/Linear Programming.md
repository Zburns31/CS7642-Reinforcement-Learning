# Linear Programming

# Intro to Linear Programming

- [Linear Programming](./Linear%20Programming%20Overview.md)

# Extending LP to MDP’s

- The basic idea of the formulation is to consider as variables in the LP the utilities *U(s)* of each state *s*, noting that the utilities for an optimal policy are the highest utilities attainable that are consistent with the Bellman equations
- In LP language, that means we seek to minimize for all subject to the inequalities (for every state *s* and action *a)*:

$$
U(s) \leq \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma U(s')]
$$
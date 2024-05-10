# What is Hierarchical RL?

- Hierarchical reinforcement learning(HRL) is a relatively new discipline in the deep learning space that models a learning problems as a hierarchy of related sub-problems

# How does it work?

- Essentially, the hierarchical RL agent is solving a Markovian decision problem with the following elements:
    - The states are the choice states $\sigma$ of the joint state space
    - The actions at $\sigma$ are the choices $c$ available in $\sigma$ according to the partial program
    - The reward function $\rho(\sigma, c, \sigma')$  is the expected sum of rewards for all physical transitions occurring between the choice states $\sigma$ and $\sigma'$
    - The transition model $\tau(\sigma, c, \sigma')$ is defined in the obvious way: if $c$ invokes a physical action $a$, then $\Gamma$ borrows from the physical model $P(s'|s,a)$; if c invokes a computational transition, such as calling a subroutine, then the transition deterministically modifies the state $m$ according to the rules of the programming language

# Terms

- **Choice State:** A choice state $\sigma = (s,m)$ is one in which the program counter for m is at a choice point in the agent program

# Resources

- [https://towardsdatascience.com/hierarchical-reinforcement-learning-56add31a21ab](https://towardsdatascience.com/hierarchical-reinforcement-learning-56add31a21ab)
- [https://towardsdatascience.com/hierarchical-reinforcement-learning-a2cca9b76097](https://towardsdatascience.com/hierarchical-reinforcement-learning-a2cca9b76097)
- [https://thegradient.pub/the-promise-of-hierarchical-reinforcement-learning/](https://thegradient.pub/the-promise-of-hierarchical-reinforcement-learning/)
- [https://link.springer.com/referenceworkentry/10.1007/978-0-387-30164-8_363](https://link.springer.com/referenceworkentry/10.1007/978-0-387-30164-8_363)
- [https://pub.towardsai.net/what-is-hierarchical-reinforcement-learning-e61f8a41c8e1](https://pub.towardsai.net/what-is-hierarchical-reinforcement-learning-e61f8a41c8e1)
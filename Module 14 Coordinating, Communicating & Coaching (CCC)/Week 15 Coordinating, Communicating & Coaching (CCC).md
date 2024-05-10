# Readings

- Zeibart et al. (2008)
- Babes et al. (2011)
- Griffith et al. (2013)

# Decentralized Partially Observable Markov Decision Processes (DEC-POMDP)

- **The decentralized partially observable Markov decision process (Dec-POMDP)**  [[1]](https://en.wikipedia.org/wiki/Decentralized_partially_observable_Markov_decision_process#cite_note-1)[[2]](https://en.wikipedia.org/wiki/Decentralized_partially_observable_Markov_decision_process#cite_note-2) is a model for coordination and [decision-making](https://en.wikipedia.org/wiki/Decision-making) among multiple agents
    - It is a [probabilistic](https://en.wikipedia.org/wiki/Probabilistic) model that can consider [uncertainty](https://en.wikipedia.org/wiki/Uncertainty) in outcomes, sensors and communication (i.e., costly, delayed, noisy or nonexistent communication)
- Dec-POMDPs represent a sequential problem. At each stage, each agent takes an action and receives:
    - A local observation
        - A local policy for each agent is a mapping from its observation sequences to actions, $\Omega * → A$
            - The state is unknown, so it may be beneficial to remember history
        - A joint policy is a local policy for each agent
    - A joint immediate reward

# Components

- **Components**
    - $I$: Finite set of agents
    - $S$: States
    - $A_i$: Agent $i$’s actions
    - $T$: Joint transition function $\rightarrow T(s,a,s')$
        - Where $a$ is a vector with one action per $I$
    - $R$: Shared reward function
    - $Z_i$ agent $i$’s observations
    - $O$: Observation function

# Inverse RL

- [Inverse Reinforcement Learning](./Week%2015%20Coordinating,%20Communicating%20&%20Coaching%20(CCC)/Inverse%20Reinforcement%20Learning.md)

# Policy Shaping

- [Policy Shaping](./Week%2015%20Coordinating,%20Communicating%20&%20Coaching%20(CCC)/Policy%20Shaping.md)

# Drama Management

- How can humans communicate with an agent?
    - Demonstrations (IRL)
    - Rewards (Reward Shaping)
    - Policies (Policy Shaping)
        - This is different from demonstrations because we are giving feedback to the agent rather than showing them what to do
- What’s a story?
    - Trajectory
- How do we influence a player to go down a good trajectory?

## Trajectories as MDPs: Targeted Trajectory Distribution

- Trajectories: Sequence so far
- Actions: Story actions
- Model: $P(t' | a,t)$
- Target Distribution: $P(T)$
    - $T$ represents final trajectories (i.e. the end of a story)
    - Good stories have higher probability, bad ones lower
- Policy: Probability distributions over actions

# Resources

- [http://rbr.cs.umass.edu/camato/decpomdp/overview.html](http://rbr.cs.umass.edu/camato/decpomdp/overview.html)
- [https://cse.buffalo.edu/~avereshc/rl_fall19/lecture_23_MDP_POMDP_DecPOMDP.pdf](https://cse.buffalo.edu/~avereshc/rl_fall19/lecture_23_MDP_POMDP_DecPOMDP.pdf)
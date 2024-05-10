# What are POMDPs?

- A Partially observable Markov decision process(POMDP) is an MDP with state uncertainty meaning we cannot know the true state, only a belief about the true state using observations
- The agent maintains a belief state, which is a probability distribution over the possible states of the environment. The belief state is updated using the current observation and the previous action taken by the agent

# How are POMDPs different than MDPs?

- A POMDP is an MDP with ***state uncertainty***
    - The agent receives an observation of the current state rather than the true state (potentially imperfect observations)
    - Using past observations, the agent builds a belief of their underlying state – Which can be represented by a probability distribution over true states
- When the environment is only partially observable, the situation is much less clear. The agent does not necessarily know which state it is in, so it cannot execute the action recommended for that state
    - ***Furthermore, the utility of a state s and the optimal action a depends not just on s, but also on how much the agent knows when it is in s***
- In POMDPs, the agent cannot directly observe the complete system state, but the agent makes observations that depend on the state
    - The agent uses these observations to form a belief about in what state the system currently is. This belief is called a ***belief state*** and is expressed as a probability distribution over all possible states
        - The solution of the POMDP is a policy prescribing which action to take in each belief state
- **Translation**
    - Take any MDP problem. It can be represented as a POMDP with z (observations) = s (states) and O(z, s) = 1 if z == s else 0. Since MDP problems can be represented as MDP problems, MDPs are problems that can be represented as POMDPs and also as MDPs
    - An MDP can always be translated to a POMDP. Any POMDP can also be written as an MDP over infinite belief states

# POMDP Definition

- **States:** $S = {s_1, s_2, s_3, ..., s_n}$ is a set of partially observable states
- ***A(s)* Actions: $A = {a_1, a_2,..., a_n}$**
    - The things you can do in a particular state
    - The choice that the agent makes at the current time step (for eg: it can move its right or left leg, or raise its arm, or lift an object, turn right or left, etc.). We know the set of actions (decisions) that the agent can perform in advance
    - State may govern the actions an agent is allowed to perform
- **Transition Model T:** T(state: *s*, action *a*, future state *s') ~= P(s'* | *s*, *a*)
    - Defines the rules of the game your playing
        - The physics of the environment we are operating in
    - The model produces a probability that you will end up transitioning to state *s'* given that you were in state *s* and took action *a*
        - **The probabilities must add up to 1**
- **Reward:** R(s), R(s, a), R(s, a, s')
    - A scalar value that you get for being in a state *s*
    - The rewards you receive from a state tells you the usefulness of entering into that state
    - R(s, a): The reward you receive for entering a state AND taking an action
    - R(s, a, s'): The reward you receive for entering a state, taking an action AND entering another state
    - **Minor changes to the reward function matter**
        - The rewards define the MDP and how quickly you want to reach the final (absorbing) state
        - They capture the domain knowledge
    - It is critical that the rewards are setup in such a way to truly indicate what we want accomplished
        - The reward signal is not the place to teach the agent HOW to achieve what we want or achieving subgoals
    - **Detailed Wiki on Rewards**

        - [More About Rewards (Utilities)](../../Module%201%20Intro%20to%20RL/More%20About%20Rewards/More%20About%20Rewards%20(Utilities).md)

- **Observations: $\Omega = {o_1, o_2, ..., o_n}$** is a set of observations
    - $Z$ is the observables
        - For an MDP $Z == S'$
- **Observation Probabilities:** is a set of observation probabilities $P(o | s',a)$ conditioned on the reached state and the taken action
    - Also known as the sensor model
    - The observations give us a hint about what state the agent is in

# State Estimation

- A belief state $b(s)$ is a probability distribution (vector) which tells us the probability we are in state $s$
- The belief state gets updated as we take actions
    - I.e. $b, a,z \rightarrow b'$
        - We have a belief state $b(s)$ and we take action $a$and observe $z$ and then get a new belief state $b'(s)$

- [State Estimation Updates](./Partially%20Observable%20MDPs%20(POMDPs)/State%20Estimation%20Updates.md)

# Solving POMDPs

- If $b$ was the previous belief state, and the agent does action $a$ and then perceives evidence $e$, then the new belief state is obtained by calculating the probability of now being in state $s'$, for each $s'$, with the following formula:

$$
b'(s') = \alpha P(e|s') \sum_s P(s'|s,a)b(s)
$$

- **Where**
    - $\alpha$ is a normalizing constant that makes the belief state sum to 1

- ***The fundamental insight required to understand POMDPs is this: the optimal action depends only on the agent’s current belief state***
    - An optimal policy can be described by a mapping $\pi^*(b)$ from belief states to actions
        - ***It does not depend on the actual state the agent is in***

# Predictive State Representation PSR

- POMDP: Belief state is distribution over states. But… the states are never observed? Do they even exist?
- PSR: Predictive state is probabilities of future predictions
    - Given our belief states, can we determine the probabilities of seeing some observation given an action?

- [Predictive State Representation](./Partially%20Observable%20MDPs%20(POMDPs)/Predictive%20State%20Representation.md)

# **Algorithm Overview**

- The decision cycle of a POMDP agent can be broken down into the following three steps:
    1. Given the current belief state $b$, execute the action $a = \pi^*(b)$
    2. Observe the evidence (percept) $e$
    3. Set the current belief state to the equation above and repeat

# Algorithms for Solving POMDPs

- [Value Iteration for POMDPs](./Partially%20Observable%20MDPs%20(POMDPs)/Value%20Iteration%20for%20POMDPs.md)

# RL for POMDPs

- Model-Based RL: Learn the POMDP & Plan from it
    - Expectation - Maximization
- Model-Free RL: Map observations to actions
- [Model - Free RL in POMDP’s](./Partially%20Observable%20MDPs%20(POMDPs)/Model%20-%20Free%20RL%20in%20POMDP%E2%80%99s.md)

# Key Terms

- **Belief State:** The set of actual states the agent might be in
    - I.e. A probability distribution over all possible states


# Resources

**POMDPs**

- [https://wiki.ubc.ca/Course:CPSC522/Partially_Observable_Markov_Decision_Processes](https://wiki.ubc.ca/Course:CPSC522/Partially_Observable_Markov_Decision_Processes)
- [https://en.wikipedia.org/wiki/Partially_observable_Markov_decision_process](https://en.wikipedia.org/wiki/Partially_observable_Markov_decision_process)
- [https://www.comp.nus.edu.sg/~leews/MLSS/POMDP.pdf](https://www.comp.nus.edu.sg/~leews/MLSS/POMDP.pdf)
- [https://cs.uwaterloo.ca/~ppoupart/teaching/cs886-fall05/lectureNotes/cs886-lecture1.pdf](https://cs.uwaterloo.ca/~ppoupart/teaching/cs886-fall05/lectureNotes/cs886-lecture1.pdf)
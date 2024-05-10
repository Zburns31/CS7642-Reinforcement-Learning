# What is it?

- In [computer science](https://en.wikipedia.org/wiki/Computer_science), a **predictive state representation** (**PSR**) is a way to model a state of controlled [dynamical system](https://en.wikipedia.org/wiki/Dynamical_system) from a history of actions taken and resulting observations
    - PSR captures the state of a system as a vector of predictions for future tests (experiments) that can be done on the system
    - A test is a sequence of action-observation pairs and its prediction is the probability of the test's observation-sequence happening if the test's action-sequence were to be executed on the system
- The key idea behind PSR is to represent the system as a finite set of states, where each state is associated with a prediction of the future behaviour of the system based on the past observations and actions
    - In other words, each state in the PSR represents a summary of the information that is relevant for predicting the future behaviour of the system
- The PSR framework defines a set of equations that describe how the system transitions between states in response to the actions taken by the agent and the observations received from the environment
- These equations are used to update the PSR as new observations and actions are received, allowing the PSR to adapt to changes in the system over time
- Once the PSR is learned from data, it can be used to make predictions about the future behaviour of the system given a particular sequence of actions and observations
    - For example, the PSR can be used to predict the expected reward or the probability of reaching a particular state given a particular policy

# PSR Theorem

- **Any n-state POMDP can be represented by a PSR with no more than n tests, each of which is no longer than n steps long**
- Can we use our belief state and translate this into a predictive state?
    - What’s the distribution over outcomes for our tests?
    - Can go from a belief state to a predictive state but not the other way around
        - Since there are infinitely many combinations of predictive states that could map back to the belief state
        - **We need enough tests to create a bijection mapping**
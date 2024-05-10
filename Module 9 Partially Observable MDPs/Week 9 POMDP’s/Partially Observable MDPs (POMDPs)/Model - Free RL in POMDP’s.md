# Learning Memoryless Policies

- Memoryless policies, also known as Markov policies, are policies in which the action taken by an agent at each time step depends only on the current state of the environment and not on any previous states or actions
- Formally, a memoryless policy is a function that maps each state in the state space of a Markov decision process (MDP) to a probability distribution over the set of possible actions that can be taken in that state
    - The policy is called "memoryless" because the probability distribution for each state is independent of any previous states or actions taken by the agent

# Bayesian RL

- In Model Based Bayesian RL, we account for the uncertainties utilized in the model
- Different transition models may yield different results for the same input. **How do we characterize the uncertainties due to these different transitions?**
- **Basic Idea:** The idea is to keep a Bayesian posterior over possible MDP’s we are in and we can use this to balance exploration and exploitation
    - **Optimal Policy:** This allows us to determine which MDP we are in and then execute the optimal policy
- Bayesian RL is actually planning in a kind of continuous MDP
    - Hidden states are the parameters of the MDP
- [Bayesian RL](./Model%20-%20Free%20RL%20in%20POMDP’s/Bayesian%20RL.md)

# Resources

- [https://cbl.eng.cam.ac.uk/pub/Intranet/MLG/ReadingGroup/Bayesian_Reinforcement_Learning_Neil.pdf](https://cbl.eng.cam.ac.uk/pub/Intranet/MLG/ReadingGroup/Bayesian_Reinforcement_Learning_Neil.pdf)
- [https://mohammadghavamzadeh.github.io/PUBLICATIONS/FoundationTrend-BRL.pdf](https://mohammadghavamzadeh.github.io/PUBLICATIONS/FoundationTrend-BRL.pdf)
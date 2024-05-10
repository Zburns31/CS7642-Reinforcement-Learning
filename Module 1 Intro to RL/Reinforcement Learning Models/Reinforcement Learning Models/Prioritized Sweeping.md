# What is Prioritized Sweeping?

- In the Dyna agents presented in the preceding sections, simulated transitions are started in state–action pairs selected uniformly at random from all previously experienced pairs
    - But a uniform selection is usually not the best; planning can be much more ecient if simulated transitions and updates are focused on particular state–action pairs
- Prioritized sweeping is just one way of distributing computations to improve planning efficiency, and probably not the best way
    - One of prioritized sweeping’s limitations is that it uses expected updates, which in stochastic environments may waste lots of computation on low-probability transitions

## Problems with Dyna-Q

![Untitled](./Prioritized%20Sweeping/Untitled.png)

- At the beginning of the second episode, only the state–action pair leading directly into the goal has a positive value; the values of all other pairs are still zero
    - This means that it is pointless to perform updates along almost all transitions, because they take the agent from one zero-valued state to another, and thus the updates would have no effect
    - Only an update along a transition into the state just prior to the goal, or from it, will change any values
        - ***If simulated transitions are generated uniformly, then many wasteful updates will be made before stumbling onto one of these useful ones***


## How does it work?

- **Backward Focusing**
    - ***In general, we want to work back not just from goal states but from any state whose value has changed***
        - Suppose that the values are initially correct given the model, as they were in the maze example prior to discovering the goal
            - Suppose now that the agent discovers a change in the environment and changes its estimated value of one state, either up or down
            - Typically, this will imply that the values of many other states should also be changed, but the only useful one-step updates are those of actions that lead directly into the one state whose value has been changed
                - If the values of these actions are updated, then the values of the predecessor states may change in turn
                - If so, then actions leading into them need to be updated, and then their predecessor states may have changed
            - In this way one can work backward from arbitrary states that have changed in value, either performing useful updates or terminating the propagation

- **Prioritized Sweeping:**
    - It is natural to prioritize the updates according to a measure of their urgency, and perform them in order of priority
    - A queue is maintained of every state–action pair whose estimated value would change nontrivially if updated , prioritized by the size of the change
        - When the top pair in the queue is updated, the effect on each of its predecessor pairs is computed
        - If the effect is greater than some small threshold, then the pair is inserted in the queue with the new priority (if there is a previous entry of the pair in the queue, then insertion results in only the higher priority entry remaining in the queue)

## Algorithm Overview

![Untitled](./Prioritized%20Sweeping/Untitled%201.png)
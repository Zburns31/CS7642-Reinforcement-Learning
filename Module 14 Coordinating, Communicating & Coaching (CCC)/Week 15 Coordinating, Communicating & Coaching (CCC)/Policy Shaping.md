# What is it?

- Policy shaping in reinforcement learning (RL) refers to the process of modifying or augmenting the rewards or observations received by an RL agent during training in order to guide its learning process towards a desired behaviour or policy
- Policy shaping can be used to bias the agent's exploration, speed up learning, and encourage the agent to take specific actions or follow certain behavioural patterns
- There are several ways to implement policy shaping in RL
    - One common approach is to add additional rewards or penalties to the original reward signal received by the agent
        - These additional rewards or penalties can be designed based on domain knowledge or expert demonstrations to encourage or discourage certain actions
        - For example, if the agent is learning to navigate a maze, a shaping reward can be added to encourage the agent to move towards the goal or avoid obstacles
    - Another approach to policy shaping is to modify the agent's observations or state representation
        - This can involve providing the agent with additional information or altering the way the state is perceived
        - For example, in a robotic manipulation task, the agent may receive additional information about the position or orientation of objects in the environment to help it grasp objects more effectively


# How to compute whether an action is optimal given evidence

**General Case (Potentially Multiple Optimal Actions)**

$$
P(a|d_a) = \dfrac {C^{\Delta_a}}{C^{\Delta_a} + (1-C)^{\Delta_a}}
$$

- $P(a|d_a)$ represents the probability for a given state, action $a$ is the optimal one given a sequence of labels that say yes this is optimal or no it is not optimal
- $C$ is the probability the label is correct
- $\Delta_a$ is the difference between the number of times yes was said and the number of times no was said
- $\Delta_j$ is the difference between the number of times yes and no was said for every other action that isn’t the action $a$ in scope
- Notes:
    - This equation tells us what the probability is, not necessarily how confident we are in that answer

**Only One Right Answer (Highlander Case)**

$$
P(a|d_a) \propto C^{\Delta_a} (1-C)^{\sum_{j \neq a} \Delta_j}
$$

- Where:
    - $\Delta_a$ = # of yes - # no
    - $d_a$ is the data set of labels, number of times you said yes or no as to whether $a$ is the optimal action
    - $\propto$ means proportional to, meaning that the quantity will need to be normalized


# Multiple Sources

- What happens if we have learned a policy that gives us a probability distribution over actions from training and a separate one from a human/expert?
    - $\pi_H \rightarrow x = \frac{2}{3} \space y = \frac{1}{6} \space z = \frac{1}{6}$
    - $\pi_A \rightarrow x = \frac{1}{3} \space y = \frac{1}{3} \space z = \frac{1}{3}$
- We can use the formula above to aggregate the two distributions into one by using $C$ (the probability the label is correct) as the mixing factor (how much we believe one estimate over the other)
- [Calculation](https://edstem.org/us/courses/33453/lessons/52872/slides/302408)
    - Take the products.
    - Maximize the probability:

    $$
    a = \argmax_a P(a|\pi_1) \cdot P(a|\pi_2)
    $$

    - This is the probability that they agree
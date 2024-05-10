# What is it?
- Nash Q-Learning is an extension of the traditional Q-learning algorithm to a noncooperative multiagent context
    - Nash Q-Learning assumes that agents behave according to Nash equilibrium during learning
    - Nash equilibrium is a concept from game theory where no agent can unilaterally improve its reward by changing its strategy while other agents keep their strategies fixed
    - In Nash Q-Learning, each agent aims to learn its equilibrium Q-values, considering the Q-values of other agents

# How does it work?

- In the Nash Q-learning algorithm, the Nash-values need to be calculated at each state in order to update the action-value functions and find the equilibrium strategies
- The Nash Q-learning algorithm extends the Q-learning algorithm to the noncooperative multiagent context
    - The learning agent maintains a Q-function over joint actions and performs updates by assuming the existence of Nash equilibrium

- Value iteration doesn’t work
- Nash-Q doesn’t converge
- No unique solution to $Q^*$
- Policies cannot be computed independently $\rightarrow$ policies can be incompatible
- Update not efficient
- Q functions are not sufficient to specify the policy

# Resources

- [https://learning.oreilly.com/library/view/multi-agent-machine-learning/9781118362082/9781118884485c04.xhtml#c04_level1_4](https://learning.oreilly.com/library/view/multi-agent-machine-learning/9781118362082/9781118884485c04.xhtml#c04_level1_4)
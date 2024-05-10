# How it works

- **Basic Idea:** is to give small intermediate rewards to the algorithm that help it converge more quickly
    - Put bonuses on some states of the world. When you achieve a certain state then you would get that bonus. When you leave that state, you lose the bonus
- Reward shaping is **a method for engineering a reward function in order to provide more frequent feedback on appropriate behaviours**
    - ***It is most often discussed in the reinforcement learning framework. Providing feedback is crucial during early learning so that promising behaviours are tried early***
- In many applications, you will have some idea of what a good solution should look like
    - For example, in our simple navigation task, it is clear that moving towards the reward of +1 and away from the reward of -1 are likely to be good solutions
    - Can we then speed up learning and/or improve our final solution by nudging our reinforcement learner towards this behaviour?
    - **Yes we can!** We can modify our reinforcement learning algorithm slightly to give the algorithm some information to help, while also guaranteeing optimality
- So, in the original MDP, we have a reward function defined as $R(s,a,s')$ but in the modified MDP which contains a shaping reward function, we now have $R(s,a,s') + F(s,a,s')$
    - This is the most general possible form of rewards shaping functions
    - An example could be that $F(s,a,s') = r$ whenever *s’* is closer to the goal than *s,* and $F(s,a,s') = 0$ otherwise

## Approaches

- How do we change the MDP reward function without changing the optimal policy?
    - Multiply by positive constant
    - Shift by some constant (positive or negative)
    - Potential-Based Rewards
- Another approach for finding a good reward signal is using something like grid search within a defined space to find an optimal reward

# Potential-Based Reward Shaping

- P*otential-based* reward shaping is a particular type of reward shaping with nice theoretical guarantees. In potential based reward shaping, *F* is of the form $F(s,s') = \gamma \phi(s') - \phi(s)$
    - Where:
        - $\phi(s)$ represents the potential of the state we are leaving
        - $\gamma \phi(s)$ represents the discounted potential of the state we are entering
        - ***The difference here represents the incremental improvement towards the goal***
            - We subtract the potential of leaving a state as this helps avoiding positive sub-optimal loops
    - ***We call $\phi$ the potential function and $\phi(s)$ is the potential of state s***
    - **Theoretical guarantee**: this will still converge to the optimal policy under the assumption that all state-action pairs are sampled infinitely often

## Why does this work?

- See [video](https://edstem.org/us/courses/33453/lessons/52864/slides/302100)
    - This happens due to the ***telescoping sum***
- Basically, using potential-based reward shaping we can prove that $Q'(s,a) = Q(s,a) - \psi(s)$
    - This basically says that the true value of being in a state and taking an action is whatever the value is minus the potential that you’re going to lose by leaving that state

## Properties

- Q learning with potential based shaping will return the same policy and go through the same action choices as Q learning with Q-function initialization

## Change-In-State-Based Bonuses

- Instead of giving little bonus rewards every time that you do a certain action, we are going to keep track of what state of the world is, and as we move to more (or less) desirable states we will get additional reward for that
    - ***The difference here between giving additional rewards for every time an action is taken, is that we reverse the state-based bonus when we leave that state***
    - Example:
        - In the below example, when a move from a state that is 10 pixels away to 5 pixels away, the change in reward we get is +0.1 ($\frac{1}{5} - \frac{1}{10}$) and -0.1 for vice versa

    ![Untitled](./Reward%20Shaping/Untitled.png)
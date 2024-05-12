# Overview

- Upper Confidence Bound (UCB) is the most widely used solution method for multi-armed bandit problems. This algorithm is based on the principle of optimism in the face of uncertainty
- ***In other words, the more uncertain we are about an arm, the more important it becomes to explore that arm***

# How it works

- Distribution of action-value functions for 3 different arms $a_1, a_2$ and $a_3$ after several trials is shown in the figure above
    - ***This distribution shows that the action value for $a_1$ has the highest variance and hence maximum uncertainty***
- UCB says that we should choose the arm $a_1$ and receive a reward making us less uncertain about its action-value. For the next trial (time step), if we still are very uncertain about a we will choose it again until the uncertainty is reduced below a threshold

![Untitled](./Upper%20Confidence%20Bound%20(UCB)/Untitled.png)

## Why does this work?

The intuitive reason this works is that when acting optimistically in this way, one of two things happen:

1. Optimism is justified and we get a positive reward which is the objective ultimately
2. The optimism was not justified
    1. In this case, we play an arm that we believed might give a large reward when in fact it does not
        1. If this happens sufficiently often, then we will learn what is the true payoff of this action and not choose it in the future

# Resources

- [https://www.analyticsvidhya.com/blog/2018/09/reinforcement-multi-armed-bandit-scratch-python/](https://www.analyticsvidhya.com/blog/2018/09/reinforcement-multi-armed-bandit-scratch-python/)
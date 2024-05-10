# What is it?

- Based on the [presentation](https://www.ics.uci.edu/~dechter/courses/ics-295/winter-2018/presentations/Praveen.pdf) by Praveen Venkateswaran on the same paper, KWIK framework is any learning algorithm that makes the following:
    1. If it knows (by its learning) the answer already, it does tell that
    2. If it is uncertain, it tells the "I don't know" mark
    3. "I don't know" marks should have an upper limit, which is the accuracy/error term of the algorithm.
- The idea is that algorithm has a trainer who knows the correct answers, like in any machine learning case, but the difference is that the algorithm agent should know when it has enough knowledge to give the result right away, and when is time to ask and learn. In learning phase the trainer does not calculate errors, only he gives right answers if
asked
- A framework for self-aware learning
    - Combines elements of Probably Approximately Correct (PAC) and mistake-bound models
    - Useful for active learning

# Overview

- The KWIK (Know What I Know) framework is a type of reinforcement learning algorithm that is designed to operate in situations where the reward function is not known in advance. In KWIK, the agent interacts with the environment to learn the optimal policy while minimizing the number of mistakes made during the learning process

The KWIK framework consists of three main components:

1. Exploration: The agent explores the environment to gather information about the reward function.
2. Exploitation: The agent uses the information gathered during exploration to choose actions that maximize the expected reward.
3. Verification: The agent periodically checks if its estimate of the reward function is accurate, and if not, it returns to the exploration phase.
- In KWIK, the agent makes decisions based on a confidence bound, which is a range of possible values for the reward function. The confidence bound is initially large, which allows the agent to explore the environment and gather information. As the agent gains more information, the confidence bound becomes smaller, and the agent can start exploiting its knowledge to maximize the expected reward
- If the agent makes a mistake, it updates the confidence bound based on the observed outcome and returns to the exploration phase to gather more information. The verification phase periodically checks if the confidence bound is small enough, and if not, the agent returns to the exploration phase

# How it works

- The goal of a learning algorithm is to learn a target function $f(x)$
- In the Online Mistake Bound Learning model, the learning session proceeds in a sequence of trials. Throughout the session, the learner maintains an updatable hypothesis $h$ and, assuming for now that $Y = {0,1}$, in each trial the learner:
    - is given an unlabeled example $x \in X$
    - outputs $h(x) \in Y \cup \perp$
        - If $h(x) \neq \perp$ then $|h(x) - c(x)| < \epsilon$ with probability $1 - \delta$
    - is told the true label $c(x)$ and if $c(x) \neq h(x)$ there is a mistake
    - Updates its hypothesis $h$ if there was a $\perp$ answer
        - $\perp$ represents when the agent does not know the proper answer
- The total number of mistakes should be bounded

# Resources

[Li-Littman-Walsh-2008.pdf](Knows%20What%20It%20Knowns%20Self-Aware%20Learning%20ca8ee21d292b43919829c5fd8afd90d8/Li-Littman-Walsh-2008.pdf)
- [https://ai.stackexchange.com/questions/20769/what-is-the-kwik-framework](https://ai.stackexchange.com/questions/20769/what-is-the-kwik-framework)
- [https://github.com/amirziai/learning/blob/master/reinforcement-learning/kwik.ipynb](https://github.com/amirziai/learning/blob/master/reinforcement-learning/kwik.ipynb)
- [https://proceedings.mlr.press/v19/szita11a/szita11a.pdf](https://proceedings.mlr.press/v19/szita11a/szita11a.pdf)
- [https://nikosfl.github.io/work/classes/projects/proj_nf_csci599b.pdf](https://nikosfl.github.io/work/classes/projects/proj_nf_csci599b.pdf)
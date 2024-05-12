# CS7642-Reinforcement-Learning
This repository contains my course notes and course summary from Reinforcement Learning

## Course Resources
- [OMSCS Course Page](https://omscs.gatech.edu/cs-7642-reinforcement-learning)
- [Textbook](https://www.andrew.cmu.edu/course/10-703/textbook/BartoSutton.pdf)
- [Professor Littman's PHD Dissertation](https://ics.uci.edu/~dechter/courses/ics-295/winter-2018/papers/littman1.pdf)
- [DeepMind & David Silver Lectures](https://www.youtube.com/playlist?list=PLqYmG7hTraZDM-OYHWgPebj2MfCFzFObQ)

## What is Reinforcement Learning?
Reinforcement learning is like teaching a robot or computer program to learn by playing games. Imagine you have a little robot friend who wants to get better at a game. Every time it does something good, like scoring a point, you give it a high-five or a thumbs-up. That’s positive reinforcement! It encourages the robot to keep doing those good things.

But sometimes, the robot makes mistakes. Maybe it walks into a wall or falls down. That’s where challenges come in. The robot needs to figure out how to avoid those mistakes and get better. It’s like when you learn to ride a bike – you fall a few times, but each time you learn something new.

Now, let’s talk about some cool things that use reinforcement learning:

- Self-Driving Cars: Imagine a car that learns to drive itself by practicing on virtual roads. It gets rewards for staying in the right lane and avoiding accidents.
- Game Playing AI: Computers can learn to play games like chess, Go, or even video games. They get better over time by trying different moves and learning from their mistakes.
- Robotics: Robots can learn to do tasks like picking up objects or folding laundry. They learn by trial and error, just like you do when you learn a new skill.


## Course Summary

Overall, this was my favourite class in the program. It was extremely challenging (especially without having a solid grasp of PyTorch and general Deep Learning methods) but also very rewarding. Of all the classes I have taken, I can without a doubt say I have learned a lot about the RL field and have taken away more knowledge from this course than many others. The content is well structured and the lectures were fantastic. I would highly recommend this course to anyone in the OMS CS program. It seems to strike the right balance between diving deep into the theory and applying this theory to real-world problems.

The content for the course can be seen below:

- [Module 1: Intro to RL](./Module%201%20Intro%20to%20RL/Module%201%20Intro%20to%20RL.md)
- [Module 2: RL Basics](./Module%202%20RL%20Basics/Week%202%20RL%20Basics.md)
- [Module 3: TD & Friends](./Module%203%20TD%20&%20Friends/Week%203%20TD%20&%20Friends.md)
- [Module 4 & 5: Convergence](./Module%204%20&%205%20Convergence/Weeks%204%20&%205%20Convergence.md)
- [Module 6: AAA & Messing with Rewards](./Module%206%20AAA%20&%20Messing%20with%20Rewards/Week%206%20AAA%20&%20Messing%20with%20Rewards.md)
- [Module 7: Exploring Exploration](./Module%207%20Exploring%20Exploration/Week%207%20Exploring%20Exploration%20.md)
- [Module 8: Generalization](./Module%208%20Generalization/Week%208%20Generalization.md)
- [Module 9: Partially Observable ](./Module%209%20Partially%20Observable%20MDPs/Week%209%20POMDP’s.md)
- [Module 10: Options](./Module%2010%20Options/Week%2010%20Options.md)
- [Module 11: Game Theory](./Module%2011%20Game%20Theory%20/Week%2012%20Game%20Theory.md)
- [Module 12: Game Theory Reloaded](./Module%2012%20Game%20Theory%20Reloaded/)
- [Module 13: Game Theory Revolutions](./Module%2013%20Game%20Theory%20Revolutions/Week%2014%20Game%20Theory%20Revolutions.md)
- [Module 14: Coordinating, Communicating & Coaching (CCC)](./Module%2014%20Coordinating,%20Communicating%20&%20Coaching%20(CCC)/Week%2015%20Coordinating,%20Communicating%20&%20Coaching%20(CCC).md)


## Homework Summaries

### HW1
HW1 was a relatively simple introduction into Value iteration through the N-sided die MDP problem. The basic premise of the game is that at any given point in time, you need to decide whether to keep rolling or play the game. You start with $0 and each time you roll a side that is a good side, you recieve that number of dollars. If you roll a bad side of the die, you lose your bankroll and the game terminates. The strategy to the game is understand and estimate the expected value of the game and create an optimal strategy given your current bank roll. Doing so will result in finding the optimal value function given enough iterations.

### HW2
HW2 focused on the TD lambda method which is essentially a weighted combination of the N-Step returns for some task.

Consider the Markov reward process described by the following state diagram and assume the agent is in state $0$ at time $t$ (also assume the discount rate is $\gamma=1$). A Markov reward process can be thought of as an MDP with only one action possible from each state (denoted as action $0$ in
the figure below).

![image](https://d1b10bmlvqabco.cloudfront.net/paste/jqmfo7d3watba/9e6fd83672f880704b8418728297fc077786c8907d87fec631601da9ff4c85ef/hw2.png)

The goal of this assignment was to estimate value function at time $t$, represented as a vector $[V(0), V(1), V(2), V(3), V(4), V(5), V(6)]$

### HW3
For this HW3, we built a Sarsa agent which learns policies in the [OpenAI Gym](http://gym.openai.com/docs/) Frozen Lake environment.  [OpenAI Gym](http://gym.openai.com/docs/) is a platform where users can test their RL algorithms on a selection of carefully crafted environments.

This HW focused on the Sarsa algorithm which uses temporal-difference learning to form a model-free on-policy reinforcement-learning algorithm that solves the *control* problem. It is model free because it does not need and does not use a model of the environment, namely neither a transition nor reward function; instead, Sarsa samples transitions and rewards online.

The goal of this assignment was to output the $\epsilon$-greedy policy in the form of movements(**<, v, >, ^,**) for each possible state in the grid world environment.

### HW4
For HW4, we were tasked with implementing a Q-learning agent to try and solve the Taxi problem which was introduced by [Dietterich 1998](https://www.jair.org/index.php/jair/article/download/10266/24463) and has been used for reinforcement-learning research in the past.  It is a grid-based environment where the goal of the agent is to pick up a passenger at one location and drop them off at another.

Q-learning is a fundamental reinforcement-learning algorithm that has been successfully used to solve a variety of  decision-making  problems.   Like  Sarsa,  it  is  a  model-free  method  based  on  temporal-difference  learning. However, unlike Sarsa, Q-learning is *off-policy*, which means the policy it learns about can be different than the policy it uses to generate its behavior.  In Q-learning, this *target* policy is the greedy policy with respect to the current value-function estimate.

The primary goal is to find the optimal Q values ($Q^*$) for the environment to dictate the optimal policy to take using an $\epsilon$-greedy policy.

### HW5
HW5 focused on implementing a Knows-What-It-Knows (KWIK) learner which aims to learn to predict whether a fight will break out among the subset of patrons present on a given evening, without initially knowing the identity of the instigator or the peacemaker.

A Knows What It Knows (KWIK) learner is a framework designed for self-aware learning, particularly in settings where active exploration can impact the training examples the learner is exposed to.

### HW6
For this assignment, we were asked to compute the Nash equilibrium for the given zero sum games. The goal was to find the ideal mixed strategy for the game. While there are different ways to calculate this, we used Linear Programming to help prepare for the final project. We used the Linear Programming solver CVXPY (<https://www.cvxpy.org/index.html>) to create a program that can solve Rock, Paper, Scissors games with arbitrary reward matrices

The goal is to return a vector of the Nash equilibrium probabilities for player A found by the linear program

## Project Summaries

### Project 1
For project 1, we were asked to read the research paper [Learning to Predict by the Methods of Temporal Differences](http://incompleteideas.net/papers/sutton-88-with-erratum.pdf), implement the algorithms outlined in the paper and attempt to replicate the results. This involved building and analyzing the behaviour of the TD($\lambda$) algorithms on the bounded random walk problem.

### Project 2
The focus of this project was to build a Reinforcement Learning (RL) agent to solve [Open AI’s Lunar Lander environment](https://gymnasium.farama.org/). First, we implemented a simple Deep Q-Network (DQN) agent as a baseline to determine performance within the environment. Then, we introduced additional variations on DQN such as Fixed-Target DQN and Double DQN to see how improving on the weaknesses of the original DQN implementation can improve performance. Additionally, we performed three experiments in which we vary the learning rate ($α$), random action selection rate ($ε$) and the discount rate ($γ$) which attempt to explain how each hyperparameter impacts the ability of the agent to learn.

### Project 3
In this project we implement, train and analyze several Multi-Agent Reinforcement Learning (MARL) methods by utilizing the [Google Research Football environment](https://github.com/google-research/football) which provides a way to train and test MARL methods in a mixed Cooperative- Competitive environment. We used RLlib to experiment with a variety of algorithms. This project was much more involved as we had to setup a docker container to run these algorithms as well as utilzing tensorboard and Ray for model training, tracking and hyperparameter tuning. For my project, I experimented with Proximal Policy Optimization, Actor-Critic Methods (A2C, A3C, SAC) and Deep Q-Network variants (APEX). Overall, it was a really fun but challenging project which addresses a heavily studied topic in the RL world (Multi-Agent RL).

### Exam
The exam was a cumulative M/C and short answer test covering all the topics from the class. It wasn't too difficult but required you to have done most, if not all of the assigned readings as most topics were covered, atleast briefly.


# Questions?

If you are a potential employer looking at this, I would be happy to go into more depth about the solutions to the work on the above assignments.

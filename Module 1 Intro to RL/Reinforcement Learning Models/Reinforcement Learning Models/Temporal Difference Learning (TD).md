# What is TD Learning?

- TD learning does not require the agent to learn the transition model (model-free). The update occurs between successive states and agent only updates states that are directly affected
    - Uses bootstrapping; Learns from incomplete episodes
- **Temporal Difference** is [an approach to learning how to predict a quantity that depends on future values of a given signal](http://www.scholarpedia.org/article/Temporal_difference_learning)
    - We look at each $<s,a,s’>$ transition and try to predict the sum of the discounted rewards
- TD learning **uses experience** to solve the prediction problem, just like [Monte Carlo Methods](https://medium.com/analytics-vidhya/monte-carlo-methods-in-reinforcement-learning-part-1-on-policy-methods-1f004d59686a?source=friends_link&sk=80e83d6c07c46830d9efc7b8ccd810d0)
- I**dea:**
    - Does not need to learn explicit model of the environment to perform its updates
    - Use update rule that implicitly reflects transition probabilities
- One of the problems with the environment is that rewards usually are not immediately observable
    - For example, in tic-tac-toe or others, we only know the reward(s) on the final move (terminal state). All other moves will have 0 immediate rewards
    - TD learning is an unsupervised technique to predict a variable's expected value in a sequence of states

# TD(0) Algorithm

## How does it work?

- ***TD(0) is basically a one-step look ahead prediction method***
- Initialize $U^\pi(s)$ with $R(s)$ when first visited
- After each transition, update with the following equation (Temporal Difference (TD(0) equation):

$$
U_T(s) = U_T(s) + \alpha [R_{t+1} + \gamma U_T(s_{t+1}) - U_T(s_t)]
$$

- Where:
    - $\alpha$ represents the learning rate
        - $\alpha$ should decrease slowly over time, so that estimates stabilize eventually
- Notes:
    - **TD Error:** $R_{t+1} + \gamma U_T(s_{t+1}) - U_T(s_t)$ is effectively an error signal, and the update is intended to reduce the error
        - The TD error at each time is the ***error in the estimate made at that time***
        - The difference between what you thought the value was and what the value turned out to be when you actually saw the reward (I.e. before action Vs. after action estimate)
    - **TD Target: $R_{t+1} + \gamma U(S_{t+1})$**
    - This update is being computed more frequently in the states we end up in more often and less frequently in states that we don’t encounter often
        - We are essentially sampling different possibly $U_T(s_{t+1})$ next states


## TD(0) Prediction

- Both TD and Monte Carlo methods use experience to solve the prediction problem. Given some experience following a policy $\pi$, both methods update their estimate V of $v_\pi$ for the nonterminal states $S_t$ occurring in that experience
- ***The equation above is called TD(0), or one-step TD, because it is a special case of the TD($\lambda$) algorithm and n-step TD methods***
- Because TD(0) bases its update in part on an existing estimate, we say that it is a bootstrapping method, like DP

## Algorithm Overview

![Untitled](./Temporal%20Difference%20Learning%20(TD)/Untitled.png)

Notes:

- We refer to TD and Monte Carlo updates as sample updates because they involve looking ahead to a sample successor state (or state–action pair), using the value of the successor and the reward along the way to compute a backed-up value, and then updating the value of the original state (or state– action pair) accordingly
    - Sample updates differ from the expected updates of DP methods in that they are based on a single sample successor rather than on a complete distribution of all possible successors

# N-Step TD

- Methods that involve an intermediate amount of bootstrapping are important because they will typically perform better than either extreme

- [N-Step TD Methods](./Temporal%20Difference%20Learning%20(TD)/N-Step%20TD%20Methods.md)

# TD($\lambda$) Algorithm & N-Step Truncated $\lambda$-Return Methods

- [TD Lambda Algorithm](./Temporal%20Difference%20Learning%20(TD)/TD%20Lambda%20Algorithm.md)

# TD Learning Characteristics & Properties

## TD Learning Vs. ADP

- Both try to make local adjustments to the utility estimates in order to make each state “agree” with its successors
- ***One difference is that TD adjusts a state to agree with its observed successor, whereas ADP adjusts the state to agree with all of the successors that might occur, weighted by their probabilities***
- A more important difference is that whereas TD makes a single adjustment per observed transition, ADP makes as many as it needs to restore consistency between the utility estimates U and the transition model P

## Advantages of TD Methods

- **Model-Free:** TD methods do not require a model of the environment, of its reward and next-state probability distributions
- **Implementation:** TD methods are naturally implemented in an online, fully incremental fashion
    - With Monte Carlo methods one must wait until the end of an episode, because only then is the return known, whereas with TD methods one need wait only one time step
    - ***Can learn before knowing the final outcome.*** TD can learn online after every step vs MC methods which must wait until the end of an episode before a return is known
    - TD can learn without the final outcome
        - I.e. Can learn from incomplete sequences
- **Convergence:** Is it guaranteed to converge on the optimal value function - $v_\pi$?
    - For any policy $\pi$, TD(0) has been shown to converge to $v_\pi$, in the mean for a constant step-size parameter if it is sufficiently small, and with probability 1 if the step-size parameter decreases according to the usual stochastic approximation conditions
- **Lower Variance:** TD target is much lower variance than the return
    - Return depends on many random actions, transitions and rewards
    - TD target depends on one random action, transition and reward

## Disadvantages of TD Methods

- **Higher Bias:** TD Target $R_{t+1} + \gamma U(S_{t+1})$ is a biased estimate of $v_\pi$

## Stochastic Principle of Approximation: Properties of Learning Rates

- In the limit, $\lim_{t\rightarrow \infin} V_T(S) = V(S)$, the approximated value function will approach the true value function once $T$ is large enough. In order for this to hold, two conditions need to put on the learning rate:
    1. $\sum_t \alpha_t = \infin$
    2. $\sum_t \alpha_t^2 < \infin$
        1. Needs to be finite to avoid large step sizes within the updates
    - These conditions need to because the learning rates need to be large enough so that you can move to whatever the true value is, no matter where you start
        - The learning rates also need to be small enough so that they don’t damp out the noise and do a proper job of averaging
    - Scenarios for $\alpha$
        - Any time that the power that we’re raising to the $T$ to in the denominator is bigger than one, it’s going to converge to a finite value
        - If the exponent in the learning rate is $\text{\textgreater}$ 1 then it will converge to a finite number
        - If the exponent in the learning rate is $\text{\textless}$ 1 then it will not converge to a finite number
        - ***If the learning rate doesn’t decrease over time, [it won’t converge](https://edstem.org/us/courses/33453/lessons/52861/slides/302022)***

## Optimality of TD Learning

- In the case of when only a finite amount of experience is available, say 10 episodes or 100 time steps, a common approach is to do ***[batch updating](https://stats.stackexchange.com/questions/297708/batch-reinforcement-learning-algorithm-example)***
    - The idea here is to present the experience repeatedly until the method converges upon an answer
    - It is called batch updating because updates of the value are only made after processing each complete batch of training data
        - The increments specified by the update rule are computed for every time step ***t*** at which a non-terminal state is visited but the value function is ***only changed once, by the sum of all the increments***
- Imagine we have interacted with an environment for 3 episodes
    - So for each updating step we consider all experiences in an episode as one batch and we iterate over each episode repeatedly for each learning step until the algorithm converges
    - Whereas after each learning step one episode is played and added to our memory. So based on our example, the learning step would be:
        - Learn from batch episodes 1,2,3
        - Play episode 4
        - Learn from batch episodes 1,2,3,4
        - Play episode 5
        - Etc…..
- Batch Monte Carlo methods always find the estimates that minimize mean-squared error on the training set, whereas batch TD(0) always finds the estimates that would be exactly correct for the maximum-likelihood model of the Markov process
    - In general, the maximum-likelihood estimate of a parameter is the parameter value whose probability of generating the data is greatest
- **Certainty-Equivalence Method:**
    - Given such a model, we can compute the estimate of the **[value function](https://medium.com/analytics-vidhya/reinforcement-learning-value-function-and-policy-c22f5bd1d1b0?source=friends_link&sk=f4892bf800fe31f670fd01ebc5d712ff)** that would be exactly correct if the model were exactly correct
    - This is called the **certainty-equivalence estimate** because it is equivalent to assuming that the estimate of the underlying process was known with certainty rather than being approximated
        - In general, batch TD(0) converges to the certainty-equivalence estimate
    - ***It is almost never feasible to compute this value directly***
    - On tasks with large state spaces, TD methods may be the only feasible way of approximating the certainty-equivalence solution

# Resources

- [https://medium.com/analytics-vidhya/reinforcement-learning-temporal-difference-learning-part-1-339fef103850](https://medium.com/analytics-vidhya/reinforcement-learning-temporal-difference-learning-part-1-339fef103850)
# What is Generalization in RL?

- For very large state spaces, it may be unfeasible to learn $10^{20}$ state values in a table, we could alternatively learn say, 20 values for the parameters $\theta = \theta_1, \theta_2, ...., \theta_{20}$ that make a $\hat U_\theta$ a good approximation to the true utility function
- Using ***Function Approximation*** makes it practical to represent utility (or Q) functions for very large state spaces, but more importantly, it allows for inductive generalization: the agent can generalize from states it has visited to states it has not yet visited

# Approximating Direct Utility Estimation

- The method of direct utility estimation generates trajectories in the state space and extracts, for each state, the sum of rewards received from that state onward until termination
    - The state and the sum of rewards received constitute a training example for a ***supervised learning algorithm***
- **Example:** Suppose we represent the utilities for the world using a simple linear function, where the features of the squares are just their and coordinates. In that case, we have:

$$
\hat U_{\theta} = \theta_0 + \theta_1 x + \theta_2 y
$$

- Thus, if we have $(\theta_0, \theta_1, \theta_2) = (0.55,0.2,0.1)$ then $\hat U_\theta(1,1) = 0.8$
    - Given a collection trials, we obtain a set of sample values of $\hat U_\theta(x,y)$, and we can find the best fit, in the sense of minimizing the squared error, using standard linear regression
- ***For reinforcement learning, it makes more sense to use an online learning algorithm that updates the parameters after each trial***

### Parameter Learning

- How should parameters be adjusted after each trial?
    - Like neural-network learning, we write an error function and compute it’s gradient with respect to the parameters
    - If $u_j(s)$ is the observed total reward from state $s$ onward in the jth trial, then the error is defined as half the squared distance of the predicted total and the actual total: $E_j(s) = \dfrac {(\hat U_\theta(s) - u_j(s))^2} {2}$
    - We can calculate the rate of change of the error with respect to each parameter $\theta_i$ by computing the partial derivate so as to move the parameter in the direction of decreasing error. To do this, we use the Widrow-Hoff (otherwise known as delta) rule

    $$
    \theta_i \leftarrow \theta_i - \alpha \dfrac {\partial E_j(s)} {\partial \theta_i} = \theta_i + \alpha[u_j(s) - \hat U_\theta(s)]\dfrac {\partial \hat U_\theta(s)} {\partial \theta_i}
    $$

    - Using this linear function approximator $\hat U_\theta(s)$, we get three simple update rules:

    $$
    \theta_0 \leftarrow \theta_0 + \alpha[u_j(s) - \hat U_\theta(s)]

    \newline

    \theta_1 \leftarrow \theta_1 + \alpha[u_j(s) - \hat U_\theta(s)]

    \newline

    \theta_2 \leftarrow \theta_2 + \alpha[u_j(s) - \hat U_\theta(s)]
    $$

    - ***Notice that changing the parameters $\theta_i$ in response to an observed transition between two states also changes the values $\hat U_\theta$ for every other state!***
        - This is what we mean by saying that function approximation allows a reinforcement learner to generalize from its experiences

# Approximating Temporal Difference Learning
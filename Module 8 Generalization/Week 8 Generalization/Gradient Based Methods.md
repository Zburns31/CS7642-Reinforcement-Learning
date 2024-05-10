# Gradient Descent Methods

- SGD methods try to minimize the error on observed examples by adjusting the weight vector after each example by a small amount in the direction that would most reduce the error on that example

### How it works:

- The weight vector is a column vector with a fixed number of real valued components, $w \approx (w_1,w_2,…,w_d)^T$
- The approximate value function $\hat v(s,w)$ is a differential function of $w$ for all $s \in S$
- We update $w$ at each time step so $w_t$ is the weight vector at step $t$
- On each step we observe a new example $S_t \rightarrow v_\pi(s,w)$, assuming for now that we have the true value $v_\pi(s,w)$
- We want to find the best possible $w$, by minimizing error on the observed samples:

    $$
    w_{t+1} \approx w_t + \alpha[v_\pi(S_t) - \hat v(S_t, w_t)] \nabla \hat v(S_t, w_t)
    $$

    - Where:
        - $\alpha$ is a positive step-size parameter
        - $\nabla f(w)$, for any scalar expression $f(w)$ that is a function of a vector (here $w$), denotes the vector of partial derivatives of the expression with respect to the components of the vector:

            $$
            \nabla f(w) = (\frac{\partial f(w)} {\partial w_1}, \frac{\partial f(w)} {\partial w_2}, ..., \frac{\partial f(w)} {\partial w_d})^T
            $$

            - The overall step in $w_t$ is proportional to the negative gradient of the examples squared error
- S**tochastic** gradient descent because the update is done on a single example which might have been selected stochastically
    - Over many examples we make many small steps and the overall effect is to minimize an average performance measure ($\overline{VE}$ for example)
- Convergence to a local optimum is guaranteed depending on $\alpha$

### True Value Estimates

- What about when we don’t know the true value $v_\pi(S_t)$ but only have an approximation $U_t$? Just plug it in place of $v_\pi(S_t)$ in the update:

    $$
    w_{t+1} \approx w_t + \alpha[U_t - \hat v(S_t, w_t)] \nabla \hat v(S_t, w_t)
    $$


# Semi-Gradient (Bootstrapping) Methods

- Although semi-gradient (bootstrapping) methods do not converge as robustly as gradient methods, they do converge reliably in important cases such as the linear TD(0) case
    - They typically offer faster learning as they enable learning to be continual and online, without waiting for the end of an episode
    - This allows them to be used in continuing problems and provides computational advantages
- ***Unbiased estimates allow for convergence to a local optimum***
    - Gradient Monte Carlo: $U_t = G_t$
- ***However, guarantees of convergence are not guaranteed if using bootstrapped estimates as the target $U_t$***
    - Dynamic Programming: $U_t = \sum_{a,s',r}\pi(a|S_t)p(s'|s,a)[r + \gamma \hat v(s', w_t)$
    - N-Step: $U_t = G_{t:t+n}$
    - Both of these depend on the current value of the weight vector $w_t$, which implies that they will be biased and that they will not produce a true gradient-descent method
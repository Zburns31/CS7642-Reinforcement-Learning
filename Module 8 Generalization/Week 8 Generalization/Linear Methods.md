# Linear Methods

- One of the simplest cases for function approximation is when the approximate function $*\hat v(\sdot , w)*$ is a linear combination of the weight vector $w$
    - In this setting, every state is represented as a vector $x(s)$ the same length as $w$ and the approximated state-value is just the inner product between $w$ and $x(s)$:

        $$
        \hat v(s,w) \approx w^T x(s) = \sum_{i=1}^d w_ix_i(s)
        $$

        - Where:
            - $x(s)$ represents the feature (observation) vector representing state $s$
- It is natural to use SGD updates with linear function approximation
    - The gradient of the approximate value function with respect to $w$ in this case is:

        $$
        \nabla \hat v(s, w) = x(s)
        $$

- Thus, in the linear case the general SGD update reduces to a simple form:

$$
w_{t+1} \approx w_t + \alpha [U_t - \hat v(S_t, w_t)]x(S_t)
$$

# Feature Construction for Linear Methods

- Sutton & Barto 9.5
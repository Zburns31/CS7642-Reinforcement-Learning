# Summary

- Focus of the [paper](http://incompleteideas.net/papers/sutton-88-with-erratum.pdf) is on incremental learning procedures and multi-step prediction techniques
- The paper implements True online TD($\lambda)$ or otherwise known as Linear Supervised TD Learning
    - This method uses value function approximation rather than tabular methods
- Whereas conventional predicting-learning methods assign credit by means of the difference between predicted and actual outcomes, the new methods assign credit by means of the difference between temporally successive predictions
- For most real-world problems, temporal-difference methods require less memory and less peak computation than conventional methods and they produce more accurate predictions
- Review usage of Eligibility Traces
- [Eligibility Traces & Lambda-Return](../Week%203%20TD%20&%20Friends/Eligibility%20Traces%20&%20Lambda-Return.md)


# Widrow-Hoff (Delta) Rule

- All learning procedures will be expressed as rules for updating $w$
    - We assume that $w$ is only updated once for each complete observation-outcome sequence and thus does not change during a sequence
    - For each observation, an increment to $w$, denoted by $\Delta w_t$, is determined
    - After a complete sequence has been processed, $w$ is changed by (the sum of) all the sequences increments (2):

    $$
    w \leftarrow w + \sum_{t=1}^m \Delta w_t
    $$

- Also known as **Delta Rule (2)**, it follows gradient descent rule for linear regression:

    $$
    \Delta w_t = \alpha (z - w^Tx_t) ==  \Delta w_t = \alpha(z-P_t)\nabla_wP_t
    $$

- Where:
    - $\alpha$ is a positive parameter affecting the rate of learning
    - $\nabla_t P_t$ represents the gradient (I.e. the partial derivatives of $P_t$ with respect to each component $w$
- **Basic Idea:** The difference $z-w^Tx_t$ represents the scalar error between the prediction, $w^Tx_t$, and what it should have been $z$
    - This is multiplied by the observation vector $x_t$ to determine the weight changes because $x_t$ indicates how changing each weight will affect the error

# Generalized Delta Rule - Backpropagation

- Backpropagation cannot be computed incrementally since the weight changes depend critically on $z$, and thus cannot be determined until the end of a sequence
    - Thus, all observations and predictions made during a sequence must be remembered until its end, when all the $\Delta w_t$’s are computed
- ***There is, however, a TD procedure that produces exactly the same result as the Delta rule which can be computed incrementally***
    - The key is to represent the error $z - P_t$ as a sum of changes in predictions, that is, as:

    $$
    z - P_t = \sum_{k=t}^m (P_{k+1} - P_k)
    $$

- Combining equations 1 and 2 from above, we find that:

$$
\Delta w_t = \alpha(P_{t+1} - P_t) \sum_{k=1}^t \nabla_w P_k
$$

- Unlike equation 2, this equation (3) can be computed incrementally, because each $\Delta w_t$ depends only on a pair of successive predictions and on the sum of all past values for $\nabla_w P_t$
- ***Note that $\nabla_w P_t$ can reduced to $x(s)$ for the linear function approximation case***


# TD($\lambda$) Family of Learning Procedures

- The hallmark of temporal-difference methods is their sensitivity to changes in successive predictions rather than to overall error between predictions and the final outcome
- First, we consider a class of TD learning procedures that make greater alterations to more recent predictions
    - We look at a learning rule which uses exponential weighting with recency, in which alterations to the predictions of observation vectors occurring k steps in the past are weighted according to $\lambda^k$ for $0 \le \lambda \le 1$:

        ### TD($\lambda$)

        $$
        \Delta w_t = \alpha(P_{t+1} - P_t) \sum_{k=1}^t \lambda^{t-k} \nabla_wP_k
        $$

        - Note that when $\lambda$ = 1 this is equivalent to equation 3
    - Alterations of past predictions can be weighted in ways other than the exponential form given above, and this may be appropriate for particular applications
        - An important advantage of the exponential form is that it can be computed incrementally

## Better Learning with TD Methods

- Since TD methods use evaluate the state of an action rather than using the actual outcome, TD methods learning a more accurate assessment of that state than supervised learning methods
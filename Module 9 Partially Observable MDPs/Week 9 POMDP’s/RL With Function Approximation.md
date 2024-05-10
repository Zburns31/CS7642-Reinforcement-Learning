# Summary

- Many of the RL algorithms we have covered so far are guaranteed to converge (assuming infinite visits to every state-action pair) to the optimal solution when used with lookup tables
    - However, these same algorithms can be come very unstable when mixed with function approximation
- A new class of algorithms, ***residual gradient algorithms***, is proposed, which perform gradient descent on the mean squared Bellman residual, guaranteeing convergence
    - However, these algorithms can learn very slowly in some cases
- A larger class of algorithms, ***residual algorithms***, is proposed that has the guaranteed convergence of the residual gradient algorithms, yet can retain the fast learning speed of direct algorithms
    - In fact, both direct and residual gradient algorithms are shown to be special cases of residual algorithms, and it is shown that residual algorithms can combine the advantages of each approach

# Direct Algorithms

- Direct RL algorithms represent the original implementation combined with function approximation
    - The direct algorithm reduces to the original algorithm when used with a lookup table

# Residual Gradient Algorithms

- The ***Bellman Residual*** is defined to be the difference between the two sides of the Bellman equation. The ***mean squared bellman residual*** for an MDP with n states is defined as:

    $$
    E = \dfrac {1}{n} \sum_x[\langle R + \gamma V(x') \rangle - V(x)]^2
    $$

- If the Bellman residual is nonzero, then the resulting policy will be suboptimal, but for a given level of Bellman residual, the degree to which the policy yields suboptimal reinforcement can be bounded
    - This suggests it might be reasonable to change the weights in the function-approximation system by performing gradient descent on the mean squared Bellman residual $E$
        - This could also be called the ***residual gradient algorithm***
- For a deterministic MDP, after a transition to state $x$ to a state $x'$, with reward $R$, a weight $w$ would change according to:

$$
\Delta w = -\alpha[R + \gamma V(x') - V(x)][\frac{\partial} {\partial w} \gamma V(x') - \frac{\partial} {\partial w} V(x)]
$$

- For a system with a finite number of states, E is zero if and only if the value function is optimal
    - Therefore, performing gradient descent on E guarantees that E will eventually converge to a local minimum
- Although residual gradient algorithms have guaranteed convergence, that does not necessarily mean that they will always learn as quickly as direct algorithms
    - Thus the residual gradient algorithm causes information to flow both ways, with information flowing in the wrong direction moving slower than information flowing in the right direction by a factor of $\gamma$
    - If $\gamma$ is close to 1.0, then it would be expected that residual gradient algorithms would learn very slowly

# Residual Algorithms

- Direct algorithms can be fast but unstable, and residual gradient algorithms can be stable but slow
    - Direct algorithms attempt to make each state match its successors, but ignore the effects of generalization during learning
    - Residual gradient algorithms take into account the effects of generalization, but attempt to make each state match both its successors and its predecessors

# Resources

- Good Explanation: [https://www.cs.cmu.edu/afs/cs.cmu.edu/project/learn-43/lib/photoz/.g/web/glossary/residual.html](https://www.cs.cmu.edu/afs/cs.cmu.edu/project/learn-43/lib/photoz/.g/web/glossary/residual.html)
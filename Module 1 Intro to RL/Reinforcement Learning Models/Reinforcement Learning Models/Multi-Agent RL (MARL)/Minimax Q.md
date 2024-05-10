# Zero-Sum Stochastic Games

- The idea is that the actions that the players take not only impact the rewards but also future states
    - The way we can deal with this issue is by defining a value function

$$
Q^*_i(s, (a,b)) = R_i(s,(a,b)) + \gamma \sum_{s'}T(s,(a,b),s') \max_{a',b'} Q^*_i(s', (a',b'))
$$

- Where:
    - $Q^*_i$ is defined over joint state action pairs
    - The immediate reward to player $i$ for that joint action in that state
    - Discounted expected value for that next state
        - Look at the $Q$ values in the next state and summarize them
- There is an issue with the $max$ operator in this equation
    - This equation assumes that the joint actions that are gonna be taken will benefit you the most
        - This will lead to delusional optimism
- To solve this issue, we can adjust the equation to:

$$
Q^*_i(s, (a,b)) = R_i(s,(a,b)) + \gamma \sum_{s'}T(s,(a,b),s') \space minimax_{a',b'} Q^*_i(s', (a',b'))
$$

- Where:
    - When we evaluate the value of a state, we actually solve the zero-sum game in the $Q$ values


# Minimax Q

- The minimax-Q algorithm uses the minimax principle to solve for players' Nash equilibrium strategies and values of states for two-player zero-sum stochastic games
- Similar to Q-learning, minimax-Q algorithm is a temporal-difference learning method that performs backpropagation on values of states or state-action pairs
- Q-Learning update:

    $\langle S, (a,b), (r_1, r_2), s' \rangle: \space Q_i(s,(a,b)) \leftarrow^\alpha r_i + \gamma \space minimax_{a',b'} \space Q_i(s', (a', b'))$

    - Where:
        - We are in some state $S$, there’s some joint action is taken, some pair of rewards and next state that is visited
        - The update moves us closer to the reward for taking that joint action + the discounted summarized value of the new states $s'$
            - We use minimax to summarize what those values are

# Properties

- Value iteration works
- Minimax-Q converges under the same conditions as normal Q-learning
- Unique solution to $Q^*$
- Policies can be computed independently
- Update efficient
- Q functions sufficient to specify the policy

# Resources

- [https://learning.oreilly.com/library/view/multi-agent-machine-learning/9781118362082/9781118884485c04.xhtml](https://learning.oreilly.com/library/view/multi-agent-machine-learning/9781118362082/9781118884485c04.xhtml)
- [https://www.cs.ubc.ca/~kevinlb/teaching/cs532l - 2013-14/Lectures/rl-pres.pdf](https://www.cs.ubc.ca/~kevinlb/teaching/cs532l%20-%202013-14/Lectures/rl-pres.pdf)
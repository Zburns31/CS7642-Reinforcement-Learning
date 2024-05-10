# Readings

- Sutton, Precup, Singh (1999)
- Jong, Hester, Sonte (2008)

# What is it?

- Game theory is a mathematical and strategic approach used to study decision-making in situations where the outcome of one person's decision depends on the decisions of others
    - It is used to understand and analyze the behaviour of individuals or groups in strategic situations, where the outcome of each player's decision depends on the decisions of others
- The basic game for understanding game theory is “2-player zero-sum finite deterministic game of perfect information”
    - The further insight can be obtained by modifying various parameters like non-deterministic, hidden information, non-zero sum
- In Game theory w.r.t RL, the policy is the strategy, mapping all possible state of actions, in respect to one of the players of the game

# Types of Games

1. Cooperative and Non-Cooperative
2. Symmetric and Asymmetric
3. Simultaneous and Sequential
4. Zero-Sum and Non-Zero-Sum
5. Perfect Information and Imperfect Information

# Types of Strategies

1. **Dominant Strategies**
    1. Dominant strategy is a strategy which is optimal regardless of what other players do. It means you perform an action without caring about your competitors action or you simply don’t care about what other players do, you are only concerned with maximizing your own payoff
2. **Pure Strategies**
    1. A pure strategy **provides a complete definition of how a player will play a game**
        1. Pure strategy can be thought about as a plan subject to the observations they make during the course of the game of play. In particular, it determines the move a player will make for any situation they could face
    2. A pure strategy is a strategy in which a player always chooses the same action in a given situation, regardless of the actions chosen by the other players
3. **Mixed Strategies**
    1. Players randomize over the set of available actions according to some probability distribution - a player randomizes and mixes between different actions
    2. A mixed strategy implies some distribution over all strategies
    3. In the mixed poker example:
        1. 50% of the time we are a holder and the rest being a resigner
        2. A pure strategy would only do either or

# Fundamental Result

- If a 2 player, zero-sum deterministic game of perfect information:
    - Minimax $\equiv$ Maximin and there always exists an optimal pure strategy for each player
        - All players are doing what’s optimal for them and assuming their opponent is doing the same
- This fails when a game becomes hidden information

# Nash Equilibrium

- A Nash Equilibrium is a set of choices in which no player has anything to gain by changing only their own choice
    - In other words, a Nash equilibrium is a set of strategies that represents a stable state in which no player has an incentive to change their strategy, given that all other players are playing their equilibrium strategies
        - In a Nash equilibrium, each player is playing their best response to the strategies of the other players, and no player can improve their payoff by deviating from their equilibrium strategy
- Consider n players in a game with strategies $s_1, s_2, s_3, ... s_n$
    - Then the strategies are said to be in NE, if and only if, strategies of all the players corresponds to the optimal strategy for that player
    - The 3 fundamental theorem for NE are:
        1. In the n-player pure strategy game, if elimination of strictly dominated strategies, eliminates all but one combination, then it is the unique NE
        2. Any NE will survive elimination of strictly dominated strategies
        3. If n is finite and s(i) is finite, there exist at least one NE
- **Pure NE Vs. Mixed ME**
    - A pure Nash equilibrium is a solution in which each player chooses a single strategy, and no player has an incentive to switch to a different strategy, given the strategies chosen by the other players
    - A mixed Nash equilibrium, on the other hand, is a solution in which each player randomizes between two or more strategies, and no player has an incentive to switch to a different mixture of strategies, given the mixture of strategies chosen by the other players
    - In other words, in a pure Nash equilibrium, each player is using a single strategy with certainty, while in a mixed Nash equilibrium, each player is using a probabilistic mixture of strategies

# 3 NE Theorems

- In the N player pure strategy game, if elimination of strictly dominated strategies eliminates all but one combination, that combination is the NE
- Any NE will survive the elimination of strictly dominated strategies
- If n is finite and for all strategies (which is finite), there exists a NE (possibly mixed)

# Framework for Temporal Abstraction in RL

TODO

# Utility of Temporal Abstraction in RL

TODO

- The types of games in Multi-Agent RL (MARL) are:
    - Static Games: players are independent and make simultaneous decision
    - Stage Games: the rules depend on specific stages
    - Repeated Games : when a game is played in sequence

# Key Terms & Concepts

- **Domination:**
    - **Strictly dominated strategy:** This is a strategy that always delivers a worse outcome than an
    alternative strategy, regardless of what strategy the opponent chooses
    - **Weakly dominated strategy**: This is a strategy that delivers an equal or worse outcome than an alternative strategy

# Resources

- [https://www-edlab.cs.umass.edu/cs240/lectures/lecture22.pdf](https://www-edlab.cs.umass.edu/cs240/lectures/lecture22.pdf)
- [https://medium.com/analytics-vidhya/reinforcement-learning-and-game-theory-28208ee795f8](https://medium.com/analytics-vidhya/reinforcement-learning-and-game-theory-28208ee795f8)
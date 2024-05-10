# Readings

- Littman (1994)
- Littman and Stone (2003)

# Iterated Prisoners Dilemma (IPD)

- We already discussed that in one game, the only rational thing to do is to defect. Even in a finite and known number of games, the ultimate choice would still be defecting
    - The question is, what happens if the number of rounds left unknown?
        - The Uncertain End:
            - The probability that the game continues is 𝛾
            - Each round could be the last one, or not
            - The expected number of rounds is $\dfrac {1} {1-\gamma}$
- The iterated [prisoner's dilemma](https://www.investopedia.com/terms/p/prisoners-dilemma.asp) is an extension of the general form except the game is repeatedly played by the same participants
    - An iterated prisoner's dilemma differs from the original concept of a prisoner's dilemma because participants can learn about the [behavioral tendencies](https://www.investopedia.com/terms/b/behavioraleconomics.asp) of their counterparty

# IPD Strategies

## Tit-for-Tat

- **Basic Idea:** Cooperates on the first round and imitates its opponent's previous move thereafter
- Example on how gamma impacts the behaviours of both players:

    ![Untitled](./Week%2013%20Game%20Theory%20Reloaded/Untitled.png)

- Always defect is best when gamma is low (when the game won’t last many rounds)
- Always cooperate is better when the game will last for many rounds (high gamma)

# Best Response to A Finite-State Strategy

- If we were in a one-round game, the matrix would be all what you need to select the best strategy
    - On the other hand, in a multi-round setting, our choice not only impacts our pay off, but also the future decisions of the opponent
    - TFT is a finite state strategy for example
- So, in a multi-round scenario, we need a state machine representation to make sense of the game
    - This state machine actually represents a deterministic MDP where we map from the opponent’s state to an action for us
- [Video](https://edstem.org/us/courses/33453/lessons/52870/slides/302307)

# Repeated Games & The Folk Theorem

- **Basic Idea:** In a repeated game setting, the possibility of retaliation (defecting) opens the door for cooperation

## Folk Theorem (Threats)

- In Game Theory, the Folk Theorem describes the set of payoffs that can result from Nash strategies in repeated games
- What is a Folk Theorem? In game theory, it refers to a particular result
    - It describes the set of payoffs that can result from Nash Strategies in repeated games
- **Folk Theorem:** Any feasible payoff profile that strictly dominates the minmax/security level profile can be realized as a nash equilbrium payoff profile, with a sufficiently large discount factor
    - ***In simpler terms, the folk theorem suggests that in a repeated game, players can cooperate and achieve a good outcome, even if that outcome would not be possible in a one-shot game***
        - This is because in a repeated game, players can build a reputation and learn to cooperate with each other over time, leading to better outcomes for everyone
    - The folk theorem is an important result in game theory, as it suggests that cooperation and trust can be established even in situations where self-interest would suggest otherwise

## Minmax Profile (Pure Strategy)

- A pair of payoffs, one for each player, that represent the payoffs that can be achieved by a player defending itself from a malicious adversary (some other player trying to lower my score)
    - Malicious Adversary $\rightarrow$  Zero-Sum Games
- In game theory, the minmax profile is a strategy profile that maximizes a player's minimum possible payoff
    - In other words, it is the best strategy for a player when they assume that their opponent will try to minimize their payoff

## Security Level Profile (Mixed Strategies)

- Minmax of PD is (D, D) because the malicious adversary will defect, and your best option is to defect as well
    - The region above and to the right of the minmax point is the (preferable) acceptable region, it is better than what you can guarantee against someone acting maliciously

        ![Untitled](./Week%2013%20Game%20Theory%20Reloaded/Untitled%201.png)

- In game theory, the security level profile is a way to analyze and compare the risks associated with different strategies in a game
    - ***Specifically, the security level of a strategy is the minimum amount that a player can expect to receive, regardless of what the other players do***
- To calculate the security level of a strategy, we first find the minimum payoff that the player can receive, assuming that the other players are playing their worst possible strategies
    - This is called the worst-case scenario. Then, we take the maximum value of the worst-case scenario over all possible strategies that the other players can play
    - This maximum value is the security level of the player's strategy

## Grim Trigger

- Cooperation keeps you in the state of mutual benefit, but if defected once, you enter the state of dealing out of vengeance and stay in that state forever. This creates a Nash Equilibrium
- The only problem with this concept is that the threat is implausible, because it doesn’t make sense that a player will stop taking the best response (in his interest) only to punish the other player. So, in this setting, we end up with a setting that is not Subgame Perfect
- **Subgame Perfect Equilibrium:** A subgame perfect equilibrium is a set of strategies (one for each of the players) with the property that the strategies constitute a Nash equilibrium for every subgame of the original game
    - ***Each player is always taking the best response independent of the history***
- In game theory, the grim trigger is a strategy used in repeated games to enforce cooperation between players. The strategy involves a player initially cooperating with their opponent, but if their opponent defects (i.e., fails to cooperate), the player will defect in all subsequent rounds of the game
    - The idea behind the grim trigger strategy is that it provides a strong incentive for players to cooperate, as defecting even once can trigger a long-term punishment
    - By threatening to defect forever, a player can deter their opponent from defecting in the first place, leading to a cooperative outcome in the long run

## Subgame Perfect

- In game theory, a subgame perfect equilibrium is a solution concept used in sequential games, where players take turns making decisions
    - A subgame perfect equilibrium is a set of strategies, one for each player, that specifies what each player should do at every point in the game to maximize their payoff, assuming that all players are rational and are using the same reasoning
- To be considered a subgame perfect equilibrium, a strategy profile must satisfy two conditions:
    1. The strategies must be optimal at every subgame of the game, which is a smaller game that starts at any point in the original game
    2. The strategies must be consistent with the players' beliefs about what the other players will do at every point in the game
- ***In other words, a subgame perfect equilibrium is a set of strategies that is optimal not only in the original game but also in every subgame that might be reached by any sequence of moves***

## Pavlov

- In Pavlov, the player cooperates if the both players agreed on the last round. Will defect otherwise (disagree)
- Pavlov vs Pavlov is a Nash Equilibrium
    - Both players start with cooperation. Because of this it makes no sense to move away from cooperation
    - Pavlov vs Pavlov is Subgame Perfect. It will always end up cooperating
- Pavlov creates plausible threats
- In other words, Pavlov:
    - cooperates when you cooperate with it, except by mistake
    - “pushes boundaries” and keeps defecting when you cooperate, until you retaliate
    - “concedes when punished” and cooperates after a defect/defect result
    - “retaliates against unprovoked aggression”, defecting if you defect on it while it cooperates

## Computational Folk Theorem

- Given a 2-player bi-matrix game (each player has a separate reward structure), you can build Pavlov-like machines for any game. And using Pavlov you can construct Subgame Perfect Nash Equilibrium for any game in polynomial time

# Stochastic (Markov) Games & Multiagent RL

- Stochastic Games is a formal model for multiagent RL (like MDP to RL)

- The Game:
    - 3 × 3 grid.
    - Two players: A and B
    - Possible directions: N, S, E, W, X
        - Transitions are deterministic except for the semi walls (dashed wall lines)
        - Semi-wall (Dashed Line): 50% it goes through, and 50% it remains in the same cell
    - If both A and B arrived, they both win
    - First to reach the goal gets $100

![Untitled](./Week%2013%20Game%20Theory%20Reloaded/Untitled%202.png)

- [Stochastic Games](./Week%2013%20Game%20Theory%20Reloaded/Stochastic%20Games.md)

# Resources

- [https://plato.stanford.edu/entries/prisoner-dilemma/strategy-table.html](https://plato.stanford.edu/entries/prisoner-dilemma/strategy-table.html)
- [https://www.cs.ubc.ca/~kevinlb/teaching/cs532l - 2008-9/lectures/lect9.pdf](https://www.cs.ubc.ca/~kevinlb/teaching/cs532l%20-%202008-9/lectures/lect9.pdf)
- [https://raw.githubusercontent.com/mohamedameen93/CS-7642-Reinforcement-Learning-Notes/master/11. Game Theory.pdf](https://raw.githubusercontent.com/mohamedameen93/CS-7642-Reinforcement-Learning-Notes/master/11.%20Game%20Theory.pdf)
- [https://www.lesswrong.com/posts/3rxMBRCYEmHCNDLhu/the-pavlov-strategy](https://www.lesswrong.com/posts/3rxMBRCYEmHCNDLhu/the-pavlov-strategy)
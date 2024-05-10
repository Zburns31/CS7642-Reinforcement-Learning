# Readings

- [Greenwald and Hall (2003)](https://courses.cs.duke.edu/spring07/cps296.3/correlated_q.pdf)
- Munoz de Cote and Littman (2008)

# Solution Concepts

- In game theory, a solution concept is **a formal rule for predicting how a game will be played**
    - These predictions are called "solutions", and describe which strategies will be adopted by players and, therefore, the result of the game
    - Examples: Nash Equilibrium, Subgame Perfect, Correlated Equilibrium


# Correlated Equilibrium

- ***The idea is that each player chooses their action according to their observation of the value of the same public signal***
    - A strategy assigns an action to every possible observation a player can make
    - If no player would want to deviate from the recommended strategy (assuming the others don’t deviate), the distribution is called a **correlated equilibrium**
    - Notes:
        - It’s an equilibrium because no player wants to change strategies
        - It’s correlated because we have some information where we can associate what we have with what the other player likely has
- [Good Example](https://en.wikipedia.org/wiki/Correlated_equilibrium)
- In game theory, a correlated equilibrium is a solution concept used in non-cooperative games where players do not necessarily have common knowledge about the game being played. In a correlated equilibrium, players use a shared randomizing device, such as a coin flip, to choose their strategy
    - A correlated equilibrium consists of two components: a correlation device and a set of strategies. The correlation device is a probability distribution over the set of possible joint actions. Each player has a conditional probability distribution over their own possible actions, given the random outcome of the correlation device. The strategies specify what action each player should take based on the random outcome.
    - To be considered a correlated equilibrium, the following two conditions must be satisfied:
        1. No player has an incentive to deviate from their strategy, given the outcome of the correlation device and the strategies of the other players
        2. The correlation device is such that the players' expected payoffs are at least as high as they would be under any other correlated equilibrium

## Correlated Facts (CE)

- CE can be found in polynomial time
- All mixed Nash are correlated, so CE exist
- All convex combinations of mixed Nash are correlated

# Coco Values

- Coco (“cooperative competitive”) values are a solution concept for two-player normal-form games with transferable utility, when binding agreements and side payments between players are possible
    - Coco gets its name from the way it is calculated: by decomposing the game into a cooperative (or team) game and a competitive (or zero-sum) game, and then combining the solutions to these games
- Game Example
    - Consider a scenario where you and a short, but strong, friend are picking bananas. Your friend cannot reach any bananas, and you can only reach two. If you try to climb on your friend to reach more bananas, you fail
    - However, if your friend is willing to give you a boost, you can climb and get two high bananas, as well as the two low ones
    - If utility is not transferable, your friend has no incentive to help you pick bananas. But, if it is, then offering her some bananas might encourage her to give you a boost
    - How many of the four bananas you pick should you offer her? One bite? All four? Coco values offer a focal point—you should offer her one banana for her efforts
    - **Solution**
        - **Use side payments. Why don’t the players split the benefit derived from cooperating**
- [Paper](https://proceedings.mlr.press/v28/sodomka13.pdf)

## Coco Definition

- The coco value of a game can be defined by:
    - $U$: Payoffs to you
    - $\bar U$: Payoffs to other

$$
Coco(U, \bar U) = \underbrace {maxmax(\dfrac {U + \bar U}{2})}_{cooperative} + \underbrace {minimax(\dfrac {U - \bar U}{2})}_{competitive}
$$

- Formula Notes:
    - Cooperative represents the average value that both players would get for the possible joint actions
    - Competitive represents the difference between the utilities that both players get and split the difference

## Side Payments

- $U$’s Side payments: $P = Coco(U, \bar U) - U(q^*, q^*)$
    - $Coco(U, \bar U)$ represents the coco value from the perspective of $U$
- $\bar U$’s Side Payments: $P = Coco(\bar U, U) - \bar U(q^*, q^*)$

## Properties

- Efficiently computable
- Utility maximizing
- Decompose game into sum of two games
- Unique
- Can be extended to stochastic games (Coco-Q, not non-expansion)
- Not necessarily equilibrium (best response)
    - Needs to be binding
- Doesn’t generalize for games with more than 2 players

# Mechanism Design

- Mechanism design is a field in economics and game theory that takes an objectives-first approach to designing economic mechanisms or incentives, toward desired objectives, in strategic settings, where players act rationally
- Mechanism Design can be thought of as a form of algorithm design, but where the entities supplying the inputs each have a stake in the outcome
    - As a result, the procedures produced should ideally be incentive-compatible, meaning that it is in each agent’s best interest to report truthfully or to otherwise act in a well-behaved manner, and agents cannot gain an advantage by misrepresenting their values or “gaming” the system
    - Many of the problems facing the internet, for example, such as inappropriate hogging of bandwidth, email spam, and web spam, can often best be viewed as problems in the mechanism: the result of imperfections in the incentive structure of the internet
    - [Link](https://www.cs.cmu.edu/~mblum/search/AGTML35.pdf)

## Peer Teaching

- How do we craft policies between two agents to achieve some particular goal?
    - Striking a balance between giving the student the right level of difficulty when answering questions
    - How do we create an incentive plan to achieve learning?
# What is it?

- MCTS is an algorithm that figures out the best move out of a set of moves by Selecting → Expanding → Simulating → Updating the nodes in tree to find the final solution
    - This method is repeated until it reaches the solution and learns the policy of the game
- Go has a branching factor of approximately 300 i.e. from each state there are around 300 actions possible, whereas chess typically has around 30 actions to choose from
    - Further, the positional nature of Go, which is all about surrounding the adversary, makes it very hard to correctly estimate the value of a given board state
- **The basic MCTS strategy does not use a heuristic evaluation function**
    - Instead, the value of a state is estimated as the average utility over a number of simulations of complete games starting from the state


# How does it work?

- The basic MCTS algorithm is simple: a search tree is built, node-by-node, according to the outcomes of simulated playouts. The process can be broken down into the following steps:
    1. **Selection - Follow the Policy $\pi$**
        1. The algorithm traverses the tree from the root to a leaf node, selecting each node based on a trade-off between the expected reward and the uncertainty of the outcome
        2. ***In the RL context, we use the tree policy to construct a path from root to most promising leaf node***
            1. A leaf node is a node which has unexplored child node(s). I.e. The policy doesn’t tell us what to do here
            2. Traverse the tree until the policy no longer tells us what action we should take
    2. **Expansion**
        1. Expand nodes by sampling actions we may take and observe the next states we could end up in
            1. For each action we might take, take this action and simulate for one step what might happen
            2. Repeat this a few times
    3. **Simulation (Rollout Policy)**
        1. Run a simulated play out from *C* until a result is achieved
        2. The algorithm plays out a random sequence of actions from the selected node to the end of the game, using the value function to estimate the reward at each step
            1. By accumulating rewards during the simulation, we can estimate a value for node C (from step 2) and taking the given action
    4. **Back-Propagation**
        1. Update all the search tree nodes with the simulation result

        ![Untitled](./Monte%20Carlo%20Tree%20Search/Untitled.png)


## Algorithm Notes

- We repeat this process either for a **set number of iterations, or until the allocated time has expired**
- **When the iterations terminate, the move with the highest number of play outs is returned**. You might think that it would be better to return the node with the highest average utility, but the idea is that a node with 65/100 wins is better than 2/3 wins, because the latter has a lot of uncertainty
    - The UCB1 formula ensures that the node with the most playouts is almost always the node with the highest win percentage, because the selection process favours win percentage more and more as the number of playouts goes up
- ***Each node must contain two important pieces of information:***
    1. An estimated value based on simulation results and,
    2. The number of times it has been visited
    - In its simplest and most memory efficient implementation, MCTS will add one child node per iteration
        - Note, however, that it may be beneficial to add more than one child node per iteration depending on the application
- Requires a transition model in order to do the rollouts or have some way of doing the simulation and sample from it

### **What is a simulation and how does it work?**

- A simulation (also called a playout or rollout) chooses moves first for one player, than for the other, repeating until a terminal position is reached
    - At that point the rules of the game (not fallible heuristics) determine who has won or lost, and by what score
    - For games in which the only outcomes are a win or a loss, “average utility” is the same as “win percentage”


### **How do we choose what moves to make during the playout?**

- To get useful information from the playout we need a ***playout policy*** that biases the moves towards good ones
    - These can be learned either from ML or DL algorithms or using game specific heuristics


### Playout Considerations:

1. What positions do we start the playouts?
    1. Easiest answer is for **Pure Monte Carlo Search**, which is to do N simulations starting from the current state of the game, and track which of the possible moves from the current position have the highest win percentage
        1. For stochastic games, this converges to optimal play as N increases
        2. **Generally, we need a selection policy** that selectively focuses on the computational resources on the important parts of the game tree. It balances two factors:
            1. **Exploration** of states that have had few playouts
            2. **Exploitation** of states that have done well in past playouts
2. How many playouts do we allocated to each position?

## MCTS Properties

- Useful for large state spaces
- Need lots of samples to get a good estimate
- Planning time independent of $|S|$
- Running time is exponential in the horizon

# MCTS Algorithm Step By Step Breakdown

# Selection Policies

- **Upper Confidence Bounds Applied to Trees (UCT)**
    - This is one very effective selection policy which ranks each possible move based on an upper confidence bound formula called UCB1
    - For a node *n*, the formula is:

    $$
    UCB1(n) = \dfrac {U(n)} {N(n)} + C * \sqrt{\dfrac{log N(Parent(n))} {N(n)}}
    $$

- Where:
    - U(n) is the total utility of all playouts that went through node *n*
    - N(n) is the number of playouts through node *n*
    - Parent(n) is the parent node of *n* in the tree
    - C is a constant that balances exploration and exploitation
        - Theoretically, there are arguments it should be $\sqrt{2}$
    - Thus: $\dfrac {U(n)} {N(n)}$ represents the exploitation term: the average utility of *n*
    - The term with the square root is the exploration term: it has the count N(n) in the denominator, ***which means the term will be high for nodes that have only been explored a few times***
        - In the numerator it has the log of the number of times we have explored the parent of
            - This means that if we are selecting some non-zero percentage of the time, **the exploration term goes to zero as the counts increase, and eventually the playouts are given to the node with highest average utility**

# Expansion

# Playout Policies

- Playout policies can be learned from ML or DL methods
- Rather than following a random policy, we can mix in constraints to give us a better policy
    - Options could also be introduced which allows us to go deeper in the tree

# Backpropagation

# Pseudocode

```python
def monte_carlo_tree_search(root):

    while resources_left(time, computational power):
        leaf = traverse(root)
        simulation_result = rollout(leaf)
        backpropagate(leaf, simulation_result)

    return best_child(root)

# function for node traversal
def traverse(node):
    while fully_expanded(node):
        node = best_uct(node)

    # in case no children are present / node is terminal
    return pick_unvisited(node.children) or node

# function for the result of the simulation
def rollout(node):
    while non_terminal(node):
        node = rollout_policy(node)
    return result(node)

# function for randomly selecting a child node
def rollout_policy(node):
    return pick_random(node.children)

# function for backpropagation
def backpropagate(node, result):
    if is_root(node) return
    node.stats = update_stats(node, result)
    backpropagate(node.parent)

# function for selecting the best child
# node with highest number of visits
def best_child(node):
    pick child with highest number of visits
```

# Implementation Details

- It is possible to combine MCTS and evaluation functions by doing a playout for a certain number of moves, but then truncating the playout and applying an evaluation function
- It is also possible to combine aspects of alpha–beta and Monte Carlo search
    - For example, in games that can last many moves, we may want to use early playout termination, in which we stop a playout that is taking too many moves, and either evaluate it with a heuristic evaluation function or just declare it a draw

# Space & Time Complexity

- The time to compute a playout is linear, not exponential, in the depth of the game tree, because only one move is taken at each choice point. That gives us plenty of time for multiple playouts
    - For example: consider a game with a branching factor of 32, where the average game lasts 100 ply. If we have enough computing power to consider a billion game states before we have to make a move, then minimax can search 6 ply deep, alpha–beta with perfect move ordering can search 12 ply, and Monte Carlo search can do 10 million playouts
- Running time is exponential in the horizon meaning that it doesn’t matter how states we have, but it does matter how far in the future we need to look
    - $O(|A| \cdot x)^H$
        - Where:
            - $|A|$ is the action space size
            - $x$ is the number of times we are running the algorithm for
            - $H$ is the horizon which is dependent upon factors like the discount factor

# **Advantages of Monte Carlo Tree Search**

1. Useful for large state spaces
    1. Planning time is independent of $|S|$
2. MCTS is a simple algorithm to implement
3. Monte Carlo Tree Search is a heuristic algorithm. MCTS can operate effectively without any knowledge in the particular domain, apart from the rules and end conditions, and can find its own moves and learn from them by playing random playouts
4. The MCTS can be saved in any intermediate state and that state can be used in future use cases whenever required
5. MCTS supports asymmetric expansion of the search tree based on the circumstances in which it is operating

# **Disadvantages of Monte Carlo Tree Search**

1. As the tree growth becomes rapid after a few iterations, it requires a huge amount of memory
2. Needs lots of samples to get an accurate estimate
3. There is a bit of a reliability issue with Monte Carlo Tree Search. In certain scenarios, there might be a single branch or path, that might lead to loss against the opposition when implemented for those turn-based games. This is mainly due to the vast amount of combinations and each of the nodes might not be visited enough number of times to understand its result or outcome in the long run
4. MCTS algorithm needs a huge number of iterations to be able to effectively decide the most efficient path. So, there is a bit of a speed issue there
5. Since single moves can change the course of a game, MCTS might fail to explore that move
6. Also, MCTS has a disadvantage when there is a clear winning move for one side or the other (according to human knowledge or an evaluation function), but where it will still take many moves in a playout to verify a winner

# Key Terms & Concepts

- A ***tree policy*** is an informed policy used for action (node) selection in the *snowcap* (explored part of the game tree as opposed to the vast unexplored bottom part)

# References

MCTS

- [https://en.wikipedia.org/wiki/Monte_Carlo_tree_search](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search)
- [https://www.cs.cmu.edu/~katef/DeepRLFall2018/MCTS_katef.pdf](https://www.cs.cmu.edu/~katef/DeepRLFall2018/MCTS_katef.pdf)
- [https://towardsdatascience.com/monte-carlo-tree-search-an-introduction-503d8c04e168](https://towardsdatascience.com/monte-carlo-tree-search-an-introduction-503d8c04e168)
- [https://www.analyticsvidhya.com/blog/2019/01/monte-carlo-tree-search-introduction-algorithm-deepmind-alphago/](https://www.analyticsvidhya.com/blog/2019/01/monte-carlo-tree-search-introduction-algorithm-deepmind-alphago/)
- [https://towardsdatascience.com/monte-carlo-tree-search-158a917a8baa](https://towardsdatascience.com/monte-carlo-tree-search-158a917a8baa)
- [https://www.geeksforgeeks.org/ml-monte-carlo-tree-search-mcts/](https://www.geeksforgeeks.org/ml-monte-carlo-tree-search-mcts/)
- [https://www.cs.swarthmore.edu/~meeden/cs63/s19/reading/mcts.html](https://www.cs.swarthmore.edu/~meeden/cs63/s19/reading/mcts.html)
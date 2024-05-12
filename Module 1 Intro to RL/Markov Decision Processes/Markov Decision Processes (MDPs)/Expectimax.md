# What is Expectimax?

- The most straightforward approach is actually a simplification of the Expectiminimax algorithm for game trees with chance nodes
    - The Expectimax algorithm builds a tree of alternating max and chance nodes
        - **There is a slight difference from the standard Expectiminimax in that there are rewards on nonterminal as well as terminal transitions**
- An evaluation function can be applied to the nonterminal leaves of the tree, or they can be given a default value

# How does it work?

- A decision can be extracted from the search tree by backing up the utility values from the leaves, taking an average at the chance nodes and taking the maximum at the decision nodes
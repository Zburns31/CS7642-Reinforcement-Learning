# What is Value Iteration?

- **Basic Idea:** In value iteration, you start with a random value function - $U(s)$ - and then find a new (improved) value function in an iterative process, until reaching the optimal value function
    - ***Notice that you can derive easily the optimal policy from the optimal value function***. This process is based on the optimality Bellman operator
- **Why does Value Iteration Work?**
    - Start with noise, continually add truth until you get closer and close to the actual truth
    - **Use values at the previous time step to update values at the current time step**
- **Why does Value Iteration converge to the optimal solution?**
    - Since we maximize our actions at any givens step, we guarantee that the next iteration will be better than the la***st***
- ***Value iteration effectively combines, in each of its sweeps, one sweep of policy evaluation and one sweep of policy improvement***

# How does it work?

- The Bellman equation is the basis of the value iteration algorithm
    - ***If there are n possible states, then there are n Bellman equations, so one for each state***
        - The n equations contain *n* unknowns - the utilities of the the states
- **In value iteration, we compute the optimal state value function by iteratively updating the estimate U(s).** Let $U_i(s)$ by the utility value for the state *s* at the *i*th iteration. The iteration step, called a ***Bellman update*** is***:***

$$
\hat U_{i+1}(s) \leftarrow \max_{a \in A(s)} \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma U_i(s')]
$$

- Alternatively, this Bellman update can also be written as:

$$
\hat U_{t+1}(s) = R(s) + \gamma * \argmax_a \sum_{s'} T(s, a, s') \space \hat U_t(s')
$$

- Notes:
    - *The update is assumed to be applied simultaneously to all the states at each iteration*
    - If we apply the Bellman update infinitely often, we are guaranteed to reach an equilibrium in which case the utility values must be solutions to the Bellman equations
        - ***The solutions are unique solutions and the corresponding policy is optimal***
        - The update step is very similar to the update step in the policy iteration algorithm
            - The only difference is that we take the maximum over all possible actions in the value iteration algorithm
        - Instead of evaluating and then improving, the value iteration algorithm updates the state value function in a single step
            - This is possible by calculating all possible rewards by looking ahead
- For value iteration, we can start with some estimate $\hat U_t(s)$ and use this to update $\hat U_{t+1}(s)$
    - Update based on the neighbours
    - **We update the utility of $s$ by looking at the utilities of all the other states (that are reachable), including $s'$ and weight those based on the probability of getting there, given that I took an action**
- Utility at time step *t + 1* is equal to:
    - The immediate reward for entering that state
    - Plus:
        - Gamma: the discount rate/factor
        - The max of the sum of transition probabilities and expected utilities from state s' (next state)

# Algorithm Overview

**Value Iteration Steps:**

1. Start off with arbitrary utilities (I.e. all states U(s) == 0 like in Week 1: Lecture 22 Finding Policies - 3 Quiz)
2. Update based on neighbours (all states that can be reached)
3. Repeat until convergence

    ![The value of entering a state (the reward) gets propagated through all of the states iteration by iteration](./Value%20Iteration/Untitled.png)

    The value of entering a state (the reward) gets propagated through all of the states iteration by iteration

- For the value iteration (picture above),  we calculate the state of the square with the green x using the initial values (arbitrary)
- For the second iteration, we include the utilities of the states reachable around that square (directly underneath with a utility of -.04

## Algorithm Pseudocode

- ***The values for all states propagate themselves from the goal state to the start state***

    AI - A Modern Approach (Russel and Norvig)


![Untitled](./Value%20Iteration/Untitled%201.png)

Intro to RL (Sutton and Barto)

![Untitled](./Value%20Iteration/Untitled%202.png)

```python
procedure Value_iteration(S,A,P,R)
		Inputs:
			- S is the set of all states
			- A is the set of all actions
			- P is state transition function specifying P(s′∣s,a)
			- R is a reward function R(s,a)

		Output
			- π[S] approximately optimal policy
			- V[S] value function

		Local variables
			- real array Vk[S] is a sequence of value functions
			- action array π[S]

		assign V0[S] arbitrarily
		k:=0
		repeat
			k:=k+1
			for each state s do:
				Vk[s]= max_a R(s,a)+γ*∑s′P(s′∣s,a)*Vk−1[s′]

			until termination
			for each state s do:
				π[s]=argmaxaR(s,a)+γ*∑s′P(s′∣s,a)*Vk[s′]

		return π,V
```

# Convergence Properties

# Pros & Cons of Value Iteration

**Cons**

- Computes utility value for every state
- Needs exact transition model
- Needs to fully observe state
- Needs to know exact reward for each state

# Resources

**Value Iteration**

- [https://www.baeldung.com/cs/ml-value-iteration-vs-policy-iteration](https://www.baeldung.com/cs/ml-value-iteration-vs-policy-iteration)
- [http://incompleteideas.net/book/ebook/node44.html](http://incompleteideas.net/book/ebook/node44.html)
- [https://artint.info/2e/html/ArtInt2e.Ch9.S5.SS2.html](https://artint.info/2e/html/ArtInt2e.Ch9.S5.SS2.html)
- [https://medium.com/@ngao7/markov-decision-process-value-iteration-2d161d50a6ff](https://medium.com/@ngao7/markov-decision-process-value-iteration-2d161d50a6ff)
- [https://web.mit.edu/1.041/www/lectures/L15-value-iteration-2021fa-pre.pdf](https://web.mit.edu/1.041/www/lectures/L15-value-iteration-2021fa-pre.pdf)
- [https://gibberblot.github.io/rl-notes/single-agent/value-iteration.html](https://gibberblot.github.io/rl-notes/single-agent/value-iteration.html)
- [https://mpatacchiola.github.io/blog/2016/12/09/dissecting-reinforcement-learning.html](https://mpatacchiola.github.io/blog/2016/12/09/dissecting-reinforcement-learning.html)

**Working Examples**

- [https://towardsdatascience.com/implement-value-iteration-in-python-a-minimal-working-example-f638907f3437](https://towardsdatascience.com/implement-value-iteration-in-python-a-minimal-working-example-f638907f3437)
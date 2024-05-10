# Thompson Sampling

---

# How it works

- To begin with, all machines are assumed to have a uniform distribution of the probability of success, in this case getting a reward
    - For each observation obtained from a Slot machine, based on the reward a new distribution is generated with probabilities of success for each slot machine
    - Further observations are made based on these prior probabilities obtained on each round or observation which then updates the success distributions
    - After sufficient observations, each slot machine will have a success distribution associated with it which can help the player in choosing the machines wisely to get the maximum rewards
- We use the **[beta distribution](https://www.notion.so/Beta-Distribution-99d811cda66440c19d7e2b9a11ca58fb?pvs=21) to model this probability**

# Algorithm Overview

- **Step 1:** At each round n, we consider two numbers for each action *i*
    - $N_i^1(n)$  —> The number of times we took action *i* and got reward 1 up to round *n*
    - $N_i^0(n)$  —> The number of times we took action *i* and got reward 0 up to round *n*
- **Step 2:** For each action *i,* we take a random draw from the distribution below
    - $\theta_i(n) = \beta(N_i^1(n) + 1, N_i^0(n) + 1)$
- **Step 3:** We select the action that has the highest $\theta_i(n)$
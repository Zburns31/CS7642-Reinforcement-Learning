# What is Direct Utility Evaluation?

- The idea of direct utility estimation is that the utility of a state is defined as the expected total reward from that state onward (called the expected reward-to-go), and that each trial provides a sample of this quantity for each state visited

# How does it work?

- In this method, the agent executes a **sequence of trials or runs** (sequences of states-actions transitions that continue until the agent reaches the terminal state)
    - Each trial gives a sample value and the agent estimates the utility based on the samples values
        - Can be calculated as **running averages of sample values**.
- ***Use observed rewards and future rewards to estimate U***
    - Take average of samples from data
    - In the limit of infinitely many trials, the sample average will converge to the true expectation
- **Idea:** Average reward from each state over all trials

# Pros & Cons

- The main drawback is that this method makes a wrong assumption that state utilities are independent while in reality they are [Markovian](https://en.wikipedia.org/wiki/Markov_property)
- Also, it is slow to converge

# Resources

- [https://courses.cs.vt.edu/cs4804/Fall16/pdfs/10 Passive Reinforcement Learning.pdf](https://courses.cs.vt.edu/cs4804/Fall16/pdfs/10%20Passive%20Reinforcement%20Learning.pdf)
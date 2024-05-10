# Original Implementation (2010)

- The original Double Q-learning algorithm uses two independent estimates $Q^A$ and $Q^B$
- With a 0.5 probability, we use estimate $Q^A$ to determine the maximizing action, but use it to update $Q^B$
    - Conversely, we use $Q^B$ to determine the maximizing action, but use it to update $Q^A$
    - By doing so, we obtain an unbiased estimator for the expected Q value and inhibit bias

## Algorithm Overview

![Untitled](./Double%20Q-Learning/Untitled.png)

# New Implementation (2015)

- In the second Double Q-learning algorithm, we have a model $Q$ and a target model $Q'$ instead of two independent models, as in [(Hasselt, 2010)](https://papers.nips.cc/paper_files/paper/2010/file/091d584fced301b442654dd8c23b3fc9-Paper.pdf)
    - We use the $Q'$ for action selection and $Q$ for action evaluation
    - We slowly move the target network $Q'$ towards the value network using either soft updates ($\tau$) or periodically copying the weights

## Update

![Untitled](./Double%20Q-Learning/Untitled%201.png)

# Resources

Original Implementation (Separate Networks)

- [Paper](https://papers.nips.cc/paper_files/paper/2010/file/091d584fced301b442654dd8c23b3fc9-Paper.pdf)

**New Implementation**

- [https://arxiv.org/pdf/1509.06461.pdf](https://arxiv.org/pdf/1509.06461.pdf)

**General**

- [https://towardsdatascience.com/double-deep-q-networks-905dd8325412](https://towardsdatascience.com/double-deep-q-networks-905dd8325412)
# Eligibility Trace Breakdown

Let’s walk through an example of how an eligibility trace works:

1. $z_0 = 0$
2. $z_1 = \gamma \lambda z_0 + \nabla v_1 \rightarrow 0 + \nabla v_1$
3. $z_2 = \gamma \lambda z_1 + \nabla v_2 \rightarrow \gamma \lambda \nabla v_1 + \nabla v_2$
4. $z_3 = \gamma \lambda z_2 + \nabla v_3 \rightarrow \gamma \lambda(\gamma \lambda \nabla v_1 + \nabla v_2) + \nabla v_3 \rightarrow (\gamma \lambda)^2 * \nabla v_1 + \gamma \lambda \nabla v_2 + \nabla v_3$

Notes:

- $z$ here represents vectors

This algorithm is said to be using a backward mechanism

![Untitled](./Eligibility%20Trace%20Breakdown/Untitled.png)

- *As each update depends on the current TD error combined with the current eligibility traces of past events*

# Resources

- [https://meatba11.medium.com/reinforcement-learning-td-λ-introduction-2-f0ea427cd395](https://meatba11.medium.com/reinforcement-learning-td-%CE%BB-introduction-2-f0ea427cd395)
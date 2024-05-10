# What is it?

**Basic Idea:** If we knew which state we were in before and assuming we know the transition model, then we know the likelihood of ending up in state $s'$

- If we want to compute $b'(s)$ which represents the probability of being state $s'$ after transition $b,a,z$, then we can create an update:

    $$
    Pr(s'|b,a,z) = \sum_{s'}Pr(s|b,a,z) \cdot Pr(s'|b,a,z) \\

    \newline

    = \sum_{s'} b(s) Pr(s'|a,z,s) \\

    \newline

    = \sum_{s'} b(s) \dfrac{Pr(z| s', a,s) \cdot Pr(s'|a,s)} {Pr(z|a,s)}
    $$

- Where:
    - $Pr(z| s', a,s)$ gives us the probability of making some observation $z$ given that we arrived in some state $s'$, took action $a$ and came from $s$
    - $Pr(s'|a,s)$ gives us the probability that we ended up that state $s'$
    - $Pr(z|a,s)$ gives us the normalization piece which tells us the probability of the observation given the action and state
- After some simplification we can modify this update to:

$$
\dfrac {\sum_s' b(s) \space O(s',z) T(s,a,s')} {\sum_{s} \sum_{s'} b(s) O(s',z) T(s,a,s')}
$$
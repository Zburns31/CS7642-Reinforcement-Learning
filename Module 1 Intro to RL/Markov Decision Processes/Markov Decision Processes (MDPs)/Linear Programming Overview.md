# Linear Programming

---

# What is Linear Programming (LP)?

- **Linear programming** is an [optimization](https://brilliant.org/wiki/optimization-problems/) technique for a [system of linear constraints](https://brilliant.org/wiki/systems-of-inequalities/) and a linear objective function
    - An **objective function** defines the quantity to be optimized, and the goal of linear programming is to find the values of the variables that maximize or minimize the objective function

# How does it work?

- Linear programming problems can be solved using various methods, but the most commonly used algorithm is the simplex method. Here are the general steps to solve a linear programming problem using the simplex method:
    1. Formulate the problem: 
        1. Write down the objective function and the constraints in terms of the decision variables
    2. Convert the problem to standard form: 
        1. Convert any maximization problems to minimization problems, ensure all variables are non-negative, and add slack or surplus variables to the constraints as necessary
    3. Create the initial simplex tableau: 
        1. Write down the coefficients of the decision variables and slack/surplus variables in a matrix, along with the objective function coefficients and constants
    4. Choose a pivot: 
        1. Select a pivot element in the tableau using a pivot rule, such as the largest coefficient rule
    5. Perform row operations: 
        1. Use row operations to make the pivot element equal to 1, and to make all other elements in its column equal to 0
    6. Repeat: 
        1. Continue selecting pivot elements and performing row operations until the optimal solution is found
    7. Interpret the results: 
        1. Once an optimal solution is found, interpret the values of the decision variables and the value of the objective function in the context of the problem

# Resources

- [https://brilliant.org/wiki/linear-programming/](https://brilliant.org/wiki/linear-programming/)
-
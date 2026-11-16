# Lab 09: Exploring D&C Algorithms

**Instructions:** Work in small groups to complete the following exercises. Try sketching out your work on the whiteboards or scratch paper. Everyone should submit their own answers to the exercises, but you are encouraged to discuss your reasoning with your group members.

Name(s): __Luke Perez__

---

## Exercise 1: Recurrence Extraction

Consider the following divide-and-conquer algorithm, which takes an array $A$ of $n$ numbers as input and returns a single number. 

```text
Procedure Mystery(A, n)
    if n <= 1:
        return A[1]                              // Base case
        
    k = floor(n / 3)                             // Find the one-third point
    
    left = Mystery(A[1..k], k)                   // Recur on the left third
    right = Mystery(A[k+1..n], n - k)            // Recur on the rest
    
    total = 0 
    for i = 1 to n:
        total = total + (A[i] * left * right)    // Combine results
        
    return total
```

### Part A: The Recurrence Relation
Let $T(n)$ be the running time of `Mystery` on an input of size $n$. Write down a recurrence relation for the worst-case running time $T(n)$. You may assume $n$ is a power of 3 for simplicity. *(Note: You do not need to solve the recurrence relation yet, just formulate it).*
- T(n) = T(n/3) + T(2n/3) + cn for n > 1, with T(1) = Theta(1).

### Part B: Visualizing Algorithm Behavior
Without solving the recurrence, look closely at the recursive calls and the combine step. If you were to draw the recursion tree for this algorithm, what would it look like? Is the tree balanced or skewed?
- The recursion tree is not balanced. The two recursive calls split the array into sizes n/3 and 2n/3, so the tree is already off as one branch is shorter than the other. The combine step does a linear amount of work, O(n), at each step. Since the total problem size across each level is still proportional to n, the total work per level is O(n), and the tree has logarithmic height. Overall, the running time is still O(n log n) in the worst case.

---

## Exercise 2: Unrolling the Recursion Tree

Suppose you have developed a new divide-and-conquer algorithm. You analyze your pseudocode and determine that its worst-case running time $T(n)$ satisfies the following recurrence relation:

$$T(n) \leq 3T\left(\frac{n}{2}\right) + cn^2 \quad \text{for } n > 1$$
$$T(1) \leq c$$

where $c$ is a positive constant. 

### Part C: Recursion Tree Analysis

Analyze the running time of the algorithm by unrolling the recursion tree. Specifically, answer the following questions:

1. **Root Work:** Find the amount of work done at the root level (level $0$) of the recursion tree.
- At the root, there is 1 node with input size n, so the work is cn^2.

2. **First Few Levels:** Find the total amount of work done at level $1$ and level $2$ of the recursion tree.
- Level 1: 3 nodes each with work cn^2/4, so work is 3(cn^2 / 4) = (3cn^2 / 4)
- Level 2: 9 nodes each with work cn^2/16, so work is 9(cn^2 / 16) = (9cn^2 / 16)

3. **Tree Height:** Find the height of the recursion tree (at what level does the input size reach $1$?).
- The input size is halved at each level, so the tree height is log_2 n.

**Bonus!** Find a general formula for the total work done at level $i$ of the recursion tree.

---

## Exercise 3: Reflection

1. During lecture last week, we looked at two different divide-and-conquer algorithms. What were these algorithms and what problems did they solve?
- The first algorithm was Merge Sort, which sorts an array in O(n log n) time. The second was the nearest neighbor divide and conquer algorithm, which finds the closest pair of points in O(n log n) time. Both are examples of divide and conquer algorithms that improve on slower O(n^2) approaches.

2. How do the algorithms you analyzed in this lab compare to the ones we discussed in lecture? Specifically, each algorithm differs from the two we discussed in lecture in one important way. For each of the algorithms above, what is that difference?
- The first algorithm, Mystery, differs from the lecture examples because it does not sort or search for a closest pair. Instead, it recursively splits the problem and then combines the results from both recursive calls by multiplying them together in linear time.
- The second recurrence, T(n) \leq 3T(n/2) + cn^2, differs from the lecture algorithms because it has a larger branching factor of 3 instead of 2 and a different combine step of cn^2 instead of linear work like cn. It is still divide and conquer, but its structure leads to a different recurrence and different complexity.

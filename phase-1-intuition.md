# CM1 — Phase 1

# Why Complexity Matters

## 1. Correctness Is Not Enough

An algorithm can be mathematically correct and still be completely unusable.

<aside>
🎇

To understand why, consider the **Travelling Salesman Problem (TSP)**.

We are given **n cities**. The goal is to visit each city exactly once and return to the starting city. A naïve solution consists of trying all possible permutations of cities.

</aside>

The number of possible tours is approximately:

$$
(n - 1)!
$$

For 10 cities: 362 880, this is manageable

For 50 cities: 

$$
49! \approx 6 \times 10^{62}
$$

Solving the problem for 50 cities using brute force would take far longer than the age of the universe.

The conclusion is fundamental:

The issue is not the computer.

The issue is the **growth rate of the algorithm**.

---

## 2. What Complexity Measures

We define the complexity of an algorithm as:

$$
T(A, d)
$$

<aside>
💡

which represents the number of elementary operations executed by algorithm A on input d

</aside>

We do not measure seconds. We count operations.

This makes complexity:

- Independent of hardware
- Independent of programming language
- Independent of implementation details

We focus only on structure.

In practice, we analyze:

$$
T(n)
$$

n is the size of the input

---

## What Is Input Size?

The definition of n depends on the problem

For example:

- For an array → number of elements
- For an integer → number of bits needed to encode it
- For a tree → number of nodes
- For a polynomial → degree

This is important: complexity depends on how we measure input size

---

## Why Constants Are Ignored

Suppose two algorithms have complexities:

$$
1000n^2 \quad \text{and} \quad n^3
$$

For small n the quadratic one may seem slower because of the large constant 1000.

But as it grows large, the cube dominates

We are interested in long-term growth, not small-scale performance.

Constants affect speed.
Growth rate determines scalability.

---

## Worst, Best, and Average Case

For all inputs of size n we define 

Worst-case complexity:

$$
T_{\max}(n)
$$

Best-case complexity:

$$
T_{\min}(n)
$$

Average-case complexity:

$$
T_{\text{moy}}(n)
$$

The worst-case complexity is the most important because:

- It provides a guarantee.
- It does not rely on probability assumptions.
- It ensures reliability in all cases.

### Example: Sequential Search

If we search for an element in a list of size n

- Best case: the element is first → 1 comparison.
- Worst case: the element is last or absent → n comparisons

So the worst-case complexity is linear.

### Example: Maximum of an Array

To compute the maximum of an array of size n

every element must be inspected.

There is no shortcut.

Best case = Worst case = n 

This shows that some problems inherently require linear time.

---

## Growth Rate vs Machine Power

Suppose a computer can execute 1.000.000 operations

What is the largest input size it can handle?

**If complexity is linear:**

$$
n \le 10^6
$$

so it can contains 1.000.000 elements

---

**If complexity is quadratic:**

$$
n^2 \le 10^6\Rightarrow n \le 1000
$$

---

**If complexity is exponential:**

$$
2^n \le 10^6\Rightarrow n \approx 20
$$

---

Now suppose we upgrade the machine to perform 10 ^ 12 operations

Linear case:

$$
n \le 10^{12}
$$

Quadratic case:

$$
n \le 10^6
$$

Exponential case:

$$
2^n \le 10^{12}\Rightarrow n \approx 40
$$

Observe what happened.

We increased machine power by a factor of one million.

Linear algorithms improved massively.

Quadratic algorithms improved significantly.

Exponential algorithms only improved from input size 20 to 40.

<aside>
🎇

That is the key insight:

Hardware improvements multiply capacity.

Exponential growth multiplies required work at every step.

When complexity doubles with each increase of n, hardware gains become negligible.

</aside>
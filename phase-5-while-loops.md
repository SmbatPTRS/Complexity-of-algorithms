# While Loops— Phase 5

# Formal General Formula for While Loops

For a while loop:

```cpp
while condition:
    sequence Ti(n)
```

Let:

- **C(n) = complexity of evaluating the condition**
- **Ti(n) = complexity of the body at iteration i**
- **k = number of iterations**

THEN

$$
T(n) = \sum_{i=1}^{k} \big(C(n) + T_i(n)\big)
$$

<aside>
💡

Unlike for loops (where k is explicit), in a while loop:

→ k must be determined from the algorithm’s logic.

So the analysis splits into two parts:

1. Determine or bound k.
2. Compute the summation.

This separation is fundamental.

</aside>

---

# First Difficulty — Determining k

There are two main cases.

## Case 1 — We Can Bound k

If the loop variable is monotonic and bounded, then k is bounded by the size of the input.

### Example — Linear Growth

```cpp
i=0
whilei<n:
i+=1
```

Here:

- i is strictly increasing.
- i ≤ n.

Thus:  K = N 

Using the formula:

$$
T(n) = \sum_{i=1}^{n} \big( C(n) + \Theta(1) \big)
$$

Since C(n) = Θ(1): just cheks the condition

$$
T(n) = n *\Theta(1) = \Theta(n)
$$

---

## Case 2 — Logarithmic Behavior

```cpp
x = n
while x > 1:
	x = x // 2
```

We solve:

$$
\frac{n}{2^k} = 1\\.\\k = \log_2 n
$$

Thus:

as C(n) = Θ(1) and the body complexity here is also  Θ(1)

$$
T(n) = \sum_{i=1}^{\log n} \Theta(1) = \Theta(\log n)
$$

---

## Variant Method — Deep Understanding

### What Is a Variant (Mathematically)?

A **variant** is a quantity V such that:

1. V changes strictly at each iteration
2. V is bounded (above or below)

$$
k \leq \text{max value of V} - \text{min value of V}
$$

<aside>
💡

So instead of counting loops directly, we count how many times V can change.

This converts loop analysis into simple bounding.

</aside>

> 
> 
> 
> Students often multiply nested loops blindly:
> 
> outer loop runs n
> inner loop runs n
> → n²
> 
> But if both loops change the same bounded variable, total work may be only n.
> 
> The variant method detects that.
> 

## Example 1 — Your Handwritten Code

From your image, the structure is essentially:

```cpp
j = 0
i = 1

while i < N - 1:

    i = i + 1

    while j < N and L[i] ≠ L[i+1]:
        j = j + 1
```

Let’s formalize this correctly.

### Step 1 — Identify Variables That Change

We see:

- i increases in the outer loop.
- j increases in the inner loop.
- Neither ever decreases.
- Both are bounded by N.

$$
0 \le i \le N\\.\\0 \le j \le N
$$

If j resets to 0 every outer iteration → quadratic.

If j keeps increasing globally → linear.

---

j  here is global and keeps increasing.

So total increments of j ≤ N.

Total increments of i ≤ N.

---

### Step 3 — Define a Strong Variant

Instead of analyzing loops separately, define:

$$
V = i + j
$$

Now observe:

- Every iteration of either loop increases V by 1.
- V starts small.
- V ≤ 2N.

Therefore:

$$
T(N) = \Theta(N)
$$

<aside>
💡

### Why Not Quadratic?

Because:

Inner loop does not restart j from 0.

It continues increasing.

So total inner iterations over entire execution ≤ N.

</aside>

## Example 2 — Segment Division

```cpp
def VW(L, k):
    N = len(L)
    i = 0
    m = 0
    while i < N:

        m += 1
        maxi = L[i]
        mini = L[i]
        i += 1  <---------------------------------------------------        

        while i < N and L[i] - mini <= k and maxi - L[i] <= k:

            if L[i] > maxi:
                maxi = L[i]
            elif L[i] < mini:
                mini = L[i]

            i += 1 <-------------------------------------------------

    return m
```

### Step 1 —What Students Usually Think

Two nested while loops → maybe Θ(N²).

**Wrong.**

### Step 2 — Identify All Variables That Move

Which variables change?

- i increases.
- m increases.
- maxi and mini update.
- L and k are fixed.

Observe carefully:

- i starts at 0.
- i only increases.
- i never decreases.
- i is bounded by N.

So:

$$
0 \le i \le N
$$

### Step 3 — Count How Many Times i Increases

Look at the code:

### In the outer loop:

```
i += 1
```

### In the inner loop:

```
i += 1
```

So every iteration of either loop increases i by 1.

No reset.

No backward move.

### Step 4 — Variant Definition

Define the variant:

$$
V = i
$$

That means:

Total executions of the inner body + outer increments ≤ N.

Suppose all elements satisfy the condition.

Then inner loop runs until i reaches N.

But after that, outer loop stops immediately.

So total i increments = N.

Suppose every segment is size 1.

Then:

Outer loop runs N times,
Inner loop never runs.

Again total i increments = N.

This algorithm scans the list once.

It partitions the list into segments.

But it never re-scans elements.

Each element is processed exactly once.

---

## Non-Rectangular Iteration Domains

Now consider true quadratic case:

```cpp
i = 0
while i < N:

    j = i
    
    while j < N:
    
        j = j + 1
        
    i = i + 1
```

Now j resets each time.

So:

For i = 0 → j runs N times
For i = 1 → j runs N-1 times
For i = 2 → j runs N-2 times

Total:

$$
T(N) = \frac{N(N+1)}{2}
$$

<aside>
💡

Why quadratic here?

Because there is no global monotone variant covering both loops.

Yes i grows , but only in the inner loop

</aside>

# Second Difficulty — Computing the Summation

The variant method gives you:

$$
k \leq \text{some bound}
$$

<aside>
💡

⁉️But that only counts **how many iterations**.

If each iteration costs something non-constant, you must compute:

</aside>

$$
T(n) = \sum_{i=1}^{k} T_i(i)
$$

```cpp
i = 1
while i <= N:
    x = 1
    while x <= i:
        x = x * 2
    i += 1 <-------------------- this shi is in OUTER loop 
```

### Step 1 — Variant

Outer loop:

- i increases
- i ≤ N

So total outer iterations = N.

### Step 2 — Cost of Inner Loop

Inner loop doubles x until it exceeds i.

We solve:

$$
2^t= i \\.\\t = \log_2 i
$$

So cost of inner loop = Θ(log i).

### Step 3 — Total Cost

$$
T(N) = \sum_{i=1}^{N} \log i
$$

Now we compute it using integral comparison, because there is no formula for this

In algorithm analysis, the base does **not matter asymptotically**.

$$
\log_2(i) = \frac{\ln(i)}{\ln(2)}\quad|=>\quad \log_2(i) = C \cdot \ln(i)
$$

so  integral comparison

$$
\int_1^N \ln(u) du \le \sum_{i=1}^{N} \ln(i) \le \int_1^{N+1} \ln(u) du
$$

We compute

$$
\int \ln(u) du = u\ln(u) - u\quad = \quad[u\ln(u) - u]_1^N\quad=N\ln N - N+1
$$

thus

$$
\sum_{i=1}^{N} \ln(i) = N\ln N - N + O(1)
$$

O(1) means: some constant that does not grow with N.

<aside>
💡

## Important Concept

Variant gives you the **number of iterations**.

But the total cost is:

number of iterations × cost per iteration.

If cost per iteration depends on i → summation required.

</aside>

---

# Probabilistic Variant (Average Case)

Until now, our variant counted:

- how many times a variable can increase,
- how many times it can decrease,
- how much distance remains to a bound.

That was deterministic.

Now we consider:

The loop may stop randomly.

So the number of iterations is not fixed — it is a random variable.

We then compute the **expected number of iterations**

## Example 1 — Searching in a Randomly Ordered List

Consider:

```cpp
i=0
whileL[i] != target:
	i+=1
```

Assume:

- The target is present exactly once.
- It is equally likely to be in any position from 0 to N-1.

### Worst Case

Target at last position:

T(N) = Θ(N)

---

### Best Case

Target at first position:

T(N) = Θ(1)

---

### Average Case

Let X be the position of the target., “How many steps until we find the target?”

If the target is at:

- index 0 → X = 0
- index 1 → X = 1
- index 2 → X = 2
- ...
- index N−1 → X = N−1

So X can take N different values.

Since every position is equally likely:

$$
P(X = k ) = 1/N
$$

Expected position : 

Now, what does expected value mean?

It means:

If you repeat the experiment many times,
what is the average value of X?

$$
E[X] = \frac{0 + 1 + 2 + ... + (N-1)}{N}
$$

we know that

$$
0 + 1 + ... + (N-1) = \frac{N(N-1)}{2}
$$

so

$$
E[X] = \frac{N-1}{2}
$$

Thus average number of iterations is about:  N /  2 so

$$
T_{avg}(N) = \Theta(N)
$$

---

## Example 2 — Rolling a Die Until You Get a 6

```cpp
while roll_die() != 6:
	pass
```

Question:

On average, how many rolls until you get a 6?

Each roll:

Probability of success = 1/6.

<aside>
💡

Intuition:

You succeed about once every 6 rolls.

So expected number of rolls = 6.

Constant.

</aside>

---

### Example 3 — Random Decrease Process

Consider:

```cpp
x=N
while random() < 0.5:
	x-=1
```

Each iteration continues with probability 0.5.

Expected number of iterations = 2.

Even though x starts at N, the loop almost never reaches 0.

So average complexity is constant.

---

# General Formula (Geometric Case)

If:

- Probability of continuing = p
- Probability of stopping = 1 − p
- p is constant (0 < p < 1)

Then expected number of iterations is:

$$
E[k] = \frac{p}{1-p}
$$

CONSTANT

$$
Therefore:T_{avg}(N) = \Theta(1)
$$

In probabilistic variant:

Remaining resource = expected number of future trials.

Instead of:

"How far until we hit N?"

We ask:

"On average, how many steps before stopping?"

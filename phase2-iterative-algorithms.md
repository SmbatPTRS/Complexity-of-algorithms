# Complexity of Iterative Algorithms— Phase 2

# Step 1: Elementary Operations

What counts as one operation?

Examples of elementary operations:

- Assignment → `x = 5`
- Arithmetic operation → `a + b`
- Comparison → `x < y`
- Increment → `i = i + 1`
- Accessing an array element → `A[i]`

Each of these costs **1 unit**.

We assume constant time.

We do not care about the real processor.

This is the atomic level.

---

# Step 2: Sequence Rule

Now suppose we have:

```
x =5
y =7
z = x + y
```

Each line costs 1.

So total cost = 1 + 1 + 1 = 3.

<aside>
💡

General rule:

If we have instructions executed one after another,

the total complexity is the **sum** of their complexities

</aside>

---

# Step 3: Selection (if / else)

Now consider:

```
if condition:
    sequence1
    
    else:
    sequence2
```

We must count:

1. The cost of evaluating the condition.
2. The cost of the branch that runs.

Since we are analyzing worst case, we take the branch that costs more.

So the rule becomes:

`T(n) = C(n) + max(T1(n), T2(n))`

Where:

- `C(n)` is cost of the condition
- `T1(n)` is cost of first branch
- `T2(n)` is cost of second branch

### Example

```
if x>0:
	y = y+1
else:
	for i in range(n):
		y = y+i
```

Condition → cost 1

First branch → cost 1

Second branch → loop of n iterations → cost n

Worst case → second branch

So total:

T(n) = 1 + n

We always assume worst case unless told otherwise.

---

# Step 4: For Loop (When Iterations Known)

Now we reach the first important structure.

Consider:

```
for i inrange( 1, n+1):
    x = x +1
```

How many iterations?

n.

What is inside?

One assignment → cost 1.

So total cost: n × 1 = n.

Now suppose inside we have:

```
for i inrange(1, n+1):
    x = x +1
    y = y +2
```

Inside cost = 2.

Total cost = 2n.

<aside>
💡

General rule:

If loop runs `k` times and body costs `T_body(n)`,

then total cost is:

Sum over all iterations of  `T_body(n).`

If `T_body(n`) is constant:

`T(n)` = `k` × constant.

</aside>

## Example — Nested Loops

```
fori inrange(n):
    for j inrange(n):
        x = x +1
```

Inner loop runs n times.

Outer loop runs n times.

So total operations:

n × n = n².

## Example :  Growing Triangle

```
for i inrange(n):
    for j inrange(i):
        x = x +1
```

For each `i`, the inner loop runs `i` times.

So total operations:

0 + 1 + 2 + ... + (n − 1)

We used the classical formula

$$
0 + 1 + ... + (n-1) = \frac{n(n-1)}{2}
$$

Exact complexity:

`T(n) = (n² − n)/2`

Asymptotically: Θ(n²)

This is a triangular pattern.

## Example : Shrinking Triangle

```
for i inrange(n):
    for j inrange(i, n):
        x = x +1
```

Now the inner loop runs:

n + (n−1) + (n−2) + ... + 1

Using the same formula:

$$
0 + 1 + ... + (n-1) = \frac{n(n-1)}{2}
$$

Exact complexity:

`T(n) = (n² + n)/2`

Asymptotically: Θ(n²)

Again quadratic.

---

# Step 5 : While Loops

Here, things become harder.

Because in a `while` loop, we do not always know how many iterations occur.

Unlike `for` loops, the number of iterations is not always obvious.

We must **bound it**.

## Example 1 — Multiplying by 2

```
i =1
while i < n:
    i = i * 2
```

Let’s simulate what happens.

Start:

i = 1

After 1st iteration → i = 2

After 2nd iteration → i = 4

…

After k iterations:

i = 2^k

The loop stops when:

2^k ≥ n

How many times can we double 1 before reaching n?

We solve:

2^k ≥ n

So the loop runs about:

log₂(n) times.

Each iteration costs constant time.

So:  T(n) ≈ log(n)

<aside>
💡

Logarithmic complexity appears whenever:

- You multiply by a constant
- Or divide by a constant
- Or cut the problem size by a constant factor
</aside>

# The Variant Method

How can we bound the number of iterations of a `while` loop when it is not obvious?

<aside>
💡

# What Is a Variant?

A variant is:

An integer expression that:

- Strictly increases (or strictly decreases) at every iteration

• Is bounded

If both conditions are satisfied, then the loop cannot execute infinitely many times.

</aside>

## 2. Simple Example — Increasing Variant

```
i =1
while i < n:
    i = i +2
```

Here:

`i` increases by 2 each iteration.

So:

1 → 3 → 5 → 7 → ...

The variant here is simply: `i`

Because:

1.It increases each time

2.It is bounded by `n`

Consider something slightly more complicated:

```
i=0
while i<n:
	if condition:
			i=i+1
		else:
			i=i+5
```

Now the increment is not fixed.

Sometimes +1  , Sometimes +5

But we observe:

`i` increases by at least 1 every iteration.

And `i` is bounded above by `n`.

So even in the worst case (when it increases by 1 each time),

the loop runs at most `n` times.

So: → T(n) = O(n)

We do not need the exact number of iterations.

We just need an upper bound.

---

# VW Algorithm

## What Is the VW Algorithm Doing?

We are given:

- A list `L`
- A number `k`

The goal is:

Cut the list into consecutive segments such that

inside each segment, the values do not differ by more than `k`.

In other words:

Inside one segment,

`max value − min value ≤ k`

## Let’s look at the code

```jsx
def VW(L, k):
    N = len(L)
    i = 0      # indice courant
    m = 0      # nombre de slices

    while i < N:

        # on determine un segment a chaque iteration
        m += 1
        maxi = L[i]
        mini = L[i]
        i += 1

        while i < N and L[i] - mini <= k and maxi - L[i] <= k:

            if L[i] > maxi:
                maxi = L[i]

            elif L[i] < mini:
                mini = L[i]

            i += 1

    return m
```

### Step 1 — What the Algorithm Does

The goal:

Cut the list `L` into consecutive segments such that inside each segment:

max − min ≤ k

The algorithm scans the list from left to right.

The variable `i` is the index that moves through the list.

The variable `m` counts how many segments we create.

---

### Step 2 — What Happens in the Outer While

```
while i < N:
```

This means:

As long as we haven't reached the end of the list, we create a new segment.

Inside:

```
m += 1
maxi = L[i]
mini = L[i]
i += 1
```

We:

- Start a new segment
- Initialize its min and max
- Move forward one position

---

### Step 3 — What Happens in the Inner While

```
while i < Nand L[i] - mini <= kand maxi - L[i] <= k:
```

As long as:

- We are still inside the list
- Adding `L[i]` does not violate the constraint

We:

- Update `maxi` or `mini` if needed
- Move `i` forward

So the inner loop extends the current segment.

---

### Step 4 — The Critical Observation

Now we focus only on complexity.

The only variable controlling movement is:

`i`

And we observe:

- `i` starts at 0
- `i` only increases
- `i` is incremented exactly once every time either loop progresses
- `i` never decreases
- `i` never resets

It eventually reaches `N`.

So how many times can `i += 1` execute?

At most `N` times.

---

### Step 5 — Why It Is Linear

Even though there are two while loops:

They do not multiply.

<aside>
💡

The inner loop does not restart scanning the list from the beginning.

It continues from where the outer loop left off.

So total increments of `i` across both loops:

Exactly N.

</aside>

All other operations inside the loops are constant time.

Therefore:

T(N) = Θ(N)

---

<aside>
💡

There are only two places where `i` increases:

Once in the outer loop (right after starting a segment)

Inside the inner loop (each time we extend the segment)

There are no other increments of `i`.

</aside>

`i` is incremented exactly N times in total.

Each time we start a new segment (outer loop), we do: i+=1

How many segments are there? → `m`

So the outer loop contributes:

`m` increments of `i`.

Total increments = N.

Outer increments = `m`.

So the rest must be:

`N − m`

## Why This Proves It Is Linear ?

Because every iteration of either loop increments `i`.

Since `i` can only increase N times total:

The total number of loop iterations across both loops is N.

Therefore:

T(N) = Θ(N)
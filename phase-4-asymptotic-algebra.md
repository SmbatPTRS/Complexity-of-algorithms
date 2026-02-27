# Algebra of Asymptotics— Phase 4

# Why Algebra of Asymptotics Exists

When you analyze a real algorithm, you never get something clean like:

T(n) = n²

You get something messy:

T(n) = 3n² + 7n log n + 12n + 5

<aside>
💡

The role of asymptotic algebra is to:

eliminate irrelevant details
isolate the dominant growth mechanism
preserve what matters for scalability

This is not simplification.
It is abstraction.

</aside>

---

# The Sum Rule — Why the Maximum Dominates

Statement

$$
O(g_1 + g_2) = O(\max(g_1, g_2))
$$

Suppose `g1` grows faster than `g2`.

Then eventually:

`g1(n) ≥ g2(n)`

So:

`g1(n) + g2(n) ≤ 2 * g1(n)`

Since constants don’t matter:

$$
g_1 + g_2 = Θ(g_1)
$$

### Real Algorithm Example

Consider:

```cpp
for i in range(n):
    do_constant_work()

for i in range(n):
    for j in range(n):
        do_constant_work()
```

The first loop costs Θ(n).

The second costs Θ(n²).

Total:

`T(n) = Θ(n) + Θ(n²)`

**Now ask: what really determines growth?**

As n grows large, the n² part completely dominates.

Formally:

$$
T(n) = Θ(n^2)
$$

---

# Product Rule — Why Multiplication Models Nesting

### Statement

If one part costs Θ(f)
and it is repeated Θ(g) times

then total cost is:

$$
Θ(f \cdot g)
$$

Everything comes from this single idea:

> Total work = number of repetitions × work per repetition
> 

## First Layer: One Loop

Example:

```cpp
for i in range(n):
    do_constant_work()
```

### Step A — Work per iteration

Each iteration costs Θ(1).

### Step B — Number of iterations

n iterations.

### Step C — Total work

We add Θ(1) to itself n times:

## Second Layer: Two Nested Loops (Rectangular Case)

Now:

```cpp
for i in range(n):
    for j in range(n):
        do_constant_work()
```

Outer loop runs n times.

Each time it triggers the entire inner loop.

So we add Θ(n) to itself n times:

$$
\sum_{i=1}^{n} Θ(n)
$$

which equals to

$$
Θ(n \cdot n) = Θ(n^2)
$$

THIS IS WHERE MULTIPLICATION HAPPENS.

---

### Non-Rectangular Case (Important)

```
for i in range(n):
    for j in range(i, n):
        do_constant_work()
```

This is not rectangular.

TOTAL NUMBER OF ITERATIONS IS

$$
\frac{n(n+1)}{2}
$$

SO AS WE OPEN IT WE CAN SAY

$$
T(n) = Θ(n^2)
$$

## General Pattern

If:

Outer loop = Θ(g(n)) iterations

Inner body = Θ(f(n)) cost

Then total:

$$
\sum_{i=1}^{Θ(g(n))} Θ(f(n))=Θ(f(n) \cdot g(n))
$$

<aside>
💡

To understand nested loops, always ask:

1️⃣ How many outer iterations?
2️⃣ What is the cost of one full inner execution?

Then multiply.

</aside>

---

# Transitivity

Transitivity means:

IF
`f` grows no faster than `g` and  `g` grows no faster than `h`

Then
`f` grows no faster than `h.`

$$
f = O(g), \quad g = O(h)\Rightarrow f = O(h) 
$$

f = O(g) means
there exists C such that eventually:

f(n) ≤ C · g(n)

g = O(h) means
there exists D such that eventually:

g(n) ≤ D · h(n)

Combine them:

`f(n) ≤ C g(n) ≤ C D h(n)`  |⇒ `f(n) ≤ (C D) h(n)`

We know:

n = O(n log n)
n log n = O(n²)

Without recalculating limits, we conclude:

n = O(n²)

---

# Θ Is an Equivalence Relation

Now something deeper.

When we say:

f = Θ(g)

We mean:

f grows at the same rate as g.

Formally:

There exist constants k₁ and k₂ such that:

$$
k_1 g(n) \le f(n) \le k_2 g(n)
$$

## Why Θ Is Special

Θ satisfies three properties:

### 1️⃣ Reflexive

Every function grows like itself.

f = Θ(f)

### 2️⃣ Symmetric

If f grows like g,

then g grows like f.

f = Θ(g) ⇒ g = Θ(f)

### 3️⃣ Transitive

If f grows like g,

and g grows like h,

then f grows like h.

---

Because of these three properties,

Θ divides functions into **growth classes**.

## Example Growth Class

All these functions belong to Θ(n²):

3n²

100n² + n

n² − log n

They are different formulas.

But same growth behaviour.

That is why complexity compares classes, not exact formulas.

---

# Now the global picture of growth:

$$
1 \ll \log n \ll n^\alpha \ll n \ll n^\beta \ll b^n \ll n!
$$

Where:

0 < α < 1 < β
1 < b
# Classical Sums and Integral Comparison— Phase 2.1

If cost at iteration i is proportional to:

- constant → total = Θ(n)
- i → total = Θ(n²)
- i² → total = Θ(n³)
- log(i) → total = Θ(n log n)

<aside>
💡

This part is important because it allows us to compute the complexity of loops where the cost is not constant

</aside>

# When Do Sums Appear?

<aside>
💡

Sums appear when:

The number of operations executed in iteration `i`

is not constant, but depends on `i`

</aside>

## First  Example

```jsx
for i in range(1, n+1):
    for j in range(i):
        x = x + 1
```

When i = 1
Inner loop runs 1 time.

When i = 2
Inner loop runs 2 times.

When i = 3
Inner loop runs 3 times.

So total operations:

1 + 2 + 3 + ... + n

$$
1 + 2 + ... + n = \frac{n(n+1)}{2}
$$

This behaves like: n² / 2

So: T(n) = Θ(n²)

---

## Second Example

```jsx
for i in range(1, n+1):
    for j in range(i*i):
        x = x + 1
```

When i = 1 → 1 operation

When i = 2 → 4 operations

…

So total cost:

1² + 2² + 3² + ... + n²

We use another classical formula:

$$
\sum i^2 = \frac{n(n+1)(2n+1)}{6}
$$

This behaves like: n³ / 3 → `T(n) = Θ(n³)`

---

## Third Correct Example — Logarithmic Cost

```jsx
for i in range(1, n+1):
    do_something_that_costs_log_i()
```

<aside>
💡

This means:

At iteration i,
the number of operations executed is proportional to log(i).

So total cost: log(1) + log(2) + log(3) + ... + log(n)

</aside>

We can write it as:

$$
\sum_{i=1}^{n} log(i)
$$

Now here is the issue:

There is no simple formula like:

- sum i = n(n+1)/2
- sum i² = ...

So what do we do?

We approximate it.

---

## Why Approximation Is Enough ?

<aside>
💡

We are studying asymptotic growth.

We only care about:

How fast does this grow when n becomes large?

So approximation is perfectly acceptable.

</aside>

## Step 1 — What does log(i) look like?

log(i) grows slowly.

For example (roughly, base 2 just for intuition):

log(1) = 0

log(2) = 1

log(4) = 2

log(8) = 3

log(16) = 4

So even when i becomes large, log(i) grows very slowly.

So every term in the sum is between 0 and log(n).

---

## Step 2 — A Simple Upper Bound (No Integrals)

There are n terms total.

Each term is at most log(n).

So the whole sum is at most:  →    `n × log(n)`

So we know:

`sum log(i) ≤ n log(n)`

So the growth is **at most**  `n log(n).` 

<aside>
💡

We have n iterations and log(n) is the worst case.

As we are examining the UPPER bound , we presume that every iteration has the worst cost

</aside>

---

## Step 3 — A Simple Lower Bound (No Integrals)

If n = 10, the sum is:

`log(1) + log(2) + log(3) + log(4) + log(5) + log(6) + log(7) + log(8) + log(9) + log(10)`

There are 10 terms.

If n = 10, then `n/2 = 5`.

So the last half corresponds to:

`log(6) + log(7) + log(8) + log(9) + log(10)`

Now why are we looking at the last half?

Because those values are large.

`log(1)` is small.
`log(2)` is small.

But `log(8), log(9), log(10)` are much bigger.

So instead of summing everything, we only look at the bigger half to estimate how large the total must be.

<aside>
💡

So roughly speaking:

You have about n/2 terms,
each roughly size log(n).

</aside>

So just that part contributes about:

`(n/2) × log(n)`

<aside>
💡

So we have:

`sum log(i) ≥ (n/2)·log(n/2)`

and

`sum log(i) ≤ n·log(n)`

</aside>

---

<aside>
💡

If cost at iteration i is proportional to:

- constant → total = Θ(n)
- i → total = Θ(n²)
- i² → total = Θ(n³)
- log(i) → total = Θ(n log n)
</aside>

---

## Why Do We Use Integrals?

Sometimes we do not know a simple formula for:

`sum f(i)`

For example:

`sum log(i)`

So instead of finding an exact formula, we estimate how fast it grows.

<aside>
💡

If `f(x)` is monotonic (increasing or decreasing), then:

The sum behaves like the area under the curve `y = f(x)`.

</aside>

Mathematically:

$$
\int_1^n f(x)dx \le \sum_{i=1}^n f(i) \le \int_1^{n+1} f(x)dx
$$

If the integral grows like `n log n`, then the sum also grows like `n log n`.

## Example 1 — √i Example (Full Code)

```
count =0
for i in range(1, n+1):
    j =0
    while j <int(sqrt(i)):
        count +=1
        j +=1
```

For each `i`, the inner loop runs approximately √i times.

So:

- When i = 1 → runs 1 time
- When i = 4 → runs 2 times

So total operations:

√1 + √2 + √3 + ... + √n

$$
\sum_{i=1}^{n} \sqrt{i}
$$

COMPUTE

$$
\int x^{1/2} dx = \frac{2}{3} x^{3/2}
$$

So roughly:

$$
\frac{2}{3} n^{3/2}
$$

T(n) = Θ(n^(3/2))

---

# Asymptotic Notations— Phase 3

# 1. Big-O — Upper Asymptotic Bound

## Formal Definition

$$
f = O(g) \iff\exists k>0 ,\exists n_0,\forall n >= n_0 : f(n)<= k*g(n)
$$

After some threshold, f is bounded above by a constant multiple of g.

<aside>
💡

## Deep Meaning

Big-O is a one-sided domination.

It does not mean equality, and it does not mean tightness.

It only guarantees that f does not grow faster than g asymptotically.

</aside>

## How to Prove

Method 1 — Inequalities: dominate lower-order terms.

Method 2 — Limit: if

$$
\lim_{n \to \infty} \frac{f(n)}{g(n)}< \infty
$$

then f = O(g).

## Correct Example

Claim:

$$
f(n) = n\sqrt{n} + 4n\ln n\\f = O(n^{3/2})
$$

Compute:

$$
\frac{f(n)}{n^{3/2}} =1 + 4\frac{\ln n}{\sqrt{n}}
$$

the ratio tends to 1.

so OUR CLAIM IS TRUE

---

## Incorrect Example

False claim:

$$
f(n)=n^{1+\sin n}\\f=O(n)
$$

We have:

$$
\frac{f(n)}{n} = n^{\sin n}
$$

So along that subsequence,

$$
n^{\sin n} \to \infty
$$

Condition fails.

---

# 2.Big-Θ — Same Order of Magnitude

## Formal Definition

$$
f = \Theta(g)\;\Longleftrightarrow\;\exists k_1,k_2 >; 0,\; \exists n_0,\; \forall n  >; n_0 :k_1 g(n)\le f(n) \le k_2 g(n)
$$

f and g are of the same order of magnitude

f is asymptotically bounded above AND below by g up to constants.

There exist constants k₁ and k₂ such that eventually:

k₁ g(n) ≤ f(n) ≤ k₂ g(n)

<aside>
💡

After some threshold n₀,

f(n) stays trapped between two constant multiples of g(n).

It is not allowed to escape upward.
It is not allowed to collapse downward.

</aside>

$$
f = \Theta(g)\quad \Longleftrightarrow \quad f = O(g) \text{ and } g = O(f)
$$

---

## CONDITION

$$
\ell = \lim_{n \to \infty} \frac{f(n)}{g(n)}
$$

## If 0 < ℓ < +∞ → f = Θ(g)

## If  ℓ = 0 |⇒  f(n) = o( g(n) ) ⇒ f(n) = O( g(n) )

## If   ℓ= +∞ |⇒ g(n) = o( f(n) )

## If  If the limit does not exist, then: The limit test is useless.

---

## Example 1:

$$
n^2 + 4n - 1 = \Theta(n^2)
$$

Lets compute :

$$
\frac{n^2 + 4n - 1}{n^2}=1 + \frac{4}{n} - \frac{1}{n^2}
$$

so the limit is equal to 1

as : 

$$
0 < 1 < +\infty
$$

THE STATEMENT IS CORRECT

---

## Example 2:

$$
f(n) = n(1+\sin n)\\g(n) = n
$$

in this case the limit does not exist, so we must find another way.

## Upper bound of f(n)

Since: sine is negative infinately many times, there are infinite number of n such that:

$$
\sin n \le1
$$

adding 1

$$
1+\sin n \le2
$$

if we multiply by n

$$
f(n) \le 2n
$$

So

$$
f(n) = O(n)
$$

---

## Lower bound of f(n)

Since sine is positive infinitely many times, there are infinitely many n such that:

$$
\sin n \ge 0
$$

add 1

$$
1+\sin n \ge 1
$$

mult by n 

$$
f(n) \ge n
$$

$$
f(n) = \Omega(n)
$$

---

![image.png](Asymptotic%20Notations%E2%80%94%20Phase%203/image.png)

---

## Example 3:

$$
f(n) = n \log n\\g(n) = n\\f(n) = \Theta(n)?
$$

Compute the limit, we will see that 

$$
\lim_{n \to \infty} \frac{f(n)}{g(n)} = +\infty
$$

this means that 

$$
g(n) = O(f(n)) \\ f(n) \ne O(g(n))
$$

so So they are not Θ-equivalent.

---

# 3.Big-Ω

We say:

**f(n) = Ω(g(n))**

if and only if:

$$
\exists k \gt; 0,\ \exists n_0,\ \forall n \ge n_0,\quad f(n) \ge k\, g(n)
$$

<aside>
💡

Big-Ω gives a **lower bound**.

It means:

For large n, f(n) grows at least as fast as g(n), up to a constant factor.

</aside>

## If g(n) is positive for large n, compute:

$$
\ell = \lim_{n \to \infty} \frac{f(n)}{g(n)}
$$

## 1. if L = 0 | ⇒  f is **not** Ω(g)

## 2. if L > 0 |⇒ f = Ω(g)

## 3. if L = + ∞ |⇒ f = Ω(g)

---

## Example 1

Check whether:

f(n) = n² + 4n

g(n) = n²

$$
\lim_{n \to \infty} \frac{n^2 + 4n}{n^2}=\lim_{n \to \infty} \left(1 + \frac{4}{n}\right)=1
$$

second case is true

### this means that f(n) = Ω(n²)

---

## Example 2 — Logarithm vs Polynomial

Let:

f(n) = n log n

g(n) = n

Compute:

$$
\lim_{n \to \infty} \frac{n \log n}{n}=\lim_{n \to \infty} \log n=+\infty
$$

Therefore:

f(n) = Ω(n)

Interpretation:

n log n grows strictly faster than n, so it is certainly at least a constant multiple of n for large n.

---

## Example 3

We take:

f(n) = n
g(n) = n^(1 + sin n)

<aside>
💡

- Sometimes sin n is close to 1
- Sometimes sin n is close to −1

That means the exponent oscillates between:

0 and 2

So `g(n)` somethimes is `n^0 = 1` and sometimes is `n^2`

</aside>

### Case A — When sin n ≈ 1

f(n) = n
g(n) ≈ n²

Clearly n is much smaller than n².

The ratio becomes:

1/n → 0.

f is much smaller than g.

### Case B — When sin n ≈ −1

Then:

g(n) ≈ 1

Compare:

f(n) = n

g(n) ≈ 1

Now n is much larger than 1.

So now f is much larger than g.

<aside>
💡

To say: f = Ω(g)

we must find a constant k > 0 such that eventually : f(n) ≥ k g(n)

for all large n

</aside>

SADLY : 

infinitely often, g(n) becomes too large compared to n.

No constant k can fix that.

Therefore: f is NOT Ω(g)

---

# 4.Little-o (small o)

This is the **strict** version of Big-O.

We say:

f(n) = o(g(n))

if and only if

$$
\lim_{n \to \infty} \frac{f(n)}{g(n)} = 0
$$

or

$$
\forall \varepsilon \gt; 0,\ \exists n_0,\ \forall n \ge n_0,\quad
f(n) \le \varepsilon\, g(n)
$$

<aside>
💡

`f(n)` grows strictly slower than `g(n)`.

In other words:

Eventually, f becomes negligible compared to g.

</aside>

---

f = o(g) ⇒ f = O(g)

But

f = O(g) ⇏ f = o(g)

---

## Example 1 — Polynomial degrees

Let:

f(n) = n

g(n) = n²

compute

$$
\lim_{n \to \infty} \frac{n}{n^2}=\lim_{n \to \infty} \frac{1}{n}=0
$$

Therefore: `n = o(n²)`

Linear growth is negligible compared to quadratic growth

---

## Example 2 — Log vs Linear

Let:

f(n) = log n

g(n) = n

$$
\lim_{n \to \infty} \frac{\log n}{n} = 0
$$

`log n = o(n)`

Even though log n grows to infinity, it is still negligible compared to n.

---

## ! Example 3 — Polynomial vs Exponential

$$
\lim_{n \to \infty} \frac{n^a}{b^n} = 0 \quad (b\gt1)
$$

Therefore:

`n^a = o(b^n)`

<aside>
💡

This is a fundamental hierarchy fact:

Every polynomial is negligible compared to an exponential.

</aside>

---

## Example 4 — When little-o does NOT hold

Let:

f(n) = 3n²

g(n) = n²

Compute:

$$
\lim_{n \to \infty} \frac{3n^2}{n^2} = 3
$$

3n² is NOT o(n²)

Even though: `3n² = Θ(n²)`

---

<aside>
💡

`1 ≪ log n ≪ n ≪ n² ≪ 2ⁿ ≪ n!`

Each “`≪`” corresponds to little-o.

</aside>

---

# 5.Asymptotic Equivalence (~)

We say:

`f(n) ~ g(n)` if: 

$$
\lim_{n \to \infty} \frac{f(n)}{g(n)} = 1
$$

or

$$
\exists \varepsilon(n) \to 0 \text{ such that } f(n) = g(n)(1+\varepsilon(n))
$$

<aside>
💡

Asymptotic equivalence means:

For large n, f and g behave almost exactly the same.

Not just same order (like Θ),

but the **ratio converges to 1**.

</aside>

If `f(n) ~ g(n)`

then  `f(n) = Θ(g(n))`

But the converse is false.

Θ ignores constant factors.

~ does NOT ignore constant factors.

---

`3n² = Θ(n²)`

but

`3n²`  is  NOT `~ n²`

because the ratio → 3, not 1.

---

## Example 1 — Simple Polynomial

Let:

`f(n) = n² + 4n`

`g(n) = n²`

Compute:

$$
\frac{f(n)}{g(n)}=\frac{n^2 + 4n}{n^2}=1 + \frac{4}{n}
$$

Now take the limit:

$$
\lim_{n\to\infty} \left(1+\frac{4}{n}\right)=1
$$

Therefore:

n² + 4n ~ n²

---

## Example 2 — Logarithm Sum (from your slides)

In the slides we have:

`Hₙ = Θ(log n)`

Hₙ is called the **harmonic sum**

$$
H_n = \sum_{i=1}^{n} \frac{1}{i}
$$

`Hₙ = 1 + 1/2 + 1/3 + 1/4 + ... + 1/n`

But more precisely, it is known:

Hₙ ~ log n

Why?

want to approximate a sum:

$$
\sum_{i=p}^{n} f(i)
$$

<aside>
💡

The key idea:

If f is monotone (increasing or decreasing), then the sum can be bounded between two integrals.

</aside>

$$
\int_{p}^{n+1} f(u)\,du\;\le\;\sum_{i=p}^{n} f(i)\;\le\;\int_{p-1}^{n} f(u)\,du
$$

![image.png](Asymptotic%20Notations%E2%80%94%20Phase%203/image%201.png)

The width of that rectangle is:

`(i) − (i−1) = 1`

The height of that rectangle is: `f(i)`

So its area is:

width × height = `1 × f(i) = f(i)`

---

## Step 2 — Apply It to the Harmonic Sum

They take: `f(u) = 1/u`

and define:

$$
H_n = \sum_{i=1}^{n} \frac{1}{i}
$$

Now we look at the integral: `∫ 1/u du`

We know:

`∫ 1/u du = log u`

But log u is only defined for u > 0.

because -> The area under 1/u from 0 to 1 is infinite.

`So we cannot integrate from 0.` 

because 

### General Proposition

$$
\int_{p}^{\,n+1} f(u)\,du\;\le\;\sum_{i=p}^{n} f(i)\;\le\;\int_{p-1}^{\,n} f(u)\_du
$$

when p =1 

then p - 1  =  0

p = 2

Why 2?

Because now:

p − 1 = 1

$$
\int_{2}^{\,n+1} \frac{1}{u}\,du\;\le\;\sum_{i=2}^{n} \frac{1}{i}\;\le\;\int_{1}^{\,n} \frac{1}{u}\,du
$$

we know that 

$$
\sum_{i=2}^{n} \frac{1}{i} = H_n-1
$$

so 

$$
\int_{2}^{\,n+1} \frac{1}{u}\,du\;\le\;H_n - 1\;\le\;\int_{1}^{\,n} \frac{1}{u}\,du
$$

Compute integrals:

$$
\ln(n+1) - \ln 2\;\le\;H_n - 1 \le \ln(n) +1
$$

but after a deeper proof (which i dont want to do) we also have that:

$$
\lim_{n \to \infty} \frac{H_n}{\ln n} = 1 \\.\\SO\\.\\H_n \sim \ln n
$$

---

## The Statement (Stirling’s Formula)

FROM the notes

$$
n! \sim \sqrt{2\pi n}\left(\frac{n}{e}\right)^n
$$

this means

$$
\lim_{n\to\infty}\frac{n!}{\sqrt{2\pi n}\left(\frac{n}{e}\right)^n}=1
$$

---

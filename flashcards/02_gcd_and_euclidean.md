+++
order = 2
subject = "Math"
tags = ["math", "number-theory", "gcd", "euclidean-algorithm", "bezout", "lcm"]
+++

# Number Theory — GCD and the Euclidean Algorithm

## 2.1 Defining the GCD

C: The [greatest common divisor] $\gcd(a, b)$ of integers $a, b$ (not both zero) is the unique positive integer $d$ such that (i) $d \mid a$ and $d \mid b$, and (ii) any common divisor of $a$ and $b$ divides $d$.

Q: Why is the "universal" characterization (every common divisor divides $d$) equivalent to "the largest"?
A: For positive $d$, "every common divisor divides $d$" forces any common divisor $\leq d$ (divisors of $d$ are $\leq d$); conversely, the largest common divisor is divided by every other common divisor (since it absorbs their contributions). The universal form is more useful in proofs: it extends naturally to abstract settings (polynomial rings, ideals).

Q: What is $\gcd(0, b)$ for $b > 0$?
A: $\gcd(0, b) = b$. Every positive integer divides $0$ (since $0 = b \cdot 0$), and $b$ divides itself, so $b$ is the greatest common divisor. Convention: $\gcd(0, 0)$ is undefined (no largest common divisor since every integer divides $0$).

## 2.2 Coprime Integers

C: Integers $a, b$ are [coprime] (or relatively prime) iff $\gcd(a, b) = 1$.

Q: Why is coprimality so important in number theory and cryptography?
A: Because coprimality is the condition under which modular inverses exist, under which the Chinese Remainder Theorem applies cleanly, and under which Fermat/Euler's theorems hold. Coprimality is the integer analogue of "linearly independent" — the structural condition enabling decomposition into simpler components.

## 2.3 The Euclidean Algorithm

C: The [Euclidean algorithm] computes $\gcd(a, b)$ by repeatedly replacing $(a, b)$ with $(b, a \bmod b)$ until the second entry is $0$; the gcd is then the first entry.

Q: Why does $\gcd(a, b) = \gcd(b, a \bmod b)$?
A: Write $a = bq + r$ with $r = a \bmod b$. Any common divisor of $(a, b)$ divides $r = a - bq$, so divides $(b, r)$. Any common divisor of $(b, r)$ divides $a = bq + r$, so divides $(a, b)$. The two sets of common divisors are equal, hence the same greatest element.

Q: Why is the Euclidean algorithm efficient?
A: Because each step reduces the smaller argument by at least half every TWO steps (Lamé's theorem). So the number of steps is $O(\log \min(a, b))$. For $1000$-digit inputs, roughly $10000$ arithmetic steps suffice — feasible for RSA-sized integers; no known general algorithm for gcd is asymptotically faster for typical inputs.

P: Compute $\gcd(2024, 748)$ via the Euclidean algorithm.
S:
**IDENTIFY**: Apply the recursion.

**PLAN**: Iterate $a \bmod b$ until $0$.

**EXECUTE**:
1. $2024 = 2 \cdot 748 + 528$.
2. $748 = 1 \cdot 528 + 220$.
3. $528 = 2 \cdot 220 + 88$.
4. $220 = 2 \cdot 88 + 44$.
5. $88 = 2 \cdot 44 + 0$.

$\gcd(2024, 748) = 44$.

**EVALUATE**: Five steps, very quick. Sanity check: $2024 = 44 \cdot 46$ and $748 = 44 \cdot 17$, with $\gcd(46, 17) = 1$ (since $46 = 2 \cdot 17 + 12$, etc.) — so $44$ is the right answer.

## 2.4 Bézout's Identity

C: [Bézout's identity]: for integers $a, b$ not both zero, there exist integers $x, y$ with $ax + by = \gcd(a, b)$.

Q: Why is Bézout's identity so central to number theory?
A: Because it links gcd (a divisibility concept) with linear combinations. Consequences: (i) existence of modular inverses when coprime; (ii) characterization of $\gcd(a,b)$ as the SMALLEST positive $ax + by$; (iii) principal ideal structure of $\mathbb{Z}$; (iv) the correctness proof for RSA. One identity unlocks a huge amount of theory.

Q: Why is $\gcd(a, b)$ the smallest positive integer of the form $ax + by$?
A: Let $d = \gcd(a, b)$. By Bézout, $d$ is of that form. Conversely, every $ax + by$ is divisible by $d$ (since $d$ divides both $a$ and $b$, hence the combination), so any positive value $ax + by$ satisfies $ax + by \geq d$. Combined: $d$ is attainable and minimal — hence the smallest.

## 2.5 Extended Euclidean Algorithm

C: The [extended Euclidean algorithm] returns $\gcd(a, b)$ and integers $x, y$ satisfying $ax + by = \gcd(a, b)$; it back-substitutes through the Euclidean steps.

Q: Why do we need the extended algorithm, not just the basic one?
A: Because many applications need the Bézout coefficients explicitly. Modular inverse: to solve $ax \equiv 1 \pmod m$, find $x, y$ with $ax + my = 1$; then $x \equiv a^{-1} \pmod m$. RSA key generation, decoding Reed–Solomon codes, and solving linear Diophantine equations all need this.

P: Use the extended Euclidean algorithm to find $x, y$ with $99x + 78y = \gcd(99, 78)$.
S:
**IDENTIFY**: Run Euclidean forward, then back-substitute.

**PLAN**:
- Forward: compute gcd and record quotients.
- Backward: express gcd as linear combination.

**EXECUTE**:
1. Forward:
   - $99 = 1 \cdot 78 + 21$.
   - $78 = 3 \cdot 21 + 15$.
   - $21 = 1 \cdot 15 + 6$.
   - $15 = 2 \cdot 6 + 3$.
   - $6 = 2 \cdot 3 + 0$. So $\gcd = 3$.
2. Backward:
   - $3 = 15 - 2 \cdot 6$.
   - $= 15 - 2(21 - 15) = 3 \cdot 15 - 2 \cdot 21$.
   - $= 3(78 - 3 \cdot 21) - 2 \cdot 21 = 3 \cdot 78 - 11 \cdot 21$.
   - $= 3 \cdot 78 - 11(99 - 78) = -11 \cdot 99 + 14 \cdot 78$.
3. So $(x, y) = (-11, 14)$.

**EVALUATE**: Check $-11 \cdot 99 + 14 \cdot 78 = -1089 + 1092 = 3$ ✓. The coefficients are not unique — $(x + 78k/3, y - 99k/3) = (x + 26k, y - 33k)$ also work for any integer $k$.

## 2.6 Consequences of Bézout

Q: Why does $\gcd(a, b) = 1$ imply there exist $x, y$ with $ax + by = 1$?
A: Direct application of Bézout — the Bézout combination equals the gcd, which is $1$. The converse also holds: if $ax + by = 1$, any common divisor of $a, b$ divides $1$, so $\gcd(a, b) = 1$. This biconditional is the modular-inverse criterion.

Q: Why does $p$ prime and $p \mid ab$ imply $p \mid a$ or $p \mid b$? ([Euclid's lemma])
A: If $p \nmid a$, then $\gcd(p, a) = 1$ (since $p$'s only divisors are $1$ and $p$). By Bézout: $px + ay = 1$, so $pxb + aby = b$. Both terms on the left are divisible by $p$ ($pxb$ trivially, $aby$ since $p \mid ab$), hence $p \mid b$. This lemma is the key to unique factorization.

## 2.7 Linear Combinations and Linearity

Q: Why is the set $\{ax + by : x, y \in \mathbb{Z}\}$ always equal to $\gcd(a, b) \mathbb{Z}$?
A: Let $d = \gcd(a, b)$. Every $ax + by$ is divisible by $d$, so $\{ax+by\} \subseteq d\mathbb{Z}$. Conversely, Bézout gives some $ax_0 + by_0 = d$, so every multiple $kd = a(kx_0) + b(ky_0)$ is a linear combination, so $d\mathbb{Z} \subseteq \{ax+by\}$. Combined: the set of linear combinations is exactly the ideal $d\mathbb{Z}$.

Q: What does this say about $\mathbb{Z}$'s ideal structure?
A: Every ideal of $\mathbb{Z}$ is principal — of the form $n\mathbb{Z}$ for some $n \geq 0$. $\mathbb{Z}$ is a [principal ideal domain] (PID). This is the ring-theoretic statement behind the gcd theory — the language algebra uses to generalize number theory.

## 2.8 Least Common Multiple

C: The [least common multiple] $\text{lcm}(a, b)$ of nonzero integers is the smallest positive integer divisible by both $a$ and $b$.

Q: What is the relationship between gcd and lcm?
A: $\gcd(a, b) \cdot \text{lcm}(a, b) = |ab|$. Proof idea: in prime factorizations, gcd takes minimums of exponents, lcm takes maximums; min + max = sum of the exponents, which is the exponent in $ab$. Computationally: $\text{lcm}(a, b) = |ab|/\gcd(a, b)$, so no separate lcm algorithm is needed.

P: Compute $\text{lcm}(24, 36)$.
S:
**IDENTIFY**: Use the gcd-lcm product identity.

**PLAN**: Compute $\gcd$ first, then divide.

**EXECUTE**:
1. $\gcd(24, 36) = 12$ (by Euclidean algorithm or inspection).
2. $\text{lcm}(24, 36) = (24 \cdot 36)/12 = 864/12 = 72$.

**EVALUATE**: Verify: $72 / 24 = 3$ ✓ and $72 / 36 = 2$ ✓. Factorizations: $24 = 2^3 \cdot 3$, $36 = 2^2 \cdot 3^2$; lcm takes max exponents: $2^3 \cdot 3^2 = 72$ ✓.

## 2.9 GCD of More Than Two Integers

Q: How do you compute $\gcd(a_1, a_2, \dots, a_n)$?
A: Iteratively: $\gcd(a_1, a_2, a_3) = \gcd(\gcd(a_1, a_2), a_3)$, and similarly for more. Associativity of gcd makes this valid. Bézout generalizes: there exist integers $x_1, \dots, x_n$ with $\gcd(a_1, \dots, a_n) = \sum x_i a_i$.

## 2.10 GCD and Coprimality Identities

Q: If $\gcd(a, c) = 1$ and $\gcd(b, c) = 1$, why does $\gcd(ab, c) = 1$?
A: Suppose $p$ is a prime dividing $\gcd(ab, c)$. Then $p \mid ab$ so (by Euclid's lemma) $p \mid a$ or $p \mid b$. And $p \mid c$. So $p$ is a common factor of either $(a, c)$ or $(b, c)$ — contradicting coprimality. No such prime exists, so $\gcd(ab, c) = 1$.

Q: Why does $\gcd(a, b) = 1$ and $a \mid bc$ imply $a \mid c$?
A: By Bézout, $ax + by = 1$ for some $x, y$. Multiply by $c$: $axc + byc = c$. Both terms on the left are divisible by $a$ ($axc$ trivially, $byc$ since $a \mid bc$), so $a \mid c$. Generalizes Euclid's lemma to composite $a$.

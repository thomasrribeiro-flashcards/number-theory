+++
order = 1
subject = "Mathematics"
tags = ["math", "number-theory", "divisibility", "division-algorithm", "integers"]
+++

# Number Theory — Divisibility and the Division Algorithm

## 1.1 Why Number Theory

Q: Why does computer science care about number theory when computers operate on bits?
A: Because many foundational algorithms rely on integer structure: RSA and Diffie–Hellman use modular arithmetic, hash functions rely on primes, error-correcting codes live over finite fields, pseudo-random generators exploit multiplicative structure mod $p$. The integers are the most-studied algebraic object, and their theory is the shared vocabulary of cryptography, algorithms, and complexity.

Q: What's the difference between "number theory" as a subject and the number-theoretic chapter of a discrete-math course?
A: A discrete-math chapter typically covers gcd, mod, Fermat, and CRT to enable cryptography. Number theory as a subject studies the integers in their own right: structure of primes, analytic estimates (prime counting), diophantine equations, algebraic number fields. This deck focuses on the computational/cryptographic core — the tools you actually wield in CS.

## 1.2 Divisibility

C: For integers $a, b$ with $a \neq 0$, we say $a$ divides $b$ (written $a \mid b$) iff [$\exists k \in \mathbb{Z}\, (b = ak)$].

Q: Why exclude $a = 0$ from the definition of $a \mid b$?
A: Because "$0 \mid b$" would mean "$b = 0 \cdot k$ for some $k$", true only if $b = 0$ but with $k$ unrestricted — making $0 \mid 0$ trivially true yet not analogous to other divisibility statements. Many texts define $0 \mid 0$ (true) and $0 \nmid b$ for $b \neq 0$, but the most useful convention excludes $a = 0$ entirely to keep quotients $b/a$ meaningful.

## 1.3 Basic Properties of Divisibility

Q: If $a \mid b$ and $a \mid c$, why does $a \mid (mb + nc)$ for any integers $m, n$?
A: Write $b = ak_1$ and $c = ak_2$. Then $mb + nc = m \cdot ak_1 + n \cdot ak_2 = a(mk_1 + nk_2)$. The scalar $mk_1 + nk_2$ is an integer, so $a$ divides the linear combination. This is the [linearity of divisibility] — the key closure property.

Q: Why is divisibility transitive?
A: If $a \mid b$ and $b \mid c$, then $b = ak_1$ and $c = bk_2 = a(k_1 k_2)$, so $a \mid c$. Transitivity follows from associativity of integer multiplication: divisibility chains compose.

Q: What is the divisibility analogue of reflexivity?
A: $a \mid a$ for every integer $a \neq 0$ (take $k = 1$).

Q: In what sense is divisibility antisymmetric?
A: On positive integers: if $a \mid b$ and $b \mid a$ with $a, b > 0$, then $a = b$ — because $a \leq b$ and $b \leq a$ forces equality. Over all of $\mathbb{Z}$, you get $a = \pm b$ since signs are unconstrained. Divisibility on $\mathbb{Z}^+$ is thus a partial order.

## 1.4 The Division Algorithm

C: [Division algorithm]: for integers $a$ and $b$ with $b > 0$, there exist UNIQUE integers $q$ (quotient) and $r$ (remainder) such that $a = bq + r$ with $0 \leq r < b$.

Q: Why is the division algorithm a theorem, not a definition?
A: Because the existence and uniqueness of $(q, r)$ must be proved. Existence: the well-ordering principle applied to $S = \{a - bq : q \in \mathbb{Z}, a - bq \geq 0\}$ (nonempty and bounded below by $0$) yields a least element $r = a - bq$, and one shows $r < b$. Uniqueness: two representations $a = bq_1 + r_1 = bq_2 + r_2$ with both remainders in $[0, b)$ force $q_1 = q_2$ and $r_1 = r_2$.

P: Apply the division algorithm to $a = -17$, $b = 5$.
S:
**IDENTIFY**: Find $q, r$ with $-17 = 5q + r$ and $0 \leq r < 5$.

**PLAN**: Choose $q$ small enough that $r \geq 0$; remainder must be nonnegative even when $a < 0$.

**EXECUTE**:
- If $q = -3$: $-17 = -15 + r \Rightarrow r = -2 < 0$ — invalid.
- If $q = -4$: $-17 = -20 + r \Rightarrow r = 3$ with $0 \leq 3 < 5$ ✓.

So $q = -4$, $r = 3$.

**EVALUATE**: Truncation-toward-zero ($-17 / 5 = -3.4$, truncates to $-3$) gives $r = -2$ — WRONG for the mathematical division algorithm, which requires nonnegative remainders. Programming languages differ: C's `%` truncates, Python's `%` uses floor division. For number theory, always use the floor convention.

## 1.5 Floor and the Quotient

Q: Why is $q = \lfloor a/b \rfloor$ in the division algorithm (with $b > 0$)?
A: Because $q$ is the largest integer with $bq \leq a$, which is $\lfloor a/b \rfloor$ by definition of floor. The remainder is then $r = a - b \lfloor a/b \rfloor$, which automatically satisfies $0 \leq r < b$. Floor division is the computational realization of the division algorithm.

## 1.6 Parity

Q: In terms of divisibility by $2$, when is an integer $n$ even, and when is it odd?
A: Even iff $2 \mid n$; odd iff $2 \nmid n$.

Q: Why does "every integer is either even or odd, but not both" follow from the division algorithm?
A: Divide $n$ by $2$: the division algorithm yields a UNIQUE $(q, r)$ with $r \in \{0, 1\}$. $r = 0$ means $n = 2q$ (even); $r = 1$ means $n = 2q + 1$ (odd). Uniqueness rules out being both; existence rules out being neither.

P: Prove: the sum of two odd integers is even.
S:
**IDENTIFY**: Divisibility / parity proof.

**PLAN**: Express odd integers as $2k+1$ and combine.

**EXECUTE**:
1. Let $m = 2a + 1$ and $n = 2b + 1$ be odd integers.
2. $m + n = 2a + 2b + 2 = 2(a + b + 1)$.
3. Since $a + b + 1 \in \mathbb{Z}$, $2 \mid (m+n)$, so $m + n$ is even.

**EVALUATE**: Template for parity proofs: parametrize by the division-algorithm form, then compute. Parity is the simplest case of modular arithmetic mod $2$, a theme this deck expands throughout.

## 1.7 Divisibility Tests

Q: Why does the rule "a number is divisible by $3$ iff the sum of its digits is divisible by $3$" work?
A: Because $10 \equiv 1 \pmod 3$, so $10^k \equiv 1$ for all $k \geq 0$. The number $n = \sum d_k 10^k$ is then $\equiv \sum d_k \pmod 3$. Divisibility by $3$ of $n$ reduces to divisibility by $3$ of the digit sum. Same argument with $9$ works (since $10 \equiv 1 \pmod 9$).

Q: Why does the divisibility-by-$11$ rule (alternating digit sum) work?
A: Because $10 \equiv -1 \pmod {11}$, so $10^k \equiv (-1)^k \pmod{11}$. Then $n = \sum d_k 10^k \equiv \sum (-1)^k d_k \pmod{11}$ — the alternating digit sum. $11 \mid n$ iff $11$ divides that alternating sum.

## 1.8 Greatest Common Divisor (Preview)

C: The greatest common divisor $\gcd(a, b)$ of integers $a, b$ (not both zero) is the [largest positive integer dividing both].

Q: Why define $\gcd(a, b)$ for arbitrary integers, not just positives?
A: Because $\gcd(a, b) = \gcd(|a|, |b|)$ — sign doesn't affect divisibility. Extending to all integers (still excluding the $(0, 0)$ case) lets us apply the gcd machinery to Bézout coefficients, which are naturally signed, without case-splitting. The Euclidean algorithm in the next chapter makes this precise.

+++
order = 3
subject = "Mathematics"
tags = ["math", "number-theory", "primes", "unique-factorization", "prime-counting", "euclid"]
+++

# Number Theory — Primes and Unique Factorization

## 3.1 Primes and Composites

C: An integer $p > 1$ is [prime] iff its only positive divisors are $1$ and $p$.

C: An integer $n > 1$ is [composite] iff it is not prime — i.e., $n = ab$ with $1 < a, b < n$.

Q: Why is $1$ excluded from being prime?
A: Because if $1$ were prime, unique factorization would fail: $6 = 2 \cdot 3 = 1 \cdot 2 \cdot 3 = 1 \cdot 1 \cdot 2 \cdot 3 = \dots$, infinite valid factorizations. The modern convention defines "prime" so that every integer $> 1$ has ONE prime factorization (up to reordering). Units (here, $\pm 1$) are separate from primes in ring theory.

## 3.2 Existence of Prime Factorization

Q: Why does every integer $n > 1$ have a prime factorization?
A: By strong induction on $n$. Base: $n = 2$ is prime, so $n$ itself is its factorization. Step: for $n > 2$, either $n$ is prime (done) or $n = ab$ with $1 < a, b < n$; by IH, $a$ and $b$ factor into primes, and concatenating those gives a factorization of $n$.

## 3.3 Euclid's Lemma

C: [Euclid's lemma]: if $p$ is prime and $p \mid ab$, then $p \mid a$ or $p \mid b$.

Q: Why is Euclid's lemma crucial to uniqueness of factorization?
A: Because if two factorizations $n = p_1 \cdots p_k = q_1 \cdots q_m$ existed, Euclid's lemma lets you "cancel" one prime at a time: $p_1 \mid q_1 \cdots q_m$ forces $p_1 = q_i$ for some $i$; cancel and recurse. Without Euclid's lemma, unique factorization could fail (as it does in some number rings like $\mathbb{Z}[\sqrt{-5}]$).

## 3.4 The Fundamental Theorem of Arithmetic

C: [Fundamental theorem of arithmetic]: every integer $n > 1$ can be written UNIQUELY (up to ordering of factors) as a product of primes.

Q: Why is uniqueness a separate claim from existence?
A: Because existence only gives SOME factorization; uniqueness asserts that there is only ONE (up to reordering). Counterexamples in other settings: in $\mathbb{Z}[\sqrt{-5}]$, $6 = 2 \cdot 3 = (1 + \sqrt{-5})(1 - \sqrt{-5})$ with all four factors irreducible. The integers are nicer — Euclid's lemma plus Bézout provides the uniqueness.

Q: How do you write $n = p_1^{e_1} p_2^{e_2} \cdots p_k^{e_k}$ and what do $e_i$ measure?
A: This is the [canonical prime factorization] — primes listed in increasing order with multiplicities $e_i \geq 1$. Each $e_i = v_{p_i}(n)$ is the [$p_i$-adic valuation], counting how many times $p_i$ divides $n$ exactly. Example: $360 = 2^3 \cdot 3^2 \cdot 5$ has $v_2(360) = 3$.

## 3.5 Divisibility via Valuations

Q: How does $a \mid b$ translate into prime factorizations?
A: $a \mid b$ iff $v_p(a) \leq v_p(b)$ for every prime $p$. Divisibility becomes coordinate-wise $\leq$ in exponent vectors. Makes gcd/lcm obvious: $\gcd(a,b)$ takes coordinate-wise $\min$, $\text{lcm}(a,b)$ takes coordinate-wise $\max$.

Q: How do you count divisors of $n = p_1^{e_1} \cdots p_k^{e_k}$?
A: A divisor corresponds to an independent choice of $0 \leq f_i \leq e_i$ for each prime, giving $(e_1 + 1)(e_2 + 1) \cdots (e_k + 1)$ divisors. Example: $12 = 2^2 \cdot 3$ has $(2+1)(1+1) = 6$ divisors: $1, 2, 3, 4, 6, 12$ ✓.

P: How many positive divisors does $2025$ have?
S:
**IDENTIFY**: Factor $2025$ and apply the divisor-count formula.

**PLAN**: Factor into primes, multiply $(e_i + 1)$.

**EXECUTE**:
1. $2025 = 81 \cdot 25 = 3^4 \cdot 5^2$.
2. Number of divisors: $(4 + 1)(2 + 1) = 15$.

**EVALUATE**: The divisors are $3^a 5^b$ for $a \in \{0,\dots,4\}, b \in \{0, 1, 2\}$ — $5 \cdot 3 = 15$ combinations. The formula is the multiplication rule applied to exponent choices.

## 3.6 Infinitude of Primes

Q: How did Euclid prove that there are infinitely many primes?
A: Suppose for contradiction there are only finitely many primes $p_1, \dots, p_n$. Consider $N = p_1 p_2 \cdots p_n + 1$. $N$ has some prime factor $p$ (by existence of factorization). But $p \neq p_i$ for any $i$ (since $N \equiv 1 \pmod{p_i}$), so $p$ is a prime not in our list — contradicting finiteness.

Q: Why is $N = p_1 p_2 \cdots p_n + 1$ from Euclid's proof not necessarily prime itself?
A: The construction only guarantees an integer whose prime factors lie outside the list — $N$ itself need not be prime. Example: $2 \cdot 3 \cdot 5 \cdot 7 \cdot 11 \cdot 13 + 1 = 30031 = 59 \cdot 509$, composite but with new primes $59, 509$ not among $\{2,3,5,7,11,13\}$. A common misunderstanding of Euclid's proof.

## 3.7 The Sieve of Eratosthenes

Q: How does the sieve of Eratosthenes list primes up to $N$?
A: Start with all integers $2, 3, \dots, N$. Mark $2$ as prime; strike out all larger multiples of $2$. Find the smallest unmarked integer ($3$), mark prime; strike out its multiples. Repeat until $p > \sqrt{N}$: remaining unmarked integers are primes. Running time $O(N \log \log N)$ — essentially linear in $N$.

Q: Why do we only need to sieve up to $\sqrt{N}$?
A: Because if $n \leq N$ is composite, its smallest prime factor $p$ satisfies $p \leq \sqrt{n} \leq \sqrt{N}$ (otherwise $n = p \cdot q$ with both factors $> \sqrt{n}$ would give $n > n$). So sieving by primes up to $\sqrt{N}$ catches every composite $\leq N$.

## 3.8 Prime Gaps and Distribution

Q: What does the [prime number theorem] say?
A: $\pi(x)$ (number of primes $\leq x$) satisfies $\pi(x) \sim x / \ln x$ as $x \to \infty$. Proven (Hadamard and de la Vallée Poussin, 1896) using complex analysis of the Riemann zeta function. An elementary consequence: the "density" of primes near $x$ is about $1/\ln x$, so a random integer near $x$ is prime with probability $\approx 1/\ln x$.

Q: Why do cryptographers care about the prime counting function?
A: Because RSA requires generating large random primes (say $1024$ bits). The prime density $1/\ln x$ predicts the expected number of random tests before finding a prime: for $1024$-bit numbers, $\ln(2^{1024}) \approx 710$, so roughly $710$ random candidates suffice on average. Feasible.

## 3.9 Primes of Special Forms

C: A [Mersenne number] is an integer of the form $M_p = 2^p - 1$; it is a [Mersenne prime] when $M_p$ is prime.

Q: Why do we require $p$ prime in $M_p = 2^p - 1$ when searching for Mersenne primes?
A: Because if $p = ab$ is composite, then $2^a - 1 \mid 2^{ab} - 1 = M_p$, so $M_p$ is composite too. So only prime exponents $p$ can yield Mersenne primes. Note: $p$ prime doesn't guarantee $M_p$ prime (e.g., $M_{11} = 2047 = 23 \cdot 89$).

C: A [Fermat prime] is a prime of the form $F_n = 2^{2^n} + 1$.

Q: Which Fermat numbers $F_n = 2^{2^n} + 1$ are known to be prime?
A: Only the first five: $F_0, \dots, F_4 = 3, 5, 17, 257, 65537$. No other Fermat primes are known — and it is conjectured there are none.

Q: What did Euler show about the Fermat number $F_5$?
A: $F_5 = 4{,}294{,}967{,}297 = 641 \cdot 6{,}700{,}417$ is composite (Euler, 1732) — the first Fermat number shown not to be prime.

Q: How do Fermat primes relate to compass-and-straightedge constructions?
A: Gauss showed that a regular $n$-gon is constructible by compass and straightedge iff $n$ is a power of $2$ times distinct Fermat primes.

## 3.10 Twin Primes and Open Problems

C: [Twin primes] are pairs of primes differing by $2$: $(3,5), (5,7), (11,13), (17,19), \dots$

Q: What is the twin prime conjecture?
A: That infinitely many twin prime pairs exist. Unproven. Yitang Zhang (2013) proved there are infinitely many prime pairs $(p, q)$ with $q - p \leq 70{,}000{,}000$; follow-up work reduced the bound to $246$. Closing the gap to $2$ remains open.

Q: What is [Goldbach's conjecture]?
A: That every even integer $\geq 4$ is a sum of two primes. Verified computationally to $4 \cdot 10^{18}$; unproven in general. One of the oldest unsolved problems in number theory (Goldbach, 1742).

## 3.11 Computing Prime Factorizations

Q: Why is prime factorization believed to be computationally hard?
A: Because no algorithm is known that factors an arbitrary $n$ in polynomial time in $\log n$. The fastest classical algorithm (general number field sieve) runs in sub-exponential time $\exp(O((\log n)^{1/3} (\log \log n)^{2/3}))$. Shor's quantum algorithm factors in polynomial time — but quantum hardware at cryptographic scales doesn't yet exist.

Q: What is [trial division], and when is it practical?
A: Try candidate divisors $2, 3, 5, 7, \dots$ up to $\sqrt{n}$, recording any that divide $n$. Running time $O(\sqrt{n})$ — only practical for $n$ up to $\sim 10^{12}$ or so. Fast for small factors but infeasible for cryptographic-size numbers.

## 3.12 Why Cryptography Relies on Factoring

Q: How does prime factorization relate to RSA security?
A: RSA uses a modulus $n = pq$ for two large primes $p, q$. Encryption uses a public exponent; decryption requires $\varphi(n) = (p-1)(q-1)$, which requires knowing $p$ and $q$. Factoring $n$ would reveal $p, q$ and break the system. The presumed hardness of factoring $\approx 2048$-bit integers underpins RSA's security.

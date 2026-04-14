+++
order = 10
subject = "Math"
tags = ["math", "number-theory", "primality", "factoring", "miller-rabin", "pollard", "aks"]
+++

# Number Theory — Primality Testing and Factoring

## 10.1 Two Different Problems

Q: How does primality testing differ from factoring?
A: [Primality testing] asks whether $n$ is prime — a yes/no decision. [Factoring] asks for the prime factorization of $n$ — producing the actual primes. Primality is in $\textsf{P}$ (AKS, 2002); factoring is believed to be harder and has no known polynomial algorithm. The asymmetry is essential for RSA: easy to generate primes, hard to factor their products.

## 10.2 Trial Division

Q: What's the simplest primality test?
A: [Trial division]: test divisibility by $2, 3, 5, 7, \dots$ up to $\sqrt{n}$. If none divides, $n$ is prime. Running time $O(\sqrt{n} / \ln n)$ with the prime-counting function — feasible for $n \leq 10^{12}$ but hopeless for cryptographic-size numbers.

## 10.3 Fermat Primality Test

Q: How does Fermat's little theorem give a primality test?
A: If $n$ is prime and $\gcd(a, n) = 1$, then $a^{n-1} \equiv 1 \pmod n$. Test: pick $a$; if $a^{n-1} \not\equiv 1 \pmod n$, then $n$ is composite. If the congruence DOES hold, $n$ might still be composite ("Fermat liar"). Repeat with different $a$ to gain confidence.

Q: What are [Carmichael numbers]?
A: Composites $n$ satisfying $a^{n-1} \equiv 1 \pmod n$ for EVERY $a$ coprime to $n$. They fool Fermat's test for every coprime witness — infinitely many exist (Alford–Granville–Pomerance, 1994). The smallest is $561 = 3 \cdot 11 \cdot 17$. Because of them, Fermat's test can be deceived, motivating stronger tests like Miller–Rabin.

## 10.4 Miller–Rabin Test

Q: What is the key insight of the [Miller–Rabin primality test]?
A: That primes satisfy a STRONGER condition than Fermat: if $n$ is an odd prime and $n - 1 = 2^s d$ with $d$ odd, then for any $a$ coprime to $n$, either $a^d \equiv 1 \pmod n$ or $a^{2^r d} \equiv -1 \pmod n$ for some $0 \leq r < s$. Composite $n$ (including Carmichaels) fail this for at least $3/4$ of witnesses.

Q: Why is Miller–Rabin practical for cryptographic primality generation?
A: Because each round catches composites with probability $\geq 3/4$; $k$ rounds give error probability $\leq 4^{-k}$. With $k = 40$, error $\leq 10^{-24}$ — indistinguishable from zero. Total cost: $k$ modular exponentiations, each $O((\log n)^3)$ bits. For $2048$-bit $n$, a few milliseconds.

P: Apply one round of Miller–Rabin to $n = 561$ with base $a = 2$.
S:
**IDENTIFY**: Factor $n - 1 = 2^s d$, compute $a^d, a^{2d}, \dots, a^{2^{s-1} d}$.

**PLAN**: $560 = 2^4 \cdot 35$, so $s = 4$, $d = 35$. Compute $2^{35} \bmod 561$ and square up.

**EXECUTE**:
1. $2^{35} \bmod 561$: compute by repeated squaring → $263$.
2. $263^2 = 69169 \bmod 561 = 166$.
3. $166^2 = 27556 \bmod 561 = 67$.
4. $67^2 = 4489 \bmod 561 = 1$.
5. Expected: some term in the sequence $\equiv 1$ OR $-1$ before reaching $1$. We got $1$ at the last step WITHOUT seeing $-1 = 560$ first. So the test declares $n$ composite.

**EVALUATE**: $561 = 3 \cdot 11 \cdot 17$, composite as declared. Miller–Rabin catches Carmichael numbers because it requires stronger behavior than just $a^{n-1} \equiv 1$.

## 10.5 AKS Primality Test

Q: What made the [AKS test] (Agrawal–Kayal–Saxena, 2002) a landmark?
A: It was the FIRST deterministic, polynomial-time, unconditional primality test. Complexity: $\tilde{O}(\log^6 n)$, later improved to $\tilde{O}(\log^{3+\epsilon} n)$. Proved $\textsf{PRIMES} \in \textsf{P}$. In practice, Miller–Rabin (probabilistic) remains faster for cryptographic use, but AKS settled a long-open complexity question.

## 10.6 Pollard's Rho for Factoring

Q: How does [Pollard's rho algorithm] factor $n$?
A: Generate a pseudorandom sequence $x_{i+1} = x_i^2 + c \bmod n$. By the birthday paradox, collisions mod the smallest prime factor $p$ appear after $\sim \sqrt{p}$ steps. Track differences and compute $\gcd(x_i - x_j, n)$ using Floyd's cycle detection; eventually get a nontrivial factor. Expected time: $O(n^{1/4})$. Practical for factors up to $\sim 10^{18}$.

## 10.7 Pollard's $p - 1$ Method

Q: When does [Pollard's $p - 1$ method] factor $n$ efficiently?
A: When $n$ has a prime factor $p$ with $p - 1$ "smooth" (all prime factors small). Idea: pick $a$ coprime to $n$; compute $a^{B!} \bmod n$ for some bound $B$; take $\gcd(a^{B!} - 1, n)$. If $p - 1 \mid B!$, then $a^{B!} \equiv 1 \pmod p$, so $p \mid \gcd$. A reason to pick "safe primes" (where $p - 1 = 2q$ for prime $q$) in RSA — avoid smoothness of $p - 1$.

## 10.8 Quadratic Sieve

Q: What is the high-level idea of the [quadratic sieve] factoring algorithm?
A: Find integers $x$ with $x^2 \equiv y^2 \pmod n$ but $x \not\equiv \pm y$; then $\gcd(x - y, n)$ is a nontrivial factor of $n$. Generate such relations by sieving for integers whose squares mod $n$ are smooth (factor over a prime base); combine them via linear algebra over $\mathbb{F}_2$. Sub-exponential running time $\exp(O(\sqrt{\log n \log \log n}))$.

## 10.9 General Number Field Sieve (GNFS)

Q: What is the current fastest factoring algorithm for large general integers?
A: The [General Number Field Sieve] (GNFS), running in $\exp\left(\left(\sqrt[3]{64/9} + o(1)\right)(\log n)^{1/3} (\log \log n)^{2/3}\right)$. Sub-exponential but super-polynomial. It's what threatens RSA at $\sim 2000$-bit sizes — RSA-768 (232 decimal digits) was factored by GNFS in 2009; RSA-2048 remains out of reach of current computing power.

## 10.10 Shor's Quantum Algorithm

Q: Why does [Shor's algorithm] threaten RSA?
A: Because Shor factors $n$ in $O((\log n)^3)$ time on a quantum computer. It works by reducing factoring to period-finding, which QFT (quantum Fourier transform) solves efficiently. A sufficiently large, noise-resilient quantum computer would break RSA and Diffie–Hellman. As of 2025, only toy-scale factorings have been demonstrated; cryptographers are migrating to post-quantum schemes (lattice-based, hash-based).

## 10.11 Generating RSA Primes

Q: How does RSA key generation produce a large prime?
A: (i) Pick a random $k$-bit integer $n$ with top and bottom bit set (making it odd and of full bit-length). (ii) Filter small-prime divisibility quickly (trial division by primes up to $\sim 10^4$). (iii) Run Miller–Rabin for $40$ iterations. (iv) If passed, emit $n$ as prime; else go to (i). Expected candidates before success: $\sim \ln(2^k)$, a few hundred for $2048$-bit RSA. Total time: seconds.

## 10.12 Practical Security Parameter Sizes

Q: What are current recommended key sizes for symmetric-key-equivalent security?
A: For $128$-bit symmetric security (comparable to AES-$128$): RSA/DH $\geq 3072$ bits; elliptic-curve $\geq 256$ bits. For $256$-bit security: RSA/DH $\geq 15360$ bits; ECC $\geq 512$ bits. ECC's much smaller keys are why it dominates modern TLS and mobile crypto — the underlying discrete log in elliptic curve groups is harder per-bit than in $\mathbb{F}_p^{\times}$.

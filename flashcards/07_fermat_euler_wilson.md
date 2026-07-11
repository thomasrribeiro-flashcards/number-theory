+++
order = 7
subject = "mathematics"
tags = ["math", "number-theory", "fermat", "euler", "wilson", "totient", "multiplicative"]
+++

# Number Theory — Fermat, Euler, and Wilson

## 7.1 Fermat's Little Theorem

C: [Fermat's little theorem]: if $p$ is prime and $\gcd(a, p) = 1$, then $a^{p-1} \equiv 1 \pmod p$.

Q: Why does Fermat's little theorem hold?
A: Multiplication by $a$ permutes the nonzero residues mod $p$ (since $a$ is invertible mod $p$). So $\{a, 2a, \dots, (p-1)a\} \equiv \{1, 2, \dots, p-1\} \pmod p$ as multisets. Multiplying all elements: $a^{p-1} (p-1)! \equiv (p-1)! \pmod p$. Cancel $(p-1)!$ (coprime to $p$): $a^{p-1} \equiv 1 \pmod p$.

Q: What's the weaker form of Fermat's theorem that drops the coprimality hypothesis?
A: $a^p \equiv a \pmod p$ for EVERY integer $a$ (not just those coprime to $p$). Equivalent to the stated form when $\gcd(a, p) = 1$ (multiply by $a$); also holds when $p \mid a$ (both sides $\equiv 0$). This form is useful when $a$ might be $0 \pmod p$.

## 7.2 Proving Fermat via Induction

P: Prove $a^p \equiv a \pmod p$ for all $a \in \mathbb{Z}^+$ and prime $p$, by induction on $a$.
S:
**IDENTIFY**: Induction on $a \geq 1$ for fixed prime $p$.

**PLAN**: Base $a = 1$: trivial. Step: expand $(a+1)^p$ using binomial theorem; show binomial coefficients $\binom{p}{k}$ are $\equiv 0 \pmod p$ for $1 \leq k \leq p - 1$.

**EXECUTE**:
1. Base: $1^p = 1 \equiv 1 \pmod p$ ✓.
2. IH: $a^p \equiv a \pmod p$.
3. $\binom{p}{k} = \frac{p!}{k!(p-k)!}$ has $p$ in the numerator; for $0 < k < p$, $p$ doesn't divide $k!$ or $(p-k)!$, so $p \mid \binom{p}{k}$.
4. $(a+1)^p = \sum_{k=0}^{p} \binom{p}{k} a^{p-k} \equiv a^p + 1 \equiv a + 1 \pmod p$ (using IH).
5. Induction closes.

**EVALUATE**: This proof uses only the binomial theorem and prime-divisor properties — it avoids the permutation-of-residues argument and extends to $a \leq 0$ as well, since $\mathbb{Z}$ is symmetric around $0$.

## 7.3 Euler's Totient Function

C: [Euler's totient] $\varphi(n)$ is the number of integers in $\{1, 2, \dots, n\}$ coprime to $n$.

Q: What is $\varphi(p)$ for prime $p$?
A: $\varphi(p) = p - 1$. All of $1, 2, \dots, p-1$ are coprime to $p$; only $p$ itself shares a factor with $p$. Fermat's theorem is Euler's theorem specialized to this case.

Q: How does $\varphi$ behave on prime powers?
A: $\varphi(p^k) = p^k - p^{k-1} = p^{k-1}(p - 1)$. Among $\{1, \dots, p^k\}$, the multiples of $p$ ($p, 2p, \dots, p^k$) are NOT coprime — there are $p^{k-1}$ of them; the rest are coprime: $p^k - p^{k-1}$.

## 7.4 Multiplicativity of $\varphi$

C: $\varphi$ is a [multiplicative function]: $\varphi(mn) = \varphi(m)\varphi(n)$ whenever $\gcd(m, n) = 1$.

Q: Why is $\varphi$ multiplicative on coprime arguments?
A: By CRT, $\mathbb{Z}/mn\mathbb{Z} \cong \mathbb{Z}/m\mathbb{Z} \times \mathbb{Z}/n\mathbb{Z}$ as rings when $\gcd(m, n) = 1$. Units go to units: the unit group $(\mathbb{Z}/mn\mathbb{Z})^{\times} \cong (\mathbb{Z}/m\mathbb{Z})^{\times} \times (\mathbb{Z}/n\mathbb{Z})^{\times}$. Counting elements on both sides gives $\varphi(mn) = \varphi(m)\varphi(n)$.

Q: What is the closed form for $\varphi(n)$ via prime factorization?
A: $\varphi(n) = n \prod_{p \mid n} \left(1 - \frac{1}{p}\right)$. Proof: write $n = \prod p_i^{e_i}$. Multiplicativity gives $\varphi(n) = \prod \varphi(p_i^{e_i}) = \prod p_i^{e_i - 1}(p_i - 1) = \prod p_i^{e_i} (1 - 1/p_i) = n \prod (1 - 1/p_i)$.

P: Compute $\varphi(360)$.
S:
**IDENTIFY**: Apply the product formula.

**PLAN**: Factor $360$; evaluate.

**EXECUTE**:
1. $360 = 2^3 \cdot 3^2 \cdot 5$.
2. $\varphi(360) = 360 \cdot (1 - 1/2)(1 - 1/3)(1 - 1/5) = 360 \cdot \frac{1}{2} \cdot \frac{2}{3} \cdot \frac{4}{5} = 360 \cdot \frac{8}{30} = 96$.

**EVALUATE**: Cross-check via multiplicativity: $\varphi(8) = 4$, $\varphi(9) = 6$, $\varphi(5) = 4$; product $= 96$ ✓.

## 7.5 Euler's Theorem

C: [Euler's theorem]: if $\gcd(a, n) = 1$, then $a^{\varphi(n)} \equiv 1 \pmod n$.

Q: Why does Euler's theorem generalize Fermat's?
A: For prime $n = p$, $\varphi(p) = p - 1$, so $a^{p - 1} \equiv 1 \pmod p$ when $\gcd(a, p) = 1$ — exactly Fermat. Euler's extends to ALL moduli: composite moduli are handled by the unit-group structure of $\mathbb{Z}/n\mathbb{Z}$.

Q: Why does Euler's theorem hold?
A: Let $U = (\mathbb{Z}/n\mathbb{Z})^{\times}$ be the unit group mod $n$; $|U| = \varphi(n)$. Multiplication by $a$ (invertible) permutes $U$. Applying the same trick as Fermat (multiply all elements, cancel) gives $a^{\varphi(n)} \equiv 1 \pmod n$. In group-theoretic language: every element $a$ of a finite group of order $\varphi(n)$ satisfies $a^{\varphi(n)} = e$ by Lagrange's theorem.

## 7.6 Reducing Exponents mod $\varphi(n)$

Q: How does Euler's theorem let you reduce exponents?
A: If $\gcd(a, n) = 1$, then $a^k \equiv a^{k \bmod \varphi(n)} \pmod n$. Because $a^k = a^{q \varphi(n) + r} = (a^{\varphi(n)})^q \cdot a^r \equiv 1^q \cdot a^r = a^r$. Crucial for making $a^k$ tractable when $k$ is astronomically large (e.g., in RSA).

P: Compute $3^{1000} \bmod 100$.
S:
**IDENTIFY**: Reduce exponent via Euler's theorem.

**PLAN**: $\gcd(3, 100) = 1$; compute $\varphi(100)$; reduce $1000$ mod $\varphi(100)$.

**EXECUTE**:
1. $100 = 4 \cdot 25$, so $\varphi(100) = \varphi(4)\varphi(25) = 2 \cdot 20 = 40$.
2. $1000 = 25 \cdot 40$, so $1000 \equiv 0 \pmod{40}$.
3. $3^{1000} \equiv 3^0 = 1 \pmod{100}$.

**EVALUATE**: Cross-check via CRT: mod $4$, $3 \equiv -1$ so $3^{1000} \equiv 1$; mod $25$, $\varphi(25) = 20 \mid 1000$ so $3^{1000} \equiv 1$; both agree with $1 \pmod{100}$ ✓. Naively this would need $\sim 1000$ modular multiplications (or $\sim 10$ via repeated squaring).

## 7.7 Wilson's Theorem

C: [Wilson's theorem]: an integer $p > 1$ is prime iff $(p - 1)! \equiv -1 \pmod p$.

Q: Why is Wilson's theorem true for primes?
A: For prime $p$, the nonzero residues $1, 2, \dots, p-1$ form a group under multiplication mod $p$. Each element has a unique inverse. Pair each $a$ with $a^{-1}$; $a = a^{-1}$ iff $a^2 = 1$ iff $a = \pm 1$. So in $(p-1)!$, all factors pair up to $1$ except the self-inverses: $1$ and $p - 1 = -1$. Hence $(p-1)! \equiv 1 \cdot (-1) = -1 \pmod p$.

Q: Why does Wilson's theorem fail for composite $n$?
A: For composite $n > 4$, $(n - 1)!$ contains $n$ as a product of smaller factors (e.g., $n = ab$ with $a, b \leq n/2$), so $n \mid (n-1)!$, giving $(n-1)! \equiv 0 \pmod n$, not $-1$. The $n = 4$ case is a special boundary: $(4-1)! = 6 \equiv 2 \pmod 4$, also not $-1$.

Q: Why isn't Wilson's theorem used as a primality test?
A: Because computing $(n-1)! \bmod n$ takes $\Theta(n)$ multiplications — exponential in $\log n$. Other tests (Miller–Rabin, AKS) are vastly faster. Wilson's theorem is a beautiful theoretical criterion but algorithmically useless.

## 7.8 The Group $(\mathbb{Z}/n\mathbb{Z})^{\times}$

C: The [unit group] $(\mathbb{Z}/n\mathbb{Z})^{\times}$ is the multiplicative group of integers coprime to $n$, mod $n$, under multiplication mod $n$. Order $\varphi(n)$.

Q: For which $n$ is $(\mathbb{Z}/n\mathbb{Z})^{\times}$ cyclic?
A: Iff $n = 1, 2, 4, p^k$, or $2 p^k$ for an odd prime $p$. For these moduli, there exists a generator (primitive root). For other composite moduli (e.g., $n = 8, 12, 15$), the group is not cyclic but still abelian — structured as a product of cyclic groups by the CRT decomposition. Chapter 8 develops this.

## 7.9 Applications to RSA

Q: How does RSA use Fermat/Euler's theorem?
A: RSA chooses $n = pq$ and $e$ with $\gcd(e, \varphi(n)) = 1$. Public key: $(n, e)$; private key: $d \equiv e^{-1} \pmod{\varphi(n)}$. Encryption: $c = m^e \bmod n$. Decryption: $m = c^d \bmod n$. Correctness: $c^d = m^{ed} = m^{1 + k\varphi(n)} = m \cdot (m^{\varphi(n)})^k \equiv m \cdot 1^k = m \pmod n$ (when $\gcd(m, n) = 1$; handle edge case separately).

Q: Why is it essential that $\varphi(n)$ is hard to compute for RSA's security?
A: Because knowing $\varphi(n) = (p-1)(q-1)$ is equivalent to factoring $n = pq$: from $n$ and $\varphi(n)$ you can solve for $p, q$ via a quadratic. So recovering $\varphi(n)$ breaks RSA. Factoring is believed hard, so $\varphi(n)$ is believed hard — security reduces to the factoring assumption.

## 7.10 Carmichael Function

C: The [Carmichael function] $\lambda(n)$ is the smallest positive integer $m$ such that $a^m \equiv 1 \pmod n$ for every $a$ coprime to $n$.

Q: How does $\lambda(n)$ compare to $\varphi(n)$?
A: $\lambda(n) \mid \varphi(n)$ always. Often $\lambda(n) < \varphi(n)$ for composite $n$. For $n = 8$: $\varphi(8) = 4$, but $\lambda(8) = 2$ (every odd $a$ satisfies $a^2 \equiv 1 \pmod 8$). $\lambda(n) = \text{lcm}(\lambda(p_1^{e_1}), \dots, \lambda(p_k^{e_k}))$; for odd prime powers $\lambda(p^k) = \varphi(p^k)$, but $\lambda(2^k) = 2^{k-2}$ for $k \geq 3$.

Q: Why do modern RSA implementations use $\lambda(n)$ instead of $\varphi(n)$?
A: Because $\lambda(n)$ can be smaller than $\varphi(n)$, letting you choose a smaller private exponent $d$ (satisfying $ed \equiv 1 \pmod{\lambda(n)}$), speeding up decryption. PKCS #1 v2 recommends $\lambda(n)$ over $\varphi(n)$. Both give valid RSA keys; $\lambda$ is tighter.

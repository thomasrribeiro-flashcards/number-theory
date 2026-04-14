+++
order = 4
subject = "Math"
tags = ["math", "number-theory", "congruences", "modular-arithmetic", "residues"]
+++

# Number Theory — Congruences and Modular Arithmetic

## 4.1 Defining Congruence

C: For $n \geq 1$ and integers $a, b$: $a \equiv b \pmod n$ (read "a is congruent to b modulo n") iff $n \mid (a - b)$.

Q: Why is congruence mod $n$ an equivalence relation on $\mathbb{Z}$?
A: (i) Reflexive: $n \mid 0 = a - a$, so $a \equiv a$. (ii) Symmetric: $n \mid (a - b) \Rightarrow n \mid (b - a)$. (iii) Transitive: if $n \mid (a - b)$ and $n \mid (b - c)$, then $n \mid ((a - b) + (b - c)) = (a - c)$. The three axioms hold, so $\equiv \pmod n$ partitions $\mathbb{Z}$ into $n$ equivalence classes.

Q: How are congruences related to the division algorithm?
A: $a \equiv b \pmod n$ iff $a$ and $b$ have the same remainder when divided by $n$. The residue classes mod $n$ are exactly the fibers of the map "take remainder mod $n$" — so $\mathbb{Z}/n\mathbb{Z}$ is indexed by $\{0, 1, \dots, n-1\}$, the set of [least non-negative residues].

## 4.2 Arithmetic Mod $n$

Q: Why do addition and multiplication respect congruence?
A: If $a \equiv a' \pmod n$ and $b \equiv b' \pmod n$, then $a + b \equiv a' + b' \pmod n$ and $ab \equiv a'b' \pmod n$. Proof: write $a - a' = nk$ and $b - b' = nl$; then $(a + b) - (a' + b') = n(k + l)$ and $ab - a'b' = a(b - b') + b'(a - a') = a \cdot nl + b' \cdot nk = n(al + b'k)$.

Q: Why can't you ALWAYS cancel in congruences?
A: $ac \equiv bc \pmod n$ does NOT always imply $a \equiv b \pmod n$. Example: $2 \cdot 3 \equiv 4 \cdot 3 \pmod 6$ ($6 \equiv 12$), but $2 \not\equiv 4 \pmod 6$. Cancellation works only when $\gcd(c, n) = 1$; otherwise, the modulus must be adjusted: $a \equiv b \pmod{n / \gcd(c, n)}$.

## 4.3 Complete Residue Systems

C: A [complete residue system mod $n$] is a set of $n$ integers, one from each congruence class; the standard choice is $\{0, 1, 2, \dots, n-1\}$.

Q: Why are other complete residue systems sometimes preferred?
A: Because $\{-\lfloor n/2 \rfloor, \dots, \lfloor n/2 \rfloor\}$ (the "balanced" system) is symmetric around $0$ — useful for minimizing the magnitude of residues in computations, especially floating-point modular reduction. For $n = 7$: balanced gives $\{-3, -2, -1, 0, 1, 2, 3\}$; standard gives $\{0, 1, \dots, 6\}$.

## 4.4 Reduced Residue Systems

C: A [reduced residue system mod $n$] is a set of $\varphi(n)$ integers, one from each coprime-to-$n$ residue class.

Q: Why does $\mathbb{Z}/n\mathbb{Z}$ have exactly $\varphi(n)$ invertible elements?
A: Because $a \in \mathbb{Z}/n\mathbb{Z}$ has a multiplicative inverse iff $\gcd(a, n) = 1$ (by Bézout). The count of $a \in \{1, \dots, n\}$ coprime to $n$ is exactly $\varphi(n)$ by definition of Euler's totient. So $(\mathbb{Z}/n\mathbb{Z})^{\times}$ — the multiplicative group of units — has order $\varphi(n)$.

## 4.5 Modular Inverses

Q: How do you compute $a^{-1} \bmod n$ when $\gcd(a, n) = 1$?
A: Use the extended Euclidean algorithm: find $x, y$ with $ax + ny = 1$. Then $ax \equiv 1 \pmod n$, so $a^{-1} \equiv x \pmod n$ (reduce to standard residue if needed). Running time $O(\log n)$ divisions — fast.

P: Compute $7^{-1} \bmod 26$.
S:
**IDENTIFY**: Modular inverse — extended Euclidean.

**PLAN**: Find $x$ with $7x \equiv 1 \pmod{26}$.

**EXECUTE**:
1. $26 = 3 \cdot 7 + 5$.
2. $7 = 1 \cdot 5 + 2$.
3. $5 = 2 \cdot 2 + 1$.
4. Back-substitute:
   - $1 = 5 - 2 \cdot 2$.
   - $= 5 - 2(7 - 5) = 3 \cdot 5 - 2 \cdot 7$.
   - $= 3(26 - 3 \cdot 7) - 2 \cdot 7 = 3 \cdot 26 - 11 \cdot 7$.
5. So $-11 \cdot 7 \equiv 1 \pmod{26}$, hence $7^{-1} \equiv -11 \equiv 15 \pmod{26}$.

**EVALUATE**: Check: $7 \cdot 15 = 105 = 4 \cdot 26 + 1$ ✓. Inverse exists because $\gcd(7, 26) = 1$.

## 4.6 Solving $ax \equiv b \pmod n$

Q: When does $ax \equiv b \pmod n$ have a solution?
A: Iff $\gcd(a, n) \mid b$. If $d = \gcd(a, n) \nmid b$: no solution (LHS always divisible by $d$, RHS not). If $d \mid b$: solutions exist. Number of solutions mod $n$: exactly $d = \gcd(a, n)$.

Q: How do you find all solutions when $\gcd(a, n) = d$ and $d \mid b$?
A: Divide through: $(a/d) x \equiv (b/d) \pmod{n/d}$. Now $\gcd(a/d, n/d) = 1$, so unique solution $x_0 \pmod{n/d}$. The $d$ solutions mod $n$ are $x_0, x_0 + n/d, x_0 + 2n/d, \dots, x_0 + (d-1)n/d$.

P: Solve $6x \equiv 4 \pmod{10}$.
S:
**IDENTIFY**: Linear congruence; check solvability, reduce.

**PLAN**: Compute $\gcd(6, 10) = 2$; check $2 \mid 4$ ✓; divide.

**EXECUTE**:
1. Divide by $2$: $3x \equiv 2 \pmod 5$.
2. $3^{-1} \equiv 2 \pmod 5$ (since $3 \cdot 2 = 6 \equiv 1$).
3. $x \equiv 2 \cdot 2 = 4 \pmod 5$.
4. Solutions mod $10$: $x = 4$ and $x = 4 + 5 = 9$.

**EVALUATE**: Check: $6 \cdot 4 = 24 \equiv 4 \pmod{10}$ ✓; $6 \cdot 9 = 54 \equiv 4 \pmod{10}$ ✓. Two solutions, matching $\gcd(6, 10) = 2$.

## 4.7 Modular Exponentiation by Repeated Squaring

Q: How do you compute $a^b \bmod n$ efficiently?
A: By [repeated squaring]: express $b$ in binary $b = b_{k-1} \cdots b_1 b_0$; compute successive squares $a, a^2, a^4, \dots, a^{2^{k-1}}$ mod $n$ by squaring each from the last; multiply together those $a^{2^i}$ where $b_i = 1$. Running time $O(\log b)$ multiplications and squarings — essential for RSA.

P: Compute $5^{117} \bmod 13$ using repeated squaring.
S:
**IDENTIFY**: Modular exponentiation.

**PLAN**: $117 = 64 + 32 + 16 + 4 + 1 = 1110101_2$; compute $5^{2^i}$ mod $13$ up to $2^6 = 64$.

**EXECUTE**:
- $5^1 \equiv 5$.
- $5^2 = 25 \equiv 12 \equiv -1$.
- $5^4 \equiv (-1)^2 = 1$.
- $5^8 \equiv 1$; $5^{16} \equiv 1$; $5^{32} \equiv 1$; $5^{64} \equiv 1$.
- $5^{117} = 5^{64} \cdot 5^{32} \cdot 5^{16} \cdot 5^4 \cdot 5^1 \equiv 1 \cdot 1 \cdot 1 \cdot 1 \cdot 5 = 5 \pmod{13}$.

**EVALUATE**: Notice $5^4 \equiv 1$, so the powers of $5$ cycle with period $4$. $117 = 4 \cdot 29 + 1$, so $5^{117} \equiv 5^1 = 5$ ✓. Fermat's little theorem gives the shortcut once $5^{12} \equiv 1 \pmod{13}$ is known.

## 4.8 Modular Arithmetic in Cryptographic Sizes

Q: Why is arithmetic modulo a $2048$-bit integer still feasible despite huge numbers?
A: Because addition, multiplication, and division mod $n$ cost $O((\log n)^k)$ time for small $k$ (quadratic or near-linear with FFT-based multiplication). Exponentiation is $O((\log b)(\log n)^2)$. For $\log n \approx 2000$, one RSA operation takes $\sim 10^9$ bit operations — milliseconds on modern hardware.

## 4.9 Canceling with Care

Q: Under what condition can you divide both sides of $ac \equiv bc \pmod n$ by $c$?
A: When $\gcd(c, n) = 1$: then $c$ is invertible mod $n$, and multiplying both sides by $c^{-1}$ gives $a \equiv b \pmod n$. If $\gcd(c, n) = d > 1$, cancellation is possible only after reducing the modulus: $a \equiv b \pmod{n/d}$.

Q: Why does this cancellation asymmetry not happen in $\mathbb{Z}$ itself?
A: Because $\mathbb{Z}$ has no zero divisors — $ac = bc$ with $c \neq 0$ forces $a = b$. But $\mathbb{Z}/n\mathbb{Z}$ has zero divisors when $n$ is composite (e.g., $2 \cdot 3 \equiv 0 \pmod 6$ with neither factor zero). The zero-divisor structure of a composite modulus is what breaks naive cancellation.

## 4.10 Fields and Composite Moduli

Q: When is $\mathbb{Z}/n\mathbb{Z}$ a field?
A: Iff $n$ is prime. Proof: if $n = p$ prime, every nonzero element is coprime to $p$, so invertible — making $\mathbb{Z}/p\mathbb{Z}$ a field (written $\mathbb{F}_p$). If $n$ is composite, some element (e.g., a prime factor of $n$) is not invertible, so no field.

Q: Why do cryptographic protocols often work in $\mathbb{F}_p$ or $\mathbb{F}_p^{\times}$?
A: Because field structure makes arithmetic clean: every nonzero element has a unique inverse, cancellation is safe, polynomial equations have at most degree-many roots. Many protocols (Diffie–Hellman, ElGamal, Schnorr) operate in multiplicative groups $\mathbb{F}_p^{\times}$ where discrete log is presumed hard.

## 4.11 Applications: Checksums and Hashing

Q: Why do checksums use modular arithmetic?
A: Because "$\bmod n$" folds arbitrary-length data into a fixed-size fingerprint while spreading small input changes across the output. Internet checksums use addition mod $2^{16}$; CRCs use polynomial arithmetic over $\mathbb{F}_2$. Both rely on modular structure to achieve error-detection properties efficiently.

## 4.12 Tip: Reduce Early, Reduce Often

Q: Why do implementers reduce intermediate results mod $n$ rather than at the end?
A: To keep intermediate values small. If $a, b < n$, then $a + b < 2n$ and $ab < n^2$ — both can be reduced mod $n$ immediately to bound their size. Deferring reduction risks overflow on fixed-size integers and balloons operand sizes unnecessarily. Arithmetic mod $n$ should propagate modularity step-by-step.

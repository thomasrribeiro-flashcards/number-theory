+++
order = 6
subject = "Math"
tags = ["math", "number-theory", "crt", "chinese-remainder", "systems", "modular"]
+++

# Number Theory — The Chinese Remainder Theorem

## 6.1 The Setup

Q: What problem does the Chinese Remainder Theorem solve?
A: Given a system of congruences $x \equiv a_1 \pmod{m_1}, \dots, x \equiv a_k \pmod{m_k}$, it describes WHEN a common $x$ exists and provides the UNIQUE solution modulo $\prod m_i$ when the $m_i$ are pairwise coprime. The theorem dates to Sun Tzu (3rd century) for astronomical calendar problems.

## 6.2 The Theorem

C: [Chinese Remainder Theorem]: if $m_1, m_2, \dots, m_k$ are pairwise coprime positive integers and $a_1, a_2, \dots, a_k$ are any integers, then the system $x \equiv a_i \pmod{m_i}$ for $i = 1, \dots, k$ has a unique solution modulo $M = m_1 m_2 \cdots m_k$.

Q: Why does CRT require pairwise coprime moduli?
A: Because two non-coprime moduli can give inconsistent conditions (e.g., $x \equiv 1 \pmod 4$ and $x \equiv 2 \pmod 6$ — first forces $x$ odd, second forces $x$ even mod $2$, contradiction). With pairwise coprime moduli, residues mod different $m_i$ are "independent" and can be freely chosen.

## 6.3 The Ring-Theoretic Statement

Q: How does CRT read in ring-theoretic language?
A: If $m_1, \dots, m_k$ are pairwise coprime, the map $\mathbb{Z}/M\mathbb{Z} \to \mathbb{Z}/m_1\mathbb{Z} \times \cdots \times \mathbb{Z}/m_k\mathbb{Z}$, $x \bmod M \mapsto (x \bmod m_1, \dots, x \bmod m_k)$, is a RING ISOMORPHISM. That is, the modular rings decompose into a product — addition, multiplication, and inverses compute component-wise. One isomorphism, many algorithmic consequences.

## 6.4 The Constructive Proof

Q: How do you construct the CRT solution?
A: Let $M = \prod m_i$ and $M_i = M / m_i$. Compute $y_i \equiv M_i^{-1} \pmod{m_i}$ (exists because $\gcd(M_i, m_i) = 1$ by pairwise coprimality). Then $x \equiv \sum_{i=1}^{k} a_i M_i y_i \pmod M$. Each term $a_i M_i y_i$ is $\equiv a_i \pmod{m_i}$ and $\equiv 0 \pmod{m_j}$ for $j \neq i$ — so the sum has the right residue at every modulus.

P: Solve the system $x \equiv 1 \pmod 5$, $x \equiv 2 \pmod 7$, $x \equiv 3 \pmod{11}$.
S:
**IDENTIFY**: CRT with three coprime moduli.

**PLAN**: $M = 5 \cdot 7 \cdot 11 = 385$; $M_i = M/m_i$; invert; combine.

**EXECUTE**:
1. $M_1 = 77$; $77 \equiv 2 \pmod 5$; $2^{-1} \equiv 3 \pmod 5$; $y_1 = 3$. Contribution: $1 \cdot 77 \cdot 3 = 231$.
2. $M_2 = 55$; $55 \equiv 6 \pmod 7$; $6^{-1} \equiv 6 \pmod 7$ (since $36 \equiv 1$); $y_2 = 6$. Contribution: $2 \cdot 55 \cdot 6 = 660$.
3. $M_3 = 35$; $35 \equiv 2 \pmod{11}$; $2^{-1} \equiv 6 \pmod{11}$ (since $12 \equiv 1$); $y_3 = 6$. Contribution: $3 \cdot 35 \cdot 6 = 630$.
4. Sum: $231 + 660 + 630 = 1521$; $1521 \bmod 385$: $1521 - 3 \cdot 385 = 1521 - 1155 = 366$.

**EVALUATE**: $x \equiv 366 \pmod{385}$. Verify: $366 = 73 \cdot 5 + 1$ (mod 5 ≡ 1 ✓), $366 = 52 \cdot 7 + 2$ (mod 7 ≡ 2 ✓), $366 = 33 \cdot 11 + 3$ (mod 11 ≡ 3 ✓).

## 6.5 Iterative CRT (Pairwise Method)

Q: How do you solve CRT systems without the "bulk" formula?
A: Iteratively: solve the first two congruences for $x \pmod{m_1 m_2}$, then combine with the third for $x \pmod{m_1 m_2 m_3}$, and so on. Each step solves $x \equiv a_i \pmod{m_i}$ and $x \equiv c \pmod{M'}$ (current combined answer) — a two-modulus CRT solved by substitution and Bézout. More memory-friendly for large $k$.

## 6.6 Non-Coprime Moduli

Q: What if the moduli aren't pairwise coprime — can CRT be salvaged?
A: Yes, with a [consistency condition]. $x \equiv a \pmod m$ and $x \equiv b \pmod n$ has a solution iff $a \equiv b \pmod{\gcd(m, n)}$. If consistent, the combined modulus is $\text{lcm}(m, n)$; if not, no solution. CRT extends by solving pairs with this check and promoting the compatible modulus.

## 6.7 CRT and Modular Exponentiation

Q: Why is CRT used to speed up RSA decryption?
A: Because decryption requires computing $c^d \bmod n$ where $n = pq$. By CRT, this is equivalent to computing $c^d \bmod p$ and $c^d \bmod q$ separately, then combining. Each sub-computation uses exponents of half the bit-length and moduli of half the size, giving a $\sim 4\times$ speedup. RSA implementations store CRT-precomputed parameters ($p, q, d \bmod (p-1), d \bmod (q-1)$, and $p^{-1} \bmod q$) for this purpose.

Q: What's the Fermat shortcut when reducing exponents mod $p$?
A: Since $c^{p-1} \equiv 1 \pmod p$ (for $\gcd(c, p) = 1$), we have $c^d \equiv c^{d \bmod (p-1)} \pmod p$. So RSA's $c^d \bmod p$ reduces to $c^{d_p} \bmod p$ with $d_p = d \bmod (p-1)$ — a much smaller exponent. Combined with CRT, this makes RSA decryption practical at $4096$-bit sizes.

## 6.8 CRT and Large-Integer Multiplication

Q: How does CRT enable parallel arithmetic on large integers?
A: Choose distinct primes $p_1, \dots, p_k$ with product $M$ exceeding the answer size. Represent $a$ by its residues $(a \bmod p_1, \dots, a \bmod p_k)$. Addition, subtraction, multiplication are done component-wise — each mod a small prime, fitting in machine words. Reconstruct via CRT only at the end. This [residue number system] powers some FFT-based big-number algorithms.

## 6.9 CRT in Secret Sharing

Q: How does CRT underpin a threshold secret sharing scheme?
A: In [Mignotte's scheme], choose pairwise coprime moduli $m_1 < m_2 < \dots < m_n$ with the property that the product of any $t$ (threshold) exceeds the secret $S$ but any $t - 1$ product is below $S$. Distribute $s_i = S \bmod m_i$ as share $i$. Any $t$ shares uniquely determine $S$ via CRT; any fewer leave $S$ ambiguous within a huge range. Information-theoretic secrecy via CRT structure.

## 6.10 CRT and Polynomial Interpolation

Q: How is Lagrange polynomial interpolation the "polynomial CRT"?
A: Given distinct $x_1, \dots, x_k$ and values $y_1, \dots, y_k$, seek a polynomial $P$ with $P(x_i) = y_i$. This is equivalent to $P(x) \equiv y_i \pmod{x - x_i}$ — $k$ polynomial congruences with pairwise coprime moduli. Lagrange's formula is the CRT construction in the polynomial ring $\mathbb{F}[x]$: same structure, different ring.

## 6.11 Pitfalls

Q: What's a common CRT mistake?
A: Forgetting that the modulus in CRT's answer is the PRODUCT $M$, not the sum, and that "unique" means unique modulo $M$ — infinitely many integer solutions exist. Also: mistakenly applying CRT to non-coprime moduli, which requires extra care (see §6.6).

## 6.12 Summary of Uses

Q: Name five algorithmic uses of the Chinese Remainder Theorem.
A: (i) RSA decryption speedup. (ii) Large-integer arithmetic via residue number systems. (iii) Parallel modular computation. (iv) Threshold secret sharing (Mignotte, Asmuth–Bloom). (v) Fast Fourier Transform over $\mathbb{Z}/N\mathbb{Z}$ via CRT decomposition into prime-modulus FFTs. A single theorem, many layers of algorithmic leverage.

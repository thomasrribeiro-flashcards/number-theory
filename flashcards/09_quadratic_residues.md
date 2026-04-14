+++
order = 9
subject = "Math"
tags = ["math", "number-theory", "quadratic-residues", "legendre", "jacobi", "reciprocity"]
+++

# Number Theory — Quadratic Residues and Reciprocity

## 9.1 Quadratic Residues

C: For an odd prime $p$ and $a$ coprime to $p$, $a$ is a [quadratic residue] mod $p$ (QR) if $x^2 \equiv a \pmod p$ has a solution; otherwise, $a$ is a [quadratic non-residue] (QNR).

Q: Why do we exclude the case $\gcd(a, p) \neq 1$?
A: Because $0$ has a trivial square root ($0 = 0^2$), but it's neither a unit nor useful in the multiplicative theory. For $p \mid a$, the question "is $a$ a QR" is trivially yes via $x = 0$. Restricting to $\gcd(a, p) = 1$ keeps the theory inside the unit group $(\mathbb{Z}/p\mathbb{Z})^{\times}$.

## 9.2 Counting QRs Mod $p$

Q: How many QRs and QNRs are there among $\{1, 2, \dots, p-1\}$?
A: Exactly $(p-1)/2$ of each. The squaring map $x \mapsto x^2 \bmod p$ sends $(\mathbb{Z}/p\mathbb{Z})^{\times}$ onto the QRs; each nonzero square has two preimages ($\pm x$). So the image has size $(p-1)/2$. The complement (QNRs) has size $(p-1)/2$.

## 9.3 The Legendre Symbol

C: The [Legendre symbol] $\left(\frac{a}{p}\right)$ for odd prime $p$ and $\gcd(a, p) = 1$ is $+1$ if $a$ is a QR mod $p$, and $-1$ if $a$ is a QNR mod $p$.

Q: Why is the Legendre symbol multiplicative: $\left(\frac{ab}{p}\right) = \left(\frac{a}{p}\right)\left(\frac{b}{p}\right)$?
A: Because the map $a \mapsto \left(\frac{a}{p}\right)$ is a group homomorphism from $(\mathbb{Z}/p\mathbb{Z})^{\times}$ to $\{\pm 1\}$. Specifically, it's the quotient by the squares subgroup. Product of residue classes = sum of signs — classic homomorphism property.

## 9.4 Euler's Criterion

C: [Euler's criterion]: $\left(\frac{a}{p}\right) \equiv a^{(p-1)/2} \pmod p$.

Q: Why does Euler's criterion hold?
A: By Fermat, $a^{p-1} \equiv 1 \pmod p$, so $(a^{(p-1)/2})^2 \equiv 1$, meaning $a^{(p-1)/2} \equiv \pm 1 \pmod p$. If $a = x^2$ is a QR, $a^{(p-1)/2} = x^{p-1} \equiv 1$. Otherwise $a^{(p-1)/2} \equiv -1$ (by exclusion, since there are $(p-1)/2$ each of $+1$ and $-1$ values among $(p-1)$ elements).

P: Is $3$ a quadratic residue modulo $11$?
S:
**IDENTIFY**: Apply Euler's criterion.

**PLAN**: Compute $3^5 \bmod 11$.

**EXECUTE**:
1. $3^2 = 9$.
2. $3^4 = 81 \equiv 4 \pmod{11}$.
3. $3^5 = 3 \cdot 4 = 12 \equiv 1 \pmod{11}$.
4. So $\left(\frac{3}{11}\right) = 1$; $3$ IS a QR mod $11$.

**EVALUATE**: Indeed, $5^2 = 25 \equiv 3 \pmod{11}$ — a concrete square root. Also $6^2 = 36 \equiv 3$ ($\pm 5 \equiv 5, 6$ mod $11$).

## 9.5 The Jacobi Symbol

C: The [Jacobi symbol] $\left(\frac{a}{n}\right)$ for odd $n = p_1^{e_1} \cdots p_k^{e_k}$ and $\gcd(a, n) = 1$ is defined as $\prod_i \left(\frac{a}{p_i}\right)^{e_i}$.

Q: Why introduce the Jacobi symbol when the Legendre symbol seems sufficient?
A: Because the Jacobi symbol is defined for composite odd moduli $n$ and its computation uses only modular arithmetic (via quadratic reciprocity), not factorization of $n$. So it's algorithmically useful — you can evaluate $\left(\frac{a}{n}\right)$ without knowing $n$'s prime factorization, critical when $n$ is a cryptographic-size composite.

Q: Why is "$\left(\frac{a}{n}\right) = 1$ does NOT imply $a$ is a QR mod $n$" the key caveat?
A: Because the Jacobi symbol is a product of Legendre symbols — two $-1$'s multiply to $+1$. Example: $\left(\frac{2}{15}\right) = \left(\frac{2}{3}\right)\left(\frac{2}{5}\right) = (-1)(-1) = 1$, but $2$ is a QNR mod $3$ (and mod $5$), so $x^2 \equiv 2 \pmod{15}$ has no solution by CRT. Jacobi $+1$ only tells you $a$'s status is "consistent" with being a QR — which is weaker.

## 9.6 Quadratic Reciprocity

C: [Law of quadratic reciprocity (Gauss)]: for distinct odd primes $p, q$: $\left(\frac{p}{q}\right)\left(\frac{q}{p}\right) = (-1)^{(p-1)(q-1)/4}$.

Q: How does quadratic reciprocity let you flip a Legendre symbol computation?
A: It transforms $\left(\frac{p}{q}\right)$ into $\left(\frac{q}{p}\right)$ (sometimes with a sign flip), reducing the "top" to a smaller number mod a smaller modulus. Combined with the supplementary laws and multiplicativity, it computes any Legendre symbol in $O(\log)$ time — analogous to the Euclidean algorithm for gcd.

## 9.7 Supplementary Laws

Q: What do the supplementary laws of quadratic reciprocity say?
A: (i) $\left(\frac{-1}{p}\right) = (-1)^{(p-1)/2}$ — so $-1$ is a QR mod $p$ iff $p \equiv 1 \pmod 4$. (ii) $\left(\frac{2}{p}\right) = (-1)^{(p^2 - 1)/8}$ — so $2$ is a QR iff $p \equiv \pm 1 \pmod 8$. These two special cases complete the toolkit for evaluating any Legendre symbol.

P: Compute $\left(\frac{5}{43}\right)$ using quadratic reciprocity.
S:
**IDENTIFY**: Apply reciprocity to flip the symbol.

**PLAN**: $5, 43$ odd primes. Compute the sign factor $(-1)^{(5-1)(43-1)/4}$; evaluate $\left(\frac{43}{5}\right)$.

**EXECUTE**:
1. Sign: $(5-1)(43-1)/4 = 4 \cdot 42 / 4 = 42$; $(-1)^{42} = 1$.
2. So $\left(\frac{5}{43}\right) = \left(\frac{43}{5}\right) = \left(\frac{43 \bmod 5}{5}\right) = \left(\frac{3}{5}\right)$.
3. Is $3$ a QR mod $5$? QRs mod $5$ are $\{1, 4\}$; $3$ is NOT. So $\left(\frac{3}{5}\right) = -1$.
4. $\left(\frac{5}{43}\right) = -1$.

**EVALUATE**: $5$ is a QNR mod $43$; $x^2 \equiv 5 \pmod{43}$ has no solution. Verify: Euler's criterion gives $5^{21} \bmod 43 = -1 \equiv 42$ (direct computation confirms).

## 9.8 Solovay–Strassen Primality Test

Q: How does the Jacobi symbol underlie the Solovay–Strassen primality test?
A: For $n$ prime and $\gcd(a, n) = 1$: by Euler's criterion, $a^{(n-1)/2} \equiv \left(\frac{a}{n}\right) \pmod n$. For composite $n$, this identity fails for at least half of the $a$'s. So: pick random $a$; compute both $a^{(n-1)/2} \bmod n$ and $\left(\frac{a}{n}\right)$; if they differ, $n$ is composite. Probability of false positive: $\leq 1/2$ per trial.

## 9.9 Quadratic Residuosity Problem

Q: What is the [quadratic residuosity problem] (QRP)?
A: Given $n = pq$ (product of two unknown primes) and $a$ with $\left(\frac{a}{n}\right) = 1$, decide whether $a$ is a QR mod $n$. Presumed hard — the strongest evidence is that it's equivalent (in polynomial time) to factoring $n$. Used in the [Goldwasser–Micali cryptosystem], the first provably-semantically-secure public-key scheme.

## 9.10 Tonelli–Shanks: Finding Square Roots

Q: When $a$ is a QR mod prime $p$, how do you find $x$ with $x^2 \equiv a \pmod p$?
A: For $p \equiv 3 \pmod 4$, a shortcut: $x = a^{(p+1)/4} \bmod p$ works. Check: $x^2 = a^{(p+1)/2} = a \cdot a^{(p-1)/2} \equiv a \cdot 1 = a$ (by Euler's criterion with $a$ a QR). For general $p$, the [Tonelli–Shanks algorithm] runs in $O((\log p)^2)$ expected time, a generalization of the shortcut.

## 9.11 Number of Solutions to $x^2 \equiv a \pmod n$

Q: How many solutions does $x^2 \equiv a \pmod n$ have when $n = p_1 p_2 \cdots p_k$ (distinct odd primes) and $a$ is a QR mod each $p_i$?
A: $2^k$ solutions. Each prime factor contributes $2$ solutions ($\pm$), and CRT combines them independently — giving $2^k$ combinations. This is the basis for the [Rabin cryptosystem], where decryption yields $4$ candidate plaintexts (for $n = pq$), to be disambiguated by redundancy.

## 9.12 Why QRs Appear Everywhere

Q: Name three cryptographic/algorithmic uses of quadratic residues.
A: (i) [Goldwasser–Micali] public-key encryption via QRP. (ii) [Blum–Blum–Shub] pseudorandom generator based on iterated squaring. (iii) [Rabin signatures] relying on $4$-to-$1$ squaring map. QR theory provides a "hardness assumption" parallel to but distinct from the discrete-log and factoring assumptions — one more arrow in the cryptographer's quiver.

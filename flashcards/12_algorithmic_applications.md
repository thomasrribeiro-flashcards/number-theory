+++
order = 12
subject = "Math"
tags = ["math", "number-theory", "algorithms", "hashing", "prng", "checksum", "coding-theory"]
+++

# Number Theory — Algorithmic Applications

## 12.1 Why Number Theory Keeps Showing Up

Q: Why does number theory appear in so many computer science algorithms beyond cryptography?
A: Because discrete structures with modular arithmetic provide cheap, well-distributed, bijective operations — exactly what algorithms need. Primes give "random-looking" wrap-around; multiplication mod $p$ permutes residues; modular exponentiation compresses history. These properties enable hashing, pseudorandom generation, error-correcting codes, polynomial arithmetic, and more.

## 12.2 Hash Tables and the Modular Trick

Q: Why do hash tables often use a prime modulus for array size?
A: Because "$\text{hash}(k) \bmod p$" with $p$ prime spreads keys more uniformly than $\bmod m$ with $m$ a power of $2$. Bad hash functions may correlate with powers of $2$ (low bits are structured); a prime modulus avoids that. Modern high-performance hash tables often use power-of-$2$ sizes with STRONG hash functions (MurmurHash, xxHash) that are uniform by construction.

## 12.3 Universal Hashing

Q: What is [universal hashing] and why does it use number theory?
A: A family of hash functions $\mathcal{H}$ is [universal] if for any two keys $k_1 \neq k_2$, the probability over random $h \in \mathcal{H}$ that $h(k_1) = h(k_2)$ is $\leq 1/m$ (where $m$ is the table size). A classic construction: $h_{a,b}(k) = ((ak + b) \bmod p) \bmod m$ with $p$ prime $> |U|$, random $a \in \{1,\dots,p-1\}$ and $b \in \{0,\dots,p-1\}$. Modular structure gives the analysis its collision bound.

## 12.4 Rolling Hashes and Rabin–Karp

Q: How does the [Rabin–Karp] string-matching algorithm use modular hashing?
A: Compute a polynomial hash of the pattern: $H(P) = \sum_{i} P_i B^{n-1-i} \bmod p$. Slide a window over the text, maintaining $H(T_{i..i+n-1})$ and updating in $O(1)$ per shift: $H_{\text{new}} = (B \cdot H_{\text{old}} - T_i B^n + T_{i+n}) \bmod p$. When hashes match, verify the actual strings. Average $O(n + m)$, much faster than $O(nm)$ naive matching when hash collisions are rare.

## 12.5 Linear Congruential Generators

C: A [linear congruential generator] (LCG) produces pseudorandom sequences via $x_{n+1} = (a x_n + c) \bmod m$ with chosen parameters $a, c, m$.

Q: What does the [Hull–Dobell theorem] say about LCG period?
A: The LCG has full period $m$ iff: (i) $\gcd(c, m) = 1$; (ii) $a - 1$ is divisible by every prime factor of $m$; (iii) $a - 1$ is divisible by $4$ if $m$ is. Number theory tells you which parameters give maximum period — essential for a useful PRNG. LCGs are fast but have known flaws (low-dimensional uniformity); modern PRNGs use richer structures (xorshift, PCG).

## 12.6 Blum–Blum–Shub

Q: How does the [Blum–Blum–Shub] PRNG use number theory?
A: Choose $n = pq$ where $p \equiv q \equiv 3 \pmod 4$. Start with seed $x_0$ coprime to $n$. Iterate $x_{n+1} = x_n^2 \bmod n$; output one bit of $x_n$ (typically LSB) per step. Cryptographically secure under the QR assumption — predicting the next bit is as hard as quadratic residuosity. Slow but provably hard.

## 12.7 Error-Correcting Codes

Q: How does number theory (over $\mathbb{F}_p$) enter [Reed–Solomon codes]?
A: A message is encoded as a polynomial $P(x)$ of degree $< k$ over $\mathbb{F}_p$. The codeword is $P$'s values at $n > k$ fixed points. Decoding: given $n$ values with up to $(n - k)/2$ errors, recover $P$ uniquely via algorithms like Berlekamp–Massey (using polynomial gcd). RS codes power CDs, DVDs, QR codes, deep-space communication, and erasure coding in distributed storage.

## 12.8 Fast Polynomial Multiplication

Q: How does modular number theory speed up large-integer and polynomial multiplication?
A: [Number-theoretic transform (NTT)] is the FFT over $\mathbb{Z}/p\mathbb{Z}$ using primitive roots of unity. Multiplying two $n$-coefficient polynomials via NTT costs $O(n \log n)$ field operations — as fast as floating-point FFT but with exact arithmetic. Used in post-quantum crypto (Kyber/Dilithium's polynomial multiplication), homomorphic encryption, and fast big-integer libraries.

## 12.9 Modular Arithmetic in Checksums

Q: How do CRC checksums use algebraic number theory?
A: A [cyclic redundancy check] treats data as a polynomial over $\mathbb{F}_2$ and computes its remainder modulo a fixed generator polynomial $G(x)$. Detects burst errors up to the degree of $G$. Standards (Ethernet CRC-32, ZIP CRC-32) use specific generator polynomials chosen for strong error-detection properties. Cheap (table lookup) and effective.

## 12.10 Zero-Knowledge Proofs

Q: How does number theory appear in [zero-knowledge proofs]?
A: Classical ZK protocols (Schnorr identification, Feige–Fiat–Shamir) reduce "I know a secret $x$" to DL/QR statements. Prover and verifier exchange challenges in $\mathbb{F}_p$ or $(\mathbb{Z}/n\mathbb{Z})^{\times}$; the protocol is sound because forging requires solving a hard number-theoretic problem. Modern ZK-SNARKs use pairing-based crypto on elliptic curves — advanced number theory powering Zcash and rollups.

## 12.11 Primes in Data Structures

Q: Why do treaps, Bloom filters, and Count-Min sketches often use prime moduli or random shifts?
A: Because [universal hashing] with modular arithmetic gives independence guarantees essential for their analysis. Bloom filter false-positive bounds assume $k$ independent hash functions; constructing them via $h_i(x) = (a_i x + b_i) \bmod p \bmod m$ gives provable independence. Count-Min's error bounds rely on pairwise-independent families constructed similarly.

## 12.12 Algorithmic Takeaway

Q: What's the pattern by which number theory enables so many algorithms?
A: "Mod a prime" supplies (i) a finite group/field with all arithmetic operations defined, (ii) cheap bijective maps (multiplication by a unit), (iii) distribution properties (Fermat, primitive roots), (iv) security assumptions (DL, QR, factoring). Pick your prime, pick your operation, and you instantly get a structured-but-random object — the algorithmic equivalent of a Swiss army knife.

Q: Where should you go next after this deck?
A: Three natural directions. (i) [Abstract algebra]: group theory, rings, fields, Galois theory — the structural framework behind everything here. (ii) [Algebraic number theory]: number fields, ideals, reciprocity in general settings. (iii) [Cryptographic protocol design]: zero-knowledge proofs, homomorphic encryption, multi-party computation, post-quantum schemes. Each builds on the number-theoretic foundation you now have.

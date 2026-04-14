+++
order = 11
subject = "Math"
tags = ["math", "number-theory", "cryptography", "rsa", "diffie-hellman", "ecc", "public-key"]
+++

# Number Theory — Public-Key Cryptography

## 11.1 Symmetric vs. Asymmetric

Q: What distinguishes [public-key cryptography] from symmetric-key cryptography?
A: Symmetric: both parties share the SAME secret key (AES, ChaCha20). Fast, but requires a secure channel to exchange keys initially. Public-key: each party has a key pair (public encryption key, private decryption key). The public key can be published; only the private-key holder can decrypt. Public-key solves the key-distribution problem at the cost of $10^3$–$10^5$× slower operations.

## 11.2 The Big Three Problems

Q: Name the three hard problems underpinning classical public-key crypto.
A: (i) [Integer factorization]: given $n = pq$, find $p$ and $q$. Underlies RSA. (ii) [Discrete logarithm] in $\mathbb{F}_p^{\times}$: given $g, h$, find $x$ with $g^x = h$. Underlies Diffie–Hellman, ElGamal, DSA. (iii) [Elliptic curve discrete log]: same as DL but in an elliptic curve group. Underlies ECDH, ECDSA. All three are presumed hard classically and broken by quantum algorithms.

## 11.3 RSA — Key Generation

Q: Walk through RSA key generation step by step.
A: (i) Choose two large distinct primes $p, q$ of similar size. (ii) Compute $n = pq$ and $\varphi(n) = (p-1)(q-1)$. (iii) Pick $e$ coprime to $\varphi(n)$ (commonly $e = 65537$). (iv) Compute $d \equiv e^{-1} \pmod{\varphi(n)}$ via extended Euclidean. (v) Public key: $(n, e)$. Private key: $(n, d)$ — or $(p, q, d_p, d_q, q^{-1} \bmod p)$ for fast CRT decryption.

## 11.4 RSA — Encrypt and Decrypt

Q: Given RSA keys $(n, e)$ and $(n, d)$, how do you encrypt and decrypt?
A: [Encrypt]: $c = m^e \bmod n$ (for message $m < n$ represented as an integer). [Decrypt]: $m = c^d \bmod n$. Correctness: $c^d = m^{ed} = m^{1 + k\varphi(n)} \equiv m \cdot (m^{\varphi(n)})^k \equiv m \cdot 1 = m \pmod n$ (using Euler's theorem, valid when $\gcd(m, n) = 1$; the edge cases $p \mid m$ or $q \mid m$ still work via CRT).

## 11.5 Why Textbook RSA Is Insecure

Q: Why is "textbook" RSA — just $m^e \bmod n$ — NOT used in practice?
A: Because raw RSA is [deterministic] (same $m$ always gives same $c$ — attacker can detect repeats) and [malleable] ($(m^e)(m'^e) = (mm')^e$ — attacker can multiply ciphertexts). Real implementations use padding schemes: OAEP for encryption (RSA-OAEP), PSS for signing (RSA-PSS). These add randomness and structure, preventing known attacks.

## 11.6 Diffie–Hellman Key Exchange

Q: How do Alice and Bob derive a shared secret over a public channel via [Diffie–Hellman]?
A: Public parameters: a large prime $p$ and generator $g$ of a subgroup of $\mathbb{F}_p^{\times}$. (i) Alice picks secret $a$, sends $A = g^a \bmod p$. (ii) Bob picks secret $b$, sends $B = g^b \bmod p$. (iii) Shared key: $K = B^a = A^b = g^{ab} \bmod p$. Eavesdropper sees $g, p, A, B$; to find $K$ must solve the [computational Diffie–Hellman (CDH)] problem.

Q: What assumption is needed for DH to provide a secret key indistinguishable from random?
A: The [Decisional Diffie–Hellman (DDH)] assumption: given $(g^a, g^b, g^c)$, distinguishing "$c = ab$" from "$c$ random" is computationally infeasible. DDH is stronger than CDH (computing $g^{ab}$) and is needed for hybrid encryption schemes. DDH holds in prime-order subgroups of $\mathbb{F}_p^{\times}$ and in generic elliptic curve groups.

## 11.7 Man-in-the-Middle and Authentication

Q: Why does raw Diffie–Hellman NOT prevent man-in-the-middle attacks?
A: Because Mallory can intercept $A$ from Alice, send $A' = g^{a'}$ to Bob, and similarly intercept $B$. Mallory now holds two shared secrets: one with Alice ($g^{aa'}$), one with Bob ($g^{a'b}$). Alice and Bob are communicating through Mallory unwittingly. Fix: authenticate the public values with signatures (signed DH) or certificates (TLS).

## 11.8 ElGamal Encryption

Q: How does ElGamal turn Diffie–Hellman into a public-key encryption scheme?
A: Alice's public key: $(p, g, h = g^a \bmod p)$; private key: $a$. To encrypt $m$: pick random $k$; send $c_1 = g^k$ and $c_2 = m \cdot h^k$. Decrypt: $m = c_2 \cdot (c_1^a)^{-1} \bmod p$. Security: under DDH, $h^k$ is indistinguishable from random, masking $m$. Unlike RSA, ElGamal is nondeterministic — different $k$ gives different ciphertexts for the same $m$.

## 11.9 Digital Signatures

Q: How does RSA provide a digital signature?
A: Signer has $(n, d)$ private. To sign $m$: compute $s = \text{hash}(m)^d \bmod n$. Verifier uses public $(n, e)$: check $s^e \equiv \text{hash}(m) \pmod n$. Only the private-key holder can produce $s$ (requires $d$); anyone can verify. PSS padding is used in practice to avoid algebraic attacks.

Q: What is the [DSA] signature scheme?
A: The [Digital Signature Algorithm] uses discrete log in $\mathbb{F}_p^{\times}$: private key $x$; public key $y = g^x$. To sign $m$: pick random $k$; compute $r = (g^k \bmod p) \bmod q$ and $s = k^{-1}(\text{hash}(m) + xr) \bmod q$. Signature: $(r, s)$. Compact and efficient; ECDSA is the elliptic-curve variant used widely (SSH, TLS, Bitcoin).

## 11.10 Elliptic Curve Cryptography

Q: Why has elliptic curve cryptography (ECC) largely replaced RSA in modern protocols?
A: Because [elliptic curve discrete log] is believed harder per bit than DL in $\mathbb{F}_p^{\times}$. For equivalent security to RSA-$3072$, ECC needs only $256$-bit keys. Smaller keys mean faster computations, less bandwidth, smaller certificates — crucial on mobile devices and in constrained protocols. TLS 1.3 prefers ECC; Bitcoin uses ECDSA on secp256k1.

Q: How does an elliptic curve form a group for crypto?
A: Points on an elliptic curve $y^2 = x^3 + ax + b$ (over $\mathbb{F}_p$) form an abelian group under the "chord-and-tangent" addition law, with a point at infinity as identity. The group size is $\approx p$ (Hasse bound). Discrete log in this group has no sub-exponential algorithm for well-chosen curves — far stronger than $\mathbb{F}_p^{\times}$ per bit.

## 11.11 Key Encapsulation and Hybrid Encryption

Q: What is [hybrid encryption]?
A: A combination: use public-key crypto to encrypt a randomly-generated symmetric key (the [key encapsulation]), then use fast symmetric encryption (AES-GCM) for the bulk data. Real protocols (TLS, PGP, Signal) all do this — public-key for authentication and key exchange, symmetric for data. Best of both worlds: asymmetric's key-distribution property, symmetric's speed.

## 11.12 Post-Quantum Cryptography

Q: What threatens RSA and ECC in the long term?
A: [Shor's algorithm] on a sufficiently large quantum computer factors integers and solves discrete logs in polynomial time. Projected "Q-day" estimates vary (10+ years, if ever). In response, NIST standardized post-quantum schemes in 2024: [CRYSTALS-Kyber] (lattice-based KEM), [CRYSTALS-Dilithium] (lattice-based signatures), [SPHINCS+] (hash-based signatures). These rely on lattice and coding problems, not number-theoretic ones.

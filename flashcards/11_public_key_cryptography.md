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

Q: What are the public and private keys in RSA, given primes $p, q$?
A: Set $n = pq$ and $\varphi(n) = (p-1)(q-1)$. Public key: $(n, e)$ with $\gcd(e, \varphi(n)) = 1$. Private key: $(n, d)$ where $d \equiv e^{-1} \pmod{\varphi(n)}$.

Q: Why is $e = 65537 = 2^{16} + 1$ the most common RSA public exponent?
A: Because it's prime (so coprime to most $\varphi(n)$ values), and its binary form $10000000000000001$ makes $m^e$ computable in just $17$ squarings and one multiplication — fast public-key encryption. Smaller exponents ($e = 3$) have known padding attacks; $65537$ is the standard compromise.

Q: How is $d$ actually computed in RSA key generation?
A: Via the extended Euclidean algorithm applied to $(e, \varphi(n))$: find $x, y$ with $ex + \varphi(n) y = 1$, then $d \equiv x \pmod{\varphi(n)}$. Cost: $O(\log \varphi(n))$ steps — fast even for $2048$-bit moduli.

Q: Why do production RSA implementations store $(p, q, d_p, d_q, q^{-1} \bmod p)$ as the private key?
A: Because CRT lets decryption compute $c^{d_p} \bmod p$ and $c^{d_q} \bmod q$ separately (with $d_p = d \bmod (p-1)$, $d_q = d \bmod (q-1)$), then recombine. Each half has half the bit-length, giving roughly $4\times$ speedup over computing $c^d \bmod n$ directly.

## 11.4 RSA — Encrypt and Decrypt

Q: Given RSA keys $(n, e)$ and $(n, d)$, how do you encrypt and decrypt?
A: [Encrypt]: $c = m^e \bmod n$ (for message $m < n$ represented as an integer). [Decrypt]: $m = c^d \bmod n$. Correctness: $c^d = m^{ed} = m^{1 + k\varphi(n)} \equiv m \cdot (m^{\varphi(n)})^k \equiv m \cdot 1 = m \pmod n$ (using Euler's theorem, valid when $\gcd(m, n) = 1$; the edge cases $p \mid m$ or $q \mid m$ still work via CRT).

## 11.5 Why Textbook RSA Is Insecure

Q: Why is textbook RSA encryption [deterministic], and why is that a security flaw?
A: Because the same plaintext $m$ always produces the same ciphertext $c = m^e \bmod n$. An attacker who knows the message space (e.g., "yes"/"no" responses) can simply encrypt each candidate and compare to the observed $c$ to recover $m$. Secure schemes must be randomized.

Q: What does it mean that textbook RSA is [malleable]?
A: Given ciphertexts $c_1 = m_1^e$ and $c_2 = m_2^e$, an attacker can compute $c_1 c_2 = (m_1 m_2)^e \bmod n$ — a valid ciphertext for $m_1 m_2$ without knowing the private key. This breaks chosen-ciphertext security.

Q: What fixes does [RSA-OAEP] apply to textbook RSA?
A: [Optimal Asymmetric Encryption Padding]: before encryption, pad $m$ with random bits and apply a two-round Feistel network with cryptographic hash functions. Output is indistinguishable from random, breaking determinism and malleability. Provably secure in the random oracle model.

Q: When is [RSA-PSS] used instead of RSA-OAEP?
A: For SIGNATURES, not encryption. [Probabilistic Signature Scheme] randomizes the message digest before applying $d$, defending against existential forgery. RSA-OAEP is for encryption; RSA-PSS is for signing — both are the current standard padding schemes (PKCS #1 v2.2).

## 11.6 Diffie–Hellman Key Exchange

C: In [Diffie–Hellman], the public parameters are a large prime $p$ and a generator $g$ of a prime-order subgroup of $\mathbb{F}_p^{\times}$.

Q: What are the two messages exchanged in Diffie–Hellman?
A: Alice picks secret $a$ and sends $A = g^a \bmod p$. Bob picks secret $b$ and sends $B = g^b \bmod p$. Each uses their own secret and the received value to derive the shared key.

Q: How do Alice and Bob each compute the same shared key after the exchange?
A: Alice computes $K = B^a = (g^b)^a = g^{ab} \bmod p$. Bob computes $K = A^b = (g^a)^b = g^{ab} \bmod p$. Both arrive at $g^{ab}$ without transmitting it.

Q: What hard problem protects Diffie–Hellman against passive eavesdroppers?
A: The [computational Diffie–Hellman (CDH)] problem: given $g, p, g^a, g^b$, compute $g^{ab}$. No polynomial-time algorithm is known classically; solving CDH is at most as hard as discrete log (and believed equivalent in prime-order subgroups).

Q: What assumption is needed for DH to provide a secret key indistinguishable from random?
A: The [Decisional Diffie–Hellman (DDH)] assumption: given $(g^a, g^b, g^c)$, distinguishing "$c = ab$" from "$c$ random" is computationally infeasible. DDH is stronger than CDH (computing $g^{ab}$) and is needed for hybrid encryption schemes. DDH holds in prime-order subgroups of $\mathbb{F}_p^{\times}$ and in generic elliptic curve groups.

## 11.7 Man-in-the-Middle and Authentication

Q: Why does raw Diffie–Hellman NOT prevent man-in-the-middle attacks?
A: Because Mallory can intercept $A$ from Alice, send $A' = g^{a'}$ to Bob, and similarly intercept $B$. Mallory now holds two shared secrets: one with Alice ($g^{aa'}$), one with Bob ($g^{a'b}$). Alice and Bob are communicating through Mallory unwittingly. Fix: authenticate the public values with signatures (signed DH) or certificates (TLS).

## 11.8 ElGamal Encryption

Q: What are Alice's public and private keys in [ElGamal]?
A: Public key: $(p, g, h = g^a \bmod p)$ with $p$ prime and $g$ a generator of a subgroup. Private key: the exponent $a$. Think of it as "half of a DH exchange" already published — anyone can encrypt to Alice by completing the exchange on the fly.

Q: How do you encrypt a message $m$ under an ElGamal public key $(p, g, h)$?
A: Pick a random ephemeral exponent $k$. Send the ciphertext $(c_1, c_2) = (g^k, m \cdot h^k \bmod p)$. The $c_1$ component lets Alice reconstruct the masking factor $h^k = g^{ak}$.

Q: How does Alice decrypt an ElGamal ciphertext $(c_1, c_2)$?
A: Compute $c_1^a = g^{ak} = h^k \bmod p$, then $m = c_2 \cdot (c_1^a)^{-1} \bmod p$. Her private exponent $a$ rebuilds the mask; inverting it recovers $m$.

Q: Why is ElGamal nondeterministic, unlike textbook RSA?
A: Because the sender picks a fresh random $k$ per message, so the ciphertext $(g^k, m h^k)$ changes with each encryption of the same $m$. Under the DDH assumption, $h^k$ is indistinguishable from random, so ElGamal is semantically secure (textbook RSA is not).

## 11.9 Digital Signatures

Q: How does RSA provide a digital signature?
A: Signer has $(n, d)$ private. To sign $m$: compute $s = \text{hash}(m)^d \bmod n$. Verifier uses public $(n, e)$: check $s^e \equiv \text{hash}(m) \pmod n$. Only the private-key holder can produce $s$ (requires $d$); anyone can verify. PSS padding is used in practice to avoid algebraic attacks.

Q: What are the public and private keys in [DSA]?
A: In the [Digital Signature Algorithm], the private key is an exponent $x \in [1, q-1]$ and the public key is $y = g^x \bmod p$, where $g$ generates a subgroup of prime order $q$ dividing $p - 1$. The nested modular structure (mod $p$ then mod $q$) compresses signatures.

Q: How do you produce a DSA signature $(r, s)$ on message $m$?
A: Pick random ephemeral $k \in [1, q-1]$. Compute $r = (g^k \bmod p) \bmod q$ and $s = k^{-1}(\text{hash}(m) + x r) \bmod q$. Signature is $(r, s)$. The nonce $k$ MUST be fresh and secret — reusing $k$ leaks the private key $x$ (Sony PS3 hack, 2010).

Q: What is [ECDSA] and why has it displaced DSA?
A: ECDSA is DSA with the group $\mathbb{F}_p^{\times}$ replaced by an elliptic-curve group. Same algorithmic skeleton, but discrete log on elliptic curves is harder per bit, so ECDSA achieves $128$-bit security with just $256$-bit keys (vs. DSA's $3072$). Standard in SSH, TLS 1.3, and Bitcoin (secp256k1).

## 11.10 Elliptic Curve Cryptography

Q: Why has elliptic curve cryptography (ECC) largely replaced RSA in modern protocols?
A: Because [elliptic curve discrete log] is believed harder per bit than DL in $\mathbb{F}_p^{\times}$. For equivalent security to RSA-$3072$, ECC needs only $256$-bit keys. Smaller keys mean faster computations, less bandwidth, smaller certificates — crucial on mobile devices and in constrained protocols. TLS 1.3 prefers ECC; Bitcoin uses ECDSA on secp256k1.

Q: How does an elliptic curve form a group for crypto?
A: Points on an elliptic curve $y^2 = x^3 + ax + b$ (over $\mathbb{F}_p$) form an abelian group under the "chord-and-tangent" addition law, with a point at infinity as identity. The group size is $\approx p$ (Hasse bound). Discrete log in this group has no sub-exponential algorithm for well-chosen curves — far stronger than $\mathbb{F}_p^{\times}$ per bit.

## 11.11 Key Encapsulation and Hybrid Encryption

Q: What is [hybrid encryption]?
A: A combination: use public-key crypto to encrypt a randomly-generated symmetric key (the [key encapsulation]), then use fast symmetric encryption (AES-GCM) for the bulk data. Real protocols (TLS, PGP, Signal) all do this — public-key for authentication and key exchange, symmetric for data. Best of both worlds: asymmetric's key-distribution property, symmetric's speed.

## 11.12 Post-Quantum Cryptography

Q: Why does Shor's algorithm threaten both RSA and ECC simultaneously?
A: Because Shor's algorithm solves both integer factorization AND discrete logarithm in polynomial time on a quantum computer. RSA's security reduces to factoring; ECC's security reduces to elliptic-curve discrete log. One quantum algorithm breaks both assumptions — so post-quantum security needs entirely different hardness assumptions.

Q: What is [CRYSTALS-Kyber]?
A: A NIST-standardized (2024) post-quantum key encapsulation mechanism based on the hardness of the [Module Learning With Errors] (M-LWE) problem in lattices. Replaces RSA and DH for key exchange in quantum-threat scenarios. Used in TLS 1.3 hybrid key exchange as of 2024.

Q: What is [CRYSTALS-Dilithium]?
A: A NIST-standardized (2024) post-quantum digital signature scheme, also based on lattice hardness (M-LWE and M-SIS). Replaces ECDSA and RSA-PSS for signing. Larger signatures ($\sim 2{-}4$ KB vs. $64$ B for ECDSA) but quantum-resistant.

Q: What is [SPHINCS+] and why keep a non-lattice alternative?
A: A hash-based signature scheme, NIST-standardized as a conservative backup. Security relies only on a cryptographic hash being collision-resistant — no algebraic assumptions. Larger signatures ($\sim 8{-}30$ KB) and slower, but immune to future breaks in lattice problems. Diversification hedge.

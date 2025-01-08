### Cheat Sheet

##### 0. Historical Ciphers

1. Caeser Cipher
2. Mono-alphabetic Cipher
3. Vegenère Cipher
4. One-time Pad

---

##### 1. Symmetric Key Encryption

1. SKE Scheme
   - Definition 
     - A SKE $(\text{KGen, Enc, Dec})$ has $\mathcal{M, C, K}$ such that
       - $\text{KGen}$ returns a $sk$ sampled from $\mathcal{K}$,
       - For $m \in \mathcal{M}$ encryption is $\text{Enc}(sk, m) = c \in C$, 
       - For $c \in \mathcal{C}$ decryption is $\text{Dec}(sk, \text{Enc}(sk, m)) = m$. 
   - Correctness
     - For all $m \in \mathcal{M}, sk \in \mathcal{K}$, we have $\text{Dec}(sk, \text{Enc}(sk, m)) = m$. 
   - Security
     - Adversaries cannot learn anything about the encrypted message given access to $\text{Enc}(sk, \cdot)$.
       - Let $sk \leftarrow \text{KGen}$ and $b \in \{0, 1\}$ sampled uniformly,
       - $\mathcal{A}$ with $\text{Enc}(sk, \cdot)$ outputs $m_0, m_1 \in \mathcal{M}$ of the same length,
       - $\mathcal{A}$ with $\text{Enc}(sk, \cdot)$ recevies $c = \text{Enc}(sk, m_b)$,
       - $\mathcal{A}$ outputs $b' \in \{0, 1\}$ and wins when $b = b'$.
     - If no adversaries win with probability (meaningfully) more than one half, then *SKE is secure*. 
2. MAC Scheme
   - Definition
     - A MAC $(\text{KGen, Tag}, V_f)$ has $\mathcal{M, T, K}$ such that:
       - $\text{KGen}$ is the interal key generation algortihm, which returns a $ik$ sampled from $\mathcal{K}$,
       - for $m \in \mathcal{K}$ forming a tag is $\text{Tag}(ik, m) = \tau \in \mathcal{T}$,
       - for $m \in \mathcal{K}$ and $\tau \in \mathcal{T}$ verification is $V_f(ik, m, \tau) \in \{0, 1\}$.
   - Correctness
     - $\forall m \in \mathcal{M}$ and $ik \in \mathcal{K}$ we have $V_f(ik, m, \text{Tag}(ik, m)) = 1$.
   - Security
     - Adversaries given $\text{Tag}(ik, \cdot)$ cannot forge a tag on a message they do not know a tag for.
     - Let $ik \leftarrow \text{KGen}, Q = \empty$,
       - Each time $\mathcal{A}$ with $\text{Tag}(ik, \cdot)$ queries $\tau = \text{Tag}(ik, m)$ add $m$ to $Q$,
       - $\mathcal{A}$ with $\text{Tag}(ik, \cdot)$ outputs $(m^*, \tau^*)$,
       - $\mathcal{A}$ wins if $V_f(ik, m^*, \tau^*) = 1$ and $m^* \notin Q$. 
     - If no adversaries win with probability (meaningfully) more than zero then *MAC is secure*. 

---

##### 2. Block Cipher

1. Block Cipher

     - Definiton

       - **Block ciphers** are a building block for SKE. A block cipher is a function
         $$
         F: \{0, 1\}^n \times \{0, 1\}^l \to \{0, 1\}^l
         $$
         where $n$ is the key length and $l$ the block length, such that for each key $sk$
         $$
         F_{sk} = F(sk, \cdot): \{0, 1\}^l \to \{0, 1\}^l
         $$
         is a permutation.


   2. SPN
      - Encryption
        - We define the function $S, P$, respectively for substitution and permutation. We then execute encryption for $k$ rounds. 
          - For each round $i$ that is not the final round:
            $m^{(i)} = P(S(sk_i ⊕ m^{(i-1)}))$.
          - For the final round:
            $m^{(i)} = sk_i ⊕ m^{(i-1)}$.
        - Return the final output: $F_{sk}(m) = m_{(r)}$.
      - Decryption
        - Inverse of the above. 

   3. Feistel Network

         - Encryption
           - Let $sk_1, ..., sk_r$ of length $n'$ be round keys derived from $sk$. 
             Let $f_1, ..., f_r$ be *round functions* (Should not be a permutation), defined as $f_i: \{0, 1\}^{n^{'}} \times \{0, 1\}^{l/2} \to \{0, 1\}^{l/2}$, 
             - For the first round: 
               - $f_1: \{0, 1\}^{n^{'}} \times \{0, 1\}^{l/2} \to \{0, 1\}^{l/2}$
               - $m^{(0)} = m = L_0 \parallel R_0$. 
             - For each round $i$ after the first round:
               - $L_i = R_{i-1}$, 
               - $R_i =  L_{i-1} ⊕ f_i(sk_i, R_{i-1})$,
               - $m^{(i)} = L_i || R_i$, 
           - The final output is $F_{sk}(m) = m^{(r)}$.

         - Decryption
           - Inverse of the above. 

---

##### 3. Mode of Operations

1. ECB - Insecure
2. CBC - Secure, cannot parallel

     - $F: \{0, 1\}^n \times \{0, 1\}^l$ is a block cipher, 
       $\mathcal{M}=\mathcal{C}=\cup_{i \in \mathbb{N}}\{0, 1\}^{il}$
       $\mathcal{K} = \{0, 1\}^n$
     - $\text{KGen}$ samples $sk$ uniformly from $\mathcal{K}$. 
         - $\text{Enc}(sk, m)$ for $m \in \{0, 1\}^{tl}$
         
           - Sample uniform $IV \in \{0, 1\}^l$ and parse $m = m_1, ..., m_t$ in $l$ bit blocks,
           - $c_0 = IV$
           - for $i \in \{1, ..., t\}$ let $c_i = F_{sk} (c_{i-1} ⊕ m_i)$ 
     - $\text{Dec}(sk, c)$ for $c \in \{0, 1\}^{tl}$
       - Parse $c = c_0 c_1 ... c_t$ as length $l$ blocks,
         - for $i \in \{1, ..., m\}$ let $m_i = c_{i-1} ⊕ F^{-1}(sk, c_i)$.

3. Counter Mode - Secure, parallel

     - $F: \{0, 1\}^n \times \{0, 1\}^l$ is a block cipher, 
       $\mathcal{M}=\mathcal{C}=\cup_{i \in \mathbb{N}}\{0, 1\}^{il}$
       $\mathcal{K} = \{0, 1\}^n$
     - $\text{KGen}$ samples $sk$ uniformly from $\mathcal{K}$. 
         - $\text{Enc}(sk, m)$ for $m \in \{0, 1\}^{tl}$
           - Sample uniform $IV \in \{0, 1\}^l$ and parse $m = m_1, ..., m_t$ in $l$ bit blocks,
           - for $i \in \{1, ..., t\}$ let $c_i = F(sk, c_i + i) ⊕ m_i$ 
     - $\text{Dec}(sk, c)$ for $c \in \{0, 1\}^{tl}$
       - Parse $c = c_0 c_1 ... c_t$ as length $l$ blocks,
         - for $i \in \{1, ..., m\}$ let $m_i = c_i ⊕ F^{-1}(sk, c_i + i)$.

4. MAC-CBC

     - $F: \{0, 1\}^n \times \{0, 1\}^l$ is a block cipher, 
       $\mathcal{M} = \{0, 1\}^{tl}$
       $ \mathcal{T} = \{0, 1\}^l$
       $\mathcal{K} = \{0, 1\}^n$
     - $\text{KGen}$ samples $sk$ uniformly from $\mathcal{K}$. 
         - $\text{Tag}(ik, m)$
           - Let $\tau_0 = 0^l$, for $i \in \{1, ..., t\}$ let $\tau_i = F_ik (\tau_{i-1} ⊕ m_i)$, return $\tau = \tau_t$.
     - $V_f(ik, m, \tau)$
       - If $m \notin \mathcal{M}$, i.e., $m$ has a length different to $tl$, output 0, else
         - calculate $\tau' = \text{Tag} (ik, m)$ and return 1 if and only if $\tau' = \tau$.

---

##### 4. Number Theory

1. Femart's Little Theorm

   - For $a$ and $n$ relatively prime, and $n$ prime:
     $$
     a^{n-1} = 1 (\text{ mod } n)
     $$

2. Euler Totient Function

   - Definition
     - $\phi(n)$ is the number of non-negative integers less than $n$ which are relatively prime to $n$, 
   - Extended Femart's Little Theorm:
     - $a^{\phi(n)} = 1 (\text{ mod } n)$ for all relatively prime $a, n$. 

3. Chinese Remainder Theorm

   - Definition

     - Let $k \in \mathbb{N}$. If $n_1, n_2, ..., n_k$ are pairwise relatively prime positive integers, then for any $a_1, a_2, ..., a_k \in \mathbb{Z}$ the following system of congruences:
       $$
       \begin{array}{lll}
       x &= &a_1 (\text{ mod } n_1) \\
       x &= &a_2 (\text{ mod } n_2) \\
       ... \\
       x &= &a_k (\text{ mod } n_k) \\
       \end{array}
       $$
       has a solution, and the solution is unique modulo $N$, where $N = n_1 \cdot n_2 \cdot ... \cdot n_k$. 

4. Group

   - Definition
     - A Group is a set $\mathbb{G}$ along with a binary operation $\circ : \mathbb{G} \times \mathbb{G} \mapsto \mathbb{G}$ that satisfies the following properties:
       - Clouser
         - $\forall g, h \in \mathbb{G}, g \circ h \in \mathbb{G}$
       - Identity
         - $\exist e \in \mathbb{G}$ such that $\forall g \in \mathbb{G}, g \circ e = e \circ g = g$
       - Inverse
         - $\forall g \in \mathbb{G}, \exist g^{-1} \in \mathbb{G}$ such that $g \circ g^{-1} = g^{-1} \circ g = e$
       - Associativity
         - $\forall g_1, g_2, g_3 \in \mathbb{G}, (g_1 \circ g_2) \circ g_3 = g_1 \circ (g_2 \circ g_3)$
   - Order
     - Order of a finite group $\mathbb{G}$ is the **number of elements** in $\mathbb{G}$. 
     - Order of an element $g \in \mathbb{G}$, denoted as $\text{ord}(g)$, is the **smallest positive integer** $m$ such that $g^m = 1$. 

5. Albelian Group

   - Definition
     - Besides the basic definition of a group, an albelian group satisfies:
       $\forall g, h \in \mathbb{G}, g \circ h = h \circ g$

6. Cyclic Group

   - Definition
     - A group $\mathbb{G}$ is **cyclic** if there exists a group element $g \in \mathbb{G}$, such that $G = \{g^k | k \in \mathbb{Z}\}$
       We call $g$ a generator of $\mathbb{G}$ and denote $\mathbb{G} = \langle g \rangle$. 
     - Every cyclic group is an albelian group. 

---

##### 5. Asymmetric Key Encryption

1. Public-key Encryption Scheme (PKE)
   - Definition
     - $\text{PKE} = (\text{KGen}, \text{Enc}, \text{Dec})$ under $\mathcal{M, C}$ is given as
       - $\text{KGen}(1^n)$: on input $1^n$ outputs a pair of keys $(pk, sk)$, 
       - $\text{Enc}(pk, m)$: on input of a public key $pk$, and a message $m \in \mathcal{M}$, outputs a ciphertext $c \in \mathcal{C}$, 
       - $\text{Dec}(sk, c)$ (Deterministic algorithm): on input of a secret key $sk$ and a ciphertext $c$ outputs message $m \in \mathcal{M}$. 
     
   - Correctness
   
     - $\forall n \in \mathbb{N}, (pk, sk) \leftarrow \text{KGen} (1^n)$ and $\forall m \in \mathcal{M}$ holds
       $$
       \Pr[\text{Dec}(sk, \text{Enc}(pk, m)) = m] = 1
       $$
       where the probability is taken over the random coins of $\text{KGen}$ and $\text{Enc}$. 
     
   - Secrutiy 
     - IND-CPA
       - Adversaries, given encryption oracle $\text{Enc}(pk,\cdot)$, cannot distinguish between the ciphertexts of two chosen plaintexts.
       - Let $(pk,sk) \leftarrow KGen(1^n)$:
         - $\mathcal{A}$ submit two plaintexts $m_0, m_1 \in \mathcal{M}$ with equal length.
         - Challenger selects a random bit $b \in \{0, 1\}$, and computes the challenge ciphertext $c^* = \text{Enc}(pk, m_b)$. 
         - $\mathcal{A}$ outputs a guess $b' \in \{0, 1\}$, attempting to determine whether $c^*$ corresponds to $m_0$ or $m_1$. 
       - The encryption scheme is **IND-CPA secure** if for any efficient adversary AA, the probability of correctly guessing bb is at most negligibly better than random guessing. 
   
1. Textbook RSA Encryption
   - Definition
     - $\text{KGen}(1^n)$:
       - Set two large prime numbers $p, q$, and let $N = p \cdot q$. 
       - Set a small integer $e$, set $d$ such that $e \cdot d \equiv 1 (\mod \phi(N))$
       - Output pair of public and secret keys as $pk := (N, e), sk := (N, d)$. 
     - $\text{Enc}(pk, m)$: Takes as input the public key, $pk= (N, e)$, and a message $m \in \mathbb{Z}^*_N$ and outputs $c := [m^e \mod N]$. 
     - $\text{Dec}(sk, c)$: Takes as input the secret key, $sk = (N, d)$, and a ciphertext $c \in \mathbb{Z}^*_N$, and it outputs $[c^d \mod N]$.
   - Correctness
     - $m = \text{Dec}(sk, \text{Enc}(pk, m)) = m^{ed} \mod N = m^{k\phi(N)+1} \mod M$, or
     - $m = 0$
   - Security
     - RSA Problem: Given a large product of two large prime numbers $N = p \cdot q$, an index $e$ and ciphertext $c$, find $m$ that satisfies $c \equiv m^e (\mod N)$. 
     - Insecure. Construct 
   
1. Diffie-Hellman Key-exchange
   - Definition
     - Assume Alice and Bob. 
     - Generate group parameters $(\mathbb{G}, q, g) \leftarrow \text{Gen}(1^n)$. Then, Alice samples $a \leftarrow \mathbb{Z}_q$ and sends $A := g^a$ to Bob, Bob samples $b \leftarrow \mathbb{Z}_q$ and sends $B := g^b$ to Alice. 
     - Alice computes the shared key as $B^a$ and Bob computes the shared key as $A^b$. 
   - Security
     - Insecure, since there might be *man-in-the-middle attack*.
   
1. El Gamal Encryption Scheme
   - Definition
     - $\text{KGen}(1^n)$: Set group parameters $(\mathbb{G}, g, q)$, where $g$ is a generator and $q$ is a large prime number, sample a random number $x \leftarrow \mathbb{Z}_q$ and output a public key $pk := (\mathbb{G}, g, q, h = g^x)$ and a secret key $sk := (\mathbb{G}, g, q, x)$.
     
     - $\text{Enc}(pk, m)$: For $m \in \mathbb{G}$, sample a random number $r \leftarrow \mathbb{Z}_q$ and then output the ciphertext
       $$
       (c_1, c_2) := (g^r, h^r \cdot m)
       $$
     
     - $\text{Dec}((c_1, c_2), sk)$: Output $\widetilde{m} := c_2 / c_2^x$. 
   - Correctness
   
     - $\widetilde{m} = \frac{c_2}{c_1^x} = \frac{h^r \cdot m}{g^{rx}} = \frac{g^{rx} \cdot m}{g^{rx}} = m$
   - Security
     - Discret Logarithm Problem: Given a generator $g$, a prime number $q$, find a integer $x$ such that $g^x \equiv h (\mod q)$. 

---

##### 6. Digital Signature

1. Digital Signature Scheme
   - Definition
     
     - A **SIG** with message space $M$ and signature space $S$ consists of a tuple of efficient algorithms $(\text{KGen, Sign}, V_f)$ given as
       - $\text{KGen}(1^n)$: On input $1^n$ outputs a pair of public & secret keys ($pk, sk$), 
       - $\text{Sign}(sk, m)$: On input of a secret key $sk$ and message $m \in \mathcal{M}$, it outputs a signature $\sigma \in \mathcal{S}$,
       - $V_f(pk, m, \sigma)$ (deterministic algorithm): on input of a public key $pk$, message $m \in \mathcal{M}$ and a signature $\sigma \in \mathcal{S}$, outputs a bit, with $b=1$ meaning valid and $b=0$ meaning invalid. 
   - Correctness

     - $\forall n \in \mathbb{N}$, $(pk, sk) \leftarrow \text{KGen}(1^n)$ and $\forall m \in \mathcal{M}$ it holds:
       $$
       \Pr[V_f(pk, m, \text{Sign}(sk, m)) = 1] = 1
       $$
       where the probability is taken over the random coins of $\text{KGen}$ and $\text{Sign}$.
     
   - EUF-CMA security

     - Adversaries given $\text{Sign}(ik, \cdot)$ cannot forge a tag on a message they do not know a signature for.
     - Let $(pk, sk) \leftarrow \text{KeyGen}(1^{\lambda}), Q = \empty$
       - Each time Adverserary $\mathcal{A}$ queries $\text{Sign}(sk, m)$, the queried message $m$ is added to the query set $Q$. 
       - $\text{Sign}(sk, m)$ outputs a signature $\sigma$ corresponding to $m$.
       - $\mathcal{A}$ wins if $V_f(pk, m^*, \sigma^*) = 1$ and $m^* \notin Q$. 
     - If no adversary wins with probability (meaningfully) more than zero, then the signature scheme is **EUF-CMA secure**.

2. Textbook RSA Signature

   - Definition
     - $\text{KGen}(1^n)$: 
       - Set two large prime numbers $p, q$, and let $N = p \cdot q$. 
       - Set a small integer $e$, set $d$ such that $e \cdot d \equiv 1 (\mod \phi(N))$
       - Output pair of public and secret keys as $pk := (N, e), sk := (N, d)$. 
     - $\text{Sign}(sk, m)$: Takes as input the secret key, $sk = (N, d)$, and a message $m \in \mathbb{Z}^*_N$ and outputs $\sigma := [m^d \mod N]$.
     - $V_f(pk, m, \sigma)$: Takes as input the public key $pk = (N, e)$, message $m$ and a signature $\sigma \in \mathbb{Z}^*_N$, and output 1 if and only if $m = [\sigma^e \mod N]$.
   - Correctness
     - $m = (m^d)^e \mod N = m^{de} \mod N = m$
   - Security
     - Insecure. 
     - Adversary can construct a new signature $\sigma = (m_1 \cdot m_2)^d \mod N$ if they know two signatures $\sigma_1 = m_1^d \mod N, \sigma_2 = m_2^d \mod N$ 
   
3. RSA-FDH Signature

   - Motivation
     - Introduce Hash functions to convert messages into fixed length message before signature. 
   - Definition
     - Similair like Textbook RSA Signature, but replace all the $m$ as $H(m)$. 
   - Correctness
     - Similair like Textbook RSA Signature. 
   
4. Hash Functions

   - Definition
     - $H(x)$ is a cryptographic hash function if it is additionally
       - **One-way** (or pre-image resistant). Given $y$, it is hard to compute an x where $H(x) = y$.
       - And usually either
         - **2nd-preimage resistance**: It is computationally infeasible to find a second input that has the same output as any specified input, i.e., given $x$ to find a second $x' \neq x$ such that $H(x) = H(x')$.
         - **Collision resistance**: It is difficult to find two distinct inputs $x, x'$ where $H(x) = H(x')$. 

---

##### 7. Identification Scheme 

1. Zero-Knowledge Proofs
   - Definition
     - The verifier (or any eavesdropper) cannot learn any information about the witness $w$ from the interaction.
     - This is usually proven by showing that one can perfectly simulate honest transcripts without knowing the witness $w$.
   - Security
     - **Correctness**: For an honest prover, the verifier always accepts.
     - **Soundness**: Any cheating prover, who does not know the witness $w$, cannot convince the verifier to accept with probability meaningfully more than zero.
   
2. Identification Scheme

   - Definition
     - **ID scheme** is an interactive proof, where the prover wants to prove its identity, e.g. to authenticate themselves when logging in to a website. The relation considered for identification schemes is of the following form:
       - $R(x, w) = 1 \iff w = sk$ is a valid corresponding secret key for the public key $x = pk$.
     - Textbook RSA PKE relation
       - Let $x = (N,e)$ be the RSA public key. Then, $R(x,w) = 1$ if and only if $w = (N,d)$ where $e \cdot d = 1 (\mod N)$.
     - El Gamal PKE relation
       - Let $x = (\mathbb{G},g,q,h)$ be the El Gamal public key. Then, $R(x,w) = 1$ if and only if $w = (\mathbb{G},g,q,x)$ where $g^x = h$.

3. Three-round Identification scheme

     - Definition
       - An ID scheme is a tuple of polynomial-time algorithms $Π = (\text{KGen}, P_1, P_2, V)$, where the latter two are deterministic.
       - Here $C_{pk}$ is the challenge space dependent on the public key $pk$.

     - Process
       1. The prover $P_1$ generate a state $state$ and a promise $I$ using the secret key $(I, state) \leftarrow P_1(sk)$, and send $I$ to the verifier $V$.
       2. The verifier $\mathcal{V}$ generates a random challenge from the challenge space $c \leftarrow \mathcal{C}_{pk}$, and send $c$ to the prover. 
       3. The prover $P_2$ computes a response by the secret key, state and challenge $r \leftarrow P_2(sk, state, c)$, and sends the result $r$ to the verifier.
       4. The verifier $\mathcal{V}$ uses the public key, the challenge, the response, and the promise to verify $\mathcal{V}(pk, c, r) \overset{?}= I$. If holds, the verifier accpets $I$ as valid. Otherwise rejects it. 
       
     - Correctness
       - ID scheme $Π = (\text{KGen}, P_1, P_2, \mathcal{V})$ satisfies (perfect) correctness if $\forall n \in \mathbb{N}, (pk, sk) \leftarrow \text{KGen}(1^n), (I, state) \leftarrow P_1(sk)$ and $c \leftarrow \mathcal{C}_{pk}$, it holds that
         $$
         \Pr[\mathcal{V}(pk, c, P_2(sk, state, c)) = I] = 1
         $$


4. Fiat-Shamir ID Scheme
   - Definition
     - Let $(N, p, q) \leftarrow \text{GenMod}(1^n)$. 
     - Pick $s \leftarrow \mathbb{Z}^∗_N$ and set $v = [s^2 \mod N]$. Then, 
       $$
       pk = (N, v) \\
       sk = (N, s)
       $$
     
   - Correctness
   
     - Since $z^2 = r^2 \cdot s^{2c} = I \cdot (s^2)^c = I \cdot v^c (\mod N)$, we can get
     - $z^2 \cdot v^{-c} = I (\mod N)$
   
   - Security
     - Sqr problem: Given $(N,v)$ it is hard to find its square root modulo $N$ such that $x^2 \mod N = y$. 
     
     - A malicious prover can convince the verifier with probability 1/2 as follows. The attacker hopes that $c=0$ and sends $I = [z^2 \mod N]$ for $z \leftarrow \mathbb{Z}^*_N$.
     
     - Given a challenge $c$, if $c=0$ then  the attacker outputs $z$ generated earlier. Then
       $$
       z^2 \cdot v^{−c} = z^2 \cdot v^{−0} = z^2= I (\mod N)
       $$
       and thus the verifier accepts.
     
     - If computing square roots is hard relative to $\text{GenMod}$ then the Fiat-Shamir ID scheme $Π$ is 1/2-secure.
     
       - The proof sketch *(non-examinable)* consists of <u>two important observations</u>:
         1. transcripts can be perfectly simulated without knowing the secret key.
         2. given two transcripts $(I , c_0, z_0), (I , c_1, z_1)$ with the same first message $I$, but two distinct challenges $c_0 \neq c_1$, one can efficiently extract the secret $sk$ (or in other words, compute the square root).
     
   - Fiat-Shamir Transformation
   
     - Definition
     
       - It is a widely used (in practice) transform <u>from identification schemes to digital signatures</u>. 
       - Interaction is removed by the prover simply computing $c = H(I)$ where $H: \{0, 1\}^* \to \mathcal{C}_{pk}$ is a hash function. 
       - 
     - Correctness
       - 
   
5. Schnorr ID Scheme
   - Definition
     - Let $\text{Gen}$ be a group generator algorithm and $(\mathbb{G}, g, q) \leftarrow \text{GenMod}(1^n)$. Then, pick $x \leftarrow \mathbb{Z}_q$ and set $h=g^x$. Then, 
       $$
       pk = (\mathbb{G}, g, q, h) \\
       sk = (\mathbb{G}, g, q, x)
       $$
     
   - Correctness
   
     - Since $g^z =g^r \cdot g^{cx} = I \cdot (g^x)^c = I\cdot h^c$, we can get
     - $g^z \cdot h^{−c} = I$
   
   - Security
   
     - Security relies on the discrete logarithm assumption (if $\mathbb{G} = ⟨g⟩$ is a cyclic group).
     - A **malicious** prover can convince the verifier with probability $1/q$ as follows. The attacker hopes that $c=0$ and sends $I=g^z$ for $z \leftarrow \mathbb{Z}^q$.
     - Given a challenge $c$, if $c = 0$ then the attacker outputs $z$ generated earlier. Then
       $$
       g^z \cdot h^{-c} = g^z \cdot h^{-0} = g^z = I
       $$
     
     - Suppose $\text{Gen}$ always outputs descriptions $(\mathbb{G},g,q)$ of prime order groups. If the discrete logarithm problem is hard relative to $\text{Gen}$ then the Schnorr ID scheme is $1/q$-secure.

---

##### 8. Lattice-Based Crytography

1. LWE Scheme
   - Definition


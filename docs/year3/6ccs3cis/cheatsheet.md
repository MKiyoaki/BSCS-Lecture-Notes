### Cheat Sheet

##### 0. Historical Ciphers

1. Caeser Cipher
   - Definition
     - In the framework of SKE the encryption has $\mathcal{M, C, K}$ such that
       - $\text{KGen}$ returns a $sk$ sampled from $\mathcal{K}$,
       - $\text{Enc}(sk, m)$:
         Parse $m = m_1m_2 ... m_l$ as individual characters. For $i \in \{1, ..., l\}$ let $c_i = m_i + sk \mod 26$ and return $c = c_1c_2 ... c_l$
       - $\text{Dec}(sk, c)$:
         Parse $c = c_1c_2 ... c_l$ as individual characters. For $i \in \{1, ..., l\}$ let $m_i = c_i - sk \mod 26$ and return $m = m_1m_2 ... m_l$
   - Security
     - No. Adversary can use select message attack by sending $m = 0$  such that $\text{Enc}(sk, m) = sk$
2. Mono-alphabetic Cipher
   - Definition
     - In the framework of SKE the encryption has $\mathcal{M, C, K}$, define $\alpha = \{a, b, ... z\}$, such that
       - $\text{KGen}$ returns a $sk$ sampled from $\mathcal{K}$, where $sk: \alpha \to \alpha$ is a permutation,
       - $\text{Enc}(sk, m)$: Parse $m = m_1m_2 ... m_l$ as individual characters. Return $c = sk(m_1) ... sk(m_l)$, 
       - $\text{Dec}(sk, c)$: Parse $c = c_1c_2 ... c_l$ as individual characters. Return $m = sk^{-1}(c_1) ... sk^{-1}(c_l)$. 
   - Security
     - No. Adversary can use select message attack by sending $m = \texttt{abcdefg...xzy}$ to observe the mapping relationship for the encryption algorithm. 
3. Vegenère Cipher
   - Definition
     - In the framework of SKE the encryption has $\mathcal{M, C, K}$ such that
       - $\text{KGen}$ returns a $sk$ sampled from $\mathcal{K}$ as $sk = sk_1 sk_2 ... sk_n$
       - $\text{Enc}(sk, m)$: Parse $m = m_1m_2...m_n...m_l$ as individual characters. $c_i=(m_i + sk_i) \mod 26$. Return $c$. 
       - $\text{Dec}(sk, c)$: Parse $c = c_1c_2...c_n...c_l$ as individual characters. $m_i=(c_i - sk_i) \mod 26$. Return $c$. 
   - Security
     - No. Adversary can use select message attack by sending $m = \texttt{aaaa...aaaa}$ with a length of $l$ such that $\text{Enc}(sk, m) = sk$. 
4. One-time Pad
   - Definition
     - In the framework of SKE the encryption has $\mathcal{M, C, K}$ such that
       - $\text{KGen}$ returns a $sk$ sampled from $\mathcal{K}$,
       - $\text{Enc}(sk, m) = sk[l] \oplus m$
       - $\text{Dec}(sk, c) = sk[l] \oplus c$
   - Security
     - Secure if the secret key is only used for once. 

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
     - Basically, $|\mathcal{C}| \geq |\mathcal{M}|$ is essential to guarantee the security of SKE. 
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
             Let $f_1, ..., f_r$ be *round functions*, defined as $f_i: \{0, 1\}^{n^{'}} \times \{0, 1\}^{l/2} \to \{0, 1\}^{l/2}$, 
             
             - For the first round: 
               - $f_1: \{0, 1\}^{n^{'}} \times \{0, 1\}^{l/2} \to \{0, 1\}^{l/2}$
               - $m^{(0)} = m = L_0 \parallel R_0$. 
             - For each round $i$ after the first round:
               
               > Should always behave as a uniform permutation
               
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
         - for $i \in \{1, ..., m\}$ let $m_i = c_{i-1} ⊕ F^{-1}_{sk}(c_i)$.
     - Insecure:
       - Assume $x = \{0, 1\}^l / \{0\}^l$, construct $m' = (m_1 \oplus x)m_2m_3....m_t$.
       - Constrcut $c_0' = c_0 \oplus x \implies c = c_0'c_1...c_t$
       - $c_1' = F_{sk}(m_1' \oplus c_0') = F_{sk}((m_1 \oplus x) \oplus (c_0 \oplus x)) = F_{sk}(m_1 \oplus c_0) = c_1$
3. Counter Mode - Secure, parallel

     - $F: \{0, 1\}^n \times \{0, 1\}^l$ is a block cipher, 
       $\mathcal{M}=\mathcal{C}=\cup_{i \in \mathbb{N}}\{0, 1\}^{il}$
       $\mathcal{K} = \{0, 1\}^n$
     - $\text{KGen}$ samples $sk$ uniformly from $\mathcal{K}$. 
     - $\text{Enc}(sk, m)$ for $m \in \{0, 1\}^{tl}$
       - Sample uniform $c_0 = IV \in \{0, 1\}^l$ and parse $m = m_1, ..., m_t$ in $l$ bit blocks,
       - for $i \in \{1, ..., t\}$ let $c_i = F_{sk}(c_i + i) ⊕ m_i$ , return $c$. 
     - $\text{Dec}(sk, c)$ for $c \in \{0, 1\}^{tl}$
       - Parse $c = c_0 c_1 ... c_t$ as length $l$ blocks,
         - for $i \in \{1, ..., m\}$ let $m_i = c_i ⊕ F^{-1}_{sk}(c_i + i)$.
4. MAC-CBC

     - $F: \{0, 1\}^n \times \{0, 1\}^l$ is a block cipher, 
       $\mathcal{M} = \{0, 1\}^{tl}$
       $ \mathcal{T} = \{0, 1\}^l$
       $\mathcal{K} = \{0, 1\}^n$
     - $\text{KGen}$ samples $sk$ uniformly from $\mathcal{K}$. 
     - $\text{Tag}(ik, m)$:
       
       - Let $\tau_0 = 0^l$, for $i \in \{1, ..., t\}$ let $\tau_i = F_{ik} (\tau_{i-1} ⊕ m_i)$, return $\tau = \tau_0\tau_t$.
       
     - $V_f(ik, m, \tau)$:
     
       - If $m \notin \mathcal{M}$, i.e., $m$ has a length different to $tl$, output 0, else
       
       - calculate $\tau' = \text{Tag} (ik, m)$ and return 1 if and only if $\tau' = \tau$.
     - Inscure: 
       - Assume $x = \{0, 1\}^l / \{0\}^l$, construct $m' = (m_1 \oplus x)m_2m_3....m_t$.
       - Constrcut $\tau_0' = \tau_0 \oplus x \implies \tau = \tau_0'\tau_1...\tau_t$
       - $\tau_1' = F_{ik}(m_1' \oplus \tau0') = F_{ik}((m_1 \oplus x) \oplus (\tau_0 \oplus x)) = F_{ik}(m_1 \oplus \tau_0) = \tau_1$

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
   - Two special groups
     - Group $(\mathbb{Z}_N, +)$
       - For $N \in \mathbb{N}$, let $\mathbb{Z}_N := \{0, ..., N-1 \}$
       - The group$(\mathbb{Z}_N, +)$ is an *abelian group* (under addition modulo $N$) of order $|\mathbb{Z}_N| = N$.
     - Group $(\mathbb{Z}_N^*, \cdot)$
       - For $N \in \mathbb{N}$, let $\mathbb{Z}_N^* := \{ a \in \{1, ..., N-1 \} | \gcd(a, N) = 1 \}$
       - The group $(\mathbb{Z}_N^*, \cdot)$ is an  *abelian group* (under multiplication modulo $N$) of order $|\mathbb{Z}_N^*| = \phi(N)$. 

5. Albelian Group

   - Definition
     - Besides the basic definition of a group, an albelian group satisfies:
       $\forall g, h \in \mathbb{G}, g \circ h = h \circ g$

   - Largrange's Theorem
     - Given a finite (abelian) group $\mathbb{G}$ and $m = |\mathbb{G}|$. For any element $g \in \mathbb{G}$, we have
       $$
       g^m = 1
       $$

6. Cyclic Group

   - Definition
     - A group $\mathbb{G}$ is **cyclic** if there exists a group element $g \in \mathbb{G}$, such that $G = \{g^k | k \in \mathbb{Z}\}$
       We call $g$ a generator of $\mathbb{G}$ and denote $\mathbb{G} = \langle g \rangle$. 
     - Every cyclic group is an albelian group. 
   - Property
     - Assume $g$ is a generator of $\mathbb{G}$, then $\text{ord}(g) = |\mathbb{G}|$. 
     - Assume a group $\mathbb{G}$, if $|\mathbb{G}|$ is prime, then every element in the group is a group generator. 

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
       - The encryption scheme is **IND-CPA secure** if for any efficient adversary $\mathcal{A}$, the probability of correctly guessing bb is at most negligibly better than random guessing. 
   
2. Textbook RSA Encryption
   - Definition
     - $\text{KGen}(1^n)$:
       - Set two large prime numbers $p, q$, and let $N = p \cdot q$. 
       - Set a small integer $e$, set $d$ such that $e \cdot d \equiv 1 (\mod \phi(N))$
       - Output pair of public and secret keys as $pk := (N, e), sk := (N, d)$. 
     - $\text{Enc}(pk, m)$: Takes as input the public key, $pk= (N, e)$, and a message $m \in \mathbb{Z}^*_N$ and outputs $c := [m^e \mod N]$. 
     - $\text{Dec}(sk, c)$: Takes as input the secret key, $sk = (N, d)$, and a ciphertext $c \in \mathbb{Z}^*_N$, and it outputs $[c^d \mod N]$.
   - Correctness
     - $\gcd(m, N) =1$: $m = \text{Dec}(sk, \text{Enc}(pk, m)) = m^{ed} \mod N = m^{k\phi(N)+1} \mod M$, or
     - $\gcd(m, N) = p \text{ or }q$: $p|m \implies m = k \cdot p \implies m^{ed} - m = k(m^{ed-1} - 1)$ and same holds for $q$. Since $N = pq$ thus the correctness holds. 
     - $m = 0$: Travail. 
   - Security
     - RSA Problem: Given a large product of two large prime numbers $N = p \cdot q$, an index $e$ and ciphertext $c$, find $m$ that satisfies $c \equiv m^e (\mod N)$. 
     - Insecure. Assume $m^* = m_1 \cdot m_2$, access $\text{Enc}(pk, \cdot)$ to generate two ciphertext $c_1, c_2$. $\text{Enc}(pk, m^*) = (m_1m_2)^e = c_1c_2$. 
   
3. Diffie-Hellman Key-exchange
   - Definition
     - Assume Alice and Bob. 
     - Generate group parameters $(\mathbb{G}, q, g) \leftarrow \text{Gen}(1^n)$. Then, Alice samples $a \leftarrow \mathbb{Z}_q$ and sends $A := g^a$ to Bob, Bob samples $b \leftarrow \mathbb{Z}_q$ and sends $B := g^b$ to Alice. 
     - Alice computes the shared key as $B^a$ and Bob computes the shared key as $A^b$. 
   - Security
     - Insecure, since there might be *man-in-the-middle attack*.
   
4. El Gamal Encryption Scheme
   - Definition
     - $\text{KGen}(1^n)$: 
       - Set group parameters $(\mathbb{G}, g, q)$, where $g$ is a **generator** and $q$ is the **order of group** $\mathbb{G}$, sample a random number $x \leftarrow \mathbb{Z}_q$ 
       - Output a public key $pk := (\mathbb{G}, g, q, h = g^x)$ and a secret key $sk := (\mathbb{G}, g, q, x)$.
     - $\text{Enc}(pk, m)$: For $m \in \mathbb{G}$, sample a random number $r \leftarrow \mathbb{Z}_q$ and then output the ciphertext
       $$
       (c_1, c_2) := (g^r, h^r \cdot m)
       $$
     
     - $\text{Dec}((c_1, c_2), sk)$: Output $\widetilde{m} := c_2 / c_1^x$. 
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
     
     - $V_f(pk ,m, \sigma = \text{Sign}(sk, m))$ checkes whether
       $$
       \begin{array}{lll}
       [\sigma^e \mod N] &= &[[H(m)^d \mod N]^e \mod N]\\
       ~ &= &[H(m)^{ed} \mod N] \\
       ~ &= &[H(m)^{k \phi(N) + 1} \mod N] \\
       ~ &= &[H(m) \mod N] \\
       ~ &= &H(m)
       \end{array}
       $$
   
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
     - **ID scheme** is an interactive proof, where the prover wants to prove its identity. The relation considered for identification schemes is of the following form:
       - $R(x, w) = 1 \iff w = sk$ is a valid corresponding secret key for the public key $x = pk$.

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
     - Let $(N, p, q) \leftarrow \text{GenMod}(1^n)$, i.e., $p, q$ are large prime numbers and $N = p \cdot q$. 
       1. Pick $s \leftarrow \mathbb{Z}^∗_N$ and set $v = [s^2 \mod N]$. Then, 
       $$
         pk = (N, v) \\
         sk = (N, s)
       $$
       2. Sample a random number $r \in \mathbb{Z}^*_N$ such that $I = r^2 \mod N$ and a random challenge value $c \in \{0, 1\}$. A response can be calcuate by 
       $$
        z = 
         \begin{cases}
         r & c = 0\\
         r \cdot s & c=1
         \end{cases}
       $$
       3. The verify function is defined as follows
       $$
         z^2 \equiv
         \begin{cases}
         I \mod N &c = 0\\
         I \cdot v \mod N & c = 1\\
         \end{cases}
       $$
         If the equation holds then the verification is done. 
     
   - Correctness
     - Since $z^2 = r^2 \cdot s^{2c} = I \cdot (s^2)^c = I \cdot v^c (\mod N)$, we can get
       $$
       z^2 \cdot v^{-c} = \frac{z^2}{s^{2c}} \implies \frac{r^2\cdot s^{2c}}{s^{2c}} = r^2 = I (\mod N)
       $$

   - Security
     - Sqr problem: Given $(N,v)$ it is hard to find its square root modulo $N$ such that $x^2 \mod N = y$. 
     - A malicious prover can convince the verifier with probability 1/2 as follows. The attacker hopes that $c=0$ and sends $I = [z^2 \mod N]$ for $z \leftarrow \mathbb{Z}^*_N$.
     - Given a challenge $c$, if $c=0$ then  the attacker outputs $z$ generated earlier. Then
       $$
       z^2 \cdot v^{−c} = z^2 \cdot v^{−0} = z^2= I (\mod N)
       $$
       and thus the verifier accepts.

     - If computing square roots is hard relative to $\text{GenMod}$ then the Fiat-Shamir ID scheme $Π$ is 1/2-secure.

5. Schnorr ID Scheme
   - Definition
     - Let $\text{Gen}$ be a group generator algorithm and $(\mathbb{G}, g, q) \leftarrow \text{GenMod}(1^n)$, where $g$ is the generator.
       1. Pick $x \leftarrow \mathbb{Z}_q$ and set $h=g^x$. Then, 
       $$
         pk = (\mathbb{G}, g, q, h) \\
         sk = (\mathbb{G}, g, q, x)
       $$
       2. Sample a random number $r \in \mathbb{Z}^*_N$ such that $I = g^r \mod q$. Verifier sample a random challenge $r \in \mathbb{Z}_q$ and send it to the prover. Compute a reponse such that 
          $$
          z = r + c \cdot x \mod |\mathbb{G}|
          $$
     
       3. The verify function is defined as follows
          $$
          g^z \mod q \overset{?}= I \cdot h^c \mod q
          $$
          If the equation holds then the verification is done. 
       
     
   - Correctness

     - Since $g^z =g^r \cdot g^{cx} = I \cdot (g^x)^c = I\cdot h^c$, we can get
     - $g^z \cdot h^{−c} \mod N = I \mod N$
   - Security
     - Security relies on the discrete logarithm assumption (if $\mathbb{G} = ⟨g⟩$ is a cyclic group).
     - A **malicious** prover can convince the verifier with probability $1/q$ as follows. The attacker hopes that $c=0$ and sends $I=g^z$ for $z \leftarrow \mathbb{Z}^q$.
     
       - Given a challenge $c$, if $c = 0$ then the attacker outputs $z$ generated earlier. Then
     
     
     $$
       g^z \cdot h^{-c} = g^z \cdot h^{-0} = g^z = I
     $$
     
       - Suppose $\text{Gen}$ always outputs descriptions $(\mathbb{G},g,q)$ of prime order groups. If the discrete logarithm problem is hard relative to $\text{Gen}$ then the Schnorr ID scheme is $1/q$-secure.
   
6. Fiat-Shamir Transformation

   - Definition
     - It is a widely used (in practice) transform <u>from identification schemes to digital signatures</u>. 
     - Interaction is removed by the prover simply computing $c = H(I)$ where $H: \{0, 1\}^* \to \mathcal{C}_{pk}$ is a hash function. 
     - Let $(\text{KGen}_{id}, P_1, P_2, \mathcal{V})$ be an identification scheme, and construct a signature scheme as follows:
       - $\text{KGen}$: On input $1^n$, run $(pk, sk) \leftarrow \text{KGen}_{id}$. The public key $pk$ determines the challenge space $\mathcal{C}_{pk}$. A function $H: \{0, 1\}^* \to \mathcal{C}_{pk}$ is specified but we leave this implicit.
       - $\text{Sign}$: On input a private key $sk$ and a message $m \in \{0, 1\}^*$, do the following:
         1. $(I, state) \leftarrow P_1(sk)$
         2. $c = H(I, m)$
         3. $z = P_2(sk, state, c)$
            Output the signature $\sigma = (c, z)$
     
       - $V_f$: On input a public key $pk$, a message $m$ and a signature $σ = (c, z)$, compute $I := \mathcal{V}(pk,c,z)$ and output 1 if and only if $H(I,m) = c$.
   - Correctness
     - Let $(\text{KGen}_{id}, P_1, P_2, \mathcal{V})$ be a correct identification scheme. Then, the Fiat-Shamir transformed signature scheme from Construction above satisfies correctness.
   - Security
     - Let $(\text{KGen}_{id}, P_1, P_2, \mathcal{V})$ be a secure (0-secure) identification scheme. Then, the Fiat-Shamir transformed signature scheme from Construction above satisfies **EUF-CMA security**, assuming $H$ is an "ideal random function".
     - It is obtained by applying the Fiat-Shamir transform on the Schnorr ID scheme.

---

##### 8. Lattice-Based Crytography

1. LWE Scheme
   - Definition
   
     - Implicitly assume that $m, q,$ and distribution $\mathcal{X}$ are determined by $n$.
   
     - $\text{KGen}(1^n)$: Sample $A \leftarrow \mathbb{Z}_q^{m×n},s \leftarrow \mathbb{Z}_q^n$ and sample $e \leftarrow \mathcal{X}$. If $\parallel e \parallel > q / (8m)$, resample until $\parallel e \parallel \leq q / (8m)$ (to guarantee **shortness**). Set $b := A \cdot s + e$ and output
       $$
       pk:= (A, b) \\
       sk:= s
       $$
   
     - $\text{Enc}(pk, \mu)$: On input of a public key $pk = (A, b)$ and message $\mu \in \{0, 1\}$, sample a random value $r \leftarrow \{0, 1\}^m$ and output ciphertext $c:= (A^T \cdot r, b^T \cdot r + \mu \cdot [q/2])$.
   
     - $\text{Dec}(sk, c)$: On input of a secret key $sk = s$ and cipher text $c = (u, v)$, output is
       $$
       \mu :=
       \begin{cases}
       0 &\text{if } |v - s^Tu| < q/4\\
       1 &\text{else}
       \end{cases}
       $$
   
   - Correctness
   
   - Security


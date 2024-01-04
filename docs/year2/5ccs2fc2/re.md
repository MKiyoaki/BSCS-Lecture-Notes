## REVISION

>[🏠 MENU - 5CCS2FC2](year2/5ccs2fc2.md)
>
>Exam Information:
>
>- 2 hours
>- 10-11 questions
>  - MCQ and typing questions
>

#### 1. Calculation

- WEEK I

  - Regular language

- WEEK IV

  - SCC and topological sorting
  - Find MST by Prim or Kruskal Algorithm

- WEEK V
  - Master Theorem

- WEEK VI

  - 2-OPT Algorithm

    > Steps
    >
    > 1. Find the MST
    > 2. Preorder the MST
    > 3. Take 2 edge and swap the connection
    > 4. Until there is no possible improvement

- WEEK V

  - N SAT reduction

    > e.g.
    >
    > 3 SAT to 2 SAT
    >
    > 1. Convert formula to implications
    > 2. Make list of all literals

- WEEK VI
  
  - Calculate Prob
  
- WEEK VII

  - Simplex Method

#### 2. Proof

>May give a template in proof

- WEEK II

  - Something is NP

    >e.g.
    >
    >- CLIQUE is NP, HAMILTONIAN is NP
    >
    >- New problem that belongs to NP
    >
    > >Steps:
    > >
    > >1. Cannot be solved in polynomial time
    > >
    > >   (for SAT like, take all solutions; for HAMILTONIAN like, take all orderring results. And there are expontential solutions at last)
    > >2. Can construct deterministic turing machine and run it in parallel in polynomial time
    > >
    > >Explain the reasons for cannot be or can be run in polynomial time is important

  - Something is P (i.e., can be solved in polynomial time)

  - Someting is NP-hard - Lower Bound questions

    >Generally by transitive and reduction the problem to SAT/CLIQUE/HAMILTONIAN

  - SAT$\leq_p$CLIQUE or SAT$\leq_p$HAMILTONIAN

- WEEK III

  - Cook-Levin Theorem for some specific result

- WEEK IX

  - The Halting Problem - Prove something is undecidable by *contradiction*
  
    > Can swap to Accept or Reject Problem


#### 3. Some Definitions

- WEEK I
  - Language - Note that empty set is the subset of every set
  
- WEEK II
  - P and NP - note that P belongs to NP
  - Satisfiable
  - Computable - terminating in polynomial time
  - Polynomial reducible
  
    >$X \leq_p Y \implies$
    >
    >there is a computable function $f: \Sigma^* \to \Sigma^*$ such that for $w \in X$ exist $f(w) \in Y$
  
- WEEK VI
  - R-Approxiable and Unapproxiable
  
    >A language or program is R-Approxiable if the local optium value is at most R times of global optium value. 
    >
    >If cannot find R then it is unapproxiable. 
  
- WEEK IX
  - Decidable & Undecidable
  
    >A language is **decidable** if
    >
    >Sound, Complete and Termating
    >
    >> Note that **an undecidable language must be NP-hard** as well
  
  - BPP & ZPP
  
    - BPP
  
      >Probabilistic Sound
      >
      >Probabilistic Complete
      >
      >NP
  
    - ZPP
  
      >Sound
      >
      >Complete
      >
      >Expected to be NP
  
- WEEK X
  
  - Mapping reduction
  
    >$w \in A \implies f(w) \in B$
  
  - C.E.
  
    >Sound, Complete
  
  - co-C.E.
  
    >If $L$ has a complement $\overline{L}$ that is C.E.
  

#### 4. Some Algorithms

- WEEK IV
  - BFS and DFS - $O(|V|+|E|)$
  - Find MST
    - Kruskal - $O(|E|\log|E|)$
    - Prim - $O|E|+|V | \log |V |$
- WEEK VI
  - 2-OPT Algorithm for TSP

- WEEK V
  
  - DPLL
  
    >1. Pure Literal Elimination
    >2. Unit Propagation
    >3. Traceback
    >
  
- WEEK IX

  - Calculate BPP and ZPP
    - Monte Carlo - Always fast, maybe correct
    - Las Vegas - Always correct, maybe fast


# Compiler Design & Theory of Computation
## Comprehensive Notes — BUET MSc Admission Preparation

> **Exam Format**: 50 MCQ (50 marks, −0.5 per wrong), 10 Written (100 marks)
> This document is your **single source of truth**. All concepts are explained for both quick MCQ recall and in-depth written answers.

---

# TABLE OF CONTENTS

## Part I — Theory of Computation
1. [Formal Languages & Chomsky Hierarchy](#1-formal-languages--chomsky-hierarchy)
2. [Finite Automata (DFA & NFA)](#2-finite-automata-dfa--nfa)
3. [Regular Languages & Regular Expressions](#3-regular-languages--regular-expressions)
4. [Context-Free Grammars (CFG)](#4-context-free-grammars-cfg)
5. [Pushdown Automata (PDA)](#5-pushdown-automata-pda)
6. [Turing Machines](#6-turing-machines)
7. [Decidability & Undecidability](#7-decidability--undecidability)
8. [Complexity Theory — P, NP, NP-Complete](#8-complexity-theory--p-np-np-complete)

## Part II — Compiler Design
9. [Phases of a Compiler](#9-phases-of-a-compiler)
10. [Lexical Analysis](#10-lexical-analysis)
11. [Syntax Analysis (Parsing)](#11-syntax-analysis-parsing)
12. [LL(1) Parsing](#12-ll1-parsing)
13. [LR Parsing (SLR, LALR, CLR)](#13-lr-parsing-slr-lalr-clr)
14. [Semantic Analysis](#14-semantic-analysis)
15. [Intermediate Code Generation](#15-intermediate-code-generation)
16. [Code Optimization](#16-code-optimization)
17. [Code Generation](#17-code-generation)
18. [Symbol Table & Error Recovery](#18-symbol-table--error-recovery)

---

# PART I — THEORY OF COMPUTATION

---

## 1. Formal Languages & Chomsky Hierarchy

### Written Explanation

A **formal language** is a set of strings over an alphabet Σ. Noam Chomsky classified all formal languages into a hierarchy of four types, each recognized by a corresponding machine.

```
        CHOMSKY HIERARCHY

  +------------------------------------------+
  |  Type 0: Recursively Enumerable (RE)     |  <-- Turing Machine
  |  +------------------------------------+  |
  |  |  Type 1: Context-Sensitive (CSL)  |  |  <-- Linear Bounded Automaton
  |  |  +------------------------------+ |  |
  |  |  |  Type 2: Context-Free (CFL)  | |  |  <-- Pushdown Automaton
  |  |  |  +------------------------+  | |  |
  |  |  |  | Type 3: Regular (RL)   |  | |  |  <-- Finite Automaton
  |  |  |  +------------------------+  | |  |
  |  |  +------------------------------+ |  |
  |  +------------------------------------+  |
  +------------------------------------------+
```

| Type | Language | Grammar Rule Form | Machine |
|------|----------|-------------------|---------|
| Type 0 | Recursively Enumerable | alpha -> beta (any) | Turing Machine |
| Type 1 | Context-Sensitive | alphaAbeta -> alphagammabeta, LHS <= RHS | Linear Bounded Automaton |
| Type 2 | Context-Free | A -> gamma (single non-terminal on left) | Pushdown Automaton |
| Type 3 | Regular | A -> aB or A -> a | Finite Automaton |

**Key Properties:**
- Type 3 ⊂ Type 2 ⊂ Type 1 ⊂ Type 0 (proper subset relationship)
- Regular languages are **closed** under: Union, Concatenation, Kleene star, Complement, Intersection
- CFLs are **closed** under: Union, Concatenation, Kleene star
- CFLs are **NOT closed** under: Intersection, Complement
- Intersection of two CFLs can be a CSL (not necessarily CFL)

### MCQ Quick Facts
- epsilon-production (A -> epsilon) allowed in Type 2 but strictly prohibited in Type 1 (except for start symbol)
- Every Regular Language is also a CFL (but not vice versa)
- Programming language syntax is mostly Type 2 (CFG)
- Natural language is roughly Type 1

---

## 2. Finite Automata (DFA & NFA)

### Written Explanation

#### Deterministic Finite Automaton (DFA)
A DFA is a 5-tuple M = (Q, Sigma, delta, q0, F) where:
- **Q**: finite set of states
- **Sigma**: finite input alphabet
- **delta**: Q x Sigma -> Q (transition function, exactly one next state)
- **q0 in Q**: start state
- **F ⊆ Q**: set of accept/final states

**DFA for strings ending in "ab" over {a, b}:**

```
         a              b
    +--------+      +--------+
    |        |      |        |
    v   b    v  a   v   b    v
--> q0 ----> q1 ---> q2 [ACCEPT]
    ^                |
    |   a            | a
    +----------------+

  q0: start (no progress)
  q1: saw 'a'
  q2: saw 'ab' (accept)

  Transitions:
  delta(q0, a) = q1,  delta(q0, b) = q0
  delta(q1, a) = q1,  delta(q1, b) = q2
  delta(q2, a) = q1,  delta(q2, b) = q0
```

#### Nondeterministic Finite Automaton (NFA)
An NFA is M = (Q, Sigma, delta, q0, F) where:
- **delta**: Q x (Sigma union {epsilon}) -> 2^Q (transition function returns a SET of states)
- Can have multiple choices and epsilon-transitions
- Accepts if ANY computation path leads to an accept state

**NFA for strings containing "ab":**
```
         a             b
  --> q0 ---> q1 ---> q2 [ACCEPT]
       |
       | a,b (self-loop)
       v

  delta(q0, a) = {q0, q1}
  delta(q0, b) = {q0}
  delta(q1, b) = {q2}
  delta(q2, a) = {q2}, delta(q2, b) = {q2}
```

#### NFA to DFA Conversion (Subset Construction)
Every NFA can be converted to an equivalent DFA via the **subset construction algorithm**.

- Each DFA state = a subset of NFA states
- DFA has at most **2^n** states for an NFA with n states
- The resulting DFA may have **exponentially more** states
- But both accept the same language (equivalence)

**Algorithm:**
```
1. Compute epsilon-closure of start state -> initial DFA state
2. For each DFA state S and each symbol a:
   - Compute epsilon-closure(delta(q, a)) for all q in S
   - This union = new DFA state
3. Repeat until no new states
4. DFA accept states = any subset containing an NFA accept state
```

#### epsilon-NFA
Has epsilon (epsilon) transitions — moves without consuming input.

**epsilon-closure(q)** = set of all states reachable from q via epsilon-transitions only (including q itself)

#### DFA Minimization (Myhill-Nerode / Table-Filling Algorithm)
```
1. Remove unreachable states
2. Mark all pairs {accept, non-accept} as distinguishable
3. For each pair {p, q} and each symbol a:
   If delta(p,a) and delta(q,a) are distinguishable -> mark {p,q}
4. Repeat until no new pairs marked
5. Unmarked pairs = equivalent states (merge them)
```

**Myhill-Nerode Theorem:** A language L is regular if and only if its number of equivalence classes (under the right-invariant equivalence relation) is finite. The minimum DFA has exactly as many states as equivalence classes.

### MCQ Quick Facts
- DFA: delta: QxSigma->Q (deterministic, total function)
- NFA: delta: Qx(Sigma union {epsilon})->2^Q (nondeterministic, returns set)
- NFA and DFA accept **exactly the same class** of languages (Regular Languages)
- NFA -> DFA: subset construction, may yield 2^n states
- DFA minimization: table-filling algorithm
- Number of states in minimum DFA = number of Myhill-Nerode equivalence classes
- A language is **not** regular if it requires unbounded memory (e.g., {a^n b^n | n>=0})

---

## 3. Regular Languages & Regular Expressions

### Written Explanation

#### Pumping Lemma for Regular Languages
**Used to PROVE a language is NOT regular (cannot prove regularity)**

**Statement:** If L is regular, then there exists pumping length p such that every string w in L with |w| >= p can be split as w = xyz where:
1. |xy| <= p
2. |y| >= 1  (y is non-empty)
3. For all i >= 0: xy^i z in L

**How to apply (contradiction proof):**
```
1. Assume L is regular with pumping length p
2. Choose a specific string w in L with |w| >= p
3. Consider ALL possible splits w = xyz satisfying |xy| <= p, |y| >= 1
4. Show that for SOME i, xy^i z not in L
5. Contradiction -> L is not regular
```

**Example:** Prove {a^n b^n | n >= 0} is not regular.
- Choose w = a^p b^p, |w| = 2p >= p
- Any split: x = a^i, y = a^j (j >= 1), z = a^(p-i-j) b^p (since |xy| <= p)
- Pump i=2: xy^2 z = a^(p+j) b^p -> more a's than b's -> not in L. Contradiction!

#### Regular Expressions (RE)
**Basis:**
- empty set: empty language
- epsilon: language containing only empty string
- a (for each a in Sigma): language {a}

**Inductive (Operations):**
- **Union:** R1 | R2 or R1 + R2
- **Concatenation:** R1 R2
- **Kleene Star:** R* (zero or more repetitions)
- **Plus:** R+ = RR* (one or more)
- **Optional:** R? = R | epsilon

**Operator Precedence (high to low):** Star > Concatenation > Union

**RE to NFA (Thompson's Construction):**
```
For symbol 'a':
  --> [s] ---a---> [accept]

For R1 | R2:
  New start --epsilon--> [NFA R1] --epsilon--> New accept
  New start --epsilon--> [NFA R2] --epsilon--> New accept

For R1 R2:
  [start] --[NFA R1]--> [intermediate] --[NFA R2]--> [accept]

For R*:
  New start --epsilon--> [NFA R] --epsilon--> New accept
  New accept --epsilon--> New start  (loop back)
  New start --epsilon--> New accept  (zero repetitions)
```

**Closure Properties of Regular Languages:**

| Operation | Closed? |
|-----------|---------|
| Union | YES |
| Intersection | YES |
| Complement | YES |
| Concatenation | YES |
| Kleene Star | YES |
| Difference (L1 - L2) | YES |
| Reverse | YES |
| Homomorphism | YES |

### MCQ Quick Facts
- RE, DFA, NFA, epsilon-NFA all describe **exactly** the same class: Regular Languages
- Kleene's theorem: RE <-> Finite Automata
- Pumping lemma: tool to prove non-regularity only
- {a^n b^n}, {ww | w in Sigma*}, {a^n | n is prime} are NOT regular
- Finite languages are always regular (can build DFA with finite paths)
- (a|b)* = Sigma* over {a,b} — all strings
- a*b* = {a^i b^j | i,j >= 0}

---

## 4. Context-Free Grammars (CFG)

### Written Explanation

A CFG is G = (V, T, P, S) where:
- **V**: set of variables (non-terminals)
- **T**: set of terminals
- **P**: set of production rules (A -> alpha, where A in V, alpha in (V union T)*)
- **S in V**: start variable

**Derivation:** S => alpha1 => alpha2 => ... => w

**Leftmost derivation:** Always expand the leftmost non-terminal
**Rightmost derivation:** Always expand the rightmost non-terminal

#### Parse Trees
A parse tree visually shows derivation structure:
```
Grammar: E -> E+E | E*E | (E) | id

Parsing: id + id * id

         E
       / | \
      E  +  E
      |    / | \
      id  E  *  E
          |     |
          id    id

This tree has TWO leftmost derivations -> AMBIGUOUS grammar!
```

#### Ambiguity
A grammar is **ambiguous** if a string has:
- More than one parse tree, OR
- More than one leftmost derivation, OR
- More than one rightmost derivation

**Disambiguating arithmetic expressions (classic approach):**
```
Ambiguous:  E -> E+E | E*E | (E) | id

Unambiguous (respects precedence + left-associativity):
  E  -> E + T | T
  T  -> T * F | F
  F  -> (E) | id
```

**Inherently Ambiguous CFL:** Some CFLs cannot be described by ANY unambiguous grammar.
- Example: {a^i b^j c^k | i=j or j=k}

#### Normal Forms

**Chomsky Normal Form (CNF):**
Every production is either:
- A -> BC (two non-terminals)
- A -> a (single terminal)
- S -> epsilon (only if epsilon in L(G))

**Conversion to CNF:**
```
Step 1: Eliminate epsilon-productions
   - Find all nullable variables (those deriving epsilon)
   - For each production with nullable var, add version without it

Step 2: Eliminate unit productions (A -> B)
   - Find unit pairs (A,B) where A =>* B
   - Add A -> alpha for each B -> alpha

Step 3: Eliminate useless symbols
   - Remove non-generating symbols (can't derive terminals)
   - Remove unreachable symbols

Step 4: Convert remaining long productions
   - A -> B1 B2 ... Bn (n>2)
     becomes: A->B1 C1, C1->B2 C2, ..., C(n-2)->B(n-1) Bn

Step 5: Convert terminals in mixed productions
   - A -> aB -> add Ta->a, replace: A -> Ta B
```

**Greibach Normal Form (GNF):**
Every production: A -> a alpha where a in T, alpha in V*
(Each production starts with exactly one terminal)

**CFL Pumping Lemma:**
If L is CFL, there exists pumping length p such that every w in L with |w| >= p can be split as w = uvxyz where:
1. |vxy| <= p
2. |vy| >= 1  (v or y is non-empty)
3. For all i >= 0: u v^i x y^i z in L

**Proves {a^n b^n c^n | n>=0} is not CFL:**
- w = a^p b^p c^p, split as uvxyz with |vxy| <= p
- vxy cannot span all three symbol types (it covers at most 2 types)
- Pumping distorts the equal count -> not in L. Contradiction!

#### Decision Properties of CFLs
| Problem | Decidable? |
|---------|-----------|
| Is w in L(G)? | YES (CYK algorithm) |
| Is L(G) empty? | YES |
| Is L(G) infinite? | YES |
| Is L(G) = L(G')? | NO (undecidable) |
| Is L(G) regular? | NO (undecidable) |
| Is G ambiguous? | NO (undecidable) |

**CYK Algorithm (Membership):**
- Input: Grammar in CNF, string w = a1 a2 ... an
- Dynamic programming O(n^3 * |G|)
- Table: X[i,j] = set of variables that derive w[i..j]
- X[i,i] = {A | A -> a_i in P}
- X[i,j] = union over all k: {A | A->BC, B in X[i,k], C in X[k+1,j]}

### MCQ Quick Facts
- CFG recognizes Type 2 languages
- All regular languages are context-free
- {a^n b^n | n>=0} is CFL but NOT regular
- {a^n b^n c^n | n>=0} is NOT CFL
- CNF: all rules are A->BC or A->a
- GNF: all rules are A->a alpha (useful for PDA construction)
- CFLs NOT closed under intersection and complement
- CFL intersect Regular Language = CFL (closed!)
- Ambiguity is undecidable for CFGs
- CYK runs in O(n^3)

---

## 5. Pushdown Automata (PDA)

### Written Explanation

A PDA is a 7-tuple M = (Q, Sigma, Gamma, delta, q0, Z0, F) where:
- **Q**: finite set of states
- **Sigma**: input alphabet
- **Gamma**: stack alphabet
- **delta**: Q x (Sigma union {epsilon}) x Gamma -> finite subsets of Q x Gamma* (transition)
- **q0**: start state
- **Z0 in Gamma**: initial stack symbol
- **F ⊆ Q**: accept states

**Configuration (Instantaneous Description):** (q, w, gamma) — state, remaining input, stack contents

**Transition notation:** delta(q, a, X) = {(p, alpha)} means:
- In state q, reading input 'a' (or epsilon), with X on top of stack
- Move to state p, replace X with alpha on stack

**Two acceptance modes (equivalent power):**
1. **Acceptance by final state:** Accept when input is exhausted AND in a final state
2. **Acceptance by empty stack:** Accept when input is exhausted AND stack is empty

```
PDA for {a^n b^n | n >= 1}:

  States: q0 (reading a's), q1 (reading b's), q2 (accept)
  Stack alphabet: {Z, A}  (Z = bottom marker, A = pushed for each 'a')

  Transitions:
  delta(q0, a, Z) = {(q0, AZ)}   -- push A for first 'a'
  delta(q0, a, A) = {(q0, AA)}   -- push A for each subsequent 'a'
  delta(q0, b, A) = {(q1, eps)}  -- switch to reading b's, pop one A
  delta(q1, b, A) = {(q1, eps)}  -- pop A for each 'b'
  delta(q1, e, Z) = {(q2, Z)}    -- only Z left -> accept

  Trace for "aabb":
  (q0, aabb, Z)
  --> (q0, abb,  AZ)    [read a, push A]
  --> (q0, bb,  AAZ)    [read a, push A]
  --> (q1, b,    AZ)    [read b, pop A]
  --> (q1, eps,   Z)    [read b, pop A]
  --> (q2, eps,   Z)    [epsilon-move to accept]   ACCEPTED
```

**Deterministic PDA (DPDA):**
- No ambiguity in transitions
- DPDA is strictly less powerful than NPDA
- DPDA can recognize {a^n b^n} but NOT palindromes {ww^R}

**CFG to PDA conversion (via GNF):**
- For each production A -> a alpha: push alpha when reading a with A on top of stack
- For each terminal: match and pop

### MCQ Quick Facts
- PDA = CFG in expressive power (both recognize exactly CFLs)
- PDA + stack recognizes exactly Context-Free Languages
- Acceptance by final state is equivalent to acceptance by empty stack
- NPDA is more powerful than DPDA
- Deterministic CFLs: {a^n b^n}, balanced parentheses
- Non-deterministic CFLs (require NPDA): palindromes {ww^R}

---

## 6. Turing Machines

### Written Explanation

A Turing Machine (TM) is the most powerful computational model.
TM = (Q, Sigma, Gamma, delta, q0, q_accept, q_reject) where:
- **Q**: finite set of states
- **Sigma**: input alphabet (subset of Gamma, blank symbol B not in Sigma)
- **Gamma**: tape alphabet (includes blank B)
- **delta**: Q x Gamma -> Q x Gamma x {L, R} (new state, write symbol, move direction)
- **q0**: start state
- **q_accept**: accept state
- **q_reject**: reject state

**The Tape:**
```
  Infinite to the right (blank-padded)

  [ B | B | a | a | b | b | B | B | B ]
                ^
              head

  - Can read, write, move left or right
  - Blank (B) represents empty cells
```

**Configuration:** (q, u a v) where q = current state, head is at 'a', u = left of head, v = right of head

**TM for {a^n b^n c^n | n >= 0}:**
```
Idea: Repeatedly find one a, one b, one c and mark them.

Phase 1: Scan right, replace leftmost 'a' with X
Phase 2: Continue right, replace leftmost 'b' with Y
Phase 3: Continue right, replace leftmost 'c' with Z
Phase 4: Scan left back to leftmost X
Phase 5: Repeat phases 1-4 until no more 'a's
Phase 6: Scan entire tape — only X, Y, Z and blanks -> ACCEPT
         Otherwise -> REJECT
```

**Variants of Turing Machines (all equivalent in power):**
| Variant | Description |
|---------|-------------|
| Multi-tape TM | Multiple tapes with separate heads |
| Nondeterministic TM | Multiple transition choices at each step |
| Enumerator | Prints (enumerates) all strings in a language |
| 2-way infinite tape | Tape extends in both directions |

**All variants recognize exactly the same class of languages (Recursively Enumerable).**

**Church-Turing Thesis:** Any algorithm (intuitively computable) can be implemented as a Turing Machine. This is a **thesis** (accepted hypothesis), not a provable theorem.

### MCQ Quick Facts
- TM recognizes Recursively Enumerable (RE) languages
- TM with multi-tape or nondeterminism: same power, possibly different time complexity
- A TM can loop forever (neither accept nor reject) on some inputs
- Decidable languages = Recursive languages (TM always halts)
- RE but not recursive: TM accepts members but may loop on non-members (e.g., A_TM)

---

## 7. Decidability & Undecidability

### Written Explanation

**Decidable (Recursive) Language:** A language L is decidable if there exists a TM M that:
- Accepts all w in L (and halts)
- Rejects all w not in L (and halts)
- M always halts on all inputs

**Recognizable (RE) Language:** A TM M that:
- Accepts all w in L
- But may loop on w not in L

**Classification of Important Languages:**
```
DECIDABLE:
  A_DFA     = {<M,w> | DFA M accepts w}
  A_NFA     = {<M,w> | NFA M accepts w}
  A_REX     = {<R,w> | RE R matches w}
  E_DFA     = {<M>   | L(M) = empty}
  EQ_DFA    = {<M1,M2> | L(M1) = L(M2)}
  A_CFG     = {<G,w> | CFG G derives w}       (CYK algorithm)
  E_CFG     = {<G>   | L(G) = empty}
  HALT_DFA  = always decidable (DFA always halts)

RE BUT NOT DECIDABLE:
  A_TM      = {<M,w> | TM M accepts w}
  HALT_TM   = {<M,w> | TM M halts on w}

NOT EVEN RE (complement not RE either):
  E_TM      = {<M> | L(M) = empty}
  EQ_TM     = {<M1,M2> | L(M1) = L(M2)}
  EQ_CFG    = {<G1,G2> | L(G1) = L(G2)}

NOT RE (complement is RE):
  Complement of A_TM
```

**The Halting Problem (HALT_TM) — Undecidable:**
Proof by diagonalization (contradiction):
```
Assume H exists and decides HALT_TM.

Build machine D:
  D(M):
    Run H on <M, M>
    If H accepts <M,M>  (M halts on M) -> LOOP FOREVER
    If H rejects <M,M>  (M loops on M) -> ACCEPT

Run D on its own description D:
  Case 1: H accepts <D,D>
    -> D halts on D (by assumption)
    -> But D loops when H accepts -> CONTRADICTION

  Case 2: H rejects <D,D>
    -> D loops on D (by assumption)
    -> But D accepts when H rejects -> CONTRADICTION

Therefore, H cannot exist. HALT_TM is undecidable.
```

**Reduction (Mapping Reduction A <=_m B):**
- A <=_m B means: "A is no harder than B"
- If B is decidable, then A is decidable
- Contrapositive: If A is undecidable, then B is undecidable
- A_TM <=_m HALT_TM  (proves HALT_TM is undecidable)
- A_TM <=_m E_TM     (proves E_TM is undecidable)

**Rice's Theorem:**
Any non-trivial property of the language recognized by a TM is undecidable.
- **Trivial property:** True for ALL TMs or false for ALL TMs
- **Non-trivial:** True for some TMs, false for others

```
ALL of these are UNDECIDABLE (by Rice's Theorem):
  Is L(M) regular?
  Is L(M) context-free?
  Is L(M) finite?
  Does L(M) contain any string?
  Is L(M) = Sigma*?
  Is epsilon in L(M)?
```

**Relationship: Decidable, RE, co-RE:**
```
  L is decidable  <=>  L is RE AND L-complement is RE
  L is RE         <=>  L is recognized by some TM
  L is decidable  =>   L is RE (but not necessarily vice versa)
  
  A_TM: RE but not decidable
  Complement of A_TM: not RE (co-RE)
  
  Decidable = RE intersection co-RE
```

### MCQ Quick Facts
- Decidable = Recursive (TM always halts)
- Recognizable = RE (TM may loop)
- Decidable is strictly contained in RE
- A_TM is RE but not decidable
- Complement of A_TM is NOT RE
- Rice's Theorem: all non-trivial semantic properties of TMs are undecidable
- Post Correspondence Problem (PCP): undecidable
- L is decidable if and only if both L and its complement are RE

---

## 8. Complexity Theory — P, NP, NP-Complete

### Written Explanation

**Time Complexity Classes:**
```
  COMPLEXITY HIERARCHY (believed, not all proven)

  +-----------------------------------------------+
  |  EXPTIME                                      |
  |  +-------------------------------------------+|
  |  |  PSPACE                                   ||
  |  |  +--------------------------------------+ ||
  |  |  |  NP                   co-NP          | ||
  |  |  |  +----------+   +----------+         | ||
  |  |  |  |     P    |   |          |         | ||
  |  |  |  | (P=NP?)  |   |          |         | ||
  |  |  |  +----------+   +----------+         | ||
  |  |  |  NP-Complete (at the intersection)   | ||
  |  |  +--------------------------------------+ ||
  |  +-------------------------------------------+|
  +-----------------------------------------------+
```

**P (Polynomial Time):**
Problems solvable in O(n^k) time by a deterministic TM.
Examples: Sorting, shortest path (Dijkstra), MST, 2-SAT, bipartite matching.

**NP (Nondeterministic Polynomial):**
Problems where a proposed solution (certificate) can be **verified** in polynomial time.
Equivalently: Solvable in polynomial time by a nondeterministic TM.

**co-NP:** Complement of NP. L in NP implies L-complement in co-NP.

**NP-Hard:** Every NP problem polynomial-time many-one reduces to it.
(May or may not be in NP itself)

**NP-Complete = NP intersect NP-Hard** — the hardest problems in NP.

**Polynomial-time reduction (A <=_p B):**
- If A <=_p B and B in P, then A in P
- If A <=_p B and A is NP-Hard, then B is NP-Hard

**Cook-Levin Theorem:** SAT (Boolean satisfiability) is NP-Complete — historically the first NP-Complete problem proved.

**Classic NP-Complete Problems:**
| Problem | Description |
|---------|-------------|
| SAT | Is a CNF boolean formula satisfiable? |
| 3-SAT | SAT with exactly 3 literals per clause |
| Vertex Cover | <=k vertices touching all edges? |
| Clique | Clique of size k in graph? |
| Independent Set | Independent set of size k? |
| Hamiltonian Path/Circuit | Simple path/cycle visiting all vertices? |
| Traveling Salesman (TSP) | Min cost tour visiting all cities? |
| Subset Sum | Subset summing to target value? |
| Graph 3-Coloring | Color with 3 colors (no adjacent same color)? |
| Partition | Split set into two equal-sum halves? |

**Reduction Chain:**
```
SAT <=_p 3-SAT <=_p CLIQUE <=_p INDEPENDENT SET <=_p VERTEX COVER
3-SAT <=_p 3-COLORABILITY
3-SAT <=_p HAMILTONIAN CIRCUIT <=_p TSP
SAT <=_p SUBSET SUM

Each arrow means: left problem reduces to right problem
(proving right problem is NP-Hard if left is NP-Hard)
```

**How to prove X is NP-Complete:**
```
Step 1: Show X is in NP
         (Give a polynomial-time verifier for a certificate)

Step 2: Show X is NP-Hard
         (Reduce a known NP-Complete problem Y to X)
         (Y <=_p X means: any instance of Y can be transformed to
          an instance of X in poly time, preserving yes/no answer)
```

**P vs NP:** The greatest open question in computer science.
If P = NP: all NP problems become efficiently solvable; cryptography breaks.
If P != NP (believed): NP-Complete problems have no efficient algorithms.

**Space Complexity:**
- **L (LOGSPACE):** O(log n) space
- **NL:** Nondeterministic log space
- **PSPACE:** Polynomial space
- **Savitch's Theorem:** NSPACE(f(n)) ⊆ DSPACE(f(n)^2)
- **Hierarchy:** L ⊆ NL ⊆ P ⊆ NP ⊆ PSPACE ⊆ EXPTIME

### MCQ Quick Facts
- P ⊆ NP (whether equal is unknown; believed P != NP)
- NP-Complete: in NP AND every NP problem reduces to it
- NP-Hard: every NP problem reduces to it (may not be in NP)
- TSP optimization: NP-Hard; TSP decision: NP-Complete
- 2-SAT is in P; 3-SAT is NP-Complete
- 2-Colorability (bipartite checking) is in P; 3-coloring is NP-Complete
- Euler circuit (all edges): P; Hamiltonian circuit (all vertices): NP-Complete
- Shortest path: P; Longest path: NP-Complete

---

# PART II — COMPILER DESIGN

---

## 9. Phases of a Compiler

### Written Explanation

A compiler translates source code to target code through a pipeline of phases:

```
SOURCE PROGRAM (character stream)
         |
         v
  +-----------------+
  | LEXICAL ANALYSIS|  (Scanner / Tokenizer)
  |  produces:      |
  |  Token stream   |
  +-----------------+
         |
         v
  +-----------------+
  | SYNTAX ANALYSIS |  (Parser)
  |  produces:      |
  |  Parse Tree/AST |
  +-----------------+
         |
         v
  +-----------------+
  |SEMANTIC ANALYSIS|  (Type checking, Scope)
  |  produces:      |
  |  Annotated AST  |
  +-----------------+
         |
         v
  +-------------------+
  | INTERMEDIATE CODE |
  |    GENERATION     |  (Three-address code)
  +-------------------+
         |
         v
  +-----------------+
  | CODE OPTIMIZER  |  (Machine-independent optimizations)
  +-----------------+
         |
         v
  +-----------------+
  |  CODE GENERATOR |  (Target machine code)
  +-----------------+
         |
         v
  +-----------------+
  | CODE OPTIMIZER  |  (Machine-dependent / Peephole)
  +-----------------+
         |
         v
TARGET PROGRAM (machine code)

  Supporting Modules (used across all phases):
    +------------------+    +------------------+
    |  Symbol Table    |    |  Error Handler   |
    |  Manager         |    |                  |
    +------------------+    +------------------+
```

**Front-end:** Lexical + Syntax + Semantic Analysis (analysis of source; machine-independent)
**Back-end:** IR Gen + Optimization + Code Generation (synthesis of target; machine-dependent)

| Phase | Input | Output | Error Type |
|-------|-------|--------|-----------|
| Lexical Analysis | Character stream | Token stream | Lexical error |
| Syntax Analysis | Token stream | Parse tree/AST | Syntax error |
| Semantic Analysis | Parse tree | Annotated AST | Semantic error |
| IR Generation | Annotated AST | Three-address code | — |
| Optimization | IR code | Optimized IR | — |
| Code Generation | Optimized IR | Target code | — |

### MCQ Quick Facts
- 6 main phases of a compiler
- Symbol table: central data structure, used by ALL phases
- First phase: Lexical Analysis; Last phase: Code Generation
- Front-end is machine-independent; back-end is machine-dependent
- Preprocessor runs BEFORE compiler (handles #include, #define in C)
- Assembler converts assembly -> object code; Linker combines object files
- Interpreter: directly executes source; Compiler: produces target code

---

## 10. Lexical Analysis

### Written Explanation

The **lexical analyzer** (scanner) reads source characters and groups them into **tokens**.

**Token, Lexeme, Pattern:**
- **Token:** Category/type of lexical unit (e.g., IDENTIFIER, NUMBER, KEYWORD)
- **Lexeme:** Actual character sequence matched (e.g., "count", "42", "if")
- **Pattern:** Rule (regex) describing what strings constitute a token

**Example tokenization:**
```
Source code: count = 42 + total;

Tokens produced:
  <IDENTIFIER, "count">
  <ASSIGN_OP, "=">
  <INTEGER_LITERAL, "42">
  <ADD_OP, "+">
  <IDENTIFIER, "total">
  <SEMICOLON, ";">
```

**Lexer Implementation Pipeline:**
```
  Specification (Regular Expressions)
          |
          v
  NFA  (Thompson's Construction)
          |
          v
  DFA  (Subset Construction)
          |
          v
  Minimized DFA
          |
          v
  Lexer (table-driven or direct-coded)
```

**Typical Token Categories with Patterns:**
```
Keywords:     if | while | for | return | int | float | ...
Operators:    [+\-*/=<>!&|]+  (or specific patterns)
Delimiters:   [(){}\[\];,]
Identifiers:  [a-zA-Z_][a-zA-Z0-9_]*
Integers:     [0-9]+
Floats:       [0-9]+\.[0-9]+([eE][+-]?[0-9]+)?
Strings:      "([^"\\]|\\.)*"
Comments:     //[^\n]*  or  /\*[\s\S]*?\*/
```

**Lexical Error Handling:**
- Unrecognized character (e.g., `@` in C)
- Unclosed string literal
- Recovery strategies: delete offending character, skip to next whitespace

**Conflict Resolution (Lex/Flex rules):**
1. **Longest match:** Always match the longest possible string
   - "iffy" -> IDENTIFIER, not IF + IDENTIFIER("fy")
2. **First rule wins:** Among equally-long matches, use first rule
   - Keywords listed before IDENTIFIER rule to take priority

### MCQ Quick Facts
- Lexer uses DFA (Finite Automaton) to recognize tokens
- Longest match rule: "ifx" -> IDENTIFIER (not keyword IF)
- Keywords usually handled by keyword lookup table after matching as identifier
- Whitespace and comments typically ignored by lexer
- Lexical errors: unrecognized token (character not in language)
- Tools: Lex (Unix), Flex (GNU), ANTLR
- Lexer is also called: scanner, tokenizer

---

## 11. Syntax Analysis (Parsing)

### Written Explanation

The **parser** checks if the token sequence conforms to the grammar and builds a parse tree.

**Two main parsing strategies:**
```
  TOP-DOWN PARSING                   BOTTOM-UP PARSING
  
  Starts at root (start symbol)      Starts at leaves (tokens)
  Builds tree downward               Builds tree upward
  Uses leftmost derivation           Uses rightmost derivation (in reverse)
  LL parsers: LL(1), Recursive       LR parsers: LR(0), SLR(1), LALR(1), CLR(1)
  Descent
  
  Predict which production to use    Shift tokens, reduce by productions
  Uses FIRST and FOLLOW sets         Uses Action/Goto tables
```

**Problems with Top-Down Parsing:**

**Left Recursion** — causes infinite loop in top-down parsing:
```
A -> A alpha | beta

Eliminate by rewriting:
A  -> beta A'
A' -> alpha A' | epsilon

Example:
E -> E + T | T
becomes:
E  -> T E'
E' -> + T E' | epsilon
```

**Left Factoring** — needed when multiple productions share a common prefix:
```
A -> alpha beta | alpha gamma
Rewrite:
A  -> alpha A'
A' -> beta | gamma

Example:
stmt -> if E then S | if E then S else S
becomes:
stmt -> if E then S stmt'
stmt' -> else S | epsilon
```

**FIRST Sets** — terminals that can begin strings derived from a symbol:
```
FIRST(a)    = {a}               for terminal a
FIRST(eps)  = {eps}
FIRST(A):
  For each production A -> X1 X2 ... Xn:
    Add FIRST(X1) - {eps}
    If eps in FIRST(X1): add FIRST(X2) - {eps}
    ...
    If eps in FIRST(Xi) for all i: add eps
```

**FOLLOW Sets** — terminals that can appear immediately after non-terminal A:
```
FOLLOW(S) contains $             (start symbol)
For each production B -> alpha A beta:
  Add FIRST(beta) - {eps} to FOLLOW(A)
  If eps in FIRST(beta): add FOLLOW(B) to FOLLOW(A)
For each production B -> alpha A:
  Add FOLLOW(B) to FOLLOW(A)
```

**FIRST/FOLLOW Example:**
```
Grammar:
  E  -> T E'
  E' -> + T E' | epsilon
  T  -> F T'
  T' -> * F T' | epsilon
  F  -> ( E ) | id

FIRST(F)  = {(, id}
FIRST(T)  = FIRST(F) = {(, id}
FIRST(E)  = FIRST(T) = {(, id}
FIRST(T') = {*, epsilon}
FIRST(E') = {+, epsilon}

FOLLOW(E)  = {$, )}
FOLLOW(E') = FOLLOW(E) = {$, )}
FOLLOW(T)  = FIRST(E')-{eps} union FOLLOW(E') = {+, $, )}
FOLLOW(T') = FOLLOW(T) = {+, $, )}
FOLLOW(F)  = FIRST(T')-{eps} union FOLLOW(T') = {*, +, $, )}
```

### MCQ Quick Facts
- Top-down: LL parsers (Left-to-right scan, Leftmost derivation)
- Bottom-up: LR parsers (Left-to-right scan, Rightmost derivation in reverse)
- Predictive parsing requires FIRST and FOLLOW sets
- Left recursion must be eliminated for LL parsing
- LR parsers are strictly more powerful than LL parsers
- Syntax errors: unexpected token, missing expected token

---

## 12. LL(1) Parsing

### Written Explanation

**LL(1):** Left-to-right scan, Leftmost derivation, **1 token lookahead**

**LL(1) Parsing Table Construction:**
```
For each production A -> alpha:
  For each terminal a in FIRST(alpha):
    Add A -> alpha to M[A, a]
  If epsilon in FIRST(alpha):
    For each b in FOLLOW(A):
      Add A -> alpha to M[A, b]
    If $ in FOLLOW(A):
      Add A -> alpha to M[A, $]
```

**Grammar is LL(1) if and only if:**
- Every cell M[A, a] has at most one entry (no conflicts)
- Equivalently: No ambiguity, no left recursion, and for each pair of alternatives A->alpha|beta, FIRST(alpha) and FIRST(beta) are disjoint (and if one can derive epsilon, the other's FIRST must not intersect FOLLOW(A))

**LL(1) Table for the Expression Grammar:**
```
        id      +       *       (       )       $
  E   | E->TE' |        |       |E->TE' |       |
  E'  |        |E'->+TE'|       |       |E'->eps|E'->eps
  T   | T->FT' |        |       |T->FT' |       |
  T'  |        |T'->eps |T'->*FT'|      |T'->eps|T'->eps
  F   | F->id  |        |       |F->(E) |       |
```

**LL(1) Stack-Based Parsing Algorithm:**
```
Initialize: Push $ then S onto stack. Let a = first input token.

While stack not empty:
  X = top of stack
  a = current input token

  If X is terminal:
    If X == a: pop X, advance to next input token
    Else: SYNTAX ERROR

  If X is non-terminal:
    If M[X, a] = X -> Y1 Y2 ... Yn (production exists):
      Pop X
      Push Yn, Yn-1, ..., Y1 (Y1 ends up on top)
    Else: SYNTAX ERROR

If both stack and input are empty ($): ACCEPT
```

**Example Parse Trace for "id + id * id":**
```
Stack          Input            Action
$ E            id+id*id$        M[E,id] = E->TE'
$ E' T         id+id*id$        M[T,id] = T->FT'
$ E' T' F      id+id*id$        M[F,id] = F->id
$ E' T' id     id+id*id$        Match id, advance
$ E' T'        +id*id$          M[T',+] = T'->eps, pop T'
$ E'           +id*id$          M[E',+] = E'->+TE'
$ E' T +       +id*id$          Match +, advance
$ E' T         id*id$           M[T,id] = T->FT'
$ E' T' F      id*id$           M[F,id] = F->id
$ E' T' id     id*id$           Match id, advance
$ E' T'        *id$             M[T',*] = T'->*FT'
$ E' T' F * *  *id$             Match *, advance
$ E' T' F      id$              M[F,id] = F->id
$ E' T' id     id$              Match id, advance
$ E' T'        $                M[T',$] = T'->eps, pop
$ E'           $                M[E',$] = E'->eps, pop
$              $                Stack empty + input empty -> ACCEPT
```

**Recursive Descent Parser** (implements LL parsing naturally):
```c
// One function per non-terminal
void parseE() {
    parseT();
    parseEprime();
}

void parseEprime() {
    if (lookahead == PLUS) {
        match(PLUS);
        parseT();
        parseEprime();
    }
    // else: epsilon production, do nothing (match epsilon)
}

void parseT() {
    parseF();
    parseTprime();
}

void parseTprime() {
    if (lookahead == STAR) {
        match(STAR);
        parseF();
        parseTprime();
    }
}

void parseF() {
    if (lookahead == ID) {
        match(ID);
    } else if (lookahead == LPAREN) {
        match(LPAREN);
        parseE();
        match(RPAREN);
    } else {
        error();
    }
}
```

### MCQ Quick Facts
- LL(1): one lookahead token, leftmost derivation
- LL(1) table conflict (multiple entries) = grammar is NOT LL(1)
- Left recursion and common prefixes make grammar non-LL(1)
- All LL(1) grammars are unambiguous (unambiguous does not imply LL(1))
- LL(k) is more powerful than LL(1) but rarely used in practice
- Recursive descent: easy to implement, directly reflects grammar structure

---

## 13. LR Parsing (SLR, LALR, CLR)

### Written Explanation

LR parsers are bottom-up parsers. They use two operations:
- **Shift:** Push the current input token onto the stack
- **Reduce:** Pop symbols from stack matching a production's RHS, push LHS

**LR(0) Items:**
A dot in a production shows how much has been parsed:
```
A -> alpha . beta  (dot before beta: alpha is on stack, expect beta)
A -> alpha .       (dot at end: this is a REDUCE item)
```

**Closure and Goto Operations:**
```
Closure(I):
  Repeat until stable:
    For each item [A -> alpha . B beta] in I:
      For each production B -> gamma:
        Add [B -> . gamma] to I

Goto(I, X):
  J = {[A -> alpha X . beta] | [A -> alpha . X beta] in I}
  Return Closure(J)
```

**Building the LR(0) Automaton:**
```
Example Grammar (augmented):
  S' -> E
  E  -> E + T
  E  -> T
  T  -> id

State I0 (closure of {S' -> . E}):
  S' -> . E
  E  -> . E + T
  E  -> . T
  T  -> . id

  Goto(I0, E) = I1:   S' -> E .       <- accept item
                      E  -> E . + T
  Goto(I0, T) = I2:   E  -> T .       <- reduce item
  Goto(I0, id) = I3:  T  -> id .      <- reduce item
  Goto(I1, +) = I4:   E  -> E + . T
                      T  -> . id
  Goto(I4, T) = I5:   E  -> E + T .   <- reduce item
  Goto(I4, id) = I3   (same state)
```

**SLR(1) Table Construction:**
```
For item A -> alpha . in state I:
  (reduce item - dot at end)
  For each a in FOLLOW(A):
    ACTION[I, a] = Reduce(A -> alpha)

For item A -> alpha . a beta in state I:
  (shift item - dot before terminal a)
  If Goto(I, a) = J:
    ACTION[I, a] = Shift(J)

For item S' -> S . in state I:
  ACTION[I, $] = Accept

For non-terminal A in state I:
  If Goto(I, A) = J:
    GOTO[I, A] = J
```

**Comparison of LR Parser Variants:**
```
            LR Power and State Count Comparison

  SLR(1)  < LALR(1) < CLR(1)   (power hierarchy)
  
  SLR(1):
    Reduce when lookahead in FOLLOW(A)
    Fewest states (but may have spurious conflicts)
    May miss conflicts that CLR/LALR would correctly identify
    States: same number of items as CLR but uses FOLLOW
  
  LALR(1):
    Merges CLR(1) states that have the same core (LR(0) items)
    Computes accurate per-state lookahead sets
    Same number of states as SLR
    Used by YACC/Bison (industry standard)
    May introduce reduce-reduce conflicts not in CLR(1)
  
  CLR(1) = LR(1):
    Full lookahead per item: [A -> alpha . beta, a]
    Most powerful, fewest conflicts
    Many more states (can be exponential vs LALR)
    Recognize all deterministic CFLs
```

**LR(1) Items:**
```
[A -> alpha . beta, a]   where a is the lookahead terminal

The lookahead a is used ONLY when this is a reduce item:
[A -> alpha . , a]  -> reduce A->alpha only when input is 'a'

This is more precise than FOLLOW sets used in SLR.
```

**LR Parsing Algorithm:**
```
Stack: s0 (state stack)
Input: a1 a2 ... an $

Loop:
  s = top of stack state
  a = current input symbol

  Case ACTION[s, a]:
    = Shift s':
        Push current symbol a and new state s' onto stack
        Advance input to next symbol

    = Reduce A -> beta:
        Pop 2*|beta| items from stack (symbol + state for each)
        s' = new top state after popping
        Push A onto symbol stack
        Push GOTO[s', A] onto state stack

    = Accept:
        Input parsed successfully. Done!

    = Error:
        Call error recovery routine.
```

**Conflicts:**
```
Shift-Reduce Conflict:
  In the same state, for the same lookahead:
    One item says SHIFT
    Another item says REDUCE
  
  Classic: Dangling else
    stmt -> if expr then stmt
    stmt -> if expr then stmt else stmt
  
  Resolution: typically prefer SHIFT (else attaches to nearest if)

Reduce-Reduce Conflict:
  In the same state, for the same lookahead:
    Two different items say REDUCE (by different productions)
  
  This usually indicates a grammar design problem.
  More serious than shift-reduce conflicts.
```

### MCQ Quick Facts
- LR(0) < SLR(1) < LALR(1) < LR(1) in power
- YACC/Bison generates LALR(1) parsers
- Shift-reduce conflict: often from ambiguous grammar or dangling else
- Reduce-reduce conflict: usually grammar design problem
- LR parsers can parse all deterministic CFLs
- LL(1) is strictly less powerful than LALR(1) and LR(1)
- LR(1) items: [A->alpha.beta, a] — LR(0) item + lookahead terminal
- All programming languages can be described by LALR(1) grammars

---

## 14. Semantic Analysis

### Written Explanation

Semantic analysis checks the **meaning** of syntactically correct programs.

**Main Tasks:**
1. **Type checking** — operand type compatibility
2. **Scope resolution** — variable declaration and use
3. **Binding** — matching identifiers to their declarations
4. **Flow checking** — control flow validity (e.g., break only inside loop)

**Attribute Grammars:**
Grammar rules augmented with **attributes** (additional information) and **semantic rules** (equations computing attributes).

**Synthesized Attributes:** Computed from children (bottom-up evaluation)
```
E -> E1 + T   { E.type = type_check('+', E1.type, T.type) }
E -> T         { E.type = T.type }
T -> id        { T.type = lookup(id.name).type }
T -> num       { T.type = int }
```

**Inherited Attributes:** Passed from parent or left siblings (top-down / left-to-right)
```
D -> T L       { L.type = T.type }
L -> L1, id   { L1.type = L.type; add_type(id.name, L.type) }
L -> id        { add_type(id.name, L.type) }
T -> int       { T.type = int }
T -> float     { T.type = float }
```

**S-Attributed Grammar:**
- Only synthesized attributes
- Can be evaluated in a single bottom-up pass
- Compatible with LR/shift-reduce parsing (embedded actions)

**L-Attributed Grammar:**
- Synthesized attributes + inherited attributes computed left-to-right
- Compatible with LL parsing (top-down, depth-first, left-to-right)
- Every S-attributed grammar is also L-attributed

```
S-Attributed subset-of L-Attributed subset-of All Attribute Grammars
```

**Dependency Graphs:**
Nodes = attribute instances; edges = dependencies (must compute A before B if A->B).
Cyclic dependency graph = grammar cannot be evaluated in a single pass.

**Type Checking:**
```
Type expressions:
  Primitive types: int, float, char, bool, void
  Constructed types:
    array(size, type)        e.g., array(10, int)
    pointer(type)            e.g., pointer(int)  [i.e., int*]
    struct(field_types)
    function(param_types, return_type)

Type equivalence:
  Structural equivalence: two types are equal if they have same structure
  Name equivalence: two types are equal only if they have same name (even if same structure)

Type coercion (implicit):
  int -> float automatically in most languages
  
Type casting (explicit):
  (float) x  in C  [programmer-requested conversion]
```

**Scope Rules:**
```
Static (Lexical) Scope:
  A variable refers to the nearest enclosing declaration in source code
  Determined at compile time
  Used by: C, Java, Python, most modern languages

  int x = 1;
  void f() { print(x); }   // Prints 1 (global x)
  void g() {
    int x = 2;
    f();                   // Still prints 1 (lexical scope)
  }

Dynamic Scope:
  A variable refers to the most recently called procedure's binding
  Determined at runtime
  Used by: early Lisp, some scripting languages

  int x = 1;
  void f() { print(x); }
  void g() {
    int x = 2;
    f();                   // Prints 2 under dynamic scope
  }
```

**Symbol Table for Nested Scopes:**
```
  Global Scope Table
       |
       v
  Function Scope Table  <-- inserted on function entry
       |
       v
  Block Scope Table     <-- inserted on block entry

  Each table has pointer to parent table (scope chain)
  Lookup: search from innermost to outermost scope
  Delete: pop entire scope table on block/function exit
```

### MCQ Quick Facts
- Semantic analysis: type checking, scope resolution, binding
- Synthesized attrs: from children (bottom-up); Inherited: from parent/siblings
- S-Attributed: only synthesized, works with LR parsers
- L-Attributed: synthesized + left-to-right inherited, works with LL parsers
- Symbol table: stores identifier names and their attributes (type, scope, location)
- Static scope: most modern languages; Dynamic scope: older Lisp
- Semantic errors: type mismatch, undeclared variable, wrong argument count

---

## 15. Intermediate Code Generation

### Written Explanation

Intermediate code is a machine-independent representation between the source AST and target machine code.

**Why Intermediate Code?**
```
  Source  -> Front-end -> IR -> Back-end -> Target

  With N source languages and M targets:
  Without IR: N x M compilers needed
  With IR:    N front-ends + M back-ends = N + M (much fewer)
  
  Also enables: machine-independent optimization
```

**Three-Address Code (TAC / 3AC):**
At most **one operator** and at most **three addresses** per instruction.

```
Form                Example
x = y op z         t1 = b * c
x = op y           t2 = -a
x = y              x = t2
goto L             goto L5
if x goto L        if t1 goto L3
if x relop y goto L  if a < b goto L1
param x            param x1
call p, n          call printf, 2
return y           return val
x = y[i]           t3 = a[i]
x[i] = y           a[i] = t3
x = &y             x = &a
x = *y             x = *ptr
*x = y             *ptr = val
```

**Representations of TAC:**

**1. Quadruples — (op, arg1, arg2, result):**
```
Source: a = b + c * d - e

Quadruples:
  #    op    arg1   arg2   result
  1    *      c      d      t1
  2    +      b      t1     t2
  3    -      t2     e      t3
  4    =      t3     -      a
```

**2. Triples — (op, arg1, arg2):** result is implicit (the triple's position number)
```
  #    op    arg1   arg2
  0    *      c      d       result referenced as (0)
  1    +      b     (0)      result referenced as (1)
  2    -     (1)     e       result referenced as (2)
  3    =      a     (2)      assigns to a
```

**3. Indirect Triples:** An array of pointers to triples (allows reordering for optimization without renumbering)

**Comparison:**
| Feature | Quadruples | Triples | Indirect Triples |
|---------|-----------|---------|-----------------|
| Explicit result | YES | NO | NO |
| Easy to reorder | YES | NO | YES |
| Space | More | Less | Medium |
| Used for | Code optimization | — | Optimization with reordering |

**DAG (Directed Acyclic Graph) Representation:**
DAGs identify common subexpressions at the IR level.
```
Expression: a + a * (b - c) + (b - c) * d

Parse Tree (redundant computation):
        +
       / \
      +   *
     / \ / \
    a  * (b-c)  d
      / \
     a  (b-c)

DAG (shared nodes, no redundancy):
        +
       / \
      +   *
     / \ / \
    a  * #  d     where # is the (b-c) node (shared!)
      /|\
     a  -
       / \
      b   c

DAG eliminates duplicate computation of (b-c)
```

**Backpatching:**
When generating code in one pass, jump targets may not be known yet.
```
Technique:
  1. Generate jump instruction with placeholder target: goto _
  2. Maintain a list of incomplete jumps (truelist, falselist)
  3. When target label is determined, PATCH all jumps in the list

Example for: if (a < b) S1 else S2

  t1 = a < b
  if t1 goto _   [add to truelist]
  goto _         [add to falselist]
  ... (S1 code)
  goto _         [add to nextlist]
  L1: ... (S2 code)   [patch falselist to L1]
  L2:                 [patch truelist to where S1 starts, nextlist to L2]
```

**Translation of Control Structures:**
```
while (E) S:
  L1: <code for E>
      if E.false goto L2
      <code for S>
      goto L1
  L2: ...

for (init; cond; update) S:
  <init>
  L1: <code for cond>
      if cond.false goto L2
      <code for S>
      <update>
      goto L1
  L2: ...
```

### MCQ Quick Facts
- TAC: at most one operator per instruction
- Quadruples: (op, arg1, arg2, result) — 4 fields, easy to reorder
- Triples: (op, arg1, arg2) — result = position number, hard to reorder
- Temporaries: compiler-generated variables for intermediate results
- DAG: identifies common subexpressions at expression level
- Backpatching: fix jump targets in a single-pass code generator
- SSA (Static Single Assignment): each variable assigned exactly once (modern IR)

---

## 16. Code Optimization

### Written Explanation

**Goal:** Transform code to improve execution speed, memory usage, or power consumption while preserving semantics (correctness).

**Classification:**
```
  By Scope:
    Local:     Within a basic block
    Global:    Across basic blocks within a function
    Interprocedural: Across function boundaries

  By Machine Dependence:
    Machine-Independent (on IR):
      Constant folding, propagation, dead code elimination, CSE, LICM, inlining
    Machine-Dependent (on target code):
      Register allocation, instruction scheduling, peephole
```

**Basic Block:**
A maximal sequence of consecutive statements with:
- Entry only at the first statement
- Exit only at (or after) the last statement

```
Identify basic blocks:
  1. First statement is a leader
  2. Target of a jump is a leader
  3. Statement after a jump is a leader
  4. A basic block = from one leader to just before the next leader
```

**Control Flow Graph (CFG):**
```
int gcd(int a, int b) {
    while (a != b) {
        if (a > b)
            a = a - b;
        else
            b = b - a;
    }
    return a;
}

CFG:
  [B1: entry]
       |
       v
  [B2: if a != b] --NO--> [B5: return a]
       |YES
       v
  [B3: if a > b] --NO--> [B4: b = b - a]
       |YES                    |
       v                       |
  [B3b: a = a - b]             |
       |                       |
       +------> [back to B2] <-+
```

**Key Optimizations:**

**1. Constant Folding:**
Evaluate constant expressions at compile time.
```
x = 2 * 3 + 1  -->  x = 7
if (true && false)  -->  if (false)  -->  dead code
```

**2. Constant Propagation:**
Replace variable with its known constant value.
```
x = 5
y = x + 3   -->  y = 5 + 3  -->  y = 8  (with folding)
```

**3. Copy Propagation:**
After a copy x = y, replace uses of x with y.
```
x = y
z = x + 1  -->  z = y + 1
```

**4. Dead Code Elimination:**
Remove code whose results are never used.
```
x = 5       // dead: x immediately overwritten
x = 10
y = x + 1   // uses x = 10 only

Also: unreachable code after unconditional goto
```

**5. Common Subexpression Elimination (CSE):**
Compute a repeated expression only once.
```
a = b + c
d = b + c   -->   t = b + c; a = t; d = t
```

**6. Loop-Invariant Code Motion (LICM):**
Move code that doesn't change inside the loop to outside.
```
while (i < n) {
    x = y + z;       // y, z not modified in loop
    a[i] = x * i;
    i++;
}

Optimized:
x = y + z;           // Hoisted out of loop
while (i < n) {
    a[i] = x * i;
    i++;
}
```

**7. Strength Reduction:**
Replace costly operations with cheaper equivalents.
```
x = y * 2    -->  x = y + y  or  x = y << 1
x = y ** 2   -->  x = y * y
t = 4 * i    -->  Use induction variable t = t + 4 (in loop)
```

**8. Induction Variable Elimination:**
In a loop, replace multiplication by index with addition.
```
for i = 0 to n-1:
    a[4*i] = ...   // 4*i recomputed each iteration

With induction variable j = 4*i:
  j = 0
  for i = 0 to n-1:
    a[j] = ...
    j = j + 4     // cheaper: add instead of multiply
```

**9. Loop Unrolling:**
Replicate loop body to reduce branch overhead.
```
for (i = 0; i < 8; i++) {
    a[i] = b[i];
}
Unrolled by 4:
for (i = 0; i < 8; i += 4) {
    a[i]   = b[i];
    a[i+1] = b[i+1];
    a[i+2] = b[i+2];
    a[i+3] = b[i+3];
}
```

**10. Peephole Optimization:**
Examine a small window of instructions and replace with better sequence.
```
Redundant load/store:
  STORE R0, x
  LOAD  R0, x    -->  (delete: x hasn't changed)

Algebraic identity:
  ADD R0, 0      -->  (delete: no effect)
  MUL R0, 1      -->  (delete: no effect)

Strength reduction at machine level:
  MUL R0, 2      -->  SHL R0, 1   (shift left = multiply by 2)

Jump to next instruction:
  JMP L1
  L1: ...        -->  (remove JMP, fall through)

Jump chains:
  JMP L1
  L1: JMP L2     -->  JMP L2  (merge)
```

**Data Flow Analysis:**
```
Framework: For each basic block B, compute GEN[B], KILL[B]:
  GEN[B]:  values generated/available at end of B
  KILL[B]: values killed/invalidated in B

Forward Analysis (information flows forward along CFG edges):
  IN[B]  = union/intersection of OUT[predecessors of B]
  OUT[B] = GEN[B] union (IN[B] - KILL[B])
  
  Examples: Reaching Definitions, Available Expressions, Constant Propagation

Backward Analysis (information flows backward):
  OUT[B] = union/intersection of IN[successors of B]
  IN[B]  = USE[B] union (OUT[B] - DEF[B])
  
  Examples: Live Variable Analysis, Busy Expressions

Analysis         | Direction | Meet Op  | Used For
-----------------|-----------|----------|---------------------
Reaching Defs    | Forward   | Union    | CSE, copy prop
Available Exprs  | Forward   | Intersect| CSE (more precise)
Live Variables   | Backward  | Union    | Dead code, reg alloc
Busy Expressions | Backward  | Intersect| Code hoisting
```

### MCQ Quick Facts
- Basic block: no internal branches or branch targets
- CFG nodes = basic blocks; CFG edges = control flow
- Reaching definitions: forward analysis (union)
- Live variables: backward analysis (union)
- Available expressions: forward analysis (intersection)
- Loop optimization: most impactful (programs spend most time in loops)
- LICM: move loop-invariant code out of loop (requires dominator analysis)
- Peephole: local machine-level optimization on small instruction windows

---

## 17. Code Generation

### Written Explanation

**Goal:** Translate optimized IR to correct, efficient target machine code.

**Three key sub-problems:**
1. **Instruction Selection:** Choose the right machine instructions for each IR operation
2. **Register Allocation:** Assign variables and temporaries to machine registers
3. **Instruction Scheduling:** Order instructions to avoid pipeline stalls

**Register Allocation — Graph Coloring:**
```
Step 1: Build the Interference Graph
  - Nodes: variables and temporaries that are live simultaneously
  - Edge between u and v if they are live at the same point
    (i.e., both need to be in registers at the same time)

Step 2: Color the graph
  - Colors = physical registers (say, k registers available)
  - Adjacent nodes must have DIFFERENT colors
  - k-colorable = all variables can be assigned a register
  - Not k-colorable = some variables must be SPILLED to memory

Step 3: Spilling
  - Choose a variable to spill (typically one with few uses or large live range)
  - Insert LOAD before each use, STORE after each definition
  - Remove spilled variable from graph, retry coloring

Example with k=2 registers:
  Variables: a, b, c, d
  Live simultaneously: {a,b} at point P1, {b,c} at P2, {c,d} at P3

  Interference edges: a-b, b-c, c-d
  Graph: a -- b -- c -- d

  2-coloring:
    a = R1, b = R2, c = R1, d = R2   -> valid! (no adjacent same color)

  If also a-c interference:
    a -- b -- c -- d -- a (cycle of 4, still 2-colorable)
  If also b-d interference:
    K4 minus one edge -> need to check...
```

**Sethi-Ullman Algorithm (Minimize Registers for Expression Trees):**
```
Label each node with minimum registers needed:

For leaf node: label = 1

For internal node with children c1 (label n1), c2 (label n2):
  If n1 == n2: label = n1 + 1
  If n1 != n2: label = max(n1, n2)

Evaluation order: always compute the subtree needing MORE registers first
(This minimizes total register usage)
```

**Activation Records (Stack Frame):**
```
High address
  +-----------------------------+
  | ...caller's frame...        |
  |-----------------------------|  <- old fp (frame pointer)
  | actual parameters           |
  | return address              |
  | saved old frame pointer     |
  | local variables             |
  | temporaries                 |
  | saved registers             |
  +-----------------------------+  <- sp (stack pointer)
Low address

Calling sequence:
  Caller: push params, call instruction (saves return address)
  Callee: save old fp, set fp=sp, allocate locals (sp = sp - size)
  Return: restore sp=fp, restore old fp, jump to return address
```

**Simple Code Generation (for basic blocks):**
```
Keep track of:
  - Register descriptor: which variable each register holds
  - Address descriptor: which registers/memory locations hold each variable

For each TAC instruction x = y op z:
  1. Get registers for y and z (load from memory if needed)
  2. Compute operation into a result register Rx
  3. Update register and address descriptors
  4. If y or z no longer needed, their registers become free

At end of basic block: store all live variables back to memory
```

### MCQ Quick Facts
- Register allocation is NP-Complete in general (reduces to k-graph coloring)
- Graph coloring: adjacent nodes (interfering variables) get different colors (registers)
- Spilling: variable stored in memory when no register available
- Sethi-Ullman: minimize registers for expression evaluation
- Instruction selection: pattern matching on IR tree
- Activation record / stack frame: bookkeeping for function calls
- Calling convention: who saves what (caller-save vs callee-save registers)

---

## 18. Symbol Table & Error Recovery

### Written Explanation

**Symbol Table:**
A data structure that maps identifier names to their attributes throughout compilation.

**Information stored:**
```
Identifier:  count
  - Kind:          variable
  - Type:          int
  - Scope:         local (function main)
  - Memory location: SP + 8 (stack offset)
  - Initialized:   yes

Identifier:  printf
  - Kind:          function
  - Return type:   int
  - Parameters:    (const char*, ...)
  - Scope:         global
  - Location:      external (linked)
```

**Implementation:**
```
1. Linear List: O(n) lookup — suitable only for tiny tables

2. BST (Binary Search Tree): O(log n) average

3. Hash Table: O(1) average — MOST COMMON IN PRACTICE
   - Hash function: h(name) = (sum of char codes) mod table_size
   - Collision handling: chaining or open addressing

4. Chained Symbol Tables for nested scopes:
   
   Scope Level 0 (Global):
   +--------+--------+
   | x: int |printf:func|...
   +--------+--------+
        |
        v
   Scope Level 1 (Function foo):
   +--------+--------+
   | a: float | n: int |...
   +--------+--------+
        |
        v
   Scope Level 2 (Inner block):
   +--------+
   | i: int  |...
   +--------+
   
   Lookup: current scope -> parent -> grandparent -> ... -> global
   Insert: into current scope table only
   Delete: entire scope table popped when block exits
```

**Error Recovery Strategies:**

**1. Panic Mode Recovery (most widely used):**
```
On syntax error:
  Discard input tokens one by one until a SYNCHRONIZING TOKEN is found
  (e.g., semicolon ';', closing brace '}', keyword 'end')
  Resume parsing from that point

Advantage: Simple to implement
Disadvantage: May skip large portions of input, missing further errors
```

**2. Phrase-Level Recovery:**
```
Perform a LOCAL CORRECTION at the error point:
  - Insert a missing token (e.g., add semicolon if missing)
  - Delete an extra token (e.g., remove spurious comma)
  - Replace a wrong token (e.g., = for ==)

Parser modifies the remaining input/stack and continues

Advantage: Can recover better and find more errors
Disadvantage: Wrong corrections can cascade into more errors
```

**3. Error Productions:**
```
Add special grammar rules for COMMON MISTAKES:
  stmt -> error ';'        (handles garbled statements before semicolon)
  
Parser recognizes the error as a valid (error) production
and continues without stopping

Advantage: Handles known common errors gracefully
Disadvantage: Grammar becomes more complex
```

**4. Global Correction:**
```
Find minimum edit distance (insertions, deletions, replacements)
to transform erroneous input into a valid string

Advantage: Theoretically optimal
Disadvantage: O(n^3) or worse — too expensive for practical compilers
```

**Classification of Errors by Phase:**
```
Phase         | Error Type     | Example
--------------|----------------|------------------------------------------
Lexical       | Lexical Error  | $x = 5  ($ not valid in identifier)
Syntax        | Syntax Error   | int a =;  (missing expression)
Semantic      | Semantic Error | int a = "hello";  (type mismatch)
              |                | use of undeclared variable
              |                | wrong number of function arguments
Logical       | Logic Error    | while(1){} without break (programmer error)
              |                | off-by-one: for(i=0; i<=n; i++)
```

### MCQ Quick Facts
- Symbol table: maps identifier names to attributes
- Hash table: O(1) average — most common implementation for symbol tables
- Nested scopes: chained symbol tables or stack-based approach
- Panic mode: most widely used error recovery; discard until sync token
- Phrase-level: local correction (insert/delete/replace token)
- Error productions: grammar rules for known common errors
- Compiler reports errors; it does NOT silently fix them
- Goal of error recovery: continue parsing to find more errors in one pass

---

# COMPREHENSIVE MCQ QUICK REFERENCE

## Theory of Computation — MCQ Cheatsheet

| Topic | Key Fact |
|-------|----------|
| DFA | delta: QxSigma->Q, exactly one transition per (state, symbol) |
| NFA | delta: Qx(Sigma union eps)->2^Q, multiple/zero transitions |
| DFA = NFA | Same power; NFA->DFA may need 2^n states |
| eps-closure | States reachable via epsilon-moves only |
| Pumping Lemma (RL) | w=xyz, |xy|<=p, |y|>=1, xy^i z in L |
| Pumping Lemma (CFL) | w=uvxyz, |vxy|<=p, |vy|>=1, uv^i xy^i z in L |
| CNF | A->BC or A->a |
| GNF | A->a alpha (a is terminal) |
| CYK | Membership for CFL, O(n^3 |G|), grammar must be CNF |
| PDA = CFG | Same expressive power (both describe CFLs) |
| TM | Most powerful; recognizes RE languages |
| Decidable | TM always halts; accepts members, rejects non-members |
| RE not decidable | A_TM, HALT_TM |
| Not RE | E_TM complement, EQ_TM, complement of A_TM |
| Rice's Theorem | All non-trivial semantic properties of TMs undecidable |
| P | Polynomial time (deterministic) |
| NP | Polynomial-time verifiable |
| NP-Complete | In NP AND NP-Hard |
| NP-Hard | Every NP problem reduces to it |
| 2-SAT | In P (linear time) |
| 3-SAT | NP-Complete |
| 2-Colorability | In P (bipartite check, BFS/DFS) |
| 3-Colorability | NP-Complete |
| Euler circuit | In P (Eulerian condition: all even-degree vertices) |
| Hamiltonian circuit | NP-Complete |
| TSP | NP-Hard (optimization), NP-Complete (decision) |

## Compiler Design — MCQ Cheatsheet

| Topic | Key Fact |
|-------|----------|
| Token | Category of lexical unit |
| Lexeme | Actual character sequence matched by token pattern |
| Pattern | Regex describing what strings constitute a token |
| Lexical error | Unrecognized character/token (e.g., $ in C) |
| Syntax error | Grammatically incorrect token sequence |
| Semantic error | Type mismatch, undeclared variable |
| FIRST(A) | Terminals beginning strings derivable from A |
| FOLLOW(A) | Terminals that can appear immediately after A |
| LL(1) | Non-recursive top-down; 1 lookahead |
| LL(1) conflict | Multiple entries in parsing table cell |
| Left recursion | Cannot handle in LL; must eliminate first |
| Power: SLR<LALR<LR(1) | LR parser hierarchy |
| LALR(1) | Used by YACC/Bison |
| LR(0) Item | Production with dot (e.g., A -> alpha . beta) |
| LR(1) Item | LR(0) item + lookahead: [A->alpha.beta, a] |
| Shift-reduce conflict | Ambiguity or dangling else |
| Reduce-reduce conflict | Grammar design problem |
| Quadruple | (op, arg1, arg2, result) — 4 fields |
| Triple | (op, arg1, arg2) — result = position number |
| TAC | At most 3 addresses, 1 operator per instruction |
| Basic block | Max sequence with no internal branches/targets |
| Reaching defs | Forward data flow analysis (union) |
| Live variables | Backward data flow analysis (union) |
| Available exprs | Forward data flow analysis (intersection) |
| CSE | Common Subexpression Elimination |
| LICM | Loop Invariant Code Motion (hoist out of loop) |
| Peephole | Local machine-level optimization |
| Register alloc | Graph coloring (NP-Complete in general) |
| Spilling | Variable sent to memory when no register available |
| Sethi-Ullman | Minimize registers needed for expression tree |
| Symbol table | Hash table O(1); chained for nested scopes |
| Panic mode | Discard tokens until synchronization token |

---

# IMPORTANT FORMULAS & PROPERTIES

## Regular Languages
```
RE Identities:
  (R*)* = R*
  (R+S)* = (R*S*)* = (R*S)*R* = R*(SR*)*
  R? = R|eps
  R+ = RR*
  eps* = eps
  empty* = eps

DFA minimum states = |Myhill-Nerode equivalence classes|
NFA->DFA: at most 2^n states (worst case)
```

## CFL Properties
```
Closure:
  CFL union CFL = CFL           YES
  CFL concat CFL = CFL          YES
  CFL* = CFL                    YES
  complement(CFL) = CFL?        NO (not closed)
  CFL1 intersect CFL2 = CFL?    NO (not closed)
  CFL intersect RL = CFL        YES (closed!)

CYK Table:
  X[i,i] = {A | A -> a_i in P}
  X[i,j] = union over k: {A | A->BC, B in X[i,k], C in X[k+1,j]}
  S in X[1,n] => string accepted
```

## Complexity
```
P subset-of NP subset-of PSPACE subset-of EXPTIME
NL subset-of P
Savitch's Theorem: NSPACE(f) subset-of DSPACE(f^2)
Cook-Levin: SAT is NP-Complete (first NP-Complete problem)

Reduction chain:
SAT <=_p 3-SAT <=_p 3-COLOR
3-SAT <=_p CLIQUE <=_p INDEPENDENT SET <=_p VERTEX COVER
3-SAT <=_p HAMILTONIAN CIRCUIT <=_p TSP
SAT <=_p SUBSET SUM
```

---

# COMMON EXAM TRICKS & TRAPS

1. **"epsilon in FIRST(A)"** only if A can derive epsilon (A is nullable)
2. **FOLLOW never contains epsilon** — only terminals and $
3. **NFA -> DFA**: A dead/trap state is needed for a COMPLETE (total) DFA
4. **Ambiguous grammar != Ambiguous language** (a language may have unambiguous grammar even if one grammar for it is ambiguous)
5. **All DFAs are also NFAs** (DFA is a special case of NFA)
6. **RE but undecidable**: A_TM — don't confuse "recognizable/RE" with "decidable"
7. **P vs NP**: P=NP is an OPEN PROBLEM; we assume P!=NP for security
8. **NP-Hard doesn't imply being in NP**: Halting problem is NP-Hard but not in NP
9. **2-SAT is in P**: Only 3-SAT and higher are NP-Complete
10. **Euler path (all edges) is P; Hamiltonian path (all vertices) is NP-Complete**
11. **CFL intersect Regular Language = CFL** (CFLs are closed under intersection with RL)
12. **Left recursion removal**: preserves the language, only changes the grammar
13. **LR parsers handle strictly more grammars than LL parsers**
14. **LALR has the same number of states as SLR** (both use LR(0) cores; differ in lookahead computation)
15. **Quadruples better than triples for code movement** (instructions can be reordered without renumbering)
16. **Pumping lemma can only DISPROVE regularity/context-freeness**, not prove it
17. **The minimum number of states in a DFA = number of Myhill-Nerode classes** (this is exactly the minimization result)
18. **S-attributed grammars are a SUBSET of L-attributed grammars**

---

*End of Notes*
*Compiler Design & Theory of Computation*
*BUET MSc Admission Preparation*

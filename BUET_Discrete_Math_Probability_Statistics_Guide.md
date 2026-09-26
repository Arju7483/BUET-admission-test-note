# Comprehensive Reference Handbook: Discrete & Concrete Mathematics, Probability & Statistics
### Master Preparation Guide for BUET Written & MCQ Examinations (MSc, BSc & Recruitment)

---

## 🎯 Examination Architecture & Strategic Roadmap

In the **BUET Admission & Recruitment Examinations** (MSc in CSE/ICT, BSc Admission, and IT Specialist Job Exams), **Discrete Mathematics, Concrete Mathematics, Probability & Statistics** account for a substantial portion of both the screening and written evaluations.

```
+----------------------------------------------------------------------------------------------------+
|                                    BUET EXAM ARCHITECTURE                                          |
+------------------------------------+---------------------------------------------------------------+
| Stage                              | Structure & Evaluation Strategy                               |
+------------------------------------+---------------------------------------------------------------+
| Phase 1: Screening MCQ             | 50 MCQs | 50 Marks | 50 Minutes | -0.5 Mark Negative Marking |
|                                    | Focus: Rapid formula application, parity checks, graph bounds |
+------------------------------------+---------------------------------------------------------------+
| Phase 2: Written Examination       | 10 Broad Questions (Sub-divided) | 100 Marks | 60 Minutes     |
|                                    | Focus: Full mathematical proofs, recurrence derivations,      |
|                                    | Bayes' theorem multi-branch trees, linearity of expectation  |
+------------------------------------+---------------------------------------------------------------+
```

---

# PART 1: DISCRETE & CONCRETE MATHEMATICS

---

## 1. Set Theory, Logic & Boolean Algebra

### 1.1 Set Operations & Algebraic Laws
Let $U$ denote the universal set and $A, B, C \subseteq U$.

1. **Power Set ($\mathcal{P}(A)$):**
   * The set of all subsets of $A$: $\mathcal{P}(A) = \{S \mid S \subseteq A\}$.
   * If $|A| = n$, then $|\mathcal{P}(A)| = 2^n$.
   * Total number of non-empty subsets = $2^n - 1$. Total number of proper subsets = $2^n - 1$.
2. **Cartesian Product ($A \times B$):**
   * $A \times B = \{(a, b) \mid a \in A \land b \in B\}$.
   * $|A \times B| = |A| \cdot |B|$.
   * Non-commutative: $A \times B \ne B \times A$ (unless $A = B$ or one is $\emptyset$).
3. **Symmetric Difference ($A \Delta B$):**
   * $A \Delta B = (A \setminus B) \cup (B \setminus A) = (A \cup B) \setminus (A \cap B)$.
   * Properties: Commutative ($A \Delta B = B \Delta A$), Associative ($A \Delta (B \Delta C) = (A \Delta B) \Delta C$), Identity ($A \Delta \emptyset = A$), Self-Inverse ($A \Delta A = \emptyset$).
4. **De Morgan’s Laws (Set & Boolean Form):**
   $$\overline{A \cup B} = \overline{A} \cap \overline{B}, \qquad \overline{A \cap B} = \overline{A} \cup \overline{B}$$

---

### 1.2 Principle of Inclusion-Exclusion (PIE)

```
             [Two-Set PIE]                             [Three-Set PIE]
           +-------+   +-------+                    +-------+   +-------+
          /         \ /         \                  /    A    \ /    B    \
         |     A     |     B     |                |     |     |     |     |
         |        +--+--+        |                |     +--+--+--+--+     |
          \      / A ∩ B \      /                  \      /  \ /  \      /
           +----+---------+----+                    +----+----+----+----+
                                                         \    C    /
                                                          +-------+
```

#### Analytical Derivation
Let $A_1, A_2, \dots, A_n$ be finite sets. To count elements in $\bigcup_{i=1}^n A_i$ exactly once:
$$\left|\bigcup_{i=1}^n A_i\right| = \sum_{i=1}^n |A_i| - \sum_{1 \le i < j \le n} |A_i \cap A_j| + \sum_{1 \le i < j < k \le n} |A_i \cap A_j \cap A_k| - \dots + (-1)^{n-1} |A_1 \cap \dots \cap A_n|$$
* **Proof Intuition:** An element belonging to exactly $m$ of the sets ($1 \le m \le n$) is counted:
  $$\binom{m}{1} - \binom{m}{2} + \binom{m}{3} - \dots + (-1)^{m-1} \binom{m}{m} = 1 - (1 - 1)^m = 1 \text{ time.}$$

---

### 1.3 Derangements ($!n$ or $D_n$)
A **derangement** is a permutation of $n$ elements such that no element appears in its original position (i.e., $\pi(i) \ne i$ for all $i \in \{1, 2, \dots, n\}$).

#### Derivation via PIE
Let $S$ be all $n!$ permutations. Let $P_i$ be the property that element $i$ is in its original position ($\pi(i) = i$).
* $|S| = n!$
* For any single element $i$: $|A_i| = (n-1)! \implies \sum |A_i| = \binom{n}{1}(n-1)! = n!$
* For any pair $i, j$: $|A_i \cap A_j| = (n-2)! \implies \sum |A_i \cap A_j| = \binom{n}{2}(n-2)! = \frac{n!}{2!}$
* By PIE, the count of valid derangements $D_n$ is:
  $$D_n = n! - \left[ \binom{n}{1}(n-1)! - \binom{n}{2}(n-2)! + \dots + (-1)^{n-1}\binom{n}{n}0! \right]$$
  $$D_n = n! \left[ 1 - \frac{1}{1!} + \frac{1}{2!} - \frac{1}{3!} + \dots + \frac{(-1)^n}{n!} \right] = n! \sum_{k=0}^n \frac{(-1)^k}{k!}$$

#### Fundamental Recurrence Relations
1. **Two-term Recurrence:**
   $$D_n = (n-1)(D_{n-1} + D_{n-2}) \quad \text{for } n \ge 2 \quad (\text{with } D_0 = 1, D_1 = 0)$$
   * *Combinatorial Insight:* Consider element 1 placed in position $k$ ($n-1$ choices). Either element $k$ goes to position 1 ($D_{n-2}$ ways for remaining $n-2$ elements) or element $k$ does not go to position 1 ($D_{n-1}$ ways).
2. **First-order Inhomogeneous Recurrence:**
   $$D_n = n D_{n-1} + (-1)^n$$
3. **Asymptotic Behavior & Rounding Trick:**
   $$\lim_{n \to \infty} \frac{D_n}{n!} = \sum_{k=0}^\infty \frac{(-1)^k}{k!} = e^{-1} \approx 0.367879$$
   $$D_n = \left\lfloor \frac{n!}{e} + \frac{1}{2} \right\rfloor \quad \text{for all } n \ge 1$$

---

### 1.4 Propositional & First-Order Logic

#### Logical Connectives & Truth Table Rules
* **Conditional (Implication $p \to q$):** Equivalent to $\neg p \lor q$. False **only** when $p = \text{True}$ and $q = \text{False}$.
* **Biconditional ($p \leftrightarrow q$):** True when $p$ and $q$ have identical truth values: $(p \land q) \lor (\neg p \land \neg q)$.
* **Contrapositive:** $\neg q \to \neg p \equiv p \to q$ (**Always equivalent**; basis for indirect proofs).
* **Converse ($q \to p$) & Inverse ($\neg p \to \neg q$):** Mutually equivalent to each other, but **NOT** equivalent to $p \to q$.

#### Quantifiers & Negation Dynamics
$$\neg [\forall x \, P(x)] \equiv \exists x \, \neg P(x)$$
$$\neg [\exists x \, P(x)] \equiv \forall x \, \neg P(x)$$
$$\neg [\forall x \, \exists y \, Q(x, y)] \equiv \exists x \, \forall y \, \neg Q(x, y)$$

---

## 2. Relations, Functions, Posets & Pigeonhole Principle

### 2.1 Properties of Binary Relations on Set $A$ ($|A| = n$)
A relation $R \subseteq A \times A$ can be represented as an $n \times n$ Boolean matrix $M_R$. Total possible relations = $2^{n^2}$.

```
             Relation Matrix M_R (n x n):
             Diagonal Entries: (1,1), (2,2), ..., (n,n) -> n entries
             Off-Diagonal Pairs: (i,j) and (j,i)         -> n(n-1)/2 pairs
```

```
+----------------------------------------------------------------------------------------------------+
|                                  BINARY RELATION CLASSIFICATION                                    |
+--------------------+---------------------------------------------+---------------------------------+
| Property           | Mathematical Definition                     | Formula for Count on Set Size n |
+--------------------+---------------------------------------------+---------------------------------+
| Reflexive          | $\forall x \in A, (x, x) \in R$             | $2^{n^2 - n} = 2^{n(n-1)}$      |
| Irreflexive        | $\forall x \in A, (x, x) \notin R$          | $2^{n^2 - n} = 2^{n(n-1)}$      |
| Symmetric          | $(x, y) \in R \implies (y, x) \in R$        | $2^{n(n+1)/2}$                  |
| Antisymmetric      | $(x, y) \in R \land (y, x) \in R \implies x=y$ | $2^n \cdot 3^{n(n-1)/2}$        |
| Asymmetric         | $(x, y) \in R \implies (y, x) \notin R$     | $3^{n(n-1)/2}$                  |
| Reflexive + Sym.   | Compatible Relation                         | $2^{n(n-1)/2}$                  |
+--------------------+---------------------------------------------+---------------------------------+
```

#### Derivation of Antisymmetric Count:
* For each of the $n$ diagonal elements $(x, x)$, there are $2$ choices (present or absent) $\implies 2^n$.
* For each of the $\frac{n(n-1)}{2}$ symmetric pairs $\{(x, y), (y, x)\}$ ($x \ne y$), we can have: (i) neither present, (ii) only $(x,y)$ present, or (iii) only $(y,x)$ present ($3$ choices) $\implies 3^{n(n-1)/2}$.
* Total $= 2^n \cdot 3^{n(n-1)/2}$.

---

### 2.2 Equivalence Relations, Partitions & Stirling Numbers
An **Equivalence Relation** is Reflexive, Symmetric, and Transitive. It partitions set $A$ into disjoint non-empty equivalence classes $[a] = \{x \in A \mid (x, a) \in R\}$.

1. **Stirling Numbers of the Second Kind $S(n, k)$ or $\left\{ \begin{matrix} n \\ k \end{matrix} \right\}$:**
   Number of ways to partition a set of $n$ labeled elements into $k$ non-empty unlabeled subsets:
   $$S(n, k) = \frac{1}{k!} \sum_{j=0}^k (-1)^{k-j} \binom{k}{j} j^n$$
   * **Recurrence:** $S(n, k) = S(n-1, k-1) + k \cdot S(n-1, k)$
   * Boundary: $S(n, 1) = 1$, $S(n, n) = 1$, $S(n, 2) = 2^{n-1} - 1$, $S(n, n-1) = \binom{n}{2}$.
2. **Bell Numbers $B(n)$ (Total Equivalence Relations):**
   $$B(n) = \sum_{k=1}^n S(n, k)$$
   * **Recurrence:** $B(n+1) = \sum_{k=0}^n \binom{n}{k} B(k)$
   * **Values:** $B(0)=1, B(1)=1, B(2)=2, B(3)=5, B(4)=15, B(5)=52, B(6)=203$.

---

### 2.3 Posets, Hasse Diagrams & Lattices
A **Partial Order (Poset)** is Reflexive, Antisymmetric, and Transitive.

* **Hasse Diagram Construction Rules:**
  1. Omit all self-loops (reflexivity is understood).
  2. Omit all directed edges implied by transitivity (if $a \to b$ and $b \to c$, omit $a \to c$).
  3. Draw edge $(a, b)$ with $b$ positioned higher than $a$ if $a < b$ (arrows omitted).
* **Extremal Elements in Posets:**
  * **Maximal Element $m$:** No element $x$ exists such that $m < x$. (A poset can have multiple maximal elements).
  * **Greatest Element (Maximum / Top $\top$):** An element $g$ such that $\forall x, x \le g$. (Unique if it exists).
  * **Minimal Element $w$:** No element $x$ exists such that $x < w$.
  * **Least Element (Minimum / Bottom $\bot$):** An element $l$ such that $\forall x, l \le x$. (Unique if it exists).
* **Lattice:** A poset $(L, \le)$ where every pair $\{a, b\}$ has:
  * A unique **Least Upper Bound (LUB / Supremum / Join $a \lor b$)**
  * A unique **Greatest Lower Bound (GLB / Infimum / Meet $a \land b$)**

---

### 2.4 Counting Mappings Between Sets
For functions $f: A \to B$ where $|A| = m$ and $|B| = n$:

```
+------------------------------------+---------------------------------------------------------------+
| Function Type                      | Cardinality Formula                                           |
+------------------------------------+---------------------------------------------------------------+
| Total Functions                    | $n^m$                                                         |
| Injective (1-to-1)                 | $P(n, m) = \frac{n!}{(n-m)!} \quad (m \le n); \quad 0 \ (m > n)$ |
| Surjective (Onto)                  | $n! \cdot S(m, n) = \sum_{j=0}^n (-1)^{n-j} \binom{n}{j} j^m$  |
| Bijective (1-to-1 & Onto)          | $n! \quad (\text{only if } m = n); \quad 0 \ (m \ne n)$       |
| Strictly Increasing ($A,B \subset \mathbb{R}$) | $\binom{n}{m} \quad (m \le n)$                                |
| Non-decreasing ($A,B \subset \mathbb{R}$)     | $\binom{n+m-1}{m}$                                            |
+------------------------------------+---------------------------------------------------------------+
```

---

### 2.5 Pigeonhole Principle (PHP) & Advanced Archetypes
* **Generalized Pigeonhole Principle:** If $N$ items are distributed into $k$ containers, then:
  * At least one container contains $\ge \lceil N/k \rceil$ items.
  * At least one container contains $\le \lfloor N/k \rfloor$ items.

#### Classical BUET Written Archetypes
1. **Subset Sum Modulo $n$:** Any subset of $n$ integers contains a non-empty subset whose sum is divisible by $n$.
   * *Proof:* Let the sequence be $a_1, a_2, \dots, a_n$. Form $n$ prefix sums: $S_k = \sum_{i=1}^k a_i \pmod n$. If any $S_k \equiv 0$, done. Otherwise, there are $n-1$ non-zero remainder pigeonholes for $n$ prefix sums. By PHP, two prefix sums $S_i \equiv S_j \pmod n$ ($i < j$). Then $\sum_{k=i+1}^j a_k = S_j - S_i \equiv 0 \pmod n$.
2. **Erdős-Szekeres Theorem:** Every sequence of $n^2 + 1$ distinct real numbers contains a monotonic subsequence (either strictly increasing or strictly decreasing) of length $n + 1$.

---

## 3. Advanced Combinatorics & Counting

### 3.1 Stars and Bars / Balls in Bins (The 12-Fold Way Core)
Number of integer solutions to $x_1 + x_2 + \dots + x_k = n$:

```
Case 1: Non-negative Integers (xi >= 0)
Place n identical stars (*) and (k-1) identical bars (|):
* * * | * * | | * * * *  ---> (n + k - 1) total slots, choose (k - 1) bars
Count = C(n + k - 1, k - 1) = C(n + k - 1, n)

Case 2: Strictly Positive Integers (xi >= 1)
Place (k-1) bars into the (n-1) spaces BETWEEN stars:
* | * | * * * | * *      ---> (n - 1) spaces, choose (k - 1) bars
Count = C(n - 1, k - 1)
```

#### Bounded Variables using PIE
To solve $x_1 + x_2 + x_3 = 15$ with $0 \le x_i \le 6$:
1. Total unrestricted solutions: $N = \binom{15 + 3 - 1}{3 - 1} = \binom{17}{2} = 136$.
2. Violations: Let $P_i$ be the condition $x_i \ge 7$.
   * For one variable violating ($x_1 \ge 7 \implies y_1 + x_2 + x_3 = 8$): $\binom{8+2}{2} = \binom{10}{2} = 45$.
   * $\sum |P_i| = \binom{3}{1} \times 45 = 135$.
   * For two variables violating ($x_1, x_2 \ge 7 \implies y_1 + y_2 + x_3 = 1$): $\binom{1+2}{2} = \binom{3}{2} = 3$.
   * $\sum |P_i \cap P_j| = \binom{3}{2} \times 3 = 9$.
   * Three variables violating: $7+7+7 = 21 > 15 \implies 0$.
3. Valid solutions $= 136 - 135 + 9 - 0 = 10$.

---

### 3.2 Combinatorial Identities & Proof Methods

```
+------------------------------------+---------------------------------------------------------------+
| Identity Name                      | Formula Statement                                             |
+------------------------------------+---------------------------------------------------------------+
| Pascal's Identity                  | $\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$            |
| Vandermonde's Convolution          | $\sum_{k=0}^r \binom{m}{k}\binom{n}{r-k} = \binom{m+n}{r}$    |
| Hockey-Stick Identity              | $\sum_{i=r}^n \binom{i}{r} = \binom{n+1}{r+1}$                |
| Weighted Sum of Coefficients       | $\sum_{k=0}^n k \binom{n}{k} = n 2^{n-1}$                     |
| Second-Moment Binomial Sum         | $\sum_{k=0}^n k^2 \binom{n}{k} = n(n+1) 2^{n-2}$              |
| Sum of Squares of Binomial Coeffs  | $\sum_{k=0}^n \binom{n}{k}^2 = \binom{2n}{n}$                 |
+------------------------------------+---------------------------------------------------------------+
```

#### Derivation of Weighted Sum ($\sum k \binom{n}{k}$):
* **Method 1 (Algebraic via Derivative):**
  $$(1 + x)^n = \sum_{k=0}^n \binom{n}{k} x^k$$
  Differentiating with respect to $x$:
  $$n(1 + x)^{n-1} = \sum_{k=0}^n k \binom{n}{k} x^{k-1}$$
  Substituting $x = 1$:
  $$n(2)^{n-1} = \sum_{k=0}^n k \binom{n}{k}$$
* **Method 2 (Combinatorial Committee Selection):**
  Number of ways to choose a committee of $k$ members from $n$ people with 1 designated leader:
  $$\sum_{k=1}^n k \binom{n}{k} = \binom{n}{1} \times 2^{n-1} = n 2^{n-1}$$

---

## 4. Concrete Mathematics: Recurrences, Sums & Generating Functions

### 4.1 Closed-Form Summations & Perturbation Method
* **Sum of Arithmetic Progression with Geometric Weight (AGP):**
  $$S_n = \sum_{k=1}^n k x^k = x + 2x^2 + 3x^3 + \dots + n x^n$$
  Multiplying by $x$:
  $$x S_n = x^2 + 2x^3 + \dots + (n-1)x^n + n x^{n+1}$$
  Subtracting:
  $$(1 - x)S_n = x + x^2 + x^3 + \dots + x^n - n x^{n+1} = \frac{x(1 - x^n)}{1 - x} - n x^{n+1}$$
  $$S_n = \frac{x - (n+1)x^{n+1} + n x^{n+2}}{(1 - x)^2} \quad (x \ne 1)$$
  * For $|x| < 1$ as $n \to \infty$: $\sum_{k=1}^\infty k x^k = \frac{x}{(1-x)^2}$.

---

### 4.2 Linear Recurrence Relations

#### Case 1: Homogeneous Recurrences ($a_n = c_1 a_{n-1} + c_2 a_{n-2}$)
Characteristic equation: $r^2 - c_1 r - c_2 = 0$ with roots $r_1, r_2$:
* **Distinct Roots ($r_1 \ne r_2$):** $a_n = C_1 r_1^n + C_2 r_2^n$.
* **Repeated Roots ($r_1 = r_2 = r$):** $a_n = (C_1 + C_2 n) r^n$.
* **Complex Conjugate Roots ($r = \rho e^{\pm i\theta} = \rho(\cos\theta \pm i\sin\theta)$):**
  $$a_n = \rho^n [C_1 \cos(n\theta) + C_2 \sin(n\theta)]$$

#### Case 2: Inhomogeneous Recurrences ($a_n = c_1 a_{n-1} + c_2 a_{n-2} + F(n)$)
General solution is $a_n = a_n^{(h)} + a_n^{(p)}$.

```
+------------------------------------+---------------------------------------------------------------+
| Inhomogeneous Term F(n)            | Trial Form for Particular Solution a_n^(p)                    |
+------------------------------------+---------------------------------------------------------------+
| Constant $C$                       | $A_0$ (if $1$ is a root of mult. $m$, use $A_0 n^m$)          |
| Degree-$d$ Polynomial $P_d(n)$     | $A_d n^d + A_{d-1} n^{d-1} + \dots + A_0$                     |
| Exponential $s^n$                  | $A s^n$ (if $s$ is char. root of mult. $m$, use $A n^m s^n$)  |
| $P_d(n) s^n$                       | $n^m (A_d n^d + \dots + A_0) s^n$                             |
+------------------------------------+---------------------------------------------------------------+
```

---

### 4.3 Master Theorem & Recursion Trees
For divide-and-conquer recurrences: $T(n) = a T(n/b) + f(n)$ where $a \ge 1, b > 1$:
* **Critical Exponent:** $c_{\text{crit}} = \log_b a$.
* **Watershed Comparison:** Compare $f(n)$ with $n^{\log_b a}$:

```
        Case 1: Leaves Dominate                Case 2: Balanced Work                Case 3: Root Dominates
      f(n) = O(n^(log_b(a) - ε))          f(n) = Θ(n^(log_b(a)) log^k n)          f(n) = Ω(n^(log_b(a) + ε))
                 |                                       |                                       |
        T(n) = Θ(n^(log_b a))               T(n) = Θ(n^(log_b a) log^(k+1) n)                 T(n) = Θ(f(n))
```

* **Important Pitfall:** Master Theorem **fails** if the ratio $\frac{f(n)}{n^{\log_b a}}$ is polynomial-gap deficient (e.g., $f(n) = n^{\log_b a} / \log n$). Use recursion tree or substitution instead.

---

### 4.4 Ordinary Generating Functions (OGF)
For sequence $\langle a_0, a_1, a_2, \dots \rangle$, its OGF is $A(x) = \sum_{n=0}^\infty a_n x^n$.

* **Transformation Properties:**
  * Shift Left: $\sum_{n=0}^\infty a_{n+1} x^n = \frac{A(x) - a_0}{x}$
  * Multiply by $n$: $\sum_{n=0}^\infty n a_n x^n = x \frac{d}{dx} A(x)$
  * Convolution ($c_n = \sum_{k=0}^n a_k b_{n-k}$): $C(x) = A(x) \cdot B(x)$
  * Partial Sums ($s_n = \sum_{k=0}^n a_k$): $S(x) = \frac{A(x)}{1 - x}$

---

## 5. Graph Theory & Network Invariants

```
                Graph Theory Core Invariants
                      |V| = n Vertices
                     /                \
        Independent Set (α)      Vertex Cover (β)
        [Max set without edges]  [Min set covering all edges]
                     \                /
                      \              /
                  Gallai's Identity: α + β = |V|
```

### 5.1 Handshaking Lemma & Degree Sequences
* **Handshaking Lemma:** $\sum_{v \in V} \deg(v) = 2|E|$.
* **Parity Property:** The number of odd-degree vertices in any undirected graph is **always even**.
* **Havel-Hakimi Theorem (Graphic Degree Sequences):**
  To test if a degree sequence $d_1 \ge d_2 \ge \dots \ge d_n$ is graphic:
  1. Remove $d_1$.
  2. Subtract $1$ from each of the next $d_1$ terms.
  3. Re-sort in non-increasing order. Repeat until all zeros (graphic) or negative numbers appear (not graphic).

---

### 5.2 Special Graph Families

```
+------------------------------------+------------+-------------------+-----------------+---------------+
| Graph Family                       | Notation   | Vertices |V|      | Edges |E|       | Chromatic χ(G)|
+------------------------------------+------------+-------------------+-----------------+---------------+
| Complete Graph                     | $K_n$      | $n$               | $\binom{n}{2}$  | $n$           |
| Complete Bipartite                 | $K_{m,n}$  | $m + n$           | $m \cdot n$     | $2$           |
| Cycle Graph                        | $C_n$      | $n$               | $n$             | $2$ (even $n$), $3$ (odd $n$) |
| Wheel Graph                        | $W_n$      | $n + 1$           | $2n$            | $3$ (even $n$), $4$ (odd $n$) |
| $d$-Dimensional Hypercube          | $Q_d$      | $2^d$             | $d \cdot 2^{d-1}$ | $2$ (bipartite) |
+------------------------------------+------------+-------------------+-----------------+---------------+
```

---

### 5.3 Planar Graphs & Kuratowski’s Theorem
A graph is **planar** if it can be drawn in a single plane without edges crossing.

1. **Euler's Planar Formula:** For any connected planar graph with $V$ vertices, $E$ edges, and $F$ faces (including external unbounded face):
   $$V - E + F = 2$$
2. **Planar Edge Inequalities:**
   * Since every face is bounded by at least 3 edges: $2E \ge 3F \implies F \le \frac{2}{3}E$.
   * Substituting into Euler's formula:
     $$V - E + \frac{2}{3}E \ge 2 \implies E \le 3V - 6 \quad (\text{for } V \ge 3)$$
   * If the planar graph is **triangle-free** (e.g., Bipartite, girth $\ge 4$): $2E \ge 4F \implies$
     $$E \le 2V - 4 \quad (\text{for } V \ge 3)$$
3. **Kuratowski's & Wagner's Theorems:** A graph is planar $\iff$ it does not contain a subgraph homeomorphic to, or contractible to, $K_5$ or $K_{3,3}$.

---

### 5.4 Graph Coloring & Chromatic Polynomial
* **Chromatic Number $\chi(G)$:** Minimal number of colors needed to color vertices such that no two adjacent vertices share a color.
* **Bipartite Equivalence:** A graph $G$ is 2-colorable ($\chi(G) \le 2$) $\iff G$ is bipartite $\iff G$ has **no odd cycles**.
* **Four-Color Theorem:** Every planar graph has $\chi(G) \le 4$.
* **Chromatic Polynomial $P(G, k)$:** Number of proper $k$-colorings of $G$.
  * For $K_n$: $P(K_n, k) = k(k-1)(k-2)\dots(k-n+1)$.
  * For Tree $T_n$: $P(T_n, k) = k(k-1)^{n-1}$.
  * For Cycle $C_n$: $P(C_n, k) = (k-1)^n + (-1)^n(k-1)$.

---

### 5.5 Eulerian vs Hamiltonian Graphs

```
+------------------------------------+---------------------------------------------------------------+
| Eulerian Characteristic            | Hamiltonian Characteristic                                    |
+------------------------------------+---------------------------------------------------------------+
| Traverses every **EDGE** once      | Traverses every **VERTEX** once                               |
| Circuit $\iff$ All vertices EVEN deg | NP-Complete to determine in general                           |
| Path $\iff$ Exactly 0 or 2 ODD deg | **Dirac's Theorem:** $\deg(v) \ge n/2, \forall v \implies$ Hamiltonian |
| Polynomial time solvable ($O(V+E)$)| **Ore's Theorem:** $\deg(u)+\deg(v) \ge n, \forall (u,v)\notin E \implies$ Ham.|
+------------------------------------+---------------------------------------------------------------+
```

---

### 5.6 Independent Sets, Vertex Covers & Gallai's Theorem

```
           G = (V, E)
           +-------------------------------------------------------+
           | Vertex Set V                                          |
           |   +-----------------------+   +---------------------+ |
           |   | Independent Set S     |   | Vertex Cover V \ S  | |
           |   | (No two adj. in S)    |   | (Covers all edges)  | |
           |   +-----------------------+   +---------------------+ |
           +-------------------------------------------------------+
```

#### Theorem 1 (Complement Duality)
A set $S \subseteq V$ is an **Independent Set** if and only if its complement $V \setminus S$ is a **Vertex Cover**.
* **Proof:**
  * $(\implies)$ Suppose $S$ is an independent set. Take any edge $(u, v) \in E$. Since $S$ is independent, $u$ and $v$ cannot both be in $S$. Therefore, at least one endpoint belongs to $V \setminus S$. Hence, $V \setminus S$ covers all edges (is a Vertex Cover).
  * $(\impliedby)$ Suppose $V \setminus S$ is a vertex cover. If $S$ contained an edge $(u, v)$, neither $u$ nor $v$ would be in $V \setminus S$, violating the fact that $V \setminus S$ covers $(u, v)$. Thus, $S$ has no edges (is an Independent Set).

#### Theorem 2 (Gallai's Identities)
For any graph $G = (V, E)$ without isolated vertices:
$$\alpha(G) + \beta(G) = |V|$$
$$\alpha'(G) + \beta'(G) = |V|$$
where:
* $\alpha(G) = \text{Maximum Independent Set Size}$
* $\beta(G) = \text{Minimum Vertex Cover Size}$
* $\alpha'(G) = \text{Maximum Matching Size}$
* $\beta'(G) = \text{Minimum Edge Cover Size}$

---

## 6. Number Theory & Modular Arithmetic Essentials

### 6.1 Extended Euclidean Algorithm & Modular Inverses
* **Bézout's Theorem:** For integers $a, b$, there exist integers $x, y$ such that $a x + b y = \gcd(a, b)$.
* **Modular Multiplicative Inverse:** $a x \equiv 1 \pmod m$ exists $\iff \gcd(a, m) = 1$.

```python
# Extended GCD Algorithm: returns (gcd, x, y) such that a*x + b*y = gcd
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return g, x, y
```

---

### 6.2 Fermat's Little Theorem & Euler's Totient
1. **Euler's Totient Function $\phi(n)$:** Count of integers $1 \le k \le n$ coprime to $n$.
   $$\phi(n) = n \prod_{p \mid n} \left(1 - \frac{1}{p}\right) = p_1^{e_1 - 1}(p_1 - 1) p_2^{e_2 - 1}(p_2 - 1) \dots$$
2. **Euler's Generalization:**
   $$a^{\phi(n)} \equiv 1 \pmod n \quad (\text{for } \gcd(a, n) = 1)$$
3. **Fermat's Little Theorem ($n = p$ is prime):**
   $$a^{p-1} \equiv 1 \pmod p \implies a^p \equiv a \pmod p$$

---

### 6.3 Chinese Remainder Theorem (CRT)
For pairwise coprime moduli $m_1, m_2, \dots, m_k$, the system of congruences:
$$x \equiv a_i \pmod{m_i} \quad (i = 1, \dots, k)$$
has a unique solution modulo $M = m_1 m_2 \dots m_k$:
$$x = \sum_{i=1}^k a_i M_i y_i \pmod M$$
where $M_i = M / m_i$ and $y_i \equiv M_i^{-1} \pmod{m_i}$.

---

# PART 2: PROBABILITY & RANDOM PROCESSES

---

## 7. Axiomatic Probability & Bayesian Inference

### 7.1 Probability Spaces & Conditional Probability
* **Kolmogorov Axioms:**
  1. $P(A) \ge 0$ for every event $A$.
  2. $P(S) = 1$ (Certain event).
  3. Countable Additivity: For disjoint events $A_1, A_2, \dots$, $P(\bigcup A_i) = \sum P(A_i)$.
* **Conditional Probability Definition:**
  $$P(A \mid B) = \frac{P(A \cap B)}{P(B)} \quad (P(B) > 0)$$
* **Multiplication Rule:**
  $$P(A \cap B) = P(A) P(B \mid A) = P(B) P(A \mid B)$$
  $$P(A_1 \cap A_2 \cap \dots \cap A_n) = P(A_1) P(A_2 \mid A_1) P(A_3 \mid A_1 \cap A_2) \dots P(A_n \mid \bigcap_{i=1}^{n-1} A_i)$$

---

### 7.2 Independent Events vs Mutually Exclusive Events

```
+------------------------------------+---------------------------------------------------------------+
| Mutually Exclusive (Disjoint)      | Statistically Independent                                     |
+------------------------------------+---------------------------------------------------------------+
| Events CANNOT occur together       | Occurrence of one provides NO information about the other     |
| $P(A \cap B) = 0$                  | $P(A \cap B) = P(A) \cdot P(B)$                               |
| $P(A \cup B) = P(A) + P(B)$        | $P(A \mid B) = P(A)$                                          |
| If $P(A), P(B) > 0 \implies$ DEPENDENT | If $P(A), P(B) > 0 \implies$ CANNOT BE MUTUALLY EXCLUSIVE     |
+------------------------------------+---------------------------------------------------------------+
```

---

### 7.3 Law of Total Probability & Bayes' Theorem

```
                      Sample Space S Partitioned by B_i
           +---------------------+---------------------+---------------------+
           |         B_1         |         B_2         |         B_3         |
           |        .---.        |       .-----.       |        .---.        |
           |       /     \       |      /       \      |       /     \       |
           |      (       )------+-----(         )-----+------(       )      |
           |       \     /       |      \       /      |       \     /       |
           |        '---'        |       '-----'       |        '---'        |
           |      P(A|B_1)P(B_1) |    P(A|B_2)P(B_2)   |    P(A|B_3)P(B_3)   |
           +---------------------+---------------------+---------------------+
                                Total P(A) = ∑ P(A|B_i)P(B_i)
```

#### Bayes' Theorem (Updating Prior Beliefs with Evidence):
$$P(B_k \mid A) = \frac{P(A \mid B_k) P(B_k)}{\sum_{i=1}^n P(A \mid B_i) P(B_i)}$$
* $P(B_k)$ = **Prior Probability** (Initial belief before evidence).
* $P(A \mid B_k)$ = **Likelihood** (Probability of evidence given hypothesis).
* $P(A)$ = **Marginal Probability / Evidence**.
* $P(B_k \mid A)$ = **Posterior Probability** (Updated belief after observing evidence).

---

## 8. Random Variables, Expectation & Variance

### 8.1 Continuous vs Discrete Random Variables

```
+-----------------------+---------------------------------------+---------------------------------------+
| Concept               | Discrete Random Variable $X$          | Continuous Random Variable $X$        |
+-----------------------+---------------------------------------+---------------------------------------+
| Density / Mass        | PMF: $p(x) = P(X = x)$                | PDF: $f(x) \ge 0$                     |
| Normalization         | $\sum_x p(x) = 1$                     | $\int_{-\infty}^\infty f(x) dx = 1$   |
| Probability in Range  | $P(a \le X \le b) = \sum_{x=a}^b p(x)$| $P(a \le X \le b) = \int_a^b f(x) dx$ |
| CDF $F(x) = P(X\le x)$| $F(x) = \sum_{t \le x} p(t)$          | $F(x) = \int_{-\infty}^x f(t) dt$     |
| Derivative Property   | $p(x_k) = F(x_k) - F(x_{k-1})$        | $f(x) = \frac{d}{dx} F(x)$            |
| Point Probability     | $P(X = x) \ge 0$                      | $P(X = c) = 0 \quad (\forall c)$      |
| Expectation $E[X]$    | $\sum_x x p(x)$                       | $\int_{-\infty}^\infty x f(x) dx$     |
+-----------------------+---------------------------------------+---------------------------------------+
```

---

### 8.2 Linearity of Expectation & The Indicator Variable Trick

```
                     UNIVERSAL THEOREM: LINEARITY OF EXPECTATION
                     E[aX + bY + c] = a E[X] + b E[Y] + c
               (Holds true ALWAYS, whether X and Y are independent or not!)
```

#### Indicator Random Variable Technique
An indicator variable for event $A$ is:
$$I_A = \begin{cases} 1 & \text{if event } A \text{ occurs} \\ 0 & \text{if event } A \text{ does not occur} \end{cases}$$
* **Fundamental Identity:** $E[I_A] = 1 \cdot P(A) + 0 \cdot P(\bar{A}) = P(A)$.
* To find the expected count of occurrences $X = \sum_{i=1}^n I_{A_i}$:
  $$E[X] = \sum_{i=1}^n E[I_{A_i}] = \sum_{i=1}^n P(A_i)$$

---

### 8.3 Variance, Standard Deviation & Covariance
* **Variance:** $\text{Var}(X) = \sigma^2 = E[(X - \mu)^2] = E[X^2] - (E[X])^2$.
* **Standard Deviation:** $\sigma = \sqrt{\text{Var}(X)}$.
* **Linear Transformations:**
  $$\text{Var}(aX + b) = a^2 \text{Var}(X)$$
* **Sum of Random Variables:**
  $$\text{Var}(X \pm Y) = \text{Var}(X) + \text{Var}(Y) \pm 2 \text{Cov}(X, Y)$$
* **Covariance:**
  $$\text{Cov}(X, Y) = E[(X - \mu_X)(Y - \mu_Y)] = E[XY] - E[X]E[Y]$$
  * If $X, Y$ are independent $\implies \text{Cov}(X, Y) = 0 \implies \text{Var}(X \pm Y) = \text{Var}(X) + \text{Var}(Y)$.
  * *Note:* $\text{Cov}(X, Y) = 0$ does NOT necessarily imply independence!

---

### 8.4 Moment Generating Functions (MGF)
The MGF of $X$ is $M_X(t) = E[e^{tX}]$.
* **Extracting Moments:**
  $$E[X^n] = M_X^{(n)}(0) = \left. \frac{d^n}{dt^n} M_X(t) \right|_{t=0}$$
* **Sum of Independent Variables:**
  $$M_{X+Y}(t) = M_X(t) \cdot M_Y(t) \quad (\text{if } X, Y \text{ are independent})$$

---

## 9. Standard Probability Distributions

### 9.1 Exhaustive Distribution Matrix

```
+----------------------------------------------------------------------------------------------------+
|                               STANDARD PROBABILITY DISTRIBUTIONS                                   |
+--------------------+--------------------------------+---------------+---------------+--------------+
| Distribution       | PMF / PDF                      | Mean E[X]     | Var(X)        | MGF M_X(t)   |
+--------------------+--------------------------------+---------------+---------------+--------------+
| **Bernoulli(p)**   | $P(X=1)=p, P(X=0)=1-p$         | $p$           | $p(1-p)$      | $1-p+pe^t$   |
| **Binomial(n, p)** | $\binom{n}{k} p^k (1-p)^{n-k}$ | $np$          | $np(1-p)$     | $(1-p+pe^t)^n$|
| **Geometric(p)**   | $(1-p)^{k-1}p \quad (k\ge 1)$  | $\frac{1}{p}$ | $\frac{1-p}{p^2}$ | $\frac{pe^t}{1-(1-p)e^t}$ |
| **Neg. Binomial(r,p)** | $\binom{k-1}{r-1} p^r (1-p)^{k-r}$ | $\frac{r}{p}$ | $\frac{r(1-p)}{p^2}$ | $\left(\frac{pe^t}{1-(1-p)e^t}\right)^r$ |
| **Poisson($\lambda$)** | $\frac{\lambda^k e^{-\lambda}}{k!}$ | $\lambda$   | $\lambda$     | $e^{\lambda(e^t - 1)}$ |
| **Uniform(a, b)**  | $\frac{1}{b-a}, \ x \in [a, b]$| $\frac{a+b}{2}$| $\frac{(b-a)^2}{12}$ | $\frac{e^{tb}-e^{ta}}{t(b-a)}$ |
| **Exponential($\lambda$)** | $\lambda e^{-\lambda x}, \ x \ge 0$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ | $\frac{\lambda}{\lambda - t} \ (t < \lambda)$ |
| **Normal($\mu, \sigma^2$)**| $\frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ | $\mu$ | $\sigma^2$ | $e^{\mu t + \frac{1}{2}\sigma^2 t^2}$ |
+--------------------+--------------------------------+---------------+---------------+--------------+
```

---

### 9.2 Memoryless Property Proof
A distribution is memoryless if $P(X > s + t \mid X > s) = P(X > t)$ for all $s, t > 0$.

#### Proof for Exponential Distribution:
* CDF: $F(x) = 1 - e^{-\lambda x} \implies P(X > x) = e^{-\lambda x}$.
* Conditional probability:
  $$P(X > s + t \mid X > s) = \frac{P(X > s + t \cap X > s)}{P(X > s)} = \frac{P(X > s + t)}{P(X > s)} = \frac{e^{-\lambda(s+t)}}{e^{-\lambda s}} = e^{-\lambda t} = P(X > t)$$
* **Significance:** Only **Geometric** (discrete) and **Exponential** (continuous) possess this property.

---

### 9.3 Central Limit Theorem (CLT)
Let $X_1, X_2, \dots, X_n$ be i.i.d. random variables with mean $\mu$ and finite variance $\sigma^2$. As $n \to \infty$, the sample mean $\bar{X}_n = \frac{1}{n} \sum X_i$ approaches a Normal distribution:
$$\bar{X}_n \sim N\left(\mu, \frac{\sigma^2}{n}\right) \iff Z = \frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} \sim N(0, 1)$$

---

## 10. Statistics Fundamentals, Regression & Bounds

### 10.1 Measures of Central Tendency & Dispersion
1. **Inequality of Means:**
   $$\text{AM} \ge \text{GM} \ge \text{HM}$$
   $$\frac{\sum x_i}{n} \ge \sqrt[n]{\prod x_i} \ge \frac{n}{\sum \frac{1}{x_i}}$$
   For two positive values $a, b$: $GM^2 = AM \times HM$.
2. **Empirical Skewness Relationship:**
   $$\text{Mode} \approx 3 \times \text{Median} - 2 \times \text{Mean}$$
3. **Coefficient of Variation ($CV$):**
   $$CV = \frac{\sigma}{\mu} \times 100\% \quad (\text{Dimensionless metric to compare variability})$$

---

### 10.2 Linear Regression & Correlation

```
                       Bivariate Regression Geometry
                 y ^
                   |            / (Line of Y on X: slope byx)
                   |           / 
                   |          /   . (x_bar, y_bar) Intersection
                   |         / .
                   |        /.
                   |       /---------------- (Line of X on Y: slope 1/bxy)
                   +----------------------------------> x
```

* **Pearson Correlation Coefficient ($r$):**
  $$r = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}, \qquad -1 \le r \le 1$$
* **Regression Lines Equations:**
  * Line of $Y$ on $X$: $(y - \bar{y}) = b_{yx} (x - \bar{x})$ where $b_{yx} = r \frac{\sigma_y}{\sigma_x} = \frac{\text{Cov}(X,Y)}{\sigma_x^2}$
  * Line of $X$ on $Y$: $(x - \bar{x}) = b_{xy} (y - \bar{y})$ where $b_{xy} = r \frac{\sigma_x}{\sigma_y} = \frac{\text{Cov}(X,Y)}{\sigma_y^2}$
* **Fundamental Properties:**
  1. $r^2 = b_{yx} \cdot b_{xy} \implies r = \pm \sqrt{b_{yx} \cdot b_{xy}}$ (Sign matches $b_{yx}$ and $b_{xy}$).
  2. If $|b_{yx}| > 1 \implies |b_{xy}| < 1$.
  3. $\frac{b_{yx} + b_{xy}}{2} \ge |r|$.
  4. Both regression lines intersect at the centroid $(\bar{x}, \bar{y})$.
  5. Acute angle $\theta$ between regression lines:
     $$\tan \theta = \left| \frac{1 - r^2}{r} \right| \left( \frac{\sigma_x \sigma_y}{\sigma_x^2 + \sigma_y^2} \right)$$
     * If $r = \pm 1 \implies \theta = 0^\circ$ (Lines coincide).
     * If $r = 0 \implies \theta = 90^\circ$ (Lines are perpendicular: $x = \bar{x}$ and $y = \bar{y}$).

---

### 10.3 Probability Bounds & Concentration Inequalities
1. **Markov's Inequality:** For non-negative RV $X \ge 0$ and $a > 0$:
   $$P(X \ge a) \le \frac{E[X]}{a}$$
   * *Proof:* $E[X] = \int_0^\infty x f(x) dx \ge \int_a^\infty x f(x) dx \ge a \int_a^\infty f(x) dx = a P(X \ge a)$.
2. **Chebyshev's Inequality:** For any RV $X$ with mean $\mu$ and variance $\sigma^2$:
   $$P(|X - \mu| \ge k\sigma) \le \frac{1}{k^2} \iff P(|X - \mu| < k\sigma) \ge 1 - \frac{1}{k^2}$$

---

# PART 3: IN-DEPTH WORKED BUET ARCHIVES & EXHAUSTIVE NUMERICALS

---

## 11. Discrete Mathematics: Detailed Step-by-Step Solutions

### 💡 Problem 11.1: Maximum Independent Set & Minimum Vertex Cover [BUET MSc CSE 21 Archive]
**Problem Statement:**
Let $G = (V, E)$ be an undirected graph where $V = \{a, b, c, d, e, f, g, h, i, j, k\}$. If $S = \{a, c, g, h, k\}$ is known to be a maximum independent set in $G$, determine:
1. The numerical size of the minimum vertex cover.
2. The exact vertex set of the minimum vertex cover.
3. The theoretical foundation supporting this relationship.

**Detailed Solution:**
* **Step 1: Vertex Count & Set Cardinality:**
  The total vertex set $V$ has size $|V| = 11$.
  The given maximum independent set is $S = \{a, c, g, h, k\}$, which has cardinality:
  $$\alpha(G) = |S| = 5$$
* **Step 2: Invoking Gallai’s Identity:**
  By Gallai's Theorem for vertex invariants, for any undirected graph $G = (V, E)$:
  $$\alpha(G) + \beta(G) = |V|$$
  where $\alpha(G)$ is the independence number and $\beta(G)$ is the vertex cover number.
  $$\beta(G) = |V| - \alpha(G) = 11 - 5 = 6$$
* **Step 3: Determining the Vertex Cover Set:**
  Let $C = V \setminus S$. For every edge $(u, v) \in E$, since $S$ is an independent set, $u$ and $v$ cannot both belong to $S$. Therefore, at least one of $u$ or $v$ must belong to $V \setminus S$. Thus, $V \setminus S$ covers all edges in $E$.
  $$V \setminus S = \{a, b, c, d, e, f, g, h, i, j, k\} \setminus \{a, c, g, h, k\} = \{b, d, e, f, i, j\}$$
**Final Answer:** Minimum vertex cover size = **6**, Vertex set = **$\{b, d, e, f, i, j\}$**.

---

### 💡 Problem 11.2: Second-Order Inhomogeneous Recurrence with Multiplicity
**Problem Statement:**
Solve the recurrence relation:
$$a_n - 4a_{n-1} + 4a_{n-2} = 2^n \quad (n \ge 2) \quad \text{with } a_0 = 1, a_1 = 4$$

**Detailed Solution:**
* **Step 1: Homogeneous Solution ($a_n^{(h)}$):**
  The homogeneous equation is $a_n - 4a_{n-1} + 4a_{n-2} = 0$.
  Characteristic equation: $r^2 - 4r + 4 = 0 \implies (r - 2)^2 = 0 \implies r = 2$ (Multiplicity $m = 2$).
  $$a_n^{(h)} = (C_1 + C_2 n) 2^n$$
* **Step 2: Particular Solution ($a_n^{(p)}$):**
  The inhomogeneous forcing term is $F(n) = 2^n = 1 \cdot 2^n$.
  Since $s = 2$ is already a characteristic root of multiplicity $m = 2$, the trial solution must be multiplied by $n^2$:
  $$a_n^{(p)} = A n^2 2^n$$
  Substitute $a_n^{(p)}$ into the recurrence:
  $$A n^2 2^n - 4 A (n-1)^2 2^{n-1} + 4 A (n-2)^2 2^{n-2} = 2^n$$
  Divide through by $2^{n-2}$:
  $$4 A n^2 - 8 A (n^2 - 2n + 1) + 4 A (n^2 - 4n + 4) = 4$$
  $$A [4n^2 - 8n^2 + 16n - 8 + 4n^2 - 16n + 16] = 4$$
  $$A [8] = 4 \implies A = \frac{1}{2}$$
  Therefore: $a_n^{(p)} = \frac{1}{2} n^2 2^n = n^2 2^{n-1}$.
* **Step 3: General Solution & Initial Conditions:**
  $$a_n = a_n^{(h)} + a_n^{(p)} = (C_1 + C_2 n) 2^n + n^2 2^{n-1}$$
  * For $n = 0$: $a_0 = C_1 \cdot 1 + 0 = 1 \implies C_1 = 1$.
  * For $n = 1$: $a_1 = (1 + C_2) 2^1 + 1^2 \cdot 2^0 = 2(1 + C_2) + 1 = 4 \implies 2 + 2C_2 + 1 = 4 \implies C_2 = \frac{1}{2}$.
* **Step 4: Final Form:**
  $$a_n = \left(1 + \frac{1}{2}n\right) 2^n + n^2 2^{n-1} = 2^n + n 2^{n-1} + n^2 2^{n-1} = 2^n + (n^2 + n) 2^{n-1}$$
**Final Answer:** $a_n = 2^n + n(n+1)2^{n-1}$.

---

## 12. Probability & Statistics: Detailed Step-by-Step Solutions

### 💡 Problem 12.1: Multi-Stage Bayes' Theorem & Diagnostic Risk
**Problem Statement:**
A high-precision semiconductor fab produces chips where $1\%$ of the output has micro-defects. An automated optical inspection (AOI) machine has:
* $98\%$ probability of correctly flagging a defective chip as defective (Sensitivity = 0.98).
* $3\%$ probability of erroneously flagging a non-defective chip as defective (False Alarm / Fall-out = 0.03).

If a batch of chips is scanned and a randomly chosen chip is flagged as defective:
1. What is the probability that this chip is genuinely defective?
2. If the chip is re-tested with an independent second inspection machine with the same parameters and flags defective again, what is the updated posterior probability?

**Detailed Solution:**

```
                  Tree Diagram for First Inspection Scan
                                  Root
                                 /    \
                     P(D) = 0.01/      \P(D') = 0.99
                               /        \
                          Defective     Non-Defective
                          /      \         /       \
                     0.98/   0.02 \   0.03/    0.97 \
                        +          -     +           -
```

* **Part 1: Single Test Posterior:**
  * Let $D$ be the event that the chip is defective $\implies P(D) = 0.01, P(\bar{D}) = 0.99$.
  * Let $T_1^+$ be the event that the first test flags defective.
  * $P(T_1^+ \mid D) = 0.98$ and $P(T_1^+ \mid \bar{D}) = 0.03$.
  * Marginal probability:
    $$P(T_1^+) = P(T_1^+ \mid D)P(D) + P(T_1^+ \mid \bar{D})P(\bar{D})$$
    $$P(T_1^+) = (0.98)(0.01) + (0.03)(0.99) = 0.0098 + 0.0297 = 0.0395$$
  * Applying Bayes' Theorem:
    $$P(D \mid T_1^+) = \frac{P(T_1^+ \mid D)P(D)}{P(T_1^+)} = \frac{0.0098}{0.0395} = \frac{98}{395} \approx 0.2481 \quad (24.81\%)$$

* **Part 2: Sequential Independent Second Test:**
  * Now, the posterior from Test 1 becomes the **new prior** for Test 2:
    $$P_{\text{new}}(D) = \frac{98}{395} \approx 0.2481, \qquad P_{\text{new}}(\bar{D}) = \frac{297}{395} \approx 0.7519$$
  * Marginal probability for second test $T_2^+$:
    $$P(T_2^+) = (0.98)\left(\frac{98}{395}\right) + (0.03)\left(\frac{297}{395}\right) = \frac{96.04 + 8.91}{395} = \frac{104.95}{395}$$
  * Posterior after two consecutive positive tests:
    $$P(D \mid T_1^+ \cap T_2^+) = \frac{96.04 / 395}{104.95 / 395} = \frac{96.04}{104.95} \approx 0.9151 \quad (91.51\%)$$
**Final Answer:** Single test posterior = **$24.81\%$**; Two consecutive tests posterior = **$91.51\%$**.

---

### 💡 Problem 12.2: Linearity of Expectation with Indicator Variables (Hat-Check / Derangement Matching)
**Problem Statement:**
$n$ candidates attend a BUET written exam and deposit their calculators at the entrance. At the end of the exam, the calculators are handed back completely at random.
1. Find the expected number of candidates who receive their own calculator.
2. Find the variance of the number of candidates who receive their own calculator.

**Detailed Solution:**
* **Part 1: Expected Value Calculation:**
  * Let $X$ be the total number of students receiving their own calculator.
  * Define indicator random variables for each candidate $i \in \{1, 2, \dots, n\}$:
    $$I_i = \begin{cases} 1 & \text{if candidate } i \text{ receives their own calculator} \\ 0 & \text{otherwise} \end{cases}$$
  * Then $X = \sum_{i=1}^n I_i$.
  * The probability that candidate $i$ gets their own calculator is:
    $$P(I_i = 1) = \frac{1}{n} \implies E[I_i] = \frac{1}{n}$$
  * By **Linearity of Expectation** (holds regardless of dependence):
    $$E[X] = \sum_{i=1}^n E[I_i] = \sum_{i=1}^n \frac{1}{n} = n \cdot \frac{1}{n} = 1$$
  *(Remarkable Result: The expected number of matches is always $1$, regardless of whether $n = 10$ or $n = 1,000,000$).*

* **Part 2: Variance Calculation:**
  $$\text{Var}(X) = E[X^2] - (E[X])^2$$
  $$X^2 = \left(\sum_{i=1}^n I_i\right)^2 = \sum_{i=1}^n I_i^2 + \sum_{i \ne j} I_i I_j$$
  * Since $I_i \in \{0, 1\}$, $I_i^2 = I_i \implies E[I_i^2] = E[I_i] = \frac{1}{n}$.
  * For $i \ne j$, $I_i I_j = 1 \iff$ both $i$ and $j$ get their own calculators:
    $$P(I_i = 1 \land I_j = 1) = \frac{1}{n} \cdot \frac{1}{n-1} \implies E[I_i I_j] = \frac{1}{n(n-1)}$$
  * Total ordered pairs $(i, j)$ with $i \ne j$ is $n(n-1)$.
  * Therefore:
    $$E[X^2] = \sum_{i=1}^n E[I_i] + \sum_{i \ne j} E[I_i I_j] = n \left(\frac{1}{n}\right) + n(n-1) \left(\frac{1}{n(n-1)}\right) = 1 + 1 = 2$$
  * Finally:
    $$\text{Var}(X) = E[X^2] - (E[X])^2 = 2 - (1)^2 = 1$$
**Final Answer:** Expected matches = **$1$**, Variance = **$1$**.

---

### 💡 Problem 12.3: Bivariate Regression Analysis & Correlation
**Problem Statement:**
In an empirical study, two linear regression equations were estimated:
$$\text{Line 1: } 4x - 5y + 33 = 0$$
$$\text{Line 2: } 20x - 9y - 107 = 0$$
Given that $\sigma_x^2 = 25$, determine:
1. The mean values $\bar{x}$ and $\bar{y}$.
2. Which line is the regression of $Y$ on $X$, and which is $X$ on $Y$.
3. The Pearson correlation coefficient $r$.
4. The standard deviation of $Y$ ($\sigma_y$).

**Detailed Solution:**
* **Step 1: Calculating Centroid Means $(\bar{x}, \bar{y})$:**
  Both regression lines must intersect at $(\bar{x}, \bar{y})$:
  $$\begin{cases} 4\bar{x} - 5\bar{y} = -33 \quad \dots (1) \\ 20\bar{x} - 9\bar{y} = 107 \quad \dots (2) \end{cases}$$
  Multiply equation (1) by 5:
  $$20\bar{x} - 25\bar{y} = -165 \quad \dots (3)$$
  Subtract (3) from (2):
  $$16\bar{y} = 272 \implies \bar{y} = 17$$
  Substitute $\bar{y} = 17$ into (1):
  $$4\bar{x} - 5(17) = -33 \implies 4\bar{x} - 85 = -33 \implies 4\bar{x} = 52 \implies \bar{x} = 13$$
  **Means:** $\mathbf{\bar{x} = 13, \bar{y} = 17}$.

* **Step 2: Identifying Lines & Consistency Verification:**
  * **Hypothesis A:** Let Line 1 be $Y$ on $X$ and Line 2 be $X$ on $Y$.
    * From Line 1: $5y = 4x + 33 \implies y = \frac{4}{5}x + \frac{33}{5} \implies b_{yx} = \frac{4}{5} = 0.8$.
    * From Line 2: $20x = 9y + 107 \implies x = \frac{9}{20}y + \frac{107}{20} \implies b_{xy} = \frac{9}{20} = 0.45$.
    * Check $r^2 = b_{yx} \cdot b_{xy}$:
      $$r^2 = (0.8)(0.45) = 0.36 \le 1 \quad \text{(Mathematically Valid!)}$$
  * Since both $b_{yx}, b_{xy} > 0 \implies r = +\sqrt{0.36} = +0.6$.

* **Step 3: Calculating $\sigma_y$:**
  We know $b_{yx} = r \frac{\sigma_y}{\sigma_x}$.
  Given $\sigma_x^2 = 25 \implies \sigma_x = 5$.
  $$\frac{4}{5} = (0.6) \frac{\sigma_y}{5} \implies 4 = 0.6 \sigma_y \implies \sigma_y = \frac{4}{0.6} = \frac{20}{3} \approx 6.67$$
**Final Answer:** Means = **$(13, 17)$**, $Y$ on $X$ is **$4x - 5y + 33 = 0$**, $r = \mathbf{+0.6}$, $\sigma_y = \mathbf{\frac{20}{3}}$.

---

# PART 4: MCQ RAPID FORMULARY & ELIMINATION CHEATSHEET

---

## 13. BUET MCQ High-Speed Reference Grid

```
+----------------------------------------------------------------------------------------------------+
|                                      HIGH-YIELD BUET MCQ FORMULARY                                  |
+------------------------------------+---------------------------------------------------------------+
| Topic / Concept                    | High-Speed Formula & Mental Checklist                         |
+------------------------------------+---------------------------------------------------------------+
| **Derangements** $D_n$             | $D_1=0, D_2=1, D_3=2, D_4=9, D_5=44, D_6=265$                |
| **Integer Partitions ($\ge 0$)**   | $\binom{n+k-1}{k-1}$                                          |
| **Integer Partitions ($> 0$)**     | $\binom{n-1}{k-1}$                                            |
| **Bell Numbers (Partitions)**      | $B(1)=1, B(2)=2, B(3)=5, B(4)=15, B(5)=52, B(6)=203$          |
| **Gallai's Identity**              | $\alpha(G) + \beta(G) = |V|$                                  |
| **Planar Graph Edge Bound**        | $E \le 3V - 6$ (General), $E \le 2V - 4$ (Bipartite/No Triangles)|
| **Euler Planar Formula**           | $V - E + F = 2$                                               |
| **Max Edges in Bipartite Graph**   | $\lfloor n^2 / 4 \rfloor$                                     |
| **Complete Graph Edges**           | $\binom{n}{2} = \frac{n(n-1)}{2}$                             |
| **Hypercube $Q_d$ Edges**          | $d \cdot 2^{d-1}$                                             |
| **Geometric Distribution**         | $E[X] = 1/p, \quad \text{Var}(X) = (1-p)/p^2$                 |
| **Poisson Distribution**           | $E[X] = \lambda, \quad \text{Var}(X) = \lambda$               |
| **Binomial Distribution**          | $E[X] = np, \quad \text{Var}(X) = np(1-p)$                    |
| **Variance Linear Shift**          | $\text{Var}(aX + b) = a^2 \text{Var}(X)$                      |
| **Correlation Coefficient**        | $r = \text{sign}(b_{yx}) \sqrt{b_{yx} \cdot b_{xy}}$          |
| **Regression Intersection**        | $(\bar{x}, \bar{y})$ is ALWAYS the solution to the 2 lines    |
| **Chebyshev's Bound**              | $P(|X - \mu| \ge k\sigma) \le \frac{1}{k^2}$                  |
| **Markov's Bound**                 | $P(X \ge a) \le \frac{E[X]}{a} \quad (X \ge 0)$               |
+------------------------------------+---------------------------------------------------------------+
```

### ⚡ MCQ Fast Trap Elimination Rules (-0.5 Marks Safety)
1. **The Graph Parity Rule:** If an option implies an odd number of odd-degree vertices or a fractional number of edges, eliminate immediately via the Handshaking Lemma.
2. **The Variance Constant Invariance:** Any option stating $\text{Var}(X + c) = \text{Var}(X) + c$ is immediately false.
3. **The Correlation Bound Constraint:** If your estimated $r^2 = b_{yx} \cdot b_{xy} > 1$, your assignment of regression lines is inverted. Swap the lines immediately.
4. **Bayes' Base Rate Neglect Trap:** In rare medical/defect conditions ($P(D) \ll 1$), high test accuracy does **NOT** equal high posterior probability. Do not pick intuitive high numbers like $98\%$ without calculating the false alarm pool.

# BUET Written & MCQ Master Guide: Discrete & Concrete Mathematics, Probability & Statistics

---

## 🎯 Executive Summary & Examination Strategy

In the **BUET Admission & Recruitment Examinations** (BSc, MSc in CSE/ICT, and Engineering Job Exams), **Discrete Mathematics, Concrete Mathematics, Probability & Statistics** form the foundational mathematical backbone.

### Exam Pattern & Allocation
* **MCQ Phase (50 Marks, 50 Mins, -0.5 Negative Marking):** Fast calculations, definition-based traps, standard distribution formulas, graph identities, and combinatorics shortcuts.
* **Written Phase (100 Marks, 60 Mins, ~6 mins per 10-mark question):** Rigorous multi-step derivations, combinatorial proofs, recurrence relation solving, Bayes' theorem multi-branch trees, expectation linearity proofs, and graph theoretical properties (e.g., Gallai's Identity).

---

# PART 1: DISCRETE & CONCRETE MATHEMATICS

---

## 1. Set Theory, Logic & Boolean Algebra

### 1.1 Fundamental Set Operations & Identities
* **Power Set:** If $|S| = n$, then $|\mathcal{P}(S)| = 2^n$. The number of non-empty subsets is $2^n - 1$.
* **Cartesian Product:** $|A \times B| = |A| \times |B|$.
* **De Morgan's Laws:**
  $$\overline{A \cup B} = \overline{A} \cap \overline{B}, \quad \overline{A \cap B} = \overline{A} \cup \overline{B}$$
* **Symmetric Difference ($\Delta$):**
  $$A \Delta B = (A \setminus B) \cup (B \setminus A) = (A \cup B) \setminus (A \cap B)$$
  * Associative: $A \Delta (B \Delta C) = (A \Delta B) \Delta C$
  * Commutative: $A \Delta B = B \Delta A$
  * Identity: $A \Delta \emptyset = A$, and $A \Delta A = \emptyset$.

### 1.2 Principle of Inclusion-Exclusion (PIE)
* **Two Sets:**
  $$|A \cup B| = |A| + |B| - |A \cap B|$$
* **Three Sets:**
  $$|A \cup B \cup C| = |A| + |B| + |C| - (|A \cap B| + |B \cap C| + |C \cap A|) + |A \cap B \cap C|$$
* **General Form:**
  $$\left|\bigcup_{i=1}^n A_i\right| = \sum_{k=1}^n (-1)^{k-1} \sum_{1 \le i_1 < \dots < i_k \le n} |A_{i_1} \cap \dots \cap A_{i_k}|$$

#### Application: Derangements ($D_n$ or $!n$)
A derangement is a permutation of $n$ elements such that no element appears in its original position.
$$D_n = n! \sum_{k=0}^n \frac{(-1)^k}{k!} = n! \left( 1 - \frac{1}{1!} + \frac{1}{2!} - \frac{1}{3!} + \dots + \frac{(-1)^n}{n!} \right)$$
* **Recurrence:** $D_n = (n-1)(D_{n-1} + D_{n-2})$ with $D_1 = 0, D_2 = 1$.
* **Approximation:** $D_n = \left\lfloor \frac{n!}{e} + \frac{1}{2} \right\rfloor$ for $n \ge 1$.
* **Quick Reference Values:**
  * $D_1 = 0$
  * $D_2 = 1$
  * $D_3 = 2$
  * $D_4 = 9$
  * $D_5 = 44$
  * $D_6 = 265$

### 1.3 Propositional & First-Order Logic
* **Truth Table Shortcuts:**
  * Implication: $p \to q \equiv \neg p \lor q$
  * Contrapositive: $p \to q \equiv \neg q \to \neg p$ (Logically Equivalent)
  * Converse: $q \to p$ (NOT equivalent to $p \to q$)
  * Inverse: $\neg p \to \neg q$ (NOT equivalent to $p \to q$)
  * Bi-conditional: $p \leftrightarrow q \equiv (p \to q) \land (q \to p) \equiv (p \land q) \lor (\neg p \land \neg q)$
* **Tautology vs Contradiction vs Contingency:**
  * **Tautology:** Statement is TRUE for all truth assignments (e.g., $p \lor \neg p$).
  * **Contradiction (Absurdity):** Statement is FALSE for all truth assignments (e.g., $p \land \neg p$).
  * **Contingency:** Statement is neither a tautology nor a contradiction.
* **Quantifiers & Negation:**
  $$\neg (\forall x P(x)) \equiv \exists x \neg P(x)$$
  $$\neg (\exists x P(x)) \equiv \forall x \neg P(x)$$

---

## 2. Relations, Functions, Posets & Pigeonhole Principle

### 2.1 Properties of Binary Relations on a Set $A$ ($|A| = n$)
A relation $R \subseteq A \times A$ (Total possible relations = $2^{n^2}$):

| Property | Formal Definition | Total Number on Set of Size $n$ |
| :--- | :--- | :--- |
| **Reflexive** | $\forall x \in A, (x, x) \in R$ | $2^{n(n-1)} = 2^{n^2 - n}$ |
| **Irreflexive** | $\forall x \in A, (x, x) \notin R$ | $2^{n(n-1)} = 2^{n^2 - n}$ |
| **Symmetric** | $\forall x, y, (x, y) \in R \implies (y, x) \in R$ | $2^{n(n+1)/2}$ |
| **Antisymmetric** | $\forall x, y, ((x, y) \in R \land (y, x) \in R) \implies x = y$ | $2^n \cdot 3^{n(n-1)/2}$ |
| **Asymmetric** | $\forall x, y, (x, y) \in R \implies (y, x) \notin R$ | $3^{n(n-1)/2}$ |
| **Equivalence Relation** | Reflexive + Symmetric + Transitive | Bell Number $B(n)$ |
| **Partial Order (Poset)** | Reflexive + Antisymmetric + Transitive | No closed form (Hasse diagram representation) |

### 2.2 Equivalence Relations & Partitions
* An equivalence relation partitions $A$ into disjoint non-empty equivalence classes $[a] = \{x \in A \mid (x, a) \in R\}$.
* **Bell Numbers $B(n)$:**
  * $B(0) = 1, B(1) = 1, B(2) = 2, B(3) = 5, B(4) = 15, B(5) = 52$.
  * Recurrence: $B(n+1) = \sum_{k=0}^n \binom{n}{k} B(k)$.

### 2.3 Posets, Hasse Diagrams & Lattices
* **Poset $(S, \le)$:** A set with a Reflexive, Antisymmetric, Transitive relation.
* **Hasse Diagram:** Directed graph without self-loops (reflexivity implied) and without transitive edges (transitivity implied), drawn bottom-to-top.
* **Maximal vs Maximum:**
  * *Maximal Element:* No element is strictly greater than it (can be multiple).
  * *Maximum (Greatest) Element:* Greater than or equal to every other element (unique if exists).
  * *Minimal vs Minimum:* Analogous.
* **Lattice:** A poset in which every pair of elements has a unique Least Upper Bound ($\text{LUB}$ or Join $\lor$) and a unique Greatest Lower Bound ($\text{GLB}$ or Meet $\land$).

### 2.4 Functions: Injective, Surjective & Bijective
For $f: A \to B$ where $|A| = m, |B| = n$:
* **Total Functions:** $n^m$
* **Injective (One-to-One):** Each element in $A$ maps to a distinct element in $B$.
  $$\text{Count} = \begin{cases} P(n, m) = \frac{n!}{(n-m)!} & \text{if } m \le n \\ 0 & \text{if } m > n \end{cases}$$
* **Surjective (Onto):** Every element in $B$ has at least one pre-image in $A$.
  $$\text{Count} = \sum_{k=0}^n (-1)^{n-k} \binom{n}{k} k^m = n! \cdot S(m, n) \quad (\text{where } S(m,n) \text{ is Stirling number of 2nd kind})$$
  *(Note: Count = 0 if $m < n$)*
* **Bijective (One-to-One & Onto):** Exists if and only if $m = n$. Total $= n!$.

### 2.5 Pigeonhole Principle (PHP)
* **Basic PHP:** If $n+1$ or more pigeons are placed in $n$ pigeonholes, at least one pigeonhole contains 2 or more pigeons.
* **Generalized PHP:** If $N$ objects are placed into $k$ boxes, then at least one box contains at least:
  $$\left\lceil \frac{N}{k} \right\rceil \text{ objects}$$
  and at least one box contains at most:
  $$\left\lfloor \frac{N}{k} \right\rfloor \text{ objects}$$

---

## 3. Combinatorics & Counting Principles

### 3.1 Fundamental Counting Rules
* **Multiplication Rule (AND):** If task 1 can be done in $n_1$ ways and task 2 in $n_2$ ways independently $\implies n_1 \times n_2$ ways.
* **Addition Rule (OR - Disjoint):** If events are mutually exclusive $\implies n_1 + n_2$ ways.

### 3.2 Permutations & Combinations Summary Matrix

| Scenario | Order Matters? (Permutation) | Order Doesn't Matter? (Combination) |
| :--- | :--- | :--- |
| **Without Repetition** | $P(n, r) = \frac{n!}{(n-r)!}$ | $\binom{n}{r} = \frac{n!}{r!(n-r)!}$ |
| **With Repetition** | $n^r$ | $\binom{n+r-1}{r} = \binom{n+r-1}{n-1}$ (Stars & Bars) |
| **Identical Items** | $\frac{n!}{n_1! n_2! \dots n_k!}$ | Multinomial coefficients |
| **Circular Permutation** | $(n-1)!$ (Clockwise $\ne$ Counter-clockwise)<br>$\frac{(n-1)!}{2}$ (Necklace / Key ring) | N/A |

### 3.3 Stars and Bars (Integer Solutions to Equations)
Number of integer solutions to $x_1 + x_2 + \dots + x_k = n$:
1. **Non-negative integers ($x_i \ge 0$):**
   $$\binom{n + k - 1}{k - 1} = \binom{n + k - 1}{n}$$
2. **Positive integers ($x_i \ge 1$ or $x_i > 0$):**
   $$\binom{n - 1}{k - 1}$$
3. **General Lower Bounds ($x_i \ge c_i$):**
   Substitute $y_i = x_i - c_i \ge 0$. The equation becomes $\sum y_i = n - \sum c_i = n'$. The number of solutions is $\binom{n' + k - 1}{k - 1}$.

### 3.4 Binomial Identities (MCQ Quick-Fire)
* $\sum_{k=0}^n \binom{n}{k} = 2^n$
* $\sum_{k=0}^n (-1)^k \binom{n}{k} = 0$
* $\sum_{k=0}^n k \binom{n}{k} = n 2^{n-1}$
* $\sum_{k=0}^n k^2 \binom{n}{k} = n(n+1) 2^{n-2}$
* **Pascal's Identity:** $\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$
* **Vandermonde's Identity:** $\sum_{k=0}^r \binom{m}{k}\binom{n}{r-k} = \binom{m+n}{r}$
* **Hockey-Stick Identity:** $\sum_{i=r}^n \binom{i}{r} = \binom{n+1}{r+1}$

---

## 4. Concrete Mathematics: Recurrences, Sums & Generating Functions

### 4.1 Closed-Form Summations
* **Arithmetic Series:** $\sum_{k=1}^n k = \frac{n(n+1)}{2}$
* **Sum of Squares:** $\sum_{k=1}^n k^2 = \frac{n(n+1)(2n+1)}{6}$
* **Sum of Cubes:** $\sum_{k=1}^n k^3 = \left[ \frac{n(n+1)}{2} \right]^2$
* **Geometric Series:** $\sum_{k=0}^{n-1} a r^k = a \frac{1 - r^n}{1 - r} \quad (r \ne 1)$; for $|r| < 1$, $\sum_{k=0}^\infty a r^k = \frac{a}{1 - r}$.
* **Harmonic Numbers:** $H_n = \sum_{k=1}^n \frac{1}{k} \approx \ln n + \gamma + O(1/n)$, where $\gamma \approx 0.577215$.

### 4.2 Solving Linear Recurrence Relations

#### Case A: Linear Homogeneous with Constant Coefficients
$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \dots + c_k a_{n-k}$$
1. Write the **Characteristic Equation:**
   $$r^k - c_1 r^{k-1} - c_2 r^{k-2} - \dots - c_k = 0$$
2. Find roots $r_1, r_2, \dots, r_k$:
   * **Distinct Real Roots:** $a_n = \alpha_1 r_1^n + \alpha_2 r_2^n + \dots + \alpha_k r_k^n$
   * **Repeated Root $r_1$ of multiplicity $m$:**
     $$a_n = (\alpha_1 + \alpha_2 n + \alpha_3 n^2 + \dots + \alpha_m n^{m-1}) r_1^n$$
   * **Complex Conjugate Roots $r = \rho e^{\pm i\theta}$:**
     $$a_n = \rho^n (\alpha_1 \cos(n\theta) + \alpha_2 \sin(n\theta))$$
3. Use initial conditions to solve for constants $\alpha_i$.

#### Case B: Inhomogeneous Recurrences
$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + F(n)$$
* **General Solution:** $a_n = a_n^{(h)} + a_n^{(p)}$ (Homogeneous solution + Particular solution).
* **Trial Particular Solutions $a_n^{(p)}$:**
  * If $F(n) = c \cdot s^n$: Try $a_n^{(p)} = A s^n$ (if $s$ is a characteristic root of multiplicity $m$, try $A n^m s^n$).
  * If $F(n) = \text{Polynomial of degree } d$: Try general degree-$d$ polynomial $A_d n^d + \dots + A_0$.

### 4.3 Master Theorem for Divide-and-Conquer Recurrences
For $T(n) = a T(n/b) + f(n)$ where $a \ge 1, b > 1$, compare $f(n)$ with $n^{\log_b a}$:

1. **Case 1:** If $f(n) = O(n^{\log_b a - \epsilon})$ for some $\epsilon > 0 \implies T(n) = \Theta(n^{\log_b a})$
2. **Case 2:** If $f(n) = \Theta(n^{\log_b a} \log^k n)$ for $k \ge 0 \implies T(n) = \Theta(n^{\log_b a} \log^{k+1} n)$
3. **Case 3:** If $f(n) = \Omega(n^{\log_b a + \epsilon})$ for some $\epsilon > 0$ AND regularity condition holds ($a f(n/b) \le c f(n)$ for some $c < 1$) $\implies T(n) = \Theta(f(n))$

### 4.4 Ordinary Generating Functions (OGF)
The OGF of a sequence $\langle a_0, a_1, a_2, \dots \rangle$ is $G(x) = \sum_{n=0}^\infty a_n x^n$.
* $\frac{1}{1-x} = \sum_{n=0}^\infty x^n \implies \langle 1, 1, 1, \dots \rangle$
* $\frac{1}{1-ax} = \sum_{n=0}^\infty a^n x^n \implies \langle 1, a, a^2, a^3, \dots \rangle$
* $\frac{1}{(1-x)^2} = \sum_{n=0}^\infty (n+1) x^n \implies \langle 1, 2, 3, 4, \dots \rangle$
* $\frac{1}{(1-x)^k} = \sum_{n=0}^\infty \binom{n+k-1}{k-1} x^n$

---

## 5. Graph Theory & Network Invariants

```
               [Graph Invariant Relationships]
                     |V| = n Vertices
                     /              \
                    /                \
        Independent Set (α)    Vertex Cover (β)
          [No two adjacent]     [Covers every edge]
                    \                /
                     \              /
                 Gallai's Identity: α + β = |V|
```

### 5.1 Graph Fundamentals & Handshaking Lemma
* **Handshaking Lemma:**
  $$\sum_{v \in V} \deg(v) = 2|E|$$
* **Corollary:** In any undirected graph, the number of vertices with **odd degree is always EVEN**.
* **Maximum edges in a simple graph with $n$ vertices:** $\binom{n}{2} = \frac{n(n-1)}{2}$.
* **Maximum edges in a bipartite graph with $n$ vertices:** $\lfloor n^2/4 \rfloor$ (partitioned as $\lfloor n/2 \rfloor$ and $\lceil n/2 \rceil$).

### 5.2 Special Graph Classes
* **Complete Graph ($K_n$):** $|E| = \binom{n}{2}$, regular of degree $n-1$.
* **Complete Bipartite ($K_{m,n}$):** $|V| = m+n$, $|E| = mn$.
* **Cycle Graph ($C_n$):** $|V| = n, |E| = n$, 2-regular.
* **Hypercube ($Q_d$):** $|V| = 2^d, |E| = d \cdot 2^{d-1}$, $d$-regular, bipartite.

### 5.3 Planar Graphs & Euler's Formula
A planar graph can be embedded in the plane without intersecting edges.
* **Euler's Planar Formula:** For any connected planar graph with $V$ vertices, $E$ edges, and $F$ faces (regions):
  $$V - E + F = 2$$
* **Planar Edge Bounds:**
  * For any simple connected planar graph with $V \ge 3$:
    $$E \le 3V - 6$$
  * If the graph is **triangle-free** (e.g., bipartite planar):
    $$E \le 2V - 4$$
* **Kuratowski's Theorem:** A graph is planar if and only if it does not contain a subgraph homeomorphic to (or a minor of) $K_5$ or $K_{3,3}$.

### 5.4 Graph Coloring & Chromatic Number $\chi(G)$
* **Chromatic Number $\chi(G)$:** Minimum colors needed to color vertices so no two adjacent vertices share a color.
  * $G$ is Bipartite $\iff \chi(G) \le 2$ (and has no odd cycles).
  * Trees: $\chi(T) = 2$ ($V \ge 2$).
  * Even Cycle $C_{2k}$: $\chi = 2$; Odd Cycle $C_{2k+1}$: $\chi = 3$.
  * Complete Graph $K_n$: $\chi(K_n) = n$.
  * **Four Color Theorem:** Every planar graph has $\chi(G) \le 4$.

### 5.5 Eulerian vs Hamiltonian Graphs

| Feature | Eulerian Graph | Hamiltonian Graph |
| :--- | :--- | :--- |
| **Traverses** | Every **Edge** exactly once | Every **Vertex** exactly once |
| **Necessary & Sufficient Condition** | Connected + **All vertices have EVEN degree** (Circuit)<br>Connected + **Exactly 0 or 2 odd vertices** (Path) | NP-Complete in general (No single simple $\iff$ test) |
| **Sufficient Theorems** | Euler's Theorem | **Dirac's Theorem:** If $n \ge 3$ and $\deg(v) \ge n/2$ for all $v \implies$ Hamiltonian.<br>**Ore's Theorem:** If $n \ge 3$ and $\deg(u) + \deg(v) \ge n$ for all non-adjacent pairs $(u, v) \implies$ Hamiltonian. |

### 5.6 Independent Sets, Vertex Covers & Gallai's Identities
* **Independent Set ($S \subseteq V$):** No two vertices in $S$ are adjacent. Maximum Independent Set size is $\alpha(G)$.
* **Vertex Cover ($C \subseteq V$):** Every edge in $E$ has at least one endpoint in $C$. Minimum Vertex Cover size is $\beta(G)$.
* **Edge Cover ($C' \subseteq E$):** Every vertex in $V$ is incident to at least one edge in $C'$. Minimum Edge Cover size is $\beta'(G)$.
* **Matching ($M \subseteq E$):** No two edges in $M$ share a common vertex. Maximum Matching size is $\alpha'(G)$.

> [!IMPORTANT]
> **Gallai's Identities (High Frequency BUET Written / MCQ):**
> 1. **Vertex Duality:** For any undirected graph $G = (V, E)$ without isolated vertices:
>    $$\alpha(G) + \beta(G) = |V|$$
>    *Moreover, $S$ is an Independent Set $\iff V \setminus S$ is a Vertex Cover.*
> 2. **Edge Duality:** For any graph $G = (V, E)$ without isolated vertices:
>    $$\alpha'(G) + \beta'(G) = |V|$$

---

## 6. Number Theory & Modular Arithmetic Essentials

### 6.1 Divisibility & Extended Euclidean Algorithm
* **Bézout's Identity:** For non-zero integers $a, b$, there exist integers $x, y$ such that:
  $$a x + b y = \gcd(a, b)$$
* **Modular Inverse:** $a x \equiv 1 \pmod m$ has a unique solution modulo $m$ if and only if $\gcd(a, m) = 1$.

### 6.2 Fermat's Little Theorem & Euler's Totient
* **Fermat's Little Theorem:** If $p$ is a prime and $\gcd(a, p) = 1$:
  $$a^{p-1} \equiv 1 \pmod p \implies a^p \equiv a \pmod p$$
* **Euler's Totient Function $\phi(n)$:** Number of integers $k \in [1, n]$ such that $\gcd(k, n) = 1$.
  $$\phi(n) = n \prod_{p \mid n} \left(1 - \frac{1}{p}\right) = p_1^{e_1 - 1}(p_1 - 1) \cdot p_2^{e_2 - 1}(p_2 - 1) \dots$$
* **Euler's Totient Theorem:** If $\gcd(a, n) = 1$:
  $$a^{\phi(n)} \equiv 1 \pmod n$$

---

# PART 2: PROBABILITY & RANDOM PROCESSES

---

## 7. Axiomatic Probability & Bayes' Theorem

### 7.1 Probability Axioms & Conditional Probability
* **Axioms of Probability:**
  1. $0 \le P(A) \le 1$ for any event $A$.
  2. $P(S) = 1$ ($S$ is the sample space).
  3. For mutually exclusive events $A_1, A_2, \dots$: $P\left(\bigcup_{i=1}^\infty A_i\right) = \sum_{i=1}^\infty P(A_i)$.
* **Inclusion-Exclusion for Probability:**
  $$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$
* **Conditional Probability:**
  $$P(A \mid B) = \frac{P(A \cap B)}{P(B)} \quad (P(B) > 0)$$
* **Independence:** $A$ and $B$ are independent $\iff P(A \cap B) = P(A) P(B) \iff P(A \mid B) = P(A)$.

### 7.2 Law of Total Probability & Bayes' Theorem
Let $B_1, B_2, \dots, B_n$ form a partition of sample space $S$ (i.e., $\bigcup B_i = S$ and $B_i \cap B_j = \emptyset$ for $i \ne j$ with $P(B_i) > 0$):

```
                      [Sample Space S Partitioned]
           +---------------+---------------+---------------+
           |      B1       |      B2       |      B3       |
           |     ....(     |      )...     |               |
           |        . (    |     ) .       |    Event A    |
           |         .(--- A ---).         |               |
           +---------------+---------------+---------------+
           P(A) = P(A|B1)P(B1) + P(A|B2)P(B2) + P(A|B3)P(B3)
```

* **Law of Total Probability:**
  $$P(A) = \sum_{i=1}^n P(A \mid B_i) P(B_i)$$
* **Bayes' Theorem (Posterior Probability):**
  $$P(B_k \mid A) = \frac{P(A \mid B_k) P(B_k)}{P(A)} = \frac{P(A \mid B_k) P(B_k)}{\sum_{i=1}^n P(A \mid B_i) P(B_i)}$$

---

## 8. Random Variables, Expectation & Variance

### 8.1 Probability Distributions & Expectations

| Metric | Discrete Random Variable $X$ | Continuous Random Variable $X$ |
| :--- | :--- | :--- |
| **Probability Function** | $\text{PMF: } p(x) = P(X = x)$ | $\text{PDF: } f(x) \ge 0$ |
| **Normalization** | $\sum_x p(x) = 1$ | $\int_{-\infty}^\infty f(x) dx = 1$ |
| **Cumulative Dist. (CDF)** | $F(x) = P(X \le x) = \sum_{t \le x} p(t)$ | $F(x) = P(X \le x) = \int_{-\infty}^x f(t) dt$ |
| **Relation (PDF / CDF)** | $p(x_k) = F(x_k) - F(x_{k-1})$ | $f(x) = \frac{d}{dx} F(x)$ |
| **Expected Value $E[X]$** | $\mu = \sum_x x \, p(x)$ | $\mu = \int_{-\infty}^\infty x \, f(x) dx$ |
| **Expectation of $g(X)$** | $E[g(X)] = \sum_x g(x) \, p(x)$ | $E[g(X)] = \int_{-\infty}^\infty g(x) \, f(x) dx$ |

### 8.2 Linearity of Expectation & Variance Rules

> [!IMPORTANT]
> **Linearity of Expectation (Universal Rule):**
> $$E[aX + bY + c] = aE[X] + bE[Y] + c$$
> **This holds ALWAYS, whether $X$ and $Y$ are independent or not!**

* **Variance Definition:**
  $$\text{Var}(X) = \sigma^2 = E\left[(X - \mu)^2\right] = E[X^2] - (E[X])^2$$
* **Variance Properties:**
  * $\text{Var}(c) = 0$ for constant $c$.
  * $\text{Var}(aX + b) = a^2 \text{Var}(X)$ (Additive constants do not alter spread).
  * $\text{Var}(X \pm Y) = \text{Var}(X) + \text{Var}(Y) \pm 2\text{Cov}(X, Y)$.
  * **If $X$ and $Y$ are independent:** $\text{Cov}(X, Y) = 0 \implies \text{Var}(X \pm Y) = \text{Var}(X) + \text{Var}(Y)$.
* **Covariance:**
  $$\text{Cov}(X, Y) = E[(X - \mu_X)(Y - \mu_Y)] = E[XY] - E[X]E[Y]$$

---

## 9. Standard Probability Distributions

### 9.1 Summary Table of Essential Distributions

| Distribution | Notation | PMF / PDF | Mean ($E[X]$) | Variance ($\text{Var}(X)$) | Key Application / Memoryless? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Bernoulli** | $\text{Bern}(p)$ | $P(X=1)=p, P(X=0)=1-p$ | $p$ | $p(1-p)$ | Single trial success/fail |
| **Binomial** | $B(n, p)$ | $\binom{n}{k} p^k (1-p)^{n-k}$ | $np$ | $np(1-p)$ | $k$ successes in $n$ independent trials |
| **Geometric** | $\text{Geom}(p)$ | $(1-p)^{k-1}p \quad (k=1, 2, \dots)$ | $\frac{1}{p}$ | $\frac{1-p}{p^2}$ | Trials until 1st success (**Memoryless**) |
| **Poisson** | $\text{Pois}(\lambda)$ | $\frac{\lambda^k e^{-\lambda}}{k!} \quad (k=0, 1, 2, \dots)$ | $\lambda$ | $\lambda$ | Rare events, network packet arrival |
| **Uniform** | $U(a, b)$ | $f(x) = \frac{1}{b-a}, x \in [a, b]$ | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ | Completely random continuous interval |
| **Exponential** | $\text{Exp}(\lambda)$ | $f(x) = \lambda e^{-\lambda x}, x \ge 0$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ | Waiting times, inter-arrival (**Memoryless**) |
| **Normal (Gaussian)** | $N(\mu, \sigma^2)$ | $\frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$ | $\mu$ | $\sigma^2$ | Central Limit Theorem, noise modeling |

### 9.2 Critical Properties & Approximations
1. **Poisson Approximation to Binomial:** When $n$ is large ($n \ge 20$) and $p$ is small ($p \le 0.05$), $B(n, p) \approx \text{Pois}(\lambda = np)$.
2. **Memoryless Property:**
   * Discrete: $P(X > m + n \mid X > m) = P(X > n)$ (Only **Geometric Distribution**).
   * Continuous: $P(X > s + t \mid X > s) = P(X > t)$ (Only **Exponential Distribution**).
3. **Standard Normal Transformation:**
   $$Z = \frac{X - \mu}{\sigma} \sim N(0, 1)$$
   * Empirical 68-95-99.7 Rule:
     * $P(\mu - \sigma \le X \le \mu + \sigma) \approx 68.27\%$
     * $P(\mu - 2\sigma \le X \le \mu + 2\sigma) \approx 95.45\%$
     * $P(\mu - 3\sigma \le X \le \mu + 3\sigma) \approx 99.73\%$

---

## 10. Statistics Fundamentals & Bounds

### 10.1 Measures of Central Tendency & Dispersion
* **Means Relationship:**
  $$\text{Arithmetic Mean (AM)} \ge \text{Geometric Mean (GM)} \ge \text{Harmonic Mean (HM)}$$
  $$\text{Equality holds } \iff x_1 = x_2 = \dots = x_n$$
  For two positive numbers $a, b$: $GM^2 = AM \times HM$.
* **Empirical Mode Relationship (for moderately skewed data):**
  $$\text{Mode} \approx 3 \times \text{Median} - 2 \times \text{Mean}$$
* **Coefficient of Variation ($CV$):**
  $$CV = \frac{\sigma}{\mu} \times 100\%$$
  *(Used to compare consistency; lower $CV \implies$ more consistent/less dispersed).*

### 10.2 Correlation & Linear Regression
* **Pearson Correlation Coefficient ($r$):**
  $$r = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y} = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2 \sum (y_i - \bar{y})^2}}, \quad -1 \le r \le +1$$
* **Regression Lines:**
  * Regression of $Y$ on $X$: $(y - \bar{y}) = b_{yx} (x - \bar{x})$ where $b_{yx} = r \frac{\sigma_y}{\sigma_x}$
  * Regression of $X$ on $Y$: $(x - \bar{x}) = b_{xy} (y - \bar{y})$ where $b_{xy} = r \frac{\sigma_x}{\sigma_y}$
* **Key Properties of Regression Coefficients:**
  1. $r = \pm \sqrt{b_{yx} \cdot b_{xy}}$ (Sign of $r$ is identical to signs of $b_{yx}$ and $b_{xy}$).
  2. If one regression coefficient is $> 1$, the other MUST be $< 1$.
  3. Arithmetic mean of regression coefficients $\ge |r|$: $\frac{b_{yx} + b_{xy}}{2} \ge |r|$.
  4. The two regression lines always intersect at the mean point $(\bar{x}, \bar{y})$.

### 10.3 Probability Inequalities & Bounds
* **Markov's Inequality:** If $X$ is a non-negative random variable and $a > 0$:
  $$P(X \ge a) \le \frac{E[X]}{a}$$
* **Chebyshev's Inequality:** For any random variable $X$ with mean $\mu$ and variance $\sigma^2$, and $k > 0$:
  $$P(|X - \mu| \ge k\sigma) \le \frac{1}{k^2} \quad \iff \quad P(|X - \mu| < k\sigma) \ge 1 - \frac{1}{k^2}$$

---

# PART 3: BUET ARCHIVE & HIGH-YIELD WORKED PROBLEMS

---

## 11. Discrete Mathematics: Written & MCQ Archives

### 💡 Problem 11.1: Maximum Independent Set & Minimum Vertex Cover [BUET MSc CSE 21]
**Problem:** Let $G = (V, E)$ be a graph where $V = \{a, b, c, d, e, f, g, h, i, j, k\}$. If $S = \{a, c, g, h, k\}$ is a maximum independent set in $G$, then what is the size and vertex set of a minimum vertex cover?

**Step-by-Step Solution:**
1. Count the total vertices: $|V| = 11$.
2. Size of Maximum Independent Set: $\alpha(G) = |S| = |\{a, c, g, h, k\}| = 5$.
3. Apply **Gallai's Identity**:
   $$\alpha(G) + \beta(G) = |V| \implies \beta(G) = |V| - \alpha(G) = 11 - 5 = 6$$
4. A set $C$ is a vertex cover if and only if $V \setminus C$ is an independent set. Thus, the complement of the maximum independent set is the minimum vertex cover:
   $$V' = V \setminus S = \{b, d, e, f, i, j\}$$
**Final Answer:** Size = **6**, Vertex Set = **$\{b, d, e, f, i, j\}$**.

---

### 💡 Problem 11.2: Planar Graph Edge Count
**Problem:** A connected planar graph has 20 vertices, each of degree 3. How many faces (regions) does the graph divide the plane into?

**Step-by-Step Solution:**
1. Handshaking Lemma: $\sum \deg(v) = 2|E| \implies 20 \times 3 = 2|E| \implies |E| = 30$.
2. Apply **Euler's Planar Formula**:
   $$V - E + F = 2 \implies 20 - 30 + F = 2 \implies F = 12$$
**Final Answer:** **12 faces**.

---

### 💡 Problem 11.3: Stars and Bars with Constraints
**Problem:** Find the number of non-negative integer solutions to $x_1 + x_2 + x_3 + x_4 = 20$ such that $x_1 \ge 2, x_2 \ge 3, x_3 \ge 0, x_4 \ge 1$.

**Step-by-Step Solution:**
1. Define new variables:
   * $y_1 = x_1 - 2 \ge 0$
   * $y_2 = x_2 - 3 \ge 0$
   * $y_3 = x_3 \ge 0$
   * $y_4 = x_4 - 1 \ge 0$
2. Substitute into equation:
   $$(y_1 + 2) + (y_2 + 3) + y_3 + (y_4 + 1) = 20 \implies y_1 + y_2 + y_3 + y_4 = 20 - 6 = 14$$
3. Stars and Bars Formula for non-negative solutions ($n=14, k=4$):
   $$\binom{n + k - 1}{k - 1} = \binom{14 + 4 - 1}{4 - 1} = \binom{17}{3} = \frac{17 \times 16 \times 15}{3 \times 2 \times 1} = 680$$
**Final Answer:** **680 solutions**.

---

### 💡 Problem 11.4: Linear Recurrence with Characteristic Equation
**Problem:** Solve the recurrence relation $a_n = 5a_{n-1} - 6a_{n-2}$ with initial conditions $a_0 = 1, a_1 = 4$.

**Step-by-Step Solution:**
1. Characteristic equation: $r^2 - 5r + 6 = 0 \implies (r - 2)(r - 3) = 0 \implies r_1 = 2, r_2 = 3$.
2. General solution: $a_n = \alpha_1 2^n + \alpha_2 3^n$.
3. Substitute initial conditions:
   * $n = 0 \implies \alpha_1 + \alpha_2 = 1$
   * $n = 1 \implies 2\alpha_1 + 3\alpha_2 = 4$
4. Solving the system:
   $$2(1 - \alpha_2) + 3\alpha_2 = 4 \implies 2 + \alpha_2 = 4 \implies \alpha_2 = 2$$
   $$\alpha_1 = 1 - 2 = -1$$
5. Particular closed form: $a_n = -1 \cdot 2^n + 2 \cdot 3^n = 2 \cdot 3^n - 2^n$.
**Final Answer:** $a_n = 2(3^n) - 2^n$.

---

## 12. Probability & Statistics: Written & MCQ Archives

### 💡 Problem 12.1: Multi-Stage Bayes' Theorem (Medical Diagnosis / False Positive Paradox)
**Problem:** A rare disease affects $0.1\%$ of the population. A diagnostic test has a $99\%$ sensitivity (true positive rate) and a $98\%$ specificity (true negative rate). If a randomly selected person tests positive, what is the probability that the person actually has the disease?

**Step-by-Step Solution:**
1. Define Events:
   * $D$: Person has the disease $\implies P(D) = 0.001$, so $P(\bar{D}) = 0.999$.
   * $T^+$: Test result is positive.
2. Given conditional probabilities:
   * Sensitivity: $P(T^+ \mid D) = 0.99$
   * Specificity: $P(T^- \mid \bar{D}) = 0.98 \implies \text{False Positive Rate: } P(T^+ \mid \bar{D}) = 1 - 0.98 = 0.02$
3. Law of Total Probability for $P(T^+)$:
   $$P(T^+) = P(T^+ \mid D)P(D) + P(T^+ \mid \bar{D})P(\bar{D})$$
   $$P(T^+) = (0.99)(0.001) + (0.02)(0.999) = 0.00099 + 0.01998 = 0.02097$$
4. Apply Bayes' Theorem:
   $$P(D \mid T^+) = \frac{P(T^+ \mid D)P(D)}{P(T^+)} = \frac{0.00099}{0.02097} = \frac{99}{2097} \approx 0.04721 \text{ (or } 4.72\%)$$
**Final Answer:** **$4.72\%$** (Even with a 99% accurate test, because the disease is rare, ~95.3% of positive tests are false positives!).

---

### 💡 Problem 12.2: Linearity of Expectation (Coupon Collector / Streaks)
**Problem:** An unbiased 6-sided die is rolled repeatedly. What is the expected number of rolls needed to see all 6 distinct faces at least once?

**Step-by-Step Solution:**
1. Let $X$ be the total number of rolls to see all 6 faces.
2. Decompose $X = X_1 + X_2 + X_3 + X_4 + X_5 + X_6$, where $X_i$ is the number of additional rolls needed to get the $i$-th new face after $(i-1)$ distinct faces have been seen.
3. Each $X_i$ is a **Geometric random variable** with success probability $p_i$:
   * $p_1 = \frac{6}{6} = 1 \implies E[X_1] = \frac{1}{p_1} = \frac{6}{6}$
   * $p_2 = \frac{5}{6} \implies E[X_2] = \frac{6}{5}$
   * $p_3 = \frac{4}{6} \implies E[X_3] = \frac{6}{4}$
   * $p_4 = \frac{3}{6} \implies E[X_4] = \frac{6}{3}$
   * $p_5 = \frac{2}{6} \implies E[X_5] = \frac{6}{2}$
   * $p_6 = \frac{1}{6} \implies E[X_6] = \frac{6}{1}$
4. By **Linearity of Expectation**:
   $$E[X] = \sum_{i=1}^6 E[X_i] = 6 \left(\frac{1}{6} + \frac{1}{5} + \frac{1}{4} + \frac{1}{3} + \frac{1}{2} + 1\right) = 6 \times H_6$$
   $$E[X] = 6 \left(1 + 0.5 + 0.3333 + 0.25 + 0.2 + 0.1667\right) = 6 \times \frac{147}{60} = 14.7$$
**Final Answer:** **$14.7$ rolls**.

---

### 💡 Problem 12.3: Geometric Continuous Probability (The Meeting Problem)
**Problem:** Two friends, Alice and Bob, agree to meet at a café between 4:00 PM and 5:00 PM. Each person arrives uniformly at random during the hour and waits for at most 15 minutes before leaving. What is the probability that they meet?

```
    Bob (y)
    60 +---------------------------+
       | . . . . . . . . . . / / / |  Meeting condition:
       | . . . . . . . . . / / / / |  |x - y| <= 15
       | . . . . . . . . / / / / / |
       | . . . . . . . / / / / / . |  Shaded Band = Meet
       | . . . . . . / / / / / . . |  Two Triangles = No Meet
       | . . . . . / / / / / . . . |
       | . . . . / / / / / . . . . |
       | . . . / / / / / . . . . . |
       | . . / / / / / . . . . . . |
       | . / / / / / . . . . . . . |
     0 +---------------------------+ 60
       0                        Alice (x)
```

**Step-by-Step Solution:**
1. Let Alice arrive at $x$ minutes past 4:00 PM ($0 \le x \le 60$).
2. Let Bob arrive at $y$ minutes past 4:00 PM ($0 \le y \le 60$).
3. Total sample space area in $[0, 60] \times [0, 60] = 60 \times 60 = 3600$.
4. They meet if and only if $|x - y| \le 15$.
5. Compute the complement (They do NOT meet): $|x - y| > 15 \implies y - x > 15$ or $x - y > 15$.
   * Region 1 ($y > x + 15$): Right triangle with legs of length $60 - 15 = 45$. Area $= \frac{1}{2} \times 45 \times 45 = 1012.5$.
   * Region 2 ($x > y + 15$): Right triangle with legs of length $45$. Area $= \frac{1}{2} \times 45 \times 45 = 1012.5$.
   * Total Area of Non-Meeting $= 1012.5 + 1012.5 = 2025 = 45^2$.
6. Probability they meet:
   $$P(\text{Meet}) = 1 - \frac{\text{Area(No Meet)}}{\text{Total Area}} = 1 - \frac{45^2}{60^2} = 1 - \left(\frac{3}{4}\right)^2 = 1 - \frac{9}{16} = \frac{7}{16} \approx 0.4375$$
**Final Answer:** $\mathbf{\frac{7}{16}}$ **or $43.75\%$**.

---

### 💡 Problem 12.4: Regression Lines & Correlation Coefficients
**Problem:** The equations of two regression lines are $2x + 3y - 8 = 0$ and $x + 2y - 5 = 0$.
1. Find the means $\bar{x}$ and $\bar{y}$.
2. Determine which line is the regression of $Y$ on $X$, and find the correlation coefficient $r$.

**Step-by-Step Solution:**
1. **Find the means $(\bar{x}, \bar{y})$:**
   Since both regression lines pass through $(\bar{x}, \bar{y})$:
   $$\begin{cases} 2\bar{x} + 3\bar{y} = 8 \quad \dots (1) \\ \bar{x} + 2\bar{y} = 5 \quad \dots (2) \end{cases}$$
   From (2), $\bar{x} = 5 - 2\bar{y}$. Substitute into (1):
   $$2(5 - 2\bar{y}) + 3\bar{y} = 8 \implies 10 - \bar{y} = 8 \implies \bar{y} = 2$$
   $$\bar{x} = 5 - 2(2) = 1$$
   **Means:** $\mathbf{\bar{x} = 1, \bar{y} = 2}$.

2. **Identify regression lines & find $r$:**
   * **Assumption 1:** Let $2x + 3y - 8 = 0$ be $Y$ on $X \implies 3y = -2x + 8 \implies y = -\frac{2}{3}x + \frac{8}{3} \implies b_{yx} = -\frac{2}{3}$.
     Then $x + 2y - 5 = 0$ is $X$ on $Y \implies x = -2y + 5 \implies b_{xy} = -2$.
     Check condition: $r^2 = b_{yx} \cdot b_{xy} = \left(-\frac{2}{3}\right)(-2) = \frac{4}{3} > 1$.
     *(Invalid! Since $r^2 \le 1$, this assumption is wrong!)*
   * **Assumption 2:** Let $x + 2y - 5 = 0$ be $Y$ on $X \implies 2y = -x + 5 \implies y = -\frac{1}{2}x + \frac{5}{2} \implies b_{yx} = -\frac{1}{2}$.
     Then $2x + 3y - 8 = 0$ is $X$ on $Y \implies 2x = -3y + 8 \implies x = -\frac{3}{2}y + 4 \implies b_{xy} = -\frac{3}{2}$.
     Check condition: $r^2 = b_{yx} \cdot b_{xy} = \left(-\frac{1}{2}\right)\left(-\frac{3}{2}\right) = \frac{3}{4} < 1$. *(Valid!)*
3. Calculate $r$:
   Since both $b_{yx}$ and $b_{xy}$ are negative, $r$ must be negative:
   $$r = -\sqrt{\frac{3}{4}} = -\frac{\sqrt{3}}{2} \approx -0.866$$
**Final Answer:** Line $Y$ on $X$ is $\mathbf{x + 2y - 5 = 0}$, Line $X$ on $Y$ is $\mathbf{2x + 3y - 8 = 0}$, and $\mathbf{r = -\frac{\sqrt{3}}{2} \approx -0.866}$.

---

# PART 4: MCQ SHORTCUTS & RAPID ELIMINATION TECHNIQUES

---

## 13. BUET MCQ Cheat Sheet & Elimination Rules

```
+----------------------------------------------------------------------------------------------------+
|                                    BUET MCQ CRITICAL FORMULAS                                      |
+------------------------------------+---------------------------------------------------------------+
| Topic                              | Formula / Shortcut                                            |
+------------------------------------+---------------------------------------------------------------+
| Derangements $D_n$                 | $D_n = \lfloor n!/e + 0.5 \rfloor$ (D3=2, D4=9, D5=44, D6=265) |
| Non-negative integer sum           | $\binom{n+k-1}{k-1}$                                          |
| Positive integer sum               | $\binom{n-1}{k-1}$                                            |
| Equivalence Relations count        | $B(0)=1, B(1)=1, B(2)=2, B(3)=5, B(4)=15, B(5)=52$            |
| Gallai's Identity                  | $\alpha(G) + \beta(G) = |V|$                                  |
| Planar Connected Graph             | $V - E + F = 2$ and $E \le 3V - 6$                            |
| Bipartite / Triangle-Free Planar   | $E \le 2V - 4$                                                |
| Maximum edges in Bipartite graph   | $\lfloor n^2 / 4 \rfloor$                                     |
| Handshaking Lemma                  | $\sum \deg(v) = 2E$                                           |
| Complete Graph edges               | $n(n-1)/2$                                                    |
| Hypercube $Q_d$ edges              | $d \cdot 2^{d-1}$                                             |
| Geometric Distribution Expectation | $E[X] = 1/p, \text{Var}(X) = (1-p)/p^2$                       |
| Poisson Distribution               | $E[X] = \lambda, \text{Var}(X) = \lambda$                     |
| Binomial Distribution              | $E[X] = np, \text{Var}(X) = np(1-p)$                          |
| Variance Scaling                   | $\text{Var}(aX + b) = a^2 \text{Var}(X)$                      |
| Regression $r$                     | $r = \text{sign}(b_{yx}) \sqrt{b_{yx} \cdot b_{xy}}$          |
| Chebyshev's Bound                  | $P(|X - \mu| \ge k\sigma) \le 1/k^2$                          |
| Markov's Bound                     | $P(X \ge a) \le E[X]/a$                                       |
+------------------------------------+---------------------------------------------------------------+
```

### 🎯 Fast Elimination Strategies for Negative Marking (-0.5 Marks)
1. **Check Boundary Constraints:** If an MCQ asks for a probability, immediately eliminate any options $< 0$ or $> 1$.
2. **Parity Check for Graphs:** If an MCQ option results in an odd sum of vertex degrees or an odd count of odd-degree vertices, eliminate immediately.
3. **Variance Invariance:** $\text{Var}(X + 10) = \text{Var}(X)$. Never choose options that scale variance with an additive constant!
4. **Independent Set Complement:** In any vertex cover question, verify if the chosen vertex set covers every edge. If $S$ is an independent set, $V \setminus S$ MUST be a vertex cover.

# BUET MSc in CSE / ICT Admission Examination: Master Preparation Handbook & Solution Guide

---

## Part 1: Examination Overview & Strategy

### 1.1 Exam Architecture & Marking Scheme
- **Phase 1: MCQ Exam**
  - **Questions:** 50 MCQs
  - **Marks:** 50 Marks (1 mark per question)
  - **Duration:** 50 Minutes (~1 minute per question)
  - **Negative Marking:** **-0.5 marks** per incorrect answer (High penalty! Random guessing will severely hurt your score).
  - **Screening Rule:** The written exam scripts are evaluated **ONLY IF** the candidate scores higher than or equal to the departmental cut-off score in the MCQ exam.
- **Phase 2: Written Exam**
  - **Questions:** 10 Questions (with sub-questions)
  - **Marks:** 100 Marks (10 marks per question)
  - **Duration:** 60 Minutes (6 minutes per 10-mark question)
  - **Format:** Conceptual derivations, diagrams, algorithm trace, code snippets, architectural trade-offs, numerical problems.
- **Calculator Policy:** Only **non-programmable** scientific calculators (e.g., fx-991MS, fx-570ES, fx-991EX) are permitted.

---

## 1. Structured & Object-Oriented Programming

### Core Concepts for MCQ & Written
- **Language Paradigms & Execution Models:**
  - **C / C++:** Compiled directly into native machine instructions via preprocessor $\to$ compiler $\to$ assembler $\to$ linker. Execution is fast with manual memory management (`malloc`/`free`, `new`/`delete`).
  - **Java:** Hybrid approach. Source code is compiled into platform-independent **Bytecode** (`.class`), which is executed by the **Java Virtual Machine (JVM)** using an interpreter and a Just-In-Time (JIT) compiler. Automatic Garbage Collection.
  - **Python:** Interpreted language compiled into intermediate bytecode (`.pyc`), executed on the **Python Virtual Machine (PVM)**. Dynamically typed.
- **OOP Pillars & Implementation Mechanics:**
  - **Encapsulation:** Bundling data and methods into classes with access specifiers (`private`, `protected`, `public`).
  - **Abstraction:** Hiding implementation details via Abstract Classes and Interfaces.
  - **Inheritance:** Code reuse and hierarchical typing (`single`, `multilevel`, `multiple`, `hierarchical`, `hybrid`). Note: Java handles multiple inheritance via Interfaces to avoid the diamond problem.
  - **Polymorphism:**
    - *Compile-Time (Static):* Method/Function Overloading, Operator Overloading.
    - *Run-Time (Dynamic):* Method Overriding using `virtual` functions and virtual method tables (`vtable` & `vptr` in C++).
- **Constructors & Destructors:** Order of constructor invocation (Base class constructor first $\to$ Derived class) vs destructor invocation (Derived first $\to$ Base). Virtual destructors prevent memory leaks when deleting derived objects through base pointers.
- **Memory Segments:** Text/Code segment, Data segment (initialized globals/statics), BSS segment (uninitialized globals/statics), Heap (dynamically allocated memory), Stack (local variables, function stack frames).

---

### Questions & Solutions (from Archives)

#### Q1.1: Define types of processors/execution models for the following languages: Python, C++, Java. [BUET MSC 15]
**Answer:**
1. **C++:** **Native Machine Code Compiler**. Source code (`.cpp`) is preprocessed, compiled, and linked directly into target-machine binary machine code (`.exe` or ELF binary) executed directly on physical hardware without any runtime virtual machine.
2. **Java:** **Hybrid (Compiler + Bytecode Interpreter / JIT Compiler)**. Java source (`.java`) is compiled by `javac` into intermediate, platform-independent bytecode (`.class`). At runtime, the Java Virtual Machine (JVM) interprets bytecode and uses a Just-In-Time (JIT) compiler to compile hot code paths directly into machine code.
3. **Python:** **Interpreted / Bytecode Interpreted**. Python source (`.py`) is compiled dynamically into intermediate bytecode (`.pyc`) and executed instruction-by-instruction by an interpreter on the Python Virtual Machine (PVM) (CPython).

---

## 2. Discrete & Concrete Mathematics, Probability & Statistics

### Core Concepts for MCQ & Written
- **Discrete Mathematics:**
  - **Set Theory & Combinatorics:** Pigeonhole Principle, Inclusion-Exclusion Principle, Permutations and Combinations with repetitions $\binom{n+r-1}{r}$.
  - **Graph Theory & Relations:** Equivalence relations (Reflexive, Symmetric, Transitive), Partial orders (Poset, Hasse diagram, Lattices). Graph isomorphisms, Planar graphs ($V - E + F = 2$), Euler paths/circuits, Hamiltonian paths.
  - **Independent Set & Vertex Cover (Gallai's Theorem):**
    For any undirected graph $G = (V, E)$:
    $$\alpha(G) + \beta(G) = |V|$$
    where $\alpha(G)$ is the size of the Maximum Independent Set and $\beta(G)$ is the size of the Minimum Vertex Cover.
- **Probability & Statistics:**
  - **Bayes' Theorem:** $P(A|B) = \frac{P(B|A)P(A)}{P(B)}$
  - **Distributions:** Binomial ($E = np, Var = np(1-p)$), Poisson ($P(X=k) = \frac{\lambda^k e^{-\lambda}}{k!}$), Normal/Gaussian distribution.
  - **Expectation & Variance:** $E[X+Y] = E[X] + E[Y]$, $Var(X) = E[X^2] - (E[X])^2$. For independent variables, $Var(X+Y) = Var(X) + Var(Y)$.

---

### Questions & Solutions (from Archives)

#### Q2.1: Let $G = (V, E)$ be a graph where $V = \{a, b, c, d, e, f, g, h, i, j, k\}$. If $S = \{a, c, g, h, k\}$ is a maximum independent set in $G$, then what is the size and vertex set of a minimum vertex cover? [BUET MSC CSE 21]
**Answer:**
- Total number of vertices $|V| = 11$.
- Maximum Independent Set $S = \{a, c, g, h, k\} \implies |S| = \alpha(G) = 5$.
- By **Gallai’s Identity**: $\alpha(G) + \beta(G) = |V|$
  $$\beta(G) = |V| - \alpha(G) = 11 - 5 = 6$$
- The complement of any independent set is a vertex cover.
  $$\text{Minimum Vertex Cover } V' = V \setminus S = \{b, d, e, f, i, j\}$$
- **Result:** Size is **6**, vertices are **$\{b, d, e, f, i, j\}$**.

---

## 3. Data Structures & Algorithms

### Core Concepts for MCQ & Written
- **Asymptotic Notations & Recurrence Relations:**
  - Big-O ($O$), Big-Omega ($\Omega$), Big-Theta ($\Theta$).
  - **Master Theorem:** For $T(n) = a T(n/b) + f(n)$ where $a \ge 1, b > 1$:
    - If $f(n) = O(n^{\log_b a - \epsilon})$ for $\epsilon > 0 \implies T(n) = \Theta(n^{\log_b a})$.
    - If $f(n) = \Theta(n^{\log_b a} \log^k n)$ for $k \ge 0 \implies T(n) = \Theta(n^{\log_b a} \log^{k+1} n)$.
    - If $f(n) = \Omega(n^{\log_b a + \epsilon})$ and regularity holds $\implies T(n) = \Theta(f(n))$.
- **Trees & Traversals:**
  - In-order (Left, Root, Right), Pre-order (Root, Left, Right), Post-order (Left, Right, Root).
  - Given Pre-order + In-order OR Post-order + In-order, a unique binary tree can be constructed.
  - **Binary Search Tree (BST):** Left child $<$ Root $<$ Right child. In-order traversal of a BST always gives elements in sorted ascending order.
  - **Binary Heap:** Complete binary tree stored in an array. For index $i$ (1-based): Left child at $2i$, Right child at $2i+1$, Parent at $\lfloor i/2 \rfloor$. Max Heap property: $A[\text{parent}] \ge A[\text{child}]$. Building a heap of $n$ elements takes $O(n)$ time.
- **Dynamic Programming (DP):**
  - **Matrix Chain Multiplication:** Given matrices $A_1, A_2, \dots, A_n$ where $A_i$ has dimension $d_{i-1} \times d_i$, the cost of multiplying $(A_1 \dots A_k)(A_{k+1} \dots A_n)$ is $m[1, k] + m[k+1, n] + d_0 d_k d_n$.
  - Longest Common Subsequence (LCS), 0/1 Knapsack ($O(nW)$), Shortest paths (Floyd-Warshall $O(V^3)$, Bellman-Ford $O(VE)$).
- **Greedy Algorithms & Huffman Coding:**
  - Prefix codes, optimal binary tree construction by repeatedly combining two smallest frequency nodes. Total encoded length = $\sum (\text{frequency}_i \times \text{codeword\_length}_i)$.

---

### Questions & Solutions (from Archives)

#### Q3.1: Let $A$ be a $30 \times 35$ matrix, $B$ be a $35 \times 15$ matrix, and $C$ is a $15 \times 5$ matrix. Then the minimum number of scalar multiplications for the matrix-chain-multiplication $ABC$ is: [BUET MSC CSE 21]
**Answer:**
- Dimensions: $d_0 = 30, d_1 = 35, d_2 = 15, d_3 = 5$.
- Two parenthesizations exist:
  1. **$(AB)C$:**
     - Cost to multiply $A \times B$: $30 \times 35 \times 15 = 15,750$ multiplications (produces $30 \times 15$ matrix).
     - Cost to multiply $(AB) \times C$: $30 \times 15 \times 5 = 2,250$ multiplications.
     - Total $= 15,750 + 2,250 = \mathbf{18,000}$.
  2. **$A(BC)$:**
     - Cost to multiply $B \times C$: $35 \times 15 \times 5 = 2,625$ multiplications (produces $35 \times 5$ matrix).
     - Cost to multiply $A \times (BC)$: $30 \times 35 \times 5 = 5,250$ multiplications.
     - Total $= 2,625 + 5,250 = \mathbf{7,875}$.
- **Result:** The minimum number of scalar multiplications is **7,875** with parenthesization $A(BC)$.

---

#### Q3.2: Algorithm $A$ solves a problem of size $n$ by dividing into 7 sub-problems of size $n/2$ each, recursively solving each sub-problem, and then combining the solutions in $O(n^2)$ time. Then the time complexity of the algorithm is closer to: [BUET MSC CSE 21]
**Answer:**
- Recurrence relation: $T(n) = 7 T(n/2) + \Theta(n^2)$
- Master Theorem parameters: $a = 7$, $b = 2$, $f(n) = \Theta(n^2) = \Theta(n^c)$ where $c = 2$.
- Critical value: $\log_b a = \log_2 7 \approx 2.807$.
- Since $c = 2 < \log_2 7 \approx 2.807$, Case 1 of Master Theorem applies.
- **Result:** $T(n) = \Theta(n^{\log_2 7}) \approx \mathbf{\Theta(n^{2.81})}$.

---

#### Q3.3: The length of a Huffman code for the message "effervescence" is: [BUET MSC 21]
**Answer:**
1. Character frequency count in `"effervescence"` (13 letters: e-f-f-e-r-v-e-s-c-e-n-c-e):
   - `e`: 5
   - `f`: 2
   - `c`: 2
   - `r`: 1
   - `v`: 1
   - `s`: 1
   - `n`: 1
   *(Total characters = 13)*
2. Huffman Tree Construction:
   - Combine lowest: `(r:1, v:1) -> node1:2`
   - Combine lowest: `(s:1, n:1) -> node2:2`
   - Active queue: `node1:2, node2:2, f:2, c:2, e:5`
   - Combine: `(node1:2, node2:2) -> node3:4`
   - Combine: `(f:2, c:2) -> node4:4`
   - Active queue: `node3:4, node4:4, e:5`
   - Combine: `(node3:4, node4:4) -> node5:8`
   - Active queue: `e:5, node5:8`
   - Combine: `(e:5, node5:8) -> Root:13`
3. Codeword Lengths:
   - `e`: Depth 1 $\implies$ Length = 1 bit (e.g., `'0'`)
   - `f, c`: Child of node4 $\implies$ Depth = $1 + 1 + 1 = 3$ bits
   - `r, v, s, n`: Child of node1/node2 $\implies$ Depth = $1 + 1 + 1 + 1 = 4$ bits
4. Total bit length $= \sum (\text{freq} \times \text{length})$:
   - `e`: $5 \times 1 = 5$
   - `f`: $2 \times 3 = 6$
   - `c`: $2 \times 3 = 6$
   - `r`: $1 \times 4 = 4$
   - `v`: $1 \times 4 = 4$
   - `s`: $1 \times 4 = 4$
   - `n`: $1 \times 4 = 4$
   - **Total Length** $= 5 + 6 + 6 + 4 + 4 + 4 + 4 = \mathbf{33\text{ bits}}$.

---

#### Q3.4: Pre-order and In-order sequences are given for a binary tree. Draw the binary tree. In-order: D B E A F C, Pre-order: A B D E C F. [BUET MSC 17]
**Answer:**
1. From Pre-order `[A, B, D, E, C, F]`, the root is **`A`**.
2. Locate `A` in In-order `[D, B, E, | A | , F, C]`:
   - Left subtree In-order: `[D, B, E]`, Pre-order: `[B, D, E]`. Root is **`B`**.
     - In In-order `[D, B, E]`, `D` is left child of `B`, `E` is right child of `B`.
   - Right subtree In-order: `[F, C]`, Pre-order: `[C, F]`. Root is **`C`**.
     - In In-order `[F, C]`, `F` is on the left of `C` $\implies$ `F` is left child of `C`, right child is empty.
3. **Tree Structure:**
   ```
          A
        /   \
       B     C
      / \   /
     D   E F
   ```

---

#### Q3.5: Write the Pseudocode for Binary Search Algorithm. [BUET MSC ICT 19]
**Answer:**
```c
int binarySearch(int arr[], int left, int right, int target) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target)
            return mid;             // Target found at index mid
        else if (arr[mid] < target)
            left = mid + 1;         // Search right half
        else
            right = mid - 1;        // Search left half
    }
    return -1;                      // Target not found
}
```
- **Time Complexity:** Worst/Average Case $O(\log n)$, Best Case $O(1)$.
- **Space Complexity:** Iterative $O(1)$, Recursive $O(\log n)$.

---

## 4. Compiler & Theory of Computation

### Core Concepts for MCQ & Written
- **Chomsky Hierarchy:**
  - Type-3: Regular Grammar / Language $\to$ Finite Automata (DFA, NFA).
  - Type-2: Context-Free Grammar (CFG) $\to$ Pushdown Automata (PDA).
  - Type-1: Context-Sensitive Grammar $\to$ Linear Bounded Automata (LBA).
  - Type-0: Unrestricted Grammar $\to$ Turing Machine.
- **Pumping Lemma for Regular Languages:**
  - If $L$ is regular, there exists pumping length $p$ such that any string $s \in L$ with $|s| \ge p$ can be split into $s = xyz$ where $|y| > 0$, $|xy| \le p$, and for all $i \ge 0$, $xy^i z \in L$.
  - **Crucial Rule:** Pumping lemma is a **negative test** (used to prove a language is NOT regular via contradiction). It CANNOT be used to prove a language IS regular.
- **DFA & NFA:**
  - $DFA = (Q, \Sigma, \delta, q_0, F)$ where transition function $\delta: Q \times \Sigma \to Q$.
  - $NFA = (Q, \Sigma, \delta, q_0, F)$ where $\delta: Q \times (\Sigma \cup \{\epsilon\}) \to 2^Q$.
  - Every NFA with $n$ states can be converted into an equivalent DFA with at most $2^n$ states (Subset Construction).
- **Phases of a Compiler:**
  1. Lexical Analyzer (Scanner $\to$ Tokens, uses DFA/NFA).
  2. Syntax Analyzer (Parser $\to$ Parse Tree, uses CFG).
  3. Semantic Analyzer (Type checking, syntax directed translation).
  4. Intermediate Code Generator (Generates 3-address code, Quadruples, Triples).
  5. Code Optimizer (Machine-independent & machine-dependent optimization).
  6. Code Generator (Target machine instructions).
  - *Supporting structures:* Symbol Table Manager, Error Handler.
- **Linker vs Loader:**
  - **Linker:** Combines object modules (`.o`/`.obj`) into a single executable, resolves external symbols and relocations.
  - **Loader:** Loads executable program into main memory from disk, assigns runtime addresses, and initiates execution.
- **Grammar Transformations:**
  - **Eliminating Left Recursion:**
    For $A \to A\alpha_1 \mid A\alpha_2 \mid \dots \mid \beta_1 \mid \beta_2$:
    Replace with:
    $$A \to \beta_1 A' \mid \beta_2 A' \mid \dots$$
    $$A' \to \alpha_1 A' \mid \alpha_2 A' \mid \dots \mid \epsilon$$
  - **Left Factoring:** Resolves common prefix ambiguity for predictive parsing: $A \to \alpha\beta_1 \mid \alpha\beta_2 \implies A \to \alpha A', A' \to \beta_1 \mid \beta_2$.
- **Parser Hierarchy & Table State Sizes:**
  - Top-down: LL(1), Recursive Descent, Operator Precedence.
  - Bottom-up: LR(0), SLR(1), LALR(1), CLR(1).
  - **State Count Relationship:**
    $$\text{Number of states: } |LR(0)| = |SLR(1)| = |LALR(1)| < |CLR(1)|$$
    *(CLR(1) splits states by lookahead, resulting in a significantly larger state table).*

---

### Questions & Solutions (from Archives)

#### Q4.1: Find the language generated by the following grammar over $\Sigma = \{a, b\}$: $S \to aSa \mid bSb \mid a \mid b$. [BUET MSC 15]
**Answer:**
- Base cases: $S \to a$ (length 1), $S \to b$ (length 1).
- Recursive step: $S \to aSa$ or $S \to bSb$ adds 1 matching character to both ends, increasing string length by 2 at each step.
- The strings produced are: $\{a, b, aaa, aba, bab, bbb, aaaaa, \dots\}$.
- All strings are palindromes of **odd length**.
- **Result:** **(B) All odd-length palindromes.**

---

#### Q4.2: Context Free Grammar of Even length non-empty palindromes comprising of $\{a, b, c\}$? [BUET MSC 17]
**Answer:**
- Even length non-empty palindromes over $\{a, b, c\}$ have the form $w w^R$ where $|w| \ge 1$.
- Base cases (length 2): $aa, bb, cc$.
- Production rules:
  $$S \to aSa \mid bSb \mid cSc \mid aa \mid bb \mid cc$$
  *(Alternative valid form: $S \to aSa \mid bSb \mid cSc \mid \epsilon$ where non-empty constraint is enforced).*

---

#### Q4.3: Write the regular expression which takes one or more alphanumeric character followed by even number of digits. [BUET MSC 17]
**Answer:**
- Let $A$ denote an alphanumeric character: $[a-zA-Z0-9]$.
- Let $D$ denote a digit: $[0-9]$.
- "One or more alphanumeric characters": $A^+ = A A^* = [a-zA-Z0-9]^+$.
- "Even number of digits": $(D D)^* = ([0-9][0-9])^* = ([0-9]^2)^*$.
- **Result:**
  $$[a\text{-}zA\text{-}Z0\text{-}9]^+ ([0-9][0-9])^* \quad \text{or} \quad (\text{alpha}+\text{digit})^+ (\text{digit}\cdot\text{digit})^*$$

---

#### Q4.4: What is Turing Test? [BSEC AP 18]
**Answer:**
Proposed by **Alan Turing in 1950**, the Turing Test is a benchmark test of a machine's ability to exhibit intelligent behavior equivalent to, or indistinguishable from, that of a human.
- A human interrogator communicates via text with two hidden entities: a human and a computer.
- If the interrogator cannot reliably tell which is the machine and which is the human, the machine is said to have **passed the Turing Test**.

---

#### Q4.5: Eliminate left recursion from the following CFG: $S \to SX \mid SSb \mid XS \mid a$. [BUET MSC 19]
**Answer:**
1. Group productions of non-terminal $S$:
   - Left-recursive terms: $S \to S(X)$ and $S \to S(Sb)$. Here $\alpha_1 = X, \alpha_2 = Sb$.
   - Non-left-recursive terms: $S \to XS$ and $S \to a$. Here $\beta_1 = XS, \beta_2 = a$.
2. Apply standard left-recursion elimination:
   $$S \to XS S' \mid a S'$$
   $$S' \to X S' \mid Sb S' \mid \epsilon$$

---

#### Q4.6: Why pumping lemma is only used for negative proofing? [BUET MSC CSE 24]
**Answer:**
- The Pumping Lemma states a **necessary property** of all regular languages:
  $$\text{Language is Regular} \implies \text{Pumping Lemma Property holds}$$
- By logical contraposition:
  $$\text{Pumping Lemma Property does NOT hold} \implies \text{Language is NOT Regular}$$
- However, the converse is **false**: satisfying the pumping lemma is NOT sufficient to prove a language is regular (some non-regular languages also satisfy the pumping lemma). Therefore, it can only be used as a negative test (proof by contradiction).

---

#### Q4.7: Construct DFA that accepts binary strings having odd number of 1's OR even number of 0's. [BUET MSC 19]
**Answer:**
- Define 4 states based on parity $(P_0, P_1)$ where $P_0 \in \{\text{Even } 0, \text{Odd } 0\}$ and $P_1 \in \{\text{Even } 1, \text{Odd } 1\}$:
  - $q_0 = (E_0, E_1)$ [Start state]: Even 0s, Even 1s (Accepting: Even 0s holds)
  - $q_1 = (E_0, O_1)$: Even 0s, Odd 1s (Accepting: Both hold)
  - $q_2 = (O_0, E_1)$: Odd 0s, Even 1s (Non-accepting: Neither holds)
  - $q_3 = (O_0, O_1)$: Odd 0s, Odd 1s (Accepting: Odd 1s holds)
- **Accepting States:** $\{q_0, q_1, q_3\}$.
- **State Transition Table:**
  | State | Input `0` | Input `1` | Accepting? |
  |---|---|---|---|
  | $\to q_0 (E_0, E_1)$ | $q_2$ | $q_1$ | **Yes** (Even 0) |
  | $* q_1 (E_0, O_1)$ | $q_3$ | $q_0$ | **Yes** (Both) |
  | $q_2 (O_0, E_1)$ | $q_0$ | $q_3$ | No |
  | $* q_3 (O_0, O_1)$ | $q_1$ | $q_2$ | **Yes** (Odd 1) |

---

#### Q4.8: Draw DFA of words ending with two a's only ('aa') for grammar over $\{a, b\}$. [BUET MSC 18]
**Answer:**
- Language $L = \{w \mid w \text{ ends with } aa\} = (a+b)^* aa$.
- States:
  - $q_0$ (Start): No trailing 'a' seen.
  - $q_1$: Exactly one trailing 'a' seen.
  - $q_2$ (Accepting): Trailing 'aa' seen.
- Transitions:
  - $\delta(q_0, a) = q_1, \quad \delta(q_0, b) = q_0$
  - $\delta(q_1, a) = q_2, \quad \delta(q_1, b) = q_0$
  - $\delta(q_2, a) = q_2, \quad \delta(q_2, b) = q_0$

---

#### Q4.9: What are the components of finite automata model? Differentiate between NFA and DFA. [BUET MSC]
**Answer:**
- **5-Tuple Definition of Finite Automata:** $M = (Q, \Sigma, \delta, q_0, F)$
  1. $Q$: Finite set of states.
  2. $\Sigma$: Finite set of input symbols (alphabet).
  3. $\delta$: Transition function.
  4. $q_0 \in Q$: Initial/Start state.
  5. $F \subseteq Q$: Set of final/accepting states.
- **Differences:**
  | Parameter | DFA (Deterministic) | NFA (Non-Deterministic) |
  |---|---|---|
  | **Transition Function** | $\delta: Q \times \Sigma \to Q$ (Unique next state) | $\delta: Q \times (\Sigma \cup \{\epsilon\}) \to 2^Q$ (Set of states) |
  | **$\epsilon$-Transitions** | Not allowed | Allowed ($\epsilon$-moves without reading input) |
  | **Multiple Paths** | Exactly one deterministic path per input string | Multiple simultaneous computation paths |
  | **Construction Complexity** | Harder to construct directly | Easier to design from regular expressions |
  | **Equivalence** | Expressive power is identical to NFA | Expressive power is identical to DFA |

---

#### Q4.10: Regular expression that generates $\{w \mid w \text{ contains at least two } a\text{'s, OR exactly two } b\text{'s}\}$.
**Answer:**
- Case 1: At least two $a$'s $\implies (a+b)^* a (a+b)^* a (a+b)^*$.
- Case 2: Exactly two $b$'s (and any number of $a$'s) $\implies a^* b a^* b a^*$.
- **Combined Regular Expression:**
  $$R = (a+b)^* a (a+b)^* a (a+b)^* + a^* b a^* b a^*$$

---

#### Q4.11: What is the role of a Linker? [BUET MSC 15]
**Answer:**
A **Linker** (link editor) performs two critical tasks:
1. **Symbol Resolution:** Connects external symbol references (function calls, global variables across multiple object files `.o`/`.obj` and static/dynamic libraries `.lib`, `.a`, `.so`, `.dll`).
2. **Relocation:** Merges multiple separate code and data sections into unified contiguous sections, and binds symbolic references to concrete relative/virtual memory addresses.

---

#### Q4.12: Why intermediate code generator is required between syntax analysis and semantic analysis/code generation? [BUET MSC 17]
**Answer:**
1. **Portability & Retargetability:** Separates compiler front-end (source language dependent) from back-end (target architecture dependent). To support $N$ languages and $M$ architectures, only $N + M$ modules are needed instead of $N \times M$.
2. **Machine-Independent Optimization:** High-level and loop optimizations (constant folding, dead-code elimination, common subexpression elimination) can be performed once on intermediate representation (IR) across all hardware architectures.

---

#### Q4.13: Why empty strings ($\epsilon$) / left recursion have to be eliminated to build a parser? [BUET MSC 18]
**Answer:**
1. **Left Recursion:** Causes top-down predictive parsers (like LL(1) and Recursive Descent) to enter infinite recursive loops without consuming any input tokens.
2. **Empty Strings / Non-determinism:** If productions contain $\epsilon$ with overlapping FIRST and FOLLOW sets, it causes parsing table conflicts (Multiple entries in LL(1) table $\implies$ grammar is ambiguous or not LL(1)).

---

## 5. Database Management Systems (DBMS)

### Core Concepts for MCQ & Written
- **Relational Concepts:**
  - Table (Relation), Tuple (Row/Record), Attribute (Column/Field), Domain (Data Type).
  - Degree (Number of columns) vs Cardinality (Number of rows).
- **SQL Commands Classification:**
  - **DDL (Data Definition):** `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`.
  - **DML (Data Manipulation):** `SELECT`, `INSERT`, `UPDATE`, `DELETE`.
  - **DCL (Data Control):** `GRANT`, `REVOKE`.
  - **TCL (Transaction Control):** `COMMIT`, `ROLLBACK`, `SAVEPOINT`.
- **Integrity Constraints & Keys:**
  - Super Key, Candidate Key, Primary Key (Unique + NOT NULL), Foreign Key (Referential Integrity), Alternate Key.
- **Relational Normalization & Functional Dependencies (FD):**
  - **1NF:** Atomic attribute values, no repeating groups.
  - **2NF:** In 1NF + No partial functional dependencies (every non-prime attribute is fully functionally dependent on whole candidate key).
  - **3NF:** In 2NF + No transitive dependencies ($X \to Y \implies X$ is superkey or $Y$ is prime attribute).
  - **BCNF:** For every non-trivial $X \to Y$, $X$ must be a superkey.
- **Transaction & Concurrency (ACID):**
  - **Atomicity:** All or nothing (handled by recovery manager / undo logs).
  - **Consistency:** Database remains in a valid state satisfying all invariants.
  - **Isolation:** Concurrent transactions execute without interference (handled by 2PL, Timestamp ordering, Serializable snapshot isolation).
  - **Durability:** Committed updates survive crashes (redo logging / WAL).

---

### Questions & Solutions (from Archives)

#### Q5.1: Definitions of DBMS Terms: Column, Record, Table, Field, Datatype. [BUET MSC ICT 16]
**Answer:**
1. **Column (Attribute):** A vertical entity in a table that represents a specific property or data characteristic of the entity.
2. **Record (Tuple / Row):** A horizontal collection of related fields representing a single, complete entity instance.
3. **Table (Relation):** A two-dimensional data structure consisting of rows and columns storing data about a specific entity set.
4. **Field:** An individual data item at the intersection of a specific row and column holding an atomic data value.
5. **Datatype:** The specification of the type and domain of values that can be stored in a column (e.g., `INT`, `VARCHAR`, `TIMESTAMP`).

---

#### Q5.2: What is the difference among DDL, DML, DCL, and TCL? Classify commands. [BUET MSC 17, 18]
**Answer:**
- **Full Forms & Definitions:**
  - **DDL (Data Definition Language):** Defines and modifies the database schema/structure.
  - **DML (Data Manipulation Language):** Retrieves, inserts, modifies, and deletes data inside tables.
  - **DCL (Data Control Language):** Manages user privileges, permissions, and security.
  - **TCL (Transaction Control Language):** Manages transactional units of work and integrity.
- **Command Classification:**
  - *DDL:* `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`
  - *DML:* `SELECT`, `INSERT`, `UPDATE`, `DELETE`
  - *DCL:* `GRANT`, `REVOKE`
  - *TCL:* `COMMIT`, `ROLLBACK`, `SAVEPOINT`

---

#### Q5.3: Definitions: View, Truncate, Foreign Key. [BUET MSC ICT 19]
**Answer:**
1. **View:** A virtual table defined by an underlying SQL query. It does not store physical data itself (except materialized views) but provides security encapsulation and simplified query abstraction.
2. **Truncate:** A DDL command that removes all rows from a table instantly by deallocating data pages. It is faster than `DELETE`, cannot have a `WHERE` clause, and resets identity seeds.
3. **Foreign Key:** A constraint that enforces **referential integrity** between two tables by ensuring that a column's value matches a primary key / unique key value in another table.

---

#### Q5.4: Convert a ternary relationship $R(A, B, C)$ with primary keys $a, b, c$ into equivalent binary E-R diagram. [BUET MSC ICT 15]
**Answer:**
- A ternary relationship $R$ connecting entity sets $A, B, C$ is converted into an equivalent binary relationship by **reification** (introducing an associative/bridge entity set $E_R$).
- **Transformation Steps:**
  1. Create a new entity set $E_R$ representing the relationship $R$.
  2. The primary key of $E_R$ is the composite key $(a, b, c)$ where $a, b, c$ are foreign keys referencing entity sets $A, B, C$ respectively.
  3. Replace ternary relationship $R$ with three binary relationships:
     - $R_A$ between $E_R$ and $A$ (Many-to-One).
     - $R_B$ between $E_R$ and $B$ (Many-to-One).
     - $R_C$ between $E_R$ and $C$ (Many-to-One).

---

#### Q5.5: E-R Diagram Design: Public Library System. [BUET MSC ICT 16]
*Requirements:* Books issued to members for a duration; late fine of 2 Tk/day; multiple copies of same book; searchable by name, ISBN, author, publisher.
**Answer (Design Specification):**
- **Entities & Attributes:**
  - `Book` (**ISBN** [PK], Title, Author, Publisher, Edition)
  - `BookCopy` (**CopyID** [PK], *ISBN* [FK], RackNo, Status [Available/Issued])
  - `Member` (**MemberID** [PK], Name, Phone, Email, Address, JoinDate)
  - `Loan/Issue` (**IssueID** [PK], *MemberID* [FK], *CopyID* [FK], IssueDate, DueDate, ReturnDate, FineAmount)
- **Relationships:**
  - `Book` has 1-to-Many `BookCopy` (1 book title has many physical copies).
  - `Member` borrows `BookCopy` through `Loan/Issue` (Many-to-Many resolved into 1-to-Many with `Loan/Issue`).

---

#### Q5.6: SQL Queries: [BUET MSC 17, 19, 21]

1. **Find Second Highest Salary of Employee:** [BUET MSC 17]
   ```sql
   -- Standard Subquery Method:
   SELECT MAX(salary) AS SecondHighestSalary
   FROM Employee
   WHERE salary < (SELECT MAX(salary) FROM Employee);

   -- Alternative using LIMIT / OFFSET:
   SELECT DISTINCT salary
   FROM Employee
   ORDER BY salary DESC
   LIMIT 1 OFFSET 1;
   ```

2. **Display Last 5 Records from Employee Table:** [BUET MSC 17]
   ```sql
   SELECT * FROM (
       SELECT * FROM Employee
       ORDER BY emp_id DESC
       LIMIT 5
   ) AS temp
   ORDER BY emp_id ASC;
   ```

3. **Update Price: Rise cost by 10% when cost < 500; decrease cost by 10% when cost > 1000:** [BUET MSC 19]
   ```sql
   UPDATE Products
   SET cost = CASE
       WHEN cost < 500 THEN cost * 1.10
       WHEN cost > 1000 THEN cost * 0.90
       ELSE cost
   END;
   ```

4. **Cartesian Product Output Count:** [BUET MSC CSE 21]
   *Problem:* `student` has 5 tuples, `takes` has 15 tuples. Output count for `SELECT * FROM student, takes;`
   **Answer:** $5 \times 15 = \mathbf{75\text{ tuples}}$.

5. **Count total employees in each district with a valid phone number:** [BUET MSC CSE 21]
   ```sql
   SELECT district, COUNT(*) AS TotalEmployees
   FROM Employee
   WHERE phone IS NOT NULL AND TRIM(phone) <> ''
   GROUP BY district;
   ```

---

## 6. Computer Networks & Security

### Core Concepts for MCQ & Written
- **OSI vs TCP/IP Models:**
  - OSI (7 Layers): Application, Presentation, Session, Transport, Network, Data Link, Physical.
  - TCP/IP (4/5 Layers): Application (combines OSI 5, 6, 7), Transport, Internet/Network, Data Link, Physical.
- **Data Link & Multiple Access:**
  - CSMA/CD (Ethernet): Collision Detection, exponential backoff. Cannot be used in wireless networks due to hidden/exposed terminal problems and high dynamic range of RF (transmitting drowns receiving) $\to$ Wireless uses **CSMA/CA** (RTS/CTS).
  - Ethernet Standards:
    - `10Base5` (Thicknet, Coaxial RG-8, 500m, 10 Mbps)
    - `10Base2` (Thinnet, Coaxial RG-58, 185m, 10 Mbps, BNC)
    - `10Base-T` (UTP Cat 3/5, 100m, 10 Mbps, RJ-45)
    - `100Base-TX` (Fast Ethernet, 100 Mbps, UTP Cat 5)
    - `1000Base-T` (Gigabit Ethernet, 1 Gbps, UTP Cat 5e/6)
- **Network Layer, IPv4 & CIDR Subnetting:**
  - Classful: A (/8), B (/16), C (/24), D (Multicast), E (Reserved).
  - **Private IP Ranges (RFC 1918):**
    - Class A: `10.0.0.0` to `10.255.255.255` (`10.0.0.0/8`)
    - Class B: `172.16.0.0` to `172.31.255.255` (`172.16.0.0/12`)
    - Class C: `192.168.0.0` to `192.168.255.255` (`192.168.0.0/16`)
  - Subnet formulas: Subnets $= 2^{\text{subnet bits}}$, Usable Hosts $= 2^{\text{host bits}} - 2$ (subtract Network ID and Broadcast ID).
- **Routing Algorithms:**
  - **Distance Vector (Bellman-Ford, RIP):** Suffers from Count-to-Infinity problem. Remedies: Split Horizon, Poison Reverse, Hold-down Timers, Triggered Updates.
  - **Link State (Dijkstra, OSPF):** Global topology knowledge, faster convergence, no count-to-infinity.
- **Transport Layer (TCP vs UDP):**
  - TCP: Connection-oriented, 3-way handshake (SYN, SYN-ACK, ACK), reliable (ACK, retransmission), flow control (Sliding Window), congestion control (Slow Start, Congestion Avoidance).
  - UDP: Connectionless, lightweight, low latency, no retransmission/ordering. Used for VoIP, video streaming, DNS, DHCP.
- **Application Protocols & Security:**
  - DNS (Port 53 UDP/TCP), DHCP (Port 67/68 UDP DORA: Discover, Offer, Request, Acknowledge), HTTP/HTTPS (Port 80/443), SMTP (25), POP3 (110), IMAP (143).
  - Security Attacks: ARP spoofing, MAC flooding, DNS poisoning/spoofing, SYN flood, Man-in-the-Middle (MITM).

---

### Questions & Solutions (from Archives)

#### Q6.1: Mesh Topology Cable and Port Calculation for 5 Devices. [BUET MSC CSE 21]
**Answer:**
For a fully connected mesh network of $n = 5$ devices:
1. **Total Cables/Links:**
   $$\text{Total Links} = \frac{n(n-1)}{2} = \frac{5 \times 4}{2} = \mathbf{10\text{ duplex cables}}$$
2. **Ports required per device:**
   $$\text{Ports per device} = n - 1 = 5 - 1 = \mathbf{4\text{ ports}}$$
   *(Total ports in system $= 5 \times 4 = 20$ ports)*.

---

#### Q6.2: Quality of Service (QoS) & Techniques to achieve QoS. [BUET MSC ICT 15]
**Answer:**
- **QoS:** The overall measurement of service performance in a network encompassing metrics like throughput, packet loss rate, propagation latency, and jitter.
- **Techniques to achieve QoS:**
  1. **Traffic Shaping & Policing:** Leaky Bucket Algorithm (smooths bursty traffic to constant rate), Token Bucket Algorithm (allows controlled bursts while bounding average rate).
  2. **Packet Scheduling:** Fair Queuing, Weighted Fair Queuing (WFQ), Priority Queuing.
  3. **Resource Reservation:** RSVP (Resource Reservation Protocol), Integrated Services (IntServ), Differentiated Services (DiffServ using DSCP bits in IP header).

---

#### Q6.3: Differences between Physical and Logical Address. [BUET MSC ICT 15]
**Answer:**
| Parameter | Physical Address (MAC) | Logical Address (IP) |
|---|---|---|
| **Layer** | Data Link Layer (Layer 2) | Network Layer (Layer 3) |
| **Length** | 48 bits (6 bytes), Hexadecimal (e.g., `00:1A:2B:3C:4D:5E`) | 32 bits (IPv4, Dotted decimal) or 128 bits (IPv6) |
| **Assignment** | Burned into NIC hardware at manufacture (OUI + Serial) | Dynamically (DHCP) or statically assigned by network admin |
| **Hierarchy** | Flat non-hierarchical address space | Hierarchical address space (Network ID + Host ID) |

---

#### Q6.4: Switching Techniques Comparison. [BUET MSC ICT 15, 16]
**Answer:**
- **Circuit Switching:** Dedicated end-to-end physical circuit established before data transfer (e.g., traditional telephone network). Fixed bandwidth, guaranteed delay, wasted resources during idle periods.
- **Packet Switching:** Data is divided into independent packets with headers and routed dynamically via store-and-forward. Statistical multiplexing, high link utilization, variable latency/jitter.
- **Cell Switching (ATM):** Uses small, fixed-length packets (53-byte cells: 5-byte header + 48-byte payload) combining high speed of hardware circuit switching with flexibility of packet switching.

---

#### Q6.5: Subnetting Calculations:

1. **Subnetting $172.16.0.0$ for 512 subnets and 100 hosts per subnet:** [BUET MSC 19]
   - Base address: $172.16.0.0$ is Class B (/16 default).
   - Subnets needed: $512 = 2^9 \implies 9$ subnet bits.
   - Hosts needed: $100 \implies 2^h - 2 \ge 100 \implies h = 7$ bits ($2^7 - 2 = 126$ hosts).
   - Check total bits: $16 + 9 + 7 = 32$ bits (Perfect match!).
   - New Subnet Mask: $/ (16 + 9) = /25 = \mathbf{255.255.255.128}$.

2. **Router Interface IP $192.168.2.1/23$: Identify valid host IPs.** [BUET MSC CSE 21]
   - Subnet mask $/23 = 255.255.254.0$. Host bits $= 32 - 23 = 9$ bits.
   - Block size in 3rd octet $= 256 - 254 = 2$.
   - For $192.168.2.1$, the subnet block is:
     - **Network ID:** `192.168.2.0`
     - **Broadcast ID:** `192.168.3.255`
     - **Usable Host Range:** `192.168.2.1` to `192.168.3.254`
   - Evaluate options:
     - `192.168.1.100`: **Invalid** (Belongs to $192.168.0.0/23$ block).
     - `192.168.1.198`: **Invalid** (Belongs to $192.168.0.0/23$ block).
     - `192.168.2.255`: **Valid Host IP!** (In a /23 subnet, `.2.255` is a regular host address, NOT broadcast).
     - `192.168.3.0`: **Valid Host IP!** (In a /23 subnet, `.3.0` is a regular host address, NOT network ID).

3. **Subnet $192.168.20.64/26$ Parameters:** [BUET MSC CSE 24]
   - (i) **Subnet Mask:** $/26 = \mathbf{255.255.255.192}$
   - (ii) **Broadcast Address:** Block size $= 256 - 192 = 64$. Subnet runs from `.64` to `.127` $\implies \mathbf{192.168.20.127}$.
   - (iii) **Usable Host IP Range:** $\mathbf{192.168.20.65\text{ to } 192.168.20.126}$ (62 usable hosts).

4. **Address $182.8.0.0$ to make 63 subnets:** [BUET MSC ICT 24]
   - $182.8.0.0$ is Class B (/16).
   - Subnets needed: $63 \implies 2^s \ge 63 \implies s = 6$ bits ($2^6 = 64$ subnets).
   - Subnet Mask: $16 + 6 = /22 = \mathbf{255.255.252.0}$.
   - Block size in 3rd octet $= 4$.
   - Highest Subnet: $182.8.252.0/22$.
   - Broadcast Address of Highest Subnet: $\mathbf{182.8.255.255}$.

---

#### Q6.6: Why CSMA/CD is not used in Wireless Networks? [BUET MSC]
**Answer:**
1. **Collision Detection Failure:** Signal attenuation is extremely high in wireless channels. A transmitting radio's own signal drowns out any faint colliding signal from another station, making simultaneous collision detection impossible.
2. **Hidden Terminal Problem:** Two nodes out of each other's radio range might transmit simultaneously to a common base station, causing collision at the receiver that neither sender can detect.
- *Solution:* Wireless uses **CSMA/CA (Collision Avoidance)** with RTS/CTS (Request/Clear to Send).

---

#### Q6.7: Remedy for the node-to-node looping / count-to-infinity problem in Distance Vector routing. [BUET MSC ICT 24]
**Answer:**
1. **Split Horizon:** A router never advertises a route back out the same interface through which it learned that route.
2. **Poison Reverse (Split Horizon with Poisoned Reverse):** Explicitly advertises a route learned from an interface back out that same interface with an infinite metric ($\infty = 16$).
3. **Hold-down Timers:** When a route goes down, the router ignores updates for a fixed time to allow network-wide convergence.
4. **Triggered Updates:** Instantly sends out routing table updates when a topology change occurs rather than waiting for the periodic timer.

---

## 7. Software Engineering & Information System Design

### Core Concepts for MCQ & Written
- **SDLC Models:**
  - **Waterfall:** Linear sequential phases. Best for stable, clear requirements. Disadvantage: Inflexible to changes, late testing.
  - **Spiral:** Risk-driven iterative model (Objective identification $\to$ Risk analysis $\to$ Engineering $\to$ Customer evaluation). Best for high-risk, large projects.
  - **Agile / Scrum:** Incremental sprints (2-4 weeks), user stories, daily standups, high customer collaboration.
- **UML Diagrams:**
  - *Structural:* Class Diagram (associations, multiplicity, composition vs aggregation), Object Diagram, Component Diagram.
  - *Behavioral:* Use Case Diagram (Actors, System Boundary, `<<include>>`, `<<extend>>`), Sequence Diagram, State Machine Diagram, Activity Diagram.
- **Design Patterns (GoF 23 Patterns):**
  - **Creational:** Singleton (single instance, private constructor), Factory Method, Abstract Factory, Builder.
  - **Structural:** Adapter, Decorator (dynamic feature addition), Facade, Proxy.
  - **Behavioral:** Strategy (pluggable interchangeable algorithms), Observer (pub-sub state updates), Command, Template Method.
- **Software Testing:**
  - **Unit Testing:** Tests individual units/methods in isolation (often mocked).
  - **Integration Testing:** Tests interactions between integrated modules (Top-down, Bottom-up, Big-bang).
  - **Acceptance Testing:** Alpha/Beta testing by users against business requirements.
  - **Black-Box vs White-Box Testing:**
    - Black-Box: Equivalence Partitioning, **Boundary Value Analysis (BVA)** (Test values: $\text{min}-1, \text{min}, \text{min}+1, \text{max}-1, \text{max}, \text{max}+1$).
    - White-Box: Statement Coverage, Branch/Decision Coverage, Cyclomatic Complexity ($V(G) = E - N + 2P$).
- **Project Management (PERT / CPM):**
  - **Critical Path:** The sequence of dependent activities with the longest duration. Activities on the critical path have **Zero Float/Slack**. Any delay on the critical path delays the entire project.

---

### Questions & Solutions (from Archives)

#### Q7.1: Fill in the Blanks. [BUET MSC 17]
**Answers:**
- a) **Data / Information** is an important factor of management information system.
- b) In a DFD external entities are represented by a **rectangle / square**.
- c) Use the new system at the same time as the old system to compare the results. This is known as **Parallel Run / Parallel Adoption**.
- d) After the design phase the document prepared is known as **Software Design Document (SDD) / Design Specification**.
- e) Actual programming of software code is done during the **Implementation / Coding** phase in the SDLC.

---

#### Q7.2: In an Assignment Submission System, a student cannot deny to have edited submission after the deadline. Which software security requirement signifies it? [BUET MSC CSE 21]
**Answer:**
- **Non-repudiation** (ensures that a user/party cannot deny the authenticity, origination, or modification of a submitted transaction or data record, typically enforced via digital signatures and cryptographic audit logs).

---

#### Q7.3: Strategy Design Pattern for Sorting Application: Class Diagram and Code. [BUET MSC 24]
**Answer:**
- **Pattern Role:** The Strategy Pattern defines a family of interchangeable algorithms (Sorting algorithms), encapsulates each one in a separate class, and allows the client context to switch algorithms at runtime.
- **C++ / Java Implementation:**
```java
// Strategy Interface
public interface SortStrategy {
    void sort(int[] array);
}

// Concrete Strategy 1
public class BubbleSort implements SortStrategy {
    public void sort(int[] array) {
        System.out.println("Sorting using Bubble Sort (good for tiny lists)");
        // Bubble sort logic...
    }
}

// Concrete Strategy 2
public class QuickSort implements SortStrategy {
    public void sort(int[] array) {
        System.out.println("Sorting using Quick Sort (general high performance)");
        // Quick sort logic...
    }
}

// Concrete Strategy 3
public class MergeSort implements SortStrategy {
    public void sort(int[] array) {
        System.out.println("Sorting using Merge Sort (stable O(N log N))");
        // Merge sort logic...
    }
}

// Context Class
public class SortContext {
    private SortStrategy strategy;

    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void executeSort(int[] array) {
        strategy.sort(array);
    }
}
```
- **UML Class Diagram Concept:**
  `SortContext` has a reference to `SortStrategy` interface (Aggregation), which is implemented by `BubbleSort`, `QuickSort`, `MergeSort`.

---

#### Q7.4: A string has to be entered whose length must be between 8-12. Validate using String Boundary Value Analysis (BVA). [BUET MSC 17]
**Answer:**
- Boundary values for valid range $[8, 12]$: Minimum $= 8$, Maximum $= 12$.
- **Test Cases:**
  1. Length = 7 ($\text{Min} - 1$): **Invalid** (Boundary below min)
  2. Length = 8 ($\text{Min}$): **Valid** (Exact lower boundary)
  3. Length = 9 ($\text{Min} + 1$): **Valid** (Just above lower boundary)
  4. Length = 11 ($\text{Max} - 1$): **Valid** (Just below upper boundary)
  5. Length = 12 ($\text{Max}$): **Valid** (Exact upper boundary)
  6. Length = 13 ($\text{Max} + 1$): **Invalid** (Boundary above max)

---

## 8. Digital Logic Design, Microprocessors & Microcontrollers

### Core Concepts for MCQ & Written
- **Microprocessor vs Microcontroller:**
  | Feature | Microprocessor (e.g., 8086, x86) | Microcontroller (e.g., 8051, PIC, AVR) |
  |---|---|---|
  | **Architecture** | Standalone CPU chip; RAM, ROM, I/O ports connected externally via buses. | All-in-one chip (CPU, RAM, Flash ROM, Timers, I/O ports integrated). |
  | **Cost & Size** | Higher board footprint, higher total cost. | Compact, single-chip solution, very low cost. |
  | **Applications** | General-purpose computing (PCs, servers). | Dedicated embedded systems (appliances, automobiles, IoT). |
  | **Power** | Higher power consumption. | Ultra-low power consumption (sleep modes). |
- **8086 Architecture & Register Organization:**
  - 16-bit processor, 20-bit address bus ($\implies 2^{20} = 1\text{ MB}$ addressable physical memory).
  - **Segment Registers (16-bit):** CS (Code), DS (Data), SS (Stack), ES (Extra).
  - **Physical Address Formula:**
    $$\text{Physical Address} = (\text{Segment Register} \times 16_{10}) + \text{Offset} = (\text{Segment Register} \times 10_{16}) + \text{Offset}$$
  - **Instruction Execution / Bus Cycle:** 4 clock cycles per standard bus cycle ($T_1, T_2, T_3, T_4$). Slow memory triggers Wait states ($T_W$).
- **Superscalar Architecture:** Capability to execute more than one instruction per clock cycle via multiple parallel pipelines (e.g., Intel Pentium with dual $U$ and $V$ integer pipelines vs 80386/80486 scalar architectures).
- **Interrupts & Priority Schemes:**
  - Hardware Interrupts (NMI, INTR in 8086), Software Interrupts (`INT n`).
  - **Daisy-Chaining:** Hardware interrupt priority management with series connection along $INTA$.

---

### Questions & Solutions (from Archives)

#### Q8.1: What are the purposes of CS, DS, SS, ES registers? [BUET MSC ICT 15]
**Answer:**
1. **CS (Code Segment):** Stores the 16-bit base address of the 64 KB code segment containing currently executing program instructions. Paired with Instruction Pointer (`IP`).
2. **DS (Data Segment):** Stores the base address of the default 64 KB memory segment containing global and static variables.
3. **SS (Stack Segment):** Stores the base address of the 64 KB stack segment used for subroutine return addresses, parameters, and local variables. Paired with Stack Pointer (`SP`) and Base Pointer (`BP`).
4. **ES (Extra Segment):** Stores the base address of an auxiliary 64 KB data segment, frequently utilized in string/memory movement operations with Destination Index (`DI`).

---

#### Q8.2: Assembly Instruction Hex Calculation: [BUET MSC CSE 21]
```assembly
MOV BL, 8CH
MOV AL, 7EH
ADD AL, BL
```
*What will the contents of register AL look like after execution?*
**Answer:**
1. Hexadecimal addition:
   $$7\text{E}_{16} + 8\text{C}_{16}$$
   - Low nibble: $\text{E}_{16} + \text{C}_{16} = 14 + 12 = 26 = 16 \times 1 + 10 = \mathbf{A}_{16}$ (with Carry 1).
   - High nibble: $7_{16} + 8_{16} + 1 (\text{carry}) = 16 = 16 \times 1 + 0 = \mathbf{0}_{16}$ (with Carry 1).
2. Sum $= 10\text{A}_{16}$.
3. Since `AL` is an 8-bit register, it retains the lowest 8 bits: $\mathbf{0A_{16}}$ (Decimal 10).
4. Status Flags: Carry Flag $CF = 1$, Auxiliary Carry $AF = 1$, Sign Flag $SF = 0$, Zero Flag $ZF = 0$.

---

#### Q8.3: Why Pentium is a superscalar processor but 80386 is not? [BUET MSC 16, 17]
**Answer:**
- A **superscalar processor** contains multiple independent execution pipelines allowing it to issue and complete more than one instruction per clock cycle ($\text{IPC} > 1$).
- **Pentium:** Has dual integer execution pipelines (the $U$-pipe and $V$-pipe) enabling superscalar execution of two instructions simultaneously.
- **80386:** Is a scalar processor with a single execution unit that requires multiple clock cycles to execute a single instruction ($\text{IPC} < 1$).

---

## 9. Computer Architecture & Operating Systems

### Core Concepts for MCQ & Written
- **Process Management & Scheduling:**
  - Process States: New, Ready, Running, Waiting/Blocked, Terminated.
  - Process Control Block (PCB): PID, registers, PC, scheduling priority, open file descriptors.
  - Metrics: Turnaround Time ($\text{TAT} = \text{Completion Time} - \text{Arrival Time}$), Waiting Time ($\text{WT} = \text{TAT} - \text{Burst Time}$), Response Time ($\text{RT} = \text{First Run Time} - \text{Arrival Time}$).
  - Algorithms: FCFS (Convoy effect), SJF/SRTF (Optimal average WT), Priority, Round Robin (Time quantum selection).
- **Process Synchronization & Critical Section:**
  - 3 Mandatory Requirements: Mutual Exclusion, Progress, Bounded Waiting.
  - Solutions: Peterson's Algorithm, Test-and-Set / Compare-and-Swap, Semaphores (Counting and Binary/Mutex), Monitors.
- **Deadlocks:**
  - **4 Coffman Conditions:** Mutual Exclusion, Hold and Wait, No Preemption, Circular Wait.
  - Handling: Prevention (break 1 of 4 conditions), Avoidance (Banker's Algorithm, Safe State), Detection & Recovery (Resource Allocation Graph cycles), Omission (Ostrich algorithm).
- **Memory Management & Virtual Memory:**
  - **Paging:** Divides logical address space into fixed-size Pages and physical memory into Frames.
  - Address translation: $\text{Logical Address} = (\text{Page Number } p, \text{Offset } d)$.
  - **Page Table Size Formula:**
    $$\text{Page Table Size} = \text{Number of Pages} \times \text{Page Table Entry (PTE) Size} = \left(\frac{2^{\text{Logical Address Bits}}}{\text{Page Size}}\right) \times \text{PTE Size}$$
  - **Translation Lookaside Buffer (TLB):** Hardware associative cache for page translations.
    $$\text{Effective Memory Access Time (EMAT)} = h \times (t_{\text{TLB}} + t_{\text{RAM}}) + (1 - h) \times (t_{\text{TLB}} + 2 \times t_{\text{RAM}})$$
  - Page Replacement Algorithms: FIFO (Belady's Anomaly), Optimal (OPT/MIN), Least Recently Used (LRU), Clock/Second-Chance.
- **Linux Shell & Permissions:**
  - Permissions: Read ($r=4$), Write ($w=2$), Execute ($x=1$).
  - Triplet: Owner / Group / Others (e.g., `chmod 755 file`).
  - Essential Commands: `mkdir -p`, `rm -rf`, `grep -rn "word" .`, `chmod`, `chown`, `ps aux`, `kill -9`.

---

### Questions & Solutions (from Archives)

#### Q9.1: Define Multitasking, Multiprogramming, and Multithreading. [BUET MSC 17]
**Answer:**
1. **Multiprogramming:** Keeping multiple jobs in main memory simultaneously so that when one process waits for I/O, the CPU switches to another, maximizing CPU utilization.
2. **Multitasking (Time-Sharing):** A logical extension of multiprogramming where the CPU rapidly switches between tasks via time-slicing, enabling concurrent interactive user execution.
3. **Multithreading:** The capability of an operating system / process to execute multiple lightweight execution streams (threads) concurrently within the same shared address space.

---

#### Q9.2: Critical Section Code Analysis for Mutual Exclusion & Deadlock. [BUET MSC ICT 15]
*Given code with shared variables $C_1 = 1, C_2 = 1$:*
```c
// Process 1:
while(True) {
    C1 = 0;
    while(C2 == 0);
    // Critical Section 1
    C1 = 1;
}

// Process 2:
while(True) {
    C2 = 0;
    while(C1 == 0);
    // Critical Section 2
    C2 = 1;
}
```
**Answer:**
1. **Is Mutual Exclusion ensured?**
   - **Yes.** If Process 1 sets $C_1 = 0$ and enters its Critical Section, Process 2 cannot enter because it sets $C_2 = 0$ and gets blocked in `while(C1 == 0);`.
2. **What other problem may arise?**
   - **Deadlock!** If Process 1 sets $C_1 = 0$ and a context switch occurs immediately before Process 1 enters CS, Process 2 executes and sets $C_2 = 0$. Now both $C_1 = 0$ and $C_2 = 0$. Process 1 is stuck waiting for $C_2 == 1$ and Process 2 is stuck waiting for $C_1 == 1$. Both processes are permanently deadlocked.

---

#### Q9.3: Safe Ordering of Database Locks to Prevent Deadlock. [BUET MSC CSE 21]
*Problem:* Process A requests database records in order $R_2 \to R_3 \to R_1$. Which order of resource requests for Process B will guarantee a deadlock-free execution?
**Answer:**
- By **Dijkstra’s Resource Ordering Principle**, deadlocks caused by circular wait are completely prevented if all concurrent processes acquire shared resources in the **exact same global linear order**.
- Therefore, Process B must also acquire the records in the order: $\mathbf{R_2 \to R_3 \to R_1}$.

---

#### Q9.4: CPU Scheduling Turnaround and Response Time Calculation. [BUET MSC ICT 15]
*Problem:* Four jobs $A, B, C, D$ arrive at time $0$ with run times $8, 4, 4, 4$ minutes respectively. Calculate Turnaround Time and Response Time using Shortest Job First (SJF).
**Answer:**
- Arrival times: $A(0)=8, B(0)=4, C(0)=4, D(0)=4$.
- SJF Execution Order (tie-breaker alphabetical): $B \to C \to D \to A$.
- **Gantt Chart:**
  `[0 --- B --- 4 --- C --- 8 --- D --- 12 --- A --- 20]`
- **Calculations Table:**
  | Process | Burst Time | Completion Time | Turnaround Time ($\text{CT} - \text{AT}$) | Response Time ($\text{Start} - \text{AT}$) |
  |---|---|---|---|---|
  | **B** | 4 | 4 | $4 - 0 = 4$ | $0 - 0 = 0$ |
  | **C** | 4 | 8 | $8 - 0 = 8$ | $4 - 0 = 4$ |
  | **D** | 4 | 12 | $12 - 0 = 12$ | $8 - 0 = 8$ |
  | **A** | 8 | 20 | $20 - 0 = 20$ | $12 - 0 = 12$ |
- **Average Turnaround Time:** $\frac{4 + 8 + 12 + 20}{4} = \frac{44}{4} = \mathbf{11\text{ minutes}}$.
- **Average Response Time:** $\frac{0 + 4 + 8 + 12}{4} = \frac{24}{4} = \mathbf{6\text{ minutes}}$.

---

#### Q9.5: Paging Address Bit Calculations. [BUET MSC ICT 16]
*Problem:* Logical address space of 16 pages of 2048 words each, mapped to physical memory of 64 frames.
**Answer:**
1. **Logical Address Bits:**
   - Number of pages $= 16 = 2^4 \implies \text{Page bits } p = 4\text{ bits}$.
   - Page size $= 2048\text{ words} = 2^{11}\text{ words} \implies \text{Offset bits } d = 11\text{ bits}$.
   - **Total Logical Address Bits** $= 4 + 11 = \mathbf{15\text{ bits}}$.
2. **Physical Address Bits:**
   - Number of frames $= 64 = 2^6 \implies \text{Frame bits } f = 6\text{ bits}$.
   - Frame size $=$ Page size $= 11\text{ bits offset}$.
   - **Total Physical Address Bits** $= 6 + 11 = \mathbf{17\text{ bits}}$.

---

#### Q9.6: Page Table Size Calculation. [BUET MSC 17]
*Problem:* 32-bit logical address space, page size $4\text{ KB}$, each page table entry is 4 bytes. What is the size of the page table?
**Answer:**
- Total logical address space $= 2^{32}\text{ bytes} = 4\text{ GB}$.
- Page size $= 4\text{ KB} = 2^{12}\text{ bytes}$.
- Number of pages $= \frac{2^{32}}{2^{12}} = 2^{20} = 1,048,576\text{ pages}$.
- Page Table Entry (PTE) size $= 4\text{ bytes}$.
- **Total Page Table Size** $= 2^{20} \times 4\text{ bytes} = 4 \times 1\text{ MB} = \mathbf{4\text{ MB}}$.

---

#### Q9.7: Linux File Permissions Command. [BUET MSC 17]
*Condition:* Anyone can execute `sample`, only the owner can read, no other users can read.
**Answer:**
- Permission Breakdown:
  - **Owner ($u$):** Read + Execute (+ Write optional) $\to r-x = 4 + 1 = 5$ (or $rwx = 4+2+1=7$).
  - **Group ($g$):** Execute only, no read $\to --x = 1$.
  - **Others ($o$):** Execute only, no read $\to --x = 1$.
- **Octal Shell Command:**
  ```bash
  chmod 711 sample
  # Or without write permission:
  chmod 511 sample
  # Or symbolic:
  chmod u=rwx,go=x sample
  ```

---

## 10. Artificial Intelligence & Machine Learning

### Core Concepts for MCQ & Written
- **Search Algorithms:**
  - **Uninformed Search:** BFS (Complete, Optimal for unit costs, Space $O(b^d)$), DFS (Space $O(bm)$, not complete/optimal), Iterative Deepening DFS (IDDFS, Space $O(bd)$, Optimal).
  - **Informed (Heuristic) Search:**
    - **A\* Search:** Evaluates $f(n) = g(n) + h(n)$. Guaranteed complete and optimal if heuristic $h(n)$ is **admissible** ($0 \le h(n) \le h^*(n)$) for tree search and **consistent / monotonic** ($h(n) \le c(n, a, n') + h(n')$) for graph search.
    - **IDA\* (Iterative Deepening A\*):** Memory-bounded variant of A* using $f$-cost limits with linear space $O(bd)$.
    - **RBFS (Recursive Best-First Search):** Linear-space heuristic search keeping track of the best alternative $f$-value of siblings.
  - **Local Search:** Hill Climbing, Simulated Annealing (accepts worse moves with probability $e^{-\Delta E / T}$ to escape local optima), Genetic Algorithms.
- **Adversarial Search (Game Playing):**
  - Minimax algorithm, Alpha-Beta pruning (prunes branches when $\alpha \ge \beta$).
  - Lookup / Transposition tables store previously evaluated board positions to avoid redundant sub-tree searches.
- **Machine Learning Classifiers & Evaluation Metrics:**
  - **Confusion Matrix:**
    | | Actual Positive | Actual Negative |
    |---|---|---|
    | **Predicted Positive** | True Positive (TP) | False Positive (FP) |
    | **Predicted Negative** | False Negative (FN) | True Negative (TN) |
  - **Formulas:**
    $$\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}$$
    $$\text{Precision} = \frac{TP}{TP + FP}$$
    $$\text{Sensitivity / Recall / True Positive Rate} = \frac{TP}{TP + FN}$$
    $$\text{Specificity / True Negative Rate} = \frac{TN}{TN + FP}$$
    $$\text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$
- **Overfitting vs Underfitting:**
  - **Overfitting:** High variance, low training error, high test error. Remedied by L1/L2 Regularization, Cross-validation, Pruning, Dropout, Early Stopping, Data Augmentation.
  - **Ensemble Learning:** Bagging (Bootstrap Aggregating, e.g., Random Forest $\to$ reduces variance), Boosting (AdaBoost, XGBoost $\to$ reduces bias).

---

### Questions & Solutions (from Archives)

#### Q10.1: Precision and Sensitivity Calculation from Confusion Matrix. [BUET MSC CSE 21]
*Given Confusion Matrix:*
| Prediction \ Ground Truth | **+** | **-** |
|---|---|---|
| **+** | 10 | 2 |
| **-** | 3 | 5 |

**Answer:**
- From the table:
  - $\text{True Positives (TP)} = 10$
  - $\text{False Positives (FP)} = 2$
  - $\text{False Negatives (FN)} = 3$
  - $\text{True Negatives (TN)} = 5$
- **1. Precision Calculation:**
  $$\text{Precision} = \frac{TP}{TP + FP} = \frac{10}{10 + 2} = \frac{10}{12} = \frac{5}{6} \approx \mathbf{0.8333\text{ (83.33\%)}}$$
- **2. Sensitivity (Recall) Calculation:**
  $$\text{Sensitivity} = \frac{TP}{TP + FN} = \frac{10}{10 + 3} = \frac{10}{13} \approx \mathbf{0.7692\text{ (76.92\%)}}$$

---

#### Q10.2: How does A* search work? Which function satisfies optimal solution? [BUET MSC CSE 24]
**Answer:**
1. **Mechanism:** A* search maintains a priority queue of open nodes ordered by the evaluation function:
   $$f(n) = g(n) + h(n)$$
   - $g(n)$: Exact path cost accumulated from the start node to node $n$.
   - $h(n)$: Estimated heuristic cost from node $n$ to the nearest goal.
   - At each step, A* expands the node with the lowest $f(n)$.
2. **Optimality Conditions:**
   - **Tree Search:** A* is optimal if $h(n)$ is **Admissible** ($h(n) \le h^*(n)$, never overestimates true cost).
   - **Graph Search:** A* is optimal if $h(n)$ is **Consistent / Monotonic** ($h(n) \le c(n, a, n') + h(n')$).

---

#### Q10.3: Overfitting Problem in Machine Learning and How to Overcome It. [BUET MSC 24]
**Answer:**
- **Definition:** Overfitting occurs when a statistical or machine learning model learns the training data and noise too closely, failing to generalize to unseen test data (high variance, low bias).
- **Techniques to Overcome Overfitting:**
  1. **Regularization:** Add penalty terms to loss function (L1 Lasso $\lambda \sum |w_i|$ for sparsity, L2 Ridge $\lambda \sum w_i^2$ for weight decay).
  2. **Cross-Validation:** Use $K$-Fold cross-validation to select hyperparameters reliably.
  3. **Early Stopping:** Terminate training when validation loss begins to increase.
  4. **Ensemble Methods:** Use Bagging / Random Forests to reduce model variance.
  5. **Pruning:** Prune deep decision trees to limit maximum depth.
  6. **Dropout & Data Augmentation:** Drop random neurons during neural network training; artificially expand training dataset size.

---

#### Q10.4: Advantages of IDA* (Iterative Deepening A*) Algorithm. [BUET MSC CSE 17]
**Answer:**
1. **Linear Memory Complexity:** Retains only the current path in memory, taking $O(bd)$ space where $b$ is branching factor and $d$ is depth (unlike standard A* which uses exponential memory $O(b^d)$).
2. **Complete & Optimal:** Preserves the completeness and optimality guarantees of A* (with an admissible heuristic).
3. **Low Overhead:** Avoids the overhead of sorting and maintaining a massive priority queue in RAM.

---

#### Q10.5: Local Search vs Global Search. [BUET MSC CSE 15]
**Answer:**
| Feature | Local Search (e.g., Hill Climbing, Simulated Annealing) | Global Search (e.g., BFS, DFS, A*) |
|---|---|---|
| **Goal** | Finds optimal/satisficing state in pure optimization problems. | Finds optimal path from start state to goal state. |
| **Memory** | Very low ($O(1)$ or $O(b)$); does not retain the search tree. | High ($O(b^d)$ or $O(bd)$); tracks explored/frontier nodes. |
| **Completeness** | Not guaranteed (can get stuck in local maxima/plateaus). | Guaranteed complete with systematic exploration. |
| **Example** | 8-Queens problem, Traveling Salesperson, VLSI layout. | Route finding, Robot path planning, Rubik's Cube solver. |

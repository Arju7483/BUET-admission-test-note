# 📘 Data Structures & Algorithms — BUET MSc Exam Notes
> **Single Source of Truth** | Covers MCQ + Written | Depth-First Approach

---

## 📋 Table of Contents

1. [Algorithm Analysis & Complexity](#1-algorithm-analysis--complexity)
2. [Sorting Algorithms](#2-sorting-algorithms)
3. [Searching Algorithms](#3-searching-algorithms)
4. [Linked Lists](#4-linked-lists)
5. [Stacks & Queues](#5-stacks--queues)
6. [Trees & Binary Search Trees](#6-trees--binary-search-trees)
7. [Heaps & Priority Queue](#7-heaps--priority-queue)
8. [Hashing](#8-hashing)
9. [Graph Algorithms](#9-graph-algorithms)
10. [Dynamic Programming](#10-dynamic-programming)
11. [Greedy Algorithms & Huffman Coding](#11-greedy-algorithms--huffman-coding)
12. [Divide and Conquer](#12-divide-and-conquer)
13. [NP-Completeness](#13-np-completeness)
14. [BUET Past Questions — Solved](#14-buet-past-questions--solved)

---

## 1. Algorithm Analysis & Complexity

### 1.1 Asymptotic Notation

| Notation | Name | Meaning |
|----------|------|---------|
| O(f(n)) | Big-O | **Upper bound** — worst case |
| Ω(f(n)) | Big-Omega | **Lower bound** — best case |
| Θ(f(n)) | Big-Theta | **Tight bound** — exact order |
| o(f(n)) | Little-o | Strictly less than (not tight) |
| ω(f(n)) | Little-omega | Strictly greater than (not tight) |

**Formal Definitions:**
- `f(n) = O(g(n))` if ∃ c > 0, n₀ > 0 such that f(n) ≤ c·g(n) for all n ≥ n₀
- `f(n) = Ω(g(n))` if ∃ c > 0, n₀ > 0 such that f(n) ≥ c·g(n) for all n ≥ n₀
- `f(n) = Θ(g(n))` if f(n) = O(g(n)) AND f(n) = Ω(g(n))

**Complexity Hierarchy (slowest to fastest growth):**
```
O(1) < O(log n) < O(sqrt(n)) < O(n) < O(n log n) < O(n^2) < O(n^3) < O(2^n) < O(n!)
```

### 1.2 Common Complexities Quick Reference

| Algorithm | Best | Average | Worst | Space |
|-----------|------|---------|-------|-------|
| Binary Search | O(1) | O(log n) | O(log n) | O(1) |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |
| BFS/DFS | — | — | O(V+E) | O(V) |
| Dijkstra (min-heap) | — | — | O((V+E) log V) | O(V) |

### 1.3 Master Theorem

For recurrences of the form: **T(n) = aT(n/b) + f(n)**
- a ≥ 1 (number of subproblems)
- b > 1 (factor by which problem size is reduced)
- f(n) = asymptotically positive function

**Three Cases:**

| Case | Condition | Solution |
|------|-----------|----------|
| Case 1 | f(n) = O(n^(log_b(a) - ε)) | T(n) = **Θ(n^log_b(a))** |
| Case 2 | f(n) = Θ(n^log_b(a)) | T(n) = **Θ(n^log_b(a) · log n)** |
| Case 3 | f(n) = Ω(n^(log_b(a) + ε)) AND a·f(n/b) ≤ c·f(n) | T(n) = **Θ(f(n))** |

**Key: compute log_b(a) first, then compare with f(n)**

**Examples:**
```
Merge Sort: T(n) = 2T(n/2) + O(n)
  a=2, b=2, log_b(a) = log₂2 = 1
  f(n) = O(n) = O(n^1)  -> Case 2 -> T(n) = O(n log n) ✓

Binary Search: T(n) = T(n/2) + O(1)
  a=1, b=2, log_b(a) = log₂1 = 0
  f(n) = O(1) = O(n^0) -> Case 2 -> T(n) = O(log n) ✓

[BUET 2021 Q]: T(n) = 7T(n/2) + O(n^2)
  a=7, b=2, log_b(a) = log₂7 ≈ 2.807
  f(n) = O(n^2), and 2 < 2.807 -> Case 1 -> T(n) = O(n^2.807) ≈ O(n^2.81)
```

> **MCQ TIP:** For BUET 2021 Q: "Algorithm A solves by dividing into 7 sub-problems of size n/2, combining in O(n²) time" → Answer: **closer to O(n³)** since n^2.807 is between n² and n³

### 1.4 Recursion Tree Example

For T(n) = 2T(n/2) + n:
```
Level 0:        n                    <- cost = n
Level 1:      n/2  n/2               <- cost = n
Level 2:   n/4 n/4 n/4 n/4           <- cost = n
...
Level k:  n/2^k ... (2^k nodes)     <- cost = n
Total levels = log₂n -> Total = n·log n = O(n log n)
```

---

## 2. Sorting Algorithms

### 2.1 Bubble Sort
**Idea:** Repeatedly compare adjacent elements, swap if out of order. Largest "bubbles" to end.

```
Array: [5, 3, 8, 1]
Pass 1: [3,5,8,1] -> [3,5,8,1] -> [3,5,1,8]  <- 8 in place
Pass 2: [3,5,1,8] -> [3,1,5,8]               <- 5 in place
Pass 3: [1,3,5,8]                            <- done
```

| Property | Value |
|----------|-------|
| Time (worst/avg) | O(n²) |
| Time (best - sorted) | O(n) |
| Space | O(1) |
| Stable? | YES |
| In-place? | YES |

### 2.2 Selection Sort
**Idea:** Find minimum in unsorted portion, place at beginning.

| Property | Value |
|----------|-------|
| Time (all cases) | O(n²) |
| Space | O(1) |
| Stable? | NO (default) |
| In-place? | YES |
| Key difference | Always O(n²), minimal swaps (O(n)) |

### 2.3 Insertion Sort
**Idea:** Build sorted array one element at a time by inserting into correct position.

```
[5, 3, 8, 1]
[3, 5, 8, 1]   <- 3 inserted
[3, 5, 8, 1]   <- 8 already in place
[1, 3, 5, 8]   <- 1 inserted
```

| Property | Value |
|----------|-------|
| Time (best) | O(n) — nearly sorted |
| Time (avg/worst) | O(n²) |
| Stable? | YES |
| Best for | Small or nearly sorted data |

### 2.4 Merge Sort ⭐ (Important)
**Idea:** Divide array in half, recursively sort each half, then merge.

```
Divide:
[38, 27, 43, 3, 9, 82, 10]
     /                    \
[38, 27, 43]         [3, 9, 82, 10]
  /     \              /          \
[38]  [27,43]        [3,9]      [82,10]
       / \            / \          / \
     [27][43]       [3] [9]     [82] [10]

Merge (bottom-up):
[27,43]  ->  [27,38,43]  ->  [3,9,10,82]
                   \               /
              [3,9,10,27,38,43,82]
```

| Property | Value |
|----------|-------|
| Time (all cases) | O(n log n) |
| Space | O(n) — extra array |
| Stable? | YES |
| Best for | Linked lists, external sorting |

**Merge Pseudocode:**
```
MERGE(A, left, mid, right):
  n1 = mid - left + 1
  n2 = right - mid
  L[1..n1] = A[left..mid]
  R[1..n2] = A[mid+1..right]
  i = 1, j = 1, k = left
  while i <= n1 and j <= n2:
    if L[i] <= R[j]: A[k++] = L[i++]
    else: A[k++] = R[j++]
  copy remaining L or R elements
```

### 2.5 Quick Sort ⭐ (Important)
**Idea:** Choose a pivot, partition array around it (elements < pivot on left, > on right), recursively sort partitions.

```
Pivot = last element:
[3, 6, 8, 10, 1, 2, 1]   pivot = 1
Partition: [1] [1] [3, 6, 8, 10, 2]
                               pivot = 2
                  [2] [3, 6, 8, 10]   ...continue
Result: [1, 1, 2, 3, 6, 8, 10]
```

**Lomuto Partition:**
```
PARTITION(A, lo, hi):
  pivot = A[hi]
  i = lo - 1
  for j = lo to hi-1:
    if A[j] <= pivot:
      i++
      swap A[i], A[j]
  swap A[i+1], A[hi]
  return i+1
```

| Property | Value |
|----------|-------|
| Time (best/avg) | O(n log n) |
| Time (worst) | O(n²) — sorted/reverse sorted |
| Space | O(log n) — stack |
| Stable? | NO |
| In-place? | YES |
| Worst case occurs when | Pivot always min or max (sorted input) |

**Randomized QuickSort:** Pick random pivot → expected O(n log n) always.

### 2.6 Heap Sort ⭐ (Important)
**Idea:** Build a max-heap, then repeatedly extract maximum.

**Steps:**
1. BUILD-MAX-HEAP(A) — O(n)
2. For i = n down to 2: swap A[1] with A[i], call MAX-HEAPIFY(A, 1, i-1) — O(n log n)

| Property | Value |
|----------|-------|
| Time (all cases) | O(n log n) |
| Space | O(1) — in-place! |
| Stable? | NO |
| Best feature | In-place with O(n log n) guarantee |

### 2.7 Sorting Comparison Summary

| Algorithm | Best | Avg | Worst | Space | Stable | In-Place |
|-----------|------|-----|-------|-------|--------|----------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | YES | YES |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | NO | YES |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | YES | YES |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | YES | NO |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | NO | YES |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | NO | YES |
| Counting | O(n+k) | O(n+k) | O(n+k) | O(k) | YES | NO |

> **Key MCQ Facts:**
> - **Only Merge Sort** is always O(n log n) AND stable
> - **Heap Sort** is only in-place O(n log n)
> - **Comparison-based sorting lower bound** = Ω(n log n)
> - **Insertion sort** is best for nearly sorted data
> - **Quick sort** has worst case O(n²) but best average performance in practice

---

## 3. Searching Algorithms

### 3.1 Linear Search
- Sequential check of each element
- Time: O(n) best=O(1), Space: O(1)
- Works on unsorted arrays

### 3.2 Binary Search ⭐ (Important — BUET Past Q)
**Prerequisite:** Array must be **sorted**

**Algorithm:**
```
BINARY-SEARCH(A, target, lo, hi):
  while lo <= hi:
    mid = lo + (hi - lo) / 2    <- avoid overflow
    if A[mid] == target: return mid
    elif A[mid] < target: lo = mid + 1
    else: hi = mid - 1
  return -1  (not found)
```

**Example:**
```
Array: [2, 5, 8, 12, 16, 23, 38, 56]   Target = 23
lo=0, hi=7 -> mid=3 -> A[3]=12 < 23 -> lo=4
lo=4, hi=7 -> mid=5 -> A[5]=23 = 23 -> FOUND at index 5
```

| Property | Value |
|----------|-------|
| Time | O(log n) |
| Space | O(1) iterative, O(log n) recursive |
| Recurrence | T(n) = T(n/2) + O(1) -> O(log n) |

> **BUET Past Q [ICT 19]:** Binary search pseudocode — always write iterative version.

---

## 4. Linked Lists

### 4.1 Types

| Type | Structure | Next | Prev |
|------|-----------|------|------|
| Singly | node -> node -> NULL | YES | NO |
| Doubly | NULL <- node <-> node -> NULL | YES | YES |
| Circular | node -> node -> (back to head) | YES | NO |

### 4.2 Operations Complexity

| Operation | Singly | Doubly |
|-----------|--------|--------|
| Access by index | O(n) | O(n) |
| Insert at head | O(1) | O(1) |
| Insert at tail (with tail ptr) | O(1) | O(1) |
| Delete at head | O(1) | O(1) |
| Delete arbitrary node | O(n) | O(1) if ptr given |
| Search | O(n) | O(n) |

### 4.3 Linked List vs Array

| Feature | Array | Linked List |
|---------|-------|-------------|
| Access | O(1) random | O(n) sequential |
| Insert/Delete | O(n) shift | O(1) if position known |
| Memory | Contiguous, fixed | Dynamic, scattered |
| Cache performance | Better | Worse |

---

## 5. Stacks & Queues

### 5.1 Stack (LIFO — Last In, First Out)

**Operations:** Push (insert top), Pop (remove top), Peek/Top (view top)
- All operations: **O(1)**
- Applications: Function calls, undo, expression evaluation, DFS

**Infix to Postfix using Stack:**
```
Expression: A + B * C - D
Rules: Higher precedence operators are pushed later, pop when lower precedence seen
Steps:
A   -> output: A
+   -> stack: [+]
B   -> output: A B
*   -> stack: [+, *]   (* > +, so push)
C   -> output: A B C
-   -> pop *, output: A B C *; pop +, output: A B C * +; push -
D   -> output: A B C * + D
End -> pop -: A B C * + D -
Result: A B C * + D -
```

### 5.2 Queue (FIFO — First In, First Out)

**Operations:** Enqueue (rear), Dequeue (front)
- All operations: **O(1)**
- Applications: BFS, scheduling, print queue

**Circular Queue:** Avoids the "false full" problem of simple queue by wrapping around.

### 5.3 Deque (Double-Ended Queue)
- Insert/Delete from both front and rear
- Applications: Sliding window problems

---

## 6. Trees & Binary Search Trees

### 6.1 Tree Terminology

```
         A          <- Root
       /   \
      B     C       <- Internal nodes
     / \     \
    D   E     F     <- Leaves (D, E, F)

Height of tree = 2 (longest path from root to leaf)
Depth of E = 2 (path from root to E)
Degree of B = 2 (number of children)
```

| Term | Definition |
|------|-----------|
| Root | Top node, no parent |
| Leaf | Node with no children |
| Height | Longest path from root to leaf |
| Depth of node | Distance from root |
| Degree | Number of children |
| Binary tree | Each node has at most 2 children |
| Full binary tree | Every node has 0 or 2 children |
| Complete binary tree | All levels filled except last (left-filled) |
| Perfect binary tree | All levels completely filled |

### 6.2 Tree Traversals ⭐ (Important — BUET Past Q)

Given tree:
```
        A
      /   \
     B     C
    / \   /
   D   E F
```

| Traversal | Order | Result |
|-----------|-------|--------|
| **Inorder** | Left -> Root -> Right | D B E A F C |
| **Preorder** | Root -> Left -> Right | A B D E C F |
| **Postorder** | Left -> Right -> Root | D E B F C A |
| **Level-order** | Level by level (BFS) | A B C D E F |

**Inorder Code:**
```
INORDER(node):
  if node == NULL: return
  INORDER(node.left)
  print(node.data)
  INORDER(node.right)
```

### 6.3 Reconstruct Tree from Traversals ⭐ (BUET Past Q)

**[BUET MSC 17]:** Inorder: `D B E A F C` | Preorder: `A B D E C F` → Draw tree.

**Method:**
1. Preorder first element = **Root** → Root = **A**
2. Find A in Inorder: `D B E | A | F C`
   - Left subtree inorder: `D B E`
   - Right subtree inorder: `F C`
3. Next in preorder after A: `B D E` = left subtree, Root = **B**
4. In `D B E`: B's left = D, B's right = E
5. Right subtree preorder: `C F`, Root = **C**, C's left = F

**Result:**
```
        A
      /   \
     B     C
    / \   /
   D   E F
```

### 6.4 Binary Search Tree (BST)

**BST Property:** For every node n:
- All nodes in **left subtree** have keys **< n.key**
- All nodes in **right subtree** have keys **> n.key**

```
        8
      /   \
     3    10
    / \     \
   1   6    14
      / \   /
     4   7 13
```

**Inorder traversal of BST gives sorted sequence: 1 3 4 6 7 8 10 13 14**

**Operations:**

| Operation | Average | Worst (skewed) |
|-----------|---------|----------------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

**BST Deletion Cases:**
1. **Node is leaf:** Simply remove
2. **Node has one child:** Replace node with its child
3. **Node has two children:** Replace with **inorder successor** (smallest in right subtree), then delete that node

### 6.5 AVL Tree (Self-Balancing BST)

**Balance Factor (BF)** = Height(left subtree) - Height(right subtree)

- Valid BF = {-1, 0, +1}
- If |BF| > 1 → **rebalance** with rotations

**Four Rotation Cases:**

| Case | Condition | Fix |
|------|-----------|-----|
| LL (Left-Left) | BF > 1, left child BF >= 0 | Right Rotation |
| RR (Right-Right) | BF < -1, right child BF <= 0 | Left Rotation |
| LR (Left-Right) | BF > 1, left child BF < 0 | Left Rotate on left child, then Right Rotate |
| RL (Right-Left) | BF < -1, right child BF > 0 | Right Rotate on right child, then Left Rotate |

**Right Rotation (LL Case) diagram:**
```
Before:          After:
    z               y
   / \            /   \
  y   T4    ->  x       z
 / \            / \    / \
x   T3         T1 T2  T3  T4
/ \
T1 T2
```

**AVL Operations:** O(log n) for all — guaranteed!
- vs BST: BST is O(n) worst case; AVL always O(log n)

---

## 7. Heaps & Priority Queue

### 7.1 Heap Properties

**Max-Heap:** Parent >= children (root is maximum)
**Min-Heap:** Parent <= children (root is minimum)

**Complete binary tree stored as array (1-indexed):**
- Parent of node i = floor(i/2)
- Left child of node i = 2i
- Right child of node i = 2i+1

```
Array: [10, 5, 3, 4, 1]  (1-indexed: indices 1..5)
Tree:
        10         (index 1)
       /  \
      5    3       (index 2, 3)
     / \
    4   1          (index 4, 5)
```

### 7.2 Building a Max-Heap ⭐ (BUET Past Q)

**[BUET MSC 17]:** Draw maximum heap from given array.

**Array → Heap Example:**
```
Input: [4, 10, 3, 5, 1]

Initial tree:
        4
       / \
      10   3
     / \
    5   1

Step 1: Start from last non-leaf (index floor(5/2) = 2 = node 10)
  10's children: 5 and 1. 10 is largest, no swap.

Step 2: Heapify node at index 1 (root = 4):
  4's children: 10 and 3. Largest = 10. Swap 4 and 10.
        10
       / \
      4    3
     / \
    5   1
  Now node 4 has children 5 and 1. Largest = 5. Swap 4 and 5.
        10
       / \
      5    3
     / \
    4   1

Final Max-Heap array: [10, 5, 3, 4, 1]
```

**BUILD-MAX-HEAP:** Start from floor(n/2) down to 1, apply MAX-HEAPIFY
- Time: **O(n)** (not O(n log n) as intuition suggests!)

### 7.3 MAX-HEAPIFY (Maintaining Heap Property)
```
MAX-HEAPIFY(A, i, n):
  left = 2i
  right = 2i + 1
  largest = i
  if left <= n and A[left] > A[largest]: largest = left
  if right <= n and A[right] > A[largest]: largest = right
  if largest != i:
    swap A[i], A[largest]
    MAX-HEAPIFY(A, largest, n)
```

### 7.4 Heap Operations

| Operation | Time |
|-----------|------|
| Build Heap | O(n) |
| Insert | O(log n) |
| Extract Max/Min | O(log n) |
| Peek Max/Min | O(1) |
| Heapify | O(log n) |

### 7.5 Priority Queue
- Implemented using heap
- Each element has a priority
- Higher priority → served first
- Applications: Dijkstra's algorithm, Prim's MST, Huffman coding

---

## 8. Hashing

### 8.1 Hash Tables
- **Hash function h(k)** maps key k to slot h(k) in array of size m
- Ideal: O(1) average for search, insert, delete

### 8.2 Hash Functions

**Division Method:** `h(k) = k mod m`
- Best when m is prime, not close to power of 2

**Multiplication Method:** `h(k) = floor(m * (k*A mod 1))`
- A ≈ 0.618 (golden ratio) works well

### 8.3 Collision Resolution

**Chaining:**
- Each slot has a linked list
- Load factor α = n/m
- Average search: O(1 + α)

**Open Addressing (Probing):**
| Method | Probe Sequence | Issue |
|--------|---------------|-------|
| Linear Probing | h(k), h(k)+1, h(k)+2, ... | Primary clustering |
| Quadratic Probing | h(k), h(k)+1², h(k)+2², ... | Secondary clustering |
| Double Hashing | h₁(k) + i·h₂(k) | Best distribution |

**Load Factor (α):**
- α = n/m (n = elements, m = table size)
- Keep α < 0.7 for open addressing
- Chaining works with α > 1

---

## 9. Graph Algorithms

### 9.1 Graph Representations ⭐ (BUET Past Q)

**[BUET MSC ICT 19]:** Find adjacency matrix from given graph.

**Graph G = (V, E):**
```
    1 --- 2
    |   / |
    |  /  |
    3 --- 4
Edges: (1,2),(1,3),(2,3),(2,4),(3,4)
```

**Adjacency Matrix (4x4):**
```
   1  2  3  4
1 [0  1  1  0]
2 [1  0  1  1]
3 [1  1  0  1]
4 [0  1  1  0]
```
- Space: O(V²)
- Check edge (u,v): O(1)
- Find all neighbors of u: O(V)

**Adjacency List:**
```
1: [2, 3]
2: [1, 3, 4]
3: [1, 2, 4]
4: [2, 3]
```
- Space: O(V + E)
- Check edge: O(degree)
- Find all neighbors: O(degree)

**When to use which:**
- Dense graph (E ≈ V²): Adjacency Matrix
- Sparse graph (E << V²): Adjacency List (preferred for most algorithms)

### 9.2 Graph Traversals

#### BFS (Breadth-First Search)
**Idea:** Explore all neighbors before going deeper (level by level). Uses a **Queue**.

```
BFS(G, source s):
  for all vertices u: color[u] = WHITE, d[u] = infinity
  color[s] = GRAY, d[s] = 0
  Q = empty queue
  ENQUEUE(Q, s)
  while Q not empty:
    u = DEQUEUE(Q)
    for each neighbor v of u:
      if color[v] == WHITE:
        color[v] = GRAY
        d[v] = d[u] + 1
        ENQUEUE(Q, v)
    color[u] = BLACK
```

**BFS on example graph (start from A):**
```
    A - B - E
    |   |
    C - D
Queue process:
  Start: Q=[A], visited={A}
  Dequeue A -> neighbors B,C -> Q=[B,C], visited={A,B,C}
  Dequeue B -> neighbors A(visited),D,E -> Q=[C,D,E]
  Dequeue C -> neighbors A(v),D(v if already) -> Q=[D,E]
  ...BFS Order: A, B, C, D, E
```

| Property | BFS |
|----------|-----|
| Time | O(V + E) |
| Space | O(V) |
| Finds shortest path? | YES (unweighted graphs) |
| Data Structure | Queue |

#### DFS (Depth-First Search)
**Idea:** Go as deep as possible before backtracking. Uses a **Stack** (or recursion).

```
DFS-VISIT(u):
  color[u] = GRAY        (discovered)
  time = time + 1
  d[u] = time           (discovery time)
  for each neighbor v of u:
    if color[v] == WHITE:
      parent[v] = u
      DFS-VISIT(v)
  color[u] = BLACK      (fully processed)
  time = time + 1
  f[u] = time           (finish time)
```

| Property | DFS |
|----------|-----|
| Time | O(V + E) |
| Space | O(V) |
| Data Structure | Stack (recursion) |
| Applications | Topological sort, SCC, cycle detection |

**DFS Edge Types (directed graphs):**
- **Tree edges:** Edge in DFS forest
- **Back edges:** Leads to ancestor (indicates CYCLE)
- **Forward edges:** Leads to descendant (not tree edge)
- **Cross edges:** All other edges

### 9.3 Shortest Path Algorithms

#### Dijkstra's Algorithm ⭐ (Single Source, Non-negative weights)

**Idea:** Greedy — always process the unvisited vertex with minimum known distance.

```
DIJKSTRA(G, source s):
  d[v] = infinity for all v; d[s] = 0
  parent[v] = NULL
  Q = min-priority queue of all vertices by d[]
  while Q not empty:
    u = EXTRACT-MIN(Q)       <- greedily pick closest
    for each neighbor v of u:
      if d[u] + weight(u,v) < d[v]:    <- RELAX edge
        d[v] = d[u] + weight(u,v)
        parent[v] = u
        UPDATE-KEY(Q, v, d[v])
```

**Step-by-step Example:**
```
Graph:
  A --1-- B --4-- D
  |       |
  3       2
  |       |
  C --1-- E

Dijkstra from A:
Initial: d[A]=0, d[B]=d[C]=d[D]=d[E]=inf
Step 1: Extract A(0). Relax B->1, C->3
        d: A=0, B=1, C=3, D=inf, E=inf
Step 2: Extract B(1). Relax C->min(3,1+?)=3, E->1+2=3, D->1+4=5
        d: A=0, B=1, C=3, D=5, E=3
Step 3: Extract C(3) or E(3). Say C. Relax E->min(3,3+1)=3
        d: A=0, B=1, C=3, D=5, E=3
Step 4: Extract E(3). Relax D->min(5, ...)
Final: A=0, B=1, C=3, E=3, D=5
```

| Property | Dijkstra |
|----------|----------|
| Time (min-heap) | O((V+E) log V) |
| Time (adj matrix) | O(V²) |
| Handles negative weights? | NO |

#### Bellman-Ford Algorithm (Handles Negative Weights)

```
BELLMAN-FORD(G, s):
  d[v] = infinity for all v; d[s] = 0
  
  // Relax all edges V-1 times
  repeat V-1 times:
    for each edge (u, v) with weight w:
      if d[u] + w < d[v]:
        d[v] = d[u] + w
  
  // Check for negative-weight cycles
  for each edge (u, v) with weight w:
    if d[u] + w < d[v]:
      return "NEGATIVE CYCLE EXISTS"
  
  return d[]
```

**Why V-1 iterations?** Shortest path can have at most V-1 edges in a graph with V vertices.

| Property | Bellman-Ford |
|----------|-------------|
| Time | O(VE) |
| Handles negative weights? | YES |
| Detects negative cycles? | YES |

#### Floyd-Warshall (All-Pairs Shortest Path)

```
Initialize: D[i][j] = weight(i,j) if edge exists; 0 if i==j; inf otherwise

for k = 1 to V:          <- try using vertex k as intermediate
  for i = 1 to V:
    for j = 1 to V:
      D[i][j] = min(D[i][j], D[i][k] + D[k][j])
```

**Intuition:** D[i][j] after k iterations = shortest path from i to j using only vertices {1,...,k} as intermediates.

| Property | Floyd-Warshall |
|----------|---------------|
| Time | O(V³) |
| Space | O(V²) |
| Purpose | All-pairs shortest paths |
| Handles negative weights? | YES (no negative cycles) |

**Comparison:**

| Algorithm | Type | Time | Neg weights? |
|-----------|------|------|-------------|
| Dijkstra | Single source | O((V+E)logV) | NO |
| Bellman-Ford | Single source | O(VE) | YES |
| Floyd-Warshall | All pairs | O(V³) | YES |

### 9.4 Minimum Spanning Tree (MST)

**MST:** Spanning tree connecting all V vertices with V-1 edges and minimum total weight.

**Key Property (Cut Property):** For any cut of the graph, the minimum weight crossing edge is in some MST.

#### Kruskal's Algorithm (Edge-based, uses Union-Find)

```
KRUSKAL(G):
  A = {} (empty MST edge set)
  Sort edges by weight (ascending)
  Initialize Union-Find for each vertex
  for each edge (u, v) in sorted order:
    if FIND(u) != FIND(v):    <- u and v in different components
      A = A + {(u,v)}
      UNION(u, v)
  return A
```

**Example:**
```
Edges sorted: (A,B,1), (C,E,1), (B,E,2), (A,C,3), (B,D,4)

Process (A,B,1): Different components -> Add. MST: {(A,B)}
Process (C,E,1): Different components -> Add. MST: {(A,B),(C,E)}
Process (B,E,2): B and E in different components -> Add. MST: {(A,B),(C,E),(B,E)}
Process (A,C,3): A and C now connected (A-B-E-C) -> Skip (cycle!)
Process (B,D,4): Different -> Add. MST complete (V-1=4 edges).
```

**Time:** O(E log E) ≈ O(E log V)

#### Prim's Algorithm (Vertex-based, grows one tree)

```
PRIM(G, root r):
  key[v] = infinity for all v; key[r] = 0
  parent[v] = NULL
  Q = min-priority queue of all vertices
  while Q not empty:
    u = EXTRACT-MIN(Q)    <- vertex with minimum key
    for each neighbor v of u:
      if v in Q and weight(u,v) < key[v]:
        key[v] = weight(u,v)
        parent[v] = u
```

**Time:** O(E log V) with min-heap

| | Kruskal | Prim |
|---|---------|------|
| Approach | Edge-based | Vertex-based |
| Best for | Sparse graphs | Dense graphs |
| Data structure | Union-Find + sorted edges | Priority Queue |

### 9.5 Independent Set & Vertex Cover ⭐ (BUET 2021 Q)

**Independent Set:** Set S ⊆ V where no two vertices in S are adjacent (no edge between them).

**Vertex Cover:** Set C ⊆ V where every edge has at least one endpoint in C.

**Complement Theorem:** S is an independent set ↔ V\S is a vertex cover.

> **[BUET MSC CSE 21]:** V = {a,b,c,d,e,f,g,h,i,j,k} (11 vertices). Max independent set S = {a,c,g,h,k} (|S|=5).
> Minimum vertex cover = V \ S, size = 11 - 5 = **6**.

### 9.6 Topological Sort

**Valid only for DAG (Directed Acyclic Graph)**

**Method 1 (DFS-based):** Process vertices in decreasing finish time order.

```
TOPOLOGICAL-SORT(G):
  Run DFS on G
  As each vertex u finishes: insert u at FRONT of list
  Return the list
```

**Method 2 (Kahn's Algorithm / BFS-based):**
```
Compute in-degree of all vertices
Q = all vertices with in-degree 0
while Q not empty:
  u = dequeue Q
  output u
  for each neighbor v of u:
    in-degree[v]--
    if in-degree[v] == 0: enqueue v
```

**Application:** Scheduling, dependency resolution, build systems.

---

## 10. Dynamic Programming

### 10.1 Core Concepts

**Two key properties for DP:**
1. **Optimal Substructure:** Optimal solution contains optimal solutions to subproblems
2. **Overlapping Subproblems:** Same subproblems solved repeatedly

**DP vs Greedy vs D&C:**
- **D&C:** Subproblems are independent (Merge Sort)
- **DP:** Subproblems overlap — cache results
- **Greedy:** Greedy choice leads to optimal — simpler than DP

**Two approaches:**
- **Memoization (Top-Down):** Recursive + cache results
- **Tabulation (Bottom-Up):** Fill table iteratively

### 10.2 Matrix Chain Multiplication ⭐ (BUET Past Q)

**Problem:** Find optimal parenthesization to minimize scalar multiplications.

> **[BUET MSC CSE 21]:** A is 30×35, B is 35×15, C is 15×5. Find minimum scalar multiplications for ABC.

**Cost of multiplying (p×q) matrix with (q×r) matrix = p × q × r**

**Two orderings:**
```
Ordering 1: (AB)C
  AB cost = 30 × 35 × 15 = 15,750
  Result AB is 30×15
  (AB)C cost = 30 × 15 × 5 = 2,250
  Total = 18,000

Ordering 2: A(BC)
  BC cost = 35 × 15 × 5 = 2,625
  Result BC is 35×5
  A(BC) cost = 30 × 35 × 5 = 5,250
  Total = 7,875  <- MINIMUM
```

> **Answer: 7,875**

**General DP Recurrence:**
```
m[i][j] = minimum cost to multiply matrices i through j
m[i][i] = 0  (single matrix, no cost)

m[i][j] = min over k from i to j-1 of:
           m[i][k] + m[k+1][j] + p[i-1] * p[k] * p[j]

where matrix i has dimensions p[i-1] x p[i]
```

### 10.3 Longest Common Subsequence (LCS)

**Problem:** Find the longest subsequence common to both sequences.
- Subsequence: characters in order but not necessarily contiguous

**Recurrence:**
```
LCS[i][j] = 0                               if i=0 or j=0
LCS[i][j] = LCS[i-1][j-1] + 1              if X[i] == Y[j]
LCS[i][j] = max(LCS[i-1][j], LCS[i][j-1]) otherwise
```

**Example: LCS("ABCBDAB", "BDCABA")**
```
      ""  B  D  C  A  B  A
   ""  0  0  0  0  0  0  0
   A   0  0  0  0  1  1  1
   B   0  1  1  1  1  2  2
   C   0  1  1  2  2  2  2
   B   0  1  1  2  2  3  3
   D   0  1  2  2  2  3  3
   A   0  1  2  2  3  3  4
   B   0  1  2  2  3  4  4

LCS length = 4 (e.g., "BCBA" or "BDAB")
```

**Time:** O(m*n), **Space:** O(m*n)

### 10.4 0/1 Knapsack

**Problem:** n items with weights w[i] and values v[i], knapsack capacity W. Maximize total value.

```
dp[i][w] = max value using first i items with capacity w

dp[0][w] = 0  (no items)
dp[i][0] = 0  (no capacity)
dp[i][w] = dp[i-1][w]                         if w[i] > w (can't fit item i)
dp[i][w] = max(dp[i-1][w],                    otherwise
               v[i] + dp[i-1][w - w[i]])
```

**Time:** O(nW), **Space:** O(nW)

### 10.5 Longest Increasing Subsequence (LIS)

**Problem:** Find length of longest strictly increasing subsequence.

```
LIS[i] = length of LIS ending at index i

for each i:
  LIS[i] = 1 + max(LIS[j]) for all j < i where A[j] < A[i]
  (if no such j, LIS[i] = 1)

Answer = max(LIS[i]) for all i
```

**Example:**
```
Array: [10, 9, 2, 5, 3, 7, 101, 18]
LIS:   [ 1, 1, 1, 2, 2, 3,  4,  4]
Answer: 4  (sequence: 2, 3, 7, 101 or 2, 5, 7, 101)
```

**Time:** O(n²) DP, O(n log n) with patience sorting

### 10.6 DP Summary Table

| Problem | Recurrence | Time | Space |
|---------|-----------|------|-------|
| Fibonacci | F(n) = F(n-1) + F(n-2) | O(n) | O(1) |
| Matrix Chain | m[i][j] = min(m[i][k]+m[k+1][j]+p*q*r) | O(n³) | O(n²) |
| LCS | LCS[i][j] based on match | O(mn) | O(mn) |
| LIS | LIS[i] = 1 + max of previous | O(n²) | O(n) |
| 0/1 Knapsack | dp[i][w] = max(skip, take) | O(nW) | O(nW) |
| Edit Distance | dp[i][j] based on ops | O(mn) | O(mn) |
| Coin Change | dp[w] = min(dp[w-coin]+1) | O(nW) | O(W) |
| Floyd-Warshall | D[i][j] = min(D[i][j], D[i][k]+D[k][j]) | O(V³) | O(V²) |

---

## 11. Greedy Algorithms & Huffman Coding

### 11.1 Greedy Strategy
- Make locally optimal choice at each step
- Works when: optimal substructure + greedy choice property
- Greedy works for: Huffman, MST (Kruskal/Prim), Activity Selection, Fractional Knapsack
- **Fractional Knapsack is Greedy; 0/1 Knapsack requires DP**

### 11.2 Huffman Coding ⭐ (BUET Past Q — CRITICAL)

**[BUET MSC 21]:** Length of Huffman code for `"effervescence"`.

**Step 1: Count frequencies**
```
"effervescence" = e,f,f,e,r,v,e,s,c,e,n,c,e
Count:
  e: 5
  f: 2
  r: 1
  v: 1
  s: 1
  c: 2
  n: 1
Total characters = 13
```

**Step 2: Build Huffman Tree (greedy: always merge two lowest frequency nodes)**

Initial sorted: {n:1, r:1, v:1, s:1, f:2, c:2, e:5}

```
Iteration 1: Combine n(1) + r(1) = nr(2)
  Queue: {v:1, s:1, nr:2, f:2, c:2, e:5}

Iteration 2: Combine v(1) + s(1) = vs(2)
  Queue: {nr:2, vs:2, f:2, c:2, e:5}

Iteration 3: Combine nr(2) + vs(2) = nrvs(4)
  Queue: {f:2, c:2, nrvs:4, e:5}

Iteration 4: Combine f(2) + c(2) = fc(4)
  Queue: {nrvs:4, fc:4, e:5}

Iteration 5: Combine nrvs(4) + fc(4) = nrvsfc(8)
  Queue: {e:5, nrvsfc:8}

Iteration 6: Combine e(5) + nrvsfc(8) = root(13)
```

**Huffman Tree (0=left, 1=right):**
```
               root(13)
              /         \
           e(5)        nrvsfc(8)
            |           /      \
          code:1    nrvs(4)   fc(4)
                    /    \    /   \
                 nr(2)  vs(2) f(2) c(2)
                 / \    / \
               n(1) r(1) v(1) s(1)
```

**Codes (traversing tree):**
```
e:    1      (depth 1 -> 1 bit)
n:    0000   (depth 4 -> 4 bits)
r:    0001   (depth 4 -> 4 bits)
v:    0010   (depth 4 -> 4 bits)
s:    0011   (depth 4 -> 4 bits)
f:    010    (depth 3 -> 3 bits)
c:    011    (depth 3 -> 3 bits)
```

**Total encoded length:**
```
e: 5 chars × 1 bit  =  5 bits
n: 1 char  × 4 bits =  4 bits
r: 1 char  × 4 bits =  4 bits
v: 1 char  × 4 bits =  4 bits
s: 1 char  × 4 bits =  4 bits
f: 2 chars × 3 bits =  6 bits
c: 2 chars × 3 bits =  6 bits
                    --------
Total             = 33 bits
```

> **Answer: 33 bits** for encoding "effervescence"

**Properties of Huffman Code:**
- Prefix-free (no code is prefix of another) → uniquely decodable
- Optimal variable-length code
- More frequent characters get shorter codes
- Time complexity: O(n log n)

### 11.3 Activity Selection Problem

**Problem:** Select maximum number of non-overlapping activities.

**Greedy Rule:** Always pick the activity that **finishes earliest** (compatible with already selected activities).

```
Activities (sorted by finish time):
   Act:   1   2   3   4   5
   Start: 1   3   0   5   8
   End:   2   4   6   7   9

Select Act 1 (finish=2)
Next compatible (start >= 2): Act 2 (start=3) -> Select
Next compatible (start >= 4): Act 4 (start=5) -> Select
Next compatible (start >= 7): Act 5 (start=8) -> Select
Result: {1, 2, 4, 5} -> 4 activities
```

---

## 12. Divide and Conquer

### 12.1 Core Paradigm
1. **Divide** problem into smaller subproblems of same type
2. **Conquer** subproblems recursively (base case: solve directly)
3. **Combine** solutions to subproblems

### 12.2 Key D&C Algorithms

| Algorithm | Divide | Combine | T(n) Recurrence | Complexity |
|-----------|--------|---------|-----------------|------------|
| Merge Sort | Split in half | Merge O(n) | 2T(n/2)+O(n) | O(n log n) |
| Quick Sort | Partition by pivot | Nothing | T(k)+T(n-k-1)+O(n) | O(n log n) avg |
| Binary Search | Split in half | Return | T(n/2)+O(1) | O(log n) |
| Strassen | 7 subproblems size n/2 | Matrix sum O(n²) | 7T(n/2)+O(n²) | O(n^2.807) |

### 12.3 Key Difference from DP

| | D&C | DP |
|---|-----|-----|
| Subproblems | Independent | Overlapping |
| Repetition | Subproblems not repeated | Same subproblems computed multiple times |
| Approach | Always recurse | Store results to avoid recomputation |
| Example | Merge Sort | LCS, Knapsack |

---

## 13. NP-Completeness

### 13.1 Complexity Classes

```
Complexity Class Diagram:
+--------------------------------------+
|              NP-Hard                 |
|  +--------------------------------+  |
|  |         NP-Complete            |  |
|  |  +--------------------------+  |  |
|  |  |           NP             |  |  |
|  |  |  +--------------------+  |  |  |
|  |  |  |         P          |  |  |  |
|  |  |  +--------------------+  |  |  |
|  |  +--------------------------+  |  |
|  +--------------------------------+  |
+--------------------------------------+
(Assuming P != NP)
```

| Class | Definition | Examples |
|-------|-----------|----------|
| **P** | Solvable in polynomial time | Sorting, BST, Dijkstra, MST |
| **NP** | Verifiable in polynomial time | SAT, Clique, Knapsack decision |
| **NP-Complete** | In NP AND NP-Hard (hardest in NP) | SAT, 3-SAT, Vertex Cover, TSP decision |
| **NP-Hard** | At least as hard as NP-Complete (may not be in NP) | TSP optimization, Halting problem |

### 13.2 Key Relationships

- **P ⊆ NP** (everything solvable in poly time is also verifiable in poly time)
- **NP-Complete ⊆ NP ∩ NP-Hard**
- **P = NP?** — Greatest unsolved problem in CS. Most believe P ≠ NP.

### 13.3 Proving NP-Completeness

To prove problem C is NP-Complete:
1. Show **C ∈ NP** (give a polynomial-time verification algorithm)
2. Show **known NP-Complete problem ≤ₚ C** (polynomial reduction)

**Key reductions chain:**
```
Circuit SAT -> SAT -> 3-SAT -> Independent Set -> Vertex Cover -> Clique
                            -> Hamiltonian Cycle -> TSP (decision)
                            -> 3-Colorability
```

### 13.4 Famous NP-Complete Problems

| Problem | Description |
|---------|-------------|
| SAT | Boolean formula satisfiability |
| 3-SAT | 3-literal clause satisfiability |
| Clique | Find clique of size k |
| Independent Set | Find independent set of size k |
| Vertex Cover | Find vertex cover of size k |
| Hamiltonian Cycle | Visit all vertices exactly once |
| TSP (decision) | Tour of cost ≤ k? |
| 0/1 Knapsack | Select items with max value ≤ W |
| Graph Coloring | Color graph with k colors |
| Subset Sum | Find subset summing to target |

> **Key MCQ:** Fractional Knapsack ∈ P (greedy). 0/1 Knapsack ∈ NP-Complete.

---

## 14. BUET Past Questions — Solved

### Q1. [BUET MSC CSE 21] Matrix Chain Multiplication
> A=30×35, B=35×15, C=15×5. Min scalar multiplications for ABC?

**Solution (Section 10.2):**
- (AB)C = 15,750 + 2,250 = 18,000
- A(BC) = 2,625 + 5,250 = **7,875** ← minimum

**Answer: 7,875**

---

### Q2. [BUET MSC CSE 21] Algorithm Complexity
> Algorithm divides into 7 sub-problems of size n/2, combining in O(n²) time.

**Solution:** T(n) = 7T(n/2) + O(n²)
- a=7, b=2, log₂7 ≈ 2.807
- f(n) = n², compare: 2 < 2.807 → Case 1 of Master Theorem
- T(n) = **O(n^log₂7) ≈ O(n^2.807)**

**Answer: closer to O(n³)** (since 2.807 is between 2 and 3)

---

### Q3. [BUET MSC 21] Huffman Code
> Length of Huffman code for "effervescence"

**Solution (Section 11.2):** **Answer: 33 bits**

---

### Q4. [BUET MSC 17] Tree Reconstruction
> Inorder: D B E A F C | Preorder: A B D E C F → Draw the tree.

**Solution (Section 6.3):**
```
        A
      /   \
     B     C
    / \   /
   D   E F
```

---

### Q5. [BUET MSC 17] Max Heap Construction
> Draw max heap from a given array (e.g., [4, 10, 3, 5, 1])

**Solution (Section 7.2):** Apply BUILD-MAX-HEAP bottom-up → [10, 5, 3, 4, 1]

---

### Q6. [BUET MSC ICT 19] Adjacency Matrix
> Find adjacency matrix from given graph.

**Solution (Section 9.1):** For undirected graph, matrix is symmetric. A[i][j]=1 if edge exists.

---

### Q7. [BUET MSC CSE 21] Independent Set → Vertex Cover
> V has 11 vertices, maximum independent set has 5 vertices.

**Solution:** Min vertex cover = V \ (max independent set) = 11 - 5 = **6**

---

### Q8. [BUET MSC ICT 19] Binary Search Pseudocode
**Solution (Section 3.2):** Write iterative version with mid = lo + (hi-lo)/2.

---

## ⚡ Quick MCQ Cheat Sheet

### Sorting Facts
- **Only stable O(n log n):** Merge Sort
- **Best for linked list:** Merge Sort (no random access needed)
- **In-place + O(n log n) guaranteed:** Heap Sort
- **Worst case O(n²):** Quick Sort (sorted input with bad pivot)
- **Best for nearly sorted:** Insertion Sort
- **Lower bound for comparison sort:** Ω(n log n)
- **Fewest swaps:** Selection Sort (O(n) swaps)

### Trees
- **BST inorder traversal gives:** Sorted sequence
- **Height of complete binary tree with n nodes:** floor(log₂n)
- **Max nodes in binary tree of height h:** 2^(h+1) - 1
- **Min nodes in AVL tree of height h:** Fibonacci-related
- **AVL always:** O(log n) height — guaranteed

### Heaps
- **BUILD-HEAP time:** O(n) — NOT O(n log n)
- **Heap is NOT a BST** — no left/right ordering between siblings
- **Root of max-heap:** always maximum element
- **Heap index (1-based):** left child = 2i, right = 2i+1, parent = floor(i/2)

### Graphs
- **BFS finds shortest path in:** Unweighted graphs only
- **Dijkstra fails with:** Negative edge weights
- **Bellman-Ford time:** O(VE) — slower but handles negatives
- **Floyd-Warshall time:** O(V³) — all pairs
- **MST edges count:** Always V-1
- **Kruskal uses:** Union-Find (Disjoint Set)
- **Prim uses:** Priority Queue
- **Cycle detection:** DFS (back edge) or Union-Find
- **Topological sort:** Only on DAG (Directed Acyclic Graph)

### DP vs Greedy
- **DP:** Overlapping subproblems + optimal substructure
- **Greedy:** Greedy choice property (local = global optimal)
- **Fractional Knapsack:** Greedy O(n log n)
- **0/1 Knapsack:** DP O(nW)
- **Huffman:** Greedy O(n log n)
- **Activity Selection:** Greedy O(n log n)

### NP-Completeness
- **P ⊆ NP ⊆ NP-Hard**
- **NP-Complete = NP ∩ NP-Hard**
- Sorting ∈ P
- TSP decision ∈ NP-Complete
- **"NP" does NOT mean non-polynomial** — it means Non-deterministic Polynomial (verifiable in polynomial time)

### Common MCQ Traps
- Quick sort space = O(log n) stack, not O(1)
- BUILD-HEAP is O(n), not O(n log n)
- Dijkstra requires ALL edge weights non-negative
- BST worst case = O(n) (skewed/degenerate tree)
- Heap is complete binary tree, NOT a BST
- BFS uses Queue; DFS uses Stack
- Topological sort works ONLY on DAG (not on cyclic graphs)
- Bellman-Ford does V-1 iterations (not V)

---

## 📊 Algorithm Complexity Master Table

| Algorithm | Time (Best) | Time (Avg) | Time (Worst) | Space |
|-----------|------------|------------|--------------|-------|
| **Searching** | | | | |
| Linear Search | O(1) | O(n) | O(n) | O(1) |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) |
| **Sorting** | | | | |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) |
| **Trees** | | | | |
| BST Search | O(log n) | O(log n) | O(n) | O(1) |
| AVL Search | O(log n) | O(log n) | O(log n) | O(1) |
| **Heaps** | | | | |
| Build Heap | O(n) | O(n) | O(n) | O(1) |
| Heap Insert | O(log n) | O(log n) | O(log n) | O(1) |
| Heap Extract | O(log n) | O(log n) | O(log n) | O(1) |
| **Graphs** | | | | |
| BFS | — | — | O(V+E) | O(V) |
| DFS | — | — | O(V+E) | O(V) |
| Dijkstra | — | — | O((V+E) log V) | O(V) |
| Bellman-Ford | — | — | O(VE) | O(V) |
| Floyd-Warshall | — | — | O(V³) | O(V²) |
| Kruskal | — | — | O(E log E) | O(V) |
| Prim | — | — | O(E log V) | O(V) |
| **Dynamic Programming** | | | | |
| LCS | — | — | O(mn) | O(mn) |
| Matrix Chain | — | — | O(n³) | O(n²) |
| Knapsack (0/1) | — | — | O(nW) | O(nW) |
| LIS | — | — | O(n²) | O(n) |
| Floyd-Warshall | — | — | O(V³) | O(V²) |

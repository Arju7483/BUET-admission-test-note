# 🤖 Comprehensive AI & Machine Learning Guide
> **A deep, in-depth reference covering State Space Search, Neural Networks, SVMs, and Core ML Concepts**
> *References: Russell & Norvig (AIMA), Goodfellow et al. (Deep Learning), Bishop (PRML), Géron (Hands-On ML)*

---

## 📚 Table of Contents

1. [State Space Search](#1-state-space-search)
2. [Blind/Uninformed Search](#2-blinduninformed-search)
3. [Heuristics & Informed Search](#3-heuristics--informed-search)
4. [Neural Networks](#4-neural-networks)
5. [Support Vector Machine (SVM)](#5-support-vector-machine-svm)
6. [Types of Machine Learning](#6-types-of-machine-learning)
7. [Classification Models](#7-classification-models)
8. [Model Performance & Generalization](#8-model-performance--generalization)

---

# 1. State Space Search

## 1.1 What is State Space Search?

State-space search is one of the **most fundamental paradigms in Artificial Intelligence**. It is a method of solving problems by treating the problem as a **navigation through a space of possible world states** — looking for a path from a starting configuration to a goal configuration.

**Intuition:** Imagine you are lost in a city. Your current location is the **initial state**, your destination is the **goal state**, and the streets and turns you can take are the **actions**. Finding your way home is a state-space search problem.

### Why is it Important?
- It provides a **universal framework** to model a huge variety of AI problems.
- Game playing (Chess, Go), robot navigation, scheduling, planning, puzzles — all can be expressed as state-space search.
- The algorithms developed here form the backbone of many modern AI systems.

---

## 1.2 Formulation of State Space Search

Before an AI can search, it must **formalize** the problem. This means defining it mathematically so an algorithm can work on it.

A state-space search problem is defined by a **7-tuple**:

```
Problem = (S, s0, A, T, G, C, h)
```

| Component | Symbol | Description |
|-----------|--------|-------------|
| **State Space** | S | The complete set of all possible states |
| **Initial State** | s0 in S | Where the agent starts |
| **Actions** | A(s) | Set of valid actions from state s |
| **Transition Model** | T(s, a) -> s' | What state results from action a in state s |
| **Goal Test** | G(s) -> {True, False} | Checks if state s is a goal state |
| **Path Cost** | C(path) | Numeric cost assigned to a path |
| **Heuristic** (optional) | h(s) | Estimated cost from s to goal (for informed search) |

### Detailed Breakdown:

#### State Space (S)
The state space is the **universe of all possible configurations** the environment can be in. It can be:
- **Finite**: Like an 8-puzzle (9! / 2 = 181,440 reachable states)
- **Infinite**: Like a continuous robot navigation space

#### Initial State (s0)
The configuration where the agent begins. Everything else is discovered from here.

#### Actions / Operators
These are the **legal moves** the agent can make. For each state s, A(s) gives the set of applicable actions. Actions must be:
- **Deterministic** (in classical search)
- **Complete** (all possible transitions are defined)

#### Transition Model T(s, a) = s'
Also called the **successor function**. Given a state s and an action a, it returns the new state s'. Together with the state space and initial state, this defines the **state space graph**.

#### Goal Test G(s)
A function that returns `True` if state s is a goal. It can be:
- **Explicit**: A specific state (e.g., pieces in a certain configuration)
- **Implicit**: A condition (e.g., "find any state where no conflicts exist")

#### Path Cost C
Assigns a **numeric cost** to each path (sequence of actions). Goal: Find the **minimum cost** path.

---

## 1.3 Examples of State Space Search

### Example 1: The 8-Puzzle

```
Initial State:        Goal State:
+---+---+---+        +---+---+---+
| 7 | 2 | 4 |        | 1 | 2 | 3 |
+---+---+---+        +---+---+---+
| 5 |   | 6 |  --->  | 4 | 5 | 6 |
+---+---+---+        +---+---+---+
| 8 | 3 | 1 |        | 7 | 8 |   |
+---+---+---+        +---+---+---+
```

- **State**: A 3x3 grid configuration (the position of all 8 tiles + blank)
- **Initial State**: Some scrambled configuration
- **Actions**: Move blank Up, Down, Left, Right
- **Goal Test**: Is the configuration equal to the goal layout?
- **Path Cost**: Number of moves (each move costs 1)
- **State Space Size**: 9! / 2 = 181,440 reachable states

### Example 2: Route Finding (Romania Map)

The classic Romania map problem from Russell & Norvig:

```
     Oradea
    /      \
Zerind   Sibiu
   |    /   \
  Arad  Fagaras  Rimnicu
              |
          Bucharest (Goal)
```

- **State**: Current city
- **Initial State**: Arad
- **Goal State**: Bucharest
- **Actions**: Drive to adjacent city
- **Path Cost**: Road distance (km)
- **Optimal solution**: Arad -> Sibiu -> Rimnicu -> Pitesti -> Bucharest (418 km)

### Example 3: Vacuum Cleaner World

```
+--------+--------+
|  Dirty |  Clean |   Agent is at Left
|  [A]   |  [B]   |
+--------+--------+
```

- **State**: Position of agent (L or R) + dirt status of each room = 2 x 2^2 = 8 total states
- **Actions**: Left, Right, Suck
- **Goal**: All rooms clean

### Example 4: N-Queens Problem

- **State**: Arrangement of N queens on NxN board
- **Goal**: No queen attacks another (no same row, column, or diagonal)
- **State Space**: N^N configurations

---

## 1.4 Classification of State Space Search

```
State Space Search
|
+-- Uninformed Search (Blind Search)
|   | (No knowledge about the goal beyond problem definition)
|   +-- Breadth-First Search (BFS)
|   +-- Depth-First Search (DFS)
|   +-- Depth-Limited Search (DLS)
|   +-- Iterative Deepening Search (IDS)
|   +-- Uniform Cost Search (UCS)
|   +-- Bidirectional Search
|
+-- Informed Search (Heuristic Search)
    | (Uses heuristic knowledge to guide search)
    +-- Greedy Best-First Search
    +-- A* Search
    +-- Iterative Deepening A* (IDA*)
    +-- Local Search Algorithms
        +-- Hill Climbing
        +-- Simulated Annealing
        +-- Genetic Algorithms
```

### Classification by Search Space:
- **Tree Search**: Does not track visited states, may revisit
- **Graph Search**: Maintains a "closed list" of visited states, avoids revisiting

### Classification by Goal:
- **Single-state problem**: Agent knows its exact state
- **Multiple-state problem**: Agent may be in one of several possible states
- **Contingency problem**: Agent uses observations during execution
- **Exploration problem**: State space is unknown

---

## 1.5 Search Strategy & General Algorithm of Goal-Finding Agent

### What Makes a Good Search Strategy?

A search strategy is evaluated on four criteria:

| Criterion | Meaning |
|-----------|---------|
| **Completeness** | If a solution exists, is it guaranteed to be found? |
| **Optimality** | Is the found solution the least-cost solution? |
| **Time Complexity** | How long does it take? (nodes generated) |
| **Space Complexity** | How much memory is needed? (max nodes stored) |

### The General Search Algorithm (TREE-SEARCH)

```
function TREE-SEARCH(problem, strategy) returns solution or failure:
    initialize frontier with the initial state of problem
    
    loop:
        if frontier is EMPTY then
            return FAILURE
        
        choose a leaf node from frontier based on STRATEGY
        remove it from frontier
        
        if node contains a GOAL STATE then
            return the SOLUTION (path from start to this node)
        
        EXPAND the chosen node
        add resulting nodes to frontier
```

### Graph Search (More Practical)

```
function GRAPH-SEARCH(problem, strategy) returns solution or failure:
    initialize frontier with initial state
    initialize explored_set = {} (empty set)
    
    loop:
        if frontier is EMPTY then return FAILURE
        
        choose leaf node from frontier (via STRATEGY)
        
        if node is GOAL STATE then
            return SOLUTION
        
        add node to explored_set
        
        for each child of EXPAND(node):
            if child NOT in explored_set AND NOT in frontier:
                add child to frontier
```

### The Node Data Structure

Each node in the search tree stores:

```python
class Node:
    state        # The state in the problem state space
    parent       # Node that generated this node (for path reconstruction)
    action       # Action applied to parent to generate this node
    path_cost    # Cost from root to this node: g(n)
    depth        # Depth in the search tree
```

---

## 1.6 State Graph vs Search Tree

These two concepts are **commonly confused** but are fundamentally different.

### State Graph

A **state graph** represents the abstract structure of the problem:
- **Nodes** = States
- **Edges** = State transitions (actions)
- A state appears **only once**, no matter how many paths lead to it

```
State Graph Example:
     A
    / \
   B   C
  / \ / \
 D   E   F
      \ /
       G (Goal)

E appears once even though B and C can both reach it.
```

### Search Tree

A **search tree** is generated during the search process:
- **Root** = Initial state
- The **same state can appear multiple times** (via different paths)
- Represents the "exploration history"

```
Search Tree (BFS from A):
         A              Level 0
        / \
       B   C            Level 1
      /\ / \
     D  E  E  F         Level 2 (E appears twice!)
        |  |
        G  G            Level 3 (G appears twice!)
```

**Key Difference**: In the state graph, E is one node. In the search tree, E may appear multiple times.

### Why does this matter?
- **Tree Search** may get stuck in **infinite loops** (if the graph has cycles)
- **Graph Search** uses an **explored set** to avoid revisiting states

---

## 1.7 State Representation

How we **represent** states is crucial for algorithm efficiency.

### Explicit State Representation
Enumerate the complete state explicitly:
- **8-puzzle**: `[[1,2,3],[4,5,6],[7,8,0]]` (0 = blank)
- **Vacuum world**: `('L', True, False)` = (location, left_dirty, right_dirty)

### Factored State Representation
State is described by a set of variables and their values:
- **N-Queens**: `queens = [2, 4, 6, 0, 3, 1, 7, 5]` (queen in column i is at row queens[i])

### Example: 8-puzzle as tuple

```python
# State as a tuple (hashable for explored set, immutable)
state = (7, 2, 4, 5, 0, 6, 8, 3, 1)  # 0 = blank tile

def get_successors(state):
    successors = []
    blank = state.index(0)
    row, col = blank // 3, blank % 3
    moves = []
    if row > 0: moves.append(-3)  # Up
    if row < 2: moves.append(+3)  # Down
    if col > 0: moves.append(-1)  # Left
    if col < 2: moves.append(+1)  # Right
    for move in moves:
        new_state = list(state)
        new_state[blank], new_state[blank+move] = new_state[blank+move], new_state[blank]
        successors.append(tuple(new_state))
    return successors
```

---

# 2. Blind/Uninformed Search

**Uninformed search** (blind search) algorithms have **no additional information** about states beyond the problem definition. They can only distinguish a goal state from a non-goal state.

## 2.1 Node Expanding & Node Exploration

### Node Exploration (Selection)
**Exploring a node** means **selecting it from the frontier** (removing it from the open list to process it).

### Node Expansion
**Expanding a node** means **applying all applicable actions** to generate its successor nodes, which are then added to the frontier.

```
EXPAND Process:
                    [N] <- Node being expanded
                   / | \
                  /  |  \
               [N1] [N2] [N3] <- Successors added to frontier
```

### The Frontier & Explored Set

```
FRONTIER (Open List):
- Contains nodes discovered but not yet expanded
- Data structure varies by algorithm (queue, stack, priority queue)

EXPLORED SET (Closed List):
- Contains states already expanded
- Prevents re-expansion, avoids infinite loops
- Hash set for O(1) lookup
```

### Life Cycle of a Node:

```
State discovered -> Node created -> Added to FRONTIER
       |
Node selected from FRONTIER (Explored/Selected)
       |
Node EXPANDED -> Successors generated
       |
Node moved to EXPLORED SET
       |
Successors added to FRONTIER (if not already explored)
```

---

## 2.2 Breadth-First Search (BFS)

### Core Idea
BFS explores the search tree **level by level**. It visits all nodes at depth d before any node at depth d+1.

### Data Structure: FIFO Queue

```
function BFS(problem) returns solution or failure:
    node = Node(state=problem.INITIAL_STATE)
    
    if problem.GOAL_TEST(node.state):
        return SOLUTION(node)
    
    frontier = Queue()   # FIFO queue
    frontier.enqueue(node)
    explored = set()
    
    while frontier is not empty:
        node = frontier.dequeue()  # Remove from front
        explored.add(node.state)
        
        for each child in EXPAND(problem, node):
            if child.state not in explored and child not in frontier:
                if problem.GOAL_TEST(child.state):
                    return SOLUTION(child)
                frontier.enqueue(child)  # Add to back
    
    return FAILURE
```

### BFS Visualization (b=2):

```
Level 0:            [S]
                   /   \
Level 1:         [A]   [B]
                / \   / \
Level 2:      [C][D] [E][F]
              /\
Level 3:    [G][H]...  <- Goal found!

Frontier progression:
[S] -> [A,B] -> [B,C,D] -> [C,D,E,F] -> [D,E,F,G,H] -> Goal!
```

### Complexity Analysis

Let b = branching factor, d = depth of shallowest goal, m = max tree depth:

| Metric | BFS |
|--------|-----|
| **Completeness** | Yes (if b is finite) |
| **Optimality** | Yes* (if all step costs equal) |
| **Time Complexity** | O(b^d) |
| **Space Complexity** | O(b^d) — MAIN WEAKNESS |

### Why is BFS Memory-Intensive?

```
At depth d, BFS has approximately b^d nodes in the frontier.

b=10, d=12:
Nodes generated: 10^12 = 1,000,000,000,000
At 1KB/node -> 1,000 TB of memory!

This is why BFS is impractical for deep problems!
```

---

## 2.3 Depth-First Search (DFS)

### Core Idea
DFS explores as **deep as possible** along a single path before backtracking.

### Data Structure: LIFO Stack (or recursion)

```
function RECURSIVE-DFS(node, problem) returns solution or failure:
    if problem.GOAL_TEST(node.state):
        return SOLUTION(node)
    
    for each action in problem.ACTIONS(node.state):
        child = CHILD-NODE(problem, node, action)
        result = RECURSIVE-DFS(child, problem)
        if result != FAILURE:
            return result
    
    return FAILURE
```

### DFS Visualization (b=2):

```
            [S]
           /   \
         [A]   [B]
        / \   / \
      [C] [D][E] [F]
      /\
    [G][H]

DFS order: S -> A -> C -> G (backtrack) -> H -> D -> B -> E -> F
           Goes deep first before backtracking!
```

### Complexity Analysis

| Metric | DFS |
|--------|-----|
| **Completeness** | No (can go infinitely deep or loop) |
| **Optimality** | No |
| **Time Complexity** | O(b^m) |
| **Space Complexity** | O(bm) <- MAIN ADVANTAGE |

### DFS Memory Advantage

```
For b=10, m=12:
DFS frontier: at most 120 nodes (the current path)
BFS frontier: up to 10^12 nodes

DFS uses ~8 million times LESS memory!
```

### DFS Failure Cases:

```
Problem 1: Loops in state graph
A -> B -> C -> A -> B -> C -> ... (infinite loop!)

Problem 2: Deep paths away from goal
S -> A -> A1 -> A2 -> A3 -> ... (explores entire left branch first)
Goal is at S -> B (depth 2) -- DFS might explore for a long time!
```

---

## 2.4 Iterative Deepening Search (IDS)

### Motivation
- BFS: Complete, Optimal, but O(b^d) space (bad)
- DFS: O(bd) space (good), but Incomplete, Not Optimal (bad)

**IDS combines the best of both!**

### Core Idea
Run a **Depth-Limited Search (DLS)** with increasing depth limits (0, 1, 2, 3, ...) until the goal is found.

### DLS (Building Block)

```
function DEPTH-LIMITED-SEARCH(problem, limit):
    return RECURSIVE-DLS(Node(problem.INITIAL_STATE), problem, limit)

function RECURSIVE-DLS(node, problem, limit):
    if problem.GOAL_TEST(node.state):
        return SOLUTION(node)
    
    if limit == 0:
        return CUTOFF    # Hit depth limit
    
    cutoff_occurred = False
    for each action in problem.ACTIONS(node.state):
        child = CHILD-NODE(problem, node, action)
        result = RECURSIVE-DLS(child, problem, limit - 1)
        if result == CUTOFF:
            cutoff_occurred = True
        elif result != FAILURE:
            return result
    
    return CUTOFF if cutoff_occurred else FAILURE
```

### IDS Algorithm

```
function ITERATIVE-DEEPENING-SEARCH(problem):
    for depth = 0 to INFINITY:
        result = DEPTH-LIMITED-SEARCH(problem, depth)
        if result != CUTOFF:
            return result  # Either SOLUTION or FAILURE
```

### IDS Visualization (b=2, goal at depth 3):

```
Iteration 1 (limit=0): [S]
Iteration 2 (limit=1): [S] -> [A], [B]
Iteration 3 (limit=2): [S] -> [A] -> [C],[D] -> [B] -> [E],[F]
Iteration 4 (limit=3): [S] -> [A] -> [C] -> [G]! <- Goal found!

Note: S, A, B are re-explored each iteration.
Why is this OK? See below!
```

### Why Re-exploration is Cheap

For branching factor b, goal at depth d:

```
Total nodes expanded by IDS:
N_IDS = (d+1)b^0 + d*b^1 + (d-1)*b^2 + ... + 1*b^d = O(b^d)

For b=10, d=5:
BFS nodes:  111,111
IDS nodes:  123,456  (only ~11% more!)

The wasted re-exploration adds only a small constant factor!
```

### Complexity Analysis

| Metric | IDS |
|--------|-----|
| **Completeness** | Yes |
| **Optimality** | Yes* (if step costs equal) |
| **Time Complexity** | O(b^d) |
| **Space Complexity** | O(bd) |

> **IDS is generally the preferred uninformed search algorithm for large state spaces when solution depth is unknown.**

---

## 2.5 Bidirectional Search

### Core Idea
Run **two simultaneous searches**:
1. **Forward search**: From initial state toward goal
2. **Backward search**: From goal state toward initial state

Stop when the two frontiers **meet** (intersect).

```
Forward:  Start -> A -> B -> (MEET) <- D <- E <- Goal :Backward

The path: Start -> ... -> MEET -> ... -> Goal
```

### Why is it Faster?

If the solution path has depth d:
```
Unidirectional: O(b^d)  nodes
Bidirectional:  O(b^(d/2)) nodes  <-- exponential speedup!

b=10, d=10:
Unidirectional: 10^10 = 10,000,000,000 nodes
Bidirectional:  2 x 10^5 = 200,000 nodes   (50,000x speedup!)
```

### Complexity Analysis

| Metric | Bidirectional BFS |
|--------|------------------|
| **Completeness** | Yes |
| **Optimality** | Yes* |
| **Time Complexity** | O(b^(d/2)) |
| **Space Complexity** | O(b^(d/2)) |

### Challenges:
1. Requires knowing predecessors (inverse transition model)
2. Multiple goal states require backward search from all simultaneously
3. Careful detection of the meeting point is needed

---

## 2.6 Comparison of Uninformed Search Algorithms

| Algorithm | Complete | Optimal | Time | Space | Data Structure |
|-----------|----------|---------|------|-------|---------------|
| **BFS** | Yes | Yes* | O(b^d) | O(b^d) | FIFO Queue |
| **DFS** | No | No | O(b^m) | O(bm) | LIFO Stack |
| **DLS** | No (l<d) | No | O(b^l) | O(bl) | Stack + limit |
| **IDS** | Yes | Yes* | O(b^d) | O(bd) | Stack (repeated) |
| **UCS** | Yes | Yes | O(b^(C*/e)) | O(b^(C*/e)) | Priority Queue (g) |
| **Bidir.** | Yes | Yes* | O(b^(d/2)) | O(b^(d/2)) | Two Queues |

*Optimal only when all step costs are equal.
Variables: b=branching factor, d=solution depth, m=max tree depth, l=depth limit, C*=optimal cost, e=min step cost

---

# 3. Heuristics & Informed Search

## 3.1 What is a Heuristic?

A **heuristic** (from Greek "heuriskein" = to discover) is **problem-specific knowledge** that estimates the cost/distance to the goal from a given state.

```
h(n) = estimated cost from state(n) to nearest goal
h(goal) = 0  (always)
```

### Example Heuristics for 8-Puzzle:

**h1 — Misplaced Tiles**: Count tiles not in their goal position.

**h2 — Manhattan Distance**: Sum of horizontal + vertical distances each tile must travel.

```
Current:          Goal:
1 2 3             1 2 3
4 _ 5    vs       4 5 6
7 8 6             7 8 _

Misplaced tiles: 5, 6, 8 -> h1 = 3
Manhattan: 5 is 1 away, 6 is 1 away, 8 is 0 away -> h2 = 2
```

Note: h2 >= h1 for all states -> h2 dominates h1 (always better or equal).

---

## 3.2 Greedy Best-First Search

### Evaluation Function
```
f(n) = h(n)    (only heuristic, ignores past cost g(n))
```

### Algorithm

```
function GREEDY-BEST-FIRST(problem, h):
    frontier = PriorityQueue(order_by=h)  # Min-heap by h(n)
    frontier.add(Node(problem.INITIAL_STATE))
    explored = set()
    
    while frontier is not empty:
        node = frontier.pop()  # Node with minimum h(n)
        
        if problem.GOAL_TEST(node.state):
            return SOLUTION(node)
        
        explored.add(node.state)
        
        for child in EXPAND(problem, node):
            if child.state not in explored:
                frontier.add(child)
    
    return FAILURE
```

### Why Greedy Fails at Optimality

```
        Start
       /     \
    [h=5]   [h=1]   <- Greedy picks h=1 (closer to goal)
     |         |
   [GOAL]   [dead]   <- But this leads to dead-end!

Greedy got stuck! The optimal path was through h=5.
```

Greedy is like always walking toward where you THINK the summit is, without tracking how far you've already walked.

### Properties

| Property | Greedy |
|----------|--------|
| Complete | No (can loop) |
| Optimal | No |
| Time | O(b^m) worst case |
| Space | O(b^m) |

---

## 3.3 A* Search

### Motivation
A* combines:
- **UCS** (tracks actual cost g(n)): Optimal but blind
- **Greedy** (tracks heuristic h(n)): Directed but not optimal

### Evaluation Function

```
f(n) = g(n) + h(n)

g(n) = actual cost from start to n (path cost so far)
h(n) = estimated cost from n to nearest goal (heuristic)
f(n) = estimated total cost of cheapest solution through n
```

### Algorithm

```
function A_STAR(problem, h):
    frontier = PriorityQueue(order_by=f)  # Min-heap by f(n) = g+h
    frontier.add(Node(state=problem.INITIAL_STATE, g=0))
    explored = set()
    
    while frontier is not empty:
        node = frontier.pop()  # Node with minimum f(n)
        
        if problem.GOAL_TEST(node.state):
            return SOLUTION(node)
        
        explored.add(node.state)
        
        for child in EXPAND(problem, node):
            child.g = node.g + step_cost(node, child)
            child.f = child.g + h(child.state)
            
            if child.state not in explored:
                if child not in frontier:
                    frontier.add(child)
                elif child.f < frontier[child].f:
                    frontier.replace(child)  # Update with better path
    
    return FAILURE
```

### A* Example: Romania

```
h_SLD (straight-line distance to Bucharest):
  Arad=366, Sibiu=253, Rimnicu=193, Pitesti=98, Bucharest=0

Step-by-step f-values:
1. frontier = {Arad: f=0+366=366}
2. Expand Arad: Sibiu f=140+253=393, Zerind f=75+374=449, ...
3. Expand Sibiu: Rimnicu f=220+193=413, Fagaras f=239+176=415
4. Expand Rimnicu: Pitesti f=317+98=415
5. Expand Fagaras: Bucharest f=450+0=450
6. Expand Pitesti: Bucharest f=418+0=418
7. Select Bucharest(418) -> OPTIMAL SOLUTION!

Path: Arad -> Sibiu -> Rimnicu -> Pitesti -> Bucharest = 418 km
```

### A* vs Greedy vs UCS

| Algorithm | f(n) | Optimal | Directed |
|-----------|------|---------|---------|
| UCS | g(n) | Yes | No |
| Greedy | h(n) | No | Yes |
| A* | g(n) + h(n) | Yes | Yes |

---

## 3.4 Proof of Optimality of A*

### Theorem
**A* using an admissible heuristic h is optimal.**

### Proof (by contradiction)

Let:
- C* = cost of the optimal solution
- G = optimal goal node
- G2 = a suboptimal goal node with g(G2) > C*
- n = a node on the optimal path from start to G, currently on the frontier

**Assume A* returns suboptimal goal G2.**

**Step 1**: Since G has not been found yet, there exists node n on the frontier that lies on the optimal path to G.

**Step 2**: Since h is admissible: h(n) <= h*(n)
Therefore:
```
f(n) = g(n) + h(n) <= g(n) + h*(n) = C*
```

**Step 3**: Since G2 is a goal state: h(G2) = 0, so:
```
f(G2) = g(G2) = g(G2) > C*
```

**Step 4**: From Steps 2 and 3:
```
f(n) <= C* < f(G2)
```

**Step 5**: A* selects nodes by minimum f-value. Since f(n) < f(G2), A* would select n before G2.

**Step 6**: Expanding n continues along the optimal path, eventually finding G with f(G) = C* < f(G2).

**Contradiction!** A* cannot select G2 before expanding the optimal path nodes. Therefore A* must return the optimal solution G. **QED**

### Consistency (for Graph Search Optimality)

```
A heuristic h is CONSISTENT (monotone) if:
h(n) <= c(n, a, n') + h(n')    for all n, action a, and successor n'

(Triangle inequality for heuristics)

If h is consistent:
- f(n) values are non-decreasing along any path
- Once a node is expanded, optimal path to it is found
- No need to re-add nodes to frontier

Every consistent heuristic is admissible, but not vice versa.
```

---

## 3.5 Admissible Heuristics & Choice of Heuristics

### Admissibility

```
h is ADMISSIBLE if: for all n, h(n) <= h*(n)
where h*(n) = true minimum cost from n to goal

An admissible heuristic NEVER OVERESTIMATES. It is "optimistic."
```

h(n) = 0 is always admissible (but gives no information — A* degenerates to UCS).

### How to Construct Admissible Heuristics: Relaxed Problems

The best method: solve a **relaxed version** of the problem (fewer constraints):

```
Original 8-puzzle constraints:
1. Tiles can only move to adjacent cells
2. Only the blank cell can be filled

Relaxation 1 (remove constraint 2):
"Tiles can move through each other" -> Manhattan Distance (h2)

Relaxation 2 (remove both constraints):
"Tiles can teleport anywhere" -> Misplaced Tiles (h1)

Optimal cost of relaxed problem <= Optimal cost of original problem
-> Relaxed-problem solutions are admissible heuristics!
```

### Heuristic Dominance

If h2(n) >= h1(n) for all n, and both are admissible, then **h2 dominates h1**.
A dominant heuristic is always better (expands fewer nodes):

```
For 8-puzzle: Manhattan (h2) >= Misplaced Tiles (h1) for all states
-> A* with h2 expands fewer nodes than A* with h1
-> Always prefer the dominant heuristic!
```

### The Max Heuristic

If you have multiple admissible heuristics h1, h2, ...:
```
h_max(n) = max(h1(n), h2(n), ...)

h_max is also admissible and dominates all individual heuristics!
```

### Pattern Database Heuristics

Precompute the exact cost to solve subproblems and use as heuristics:
- Disjoint pattern databases for 15-puzzle can reduce search by orders of magnitude

---

## 3.6 IDA* (Iterative Deepening A*)

### Motivation

**A\*** is optimal and complete, but has a critical weakness: **memory**. A\* keeps ALL generated nodes in memory (the frontier + explored set). For a state space of branching factor b at depth d, A\* uses O(b^d) space — the same exponential blowup as BFS.

**IDA\*** (Iterative Deepening A\*, Korf 1985) solves this by combining:
- The **optimal path guarantee** of A\* (via admissible heuristics)
- The **linear space usage** of IDS (via depth-first exploration)

It is the most widely used algorithm for large-scale optimal heuristic search.

### Core Idea

Instead of using a priority queue like A\*, IDA\* uses **depth-first search** with a **cost threshold** based on the f-value (f = g + h).

At each iteration:
1. Run DFS, but **prune any node** whose f(n) = g(n) + h(n) **exceeds the threshold**
2. After each iteration, set the new threshold = **minimum f-value that was pruned**
3. Repeat until the goal is found

```
Iteration 1: threshold = f(start) = h(start)
             DFS, prune if f(n) > threshold
             -> Some nodes pruned, record min pruned f-value

Iteration 2: threshold = min pruned f from iteration 1
             DFS again with new threshold
             -> More of the search tree explored

...

Iteration k: threshold = C* (optimal solution cost)
             DFS finds the goal!
```

### IDA* Algorithm

```
function IDA_STAR(problem, h):
    threshold = h(problem.INITIAL_STATE)   <- start with f(start) = 0 + h(start)
    path = [problem.INITIAL_STATE]         <- current path (stack)
    
    loop:
        result = SEARCH(path, g=0, threshold, problem, h)
        
        if result == FOUND:
            return path                    <- solution found!
        if result == INFINITY:
            return FAILURE                 <- no solution exists
        
        threshold = result                 <- update threshold to min exceeded f-value
    end loop


function SEARCH(path, g, threshold, problem, h):
    node = path.last()
    f = g + h(node)
    
    if f > threshold:
        return f                           <- pruned! Return this f-value
    
    if problem.GOAL_TEST(node):
        return FOUND
    
    min_exceeded = INFINITY
    
    for each action in problem.ACTIONS(node):
        child = RESULT(node, action)
        step = STEP_COST(node, action, child)
        
        if child NOT in path:              <- avoid cycles (simple check)
            path.append(child)
            result = SEARCH(path, g + step, threshold, problem, h)
            
            if result == FOUND:
                return FOUND
            if result < min_exceeded:
                min_exceeded = result      <- track minimum f that was pruned
            
            path.pop()                     <- backtrack
    
    return min_exceeded
```

### IDA* Step-by-Step Example (8-Puzzle)

Using Manhattan Distance heuristic (h2):

```
Initial state:         Goal state:
1  2  3                1  2  3
4  _  6     ->         4  5  6
7  5  8                7  8  _

h(start) = Manhattan distances:
  5 is at (2,1), goal is (1,1) -> |2-1|+|1-1| = 1
  6 is at (1,2), goal is (1,2) -> 0
  8 is at (2,2), goal is (2,1) -> 1
  h(start) = 2

Iteration 1: threshold = 0 + 2 = 2
  Expand start (f=2): prune children with f > 2
  ...some branches pruned, min exceeded f = 4

Iteration 2: threshold = 4
  Deeper DFS, prune f > 4
  ...min exceeded f = 6

Iteration 3: threshold = 6
  Even deeper DFS
  ...GOAL FOUND! Return solution path.
```

### How IDA* Avoids Redundant States

The simple cycle check `if child NOT in path` only checks the **current path** (ancestors), not all visited states globally. This uses O(d) space but may revisit states reachable via different paths (unlike A\* which has the full explored set).

**Trade-off**: IDA\* may re-expand some states, but saves enormous amounts of memory.

### Complexity Analysis

| Metric | IDA* |
|--------|------|
| **Completeness** | Yes (if h is admissible and step costs > 0) |
| **Optimality** | Yes (if h is admissible) |
| **Time Complexity** | O(b^d) — same as A* in most cases |
| **Space Complexity** | O(d) — linear! (only stores current path) |

### Proof of Optimality of IDA*

**Claim**: IDA\* with an admissible heuristic h returns the optimal solution.

**Proof sketch**:
1. IDA\* starts with threshold = h(s₀). Since h is admissible, h(s₀) ≤ C* → the optimal solution path is NOT pruned in the first iteration (all nodes on the optimal path have f ≤ C*).
2. The threshold only increases to the minimum exceeded f-value. Since f(n) = g(n) + h(n) ≤ C* for all nodes on the optimal path (by admissibility), the optimal path nodes are never pruned until the threshold reaches C*.
3. When threshold = C*, IDA\* will find the goal node G with f(G) = C* via DFS, and return the path.
4. IDA\* always selects the **smallest new threshold** → the threshold sequence is strictly increasing toward C\*, guaranteeing that the first goal found has cost = C*. ∎

### IDA* vs A* — Detailed Comparison

| Feature | A* | IDA* |
|---------|-----|------|
| **Space** | O(b^d) — exponential ❌ | O(d) — linear ✅ |
| **Time** | O(b^d) | O(b^d) (may re-expand) |
| **Re-expansion** | None (explored set) | Yes (states on different paths) |
| **Optimal** | Yes (admissible h) | Yes (admissible h) |
| **Complete** | Yes | Yes |
| **Data structure** | Priority Queue (heap) | Stack (recursion) |
| **Overhead** | Hash table lookups | Re-computation of f-values |
| **Best for** | Moderate state spaces (fits in memory) | Large state spaces (memory-limited) |

### IDA* vs IDS

| Feature | IDS | IDA* |
|---------|-----|------|
| **Threshold type** | Depth limit (integer) | f-value limit (real number) |
| **Uses heuristic** | No | Yes (informed search) |
| **Optimality** | Yes* (unit costs) | Yes (any admissible h) |
| **Efficiency** | Blind | Directed toward goal |
| **Nodes expanded** | More (no heuristic pruning) | Fewer (h prunes bad paths) |

### When to Use IDA*

- **Large state spaces** where A\* runs out of memory
- **Optimal solution required** (not just any solution)
- **Good admissible heuristic** available
- Classic applications: 15-puzzle, Rubik's Cube, robot path planning

### Variants and Improvements

**RBFS (Recursive Best-First Search)**: Tracks the f-value of the best alternative path at each node. More memory-efficient than A\*, slightly better than IDA\* (avoids some re-expansions).

**MA\* / SMA\* (Simplified Memory-Bounded A\*)**: Uses all available memory, discards least-promising nodes when full. Best of both worlds between A\* and IDA\*.

**IDA\*-CR (with Cycle Restriction)**: Enhanced cycle detection without full explored set.

---

# 4. Neural Networks

## 4.1 Definition & Properties

### Definition
An **Artificial Neural Network (ANN)** is a computational model **inspired by biological neural networks** (the brain). It consists of interconnected nodes ("neurons") organized in layers that learn by adjusting connection weights.

### Biological vs Artificial Neuron

```
BIOLOGICAL NEURON:
Dendrites --> [Soma/Cell Body] --> Axon --> next neuron
(inputs)      (integrates signals,
               fires if threshold exceeded)

ARTIFICIAL NEURON:
x1 -(w1)-+
x2 -(w2)-+
x3 -(w3)-+--[Sum + bias]--[Activation f]--> output y
...      |
xn -(wn)-+

y = f(w1*x1 + w2*x2 + ... + wn*xn + b)
```

### Key Properties of Neural Networks

1. **Universal Approximation**: A single hidden layer with enough neurons can approximate any continuous function.

2. **Distributed Representation**: Knowledge stored across many weights, not in a single location.

3. **Parallel Processing**: All neurons in a layer compute simultaneously.

4. **Adaptivity**: Weights adjusted through learning (training).

5. **Non-linearity**: Activation functions enable learning of complex patterns.

6. **Generalization**: Can generalize from seen examples to unseen data.

7. **Fault Tolerance**: Degrades gracefully with some damage (like the brain).

---

## 4.2 Elements of a Simple Neuron

```
Output = Activation_Function(Weighted_Sum_of_Inputs + Bias)

z = sum_i(w_i * x_i) + b     (pre-activation / net input)
y = f(z)                      (post-activation output)
```

### Components:

#### 1. Inputs (x1, x2, ..., xn)
Features or signals fed into the neuron (raw data or outputs from previous layer).

#### 2. Weights (w1, w2, ..., wn)
**Learnable parameters** that determine the strength and sign of each input's influence:
- Positive weight: input reinforces the neuron's output
- Negative weight: input suppresses the neuron's output
- Near-zero weight: input has little influence

#### 3. Bias (b)
A single **learnable scalar** that shifts the activation threshold. Without bias, the model can only learn functions through the origin (y = wx). With bias, it can learn y = wx + b.

#### 4. Summation (Linear Combination)
```
z = w1*x1 + w2*x2 + ... + wn*xn + b = w^T * x + b
```

#### 5. Activation Function f(z)
Introduces **non-linearity**. Without it, a multi-layer network would collapse to a single linear transformation (no expressive power gained from depth).

#### 6. Output y = f(z)

### Matrix Notation for a Layer

For a layer with n inputs and m neurons:
```
Z = W * X + b

X: input vector (n x 1)
W: weight matrix (m x n)
b: bias vector (m x 1)
Z: pre-activation vector (m x 1)
A = f(Z): output vector (m x 1)
```

---

## 4.3 Activation Functions

### 1. Step Function (Heaviside)
```
f(z) = 1 if z >= 0, else 0
```
**Problem**: Non-differentiable (gradient = 0 everywhere) -> cannot train with backprop.
**Use**: Binary thresholding only (not for training).

### 2. Sigmoid (Logistic)
```
f(z) = 1 / (1 + e^(-z))
Range: (0, 1)
```
**Pros**: Output interpretable as probability, smooth/differentiable.
**Cons**: Vanishing gradient for large |z|; not zero-centered (all positive).
**Use**: Binary classification output layer.

### 3. Tanh (Hyperbolic Tangent)
```
f(z) = (e^z - e^(-z)) / (e^z + e^(-z))
Range: (-1, 1)
```
**Pros**: Zero-centered, stronger gradient than sigmoid.
**Cons**: Still vanishing gradient for large |z|.
**Use**: Hidden layers (historically), RNNs.

### 4. ReLU (Rectified Linear Unit) - Most Popular

```
f(z) = max(0, z)
Range: [0, infinity)

Derivative:
f'(z) = 1 if z > 0
        0 if z <= 0
```

**Pros**: No vanishing gradient for positive values; computationally fast; sparse activations (~50% zeros).
**Cons**: Dying ReLU problem (if z < 0 always, neuron never activates).
**Use**: Default for hidden layers in deep networks.

### 5. Leaky ReLU
```
f(z) = z if z > 0, else alpha*z  (alpha typically 0.01)
```
**Fixes**: Dying ReLU problem (small gradient for negative inputs).

### 6. ELU (Exponential Linear Unit)
```
f(z) = z if z > 0, else alpha*(e^z - 1)
```
**Pros**: Smooth everywhere, negative values push mean toward zero.

### 7. Softmax (Multi-Class Output)
```
f(z_i) = e^(z_i) / sum_j(e^(z_j))  for class i
```
**Properties**: Output is a probability distribution (all values sum to 1).
**Use**: Multi-class classification output layer.

### Comparison Summary

| Function | Range | Zero-Centered | Vanishing Gradient | Dying Neurons |
|----------|-------|---------------|-------------------|---------------|
| Step | {0,1} | No | Yes (all) | N/A |
| Sigmoid | (0,1) | No | Yes (large z) | No |
| Tanh | (-1,1) | Yes | Yes (large z) | No |
| ReLU | [0,inf) | No | No (positive) | Yes |
| Leaky ReLU | (-inf,inf) | No | No | No |
| ELU | (-alpha,inf) | Yes | No | No |
| Softmax | (0,1) | No | Depends | No |

---

## 4.4 Classification of Neural Networks

```
Neural Networks
|
+-- Feedforward Networks (FNN)
|   +-- Single-Layer Perceptron
|   +-- Multi-Layer Perceptron (MLP)
|
+-- Recurrent Networks
|   +-- RNN (Recurrent Neural Network)
|   +-- LSTM (Long Short-Term Memory)
|   +-- GRU (Gated Recurrent Unit)
|
+-- Convolutional Networks
|   +-- CNN (Convolutional Neural Network)
|
+-- Generative Networks
|   +-- Autoencoders
|   +-- VAE (Variational Autoencoders)
|   +-- GAN (Generative Adversarial Networks)
|
+-- Attention/Transformer Networks
    +-- Transformer (BERT, GPT, etc.)
```

---

## 4.5 Feed-Forward Network

### Architecture

Information flows in **one direction only**: input -> hidden layers -> output. No loops.

```
INPUT LAYER    HIDDEN LAYERS    OUTPUT LAYER

   x1 -------> [h11]
               [h12] --> [h21] --> [o1]  y1
   x2 -------> [h13] --> [h22] --> [o2]  y2
               [h14] --> [h23]
   x3 -------> [h15]

Every arrow is a weighted connection.
```

### Forward Pass (Inference)

```
Layer 1: Z1 = W1 * X + b1
         A1 = f1(Z1)        <- Apply activation

Layer 2: Z2 = W2 * A1 + b2
         A2 = f2(Z2)

...

Layer L: ZL = WL * A(L-1) + bL
         Output = fL(ZL)    <- softmax for classification
```

### Universal Approximation Theorem

> A feedforward network with a single hidden layer containing sufficient neurons can approximate any continuous function on compact subsets of R^n, given appropriate activation functions.

**Implication**: MLPs are theoretically powerful enough to learn any function. The challenge is *training* them efficiently.

---

## 4.6 Backpropagation Network

Backpropagation is the **algorithm for efficiently computing gradients** in a neural network.

### Training Overview

```
FORWARD PASS:
Input X -> [Layer 1] -> [Layer 2] -> ... -> Predicted Output Y_hat
                                                    |
                                           Loss = L(Y_hat, Y)

BACKWARD PASS:
Compute gradients via chain rule, flowing backward:
dL/dWL <- [Layer L] <- [Layer L-1] <- ... <- [Layer 1]

WEIGHT UPDATE:
W = W - eta * dL/dW    (gradient descent step)
```

### Loss Functions

**Mean Squared Error (Regression)**:
```
MSE = (1/n) * sum((y_i - y_hat_i)^2)
```

**Cross-Entropy Loss (Classification)**:
```
Binary:     BCE = -(y*log(y_hat) + (1-y)*log(1-y_hat))
Multi-class: CE = -sum_i(y_i * log(y_hat_i))
```

### The Chain Rule — Foundation of Backpropagation

For composite function z = f(g(x)):
```
dz/dx = (dz/dg) * (dg/dx)
```

In neural networks:
```
dL/dW = (dL/dA) * (dA/dZ) * (dZ/dW)
```

### Backpropagation Step-by-Step (2-layer example)

```
Forward Pass:
z1 = W1*x + b1
a1 = sigmoid(z1)
z2 = W2*a1 + b2
y_hat = sigmoid(z2)
L = loss(y_hat, y)

Backward Pass (chain rule):
delta2 = dL/dz2 = (dL/dy_hat) * sigmoid'(z2)   <- output delta

dL/dW2 = delta2 * a1^T                           <- gradient of W2
dL/db2 = delta2                                   <- gradient of b2

delta1 = (W2^T * delta2) * sigmoid'(z1)          <- backpropagated error

dL/dW1 = delta1 * x^T                            <- gradient of W1
dL/db1 = delta1                                   <- gradient of b1
```

### Efficiency of Backpropagation

Without backprop: would need one forward pass per parameter to compute gradients.
With backprop: computes ALL gradients in a SINGLE backward pass using dynamic programming.

**Time complexity**: O(number of weights) per training example — same as forward pass!

---

## 4.7 Gradient Descent Algorithm

### Core Idea

Adjust weights to **minimize the loss function** by moving in the direction of steepest descent.

```
Loss L(W)
     |  Current weights are here
    /|\
   / | \
  /  |  \      <-- Loss landscape
 /   |   \____
/    |        \____  <- minimum

Move in the direction that decreases L fastest.
```

### The Update Rule

```
W_new = W_old - eta * dL/dW

eta = learning rate (step size)
dL/dW = gradient (direction of steepest ASCENT)
Minus sign = we go OPPOSITE to ascent (descent)
```

### Variants of Gradient Descent

#### 1. Batch Gradient Descent
- Compute gradient using the **entire training dataset** before updating
- Very stable convergence, but very slow for large datasets

```python
for epoch in range(epochs):
    gradient = compute_gradient(ENTIRE_DATASET)
    W = W - eta * gradient
```

#### 2. Stochastic Gradient Descent (SGD)
- Update after **each individual training example**
- Fast but noisy (high variance in gradient estimates)
- Noise can help escape local minima

```python
for epoch in range(epochs):
    shuffle(training_data)
    for x, y in training_data:
        gradient = compute_gradient(x, y)
        W = W - eta * gradient
```

#### 3. Mini-Batch Gradient Descent (Most Common)
- Update after a **small batch** (16, 32, 64, 128 examples)
- Balances stability and speed; efficient on GPUs

```python
for epoch in range(epochs):
    shuffle(training_data)
    for batch in mini_batches(training_data, batch_size=32):
        gradient = compute_gradient(batch)
        W = W - eta * gradient
```

### Advanced Optimizers

#### Momentum
```
v = beta * v - eta * grad_L    (beta ~= 0.9)
W = W + v

Adds "velocity" to prevent oscillations, speeds up convergence.
```

#### RMSProp
```
E[g^2] = beta * E[g^2] + (1-beta) * g^2
W = W - (eta / sqrt(E[g^2] + eps)) * g

Adapts learning rate per parameter based on recent gradient magnitudes.
```

#### Adam (Adaptive Moment Estimation) - Default Choice
```
m = beta1 * m + (1-beta1) * g           (first moment, mean)
v = beta2 * v + (1-beta2) * g^2         (second moment, variance)
m_hat = m / (1 - beta1^t)               (bias correction)
v_hat = v / (1 - beta2^t)               (bias correction)
W = W - eta * m_hat / (sqrt(v_hat) + eps)

Typical: beta1=0.9, beta2=0.999, eps=1e-8, eta=0.001
Combines momentum and RMSProp.
```

### Learning Rate Selection

```
Too high eta: W oscillates or diverges
Too low eta: Very slow convergence
Just right: Smooth, fast convergence

Learning rate schedules:
- Step decay: Multiply eta by factor every N epochs
- Cosine annealing: eta follows cosine curve
- Warmup: Start low, increase, then decrease
```

---

## 4.8 Convolutional Neural Network (CNN)

### Motivation

Regular MLPs fail on images:
- A 224x224 RGB image = 150,528 inputs
- 1000 neurons in first hidden layer = 150 million weights!

Problems:
1. Computationally prohibitive
2. Ignores spatial structure (nearby pixels are related)
3. Not translation invariant (cat at top-left != cat at bottom-right to MLP)

CNNs solve this with **local connectivity and weight sharing**.

### Key Operations

#### 1. Convolution Layer

A **filter/kernel** slides over the input and computes dot products:

```
Input (5x5):         Filter (3x3):        Output (3x3):
1  2  3  0  1        1  0 -1              ?
0  1  2  3  0   *    1  0 -1     =        ?
1  0  1  2  3        1  0 -1                 ?
0  1  0  1  2
1  2  1  0  1

Filter slides across input with stride S, producing a feature map.
Same filter weights applied everywhere = weight sharing!
```

**Example filters** (CNN learns these from data!):
```
Horizontal edge detector:    Vertical edge detector:
 1  2  1                      1  0 -1
 0  0  0                      2  0 -2
-1 -2 -1                      1  0 -1
```

#### 2. Padding
- **Valid padding**: No padding, output is smaller than input
- **Same padding**: Zero-pad input so output has same spatial size

#### 3. Stride
How many pixels the filter slides at each step:
- Stride 1: Dense coverage, output ~same size
- Stride 2: Skip pixels, output ~half size (downsampling)

#### 4. Pooling Layer

Reduces spatial dimensions while retaining important features:

```
Max Pooling (2x2, stride 2):
Input:              Output:
1  3 | 2  4         3  4
5  6 | 7  8    ->   8  ...
-----------
3  2 | 1  0
Takes MAX in each region -> captures strongest feature
```

#### 5. Typical CNN Architecture

```
Input Image
    -> [Conv + ReLU] -> [Max Pool]
    -> [Conv + ReLU] -> [Max Pool]
    -> [Conv + ReLU] -> [Max Pool]
    -> [Flatten]
    -> [Fully Connected]
    -> [Softmax Output]
```

### Famous CNN Architectures

| Architecture | Year | Key Innovation |
|-------------|------|----------------|
| LeNet-5 | 1998 | First successful CNN |
| AlexNet | 2012 | Deep CNN with ReLU, dropout, GPU training |
| VGGNet | 2014 | Very deep, small (3x3) filters |
| GoogLeNet/Inception | 2014 | Inception modules (multi-scale) |
| ResNet | 2015 | Residual/skip connections -> 152 layers! |
| EfficientNet | 2019 | Compound scaling |

### Why CNNs Work So Well

1. **Local connectivity**: Neurons connect only to nearby pixels (spatial locality)
2. **Weight sharing**: Same filter for entire image (translation equivariance)
3. **Hierarchical features**: edges -> shapes -> objects -> scenes
4. **Parameter efficiency**: Far fewer parameters than fully connected

---

## 4.9 Recurrent Neural Network (RNN)

### Motivation

MLPs and CNNs are **stateless** — no memory between inputs. Many real-world problems are **sequential**:
- Text: "The bank was steep" vs "The bank was bankrupt" — context matters!
- Time series, speech, video, music

RNNs maintain a **hidden state** (memory) across time steps.

### Architecture

```
Unrolled through time:

              t=1         t=2         t=3
Input:        x1          x2          x3
               |           |           |
               v           v           v
h0 ---[RNN]---h1---[RNN]---h2---[RNN]---h3
          |           |           |
         y1          y2          y3

Same weights (W_hh, W_xh, W_hy) used at EVERY timestep.
```

### RNN Equations

```
At each timestep t:
h_t = f(W_hh * h_{t-1} + W_xh * x_t + b_h)    <- hidden state update
y_t = g(W_hy * h_t + b_y)                       <- output (optional at each step)

Where:
x_t = input at time t
h_t = hidden state (memory) at time t
h_0 = initial hidden state (usually zeros)
W_hh = recurrent weights (hidden-to-hidden)
W_xh = input weights (input-to-hidden)
f = tanh or ReLU
```

### Training: Backpropagation Through Time (BPTT)

Unroll the RNN through T timesteps and apply backpropagation:
```
Total loss: L = sum_t(L_t(y_t, y_hat_t))
Gradients flow backward through all timesteps.
```

### The Vanishing/Exploding Gradient Problem

During BPTT, gradients are multiplied by W_hh repeatedly:
```
dh_100/dh_1 proportional to (W_hh)^99

If max eigenvalue of W_hh < 1: gradients -> 0 (VANISHING)
If max eigenvalue of W_hh > 1: gradients -> infinity (EXPLODING)
```

**Consequences**:
- **Vanishing**: Network can't learn long-range dependencies (can't remember info from 50+ steps ago)
- **Exploding**: Training becomes unstable

**Solutions**:
- **Gradient clipping**: Cap gradients if they exceed a threshold (fixes exploding)
- **LSTM/GRU**: Architectural solutions (fixes vanishing)

---

## 4.10 Long Short-Term Memory (LSTM)

### Motivation
LSTMs (Hochreiter & Schmidhuber, 1997) use **gating mechanisms** to control what information is remembered, forgotten, and output, solving the vanishing gradient problem.

### Architecture

LSTM has TWO state vectors:
- **h_t**: Short-term memory (hidden state)
- **C_t**: Long-term memory (cell state) — the "memory highway"

### LSTM Equations

```
Forget Gate:    f_t = sigmoid(Wf * [h_{t-1}, x_t] + bf)
                     <- What fraction of cell state to FORGET

Input Gate:     i_t = sigmoid(Wi * [h_{t-1}, x_t] + bi)
                     <- What new info to STORE

Candidate:      g_hat_t = tanh(Wg * [h_{t-1}, x_t] + bg)
                     <- New candidate values to potentially add

Cell Update:    C_t = f_t (x) C_{t-1} + i_t (x) g_hat_t
                     <- Forget old + Add new

Output Gate:    o_t = sigmoid(Wo * [h_{t-1}, x_t] + bo)
                     <- What to OUTPUT from cell state

Hidden State:   h_t = o_t (x) tanh(C_t)
                     <- Output filtered by output gate

(x) = element-wise multiplication
```

### Gate Intuition

| Gate | Range | Role | Analogy |
|------|-------|------|---------|
| Forget Gate f_t | [0,1] | What to erase from cell state | Selective amnesia |
| Input Gate i_t | [0,1] | How much new info to add | New info filter |
| Output Gate o_t | [0,1] | How much cell state to expose | What to reveal now |

### The Cell State — Information Highway

```
C_t = f_t (x) C_{t-1} + i_t (x) g_hat_t
       ^                  ^
  Forget old         Add new

If f_t ~= 1, i_t ~= 0: Cell unchanged (perfect memory)
If f_t ~= 0, i_t ~= 1: Cell state replaced with new info

The cell state flows with only element-wise ops (no deep non-linearity)
-> Gradient highway that allows gradients to flow back through many steps!
```

### LSTM Applications
- Machine translation, speech recognition
- Text generation, question answering
- Music generation, handwriting recognition

---

## 4.11 Gated Recurrent Unit (GRU)

### Overview
GRU (Cho et al., 2014) is a **simplified LSTM** with similar performance but fewer parameters. The cell state is merged with the hidden state.

### GRU Equations

```
Update Gate:    z_t = sigmoid(Wz * [h_{t-1}, x_t])
                     <- How much of old state to keep vs update

Reset Gate:     r_t = sigmoid(Wr * [h_{t-1}, x_t])
                     <- How much of past to use in new candidate

Candidate:      h_hat_t = tanh(W * [r_t (x) h_{t-1}, x_t])
                     <- New candidate hidden state

Hidden Update:  h_t = (1 - z_t) (x) h_{t-1} + z_t (x) h_hat_t
                     <- Interpolate between old and new
```

### LSTM vs GRU

| Aspect | LSTM | GRU |
|--------|------|-----|
| **Gates** | 3 (forget, input, output) | 2 (update, reset) |
| **Memory** | Cell state + hidden state | Hidden state only |
| **Parameters** | More (4 weight matrices) | Fewer (3 weight matrices) |
| **Speed** | Slower to train | Faster |
| **Performance** | Slightly better (long sequences) | Similar, often comparable |

**When to use**:
- **LSTM**: Very long sequences, complex memory control needed
- **GRU**: Most sequence tasks (faster, simpler, similar performance)

---

# 5. Support Vector Machine (SVM)

## Overview

**SVM** is a powerful supervised learning algorithm primarily for classification (also SVR for regression). It finds the **optimal hyperplane** separating classes with the **maximum margin**.

## The Core Concepts

### Hyperplane and Margin

In n-dimensional space: **w^T * x + b = 0** is the decision boundary (hyperplane).

```
        Class 2 (o):  o  o  o
                     [o]           <- Support vector
        w^T*x + b = +1 ---+
                           +-- MARGIN = 2/||w||
        w^T*x + b = -1 ---+
                     [*]           <- Support vector
        Class 1 (*):  *  *  *

Support vectors = data points closest to the hyperplane (they "support" it).
Margin = width of the "street" between classes.
```

### Mathematical Formulation

**Goal**: Find w and b such that:
```
y_i * (w^T * x_i + b) >= 1  for all i (class i correctly classified with margin)
```

**Maximize margin = minimize ||w||**:
```
Minimize:    (1/2) * ||w||^2
Subject to:  y_i * (w^T * x_i + b) >= 1  for all i

Quadratic Programming (QP) -> unique global solution!
```

### Soft-Margin SVM (C-SVM)

For non-perfectly-separable data, introduce slack variables xi_i:

```
Minimize:    (1/2)||w||^2 + C * sum(xi_i)
Subject to:  y_i*(w^T*x_i + b) >= 1 - xi_i,  xi_i >= 0

C (regularization):
- Large C: Penalize misclassification heavily -> small margin, risk overfitting
- Small C: Allow more errors -> large margin, risk underfitting
```

### The Kernel Trick — Non-Linear Data

Most real data is NOT linearly separable. The kernel trick maps data to a higher-dimensional space where linear separation is possible.

```
Original 2D space:          After mapping to 3D:
  o  o  *  *  o  o    ->    o  o  o (on sphere surface)
Not separable!              *  *  * (inside sphere)
                            Now linearly separable!
```

**Key insight**: Instead of computing the mapping phi(x) explicitly, use a kernel function:
```
K(x, z) = phi(x)^T * phi(z)
```

**Common Kernels**:

| Kernel | Formula | Use Case |
|--------|---------|---------|
| **Linear** | K(x,z) = x^T*z | Linearly separable data |
| **Polynomial** | K(x,z) = (gamma*x^T*z + r)^d | Polynomial boundaries |
| **RBF/Gaussian** | K(x,z) = exp(-gamma*||x-z||^2) | General non-linear |
| **Sigmoid** | K(x,z) = tanh(gamma*x^T*z + r) | Neural network-like |

The **RBF kernel** maps to infinite-dimensional space and can separate almost any data!

### Why SVM Works Well

1. **Maximum margin**: The margin acts as a buffer -> good generalization
2. **Support vectors only**: Robust to outliers far from the margin
3. **Kernel trick**: Complex non-linear boundaries without explicit high-dim computation
4. **Global optimum**: Convex optimization -> no local minima
5. **Effective in high dimensions**: Works well when features >> samples (e.g., text classification)

### SVM Limitations

- Slow for large datasets (quadratic in training examples)
- Sensitive to feature scaling (always normalize!)
- Kernel and C selection requires cross-validation
- Less interpretable than linear models

---

# 6. Types of Machine Learning

## 6.1 Supervised Learning

### Definition
Learn a mapping **f: X -> Y** from labeled training examples {(x1,y1), (x2,y2), ..., (xn,yn)}.

```
Training:
x1 (email text) -> y1 (SPAM)
x2 (email text) -> y2 (NOT SPAM)
...
Model learns the text -> spam mapping.

Prediction:
x_new (new email) -> ? (model predicts SPAM or NOT SPAM)
```

### Sub-types

**Classification**: Predict a discrete label (spam/not spam, cat/dog/bird, digit 0-9).

**Regression**: Predict a continuous value (house price, temperature, stock price).

### Key Assumptions
- Training and test data drawn from the **same distribution** (i.i.d.)
- Labels are correct (or mostly correct)

### Common Algorithms
Logistic Regression, SVM, Decision Trees, Random Forest, Neural Networks, KNN, Naive Bayes, Gradient Boosting

---

## 6.2 Unsupervised Learning

### Definition
No labels! The model must **discover structure** in raw data X.

```
Input: [height, weight, age] for 1000 people
No labels provided.

Model discovers: "There are 3 natural groups in this data"
(e.g., children, adults, elderly)
```

### Sub-types

#### Clustering
Group similar data points:
- **K-Means**: Partition into K clusters; minimize within-cluster variance

```
K-Means Algorithm:
1. Initialize K centroids randomly
2. Assign each point to nearest centroid
3. Recompute centroids as mean of assigned points
4. Repeat 2-3 until convergence
```

- **DBSCAN**: Density-based; discovers clusters of arbitrary shape; handles noise
- **Hierarchical**: Builds dendrogram of nested clusters

#### Dimensionality Reduction
Compress data while preserving structure:
- **PCA (Principal Component Analysis)**: Finds linear directions of maximum variance
- **t-SNE**: Non-linear; excellent for 2D/3D visualization
- **UMAP**: Faster than t-SNE; preserves global structure better
- **Autoencoders**: Neural network-based non-linear compression

#### Density Estimation
Model the underlying distribution P(X):
- **Gaussian Mixture Models (GMM)**
- **Kernel Density Estimation (KDE)**

#### Anomaly Detection
Identify unusual data points not fitting learned distribution:
- **Isolation Forest**
- **One-Class SVM**
- **Autoencoders** (anomalies have high reconstruction error)

---

## 6.3 Reinforcement Learning

### Definition
An **agent** learns to take **actions** in an **environment** to **maximize cumulative reward** through trial and error.

```
+---------+  Action a_t  +-------------+
|         |  ----------->|             |
|  Agent  |              | Environment |
|         |  <-----------+             |
+---------+  State s_{t+1}+-------------+
             Reward r_t
```

### Key Components

| Component | Description |
|-----------|-------------|
| **Agent** | The learner/decision-maker |
| **Environment** | The world the agent interacts with |
| **State (s)** | Current situation/observation |
| **Action (a)** | What the agent does |
| **Reward (r)** | Immediate feedback (+/-) |
| **Policy (pi)** | Strategy mapping states to actions |
| **Value function** | Expected future reward from a state |

### Markov Decision Process (MDP)

```
MDP = (S, A, T, R, gamma)

S: State space
A: Action space
T(s, a, s') = P(s'|s,a): Transition probability
R(s, a): Reward function
gamma: Discount factor (0 <= gamma < 1)
```

### Goal
Maximize **cumulative discounted reward**:
```
G_t = r_t + gamma*r_{t+1} + gamma^2*r_{t+2} + ...

gamma = 0: Only care about immediate reward
gamma ~= 1: Care about all future rewards equally
```

### Q-Learning

```
Q(s, a) = expected future reward from taking action a in state s

Q-update (Bellman equation):
Q(s, a) <- Q(s, a) + alpha * [r + gamma * max_{a'} Q(s', a') - Q(s, a)]
                                 ^TD Target^           ^Current estimate^
```

### Famous RL Achievements
- **DQN** (2015): Learned to play 49 Atari games from pixels (superhuman level)
- **AlphaGo** (2016): Defeated world Go champion
- **ChatGPT**: RLHF (Reinforcement Learning from Human Feedback) to align LLMs

---

# 7. Classification Models

## 7.1 Logistic Regression

Despite its name, it's a **classification** algorithm:

```
P(y=1|x) = sigmoid(w^T*x + b) = 1 / (1 + e^(-(w^T*x + b)))

Decision rule: Predict 1 if P > 0.5, else predict 0
```

**Properties**: Linear decision boundary; interpretable; calibrated probabilities; fast to train.

## 7.2 K-Nearest Neighbors (KNN)

No training phase — classify based on K nearest training examples:

```
For test point x:
1. Find K nearest training points (Euclidean distance)
2. Majority vote among their labels
3. Return winning label

Example (K=3):
     o *
   o   (test point)   K=3 neighbors: two * and one o
     * *              -> Predict * (majority)
```

**Properties**: Non-parametric; works for complex boundaries; slow at prediction O(n); sensitive to feature scale.

## 7.3 Naive Bayes

Based on Bayes' theorem with conditional independence assumption:

```
P(y|x1,x2,...,xn) proportional to P(y) * P(x1|y) * P(x2|y) * ... * P(xn|y)

Predict: argmax_y P(y) * product_i(P(x_i|y))
```

**Properties**: Very fast; works well for text; independence assumption often violated but often works well anyway.

## 7.4 Decision Trees

```
            [Age < 30?]
           /           \
         Yes            No
     [Student?]      [Credit?]
      /      \         /      \
    Yes       No     Fair    Excellent
    |          |       |         |
  Buy(yes)  Buy(no) Buy(no)  Buy(yes)
```

**Splitting criteria**:
- **Information Gain** (based on Entropy): IG(S, A) = H(S) - sum(|Sv|/|S| * H(Sv))
- **Gini Impurity**: G(S) = 1 - sum(p_i^2)
- **Entropy**: H(S) = -sum(p_i * log2(p_i))

**Properties**: Highly interpretable; can overfit easily (needs pruning); handles mixed feature types.

## 7.5 Random Forest (Ensemble)

An **ensemble** of many decision trees:

```
Bootstrap sample 1 -> Tree1  -+
Bootstrap sample 2 -> Tree2  -+
...                            +-> Majority Vote -> Final Prediction
Bootstrap sample N -> TreeN  -+
```

**Two randomness sources**:
1. **Bootstrap sampling**: Each tree trains on a random subset (with replacement)
2. **Random feature selection**: At each split, only a subset of features is considered

**Properties**: Very robust; rarely overfits; provides feature importance; slower than single tree.

## 7.6 Gradient Boosting

Build trees **sequentially**, each correcting errors of previous trees:

```
F_m(x) = F_{m-1}(x) + eta * f_m(x)

Each f_m is trained to predict the RESIDUALS of F_{m-1}
eta = learning rate (shrinkage)
```

**Variants**: XGBoost, LightGBM, CatBoost — top performers in ML competitions.

## 7.7 Comparison Table

| Algorithm | Interpretable | Train Speed | Pred Speed | Non-Linear | High-Dim |
|-----------|--------------|-------------|------------|------------|----------|
| Logistic Reg | Yes | Fast | Fast | No | Yes |
| KNN | Yes | Instant | Slow O(n) | Yes | No |
| Naive Bayes | Yes | Fast | Fast | No | Yes |
| Decision Tree | Yes | Fast | Fast | Yes | Moderate |
| Random Forest | Moderate | Moderate | Moderate | Yes | Yes |
| SVM | No | Slow (large n) | Fast | Yes (kernel) | Yes |
| Neural Network | No | Slow | Fast | Yes | Yes |
| Gradient Boosting | No | Slow | Fast | Yes | Yes |

---

# 8. Model Performance & Generalization

## 8.1 Overfitting & Underfitting

### The Goal
A model should **generalize** — perform well on **unseen data**, not just training data.

### Underfitting (High Bias)

**Definition**: Model too simple to capture underlying patterns.

```
Training data:    Model fit:
  *    *         ------------ (straight line, too simple)
*          *     Model barely fits even training data!
       *
```

**Symptoms**: High training error; high test error; both similar (both bad).

**Causes**: Model too simple; too much regularization; insufficient training time.

**Solutions**: Use more complex model; add more features; reduce regularization; train longer.

### Overfitting (High Variance)

**Definition**: Model learns training data **too well**, including noise, failing to generalize.

```
Training data:    Overfit model:
  *    *           ~~~~(complex wiggly curve)~~~~
*          *     Perfect fit on training but crazy on new data!
       *
```

**Symptoms**: Very low training error; high test error; large gap between train and test.

**Causes**: Model too complex; insufficient training data; too little regularization; training too long.

**Solutions**: Reduce complexity; get more data; add regularization; early stopping; dropout; data augmentation.

### Visual Summary

```
Underfitting            Good Fit              Overfitting

*     *              *     *                *     *
           *       (-)     (-)            (-) wiggly (-)
  *                    *         *       *           *

Training Err: HIGH  Training: LOW      Training: VERY LOW
Test Error:   HIGH  Test: LOW          Test: HIGH
```

---

## 8.2 Bias-Variance Tradeoff

### Definitions

**Bias**: Systematic error from incorrect assumptions. High-bias model consistently misses the true relationship.

**Variance**: Error from sensitivity to small fluctuations in training set. High-variance model changes dramatically with different training data.

### Mathematical Decomposition

```
E[(y - y_hat)^2] = Bias^2(y_hat) + Variance(y_hat) + sigma_e^2

Bias(y_hat) = E[y_hat] - y_true        (systematic error)
Variance(y_hat) = E[(y_hat - E[y_hat])^2]  (spread of predictions)
sigma_e^2 = irreducible noise (can't reduce this)
```

### The Tradeoff

```
Error
  |
  |  \.    Total Error (Bias^2 + Variance)
  |   \. - - - - - - - -  <- this U-shape
  |    \_____/
  |        ^Bias^2     Variance/
  |         decreasing |increasing
  |
  +------------------------------------------
     Simple Model              Complex Model
     (Underfitting)            (Overfitting)

Optimal: Find the sweet spot that minimizes TOTAL error.
```

### Regularization

**L2 Regularization (Ridge)**:
```
Loss = Original Loss + lambda * ||w||^2
Effect: Shrinks weights toward zero; prevents large weights; reduces variance.
```

**L1 Regularization (Lasso)**:
```
Loss = Original Loss + lambda * ||w||_1
Effect: Drives some weights exactly to zero; feature selection; sparser model.
```

**Elastic Net** (combines L1 and L2):
```
Loss = Original Loss + lambda1 * ||w||_1 + lambda2 * ||w||^2
```

### Cross-Validation

**K-Fold Cross-Validation** (estimate generalization without separate test set):

```
Data: [----------------------------------]

Fold 1: [TEST][-------Training-----------]
Fold 2: [----][TEST][-----Training-------]
Fold 3: [--------][TEST][---Training-----]
...
Fold K: [-------------------][TEST]

CV Score = Average of scores across all K folds
```

Typical K = 5 or 10.

---

## 8.3 Performance Measurement

### Confusion Matrix (Classification)

```
                  PREDICTED
                  Positive  Negative
ACTUAL  Positive |   TP   |   FN   |
        Negative |   FP   |   TN   |
```

| Term | Meaning |
|------|---------|
| **TP** (True Positive) | Correctly predicted positive |
| **TN** (True Negative) | Correctly predicted negative |
| **FP** (False Positive) | Predicted positive, actually negative (Type I) |
| **FN** (False Negative) | Predicted negative, actually positive (Type II) |

### Key Metrics

#### Accuracy
```
Accuracy = (TP + TN) / (TP + TN + FP + FN)

Misleading for imbalanced data!
(e.g., 99% negative class -> predict all negative -> 99% accuracy but useless)
```

#### Precision
```
Precision = TP / (TP + FP)
"Of all predicted positives, how many were truly positive?"
High precision = low false positive rate.
Important when FP is costly (spam detection -- don't block real emails).
```

#### Recall (Sensitivity / True Positive Rate)
```
Recall = TP / (TP + FN)
"Of all actual positives, how many did we detect?"
High recall = low false negative rate.
Important when FN is costly (cancer detection -- don't miss any!).
```

#### F1-Score (Harmonic Mean of Precision and Recall)
```
F1 = 2 * (Precision * Recall) / (Precision + Recall)

Balances precision and recall; useful for imbalanced classes.
```

#### Precision-Recall Tradeoff

```
Increasing threshold -> Precision increases, Recall decreases
Decreasing threshold -> Precision decreases, Recall increases

PR Curve plots Precision vs Recall across thresholds.
AUC-PR (higher = better).
```

#### ROC Curve and AUC

```
ROC: Plots True Positive Rate (Recall) vs False Positive Rate

FPR = FP / (FP + TN)
TPR = TP / (TP + FN)

TPR
1.0 (---)  <- Perfect classifier
     |  /
0.5 _|_/
     |/  <- Random (diagonal)
0.0 ____________
    0   0.5  1.0  FPR

AUC (Area Under Curve):
1.0 = perfect, 0.5 = random, <0.5 = worse than random
```

### Regression Metrics

| Metric | Formula | Notes |
|--------|---------|-------|
| **MAE** | (1/n)*sum(\|yi - y_hat_i\|) | Robust to outliers; interpretable units |
| **MSE** | (1/n)*sum((yi - y_hat_i)^2) | Penalizes large errors; differentiable |
| **RMSE** | sqrt(MSE) | Same units as target; sensitive to outliers |
| **R^2** | 1 - SS_res/SS_tot | 1=perfect, 0=baseline(mean), <0=worse than baseline |

### Model Selection & Hyperparameter Tuning

#### Grid Search
Exhaustively search all combinations:
```python
param_grid = {
    'C': [0.1, 1, 10, 100],
    'kernel': ['linear', 'rbf'],
    'gamma': ['scale', 'auto']
}
# 4 x 2 x 2 = 16 models trained x K folds
```

#### Random Search
Sample random combinations — often as good as grid search with far fewer evaluations.

#### Bayesian Optimization
Uses probabilistic model of objective function to choose next hyperparameters intelligently.

### Learning Curves (Diagnosing Problems)

```
Error
  |
  | Training ----             <- Training error too high (underfitting)
  |           Validation ---- <- Similar to training error
  |
  |                           
  | Training ────             <- Training error low
  |                Validation <- Validation error much higher (overfitting)
  |                    \────
  |
  +---------------------------
       Training Set Size

Left plot: Underfitting (more data won't help -- need better model)
Right plot: Overfitting (more data would help; also regularize more)
```

---

## Quick Reference Summary

### Search Algorithm Comparison
| Algorithm | Strategy | Complete | Optimal |
|-----------|----------|---------|---------|
| BFS | Shallowest first | Yes | Yes* |
| DFS | Deepest first | No | No |
| IDS | Iterative depth | Yes | Yes* |
| UCS | Cheapest g(n) | Yes | Yes |
| GBFS | Best h(n) | No | No |
| A* | Best f=g+h | Yes | Yes (admissible h) |

### Activation Functions Quick Reference
| Function | Formula | Best For |
|----------|---------|---------|
| ReLU | max(0,x) | Hidden layers (default) |
| Sigmoid | 1/(1+e^-x) | Binary classification output |
| Softmax | e^xi/sum(e^xj) | Multi-class output |
| Tanh | (e^x-e^-x)/(e^x+e^-x) | RNNs, zero-centered |

### Key Formulas
```
A* evaluation:       f(n) = g(n) + h(n)
LSTM forget gate:    f_t = sigmoid(Wf * [h_{t-1}, x_t] + bf)
LSTM cell update:    C_t = f_t (x) C_{t-1} + i_t (x) g_hat_t
SVM margin:          2 / ||w||
Adam optimizer:      W <- W - eta * m_hat / sqrt(v_hat + eps)
Bias-Var decomp:     E[error] = Bias^2 + Variance + Noise
F1 Score:            2 * P * R / (P + R)
Softmax:             f(z_i) = e^(z_i) / sum_j(e^(z_j))
```

---

*This guide covers fundamental and advanced topics in AI/ML. For deeper study, refer to:*
- *"Artificial Intelligence: A Modern Approach" by Russell & Norvig*
- *"Deep Learning" by Goodfellow, Bengio & Courville*
- *"Pattern Recognition and Machine Learning" by Bishop*
- *"Hands-On Machine Learning" by Géron (O'Reilly)*

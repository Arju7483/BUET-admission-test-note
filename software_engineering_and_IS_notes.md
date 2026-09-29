# 📘 Software Engineering & Information System
### Complete Exam Notes — BUET MSc Admission

---

## 📋 Table of Contents

1. [Software Development Life Cycle (SDLC) Models](#1-software-development-life-cycle-sdlc-models)
2. [Requirements Engineering](#2-requirements-engineering)
3. [UML Diagrams](#3-uml-diagrams)
4. [Design Patterns](#4-design-patterns)
5. [Software Testing](#5-software-testing)
6. [Project Management (PERT / CPM)](#6-project-management-pert--cpm)
7. [Software Security & Non-Functional Requirements](#7-software-security--non-functional-requirements)
8. [Information Systems (IS)](#8-information-systems-is)
9. [Data Flow Diagrams (DFD)](#9-data-flow-diagrams-dfd)
10. [System Analysis & Design](#10-system-analysis--design)
11. [Quick Reference & Fill-in-the-Blanks](#11-quick-reference--fill-in-the-blanks)

---

## 1. Software Development Life Cycle (SDLC) Models

### 1.1 What is SDLC?
SDLC is a structured process for planning, creating, testing, and deploying an information system. It provides a framework for managing the stages of a software project.

---

### 1.2 Waterfall Model ⭐ [BUET 2017 — Most Repeated]

The **Waterfall Model** is a sequential, linear software development model where progress flows steadily downward (like a waterfall) through defined phases.

```
┌─────────────────────────────────────────────────────┐
│                   WATERFALL MODEL                    │
│                                                     │
│  ┌──────────────────────┐                          │
│  │  1. Requirements     │                          │
│  └──────────┬───────────┘                          │
│             │                                       │
│             ▼                                       │
│  ┌──────────────────────┐                          │
│  │  2. System Design    │                          │
│  └──────────┬───────────┘                          │
│             │                                       │
│             ▼                                       │
│  ┌──────────────────────┐                          │
│  │  3. Implementation   │ ◄── Actual coding here   │
│  └──────────┬───────────┘                          │
│             │                                       │
│             ▼                                       │
│  ┌──────────────────────┐                          │
│  │  4. Testing          │                          │
│  └──────────┬───────────┘                          │
│             │                                       │
│             ▼                                       │
│  ┌──────────────────────┐                          │
│  │  5. Deployment       │                          │
│  └──────────┬───────────┘                          │
│             │                                       │
│             ▼                                       │
│  ┌──────────────────────┐                          │
│  │  6. Maintenance      │                          │
│  └──────────────────────┘                          │
└─────────────────────────────────────────────────────┘
```

#### Phases:
| Phase | Description | Output Document |
|-------|-------------|-----------------|
| Requirements | Gather & document all requirements | Software Requirement Specification (SRS) |
| System Design | Architecture, DB schema, UI design | Design Document Specification (DDS) ⭐ |
| Implementation | Coding / Programming | Source code |
| Testing | Verify the software works correctly | Test Reports |
| Deployment | Release to production | Deployed product |
| Maintenance | Bug fixes, updates | Maintenance logs |

> **[BUET FILL-IN]** After the **design phase**, the document prepared is known as the **Design Document Specification (DDS)**
> **[BUET FILL-IN]** Actual programming of software code is done during the **Implementation phase** in the SDLC.

#### Advantages:
- Simple and easy to understand
- Well-documented; each phase has defined output
- Works well when requirements are **well-known and stable**
- Easy to manage due to rigidity

#### Disadvantages:
- **No feedback loops** — cannot go back easily to a previous phase
- Not suitable when requirements are **unclear or changing**
- Testing happens late — bugs found late are costly to fix
- **Customer does not see the product until the end**
- High risk for long projects

---

### 1.3 Iterative Model
Development is done in **repeating cycles (iterations)**. Each iteration produces a working version of the product. Requirements are refined in each cycle.

```
  Plan -> Requirement -> Design -> Code -> Test
   ^                                          |
   +-------------- (next iteration) ----------+
```

---

### 1.4 Spiral Model
Combines **iterative development** with **risk analysis**. Each spiral loop has four phases:
1. **Planning** — objectives, alternatives
2. **Risk Analysis** — identify & resolve risks
3. **Engineering** — development & testing
4. **Evaluation** — customer review

Best for: large, complex, high-risk projects.

---

### 1.5 Agile Model
Focuses on **flexibility, customer collaboration, and iterative delivery**. Work is done in short cycles called **Sprints** (usually 2-4 weeks).

**Agile Manifesto Values:**
- Individuals & interactions > processes & tools
- Working software > comprehensive documentation
- Customer collaboration > contract negotiation
- Responding to change > following a plan

**Scrum** is the most common Agile framework.

---

### 1.6 Software Maintenance Life Cycle ⭐ [BUET 2019]

After deployment, software enters the **Maintenance Phase**. It is the **longest and most expensive** phase.

#### Types of Software Maintenance:
| Type | Description | Example |
|------|-------------|---------|
| **Corrective** | Fix bugs/defects found after release | Fix login crash |
| **Adaptive** | Modify software for new environment | Upgrade for new OS |
| **Perfective** | Improve performance/new features | Add dark mode |
| **Preventive** | Prevent future problems (re-engineering) | Refactor code |

```
        MAINTENANCE LIFE CYCLE
  +------------------------------+
  |  1. Problem Identification   |
  +-------------+----------------+
                |
  +-------------v----------------+
  |  2. Analysis & Design        |
  +-------------+----------------+
                |
  +-------------v----------------+
  |  3. Implementation/Coding    |
  +-------------+----------------+
                |
  +-------------v----------------+
  |  4. Testing & QA             |
  +-------------+----------------+
                |
  +-------------v----------------+
  |  5. Delivery / Deployment    |
  +------------------------------+
```

---

### 1.7 System Conversion Strategies ⭐ [BUET Fill-in 2017]

When transitioning from old system to new:

| Strategy | Description |
|----------|-------------|
| **Direct/Abrupt Cutover** | Old system shut down, new one starts immediately |
| **Parallel Running** | Both old & new systems run simultaneously to compare results ⭐ |
| **Pilot** | New system deployed to a small group first |
| **Phased** | New system introduced in stages/modules |

> **[BUET FILL-IN]** "Use the new system at the same time as the old system to compare the results" = **Parallel Running**

---

### 1.8 Reasons for Software Project Failure ⭐ [BUET CSE 2024]

- Unclear or changing requirements
- Poor project planning & estimation
- Lack of communication among stakeholders
- Inadequate testing
- Scope creep (uncontrolled feature additions)
- Lack of skilled resources
- Poor risk management
- Management/leadership failures
- Unrealistic deadlines and budget

---

## 2. Requirements Engineering

### 2.1 Types of Requirements

#### Functional Requirements:
Define **what the system should do**.
- Example: "A student can submit assignments before deadline."

#### Non-Functional Requirements: ⭐ [BUET CSE 2021]
Define **quality attributes** — how the system performs.

| Type | Example |
|------|---------|
| **Performance** | System must respond in < 2 seconds |
| **Security** | Data must be encrypted |
| **Reliability** | 99.9% uptime |
| **Scalability** | Handle 10,000 concurrent users |
| **Usability** | Interface must be accessible |
| **Maintainability** | Code must be well-documented |

> **[BUET TRICK]** For BUET Student Management System, something like "A student can register for a course" is a **FUNCTIONAL** requirement. "Non-functional" = quality attributes.

---

### 2.2 Security Requirements ⭐ [BUET CSE 2021]

The **CIA Triad + Non-repudiation**:

| Property | Definition |
|----------|-----------|
| **Confidentiality** | Only authorized users can access data |
| **Integrity** | Data is not modified without authorization |
| **Availability** | System is accessible when needed |
| **Non-repudiation** | A user cannot deny performing an action ⭐ |
| **Authentication** | Verifying identity |
| **Authorization** | Granting access rights |

> **[BUET 2021]** "A student cannot deny having edited submission after the deadline" = **Non-repudiation**

---

## 3. UML Diagrams

### 3.1 Types of UML Diagrams

```
                UML DIAGRAMS
    +-----------------------------------+
    |                                   |
Structural                          Behavioral
Diagrams                            Diagrams
+----------+                  +------------------+
| Class    |                  | Use Case Diagram |
| Object   |                  | Sequence Diagram |
| Component|                  | Activity Diagram |
| Deployment|                 | State Diagram    |
| Package  |                  | Communication    |
+----------+                  +------------------+
```

---

### 3.2 Use Case Diagram ⭐ [BUET 2015, 2017, 2019, 2021 — Very Repeated]

A Use Case Diagram shows the **interactions between users (actors) and the system**.

#### Components:
| Symbol | Element | Description |
|--------|---------|-------------|
| Stick figure | **Actor** | External entity (user, system) |
| Oval | **Use Case** | System functionality |
| Rectangle | **System Boundary** | Scope of the system |
| Solid arrow | **Association** | Actor <-> use case link |
| Dashed arrow `<<include>>` | **Include** | Always included sub-use case |
| Dashed arrow `<<extend>>` | **Extend** | Optional behavior |
| Solid line + triangle | **Generalization** | Inheritance |

#### Relationships Explained: ⭐ [BUET 2015]

**Include (`<<include>>`):**
- One use case **always** calls another
- The included use case is **mandatory**
- Arrow goes FROM the base TO the included use case
- Example: "Make Payment" includes "Verify Card"

```
  [Checkout] --<<include>>--> [Verify Payment]
```

**Extend (`<<extend>>`):**
- One use case **optionally** extends another
- Arrow goes FROM the extending TO the base use case
- Example: "Add Coupon" extends "Checkout"

```
  [Add Coupon] --<<extend>>--> [Checkout]
```

**Generalization:**
- An actor or use case **inherits** from another
- Example: "Admin" generalizes "User"

```
  [Admin] --------> [User]
```

#### E-Commerce Use Case Diagram ⭐ [BUET 2017]:

```
+----------------------------------------------------------+
|                    E-Commerce System                      |
|                                                          |
|    +---------------+      +--------------------+        |
|    | Browse Items  |      |   Search Product   |        |
|    +---------------+      +--------------------+        |
|    +---------------+      +--------------------+        |
|    | Add to Cart   |      |  View Order History|        |
|    +-------+-------+      +--------------------+        |
|            |                                             |
|    +-------v-------+  <<include>> +------------------+  |
|    |   Checkout    |------------>| Verify Payment   |  |
|    +---------------+             +------------------+  |
|                                                          |
|                    +------------------+                  |
|                    |  Manage Orders   |                  |
|                    +------------------+                  |
+----------------------------------------------------------+
   [Customer]                                 [Admin]
```

#### ATM Use Case Actors ⭐ [BUET CSE 2021]:
Actors of ATM system:
- **Customer** (primary actor)
- **Bank System** (secondary actor / external system)
- **Maintenance Personnel** (secondary actor)

> Bank employees who process transactions in backend = external system actor, NOT customer.

---

### 3.3 Class Diagram ⭐ [BUET 2019]

Shows the **static structure** of a system — classes, attributes, methods, and relationships.

#### Class Notation:
```
+----------------------+
|      ClassName       |   <- Class Name (bold, centered)
+----------------------+
| -attribute: Type     |   <- Attributes (with visibility)
| +publicAttr: String  |
+----------------------+
| +method(): ReturnType|   <- Methods/Operations
| -privateMethod()     |
+----------------------+

Visibility:
  + = public
  - = private
  # = protected
  ~ = package
```

#### Relationships:
| Relationship | Symbol | Meaning |
|-------------|--------|---------|
| **Association** | -------- | "uses" or "has" |
| **Aggregation** | <>------ | "has-a" (weak) — parts can exist alone |
| **Composition** | <filled>------ | "part-of" (strong) — parts die with parent |
| **Inheritance** | -------> | "is-a" |
| **Dependency** | - - - -> | "uses temporarily" |
| **Realization** | - - - |> | Class implements interface |

#### Online Shopping Class Diagram ⭐ [BUET 2019]:

```
  +------------+          +--------------+
  |   User     |          |   Product    |
  +------------+          +--------------+
  |-userId     |  1    *  |-productId    |
  |-name       |<>--------|-name         |
  |-email      |          |-price        |
  +------------+          |-stock        |
  |+login()    |          +--------------+
  |+logout()   |          |+getDetails() |
  +-----+------+          +--------------+
        | (inheritance)
  +-----v----------+     +------------------+
  |   Customer     | 1 * |      Order       |
  +----------------+-----+------------------+
  |-address        |     |-orderId          |
  |-phone          |     |-orderDate        |
  +----------------+     |-totalAmount      |
  |+placeOrder()   |     +------------------+
  |+viewCart()     |     |+calculateTotal() |
  +----------------+     +--------+---------+
                                   | 1..*
                          +--------v--------+
                          |   OrderItem     |
                          +-----------------+
                          |-quantity        |
                          |-unitPrice       |
                          +-----------------+
```

---

### 3.4 State Diagram ⭐ [BUET 2019]

Shows the **states** an object goes through and the **transitions** between states in response to events.

#### E-Commerce Order State Diagram:

```
        +-----------------------------------------------------------+
        |               Order State Diagram                         |
        +-----------------------------------------------------------+

 (*)  --> [Pending] --payment confirmed--> [Confirmed]
                                                |
                                         item dispatched
                                                |
                                                v
                                           [Shipped]
                                                |
                                            delivered
                                                |
                                                v
 [Cancelled] <--cancel----------        [Delivered]
                                                |
                                         return requested
                                                |
                                                v
                                           [Returned] --> (**)
```

#### Components:
| Symbol | Meaning |
|--------|---------|
| (*) filled circle | Initial state |
| (**) circle in circle | Final state |
| Rounded rectangle | State |
| Arrow with label | Transition + event/condition |

---

### 3.5 Sequence Diagram
Shows **interaction between objects over time** (time flows downward).

```
  User          Browser         Server          DB
   |               |               |             |
   |--request----->|               |             |
   |               |--HTTP GET---->|             |
   |               |               |--SQL Query->|
   |               |               |<---Result---|
   |               |<--Response----|             |
   |<--display-----|               |             |
```

---

### 3.6 Employee Management System Use Case ⭐ [BUET]

```
+--------------------------------------------+
|         Employee Management System         |
|                                            |
|  +----------------------+                 |
|  |  Manage Employee Info|                 |
|  +----------------------+                 |
|  +----------------------+                 |
|  |  Manage Employee     |                 |
|  |  Status              |                 |
|  +----------------------+                 |
|  +----------------------+                 |
|  |  Manage Job          |                 |
|  +----------------------+                 |
|  +----------------------+                 |
|  |  Manage Department   |                 |
|  +----------------------+                 |
+--------------------------------------------+
                            [Admin]
```

---

## 4. Design Patterns

### 4.1 What Are Design Patterns?
Reusable solutions to commonly occurring problems in software design. Categorized by **GoF (Gang of Four)** into:

| Category | Patterns |
|----------|---------|
| **Creational** | Singleton, Factory, Abstract Factory, Builder, Prototype |
| **Structural** | Adapter, Bridge, Decorator, Facade, Proxy, Composite |
| **Behavioral** | Observer, Strategy, Command, Iterator, Template, State |

---

### 4.2 Singleton Pattern ⭐ [BUET CSE 2021]

**Problem:** Need exactly **one instance** of a class across the system.

**Real Use Case [BUET]:** In a DBMS, generating **unique sequence numbers** — only one generator should exist so multiple users don't get duplicate IDs.

```
Class: SequenceGenerator (Singleton)
--------------------------------------
- instance: SequenceGenerator  (static, private)
- currentNumber: int

- SequenceGenerator()  (private constructor)
+ getInstance(): SequenceGenerator  (static)
+ nextNumber(): int
```

#### Implementation Logic:
```python
class SequenceGenerator:
    _instance = None

    def __init__(self):
        self._current = 0

    @classmethod
    def get_instance(cls):
        if cls._instance is None:
            cls._instance = SequenceGenerator()
        return cls._instance

    def next_number(self):
        self._current += 1
        return self._current
```

#### Key Properties of Singleton:
- Private constructor
- Static instance variable
- Public static `getInstance()` method
- Thread-safe in multi-threaded environments (use double-checked locking)

---

### 4.3 Factory Pattern ⭐ [BUET CSE 2021]

**Problem:** Client needs to create objects without knowing the exact class.

**Structure:**
```
        +--------------+
        |   Creator    | (Factory)
        |+createShape()|
        +------+-------+
               |
     +---------+---------+
     |         |         |
  +--v---+ +---v---+ +---v----+
  |Circle| |Square | |Triangle|
  +------+ +-------+ +--------+
```

#### Advantages of Factory Pattern:
- Promotes **loose coupling**
- Code is **open for extension, closed for modification** (Open/Closed Principle)
- Client doesn't need to know the concrete class
- Easy to add new product types

> **[BUET 2021 — "NOT an advantage"]:** Factory pattern does NOT improve **runtime performance** directly.

---

### 4.4 Strategy Pattern ⭐ [BUET 2024 — Most Detailed]

**Problem:** Need to select an **algorithm at runtime** from a family of algorithms.

**Real Example [BUET 2024]:** Sorting application — choose Bubble Sort, Quick Sort, or Merge Sort depending on list size.

#### Class Diagram:
```
        +------------------+
        |   SortContext    |
        +------------------+
        |-strategy: Sorter |
        +------------------+
        |+setStrategy()    |
        |+sort(list)       |
        +--------+---------+
                 | uses
         +-------v--------+  (interface)
         |   <<Sorter>>   |
         |+sort(list)     |
         +-------+--------+
                 |
    +------------+------------+
    |            |            |
+---v----+  +---v----+  +---v------+
|Bubble  |  |Quick   |  |Merge     |
|Sort    |  |Sort    |  |Sort      |
+--------+  +--------+  +----------+
|+sort() |  |+sort() |  |+sort()   |
+--------+  +--------+  +----------+
```

#### Code Implementation:
```python
# Strategy Interface
class Sorter:
    def sort(self, data: list) -> list:
        raise NotImplementedError

# Concrete Strategies
class BubbleSort(Sorter):
    def sort(self, data):
        # O(n^2) — good for very small lists
        n = len(data)
        for i in range(n):
            for j in range(n-i-1):
                if data[j] > data[j+1]:
                    data[j], data[j+1] = data[j+1], data[j]
        return data

class QuickSort(Sorter):
    def sort(self, data):
        # O(n log n) avg — good for medium lists
        if len(data) <= 1:
            return data
        pivot = data[len(data)//2]
        left = [x for x in data if x < pivot]
        mid  = [x for x in data if x == pivot]
        right= [x for x in data if x > pivot]
        return self.sort(left) + mid + self.sort(right)

class MergeSort(Sorter):
    def sort(self, data):
        # O(n log n) — good for large lists, stable
        if len(data) <= 1:
            return data
        mid = len(data) // 2
        left = self.sort(data[:mid])
        right = self.sort(data[mid:])
        return self._merge(left, right)

    def _merge(self, l, r):
        result = []
        i = j = 0
        while i < len(l) and j < len(r):
            if l[i] <= r[j]:
                result.append(l[i]); i += 1
            else:
                result.append(r[j]); j += 1
        result.extend(l[i:])
        result.extend(r[j:])
        return result

# Context
class SortContext:
    def __init__(self, strategy: Sorter):
        self._strategy = strategy

    def set_strategy(self, strategy: Sorter):
        self._strategy = strategy

    def sort(self, data):
        return self._strategy.sort(data)

# Client Usage
def sort_list(data: list) -> list:
    n = len(data)
    if n < 10:
        ctx = SortContext(BubbleSort())
    elif n < 10000:
        ctx = SortContext(QuickSort())
    else:
        ctx = SortContext(MergeSort())
    return ctx.sort(data)
```

---

### 4.5 Observer Pattern
**Problem:** One object changes state, many objects need to be notified automatically.

```
    Subject ---notifies---> Observer1
                            Observer2
                            Observer3
```

---

### 4.6 Decorator Pattern
**Problem:** Add behavior to objects dynamically without subclassing.

---

## 5. Software Testing

### 5.1 Levels of Testing ⭐ [BUET 2019]

```
            TESTING PYRAMID
                  +---------------+
                  |  Acceptance   |  <- End-to-end, customer
                  |   Testing     |
              +---+---------------+---+
              |   Integration Testing |  <- Modules interact
          +---+-----------------------+---+
          |        System Testing         |  <- Full system
      +---+-------------------------------+---+
      |            Unit Testing               |  <- Individual units
      +---------------------------------------+
```

| Level | Description | Who Performs |
|-------|-------------|-------------|
| **Unit Testing** | Test smallest individual unit (function/method) in isolation | Developer |
| **Integration Testing** | Test combined modules | Developer/Tester |
| **System Testing** | Test entire integrated system | QA Team |
| **Acceptance Testing** | Verify against customer requirements | Customer/QA |

#### Unit Testing ⭐ [BUET 2019]:
- Tests a **single function or module** in isolation
- Uses **mock objects** for dependencies
- Example: Test the `calculateTotal()` function alone

#### Acceptance Testing ⭐ [BUET 2019]:
- Validates the software meets **business requirements**
- Done by the **end user or client**
- Two types:
  - **Alpha Testing**: Done by internal testers at developer's site
  - **Beta Testing**: Done by real users in real environment

---

### 5.2 Black Box vs White Box Testing

| Aspect | Black Box | White Box |
|--------|-----------|-----------|
| Knowledge of code | No (external view) | Yes (internal view) |
| Based on | Requirements/specifications | Code structure |
| Done by | Testers | Developers |
| Techniques | BVA, Equivalence, Decision Table | Statement, Branch, Path coverage |

---

### 5.3 Boundary Value Analysis (BVA) ⭐ [BUET 2017]

**Problem [BUET]:** A string must be between 8 and 12 characters.

**BVA Rule:** Test at:
- Minimum boundary: 8
- Just below minimum: 7 (invalid)
- Just above minimum: 9 (valid)
- Maximum boundary: 12
- Just above maximum: 13 (invalid)
- Just below maximum: 11 (valid)

| Test Case | Length | Expected |
|-----------|--------|----------|
| TC1 | 7 | INVALID |
| TC2 | 8 | VALID (min boundary) |
| TC3 | 9 | VALID |
| TC4 | 11 | VALID |
| TC5 | 12 | VALID (max boundary) |
| TC6 | 13 | INVALID |

**Total BVA test cases = 6** (for one boundary)

---

### 5.4 Equivalence Partitioning
Divides input data into partitions where all values in a partition are expected to behave the same.

For string length 8-12:
- **Invalid partition 1:** Length < 8
- **Valid partition:** 8 <= Length <= 12
- **Invalid partition 2:** Length > 12

---

### 5.5 Regression Testing
Re-running previously passed tests after code changes to ensure nothing broke.

---

### 5.6 Types of Testing Summary:
| Type | Description |
|------|-------------|
| **Smoke Testing** | Quick test to check if basic functions work |
| **Sanity Testing** | Focused test after minor change |
| **Regression Testing** | Ensure new changes didn't break old functionality |
| **Performance Testing** | Test speed, scalability under load |
| **Load Testing** | Test at expected peak load |
| **Stress Testing** | Test beyond peak load |
| **Security Testing** | Test for vulnerabilities |

---

## 6. Project Management (PERT / CPM)

### 6.1 PERT (Program Evaluation and Review Technique) ⭐ [BUET 2015, 2016, 2017]

PERT is a project management tool used to:
- Represent **tasks and their dependencies**
- Find the **Critical Path** (longest path = minimum project duration)
- Estimate **project completion time**

#### Key Concepts:
| Term | Definition |
|------|-----------|
| **Activity** | A task/job with duration |
| **Event/Node** | Start or end of an activity |
| **Critical Path** | Longest path from start to end (no slack) |
| **Slack/Float** | Delay an activity can have without delaying the project |
| **EST** | Earliest Start Time |
| **EFT** | Earliest Finish Time |
| **LST** | Latest Start Time |
| **LFT** | Latest Finish Time |

#### Formula:
```
EFT  = EST + Duration
Slack = LST - EST  =  LFT - EFT
Critical Path activities have Slack = 0
```

#### Example [BUET 2015 — PERT chart]:

| Activity | Predecessor | Duration |
|----------|-------------|----------|
| A | None | 3 |
| B | None | 4 |
| C | A | 2 |
| D | A, B | 5 |
| E | C | 4 |
| F | D, E | 3 |

```
PERT Network Diagram:
              C(2)
   (1)--A(3)--(2)--E(4)--(3)--+
    |                          |
    +--B(4)--(4)--D(5)--(5)---+--F(3)--(6)

Steps to find Critical Path:
1. Draw the network
2. Calculate EST/EFT for each node (forward pass)
3. Calculate LST/LFT for each node (backward pass)
4. Activities with Slack=0 are on Critical Path
```

#### How to Calculate (General Steps):
1. **Forward Pass:** EST[start]=0; EFT = EST + Duration; EST of next node = max(all EFTs leading to it)
2. **Backward Pass:** LFT[end] = EFT[end]; LST = LFT - Duration; LFT of previous node = min(all LSTs leaving it)
3. **Slack** = LFT - EFT (or LST - EST)
4. **Critical Path** = all activities with Slack = 0

---

### 6.2 CPM (Critical Path Method)
Similar to PERT but uses **deterministic** (fixed) time estimates instead of probabilistic estimates.

**PERT vs CPM:**
| Aspect | PERT | CPM |
|--------|------|-----|
| Time estimate | Probabilistic (3 estimates) | Deterministic (1 estimate) |
| Best for | R&D, uncertain projects | Construction, well-known tasks |
| Focus | Time | Time & Cost |

---

### 6.3 Gantt Chart
A bar chart showing project tasks vs time. Simple visual scheduling tool (not network-based like PERT).

---

## 7. Software Security & Non-Functional Requirements

### 7.1 CIA Triad
```
          +--------------------+
          |  Confidentiality   |
          |  (only authorized  |
          |    can access)     |
          +---------+----------+
                    |
         +----------+-----------+
         |   INFORMATION        |
         |    SECURITY          |
         +----------+-----------+
         |                      |
+--------v---------+  +---------v--------+
|    Integrity     |  |  Availability    |
|(data not modified|  |(system accessible|
|  without auth)   |  |  when needed)    |
+------------------+  +------------------+
```

### 7.2 STRIDE Threat Model (Software):
| Threat | Violated Property |
|--------|-------------------|
| **S**poofing | Authentication |
| **T**ampering | Integrity |
| **R**epudiation | Non-repudiation |
| **I**nformation Disclosure | Confidentiality |
| **D**enial of Service | Availability |
| **E**levation of Privilege | Authorization |

### 7.3 Non-Functional Requirements for Exam:

```
Non-Functional Requirements (NFR) Types:
------------------------------------------
- Functional    -> What system does (NOT NFR!)
- Performance   -> Speed, throughput
- Scalability   -> Handle growth
- Reliability   -> Uptime, fault tolerance
- Security      -> CIA + non-repudiation
- Usability     -> User experience
- Maintainability -> Ease of change
- Portability   -> Run on different platforms
```

---

## 8. Information Systems (IS)

### 8.1 What is an Information System?

An **Information System (IS)** is an organized combination of:
- **People** (users, operators)
- **Hardware** (computers, devices)
- **Software** (programs, applications)
- **Data** (raw facts)
- **Procedures** (rules, policies)
- **Networks** (communication links)

That **collect, process, store, and communicate** information to support decision-making, coordination, and control in an organization.

---

### 8.2 Types of Information Systems

```
             ORGANIZATIONAL LEVELS
  +------------------------------------------+
  |   Executive Support Systems (ESS)        | <- Strategic level (top mgmt)
  +------------------------------------------+
  |  Decision Support Systems (DSS)          | <- Management level
  |  Management Info Systems (MIS)           |
  +------------------------------------------+
  |  Knowledge Work Systems (KWS)            | <- Knowledge level
  |  Office Automation Systems (OAS)         |
  +------------------------------------------+
  |  Transaction Processing Systems (TPS)    | <- Operational level
  +------------------------------------------+
```

#### Types Explained:

| System | Level | Purpose | Example |
|--------|-------|---------|---------|
| **TPS** | Operational | Record daily transactions | ATM, POS, payroll |
| **MIS** | Management | Reports for middle managers | Sales reports, inventory |
| **DSS** | Management | Help semi-structured decisions | Financial modeling |
| **ESS** | Strategic | Long-term strategic decisions | Dashboard KPIs |
| **KWS** | Knowledge | Support knowledge workers | CAD, scientific tools |
| **OAS** | Knowledge | Support clerical workers | Word processors, email |

---

### 8.3 Management Information System (MIS) ⭐ [BUET Fill-in 2017]

**Definition:** MIS is a computer-based system that provides managers with tools to **organize, evaluate, and efficiently manage** a department or the entire organization.

**Key Characteristics:**
- Collects data from **Transaction Processing Systems (TPS)**
- Produces **structured, periodic reports** (daily/weekly/monthly)
- Supports **operational and tactical decision-making**
- Data is internally oriented

**Components of MIS:**
1. **People** — the most important factor ⭐ [BUET Fill-in 2017]
2. Data/Information
3. Hardware
4. Software
5. Procedures/Processes

> **[BUET FILL-IN]** "An important factor of management information system" = **People** (human factor)

**MIS Functions:**
```
Data Collection -> Data Processing -> Information Storage -> Information Retrieval -> Report Generation
```

---

### 8.4 Decision Support System (DSS)

**Purpose:** Helps managers make **semi-structured** decisions using:
- Interactive queries
- Data analysis tools
- Simulation models

**Example:** A sales manager uses DSS to analyze "What will happen to profits if we discount by 15%?" (what-if analysis)

**DSS Components:**
1. **Database** — data storage
2. **Model Base** — analytical models (statistical, financial)
3. **User Interface** — for interaction

---

### 8.5 Enterprise Resource Planning (ERP)

Integrates all business functions into one unified system:
- Finance, HR, Supply Chain, Sales, Manufacturing
- Examples: SAP, Oracle ERP, Microsoft Dynamics

---

## 9. Data Flow Diagrams (DFD)

### 9.1 What is a DFD?

A **Data Flow Diagram (DFD)** represents the **flow of information** through a system. It shows:
- **Where data comes from** (external entities)
- **Where it goes** (processes, stores, entities)
- **What transformations** happen to it

---

### 9.2 DFD Symbols ⭐ [BUET Fill-in 2017]

| Symbol | Shape | Represents |
|--------|-------|-----------|
| **External Entity** | **Rectangle (square)** ⭐ | Source/Sink of data (outside system) |
| **Process** | Circle or Rounded Rectangle | Transformation/action on data |
| **Data Store** | Open-ended rectangle | Where data is stored (DB, file) |
| **Data Flow** | Arrow | Movement of data |

> **[BUET FILL-IN]** "In a DFD, external entities are represented by a **rectangle (square)**"

---

### 9.3 DFD Levels

| Level | Name | Description |
|-------|------|-------------|
| **Level 0** | Context Diagram | Entire system as one process with external entities |
| **Level 1** | Top-level DFD | Major processes broken down |
| **Level 2+** | Detailed DFD | Each process further decomposed |

#### Example — Library System DFD (Level 0 / Context Diagram):

```
                   +--------------------+
   Book Request    |                    |   Catalog Info
 ----------------> |   Library          | ----------------> Librarian
                   |   Management       |
 <---------------- |   System           | <---------------- Admin
   Fine Notice     |                    |   Reports
                   +--------------------+
     Member                                Publisher
     -----------------> (issues/returns)
```

---

### 9.4 DFD Rules:
- A process must have **at least one input and one output**
- A data store cannot connect **directly to another data store** (must go through a process)
- External entities cannot connect **directly to other external entities**
- A data flow arrow must be **labeled** (named)

---

## 10. System Analysis & Design

### 10.1 System Analysis
Process of **studying** an existing system to understand its problems and requirements:
1. Problem identification
2. Feasibility study
3. Requirements gathering
4. Requirements analysis

### 10.2 Feasibility Study
Determines whether the project is worth pursuing:

| Type | Question |
|------|---------|
| **Technical Feasibility** | Can we build it with available technology? |
| **Economic Feasibility** | Is it cost-effective? (ROI) |
| **Operational Feasibility** | Will it work in the organization? |
| **Legal Feasibility** | Are there legal/regulatory issues? |
| **Schedule Feasibility** | Can it be done in time? |

---

### 10.3 System Design Phases
1. **Logical Design** — What the system should do (independent of technology)
2. **Physical Design** — How the system will be implemented (hardware, software, DB)

---

### 10.4 Object-Oriented Analysis & Design (OOAD)

Key OOP Concepts:
| Concept | Description |
|---------|-------------|
| **Encapsulation** | Hide internal details; expose only interface |
| **Inheritance** | Child class inherits from parent |
| **Polymorphism** | One interface, multiple implementations |
| **Abstraction** | Show only essential details |

**SOLID Principles:**
| Principle | Meaning |
|-----------|---------|
| **S** Single Responsibility | A class should have only one reason to change |
| **O** Open/Closed | Open for extension, closed for modification |
| **L** Liskov Substitution | Subclasses should be replaceable for base classes |
| **I** Interface Segregation | Don't force clients to implement unused methods |
| **D** Dependency Inversion | Depend on abstractions, not concretions |

---

## 11. Quick Reference & Fill-in-the-Blanks

### BUET Exam Quick Answers:

| Question | Answer |
|----------|--------|
| Important factor of MIS | **People** |
| DFD external entities represented by | **Rectangle (Square)** |
| Using new + old systems simultaneously | **Parallel Running** |
| Document after design phase | **Design Document Specification (DDS)** |
| Programming in SDLC happens during | **Implementation Phase** |
| "Can't deny editing after deadline" = | **Non-repudiation** |
| Multiple users need unique sequence numbers | **Singleton Pattern** |
| PERT critical path characteristic | **Slack = 0** |
| Waterfall goes back easily | **No (inflexible)** |
| Software maintenance longest phase | **Maintenance** |
| Alpha testing done by | **Internal testers at developer site** |
| Beta testing done by | **Real users in real environment** |
| ATM primary actor | **Customer** |
| Include relationship | **Mandatory / always included** |
| Extend relationship | **Optional / conditionally added** |

---

### Comparison Table: SDLC Models

| Model | Flexibility | Customer Involvement | Best For |
|-------|------------|---------------------|---------|
| Waterfall | Low | Only at start | Stable, well-known requirements |
| Iterative | Medium | Each iteration | Evolving requirements |
| Spiral | High | Yes | High-risk, large projects |
| Agile | Very High | Continuous | Fast-changing requirements |

---

### Testing Types Quick Reference:

| Type | Technique | Who |
|------|-----------|-----|
| Unit | White box | Developer |
| Integration | Both | Developer/Tester |
| System | Black box | QA Team |
| Acceptance | Black box | Client |
| Regression | Automated | QA/Dev |

---

### UML Diagrams Quick Reference:

| Diagram | Shows | Key Element |
|---------|-------|------------|
| Use Case | System functionality + actors | Actor, Use Case, Include, Extend |
| Class | Static structure | Attributes, Methods, Relationships |
| Sequence | Interactions over time | Lifelines, Messages |
| State | Object state transitions | States, Transitions, Events |
| Activity | Workflow / process | Actions, Decisions, Forks |

---

### Design Patterns Summary:

| Pattern | Category | Problem | Solution |
|---------|---------|---------|---------|
| Singleton | Creational | Need one instance | Private constructor + static instance |
| Factory | Creational | Object creation without knowing class | Factory method |
| Strategy | Behavioral | Switch algorithms at runtime | Encapsulate each algorithm |
| Observer | Behavioral | One-to-many notification | Subject notifies all observers |
| Decorator | Structural | Add behavior dynamically | Wrap object with decorator |

---

## Key Formulas & Definitions

### PERT Formulas:
```
Expected Time = (Optimistic + 4 x Most Likely + Pessimistic) / 6
Variance      = ((Pessimistic - Optimistic) / 6)^2
Slack         = Latest Start - Earliest Start
Critical Path = Path with Slack = 0
```

### BVA Boundaries:
```
For range [min, max]:
Test values: min-1, min, min+1, max-1, max, max+1
```

### Confusion Matrix Metrics: ⭐ [BUET CSE 2021]
```
                Predicted +    Predicted -
Actual +    |     TP         |     FN      |
Actual -    |     FP         |     TN      |

Precision   = TP / (TP + FP)   <- Of predicted positives, how many are correct
Recall      = TP / (TP + FN)   <- Of actual positives, how many were caught
(Sensitivity = Recall)
Accuracy    = (TP + TN) / (TP + TN + FP + FN)
F1 Score    = 2 x (Precision x Recall) / (Precision + Recall)
```

---

## Final Exam Cheat Sheet

```
=========================================================
        SOFTWARE ENGINEERING -- EXAM CHEAT SHEET
=========================================================

SDLC PHASES (Waterfall):
  Requirements -> Design -> Implementation -> Testing
  -> Deployment -> Maintenance

DOCUMENT AFTER DESIGN = Design Document Specification (DDS)
PROGRAMMING PHASE     = Implementation
PARALLEL RUNNING      = Old + New systems simultaneously

UML RELATIONSHIPS:
  <<include>> = ALWAYS happens (mandatory)
  <<extend>>  = OPTIONALLY happens (conditional)
  Generalization = Inheritance (actor or use case)

DESIGN PATTERNS:
  Singleton  = One instance only (sequence generator!)
  Factory    = Create objects without knowing class
  Strategy   = Select algorithm at runtime

SECURITY:
  Non-repudiation = Cannot deny doing an action (IMPORTANT!)
  CIA = Confidentiality + Integrity + Availability

TESTING:
  BVA for [8-12] = test: 7, 8, 9, 11, 12, 13
  Unit Test = smallest unit, isolated
  Acceptance = by customer

IS TYPES:
  TPS -> MIS -> DSS -> ESS (bottom to top of org)
  MIS important factor = PEOPLE

DFD:
  External Entity = Rectangle/Square
  Process         = Circle
  Data Store      = Open rectangle (two parallel lines)
  Data Flow       = Arrow

PERT:
  Critical Path = Slack = 0
  Slack = LST - EST

=========================================================
```

---

*Notes compiled for BUET MSc Admission Exam — Software Engineering & Information System topic.*
*Sources: BUET MSc past papers (2015-2024)*

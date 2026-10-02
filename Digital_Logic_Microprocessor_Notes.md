# 🔌 Digital Logic Design, Microprocessors & Microcontrollers
## BUET MSc Exam — Complete Notes (Single Source of Truth)

> **Time is short — focus only on what's marked ⭐ (high probability) or 🔥 (appeared in past papers)**
> All past BUET MSc questions from the archive are fully covered here.

---

## 📋 TABLE OF CONTENTS

**Part A — Digital Logic Design**
1. [Number Systems & Boolean Algebra](#1-number-systems--boolean-algebra)
2. [Logic Gates & Universal Gates](#2-logic-gates--universal-gates)
3. [Karnaugh Maps (K-Map)](#3-karnaugh-maps-k-map)
4. [Combinational Circuits](#4-combinational-circuits)
5. [Flip-Flops & Latches](#5-flip-flops--latches)
6. [Sequential Circuits — Counters & Registers](#6-sequential-circuits--counters--registers)
7. [State Machines (Mealy & Moore)](#7-state-machines-mealy--moore)

**Part B — Microprocessors**
8. [Microprocessor vs Microcontroller](#8-microprocessor-vs-microcontroller)
9. [8086 Architecture & Registers](#9-8086-architecture--registers)
10. [Memory Segmentation & Physical Address](#10-memory-segmentation--physical-address)
11. [Interrupts in 8086](#11-interrupts-in-8086)
12. [Fetch & Execute Cycle / Timing Diagram](#12-fetch--execute-cycle--timing-diagram)
13. [Superscalar & Pipelining (Pentium vs 80386)](#13-superscalar--pipelining-pentium-vs-80386)
14. [Assembly Language — Key Instructions](#14-assembly-language--key-instructions)

**Part C — Microcontrollers**
15. [Microcontroller Overview (8051 & AVR)](#15-microcontroller-overview-8051--avr)
16. [Daisy-Chain Priority Interrupts](#16-daisy-chain-priority-interrupts)
17. [Quick MCQ Fact Sheet](#17-quick-mcq-fact-sheet)

---

# PART A — DIGITAL LOGIC DESIGN

---

## 1. Number Systems & Boolean Algebra

### 🔥 Number System Conversions (MCQ bread-and-butter)

| System | Base | Digits |
|--------|------|--------|
| Binary | 2 | 0, 1 |
| Octal | 8 | 0–7 |
| Decimal | 10 | 0–9 |
| Hexadecimal | 16 | 0–9, A–F |

**Key Conversions:**
- Binary → Hex: group bits in 4s from right: `1011 0110` = `B6`
- Hex → Binary: expand each hex digit to 4 bits: `3A` = `0011 1010`
- BCD (Binary Coded Decimal): each decimal digit encoded in 4 bits: `97` in BCD = `1001 0111`

### ⭐ Boolean Algebra Laws (MCQ — know these cold)

| Law | Expression |
|-----|------------|
| Identity | A+0=A, A·1=A |
| Null | A+1=1, A·0=0 |
| Idempotent | A+A=A, A·A=A |
| Complement | A+A'=1, A·A'=0 |
| Double Complement | (A')'=A |
| Commutative | A+B=B+A |
| Associative | (A+B)+C=A+(B+C) |
| Distributive | A(B+C)=AB+AC |
| **De Morgan's** | **(AB)'=A'+B'** and **(A+B)'=A'·B'** |
| Absorption | A+AB=A, A(A+B)=A |

### ⭐ De Morgan's Theorem (Most Important)
```
(A · B)' = A' + B'    ← NAND becomes OR with inverted inputs
(A + B)' = A' · B'    ← NOR becomes AND with inverted inputs
```
**Trick to remember:** Break the bar, change the sign (AND↔OR).

---

## 2. Logic Gates & Universal Gates

### Basic Gates Truth Table

```
AND Gate:            OR Gate:             NOT Gate:
A B | A·B           A B | A+B            A | A'
0 0 |  0            0 0 |  0             0 |  1
0 1 |  0            0 1 |  1             1 |  0
1 0 |  0            1 0 |  1
1 1 |  1            1 1 |  1

NAND Gate:           NOR Gate:            XOR Gate:
A B | (AB)'          A B | (A+B)'         A B | A⊕B
0 0 |  1             0 0 |  1             0 0 |  0
0 1 |  1             0 1 |  0             0 1 |  1
1 0 |  1             1 0 |  0             1 0 |  1
1 1 |  0             1 1 |  0             1 1 |  0

XNOR Gate:
A B | A⊙B
0 0 |  1    ← outputs 1 when inputs are EQUAL
0 1 |  0
1 0 |  0
1 1 |  1
```

### ⭐ Universal Gates — NAND and NOR (High MCQ probability)

**NAND is universal** — can implement ANY Boolean function:

```
NOT using NAND:            AND using NAND (2 NANDs):
  A--+--[NAND]-- A'          A--[NAND]--+--[NAND]-- A·B
  A--+                       B--+       +-- (tie both inputs)
  (tie both inputs)

OR using NAND (3 NANDs):
  A--[NAND(A,A)]--+
                  [NAND]-- A+B
  B--[NAND(B,B)]--+
  (invert both, then NAND them)
```

**NOR is universal** — similarly, NOR can implement NOT, OR, AND.

```
NOT using NOR:           OR using NOR (2 NORs):
  A--+--[NOR]-- A'         A--[NOR]--+--[NOR(x,x)]-- A+B
  A--+                     B--+
  (tie both inputs)
```

> **MCQ Tip:** NAND and NOR are called **universal** because any logic function can be realized using only these gates.

---

## 3. Karnaugh Maps (K-Map)

### ⭐ K-Map — Most Common Written Exam Topic

**Purpose:** Simplify Boolean expressions graphically (minimize gates needed).

### 3-Variable K-Map Layout
```
         AB
  C  | 00 | 01 | 11 | 10 |
  0  | m0 | m2 | m6 | m4 |
  1  | m1 | m3 | m7 | m5 |
```
Note: Column order 00→01→11→10 uses Gray code (only 1 bit changes).

### 4-Variable K-Map Layout (Most tested)
```
            CD
  AB  | 00 | 01 | 11 | 10 |
  00  |  0 |  1 |  3 |  2 |
  01  |  4 |  5 |  7 |  6 |
  11  | 12 | 13 | 15 | 14 |
  10  |  8 |  9 | 11 | 10 |
```

### Grouping Rules (MEMORIZE)
1. Groups must contain **1, 2, 4, 8, or 16** cells (powers of 2 only)
2. All cells in group must have **value 1** (for SOP minimization)
3. Groups must be **rectangular** — wrapping around edges is allowed
4. Make groups as **large as possible**
5. Use **minimum number of groups** to cover all 1s
6. Every 1 must be covered by at least one group
7. **Don't care (X)** cells can be included to make larger groups

### Worked Example ⭐
```
Minimize: F(A,B,C,D) = Σm(0,1,2,5,8,9,10)

4-variable K-Map:
            CD
  AB  | 00 | 01 | 11 | 10 |
  00  |  1 |  1 |  0 |  1 |   ← m0, m1, m2
  01  |  0 |  1 |  0 |  0 |   ← m5
  11  |  0 |  0 |  0 |  0 |
  10  |  1 |  1 |  0 |  1 |   ← m8, m9, m10

Groups:
Group 1 (m0,m2,m8,m10): These are cells where B=0, D=0 → B'D'
Group 2 (m0,m1,m8,m9): These are cells where B=0, C=0 → B'C'
Group 3 (m1,m5):        These have A=0, C=0, D=1 → A'C'D

Minimal SOP: F = B'D' + B'C' + A'C'D
```

### SOP vs POS
- **SOP (Sum of Products):** Group the **1s** → e.g., F = AB + A'C
- **POS (Product of Sums):** Group the **0s** → e.g., F = (A+B)(A'+C)

---

## 4. Combinational Circuits

### ⭐ Half Adder
```
Inputs: A, B
Outputs: Sum (S), Carry (C_out)

Truth Table:
A  B  | S   C_out
0  0  | 0     0
0  1  | 1     0
1  0  | 1     0
1  1  | 0     1

Equations: S = A XOR B,   C_out = A AND B

Circuit Diagram:
A ---+---[XOR]--- S
     |
B ---+---[AND]--- C_out
```

### ⭐ Full Adder
```
Inputs: A, B, Cin (carry-in)
Outputs: Sum (S), Cout (carry-out)

Truth Table:
A  B  Cin | S    Cout
0  0   0  | 0     0
0  0   1  | 1     0
0  1   0  | 1     0
0  1   1  | 0     1
1  0   0  | 1     0
1  0   1  | 0     1
1  1   0  | 0     1
1  1   1  | 1     1

Equations:
  S    = A XOR B XOR Cin
  Cout = AB + BCin + ACin  (carry if any two inputs are 1)

Implementation: Two Half Adders + OR gate
  HA1: inputs (A, B) → Sum1, C1
  HA2: inputs (Sum1, Cin) → S, C2
  Cout = C1 OR C2
```

### ⭐ Multiplexer (MUX)
```
A MUX selects one of N inputs to pass to output based on select lines.

2-to-1 MUX (1 select line S):
  Y = S'·I0 + S·I1
  S=0 → Y=I0,  S=1 → Y=I1

4-to-1 MUX (2 select lines S1, S0):
S1 S0 | Y
 0  0 | I0
 0  1 | I1
 1  0 | I2
 1  1 | I3

Y = S1'S0'·I0 + S1'S0·I1 + S1S0'·I2 + S1S0·I3

KEY FACT: A MUX can implement ANY Boolean function.
Example: Implement F(A,B,C) using 4-to-1 MUX (A,B as selects):
  - Evaluate F for each combination of A,B, leaving C as variable
  - Connect each I input to: 0, 1, C, or C' accordingly
```

### ⭐ Decoder
```
2-to-4 Decoder: 2 inputs → 4 outputs (only 1 active HIGH at a time)

Input: A, B
A  B  | D0  D1  D2  D3
0  0  |  1   0   0   0
0  1  |  0   1   0   0
1  0  |  0   0   1   0
1  1  |  0   0   0   1

Equations: D0=A'B', D1=A'B, D2=AB', D3=AB

Note: Each output is a minterm → Decoders naturally implement minterms.
Any Boolean function can be implemented with a decoder + OR gate.
```

### Encoder & Priority Encoder
```
4-to-2 Encoder: 4 inputs (one-hot) → 2 output bits
I3  I2  I1  I0 | A1  A0
 0   0   0   1 |  0   0
 0   0   1   0 |  0   1
 0   1   0   0 |  1   0
 1   0   0   0 |  1   1

Problem: Standard encoder fails if multiple inputs are HIGH simultaneously.
Solution: Priority Encoder — highest-numbered input has highest priority.
```

---

## 5. Flip-Flops & Latches

### Latch vs Flip-Flop
| Feature | Latch | Flip-Flop |
|---------|-------|-----------|
| Trigger | Level-triggered | Edge-triggered (↑ or ↓) |
| Active when | Clock is HIGH | Only at clock transition |
| Stability | Can be transparent (unstable loop risk) | Stable, synchronized |
| Use | Temporary storage | Registers, counters, FSMs |

### ⭐ SR Flip-Flop
```
Inputs: S (Set), R (Reset), CLK
Output: Q and Q'

Characteristic Table:
S  R  | Q(next)  | Notes
0  0  |    Q     | Hold (no change)
0  1  |    0     | Reset
1  0  |    1     | Set
1  1  |    ?     | FORBIDDEN (indeterminate)

Excitation Table (needed for circuit design):
Q  → Q(next) | S   R
0  →   0     | 0   X  (X = don't care)
0  →   1     | 1   0
1  →   0     | 0   1
1  →   1     | X   0
```

### ⭐ JK Flip-Flop (Most versatile)
```
Inputs: J (like S), K (like R), CLK
Output: Q and Q'

Characteristic Table:
J  K  | Q(next)  | Notes
0  0  |    Q     | Hold
0  1  |    0     | Reset
1  0  |    1     | Set
1  1  |    Q'    | TOGGLE ← This is the KEY difference from SR!

Excitation Table:
Q  → Q(next) | J   K
0  →   0     | 0   X
0  →   1     | 1   X
1  →   0     | X   1
1  →   1     | X   0

(X = don't care)

Characteristic Equation: Q(next) = JQ' + K'Q
```

### ⭐ D Flip-Flop (Data / Delay)
```
Input: D, CLK
Output: Q

Characteristic Table:
D  | Q(next)
0  |   0      ← Q simply follows D at clock edge
1  |   1

Excitation Table:
Q  → Q(next) | D
0  →   0     | 0
0  →   1     | 1
1  →   0     | 0
1  →   1     | 1

Characteristic Equation: Q(next) = D

Most commonly used for registers and data storage.
Simplest flip-flop to use for sequential circuit design.
```

### ⭐ T Flip-Flop (Toggle)
```
Input: T, CLK
Output: Q

Characteristic Table:
T  | Q(next)
0  |    Q    ← Hold (no change)
1  |    Q'   ← Toggle

Excitation Table:
Q  → Q(next) | T
0  →   0     | 0
0  →   1     | 1
1  →   0     | 1
1  →   1     | 0

Characteristic Equation: Q(next) = T XOR Q = TQ' + T'Q

Primary use: Counters (T=1 always → divides clock by 2)
```

### Key Equations Summary
```
SR FF:  Q(next) = S + R'Q    (constraint: S·R = 0)
JK FF:  Q(next) = JQ' + K'Q
D FF:   Q(next) = D
T FF:   Q(next) = T XOR Q
```

---

## 6. Sequential Circuits — Counters & Registers

### ⭐ Ripple Counter (Asynchronous)
```
3-bit UP counter (using T flip-flops with T=1, i.e., toggle on every clock):

Clock → [TFF0] → Q0 → [TFF1] → Q1 → [TFF2] → Q2

FF1's clock = Q0,  FF2's clock = Q1  (output of previous FF clocks next FF)

Count sequence:
Q2 Q1 Q0:  000 → 001 → 010 → 011 → 100 → 101 → 110 → 111 → 000...

Problem: Propagation delay accumulates across flip-flops
(not all bits update simultaneously → "ripple" effect)
Max frequency = 1 / (n × t_pd)  where n=stages, t_pd=FF delay
```

### ⭐ Synchronous Counter
```
All flip-flops share the SAME clock → no ripple delay.

3-bit synchronous UP counter (JK FFs):
  FF0 (LSB): J0 = K0 = 1                  (always toggles)
  FF1:       J1 = K1 = Q0                 (toggles when Q0=1)
  FF2 (MSB): J2 = K2 = Q0 AND Q1          (toggles when Q0=1 AND Q1=1)

Design steps:
1. Make state/transition table
2. Determine required J,K inputs for each FF using JK excitation table
3. Minimize with K-map
4. Draw circuit

Advantage over ripple: All bits update simultaneously → faster, more reliable.
```

### Shift Registers
```
Types:
1. SISO (Serial In, Serial Out)  — basic delay line
2. SIPO (Serial In, Parallel Out) — serial-to-parallel conversion
3. PISO (Parallel In, Serial Out) — parallel-to-serial conversion
4. PIPO (Parallel In, Parallel Out) — simple register/buffer

4-bit SISO (4 D flip-flops in series):
  DataIn → [D FF0] → [D FF1] → [D FF2] → [D FF3] → DataOut
             CLK        CLK       CLK        CLK  (all same clock)

Each clock pulse shifts all data one position to the right.
After 4 pulses, data appears at output.
```

### MOD-N Counter Design
```
MOD-N counter: counts 0 to N-1, then resets to 0.
Number of FFs needed: smallest k such that 2^k >= N

Example: MOD-6 counter → k=3 (2^3=8 >= 6), uses 3 FFs
Strategy: 
  Option 1 (Synchronous Clear): Use NAND to detect state 6 (Q2=1,Q1=1,Q0=0)
           and feed to active-low CLR of all FFs simultaneously.
  Option 2: Preset the excitation equations to skip states 6 and 7.

Decade Counter (MOD-10 / BCD counter):
  4 FFs, detect state 10 (1010), reset. Widely used in digital clocks.
```

---

## 7. State Machines (Mealy & Moore)

### ⭐ Mealy vs Moore Machine

| Feature | Mealy Machine | Moore Machine |
|---------|--------------|---------------|
| Output depends on | Current state + Input | Current state only |
| Output changes | Immediately with input (async) | With clock (sync) |
| States needed | Fewer states | More states |
| Speed of response | Faster (1 clock earlier) | One clock cycle delayed |
| Notation | Output on transition arrow | Output inside state circle |
| Preferred for | Faster response systems | Safer, glitch-free output |

### Sequential Circuit Design Procedure
```
Step 1: Understand — identify inputs, outputs, states
Step 2: Draw state diagram (circles=states, arrows=transitions/outputs)
Step 3: Create state table (Present State, Input → Next State, Output)
Step 4: State encoding (assign binary codes to each state)
Step 5: Create excitation table (using chosen FF type's excitation table)
Step 6: Minimize using K-map to find FF input equations
Step 7: Derive output equations
Step 8: Draw final circuit
```

### Example — Sequence Detector (Moore Machine, detects "101")
```
States: S0=initial, S1=received "1", S2=received "10", S3=received "101"

State Diagram:
             0              1             0              0/1
    S0 ──────────→ S0    S0 ──→ S1    S1 ──→ S2    S2 ──→ S0 (output=0)
    (output=0)           (out=0)      (out=0)
                                               1
                                      S2 ──→ S3 (output=1, detected!)
                                      S3 → S1 if input=1 (overlap handling)
                                      S3 → S2 if input=0

State Table:
State | Input=0 | Input=1 | Output
  S0  |   S0    |   S1    |   0
  S1  |   S2    |   S1    |   0
  S2  |   S0    |   S3    |   0
  S3  |   S2    |   S1    |   1    ← output HIGH when in S3
```

---

# PART B — MICROPROCESSORS

---

## 8. Microprocessor vs Microcontroller

### 🔥 Key Differences (Asked in BUET MSc 15, 16, 17, 19, 21)

| Feature | Microprocessor | Microcontroller |
|---------|---------------|-----------------|
| **What it is** | CPU only on a chip | Complete computer on a chip |
| **Contains** | CPU only; needs external RAM, ROM, I/O | CPU + RAM + ROM + I/O + Timers on-chip |
| **Architecture** | Mostly Von Neumann | Mostly Harvard |
| **Application** | General purpose (PCs, servers) | Specific/embedded tasks |
| **Cost** | Higher (needs many external ICs) | Lower (all integrated) |
| **Power** | Higher consumption | Lower (embedded-optimized) |
| **PCB complexity** | Complex (many chips) | Simple (single chip) |
| **Examples** | Intel 8086, Pentium, Core i7 | Intel 8051, AVR ATmega, PIC |

### ⭐ Advantages of Microcontroller over Microprocessor (BUET MSc 15, 16)
1. **Self-contained system** — CPU, RAM, ROM, I/O ports, Timers all on one chip; no external components needed
2. **Lower cost** — single chip vs. multi-chip board
3. **Lower power consumption** — critical for battery-powered embedded devices
4. **Compact size** — single-chip design is smaller
5. **Higher reliability** — fewer external connections = fewer failure points
6. **Faster development** — simpler hardware design
7. **Built-in real-time control** — hardware timers, interrupt controllers for responsive control
8. **Harvard architecture** — separate buses for code and data allow simultaneous fetch

### ⭐ 8-bit vs 16-bit Microprocessor (PGCB SAE 21)
```
8-bit Microprocessor:
  - Data bus: 8 bits → processes 8 bits per clock cycle
  - Registers: 8-bit wide (e.g., accumulator is 8-bit)
  - ALU: 8-bit operations (larger numbers need multiple operations)
  - Address bus: typically 16-bit → max 64KB addressable memory
  - Examples: Intel 8085, Motorola 6800, Zilog Z80

16-bit Microprocessor:
  - Data bus: 16 bits → processes 16 bits per clock cycle (2× more)
  - Registers: 16-bit wide (can split into 8-bit halves, e.g., AX→AH+AL)
  - ALU: 16-bit operations → faster for 16-bit data
  - Address bus: 20-bit (8086) → 1 MB addressable memory (16× more)
  - Supports more complex operations natively
  - Examples: Intel 8086, 80286, Motorola 68000

Key difference: A 16-bit processor can process twice as much data per
clock cycle and address significantly more memory than an 8-bit processor.
```

---

## 9. 8086 Architecture & Registers

### 🔥 8086 Architecture Overview (Appeared in multiple BUET papers)

```
╔══════════════════════════════════════════════════════════╗
║                    8086 CPU ARCHITECTURE                 ║
║                                                          ║
║  ┌─────────────────────┐    ┌────────────────────────┐  ║
║  │      BIU            │    │     EU                 │  ║
║  │  (Bus Interface     │    │  (Execution Unit)      │  ║
║  │   Unit)             │    │                        │  ║
║  │                     │    │  ┌──────────────────┐  │  ║
║  │  ┌───────────────┐  │    │  │ General Registers│  │  ║
║  │  │ Instruction   │  │◄───│  │ AX,BX,CX,DX     │  │  ║
║  │  │ Queue (6 byte)│  │    │  │ SP,BP,SI,DI     │  │  ║
║  │  └───────────────┘  │    │  └──────────────────┘  │  ║
║  │                     │    │                        │  ║
║  │  ┌───────────────┐  │    │  ┌──────────────────┐  │  ║
║  │  │ Segment Regs  │  │    │  │ ALU              │  │  ║
║  │  │ CS,DS,SS,ES   │  │    │  │ (Arithmetic &   │  │  ║
║  │  └───────────────┘  │    │  │  Logic Unit)    │  │  ║
║  │                     │    │  └──────────────────┘  │  ║
║  │  ┌───────────────┐  │    │                        │  ║
║  │  │ IP Register   │  │    │  ┌──────────────────┐  │  ║
║  │  └───────────────┘  │    │  │ Flags Register   │  │  ║
║  └─────────────────────┘    │  │ CF,ZF,SF,OF...  │  │  ║
║                             │  └──────────────────┘  │  ║
║                             └────────────────────────┘  ║
║                                                          ║
║  ← 20-bit Address Bus (A0–A19) →                        ║
║  ← 16-bit Data Bus (D0–D15)   →                        ║
╚══════════════════════════════════════════════════════════╝

KEY FEATURE: BIU prefetches instructions into 6-byte queue while EU
executes — this overlap is 8086's internal pipeline.
```

### ⭐ 8086 Register Set (BUET MSc ICT 15)

#### General Purpose Registers (16-bit, splittable into 8-bit halves)
```
AX = [AH | AL]  — Accumulator: I/O, multiply/divide, string operations
BX = [BH | BL]  — Base: memory addressing (base address register)
CX = [CH | CL]  — Count: loop counter, shift/rotate count (REP prefix)
DX = [DH | DL]  — Data: I/O port addressing, MUL/DIV overflow storage
```

#### Segment Registers (16-bit) — 🔥 BUET MSc ICT 15
```
CS — Code Segment:   Holds segment address of current code.
                     Next instruction physical addr = CS×10H + IP
DS — Data Segment:   Holds segment address of data (variables, arrays).
                     Data physical addr = DS×10H + effective address
SS — Stack Segment:  Holds segment address of stack.
                     Stack physical addr = SS×10H + SP
ES — Extra Segment:  Additional data segment, used mainly for string
                     operations (MOVS, CMPS). Destination = ES×10H + DI
```

**Purpose of Segment Registers (Written Answer — BUET MSc ICT 15):**
- **CS (Code Segment):** Points to the segment containing the program code. The processor always fetches the next instruction from the address CS×10H + IP. You cannot manually write to CS normally during execution.
- **DS (Data Segment):** Points to the segment where program data (variables) reside. Most data access instructions default to DS. E.g., `MOV AX, [1000H]` reads from DS:1000H.
- **SS (Stack Segment):** Points to the segment used for the stack. All PUSH, POP, CALL, RET operations use SS:SP for the stack top. The stack grows downward (SP decrements on PUSH).
- **ES (Extra Segment):** A second data segment used as destination in string operations. `MOVS` copies from DS:SI to ES:DI.

#### Pointer and Index Registers
```
SP — Stack Pointer:      Offset of top of stack within SS segment
BP — Base Pointer:       For accessing stack frame data (parameters, locals)
SI — Source Index:       Source address in string operations (DS:SI)
DI — Destination Index:  Destination address in string operations (ES:DI)
IP — Instruction Pointer: Offset of next instruction within CS segment
```

#### Flag Register (16-bit — 9 flags used)
```
Bit: 15 14 13 12 | 11 | 10 |  9 |  8 |  7 |  6 |  5 |  4 |  3 |  2 |  1 |  0
Flag:  -  -  -  -   OF   DF   IF   TF   SF   ZF   -    AF   -    PF   -    CF

STATUS FLAGS (set by ALU results):
CF — Carry Flag:     Set if carry out of MSB (unsigned overflow)
PF — Parity Flag:    Set if result has even number of 1-bits
AF — Auxiliary CF:   Carry from bit 3 to 4 (for BCD arithmetic)
ZF — Zero Flag:      Set if result is ZERO  ← most commonly tested
SF — Sign Flag:      Set if result is NEGATIVE (MSB=1)
OF — Overflow Flag:  Set if signed arithmetic overflow

CONTROL FLAGS:
TF — Trap Flag:      1 = single-step mode (generates INT 1 after each instr)
IF — Interrupt Flag: 1 = INTR interrupts enabled; 0 = disabled (STI/CLI)
DF — Direction Flag: 0 = string ops auto-increment SI/DI; 1 = decrement
```

---

## 10. Memory Segmentation & Physical Address

### ⭐ Why Segmentation?
```
PROBLEM: 8086 has 16-bit registers but needs to address 1 MB of memory
         (1 MB needs 20-bit addresses, but registers are only 16-bit)

SOLUTION: Segmented Memory Model
  Physical Address = Segment Register × 16 + Offset
                   = (Segment << 4) + Offset
                   = Segment × 10H + Offset

Example Calculation:
  CS = 2000H, IP = 0150H
  Physical Address = 2000H × 10H + 0150H
                   = 20000H + 0150H
                   = 20150H  (20-bit physical address)

Range: 00000H to FFFFFH = 1,048,576 bytes = 1 MB

Segment size: Each segment can be max 64KB (16-bit offset: 0000H–FFFFH)
Segments can overlap! Same physical address can have multiple logical addresses.
```

### Memory Map of 8086 System
```
Physical Address | Contents
─────────────────────────────────────────────────────
00000H – 003FFH  | Interrupt Vector Table (IVT) — 1024 bytes
00400H – 004FFH  | BIOS Data Area
00500H and above | Free for DOS, programs, and data
A0000H – BFFFFH  | Video memory (CGA/EGA/VGA)
C0000H – FFFFFH  | ROM BIOS (128KB)
```

---

## 11. Interrupts in 8086

### 🔥 Types of Interrupts (BUET MSc ICT 15, 16, 19, CSE 21)

```
INTERRUPT CLASSIFICATION IN 8086
═════════════════════════════════════════════════════
                    Interrupts
                   /           \
          Hardware               Software
         /        \              (INT n instruction)
      External    Internal
      /      \     (CPU-generated)
   INTR      NMI       |
(maskable) (non-    ┌──┴──────────────┐
           maskable)│                 │
                 Divide-by-0    Single-step
                 (Type 0)       (Type 1, TF=1)
                    │
                  INTO (Type 4, overflow)
                  INT 3 (breakpoint)
```

#### 1. Hardware Interrupts
- **INTR (Interrupt Request Pin):**
  - **Maskable** — can be disabled by clearing IF flag using CLI instruction
  - Enabled by STI instruction
  - External device signals CPU via this pin
  - CPU acknowledges via INTA signal; device then puts interrupt type number on data bus
  
- **NMI (Non-Maskable Interrupt):**
  - **Cannot be disabled** regardless of IF flag
  - Used for catastrophic events: power failure, memory error, hardware fault
  - Always assigned to Type 2; always serviced immediately

#### 2. Software Interrupts
- **INT n:** 8-bit type number (n = 0 to 255). Explicitly coded by programmer. `INT 21H` is DOS services interrupt. Works same as hardware interrupt except it's always serviced (no masking).
- **INTO (Interrupt on Overflow):** Only triggers if OF=1 after arithmetic operation. Type 4.
- **INT 3 (Breakpoint):** 1-byte instruction, used by debuggers to set breakpoints.

#### 3. Internal / Exception Interrupts (CPU-generated)
- **Type 0 — Divide by Zero:** Triggered when DIV/IDIV divisor=0 or quotient too large
- **Type 1 — Single-step:** Generated after each instruction when TF=1 (debugger mode)
- **Type 4 — Overflow:** Same as INTO

### ⭐ Interrupt Vector Table (IVT)
```
Location: Physical addresses 00000H to 003FFH (first 1 KB of RAM)
Total entries: 256 interrupt vectors
Each entry: 4 bytes → [IP_low, IP_high, CS_low, CS_high]

To find ISR for Interrupt Type N:
  Vector table address = N × 4
  ISR IP = word at address (N × 4)
  ISR CS = word at address (N × 4 + 2)
  Physical address of ISR = ISR_CS × 10H + ISR_IP

Example: INT 21H (type 33 decimal):
  Vector address = 33 × 4 = 132 = 0084H
  Memory [0084H], [0085H] = IP of DOS ISR
  Memory [0086H], [0087H] = CS of DOS ISR
```

### ⭐ Interrupt Execution Steps (Written Answer — BUET MSc ICT 15, 16, 19)
```
When 8086 services an interrupt:

1. FINISH current instruction (complete the current machine cycle)
2. PUSH FLAGS register onto stack  → [SS:SP-2] = FLAGS, SP = SP-2
3. CLEAR IF flag (disable further maskable interrupts)
4. CLEAR TF flag (disable single-step mode)
5. PUSH CS register onto stack      → [SS:SP-2] = CS, SP = SP-2
6. PUSH IP register onto stack      → [SS:SP-2] = IP, SP = SP-2
7. FETCH ISR address from IVT at address (Type × 4)
8. LOAD IP and CS with ISR address → CPU jumps to ISR
9. EXECUTE the Interrupt Service Routine (ISR)
10. ISR ends with IRET instruction:
    IRET: POP IP, POP CS, POP FLAGS → resume interrupted program
```

### 🔥 Hardware Interrupt Priority in 8086 (BUET MSc CSE 21)
```
Priority (Highest → Lowest):
1. Internal interrupts (Divide-by-zero, INTO, Single-step)
2. NMI (Non-maskable interrupt)
3. INTR (Maskable hardware interrupt)
4. Single-step (lowest priority of hardware types)

Which are HARDWARE interrupts? Answer: INTR and NMI

INTR vs NMI comparison:
              INTR                    NMI
Maskable?     Yes (IF flag)           No
Priority?     Lower                   Higher
Pin?          INTR pin                NMI pin
Type number?  Provided by device      Fixed: Type 2
Use?          Normal peripherals      Critical failures
```

---

## 12. Fetch & Execute Cycle / Timing Diagram

### ⭐ Fetch-Execute Cycle Activities (BUET MSc ICT 16)

**FETCH Phase Activities:**
1. IP/PC contents placed on Address Bus
2. Memory Read (RD#) signal asserted LOW
3. Instruction byte fetched from memory via Data Bus
4. Instruction byte stored in Instruction Register (IR)
5. IP incremented to point to next instruction byte

**EXECUTE Phase Activities:**
1. Instruction Decoder decodes opcode in IR
2. Control Unit generates control signals for execution
3. Operands fetched from registers or memory (if needed — extra bus cycles)
4. ALU performs operation (add, subtract, logic, shift, etc.)
5. Result written to destination (register or memory)
6. Flag register updated based on result (ZF, SF, CF, OF, etc.)

### ⭐ Memory Read Timing Diagram — 4 Clock Cycles (BUET MSc ICT 15)
```
MEMORY READ CYCLE (8086 Minimum Mode)

         ___       ___       ___       ___
CLK:  __|   |_____|   |_____|   |_____|   |_____
         T1        T2        T3        T4

         ←A19-A16→ ←—— address stable ——→
A19-A16: ══════════════════════════════════════════

         ←— ADDRESS —→         ← DATA VALID →
AD15-0:  ════[Addr]════════════════[Data]════════
         (AD lines mux addr out then data in)

         ___
ALE:  __|   |____________________________________________
     (Address Latch Enable: HIGH in T1 to capture address
      in external latch, so address remains stable while
      AD lines switch to data mode in T3)

            _________________________
RD#:  _____|                         |_______________
      (Active LOW: asserted in T2, deasserted after T4)

Wait states (Tw): Inserted between T3 and T4 when memory
is slow (READY signal held LOW by memory controller).
Total cycle = T1 + T2 + T3 + Tw + T4 = (4 + Tw) cycles
```

**Answer to BUET question:** Minimum **4 clock cycles** (T1, T2, T3, T4). With slow memory, wait states are inserted: total = **4 + Tw** cycles.

---

## 13. Superscalar & Pipelining (Pentium vs 80386)

### 🔥 Why Pentium is Superscalar but 80386 is NOT (BUET MSc 16, 17)

**Definition — Superscalar:** A processor architecture that can dispatch and execute **more than one instruction per clock cycle** by having multiple independent execution units (pipelines) operating in parallel.

```
80386 — SCALAR (NOT superscalar):
┌────────────────────────────────────────────────────────┐
│  Single Integer Pipeline                               │
│  Fetch → Decode → Execute → Write-back                │
│                                                        │
│  Only 1 instruction can be in each stage at a time    │
│  Maximum throughput: 1 instruction/clock cycle         │
│  (or less for complex instructions)                    │
└────────────────────────────────────────────────────────┘

PENTIUM — SUPERSCALAR:
┌────────────────────────────────────────────────────────┐
│  TWO Parallel Integer Pipelines: U-pipe and V-pipe     │
│                                                        │
│  U-pipe: F→D→AG→E→WB  ← handles ANY instruction      │
│  V-pipe: F→D→AG→E→WB  ← handles simple/RISC instrs   │
│                                                        │
│  Both pipelines operate simultaneously in parallel!   │
│  Can complete 2 instructions per clock cycle           │
│                                                        │
│  Also has: Separate FPU pipeline (for floating-point) │
│            Separate cache for instructions and data    │
└────────────────────────────────────────────────────────┘
```

**Technical reasons Pentium is superscalar:**
1. Contains **two 5-stage pipelines** (U and V) that execute concurrently
2. Has **out-of-order execution** logic to find parallelism in instruction stream
3. Has a **branch prediction unit** to reduce pipeline stalls
4. Has **separate L1 instruction cache (8KB)** and **data cache (8KB)**
5. Has dedicated floating-point pipeline operating independently
6. Hardware detects data dependencies — if V-pipe instruction depends on U-pipe result, they execute sequentially; otherwise, they pair and execute together

**Why 80386 is NOT superscalar:**
1. Has only a **single integer pipeline** — one instruction flows through at a time
2. No multiple execution units — maximum one instruction per cycle
3. Is a **scalar processor** by definition (instructions per cycle ≤ 1)
4. Does have a simple stage-based pipeline, but not parallelism

### Pipelining Concepts
```
Without Pipelining (4 instructions, each with 4 stages):
Cycle:    1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16
Instr 1:  F   D   E   W
Instr 2:                  F   D   E   W
Instr 3:                              F   D   E   W
Instr 4:                                          F   D   E   W
Total: 4 × 4 = 16 cycles

WITH Pipelining:
Cycle:    1   2   3   4   5   6   7
Instr 1:  F   D   E   W
Instr 2:      F   D   E   W
Instr 3:          F   D   E   W
Instr 4:              F   D   E   W
Total: 4 + 3 = 7 cycles  (significantly faster!)

Speedup: n×k / (k + n - 1)  where n=instructions, k=stages
For large n: Speedup → k  (ideal case: k times faster)

Pipeline hazards (problems):
1. Data hazard: Instruction needs result of previous not-yet-done instr
   Solution: Data forwarding / stalls / out-of-order execution
2. Control hazard: Branch instruction — don't know which instruction to fetch
   Solution: Branch prediction, delayed branching
3. Structural hazard: Two instructions need same hardware resource
   Solution: Multiple units, pipeline interlocking
```

### 🔥 Advantages of AVR over 80386 (BUET MSc 17)
```
1. RISC Architecture: Simple, regular instruction set → easier pipelining
2. Single-cycle execution: Most AVR instructions execute in 1 clock cycle;
   80386 takes 2-many cycles depending on instruction complexity
3. Harvard Architecture: Separate program and data memory → both can be
   accessed simultaneously (no bus contention)
4. 32 general-purpose registers (R0–R31): No accumulator bottleneck;
   80386 has only 8 general-purpose registers
5. On-chip Flash memory: Electrically reprogrammable without UV eraser
6. Rich on-chip peripherals: ADC, PWM, SPI, I2C, UART built-in
7. Multiple power-saving modes: Idle, Power-down, Standby, etc.
8. Much lower power consumption: Ideal for battery-powered devices
9. In-System Programming (ISP): Can be programmed while in circuit
10. Smaller, cheaper: Much lower unit cost and PCB footprint
```

---

## 14. Assembly Language — Key Instructions

### ⭐ Important 8086 Instructions by Category

#### Data Transfer
```assembly
MOV dst, src    ; dst = src  (most basic: copy data)
XCHG op1, op2  ; exchange values between two operands
PUSH op         ; SP = SP-2, [SS:SP] = op  (push to stack)
POP  op         ; op = [SS:SP], SP = SP+2  (pop from stack)
LEA  reg, mem   ; load effective address (address itself, not data)
XLAT            ; AL = [DS:BX+AL]  (table lookup)
```

#### Arithmetic
```assembly
ADD  dst, src   ; dst = dst + src
SUB  dst, src   ; dst = dst - src
INC  op         ; op = op + 1  (NOTE: does NOT affect CF!)
DEC  op         ; op = op - 1
MUL  src        ; unsigned: AX = AL × src (8-bit)
                ;           DX:AX = AX × src (16-bit)
IMUL src        ; signed multiplication
DIV  src        ; AX ÷ src → AL=quotient, AH=remainder (8-bit)
ADC  dst, src   ; dst = dst + src + CF  (add with carry)
SBB  dst, src   ; dst = dst - src - CF  (subtract with borrow)
NEG  op         ; op = 0 - op  (two's complement negate)
CMP  op1, op2   ; op1 - op2, sets flags but doesn't store result
```

#### Logical
```assembly
AND  dst, src   ; dst = dst AND src
OR   dst, src   ; dst = dst OR src
XOR  dst, src   ; dst = dst XOR src
                ; Trick: XOR AX, AX → AX = 0 (fastest way to clear register)
NOT  op         ; op = NOT op  (bitwise complement)
TEST op1, op2   ; AND without storing result, only sets flags
```

#### Shift and Rotate
```assembly
SHL/SAL op, cnt  ; shift left logical/arithmetic (fill with 0)
SHR     op, cnt  ; shift right logical (fill MSB with 0)
SAR     op, cnt  ; shift right arithmetic (fill MSB with SIGN bit)
ROL     op, cnt  ; rotate left (bit from MSB goes to LSB and CF)
ROR     op, cnt  ; rotate right (bit from LSB goes to MSB and CF)
RCL     op, cnt  ; rotate left through carry flag
RCR     op, cnt  ; rotate right through carry flag
```

#### Control Transfer (Jumps)
```assembly
JMP  label      ; unconditional jump
JZ   label      ; jump if ZF=1  (result was zero / equal)
JNZ  label      ; jump if ZF=0  (result non-zero / not equal)
JE   label      ; same as JZ (after CMP: jump if equal)
JNE  label      ; same as JNZ
JC   label      ; jump if CF=1 (carry / borrow)
JNC  label      ; jump if CF=0
JS   label      ; jump if SF=1 (result negative)
JO   label      ; jump if OF=1 (signed overflow)
JA   label      ; jump if above (unsigned: CF=0 and ZF=0)
JB   label      ; jump if below (unsigned: CF=1)
JG   label      ; jump if greater (signed)
JL   label      ; jump if less (signed: SF≠OF)
CALL label      ; push IP (return address), then JMP to label
RET             ; pop IP, return to caller
IRET            ; pop IP, CS, FLAGS — return from interrupt
LOOP label      ; DEC CX; if CX≠0, JMP label  (efficient loop)
```

### 🔥 Worked Assembly Problem (BUET MSc CSE 21)
```assembly
; Problem: What is AL after these instructions?
MOV BL, 8CH      ; BL = 8CH = 1000 1100 in binary
MOV AL, 7EH      ; AL = 7EH = 0111 1110 in binary
ADD AL, BL       ; AL = AL + BL

Solution (binary addition):
     0111 1110   (7EH = 126 decimal)
  +  1000 1100   (8CH = 140 decimal)
  ──────────────
  1  0000 1010   → Sum = 0000 1010 = 0AH, with carry out (CF=1)

Result: AL = 0AH  (10 in decimal)

Flag analysis:
  CF = 1  (carry out of MSB occurred)
  ZF = 0  (result 0AH ≠ 0)
  SF = 0  (MSB of 0AH is 0, so result is positive)
  OF = ?  (signed perspective: +126 + (-116) = +10, no signed overflow → OF=0)
```

### Addressing Modes Summary
```
1. Immediate:              MOV AX, 1234H   (operand IS the data)
2. Register:               MOV AX, BX      (operand is in a register)
3. Direct:                 MOV AX, [2000H] (address specified directly)
4. Register Indirect:      MOV AX, [BX]    (address is in BX)
5. Based:                  MOV AX, [BX+10H](effective addr = BX + disp)
6. Indexed:                MOV AX, [SI+10H](effective addr = SI + disp)
7. Based+Indexed:          MOV AX, [BX+SI] (effective addr = BX+SI)
8. Based+Indexed+Disp:     MOV AX, [BX+SI+10H]
```

---

# PART C — MICROCONTROLLERS

---

## 15. Microcontroller Overview (8051 & AVR)

### ⭐ 8051 Microcontroller
```
8051 ARCHITECTURE (Harvard — separate code/data memory):

┌─────────────────────────────────────────────────────────┐
│                    8051/8052 MCU                        │
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌───────────────────────┐ │
│  │   CPU    │  │ 128B RAM │  │   4KB ROM (Program)   │ │
│  │  8-bit   │  │  (Data)  │  │   (Code Memory)       │ │
│  │  ALU     │  └──────────┘  └───────────────────────┘ │
│  └──────────┘                                           │
│  ┌──────────────────────────────────────────────────┐   │
│  │  4 × 8-bit I/O Ports: P0, P1, P2, P3            │   │
│  └──────────────────────────────────────────────────┘   │
│  ┌──────────────┐  ┌──────────┐  ┌──────────────────┐  │
│  │  2 × 16-bit  │  │  UART    │  │ Interrupt Control│  │
│  │  Timers      │  │  Serial  │  │ (5 sources)      │  │
│  └──────────────┘  └──────────┘  └──────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

**8051 Key Specifications:**
- 8-bit CPU (ALU processes 8 bits at a time)
- Internal ROM: **4 KB** (program storage)
- Internal RAM: **128 bytes** (data storage + SFRs)
- I/O: **4 × 8-bit** bidirectional ports (P0, P1, P2, P3)
- Timers: **2 × 16-bit** timer/counters
- Serial: Full-duplex UART
- Interrupts: **5 sources** (INT0, INT1, Timer0, Timer1, Serial)
- Clock: 12 oscillator cycles per machine cycle (at 12 MHz → 1 MIPS)
- Architecture: **Harvard** (separate code and data memory address spaces)

### ⭐ AVR Microcontroller (ATmega series)

| Feature | 8051 | AVR (ATmega328) |
|---------|------|-----------------|
| Architecture | Harvard, CISC-like | Modified Harvard, RISC |
| Instruction cycles | **12 clock cycles/instr** | **1 clock cycle/instr** |
| General registers | 1 accumulator (A) + R0-R7 | **32 × 8-bit** (R0–R31) |
| Program memory | Mask ROM / EPROM | **Flash** (reprogrammable) |
| Data memory | 128B SRAM | 2KB SRAM + 1KB EEPROM |
| On-chip ADC | No | Yes (10-bit, 6/8 channels) |
| PWM | No | Yes (multiple channels) |
| Power modes | Basic | Multiple sleep modes |
| ISP | Limited | Yes (In-System Programming) |
| Performance | Moderate | **Much higher** |

**Advantages of AVR over 8051:**
1. **Speed:** Single-cycle execution → same clock = 12× more work than 8051
2. **32 registers:** No accumulator bottleneck; all registers are general-purpose
3. **Flash memory:** Reprogrammable electrically; 8051 often needed UV eraser
4. **Richer peripherals:** Built-in ADC, PWM, SPI, TWI (I2C), multiple UARTs
5. **Power saving:** Multiple sleep modes for battery-efficient designs
6. **ISP (In-System Programming):** Program without removing chip from circuit

---

## 16. Daisy-Chain Priority Interrupts

### 🔥 Daisy-Chaining Interrupts (BUET MSc ICT 15 — Full Explanation + Diagram)

**Problem:** Multiple I/O devices share one interrupt request line to the CPU. When an interrupt occurs, CPU needs to know WHICH device interrupted and must prioritize them.

**Solution: Daisy-Chain Configuration**

```
DAISY-CHAIN INTERRUPT DIAGRAM:
═══════════════════════════════════════════════════════════════

    ┌─────────────────────────────────────────────────────┐
    │                    CPU                              │
    │                                                     │
    │ INTR ←──────────────────────────────────────────── │── Shared INTR line
    │                                                     │   (wired-OR of all devices)
    │ INTA ──→ [Device 1] ──INTA→ [Device 2] ──INTA→ [Device 3]
    │           (Pri=1st)          (Pri=2nd)           (Pri=3rd)
    │           Highest                                 Lowest
    │           Priority                                Priority
    └─────────────────────────────────────────────────────┘

Device connections:
Each device has:
  - INT output: connected to shared INTR line (wired-OR)
  - INTA input: receives acknowledge from previous device or CPU
  - INTA output: passes acknowledge to next device (if it has no interrupt)
```

**Operation Step by Step:**
```
1. Device 2 and Device 3 both need service — they pull INTR HIGH
2. CPU detects INTR HIGH, finishes current instruction
3. CPU sends INTA pulse on INTA line
4. INTA arrives at Device 1:
   - Device 1 has NO pending interrupt
   - Device 1 passes INTA to Device 2
5. INTA arrives at Device 2:
   - Device 2 HAS a pending interrupt
   - Device 2 BLOCKS the INTA from reaching Device 3
   - Device 2 puts its interrupt vector number on Data Bus
   - CPU uses vector to call Device 2's ISR
6. After Device 2 is serviced, CPU returns to main program
7. Process repeats for Device 3
```

**Priority:** Device 1 has highest priority (closest to CPU in chain). Device 3 has lowest priority.

**Advantages:**
- Simple hardware — only 2 extra wires per device (INTA in/out)
- Easily expandable — add more devices at the end
- No additional priority controller chip needed

**Disadvantages:**
- Low-priority devices may starve if high-priority device is always busy
- Adding priority changes requires physical rewiring
- Higher propagation delay in long chains

---

## 17. Quick MCQ Fact Sheet

### 🔥 High-Probability MCQ Facts — Digital Logic

| Question Type | Answer |
|--------------|--------|
| NAND is a ___ gate | Universal gate |
| NOR is a ___ gate | Universal gate |
| XOR outputs 1 when | Inputs are DIFFERENT |
| XNOR outputs 1 when | Inputs are SAME (equal) |
| K-Map group sizes must be | Powers of 2: 1, 2, 4, 8, 16 |
| K-Map wrapping allowed? | Yes, edges wrap around |
| SOP minimization: group the | 1s |
| POS minimization: group the | 0s |
| SR FF forbidden state | S=1, R=1 (output indeterminate) |
| JK FF when J=K=1 | Toggle (output flips) |
| D FF: Q(next) = | D (follows input directly) |
| T FF: Q(next) when T=1 | Q' (toggles) |
| Mealy output depends on | State + Input |
| Moore output depends on | State only |
| Ripple counter problem | Propagation delay (glitches) |
| Synchronous counter advantage | All FFs update simultaneously |

### 🔥 High-Probability MCQ Facts — 8086 Microprocessor

| Question Type | Answer |
|--------------|--------|
| 8086 data bus width | 16 bits |
| 8086 address bus width | 20 bits |
| 8086 max addressable memory | 2²⁰ = 1 MB = 1,048,576 bytes |
| Physical address formula | Segment × 10H + Offset |
| Instruction prefetch queue | 6 bytes |
| IVT location | 00000H – 003FFH (first 1 KB) |
| IVT entry size | 4 bytes (IP word + CS word) |
| Number of IVT entries | 256 |
| Hardware interrupts in 8086 | INTR (maskable) and NMI (non-maskable) |
| INTR controlled by | IF flag (STI enables, CLI disables) |
| NMI is | Non-maskable (always serviced) |
| Memory read cycle duration | 4 clock cycles minimum |
| Segment registers | CS, DS, SS, ES |
| CS used for | Code (instructions) |
| DS used for | Data (variables) |
| SS used for | Stack |
| ES used for | String operations (extra/destination) |
| Stack grows direction | Downward (SP decrements on PUSH) |
| IRET pops | IP, CS, FLAGS (in that order) |
| ZF=1 means | Result was zero |
| CF=1 means | Carry / unsigned overflow |
| OF=1 means | Signed overflow |
| XOR AX, AX result | AX=0, ZF=1, CF=0 (quick way to clear register) |

### 🔥 High-Probability MCQ Facts — Microcontrollers

| Question Type | Answer |
|--------------|--------|
| 8051 internal RAM | 128 bytes |
| 8051 internal ROM | 4 KB |
| 8051 I/O ports | 4 × 8-bit (P0, P1, P2, P3) |
| 8051 timers | 2 × 16-bit |
| 8051 interrupt sources | 5 (2 external, 2 timer, 1 serial) |
| 8051 architecture | Harvard |
| 8051 clock cycles/instruction | 12 |
| AVR clock cycles/instruction | 1 (single-cycle for most instructions) |
| AVR general-purpose registers | 32 (R0–R31) |
| AVR program memory type | Flash (reprogrammable) |
| AVR architecture | Modified Harvard + RISC |
| Microcontroller key advantage | Self-contained (CPU+RAM+ROM+I/O on chip) |
| Pentium pipelines | 2 (U-pipe + V-pipe) = SUPERSCALAR |
| 80386 pipelines | 1 = SCALAR (NOT superscalar) |
| Superscalar means | Multiple instructions per clock cycle |

---

## 🔥 PAST PAPER DIRECT ANSWERS

### Q: Write down advantages of microcontroller over microprocessor. [BUET MSc 15, 16]
1. **Self-contained:** MCU integrates CPU, RAM, ROM, I/O, Timers on one chip
2. **Lower cost:** Single chip vs. multi-chip board with many external ICs
3. **Lower power:** Optimized for embedded, battery-powered applications
4. **Compact size:** Single chip greatly reduces board size
5. **Higher reliability:** Fewer PCB connections = fewer failure points
6. **Simpler design:** Less hardware design complexity
7. **Built-in real-time support:** Hardware timers, interrupt system, watchdog timer
8. **Harvard architecture:** Simultaneous instruction fetch and data access

---

### Q: Purposes of CS, DS, SS, ES registers? [BUET MSc ICT 15]
- **CS:** Segment of code (instructions). Next instruction at CS×10H + IP
- **DS:** Segment of data (variables, arrays). Data access uses DS by default
- **SS:** Segment of stack. PUSH/POP use SS:SP
- **ES:** Extra segment for string operations. MOVS destination = ES:DI

---

### Q: How many clock cycles to read from memory? [BUET MSc ICT 15]
**4 clock cycles minimum** (T1, T2, T3, T4). With slow memory, wait states (Tw) are inserted between T3 and T4 → total = **4 + Tw cycles**.

---

### Q: What are interrupts? Types in 80386? [BUET MSc ICT 16]
An interrupt is a signal that causes the CPU to pause current execution and call a special service routine. After servicing, execution resumes where it was interrupted.

**Types in 80386:**
1. **Hardware Interrupts:** INTR (maskable), NMI (non-maskable)
2. **Software Interrupts:** INT n instruction, INTO
3. **Exceptions:**
   - *Faults:* Page fault (restartable), General Protection Fault
   - *Traps:* Debug/breakpoint (after instruction)
   - *Aborts:* Double fault (unrecoverable)
4. **Internal/Processor-generated:** Divide-by-zero (Type 0), Debug (Type 1), Breakpoint (Type 3), Invalid Opcode (Type 6), Page Fault (Type 14)

---

### Q: Hardware interrupts in 8086? [BUET MSc CSE 21]
**INTR** (maskable, via IF flag) and **NMI** (non-maskable).

---

### Q: AL after MOV BL,8CH / MOV AL,7EH / ADD AL,BL? [BUET MSc CSE 21]
**AL = 0AH** (7EH + 8CH = 10AH, truncated to 0AH with CF=1)

---

### Q: Why Pentium is superscalar but 80386 is not? [BUET MSc 16, 17]
Pentium has **two parallel integer pipelines** (U-pipe and V-pipe) and can execute **two instructions per clock cycle**. 80386 has only **one pipeline** and can execute at most one instruction per cycle (scalar). See Section 13 for full details.

---

### Q: Advantages of AVR over 80386? [BUET MSc 17]
See Section 13 — 10 key advantages including single-cycle execution, 32 registers, Harvard architecture, Flash memory, and rich on-chip peripherals.

---

### Q: What is not a feature of a microprocessor? [BUET MSc CSE 21]
**On-chip RAM, ROM, I/O ports, and timers** are NOT features of a pure microprocessor. These are features of a **microcontroller**. A microprocessor is only the CPU core.

---

## 📌 Last-Minute Revision Cheat Sheet

```
══════════════════════════════════════════════
DIGITAL LOGIC QUICK REFERENCE
══════════════════════════════════════════════
De Morgan:   (AB)' = A'+B'
             (A+B)' = A'·B'

Universal:   NAND and NOR (can implement any logic)

XOR: output 1 when inputs DIFFER
XNOR: output 1 when inputs SAME

K-Map rules: Group sizes = 1,2,4,8 (powers of 2)
             Groups must be rectangular
             Wrap-around allowed
             Group SOP→1s, POS→0s

Half Adder:  S = A⊕B,   C = AB
Full Adder:  S = A⊕B⊕Cin,  Cout = AB+BCin+ACin

FF Summary:
  SR: S=R=1 is FORBIDDEN
  JK: J=K=1 → TOGGLE (no forbidden state!)
  D:  Q(next) = D  (simplest!)
  T:  Q(next) = T⊕Q  (toggles when T=1)

Excitation (for design):
  D FF: D = Q(next)  [directly!]
  T FF: T = Q XOR Q(next)
  JK: J=Q', K=Q when transitioning (use excitation table)

Mealy → output on arrow (faster, fewer states)
Moore → output in circle (simpler, one clock later)

══════════════════════════════════════════════
MICROPROCESSOR QUICK REFERENCE
══════════════════════════════════════════════
8086:
  Data bus: 16-bit
  Address bus: 20-bit → 1 MB
  PA = Segment × 10H + Offset
  Segment regs: CS, DS, SS, ES
  General regs: AX(AH/AL), BX, CX, DX
  Index regs: SP, BP, SI, DI, IP
  Key flags: CF, ZF, SF, OF, TF, IF, DF
  IVT: 00000H–003FFH (256 vectors × 4 bytes)
  Memory read: 4 cycles + wait states
  Prefetch queue: 6 bytes

Interrupts:
  INTR: maskable hardware (IF flag controls)
  NMI: non-maskable (always serviced, Type 2)
  INT n: software interrupt
  Priority: Internal > NMI > INTR > Software

Superscalar:
  Pentium: 2 parallel pipes (U+V) → 2 instr/cycle
  80386: 1 pipe → 1 instr/cycle (scalar)

AVR advantages:
  1 cycle/instr, 32 regs, RISC, Flash, ADC/PWM

MCU:
  8051: 128B RAM, 4KB ROM, 4 ports, 2 timers, Harvard
  AVR: 1-cycle, 32 regs, Flash, rich peripherals, RISC

Microcontroller wins over microprocessor:
  Self-contained, cheaper, lower power, smaller, reliable

Daisy-chain interrupts:
  Devices in series, INTA propagates through chain,
  First pending device blocks INTA and sends vector.
  Positional priority (first in chain = highest priority)

Assembly key:
  MOV: copy data
  ADD/SUB: arithmetic
  AND/OR/XOR: logic (XOR AX,AX clears AX)
  JZ/JNZ/JC: conditional jumps based on flags
  PUSH/POP: stack operations
  CALL/RET: subroutine
  IRET: return from interrupt (pops IP, CS, Flags)
══════════════════════════════════════════════
```

---
*Created: October 2026 | For BUET MSc Admission Exam*
*Covers: BUET MSc 2015, 2016, 2017, 2018, 2019, 2021, 2024 past papers*
*Reference: Digital Logic Design - Morris Mano, 8086 Microprocessor - Liu & Gibson*

# 📡 Computer Networking — Master Notes for Competitive Exams
> **Single source of truth** | Covers MCQ + Written | In-depth explanations + Diagrams

---

## 📋 Table of Contents

1. [Fundamentals & Classification](#1-fundamentals--classification)
2. [OSI Model — 7 Layers](#2-osi-model--7-layers)
3. [TCP/IP Model](#3-tcpip-model)
4. [Physical Layer](#4-physical-layer)
5. [Data Link Layer](#5-data-link-layer)
6. [Network Layer & IP Addressing](#6-network-layer--ip-addressing)
7. [Subnetting & CIDR](#7-subnetting--cidr)
8. [Routing & Routing Protocols](#8-routing--routing-protocols)
9. [Transport Layer — TCP & UDP](#9-transport-layer--tcp--udp)
10. [Application Layer Protocols](#10-application-layer-protocols)
11. [DNS — Domain Name System](#11-dns--domain-name-system)
12. [HTTP & HTTPS](#12-http--https)
13. [Network Devices](#13-network-devices)
14. [LAN Technologies & Ethernet](#14-lan-technologies--ethernet)
15. [Wireless Networking (WiFi)](#15-wireless-networking-wifi)
16. [Network Security](#16-network-security)
17. [IPv6](#17-ipv6)
18. [Flow Control & Congestion Control](#18-flow-control--congestion-control)
19. [Error Detection & Correction](#19-error-detection--correction)
20. [MAC Addresses & ARP](#20-mac-addresses--arp)
21. [NAT & DHCP](#21-nat--dhcp)
22. [VPN & Tunneling](#22-vpn--tunneling)
23. [Cloud & SDN Basics](#23-cloud--sdn-basics)
24. [Quick MCQ Cheat Sheet](#24-quick-mcq-cheat-sheet)

---

## 1. Fundamentals & Classification

### What is a Network?
A **network** is a collection of interconnected devices (nodes) that communicate and share resources.

### Network Classification by Size

| Type | Full Form | Coverage | Example |
|------|-----------|----------|---------|
| **PAN** | Personal Area Network | ~10 m | Bluetooth, USB |
| **LAN** | Local Area Network | Building/campus | Office Ethernet, WiFi |
| **MAN** | Metropolitan Area Network | City-wide | Cable TV network |
| **WAN** | Wide Area Network | Country/globe | Internet, leased lines |

### Network Topologies

```
BUS Topology:          STAR Topology:          RING Topology:
                              Hub/Switch
 A---B---C---D           A----[SW]----B          A->B->C->D->A
                              |    |
                              C    D

MESH Topology:          TREE Topology:
A---B                        Root
|\ /|                       /    \
| X |                    Node1   Node2
|/ \|                    /  \       \
C---D                  L1   L2      L3
```

| Topology | Advantage | Disadvantage |
|----------|-----------|--------------|
| Bus | Simple, cheap | Single point of failure (cable) |
| Star | Easy fault isolation | Hub/switch failure = all fail |
| Ring | Equal access, orderly | One node failure = ring breaks |
| Mesh | Highly reliable | Expensive, complex wiring |
| Tree | Scalable, hierarchical | Root failure = entire subtree fails |

### Transmission Modes

| Mode | Description | Example |
|------|-------------|---------|
| **Simplex** | One direction only | TV broadcast |
| **Half-duplex** | Both directions, not simultaneously | Walkie-talkie |
| **Full-duplex** | Both directions simultaneously | Phone call |

### Switching Techniques

| Method | Description | Delay | Example |
|--------|-------------|-------|---------|
| **Circuit Switching** | Dedicated path reserved before data | Setup delay, no queuing | PSTN (telephone) |
| **Packet Switching** | Data split into packets, routed independently | Variable delay | Internet |
| **Message Switching** | Entire message stored at each node | High delay | Old telegraph |

> **MCQ Tip:** Internet uses **packet switching**. Telephone uses **circuit switching**.

---

## 2. OSI Model — 7 Layers

### The Stack (Memorize: **"All People Seem To Need Data Processing"** or **"Please Do Not Throw Sausage Pizza Away"**)

```
+----------------------------------------------------------+
|  Layer 7 | APPLICATION   | HTTP, FTP, SMTP, DNS, Telnet  |
+----------------------------------------------------------+
|  Layer 6 | PRESENTATION  | Encryption, Compression, JPEG |
+----------------------------------------------------------+
|  Layer 5 | SESSION       | NetBIOS, RPC, SQL sessions    |
+----------------------------------------------------------+
|  Layer 4 | TRANSPORT     | TCP, UDP, Port numbers        |
+----------------------------------------------------------+
|  Layer 3 | NETWORK       | IP, ICMP, OSPF, BGP, Routers  |
+----------------------------------------------------------+
|  Layer 2 | DATA LINK     | Ethernet, MAC, ARP, Switch    |
+----------------------------------------------------------+
|  Layer 1 | PHYSICAL      | Cables, bits, Hub, NIC        |
+----------------------------------------------------------+
```

### Layer-by-Layer Detail

#### Layer 1 — Physical
- **Unit:** Bit
- **Function:** Transmits raw bits over physical medium (cable, radio waves)
- **Devices:** Hub, Repeater, NIC, Modem
- **Concerns:** Voltage levels, bit rate, cable types, pin layout
- **Standards:** RS-232, DSL, 10BASE-T, 100BASE-TX

#### Layer 2 — Data Link
- **Unit:** Frame
- **Function:** Node-to-node delivery, error detection, MAC addressing, flow control
- **Sub-layers:**
  - **LLC** (Logical Link Control) — error & flow control
  - **MAC** (Media Access Control) — physical addressing, channel access
- **Devices:** Switch, Bridge
- **Protocols:** Ethernet (IEEE 802.3), WiFi (IEEE 802.11), PPP
- **Error detection:** CRC (Cyclic Redundancy Check)

#### Layer 3 — Network
- **Unit:** Packet
- **Function:** Logical addressing (IP), routing between different networks
- **Devices:** Router
- **Protocols:** IP, ICMP, IGMP, ARP (sometimes placed here), OSPF, BGP
- **Key concept:** Routing table lookups, TTL (Time to Live)

#### Layer 4 — Transport
- **Unit:** Segment (TCP) / Datagram (UDP)
- **Function:** End-to-end delivery, multiplexing (port numbers), reliability (TCP)
- **Protocols:** TCP, UDP, SCTP
- **Key concepts:** Port numbers, flow control, congestion control, 3-way handshake

#### Layer 5 — Session
- **Unit:** Data
- **Function:** Establish, manage, terminate sessions between applications
- **Protocols:** NetBIOS, RPC, PPTP
- **Key concept:** Session establishment, synchronization (checkpoints)

#### Layer 6 — Presentation
- **Unit:** Data
- **Function:** Translation (ASCII<->EBCDIC), encryption, compression
- **Formats:** JPEG, MP3, MPEG, GIF, SSL/TLS (encryption)
- **Key concept:** Data format conversion between systems

#### Layer 7 — Application
- **Unit:** Data / Message
- **Function:** Interface between user and network
- **Protocols:** HTTP, FTP, SMTP, POP3, IMAP, DNS, Telnet, SSH, SNMP

### OSI Data Encapsulation

```
Sender Side                              Receiver Side
App Data ---> [Message]                  [Message] ---> App Data
+ TCP Hdr --> [Segment]                  [Segment] ---> Strip TCP Hdr
+ IP Hdr  --> [Packet]                   [Packet]  ---> Strip IP Hdr
+ Frame   --> [Frame]                    [Frame]   ---> Strip Frame
             --> bits on wire ----------->
```

> **Written Exam:** Be able to name the PDU (Protocol Data Unit) at each layer:
> - L7-L5: **Message/Data**
> - L4: **Segment** (TCP) / **Datagram** (UDP)
> - L3: **Packet**
> - L2: **Frame**
> - L1: **Bit**

---

## 3. TCP/IP Model

The **TCP/IP model** (also called DoD model or Internet model) has **4 layers**:

```
+-------------------------------------------------------+
|  Layer 4 | APPLICATION   | HTTP, FTP, DNS, SMTP        |
+-------------------------------------------------------+
|  Layer 3 | TRANSPORT     | TCP, UDP                    |
+-------------------------------------------------------+
|  Layer 2 | INTERNET      | IP, ICMP, ARP               |
+-------------------------------------------------------+
|  Layer 1 | NETWORK ACCESS| Ethernet, WiFi, Frame Relay |
+-------------------------------------------------------+
```

### OSI vs TCP/IP Comparison

```
OSI Model              TCP/IP Model
+-------------+       +-----------------+
| Application |---+   |                 |
| Presentation|   +-->|   Application   |
| Session     |---+   |                 |
+-------------+       +-----------------+
| Transport   |------>|   Transport     |
+-------------+       +-----------------+
| Network     |------>|   Internet      |
+-------------+       +-----------------+
| Data Link   |---+   |                 |
| Physical    |---+-->|  Network Access |
+-------------+       +-----------------+
```

| Feature | OSI | TCP/IP |
|---------|-----|--------|
| Layers | 7 | 4 |
| Developed by | ISO | DARPA/DoD |
| Purpose | Theoretical model | Practical implementation |
| Protocol-specific | No (generic) | Yes (TCP, IP specific) |
| Session/Presentation | Separate layers | Part of Application |

---

## 4. Physical Layer

### Transmission Media

#### Guided (Wired) Media

| Medium | Speed | Max Distance | Interference | Cost |
|--------|-------|--------------|-------------|------|
| **Twisted Pair (UTP)** | 10 Mbps-10 Gbps | 100 m | Susceptible | Low |
| **Twisted Pair (STP)** | 10 Mbps-10 Gbps | 100 m | Shielded | Medium |
| **Coaxial Cable** | Up to 1 Gbps | 500 m | Good shielding | Medium |
| **Fiber Optic** | Up to 100 Tbps | Thousands of km | Immune | High |

**UTP Categories:**
```
Cat 3  -> 10 Mbps   (voice/old LAN)
Cat 5  -> 100 Mbps  (Fast Ethernet)
Cat 5e -> 1 Gbps    (Gigabit Ethernet)
Cat 6  -> 10 Gbps   (short distance)
Cat 7  -> 10 Gbps   (better shielding)
```

**Fiber Optic Types:**
- **Single-mode (SMF):** Thin core (~8um), long distance, laser source, expensive
- **Multi-mode (MMF):** Thick core (~50-62.5um), short distance, LED source, cheaper

#### Unguided (Wireless) Media

| Type | Frequency | Range | Example |
|------|-----------|-------|---------|
| **Radio Waves** | 3 kHz - 1 GHz | Long range | AM/FM, WiFi |
| **Microwaves** | 1-300 GHz | Line-of-sight | Satellite, 5G |
| **Infrared** | 300 GHz-400 THz | Short range | Remote controls |

### Signal Types & Modulation

**Analog vs Digital Signals:**
```
Analog:   /\/\/\   (continuous wave - values vary smoothly)
Digital: _|--|__|   (discrete 0s and 1s - square waves)
```

**Modulation Techniques:**

| Technique | What Varies | Type |
|-----------|-------------|------|
| **AM** (Amplitude Modulation) | Amplitude | Analog |
| **FM** (Frequency Modulation) | Frequency | Analog |
| **PM** (Phase Modulation) | Phase | Analog |
| **ASK** (Amplitude Shift Keying) | Amplitude | Digital->Analog |
| **FSK** (Frequency Shift Keying) | Frequency | Digital->Analog |
| **PSK** (Phase Shift Keying) | Phase | Digital->Analog |
| **QAM** (Quadrature AM) | Amplitude+Phase | High-speed |

### Key Formulas

**Nyquist Theorem** (Noiseless channel):
```
Max Bit Rate = 2 x Bandwidth x log2(L)
where L = number of signal levels
```

**Shannon's Theorem** (Noisy channel):
```
Channel Capacity = B x log2(1 + S/N)
where B = bandwidth, S/N = Signal-to-Noise ratio
```

> **MCQ:** Nyquist is for **noiseless**, Shannon is for **noisy** channels.

**Bandwidth-Delay Product:**
```
BDP = Bandwidth x Round-Trip Time
(measures "pipe fullness" -- amount of data in transit)
```

---

## 5. Data Link Layer

### Key Functions
1. **Framing** — Encapsulate packets into frames
2. **Physical addressing** — MAC addresses
3. **Error detection** — CRC, parity
4. **Flow control** — Prevent sender from overwhelming receiver
5. **Access control** — Determine who uses the channel (CSMA/CD, CSMA/CA)

### Framing Methods
- **Character count** — First field says how many characters in frame
- **Flag bytes with byte stuffing** — Use special flag (01111110) + escape for same data
- **Bit stuffing** — After 5 consecutive 1s, insert a 0

### Error Detection

#### Parity Bit
- **Even parity:** Add bit so total 1s is even
- **Odd parity:** Add bit so total 1s is odd
- **Limitation:** Can't detect even number of errors

#### CRC (Cyclic Redundancy Check) — Most Important
```
Process:
1. Append (r) zeros to message M  [r = degree of generator polynomial G]
2. Divide M x 2^r by G using XOR (modulo-2 division)
3. Remainder R is the CRC (checksum)
4. Transmitted frame = M concatenated with R
5. Receiver divides received frame by G; remainder=0 means no error

Example: M = 1011, G = 1001 (degree 3)
Augmented M: 1011 000
Divide by G using XOR -> get remainder = CRC
```

#### Hamming Code (Error Correction)
- Positions of parity bits: **powers of 2** (1, 2, 4, 8, ...)
- Can **detect 2-bit errors** and **correct 1-bit errors**
- For `m` data bits, need `r` parity bits such that: `2^r >= m + r + 1`

```
Example: 4-bit data -> need 3 parity bits (2^3 = 8 >= 4+3+1 = 8) OK
Positions: 1  2  3  4  5  6  7
           P1 P2 D1 P4 D2 D3 D4

P1 covers positions: 1, 3, 5, 7
P2 covers positions: 2, 3, 6, 7
P4 covers positions: 4, 5, 6, 7
```

### MAC — Media Access Control

#### CSMA/CD (Collision Detection) — Used in Ethernet (Wired)
```
Process:
1. Sense channel -> if IDLE, transmit
2. If BUSY, wait (backoff)
3. While transmitting, detect collision
4. If COLLISION detected:
   a. Send jam signal (32 bits)
   b. Stop transmission
   c. Wait random backoff time (Binary Exponential Backoff)
   d. Retry

Minimum frame size = 2 x propagation delay x bandwidth
For Ethernet: 64 bytes minimum frame size
```

**Binary Exponential Backoff:**
```
After k-th collision: wait random time in [0, 2^k - 1] slot times
Max k = 10 for backoff, retry up to 16 times, then abort
```

#### CSMA/CA (Collision Avoidance) — Used in WiFi (Wireless)
```
Reason: Cannot detect collisions in wireless (hidden terminal problem)
1. Sense channel -> if IDLE, wait DIFS time
2. Start random backoff timer
3. If channel stays idle -> transmit
4. Wait for ACK
5. If no ACK -> collision assumed -> retry with larger backoff
```

**Hidden Terminal Problem:**
```
A <---- B ----> C
A and C can't hear each other but both reach B
-> Collision at B without A or C knowing
Solution: RTS/CTS (Request-To-Send / Clear-To-Send)
```

#### Other Access Methods

| Method | Description | Use |
|--------|-------------|-----|
| **TDMA** | Time Division Multiple Access - fixed time slots | GSM, satellites |
| **FDMA** | Frequency Division Multiple Access - different frequencies | Radio |
| **CDMA** | Code Division Multiple Access - unique codes | 3G cellular |
| **ALOHA** | Transmit anytime; if collision, retry after random time | Satellite |
| **Slotted ALOHA** | Transmit only at start of time slot | Better than ALOHA |

**ALOHA Efficiency:**
```
Pure ALOHA:    Max throughput = 1/(2e) ≈ 18.4%
Slotted ALOHA: Max throughput = 1/e   ≈ 36.8%
CSMA/CD:       Much higher efficiency (> 90% at low load)
```

### Sliding Window Protocols

#### Stop-and-Wait (SW)
```
Sender:   [Frame 0] ------> Receiver
                   <--[ACK 0]--
          [Frame 1] ------> Receiver
                   <--[ACK 1]--
Efficiency = T_t / (T_t + 2 x T_p)
where T_t = transmission time, T_p = propagation delay
```

#### Go-Back-N (GBN)
- **Window size:** Sender <= 2^n - 1, Receiver = 1
- On error: **retransmit from errored frame** onwards
- Receiver **discards out-of-order** frames

#### Selective Repeat (SR)
- **Window size:** Sender <= 2^(n-1), Receiver <= 2^(n-1)
- On error: **retransmit only** the errored frame
- Receiver **buffers out-of-order** frames

```
Comparison:        GBN                    SR
Sender window     2^n - 1               2^(n-1)
Receiver window       1                 2^(n-1)
On Error       Retransmit all        Retransmit only bad
Buffer         Not needed at RX      Needed at receiver
Efficiency     Lower on bad links    Higher on bad links
```

**Efficiency Formula:**
```
GBN: eta = W/(1+2a)         if W < 1+2a   [a = T_p/T_t]
     eta = 1                if W >= 1+2a

SR:  eta = W/(1+2a)         if W < 1+2a
     eta = 1                if W >= 1+2a
(SR needs W = 2^(n-1) vs GBN needs W = 2^n - 1 for same result)
```

---

## 6. Network Layer & IP Addressing

### IPv4 Address

- **Size:** 32 bits = 4 octets
- **Notation:** Dotted decimal (e.g., 192.168.1.1)
- **Range:** 0.0.0.0 to 255.255.255.255
- **Total addresses:** 2^32 = ~4.3 billion

### IP Address Classes (Classful Addressing)

```
Class A: 0xxxxxxx.host.host.host
         Network(8 bits) + Host(24 bits)
         Range: 1.0.0.0 - 126.255.255.255
         Default Mask: 255.0.0.0 (/8)
         Networks: 126  |  Hosts per network: 16,777,214

Class B: 10xxxxxx.net.host.host
         Network(16 bits) + Host(16 bits)
         Range: 128.0.0.0 - 191.255.255.255
         Default Mask: 255.255.0.0 (/16)
         Networks: 16,384  |  Hosts per network: 65,534

Class C: 110xxxxx.net.net.host
         Network(24 bits) + Host(8 bits)
         Range: 192.0.0.0 - 223.255.255.255
         Default Mask: 255.255.255.0 (/24)
         Networks: 2,097,152  |  Hosts per network: 254

Class D: 1110xxxx...  -> 224.0.0.0 - 239.255.255.255  (Multicast)
Class E: 1111xxxx...  -> 240.0.0.0 - 255.255.255.255  (Reserved)
```

> **MCQ Tip:** Class A starts with 0, B with 10, C with 110, D with 1110, E with 1111

### Special IP Addresses

| Address | Purpose |
|---------|---------|
| `127.0.0.1` | Loopback (localhost) |
| `0.0.0.0` | This host on this network |
| `255.255.255.255` | Limited broadcast |
| `x.x.x.0` | Network address |
| `x.x.x.255` | Directed broadcast |
| `169.254.x.x` | APIPA (Automatic Private IP) -- DHCP failed |

### Private IP Ranges (RFC 1918)

| Class | Range | CIDR |
|-------|-------|------|
| A | 10.0.0.0 - 10.255.255.255 | 10.0.0.0/8 |
| B | 172.16.0.0 - 172.31.255.255 | 172.16.0.0/12 |
| C | 192.168.0.0 - 192.168.255.255 | 192.168.0.0/16 |

> **Private IPs are NOT routable on the Internet** — NAT is needed.

### IP Packet Header Fields

```
 0         1         2         3  (bytes)
 +----+----+--------+---------+----------+
 |Ver |IHL |  TOS   |    Total Length    |
 +----+----+--------+---------+----------+
 |   Identification   |Flags| Frag Offset|
 +--------------------+------+-----------+
 |  TTL   | Protocol |   Checksum        |
 +--------+----------+-------------------+
 |          Source IP Address            |
 +---------------------------------------+
 |       Destination IP Address          |
 +---------------------------------------+
 |     Options (if IHL > 5)              |
 +---------------------------------------+
```

**Key Header Fields:**

| Field | Bits | Purpose |
|-------|------|---------|
| Version | 4 | IPv4 = 4, IPv6 = 6 |
| IHL | 4 | Header length in 32-bit words (min=5, so min header=20 bytes) |
| TOS/DSCP | 8 | Quality of Service |
| Total Length | 16 | Header + Data (max 65,535 bytes) |
| TTL | 8 | Decremented at each hop; packet dropped when TTL=0 |
| Protocol | 8 | TCP=6, UDP=17, ICMP=1, OSPF=89 |
| Checksum | 16 | Header error detection only |

**IP Fragmentation:**
- When packet > MTU (Maximum Transmission Unit), it's fragmented
- **MTU of Ethernet = 1500 bytes**
- Fragments reassembled at **destination** only
- **DF flag:** Don't Fragment | **MF flag:** More Fragments
- **Fragment Offset:** Position in original datagram (units of 8 bytes)

```
Fragmentation Example:
Original: 4000 bytes total, MTU = 1500 bytes
IP header = 20 bytes, so data per fragment = 1480 bytes

Fragment 1: data bytes 0-1479,    offset=0,    MF=1
Fragment 2: data bytes 1480-2959, offset=185,  MF=1  (1480/8=185)
Fragment 3: data bytes 2960-3979, offset=370,  MF=0  (2960/8=370)
```

### ICMP (Internet Control Message Protocol)

- Works at Network layer (Protocol number = **1**)
- Used for **error reporting** and **diagnostics**
- **Ping** = ICMP Echo Request (type 8) / Echo Reply (type 0)
- **Traceroute** = uses ICMP Time Exceeded (type 11)

| Message Type | Code | Meaning |
|-------------|------|---------|
| Echo Reply | 0,0 | Ping reply |
| Destination Unreachable | 3,x | Can't reach host/port/network |
| Echo Request | 8,0 | Ping request |
| Time Exceeded | 11,0 | TTL expired (used by traceroute) |
| Redirect | 5,x | Better route available |

---

## 7. Subnetting & CIDR

### Subnet Mask

- A 32-bit number where 1s = network part, 0s = host part
- `255.255.255.0` = `/24` in CIDR notation

**CIDR Fast Reference:**

```
/8  -> 255.0.0.0       -> 2^24 - 2 = 16,777,214 hosts
/16 -> 255.255.0.0     -> 2^16 - 2 = 65,534 hosts
/24 -> 255.255.255.0   -> 2^8  - 2 = 254 hosts
/25 -> 255.255.255.128 -> 2^7  - 2 = 126 hosts
/26 -> 255.255.255.192 -> 2^6  - 2 = 62 hosts
/27 -> 255.255.255.224 -> 2^5  - 2 = 30 hosts
/28 -> 255.255.255.240 -> 2^4  - 2 = 14 hosts
/29 -> 255.255.255.248 -> 2^3  - 2 = 6 hosts
/30 -> 255.255.255.252 -> 2^2  - 2 = 2 hosts  (point-to-point)
/32 -> 255.255.255.255 -> 1 host (single host route)
```

> **Formula:** Usable hosts = 2^(32-prefix) - 2

### Subnetting Step-by-Step

**Example:** Subnet `192.168.1.0/24` into 4 equal subnets.

```
Step 1: Need 4 subnets -> 2^2 = 4 -> borrow 2 bits -> new prefix = /26
Step 2: New mask = 255.255.255.192 (block size = 64)
Step 3:
  Subnet 0: 192.168.1.0/26   Network:.0   Bcast:.63   Hosts:.1-.62
  Subnet 1: 192.168.1.64/26  Network:.64  Bcast:.127  Hosts:.65-.126
  Subnet 2: 192.168.1.128/26 Network:.128 Bcast:.191  Hosts:.129-.190
  Subnet 3: 192.168.1.192/26 Network:.192 Bcast:.255  Hosts:.193-.254
```

### VLSM (Variable Length Subnet Masking)
- Different subnets can have **different-sized masks**
- More efficient use of IP space
- **Allocate largest subnet first**

**VLSM Example:** Network: 200.1.1.0/24, needs: 100, 60, 30, and 2 hosts

```
Subnet A (100 hosts): need 128 addresses -> /25
  200.1.1.0/25 -> hosts: .1 to .126

Subnet B (60 hosts): need 64 addresses -> /26
  200.1.1.128/26 -> hosts: .129 to .190

Subnet C (30 hosts): need 32 addresses -> /27
  200.1.1.192/27 -> hosts: .193 to .222

Subnet D (2 hosts): need 4 addresses -> /30
  200.1.1.224/30 -> hosts: .225 to .226
```

### Supernetting / Route Aggregation

Combining multiple subnets into one larger block:
```
192.168.0.0/24 |
192.168.1.0/24 |-> 192.168.0.0/22  (4 networks combined)
192.168.2.0/24 |
192.168.3.0/24 |

Rule: Starting address must be divisible by the block size
```

---

## 8. Routing & Routing Protocols

### Routing Concepts

**Routing Table Entry:**
```
Destination | Subnet Mask      | Next Hop     | Interface | Metric
0.0.0.0     | 0.0.0.0          | 10.0.0.1     | eth0      | 0  (default)
192.168.1.0 | 255.255.255.0    | 0.0.0.0      | eth1      | 0  (connected)
10.0.0.0    | 255.0.0.0        | 192.168.1.254| eth1      | 10 (dynamic)
```

**Longest Prefix Match:** When multiple entries match, the most specific (longest prefix) wins.

### Types of Routing

| Type | Description | Use |
|------|-------------|-----|
| **Static** | Manually configured | Small networks |
| **Dynamic** | Automatically learned via protocols | Enterprise/Internet |
| **Default** | Route of last resort (0.0.0.0/0) | Gateway to Internet |

### Distance Vector Routing

- Each router knows distance to every destination
- Routers share **entire routing table** with **neighbors** periodically
- Algorithm: **Bellman-Ford** (V-1 iterations for V nodes)
- **Problem:** Count to infinity, routing loops
- **Solutions:** Split horizon, route poisoning, holddown timers

```
Count to Infinity Problem:
A --- B --- C
If A-B link fails:
  B asks C "can you reach A?"
  C says "yes, via B with cost 2" (stale info!)
  B updates to cost 3, tells C
  C updates to 4... loops to infinity (16 = infinity in RIP)
```

**Split Horizon:** Don't advertise a route back to the interface you learned it from.
**Route Poisoning:** Advertise failed route with metric = infinity (16 in RIP).

### Link State Routing

- Each router knows the **complete topology**
- Routers **flood** Link State Advertisements (LSAs) to ALL routers
- Each router independently runs **Dijkstra's SPF** algorithm
- **Fast convergence**, no loops
- Protocol: **OSPF, IS-IS**

```
Process:
1. Discover neighbors (Hello packets)
2. Measure link costs
3. Create LSA (Link State Advertisement)
4. Flood LSA to all routers in the area
5. Each router runs Dijkstra's algorithm
6. Build routing table from shortest path tree
```

### Routing Protocols Comparison

| Protocol | Type | Algorithm | Metric | AD | Max Hops |
|----------|------|-----------|--------|-----|---------|
| **RIP v1/v2** | IGP, Distance Vector | Bellman-Ford | Hop count | 120 | 15 |
| **EIGRP** | IGP, Hybrid | DUAL | BW+Delay+Load+Reliability | 90 | 255 |
| **OSPF** | IGP, Link State | Dijkstra | Cost (ref BW/link BW) | 110 | None |
| **IS-IS** | IGP, Link State | Dijkstra | Cost | 115 | None |
| **BGP** | EGP, Path Vector | Best Path | AS Path, LP, MED | 20/200 | None |

> **AD = Administrative Distance** — lower = more trusted
> **IGP** = Interior Gateway Protocol (within an Autonomous System)
> **EGP** = Exterior Gateway Protocol (between ASes)

### OSPF In Depth

```
OSPF Areas:
         Area 0 (Backbone Area -- required)
        +------------------------+
Area 1 -|-- ABR            ABR --|- Area 2
        +------------------------+
                   |
        Area 3 --- ABR

ABR = Area Border Router (connects multiple areas)
ASBR = Autonomous System Boundary Router (connects to external AS)
```

**OSPF Neighbor States (in order):**
`Down -> Init -> 2-Way -> Exstart -> Exchange -> Loading -> Full`

**DR/BDR Election** (on broadcast networks like Ethernet):
- DR (Designated Router) and BDR reduce flooding
- Highest **Router Priority** wins (default=1), tie-break: highest **Router ID**
- Router ID = highest loopback IP, else highest active interface IP

**OSPF Cost:** Cost = Reference Bandwidth / Link Bandwidth (default ref = 100 Mbps)
```
100 Mbps link: cost = 100/100 = 1
10 Mbps link:  cost = 100/10  = 10
1 Mbps link:   cost = 100/1   = 100
```

### BGP (Border Gateway Protocol)

- The **routing protocol of the Internet** (de facto standard)
- Connects **Autonomous Systems (AS)** — each AS has unique ASN (1-65535 public, 64512-65535 private)
- Uses **TCP port 179** (reliable transport)
- **Path vector** protocol (stores entire AS path to detect loops)
- **eBGP:** Between different ASes (usually directly connected)
- **iBGP:** Within the same AS (requires full mesh or route reflector)

**BGP Path Selection (simplified order):**
```
1. Highest Weight (Cisco proprietary, local only)
2. Highest Local Preference (iBGP, whole AS preference)
3. Locally originated routes (network or redistribute)
4. Shortest AS Path
5. Lowest Origin type (i < e < ?)
6. Lowest MED (Multi-Exit Discriminator - hint to external AS)
7. eBGP over iBGP
8. Lowest IGP metric to next-hop
9. Oldest eBGP route
10. Lowest Router ID
```

---

## 9. Transport Layer — TCP & UDP

### Port Numbers

| Range | Type | Description |
|-------|------|-------------|
| 0-1023 | Well-known | Reserved, privileged (HTTP=80, FTP=21) |
| 1024-49151 | Registered | Application use |
| 49152-65535 | Dynamic/Ephemeral | Client-side temporary ports |

**Important Port Numbers (MUST memorize):**

| Port | Protocol | Service |
|------|----------|---------|
| 20 | TCP | FTP Data |
| 21 | TCP | FTP Control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67 | UDP | DHCP Server |
| 68 | UDP | DHCP Client |
| 69 | UDP | TFTP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP Agent |
| 162 | UDP | SNMP Trap |
| 179 | TCP | BGP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 514 | UDP | Syslog |
| 993 | TCP | IMAPS |
| 995 | TCP | POP3S |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |
| 5432 | TCP | PostgreSQL |

### TCP (Transmission Control Protocol)

**Characteristics:**
- Connection-oriented (3-way handshake)
- Reliable, ordered, error-checked delivery
- Flow control (sliding window)
- Congestion control (slow start, AIMD)
- Full-duplex communication

**TCP Header (minimum 20 bytes):**
```
Bytes 0-1:  Source Port
Bytes 2-3:  Destination Port
Bytes 4-7:  Sequence Number
Bytes 8-11: Acknowledgment Number
Byte 12:    Data Offset (4 bits) + Reserved + Control Flags
Bytes 13:   Control Flags [URG|ACK|PSH|RST|SYN|FIN]
Bytes 14-15: Window Size
Bytes 16-17: Checksum
Bytes 18-19: Urgent Pointer
Bytes 20+:  Options (if Data Offset > 5)
```

**TCP Flags:**

| Flag | Meaning |
|------|---------|
| **SYN** | Synchronize -- initiate connection |
| **ACK** | Acknowledgment -- confirms receipt |
| **FIN** | Finish -- graceful termination |
| **RST** | Reset -- abort connection immediately |
| **PSH** | Push -- deliver data to app immediately |
| **URG** | Urgent data present |

### TCP 3-Way Handshake (Connection Establishment)

```
Client                          Server
  |                               |
  |--- SYN (seq=x) -------------->|   Client: "Let's talk, my seq=x"
  |                               |
  |<-- SYN+ACK (seq=y, ack=x+1) --|   Server: "OK, my seq=y, got your x"
  |                               |
  |--- ACK (ack=y+1) ------------>|   Client: "Got it, your seq=y confirmed"
  |                               |
  |    Connection Established     |
```

**Why 3-way?** Both sides must:
1. Agree on each other's Initial Sequence Numbers (ISN)
2. Confirm they can both SEND and RECEIVE

### TCP 4-Way Termination (Connection Teardown)

```
Client                          Server
  |                               |
  |--- FIN ---------------------->|   Client done sending
  |<-- ACK -----------------------|   Server ACKs
  |                               |   [Server may still send data]
  |<-- FIN -----------------------|   Server done sending
  |--- ACK ---------------------->|   Client ACKs
  |                               |
  | [Client waits TIME_WAIT = 2xMSL before CLOSED]
```

> **TIME_WAIT = 2 x MSL (Maximum Segment Lifetime)** = ~60-120 seconds
> Purpose: Ensure last ACK reaches server; absorb delayed packets

### TCP States (Full State Machine)

```
CLOSED
  |--[passive open]--> LISTEN (server waiting)
  |--[active open, send SYN]--> SYN_SENT (client)

LISTEN --[receive SYN, send SYN+ACK]--> SYN_RCVD
SYN_SENT --[receive SYN+ACK, send ACK]--> ESTABLISHED
SYN_RCVD --[receive ACK]--> ESTABLISHED

ESTABLISHED (data transfer)
  |--[send FIN]--> FIN_WAIT_1
  |--[receive FIN, send ACK]--> CLOSE_WAIT

FIN_WAIT_1 --[receive ACK]--> FIN_WAIT_2
FIN_WAIT_2 --[receive FIN, send ACK]--> TIME_WAIT
TIME_WAIT --[2xMSL timeout]--> CLOSED

CLOSE_WAIT --[send FIN]--> LAST_ACK
LAST_ACK --[receive ACK]--> CLOSED
```

### UDP (User Datagram Protocol)

**Characteristics:**
- Connectionless (no handshake)
- Unreliable (no ACK, no retransmission)
- No flow or congestion control
- No ordering guarantee
- Low overhead, very fast

**UDP Header (only 8 bytes):**
```
Bytes 0-1: Source Port
Bytes 2-3: Destination Port
Bytes 4-5: Length (header + data)
Bytes 6-7: Checksum (optional in IPv4, mandatory in IPv6)
```

### TCP vs UDP Comparison

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable (ACK + retransmit) | Unreliable |
| Ordering | Ordered | Not ordered |
| Speed | Slower (overhead) | Faster |
| Header size | 20-60 bytes | 8 bytes |
| Flow control | Yes (window) | No |
| Congestion control | Yes | No |
| Use cases | HTTP, FTP, SMTP, SSH | DNS, DHCP, Video/VoIP, Gaming |

---

## 10. Application Layer Protocols

### FTP (File Transfer Protocol)
- Port **21** (control connection), Port **20** (data connection)
- **Active mode:** Server initiates data connection back to client
- **Passive mode (PASV):** Client initiates both connections (firewall-friendly)
- **SFTP** (SSH FTP) = secure FTP via SSH, port **22**
- **FTPS** = FTP over SSL/TLS, ports 989/990

### Email Protocols

```
Email Flow:
[Sender App (MUA)] --SMTP(25)--> [Sender's Mail Server (MTA)]
                               --SMTP(25)--> [Receiver's MTA]
                                            |
                           POP3(110) or IMAP(143)
                                            |
                                   [Receiver App (MUA)]
```

| Protocol | Port | Function |
|----------|------|----------|
| SMTP | 25 (or 587 with AUTH) | Send mail / server-to-server |
| POP3 | 110 | Download mail and delete from server |
| IMAP | 143 | Sync with server, mail stays on server |
| SMTPS | 465 | SMTP over SSL |
| IMAPS | 993 | IMAP over SSL |
| POP3S | 995 | POP3 over SSL |

**IMAP vs POP3:**
- POP3: Download-and-delete (works offline, one device)
- IMAP: Synchronized (works across multiple devices, mail stays on server)

### Telnet vs SSH

| Feature | Telnet | SSH |
|---------|--------|-----|
| Port | 23 | 22 |
| Encryption | None (plaintext) | Strong encryption |
| Authentication | Password | Password or Public Key |
| Security | Insecure -- avoid | Secure |

### SNMP (Simple Network Management Protocol)
- Manages network devices (routers, switches, servers)
- Port **161** (agent queries), **162** (traps to manager)
- **Manager:** Sends GET, SET, GETNEXT requests
- **Agent:** Software on device, responds + sends traps
- **MIB:** Management Information Base -- database of managed objects
- **Trap:** Unsolicited alert from agent to manager (event-driven)

| Version | Security |
|---------|----------|
| SNMPv1 | Community strings in plaintext -- weak |
| SNMPv2c | Community strings + GetBulk -- still weak |
| SNMPv3 | Authentication (MD5/SHA) + Encryption (DES/AES) -- secure |

---

## 11. DNS — Domain Name System

### What is DNS?
Translates **human-readable domain names** (www.google.com) into **IP addresses** (142.250.80.46).

- Uses **port 53** (UDP for queries <= 512 bytes, TCP for large responses and zone transfers)
- **Hierarchical**, **distributed**, and **cached** system

### DNS Hierarchy

```
                    . (Root)
                   / | \
                .com .bd .org ...
               /         \
           .google      .buet.ac
              |               \
             www              www
```

- **Root Servers:** 13 logical servers (A-M), managed by 12 organizations
- **TLD Servers:** Handle .com, .org, .bd, etc.
- **Authoritative Servers:** Hold actual DNS records for a domain

### DNS Resolution Process

```
Recursive Query Model:

Client Browser
    |
    | 1. Query: "What is IP of www.example.com?"
    v
Local Recursive Resolver (ISP/Google 8.8.8.8)
    |
    | 2. Ask Root Server -> "Ask .com TLD server at X.X.X.X"
    | 3. Ask .com TLD   -> "Ask example.com NS at Y.Y.Y.Y"
    | 4. Ask example.com -> "www.example.com = 93.184.216.34"
    | 5. Cache result (for TTL duration)
    |
    | 6. Return "93.184.216.34" to client
    v
Client connects to 93.184.216.34
```

**Recursive Query:** Client asks resolver to do everything.
**Iterative Query:** Resolver makes each query itself, gets referrals.

### DNS Record Types

| Type | Purpose | Example |
|------|---------|---------|
| **A** | IPv4 address mapping | www -> 93.184.216.34 |
| **AAAA** | IPv6 address mapping | www -> 2606:2800:... |
| **CNAME** | Alias/Canonical name | ftp.example.com -> www.example.com |
| **MX** | Mail exchanger | example.com -> mail.example.com (priority 10) |
| **NS** | Name server for domain | example.com -> ns1.example.com |
| **PTR** | Reverse DNS (IP->name) | 34.216.184.93.in-addr.arpa -> www.example.com |
| **SOA** | Start of Authority | Primary NS, admin, serial, refresh, retry |
| **TXT** | Text record | SPF, DKIM, domain verification |
| **SRV** | Service location | _http._tcp.example.com |

### DNS Caching & TTL
- Responses cached for TTL (Time to Live) seconds
- **TTL too low:** High DNS traffic, fast propagation
- **TTL too high:** Slow change propagation, fewer queries

---

## 12. HTTP & HTTPS

### HTTP Methods

| Method | Purpose | Body | Safe | Idempotent |
|--------|---------|------|------|------------|
| **GET** | Retrieve resource | No | Yes | Yes |
| **POST** | Submit data/create | Yes | No | No |
| **PUT** | Replace resource completely | Yes | No | Yes |
| **PATCH** | Partial update | Yes | No | No |
| **DELETE** | Delete resource | No | No | Yes |
| **HEAD** | Headers only (like GET) | No | Yes | Yes |
| **OPTIONS** | Get supported methods | No | Yes | Yes |

> **Safe** = no side effects | **Idempotent** = same result if called multiple times

### HTTP Status Codes

| Range | Category | Key Codes |
|-------|---------|---------|
| **1xx** | Informational | 100 Continue, 101 Switching Protocols |
| **2xx** | Success | 200 OK, 201 Created, 204 No Content |
| **3xx** | Redirection | 301 Moved Permanently, 302 Found, 304 Not Modified |
| **4xx** | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 429 Too Many Requests |
| **5xx** | Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout |

### HTTP Versions

| Version | Key Features |
|---------|-------------|
| **HTTP/1.0** | New TCP connection per request |
| **HTTP/1.1** | Persistent connections (keep-alive), pipelining, chunked encoding |
| **HTTP/2** | Binary framing, multiplexing (multiple requests on 1 connection), header compression (HPACK), server push |
| **HTTP/3** | Over QUIC (UDP-based), eliminates TCP head-of-line blocking, 0-RTT |

### HTTPS & TLS Handshake

```
Client                          Server
  |                               |
  |-- ClientHello ---------------->|  (TLS version, cipher suites, random)
  |<-- ServerHello ----------------|  (chosen cipher, server random)
  |<-- Certificate (public key) ---|
  |<-- ServerHelloDone ------------|
  |                               |
  |-- ClientKeyExchange ---------->|  (pre-master secret encrypted with server's pub key)
  |-- ChangeCipherSpec ----------->|  (switching to encrypted mode)
  |-- Finished ------------------->|  (encrypted)
  |<-- ChangeCipherSpec -----------|
  |<-- Finished -------------------|
  |                               |
  |  === Encrypted HTTPS Data ===  |
```

**TLS 1.3 improvements:**
- 1-RTT handshake (vs 2-RTT in TLS 1.2)
- 0-RTT session resumption
- Removed weak ciphers (RC4, DES, 3DES, MD5 signatures)
- Perfect Forward Secrecy mandatory (ECDHE)

---

## 13. Network Devices

### Layer-by-Layer Device Overview

```
Layer 7  | Firewall (stateful/NGFW), Load Balancer, Proxy, WAF
---------+---------------------------------------------------------
Layer 3  | Router, Layer-3 Switch, Firewall
---------+---------------------------------------------------------
Layer 2  | Switch, Bridge
---------+---------------------------------------------------------
Layer 1  | Hub, Repeater, NIC, Modem
```

### Device Comparison Table

| Device | Layer | Intelligence | Collision Domain | Broadcast Domain |
|--------|-------|-------------|-----------------|-----------------|
| **Hub** | L1 | None (repeats all) | 1 shared (all ports) | 1 shared |
| **Bridge** | L2 | MAC table | Per segment | 1 shared |
| **Switch** | L2 | MAC table | 1 per port | 1 shared |
| **Router** | L3 | Routing table | 1 per port | 1 per port |

> **Key:** Router **separates broadcast domains**. Switch **separates collision domains**.

### Hub
- **Layer 1** device
- Broadcasts all traffic to all ports (dumb repeater)
- All ports in **same collision domain**
- Obsolete -- replaced by switches

### Switch
- **Layer 2** device
- Learns MAC addresses → builds **MAC Address Table (CAM Table)**
- Forwards frames only to correct port (unicast)
- Each port = separate collision domain

**Switch Forwarding Logic:**
```
Frame arrives on port 1 from MAC_A, destined to MAC_B:

Step 1: Learn - add "MAC_A -> Port 1" to MAC table
Step 2: Look up MAC_B in MAC table:
  FOUND on Port 3   -> Forward ONLY to Port 3 (unicast)
  NOT FOUND          -> Flood to ALL other ports (unknown unicast)
  FF:FF:FF:FF:FF:FF  -> Flood to ALL ports (broadcast)
```

### Spanning Tree Protocol (STP — IEEE 802.1D)

**Problem:** Redundant switch links cause loops -> broadcast storms.

```
Broadcast Storm Without STP:
Switch A ----- Switch B
    |               |
    +--- Switch C --+
Broadcast frame loops forever between A, B, C!
```

**STP Solution:** Block redundant paths while keeping them as backup.

```
STP Process:
1. Elect Root Bridge (lowest Bridge Priority + MAC, default priority=32768)
2. Each non-root switch selects Root Port (port closest to root)
3. Each network segment selects Designated Port (bridge closest to root)
4. All remaining ports -> BLOCKED state

Port States:
BLOCKING  -> Not forwarding, receives BPDUs only
LISTENING -> Preparing (15 sec) - not learning
LEARNING  -> Learning MAC addresses (15 sec) - not forwarding
FORWARDING-> Active, forwarding traffic
DISABLED  -> Administratively shut down

RSTP (802.1w): Rapid STP - converges in seconds (vs 30-50 sec for STP)
MSTP (802.1s): Multiple STP - one instance per VLAN group
```

### VLAN (Virtual LAN)
- Logically segments physical network into multiple virtual networks
- Provides **broadcast domain isolation** on a single switch
- **IEEE 802.1Q** standard adds 4-byte VLAN tag to Ethernet frame

```
Switch VLAN Example:
Port 1,2,3 = VLAN 10 (HR)
Port 4,5,6 = VLAN 20 (Finance)
Port 7     = Trunk (carries both VLANs with tags)

VLAN 10 hosts CANNOT communicate with VLAN 20 hosts at Layer 2
-> Need Router or L3 Switch for inter-VLAN routing
```

**Trunk Port:** Carries multiple VLANs (tagged with 802.1Q)
**Access Port:** Belongs to single VLAN (untagged)
**Native VLAN:** Untagged traffic on a trunk port (default VLAN 1)

---

## 14. LAN Technologies & Ethernet

### Ethernet Frame Format (IEEE 802.3)

```
+----------+--------+--------+------+----------+-----+
| Preamble |  Dst   |  Src   | Type |  Payload |  FCS|
|  7 bytes | MAC 6B | MAC 6B |  2B  | 46-1500B | 4B  |
| + SFD 1B |        |        |      |          |(CRC)|
+----------+--------+--------+------+----------+-----+
Total frame: 64 bytes minimum, 1518 bytes maximum
(with 802.1Q VLAN tag: 1522 bytes max)

Preamble: 10101010 x 7 bytes (synchronization)
SFD: Start Frame Delimiter = 10101011 (marks start of frame)
FCS: Frame Check Sequence (CRC-32 error detection)
```

### Ethernet Speed Standards

| Standard | Speed | Medium | Distance |
|----------|-------|--------|---------|
| 10BASE-T | 10 Mbps | Cat3 UTP | 100 m |
| 100BASE-TX | 100 Mbps | Cat5 UTP | 100 m |
| 1000BASE-T | 1 Gbps | Cat5e UTP | 100 m |
| 10GBASE-T | 10 Gbps | Cat6a UTP | 100 m |
| 1000BASE-SX | 1 Gbps | Multi-mode fiber | 550 m |
| 1000BASE-LX | 1 Gbps | Single-mode fiber | 5 km |
| 10GBASE-SR | 10 Gbps | Multi-mode fiber | 300 m |
| 10GBASE-LR | 10 Gbps | Single-mode fiber | 10 km |

**Naming Convention:** `Speed - Signaling - Medium`
- BASE = Baseband
- T = Twisted pair
- F/S/L = Fiber (Short/Long range)
- X = Fixed encoding

---

## 15. Wireless Networking (WiFi)

### IEEE 802.11 Standards

| Standard | Freq | Max Speed | Key Technology |
|----------|------|-----------|---------------|
| **802.11b** | 2.4 GHz | 11 Mbps | DSSS |
| **802.11a** | 5 GHz | 54 Mbps | OFDM |
| **802.11g** | 2.4 GHz | 54 Mbps | OFDM (backward compat) |
| **802.11n (WiFi 4)** | 2.4/5 GHz | 600 Mbps | MIMO, channel bonding |
| **802.11ac (WiFi 5)** | 5 GHz | 3.5 Gbps | MU-MIMO, 256-QAM, 80MHz |
| **802.11ax (WiFi 6)** | 2.4/5/6 GHz | 9.6 Gbps | OFDMA, 1024-QAM, BSS Color |

### WiFi Terms

- **AP (Access Point):** Connects wireless clients to wired network
- **SSID:** Network name broadcast by AP
- **BSSID:** MAC address of the AP radio
- **BSS:** Basic Service Set -- one AP + its clients
- **ESS:** Extended Service Set -- multiple APs same SSID (roaming)
- **2.4 GHz Channels:** 14 channels total, only **1, 6, 11** are non-overlapping

### WiFi Security Evolution

| Protocol | Encryption | Auth | Status |
|----------|-----------|------|--------|
| **WEP** | RC4 (40/104-bit) | Shared key | BROKEN -- never use |
| **WPA** | TKIP + RC4 | PSK or 802.1X | Deprecated |
| **WPA2** | AES-CCMP (128-bit) | PSK or 802.1X | Current standard |
| **WPA3** | AES-GCMP-256, SAE | SAE or 802.1X | Latest, strongest |

**WPA2 Modes:**
- **Personal (PSK):** Pre-Shared Key (passphrase), home use
- **Enterprise (802.1X):** RADIUS server authentication, corporate use

---

## 16. Network Security

### Firewalls

**Types:**
1. **Packet Filter (Stateless):** ACL-based, checks each packet independently
2. **Stateful Inspection:** Tracks TCP/UDP sessions, allows return traffic
3. **Application Firewall (L7):** Inspects application payload, understands protocols
4. **NGFW (Next-Gen):** IPS, deep packet inspection, user/app identity

**Firewall Rule Example (stateless ACL):**
```
PERMIT TCP any  -> any:80    (Allow HTTP inbound)
PERMIT TCP any  -> any:443   (Allow HTTPS inbound)
PERMIT TCP any:80 -> any     (Allow HTTP responses outbound)
DENY   ALL                   (Implicit deny)
```

### Common Attack Types

| Attack | Description | Defense |
|--------|-------------|---------|
| **DoS/DDoS** | Flood target to exhaust resources | Rate limiting, scrubbing, anycast |
| **SYN Flood** | Half-open connections fill backlog | SYN cookies, rate limiting |
| **Ping of Death** | Oversized ICMP fragments crash OS | OS patches |
| **Smurf Attack** | Amplified ICMP via broadcast | Disable directed broadcasts |
| **MITM** | Intercept and alter traffic | TLS, HSTS, certificate pinning |
| **Replay Attack** | Capture and resend valid packets | Nonces, timestamps, sequence numbers |
| **ARP Spoofing** | Fake ARP replies -> MITM | Dynamic ARP Inspection (DAI) |
| **DNS Spoofing** | Fake DNS responses | DNSSEC |
| **IP Spoofing** | Fake source IP | Ingress/egress filtering (BCP38) |
| **SQL Injection** | Inject SQL via user input | Parameterized queries, WAF |

### Symmetric vs Asymmetric Encryption

| Feature | Symmetric | Asymmetric |
|---------|-----------|-----------|
| Keys | One shared key | Public/private key pair |
| Speed | Very fast | Slow (computationally expensive) |
| Use | Bulk data (after key exchange) | Key exchange, digital signatures |
| Examples | AES-128/256, 3DES, ChaCha20 | RSA, ECC, Diffie-Hellman |
| Key Problem | How to share key securely? | Solved -- share public key freely |

**How HTTPS combines both:**
```
Step 1: Asymmetric (RSA/ECDH) -> securely establish session key
Step 2: Symmetric (AES-256) -> encrypt all data (fast)
```

### Hashing Algorithms

| Algorithm | Output Size | Status |
|-----------|-------------|--------|
| MD5 | 128 bits | Broken -- collision attacks |
| SHA-1 | 160 bits | Weak -- deprecated |
| SHA-256 | 256 bits | Secure -- widely used |
| SHA-3 | 224-512 bits | Secure -- newer |
| HMAC | Same as underlying hash | Keyed hash for MAC |

### PKI & Digital Certificates

- **CA (Certificate Authority):** Signs certificates (VeriSign, Let's Encrypt, DigiCert)
- **Certificate:** Contains: Subject, Public Key, Issuer (CA), Validity period, Signature
- **Chain of Trust:** Root CA -> Intermediate CA -> Server Certificate
- **CRL:** Certificate Revocation List
- **OCSP:** Online Certificate Status Protocol (real-time revocation)

### IPSec

**Modes:**
- **Transport Mode:** Encrypts payload only (original IP header kept) -- host-to-host
- **Tunnel Mode:** Encrypts entire packet + new IP header added -- site-to-site VPN

**Protocols:**
| Protocol | Provides | Port/Proto |
|----------|---------|-----------|
| **AH** (Authentication Header) | Integrity + Authentication, NO encryption | Protocol 51 |
| **ESP** (Encapsulating Security Payload) | Integrity + Auth + **Encryption** | Protocol 50 |
| **IKE** (Internet Key Exchange) | Key negotiation | UDP 500/4500 |

---

## 17. IPv6

### Why IPv6?
- IPv4 exhaustion: only 4.3 billion (2^32) addresses
- IPv6: 128-bit = 3.4 x 10^38 addresses (enough for every grain of sand)

### IPv6 Address Format

```
Full:    2001:0db8:85a3:0000:0000:8a2e:0370:7334
         |--- 8 groups of 4 hex digits = 128 bits ---|

Shortening Rules:
1. Drop leading zeros in each group: 0db8 -> db8, 0000 -> 0
2. Replace one longest run of consecutive all-zero groups with ::

Examples:
2001:0db8:0000:0000:0001:0000:0000:0001
-> 2001:db8::1:0:0:1
-> 2001:db8:0:0:1::1
```

### IPv6 Address Types

| Type | Prefix | Description |
|------|--------|-------------|
| **Global Unicast** | 2000::/3 | Internet-routable addresses |
| **Link-Local** | FE80::/10 | Same-link only, auto-configured |
| **Loopback** | ::1/128 | Self (like 127.0.0.1) |
| **Unspecified** | ::/128 | Source when no address (like 0.0.0.0) |
| **Unique Local** | FC00::/7 | Private use (like RFC1918 private IPs) |
| **Multicast** | FF00::/8 | One-to-many (no broadcast!) |
| **Anycast** | Unicast range | Routed to nearest member |

> **CRITICAL MCQ:** IPv6 has **NO BROADCAST** -- replaced entirely by multicast!

### IPv6 vs IPv4 Comparison

| Feature | IPv4 | IPv6 |
|---------|------|------|
| Address size | 32 bits | 128 bits |
| Notation | Dotted decimal | Colon-separated hex |
| Header size | 20-60 bytes (variable) | 40 bytes (fixed) |
| Header checksum | Yes | **No** (removed for speed) |
| NAT required | Yes | No (enough addresses) |
| Broadcast | Yes | **No** (multicast instead) |
| Auto-config | DHCP | **SLAAC** (stateless, no server) |
| Fragmentation | Routers + hosts | **Hosts only** |
| IPSec | Optional | Optional (was originally mandatory) |

### IPv6 Header (Fixed 40 bytes)

```
+---------+------------------+--------------------+
| Version | Traffic Class(8) |  Flow Label (20)   |
| (4 bits)|                  |                    |
+---------+------------------+--------------------+
|  Payload Length (16)  | Next Header(8) | Hop Limit(8)|
+-----------------------+----------------+-------------+
|                                                      |
|              Source Address (128 bits)               |
|                                                      |
+------------------------------------------------------+
|                                                      |
|           Destination Address (128 bits)             |
|                                                      |
+------------------------------------------------------+
```

**SLAAC (Stateless Address Autoconfiguration):**
```
IPv6 host auto-configures its own global unicast address:
1. Use link-local prefix FE80::/10 + EUI-64 (from MAC address)
2. Router sends Router Advertisement with global prefix (e.g., 2001:db8::/64)
3. Host combines: prefix (64 bits) + EUI-64 (64 bits) = full 128-bit address
4. No DHCP server needed!
```

---

## 18. Flow Control & Congestion Control

### Flow Control
**Goal:** Prevent a fast sender from overwhelming a **slow receiver's buffer**.
**Mechanism:** Receiver advertises its buffer space as **window size** in TCP header.

```
Receiver buffer = 16KB
Receiver tells sender: "Window = 16384 bytes"
Sender sends up to 16KB -> waits for ACK + new window

Buffer fills up: Receiver sends "Window = 4096"
Buffer full:     Receiver sends "Window = 0" -> sender STOPS

When space frees: Receiver sends Window Update -> sender resumes
```

### Congestion Control
**Goal:** Prevent a fast sender from overwhelming the **network**.
**Mechanism:** TCP infers congestion from packet loss signals.

```
Congestion Window (cwnd): How much unACKed data sender can have
Effective send rate = min(cwnd, receiver_window)
```

#### Phase 1: Slow Start (Exponential)
```
cwnd = 1 MSS (Maximum Segment Size, typically 1460 bytes)
For each ACK received: cwnd += 1 MSS
-> Doubles every RTT (exponential growth)
-> Continues until cwnd >= ssthresh (slow start threshold)
-> Then switches to Congestion Avoidance
```

#### Phase 2: Congestion Avoidance (Linear - AIMD)
```
For each full window ACKed: cwnd += 1 MSS (per RTT)
-> Linear growth (Additive Increase)

On Congestion (loss detected):
  [Timeout loss]:     ssthresh = cwnd/2, cwnd = 1 MSS -> Slow Start
  [3 dup ACKs loss]:  ssthresh = cwnd/2, cwnd = ssthresh -> Fast Recovery
```

#### TCP Congestion Control Diagram

```
cwnd |          *                    (timeout resets to 1)
(MSS)|         **
     |        * *      /--*
     |       *  *    /--  *\
     |      *   *  /--     *\
     |    **    **            \   /
     |  **    ^ssthresh        \ /
     | *         ^slow start    *
     |*                         * (3 dup ACKs, fast recovery)
     +-----------------------------------------> time
        SS   CA   SS   CA
```

**TCP Reno vs TCP Cubic:**
- **TCP Reno:** AIMD; halves cwnd on loss
- **TCP Cubic (Linux default):** Uses cubic function for cwnd growth; aggressive on high-BDP links
- **TCP BBR (Google):** Model-based; estimates bottleneck BW + RTT

#### Fast Retransmit & Fast Recovery

```
Scenario: Packet 3 lost, packets 4,5,6 arrive
Receiver: sends duplicate ACK 2 for each out-of-order packet

Sender receives 3 duplicate ACK 2:
  -> Fast Retransmit: Immediately retransmit packet 3 (don't wait for timeout)
  -> Fast Recovery (TCP Reno): ssthresh = cwnd/2, cwnd = ssthresh
  -> Skip Slow Start, go directly to Congestion Avoidance
```

---

## 19. Error Detection & Correction

### Error Types
- **Single-bit error:** Only 1 bit flipped
- **Burst error:** 2+ consecutive bits changed (more common in practice)

### Detection vs Correction
- **Detection only:** Know error occurred -> request retransmission (ARQ)
- **Forward Error Correction (FEC):** Correct without retransmission (need more redundancy)

### Parity Bit
```
Single parity: 1 bit added
  Even parity: 1011001 -> count of 1s=4, add 0 -> 10110010
  Odd parity:  1011001 -> count of 1s=4, add 1 -> 10110011
Detects: 1-bit errors (odd number of errors)
Cannot detect: 2-bit errors

2D Parity (VRC + LRC):
  VRC: Vertical parity per byte
  LRC: Longitudinal parity (extra row)
  Can DETECT and CORRECT single-bit errors
```

### Internet Checksum (Used in IP, TCP, UDP)
```
Algorithm:
1. Divide data into 16-bit segments
2. Add all segments (ones' complement arithmetic)
3. Ones' complement of sum = checksum
4. Receiver: sum all including checksum; result = 1111...1111 means no error

Ones' complement addition: if carry out of MSB, add carry back
Example: 1111 + 1111 = 1 1110 -> 1110 + 1 = 1111
```

### CRC (Cyclic Redundancy Check)
```
Best for burst error detection
Standard: CRC-32 (Ethernet), CRC-16 (HDLC)

Steps (Sender):
1. M = data bits, G = generator polynomial (known to both)
2. Append r zeros to M  (r = degree of G)
3. Divide M*2^r by G (XOR/modulo-2 division)
4. Remainder R = CRC checksum
5. Send: M + R (total bits = original + r bits)

Steps (Receiver):
1. Divide received frame by G
2. Remainder = 0 -> no error
3. Remainder != 0 -> error detected

CRC-32 can detect:
  - All burst errors of length < 32
  - All burst errors of length = 32 with probability 1 - 2^-31
```

### Hamming Code (Error Correction)
```
Need r parity bits for m data bits: 2^r >= m + r + 1

For 4 data bits:
  2^3 = 8 >= 4+3+1 = 8  -> need 3 parity bits

Code positions: 1  2  3  4  5  6  7
                P1 P2 D1 P4 D2 D3 D4

Parity bit coverage (binary position):
  P1 (pos 1 = 001): covers pos 1,3,5,7 (positions with bit 0 = 1)
  P2 (pos 2 = 010): covers pos 2,3,6,7 (positions with bit 1 = 1)
  P4 (pos 4 = 100): covers pos 4,5,6,7 (positions with bit 2 = 1)

Error Correction:
  Receiver computes syndrome: [P4_check][P2_check][P1_check]
  Syndrome = 0 -> no error
  Syndrome = 101 -> error in position 5, flip that bit
```

### Hamming Distance

```
Hamming distance = number of bit positions where two codewords differ
Example: 10001001 and 10110001 differ in positions 3,4 -> d = 2

Minimum Hamming distance (d_min) of code:
  To DETECT d errors:  d_min >= d + 1
  To CORRECT d errors: d_min >= 2d + 1
```

| d_min | Detect | Correct |
|-------|--------|---------|
| 2 | 1-bit errors | 0 (detect only) |
| 3 | 2-bit errors | 1-bit errors |
| 4 | 3-bit errors | 1-bit errors |
| 5 | 4-bit errors | 2-bit errors |

---

## 20. MAC Addresses & ARP

### MAC Address Structure

```
AA:BB:CC:DD:EE:FF
|--------|----------|
OUI (3B) | Device ID (3B)
Vendor   | Unique per NIC

OUI = Organizationally Unique Identifier (assigned by IEEE)
Examples: 00:1A:2B = Cisco, 3C:22:FB = Apple
```

**Special MAC Addresses:**
- `FF:FF:FF:FF:FF:FF` = Layer 2 Broadcast (all devices on segment)
- First byte odd (LSB=1) = Multicast
- First byte even (LSB=0) = Unicast

### ARP (Address Resolution Protocol)

Maps **IP address -> MAC address** on same network.
- **ARP Request:** Broadcast asking "Who has IP X?"
- **ARP Reply:** Unicast response "IP X is at MAC Y"
- **ARP Cache:** Table stores IP-to-MAC mappings (with timeout)

```
ARP Process:
Host A (192.168.1.1) wants to reach Host B (192.168.1.5)
A has no MAC for B in cache:

1. A broadcasts ARP Request:
   "Who has 192.168.1.5? Tell 192.168.1.1"
   Source: A's MAC | Dest: FF:FF:FF:FF:FF:FF

2. B sees its IP, replies ARP Reply (unicast):
   "192.168.1.5 is at AA:BB:CC:DD:EE:FF"
   Source: B's MAC | Dest: A's MAC

3. A stores "192.168.1.5 -> AA:BB:CC:DD:EE:FF" in ARP cache
4. A can now send Ethernet frame directly to B
```

**ARP Variants:**

| Type | Purpose |
|------|---------|
| **Gratuitous ARP** | Host ARPs for its own IP (conflict detection, failover) |
| **Proxy ARP** | Router answers ARP for hosts on other networks |
| **Reverse ARP (RARP)** | MAC -> IP (obsolete, replaced by DHCP) |
| **InARP** | Used in Frame Relay to map DLCI -> IP |

**ARP Spoofing Attack:**
```
Attacker sends fake ARP Reply: "Gateway IP is at ATTACKER_MAC"
Victim updates ARP cache -> sends traffic to attacker -> MITM!
Defense: Dynamic ARP Inspection (DAI), static ARP entries
```

---

## 21. NAT & DHCP

### NAT (Network Address Translation)

**Purpose:** Allow private IPs to communicate on Internet using public IPs.

```
Private Network          NAT Router              Internet
192.168.1.10:5000 -> 203.0.113.1:40001 -> 93.184.216.34:80
192.168.1.11:5001 -> 203.0.113.1:40002 -> 93.184.216.34:80
192.168.1.12:5002 -> 203.0.113.1:40003 -> 8.8.8.8:53

NAT Translation Table (PAT):
Inside Local          Inside Global           Outside Global
192.168.1.10:5000  -> 203.0.113.1:40001  ->  93.184.216.34:80
192.168.1.11:5001  -> 203.0.113.1:40002  ->  93.184.216.34:80
```

**NAT Types:**

| Type | Description | Public IPs |
|------|-------------|-----------|
| **Static NAT** | 1:1 permanent mapping | 1 per private host |
| **Dynamic NAT** | Pool of public IPs, assigned as needed | Pool (1 per session) |
| **PAT / NAT Overload** | Many:1 using different source ports | Just 1 public IP |

> **Home routers use PAT** -- many devices share one public IP.

**NAT Limitations:**
- Breaks end-to-end connectivity (servers behind NAT unreachable)
- Complicates protocols embedding IP in payload (FTP, SIP, H.323)
- Must recalculate IP/TCP/UDP checksums
- IPv6 designed to eliminate NAT

### DHCP (Dynamic Host Configuration Protocol)

**Provides automatically:** IP address, subnet mask, default gateway, DNS servers, lease time.

**DHCP DORA Process:**

```
Client                                   DHCP Server
  |                                            |
  |-- DHCPDISCOVER (broadcast) -------------->|
  |   "I need an IP address!" (src: 0.0.0.0)  |
  |                                            |
  |<-- DHCPOFFER (broadcast) -----------------|
  |   "How about 192.168.1.100 for 24 hours?" |
  |                                            |
  |-- DHCPREQUEST (broadcast) --------------->|
  |   "I'll take 192.168.1.100, please"       |
  |                                            |
  |<-- DHCPACK (broadcast) ------------------|
  |   "192.168.1.100 is yours until expiry"   |
  |                                            |
  |  [Client configures IP + mask + GW + DNS] |

Ports: Client UDP 68, Server UDP 67
Protocols use broadcast because client has no IP yet!
```

**DHCP Lease Renewal:**
```
t = 0     : Lease granted (T1 = 50%, T2 = 87.5% of lease)
t = T1    : Client tries to renew (unicast to same server)
t = T2    : Client broadcasts to any DHCP server (if T1 renewal failed)
t = Expiry: Client must release IP and restart DORA
```

**Key DHCP Options:**

| Option | Meaning |
|--------|---------|
| Option 1 | Subnet mask |
| Option 3 | Default gateway |
| Option 6 | DNS server(s) |
| Option 12 | Hostname |
| Option 15 | Domain name |
| Option 43 | Vendor-specific info |
| Option 51 | IP address lease time |
| Option 53 | DHCP message type |
| Option 66 | TFTP server name |

---

## 22. VPN & Tunneling

### VPN (Virtual Private Network)

Creates **encrypted tunnel** over public network to securely connect private networks or hosts.

**Types:**
1. **Site-to-Site VPN:** Two routers connect entire office networks
2. **Remote Access VPN:** Individual user connects to corporate network
3. **SSL VPN:** VPN over HTTPS (browser-based or client)

**VPN Protocols:**

| Protocol | Security | Layer | Notes |
|----------|----------|-------|-------|
| **IPSec/IKEv2** | Strong (AES) | L3 | Most common site-to-site |
| **OpenVPN** | Strong (TLS) | L3/L2 | Open source, flexible |
| **WireGuard** | Modern (ChaCha20) | L3 | Fast, simple, newer |
| **L2TP/IPSec** | Good | L2+L3 | Windows built-in |
| **PPTP** | Broken -- insecure | L2 | Never use |
| **SSL VPN** | Good (TLS) | L4-7 | Clientless option |

### Tunneling

Encapsulating one protocol inside another:

```
GRE Tunnel:
Original IP packet:  [IP Header | Payload]
After GRE:          [Outer IP | GRE Header | Inner IP | Payload]

Use: Connecting non-contiguous networks, carrying IPv6 over IPv4 (6in4)
```

| Protocol | Encapsulates | Use Case |
|----------|-------------|---------|
| **GRE** | Any L3 protocol | Generic tunneling |
| **L2TP** | L2 frames | VPN with PPP |
| **MPLS** | IP packets with labels | ISP traffic engineering |
| **VXLAN** | Ethernet frames | Data center overlays |

---

## 23. Cloud & SDN Basics

### SDN (Software-Defined Networking)

**Problem with traditional networking:**
- Control plane (routing decisions) tied to each device
- Hard to program, manage centrally, or change behavior quickly

**SDN Solution:** Decouple control plane from data plane.

```
Traditional:                    SDN:
+----------+                 +-----------------+
| Router   |                 | SDN Controller  |  <- Control Plane
| [Control]|                 | (centralized)   |     (OpenFlow)
| [Data]   |                 +-----------------+
+----------+                        |
                                    | Southbound API
                    +-------+-------+--------+
                    |Switch |Switch |  Switch |  <- Data Plane
                    |(dumb) |(dumb) |  (dumb) |     (just forward)
```

**SDN Benefits:**
- Centralized network programmability
- Rapid provisioning and change
- Network virtualization
- Vendor-independent management

**Key Standards:** OpenFlow (control protocol), NETCONF/YANG, REST APIs

### Cloud Computing Service Models

```
On-Premise  IaaS        PaaS        SaaS
            (You)       (You)       (Provider)
Apps      | Apps      |           |
Runtime   | Runtime   | Apps      |
OS        | OS        | Runtime   | Everything
Servers   | Servers   | OS        | managed by
Storage   | Storage   | Servers   | provider
Network   | Network   | Storage   |
          | Managed   | Network   |
          | by you    | Managed   |
          | hardware  | by prov.  |
```

| Model | Provider Manages | Customer Manages | Examples |
|-------|-----------------|-----------------|---------|
| **IaaS** | Hardware, virtualization | OS, runtime, apps, data | AWS EC2, Azure VMs, GCP Compute |
| **PaaS** | Everything + OS + runtime | Apps and data | Google App Engine, Heroku, Azure App Service |
| **SaaS** | Everything | Only user/data | Gmail, Office 365, Salesforce, Zoom |

### Cloud Deployment Models

| Type | Description | Pros | Cons |
|------|-------------|------|------|
| **Public** | Shared, provider-owned | Low cost, elastic | Less control, shared resources |
| **Private** | Dedicated, org-owned | Full control, security | High cost, less elastic |
| **Hybrid** | Mix of public + private | Flexible, burst to cloud | Complex management |
| **Community** | Shared by specific group | Shared cost, compliance | Limited by community |

---

## 24. Quick MCQ Cheat Sheet

### Critical Numbers to Memorize

| Item | Value |
|------|-------|
| IPv4 address size | 32 bits |
| IPv6 address size | 128 bits |
| MAC address size | 48 bits (6 bytes) |
| Ethernet MTU | 1500 bytes |
| Ethernet min frame | 64 bytes |
| Ethernet max frame | 1518 bytes (1522 with 802.1Q) |
| IPv4 min header | 20 bytes |
| IPv4 max header | 60 bytes |
| TCP min header | 20 bytes |
| TCP max header | 60 bytes |
| UDP header | 8 bytes (fixed) |
| IP Total Length max | 65,535 bytes |
| Fragment offset unit | 8 bytes |
| DHCP client port | 68 (UDP) |
| DHCP server port | 67 (UDP) |
| OSI layers | 7 |
| TCP/IP layers | 4 |

### Complete Port Reference

| Port | Protocol | Service |
|------|----------|---------|
| 20 | TCP | FTP Data |
| 21 | TCP | FTP Control |
| 22 | TCP | SSH / SFTP |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP+UDP | DNS |
| 67 | UDP | DHCP Server |
| 68 | UDP | DHCP Client |
| 69 | UDP | TFTP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 119 | TCP | NNTP (Usenet) |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP (agent) |
| 162 | UDP | SNMP (traps) |
| 179 | TCP | BGP |
| 389 | TCP | LDAP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB/CIFS |
| 465 | TCP | SMTPS |
| 514 | UDP | Syslog |
| 636 | TCP | LDAPS |
| 993 | TCP | IMAPS |
| 995 | TCP | POP3S |
| 1194 | UDP | OpenVPN |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |
| 5432 | TCP | PostgreSQL |

### Layer-Protocol Mapping

| OSI Layer | Protocols & Technologies |
|-----------|--------------------------|
| Application (7) | HTTP, HTTPS, FTP, SMTP, POP3, IMAP, DNS, Telnet, SSH, SNMP, DHCP |
| Presentation (6) | SSL/TLS, JPEG, MPEG, ASCII, EBCDIC, encryption, compression |
| Session (5) | NetBIOS, RPC, PPTP, SQL sessions |
| Transport (4) | TCP, UDP, SCTP |
| Network (3) | IPv4, IPv6, ICMP, IGMP, OSPF, BGP, RIP |
| Data Link (2) | Ethernet (802.3), WiFi (802.11), PPP, HDLC, Frame Relay, ARP* |
| Physical (1) | DSL, SONET, 10BASE-T, RS-232, hubs, cables |

> *ARP placement varies: IEEE says L2, many texts say L3. GATE/exams: usually **Data Link**.

### Key Algorithm Mappings

| Problem | Solution |
|---------|---------|
| Distance vector routing | Bellman-Ford algorithm |
| Link state routing | Dijkstra's SPF algorithm |
| Switching loop prevention | STP (Spanning Tree Protocol) |
| Error correction | Hamming code |
| Error detection | CRC, Checksum, Parity |
| Flow control | Sliding window protocol |
| Congestion control | TCP Slow Start + AIMD |
| Wired channel access | CSMA/CD |
| Wireless channel access | CSMA/CA |
| IP assignment | DHCP (DORA process) |
| Name resolution | DNS (recursive/iterative) |

### Essential Formulas

```
# Propagation & Transmission Delays
Propagation delay  = Distance / Propagation speed  (~2x10^8 m/s in copper)
Transmission delay = Frame size (bits) / Bandwidth (bps)
Total RTT          = 2 x Propagation delay (approx)

# Efficiency
Stop-and-Wait:     eta = 1 / (1 + 2a),  where a = T_prop / T_trans
Sliding Window:    eta = W / (1 + 2a),  if W < 1+2a  (W = window size)
                   eta = 1,             if W >= 1+2a

# ALOHA Throughput
Pure ALOHA:    S = G x e^(-2G),  max S = 18.4% at G = 0.5
Slotted ALOHA: S = G x e^(-G),   max S = 36.8% at G = 1

# Channel Capacity
Shannon: C = B x log2(1 + S/N)  [S/N = SNR]
Nyquist: C = 2 x B x log2(L)   [L = signal levels, noiseless]

# Subnetting
Hosts per subnet = 2^(host bits) - 2
Subnets created  = 2^(borrowed bits)
New prefix       = original prefix + borrowed bits

# Hamming Code
Parity bits r: 2^r >= m + r + 1  (m = data bits)
Error position = XOR of all set bit positions

# CRC
FCS bits = degree of generator polynomial
Burst error length <= r -> always detected
```

### Comparison Tables for MCQ

**Routing Protocols AD (Administrative Distance):**
```
Connected:    0
Static:       1
EIGRP:       90 (internal)
OSPF:       110
IS-IS:      115
RIP:        120
eBGP:        20
iBGP:       200
```

**Switching Protocols Timeline:**
```
STP (802.1D)  -> 30-50 sec convergence (traditional)
RSTP (802.1w) -> 1-2 sec convergence   (rapid)
MSTP (802.1s) -> per-VLAN instances    (scalable)
```

**WiFi Frequency vs Range:**
```
2.4 GHz: Longer range, more interference (overlaps with Bluetooth, microwave)
5 GHz:   Shorter range, less interference, higher speed
6 GHz:   WiFi 6E only, least interference, highest speed
```

### Mnemonics Summary

| Mnemonic | Stands for |
|----------|-----------|
| **All People Seem To Need Data Processing** | OSI L7->L1 (Application, Presentation, Session, Transport, Network, Data Link, Physical) |
| **Please Do Not Throw Sausage Pizza Away** | OSI L1->L7 |
| **DORA** | DHCP: Discover, Offer, Request, Acknowledge |
| **SYN SYN-ACK ACK** | TCP 3-way handshake |
| **FIN ACK FIN ACK** | TCP 4-way teardown |
| **BLLD** | STP port states: Blocking, Listening, Learning, Disabled (+ Forwarding) |
| **A=1-126, B=128-191, C=192-223, D=224-239** | IP Address Class ranges |
| **10, 172.16, 192.168** | Private IP ranges (RFC 1918) |
| **127.0.0.1** | Loopback / localhost |
| **169.254.x.x** | APIPA (no DHCP found) |

---

## Key Topics for Written Exam (Deep Answers)

### 1. OSI Model — Full Explanation
Mention: all 7 layers, PDU at each layer, protocols, devices, function.
Include encapsulation process top-down (sender) and decapsulation bottom-up (receiver).

### 2. TCP Reliable Delivery Mechanism
- **Connection setup:** 3-way handshake (SYN, SYN-ACK, ACK)
- **Sequence + ACK numbers:** Track bytes, ensure ordering
- **Retransmission:** Timeout + 3 dup-ACK triggers
- **Flow control:** Receiver window (rwnd)
- **Congestion control:** Slow start, AIMD, fast retransmit/recovery
- **Connection teardown:** 4-way FIN handshake + TIME_WAIT

### 3. TCP vs UDP — When to Use Each
TCP: When reliability, ordering, and correctness matter (file transfer, web, email)
UDP: When speed matters more than reliability (live video, VoIP, gaming, DNS)

### 4. DNS Resolution — Trace Full Process
1. Browser cache -> OS cache -> Local recursive resolver cache
2. Root server -> TLD server -> Authoritative server
3. Response cached at each level for TTL
4. Mention recursive vs iterative queries, record types

### 5. Subnetting — Solve Any Problem
Given: Network address + requirement
Find: Subnet mask, block size, network address, broadcast, host range for each subnet

### 6. CSMA/CD vs CSMA/CA — Why the Difference?
- Wired: can detect collision (voltages add) -> CSMA/CD: detect + abort + backoff
- Wireless: cannot detect collision (own signal drowns others) -> CSMA/CA: avoid using RTS/CTS
- Hidden terminal problem justifies RTS/CTS in wireless

### 7. Routing Protocols — RIP vs OSPF vs BGP
Focus: algorithm used, metric, convergence speed, scalability, loop prevention, use case

### 8. TCP Congestion Control — Draw the Graph
Slow start (exponential) -> ssthresh -> Congestion Avoidance (linear) -> loss event -> reset/halve
Show AIMD sawtooth pattern

### 9. IPv6 vs IPv4 — Comprehensive Comparison
Address space, header design, NAT elimination, SLAAC, no broadcast, no fragmentation by routers

### 10. Subnetting + VLSM Problem Solving
Always: allocate largest first, check block size is power of 2, verify no overlaps

---

*Compiled for BUET MSc | University Admission | BCS Preli+Written | Bank IT Officer*
*Covers: Gate CS, BUET, DU, KUET admission exams*
*Last Updated: October 2026*

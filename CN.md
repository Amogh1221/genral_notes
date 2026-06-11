# Computer Networks — Complete Notes
### Interview-Ready | Placement-Focused | Intuition + Theory + Q&A

> **How to use:** Every section opens with the interview question it answers.
> The explanation *is* the answer. Read once → answer any CN question.

---

## Table of Contents

**PART 1 — FOUNDATIONS**
1. [What is a Computer Network?](#1-what-is-a-computer-network)
2. [Network Types — LAN, WAN, MAN, PAN](#2-network-types)
3. [Network Topologies](#3-network-topologies)
4. [Network Devices](#4-network-devices)

**PART 2 — LAYERED MODELS**
5. [OSI Model — All 7 Layers](#5-osi-model)
6. [TCP/IP Model](#6-tcpip-model)
7. [OSI vs TCP/IP](#7-osi-vs-tcpip)

**PART 3 — ADDRESSING**
8. [IP Addressing — IPv4 & IPv6](#8-ip-addressing)
9. [Subnetting & CIDR](#9-subnetting--cidr)
10. [MAC Address & ARP](#10-mac-address--arp)
11. [NAT & DHCP](#11-nat--dhcp)

**PART 4 — TRANSPORT LAYER**
12. [TCP — Connection, Reliability, Flow Control](#12-tcp)
13. [UDP](#13-udp)
14. [TCP vs UDP](#14-tcp-vs-udp)
15. [TCP Congestion Control](#15-tcp-congestion-control)
16. [Ports & Sockets](#16-ports--sockets)

**PART 5 — APPLICATION LAYER PROTOCOLS**
17. [DNS](#17-dns)
18. [HTTP & HTTPS](#18-http--https)
19. [Other Key Protocols](#19-other-key-protocols)

**PART 6 — NETWORK LAYER**
20. [Routing — Algorithms & Protocols](#20-routing)
21. [IP Fragmentation & TTL](#21-ip-fragmentation--ttl)

**PART 7 — DATA LINK & PHYSICAL LAYER**
22. [Data Link Layer — MAC, Framing, Error Detection](#22-data-link-layer)
23. [Switching — Circuit, Packet, Message](#23-switching)

**PART 8 — SECURITY**
24. [Network Security Basics](#24-network-security-basics)
25. [SSL/TLS & HTTPS](#25-ssltls--https)
26. [Common Attacks & Defences](#26-common-attacks--defences)

**PART 9 — INTERVIEW Q&A**
27. [Interview Q&A — Models & Addressing](#27-interview-qa--models--addressing)
28. [Interview Q&A — TCP/UDP & Transport](#28-interview-qa--tcpudp--transport)
29. [Interview Q&A — Application Layer & Protocols](#29-interview-qa--application-layer--protocols)
30. [Interview Q&A — Advanced & Tricky](#30-interview-qa--advanced--tricky)
31. [The Classic: What happens when you type google.com?](#31-what-happens-when-you-type-googlecom)
32. [Quick Reference Cheat Sheet](#32-quick-reference-cheat-sheet)

---

# PART 1 — FOUNDATIONS

---

## 1. What is a Computer Network?

> **Interview Q:** *What is a computer network? Why do we need it?*

A **computer network** is a collection of two or more devices (computers, servers, phones, routers) interconnected by communication links so they can share data, resources, and services.

### Why networks exist

| Need | Example |
|---|---|
| **Resource sharing** | Multiple PCs share one printer |
| **Data sharing** | Google Drive, shared databases |
| **Communication** | Email, video calls, chat |
| **Centralised management** | IT admins manage 10,000 machines remotely |
| **Reliability** | Redundant paths keep service up when one link fails |

### Key Terminology

| Term | Meaning |
|---|---|
| **Node** | Any device connected to a network (PC, phone, router) |
| **Link** | Physical or logical connection between two nodes |
| **Bandwidth** | Maximum data transfer rate of a link (Mbps, Gbps) |
| **Latency** | Time for a packet to travel from source to destination |
| **Throughput** | Actual data transfer rate achieved in practice |
| **Jitter** | Variation in packet delay — bad for real-time audio/video |
| **Packet** | Unit of data transmitted over a network |
| **Protocol** | Set of rules governing communication between devices |

> **Bandwidth vs Throughput:** Bandwidth is the theoretical maximum (like a road's speed limit). Throughput is what you actually get (like your actual driving speed in traffic).

---

## 2. Network Types

> **Interview Q:** *What is the difference between LAN, WAN, and MAN?*

| Type | Full Name | Coverage | Speed | Example |
|---|---|---|---|---|
| **PAN** | Personal Area Network | ~10 metres | Low | Bluetooth between phone and laptop |
| **LAN** | Local Area Network | Building / campus | Very High (1–100 Gbps) | Office network, home WiFi |
| **MAN** | Metropolitan Area Network | City | High | City-wide ISP network |
| **WAN** | Wide Area Network | Country / globe | Variable | The Internet, corporate VPN |

### Other Classifications

| Type | Description |
|---|---|
| **Intranet** | Private network within an organisation (uses internet protocols internally) |
| **Extranet** | Intranet extended to trusted external partners (suppliers, customers) |
| **Internet** | Global public WAN connecting billions of devices |
| **VPN** | Virtual Private Network — encrypted tunnel over the public internet |
| **SDN** | Software-Defined Networking — network configuration controlled via software |

---

## 3. Network Topologies

> **Interview Q:** *What is network topology? What are the types?*

**Network topology** describes how devices are physically or logically arranged and connected.

| Topology | Structure | Pros | Cons |
|---|---|---|---|
| **Bus** | All devices on one cable | Simple, cheap | One break = whole network down; performance degrades with more devices |
| **Star** | All devices connect to central hub/switch | Easy to add/remove; one failure doesn't affect others | Central point failure kills whole network |
| **Ring** | Devices in circular chain | Predictable performance | One break disrupts whole ring (unless dual ring) |
| **Mesh** | Every device connects to every other | Highly fault-tolerant; redundant paths | Expensive; complex wiring |
| **Tree** | Hierarchical star-of-stars | Scalable; easy to manage | Root failure kills the tree |
| **Hybrid** | Combination of above | Flexible | Complex |

> **Most real networks use Star or Hybrid topology.** The internet itself is a mesh at the backbone level.

---

## 4. Network Devices

> **Interview Q:** *What is the difference between a hub, switch, and router?*

| Device | OSI Layer | Function | Key Point |
|---|---|---|---|
| **Hub** | Layer 1 (Physical) | Broadcasts incoming signal to ALL ports | Dumb — no intelligence; causes collisions; obsolete |
| **Switch** | Layer 2 (Data Link) | Forwards frames to the correct port using MAC address table | Smart — learns MAC addresses; creates separate collision domains |
| **Router** | Layer 3 (Network) | Routes packets between different networks using IP addresses | Connects LAN to WAN (internet) |
| **Bridge** | Layer 2 | Connects two LAN segments, filters by MAC | Like a 2-port switch |
| **Gateway** | Layer 4–7 | Protocol translator between different networks | Default gateway = your router |
| **Repeater** | Layer 1 | Amplifies signal to extend range | No intelligence |
| **Firewall** | Layer 3–7 | Filters traffic based on rules | Security device |
| **Access Point (AP)** | Layer 2 | Provides wireless connectivity | WiFi router has built-in AP |
| **NIC** | Layer 1–2 | Network Interface Card — connects device to network | Has a unique MAC address |
| **Modem** | Layer 1 | Modulates/demodulates signal for ISP connection | Converts digital ↔ analog |

### Hub vs Switch vs Router — One-liner summary

```
Hub    : shouts to everyone (broadcast)
Switch : whispers to the right person (MAC-based forwarding)
Router : knows the route to other cities (IP-based routing)
```

---

# PART 2 — LAYERED MODELS

---

## 5. OSI Model

> **Interview Q:** *Explain the OSI model. What does each layer do?*

The **OSI (Open Systems Interconnection)** model is a conceptual 7-layer framework that standardises how data is transmitted between devices, regardless of the underlying hardware or software.

**Mnemonic (top to bottom): "All People Seem To Need Data Processing"**
**Mnemonic (bottom to top): "Please Do Not Throw Sausage Pizza Away"**

```
Layer 7 — Application    ← user-facing protocols
Layer 6 — Presentation   ← data format, encryption
Layer 5 — Session        ← connection management
Layer 4 — Transport      ← end-to-end delivery
Layer 3 — Network        ← routing, IP addressing
Layer 2 — Data Link      ← MAC addressing, framing
Layer 1 — Physical       ← bits, cables, signals
```

### Layer-by-Layer Detail

#### Layer 7 — Application

- **What it does:** Provides network services directly to user applications
- **Not** the application itself (Chrome, Outlook) — it is the protocol the app uses
- **Protocols:** HTTP, HTTPS, FTP, SMTP, DNS, SSH, Telnet, SNMP
- **Data unit:** Message / Data

#### Layer 6 — Presentation

- **What it does:** Translates data between application format and network format
- Handles: **Encryption/Decryption** (SSL/TLS), **Compression**, **Encoding** (ASCII, JPEG, MP4)
- **Analogy:** Translator — converts your language to a common language the network understands
- **Protocols:** SSL/TLS, JPEG, MPEG, ASCII

#### Layer 5 — Session

- **What it does:** Establishes, manages, and terminates sessions (logical connections) between applications
- Handles: Authentication, Authorisation, session checkpointing (resume from failure)
- **Protocols:** NetBIOS, RPC, PPTP
- **Analogy:** The moderator in a phone conference call — starts the session, keeps it going, ends it

#### Layer 4 — Transport

- **What it does:** End-to-end communication between processes on different hosts
- Handles: **Segmentation/Reassembly**, **Flow control**, **Error detection**, **Port numbers**
- **Protocols:** TCP (reliable), UDP (fast)
- **Data unit:** Segment (TCP) / Datagram (UDP)
- **Key concept:** Multiplexing — many applications use the network simultaneously via ports

#### Layer 3 — Network

- **What it does:** Logical addressing and routing — how to get data from source to destination across multiple networks
- Handles: **IP addressing**, **Routing**, **Packet forwarding**, **Fragmentation**
- **Protocols:** IP (IPv4, IPv6), ICMP (`ping`), OSPF, BGP, RIP
- **Devices:** Router
- **Data unit:** Packet

#### Layer 2 — Data Link

- **What it does:** Node-to-node data transfer on the same network. Packages bits into frames and handles MAC addressing
- Handles: **Framing**, **MAC addressing**, **Error detection (CRC)**, **Flow control (local)**
- Two sub-layers: **LLC** (Logical Link Control) and **MAC** (Media Access Control)
- **Protocols:** Ethernet, WiFi (802.11), PPP, ARP
- **Devices:** Switch, Bridge, NIC
- **Data unit:** Frame

#### Layer 1 — Physical

- **What it does:** Transmits raw bits (0s and 1s) over a physical medium
- Handles: **Cables**, **Connectors**, **Signal encoding**, **Bit rate**, **Modulation**
- **Media:** Copper wire (Ethernet), Fibre optic, Radio waves (WiFi), Coaxial
- **Devices:** Hub, Repeater, Cables, NIC (hardware part)
- **Data unit:** Bit

### Data Encapsulation / Decapsulation

```
Sender (top → bottom):                Receiver (bottom → top):
  Application: Data                     Physical: bits arrive
  Transport:   Data + TCP header        Data Link: removes frame header
               = Segment                Network:   removes IP header
  Network:     Segment + IP header      Transport: removes TCP header
               = Packet                 Application: Data received!
  Data Link:   Packet + Frame header
               = Frame
  Physical:    Frame → bits → medium
```

> At each layer going down, a **header is added (encapsulation)**.
> At each layer going up, the header is **stripped (decapsulation)**.

---

## 6. TCP/IP Model

> **Interview Q:** *What is the TCP/IP model? How many layers does it have?*

The **TCP/IP model** (also called the Internet model) is the practical, implementation-oriented model on which the real internet is built. Developed by DARPA in the 1970s.

```
Layer 4 — Application      ← HTTP, FTP, DNS, SMTP, SSH
Layer 3 — Transport        ← TCP, UDP
Layer 2 — Internet         ← IP, ICMP, ARP
Layer 1 — Network Access   ← Ethernet, WiFi, physical hardware
```

| TCP/IP Layer | Corresponds to OSI Layers | Key Protocols |
|---|---|---|
| **Application** | Application + Presentation + Session (7, 6, 5) | HTTP, HTTPS, FTP, DNS, SMTP, SSH |
| **Transport** | Transport (4) | TCP, UDP |
| **Internet** | Network (3) | IP, ICMP, ARP, OSPF |
| **Network Access** | Data Link + Physical (2, 1) | Ethernet, WiFi, PPP |

> The TCP/IP model is what the real internet uses. The OSI model is a conceptual reference. In interviews you must know both — OSI for explaining concepts layer by layer, TCP/IP for understanding how the internet actually works.

---

## 7. OSI vs TCP/IP

> **Interview Q:** *What is the difference between the OSI and TCP/IP models?*

| | OSI Model | TCP/IP Model |
|---|---|---|
| Layers | 7 | 4 |
| Developed by | ISO (International Standards Organisation) | DARPA / US DoD |
| Purpose | Conceptual framework / reference | Practical implementation |
| Protocol-specific | No — protocol-independent | Yes — built around TCP and IP |
| Usage | Teaching, troubleshooting reference | Actual internet |
| Session/Presentation | Separate layers (5 and 6) | Merged into Application layer |
| Physical/Data Link | Separate layers (1 and 2) | Merged into Network Access layer |

---

# PART 3 — ADDRESSING

---

## 8. IP Addressing

> **Interview Q:** *What is an IP address? What is the difference between IPv4 and IPv6?*

An **IP address** (Internet Protocol address) is a **logical address** assigned to every device on a network, used to identify and locate it for routing purposes.

### IPv4

- **32-bit** address written in dotted-decimal: `192.168.1.1`
- Each octet = 8 bits → 4 octets × 8 bits = 32 bits
- Total addresses: 2³² ≈ **4.3 billion**
- **Problem:** Exhausted — not enough for every device on earth

```
192.168.1.1
│   │   │ └── Host part
│   │   └──── Host part
│   └──────── Network part
└──────────── Network part
```

### IPv4 Address Classes

| Class | Range | Default Subnet Mask | Usage |
|---|---|---|---|
| **A** | 1.0.0.0 – 126.0.0.0 | /8  (255.0.0.0) | Large networks (ISPs) |
| **B** | 128.0.0.0 – 191.255.0.0 | /16 (255.255.0.0) | Medium networks |
| **C** | 192.0.0.0 – 223.255.255.0 | /24 (255.255.255.0) | Small networks |
| **D** | 224.0.0.0 – 239.255.255.255 | N/A | Multicast |
| **E** | 240.0.0.0 – 255.255.255.255 | N/A | Reserved / experimental |

### Private IP Ranges (RFC 1918 — not routable on internet)

| Range | Class | Common use |
|---|---|---|
| 10.0.0.0 – 10.255.255.255 | A | Large private networks |
| 172.16.0.0 – 172.31.255.255 | B | Medium private networks |
| 192.168.0.0 – 192.168.255.255 | C | Home/office routers |

### Special Addresses

| Address | Purpose |
|---|---|
| `127.0.0.1` | Loopback — sends to itself (tests TCP/IP stack) |
| `0.0.0.0` | This host / any address (used in routing tables) |
| `255.255.255.255` | Limited broadcast (all hosts on local network) |
| `169.254.x.x` | APIPA — auto-assigned when DHCP fails |

### IPv6

- **128-bit** address written in hexadecimal: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Total addresses: 2¹²⁸ ≈ **340 undecillion** — essentially unlimited
- Written in 8 groups of 4 hex digits separated by colons
- Leading zeros can be omitted; consecutive all-zero groups replaced by `::`

```
Full:       2001:0db8:0000:0000:0000:0000:0000:0001
Compressed: 2001:db8::1
```

### IPv4 vs IPv6

| | IPv4 | IPv6 |
|---|---|---|
| Address size | 32-bit | 128-bit |
| Address space | ~4.3 billion | ~340 undecillion |
| Format | Dotted decimal (192.168.1.1) | Hexadecimal (2001:db8::1) |
| Header size | 20–60 bytes (variable) | 40 bytes (fixed) |
| NAT needed | Yes (address exhaustion) | No (enough addresses for everything) |
| Security | Optional (IPSec) | Built-in (IPSec mandatory) |
| Broadcast | Yes | No — uses multicast/anycast |
| Configuration | Manual or DHCP | SLAAC (auto-configure) or DHCPv6 |
| Checksum in header | Yes | No (offloaded to transport layer) |

---

## 9. Subnetting & CIDR

> **Interview Q:** *What is subnetting? What is CIDR notation?*

### Subnetting

**Subnetting** divides a large network into smaller sub-networks (subnets). Benefits:
- Better security (isolate departments)
- Reduce broadcast traffic (smaller broadcast domains)
- Efficient IP address usage

### Subnet Mask

A subnet mask is a 32-bit number that divides an IP address into **network** and **host** portions.

```
IP address:    192.168.1.100  =  11000000.10101000.00000001.01100100
Subnet mask:   255.255.255.0  =  11111111.11111111.11111111.00000000
                                  └──────────────────┘ └──────────┘
                                       Network part        Host part

Network address: 192.168.1.0     (all host bits = 0)
Broadcast:       192.168.1.255   (all host bits = 1)
Usable hosts:    192.168.1.1 – 192.168.1.254  (254 hosts)
```

### CIDR — Classless Inter-Domain Routing

CIDR replaces class-based addressing with a prefix length notation: `192.168.1.0/24`

The `/24` means 24 bits are the network portion → 8 bits for hosts → 2⁸ - 2 = 254 usable hosts.

```
/24 → 256 addresses,  254 usable  (most common home/office subnet)
/25 → 128 addresses,  126 usable
/26 → 64  addresses,  62  usable
/27 → 32  addresses,  30  usable
/28 → 16  addresses,  14  usable
/30 → 4   addresses,  2   usable  (point-to-point links)
/32 → 1   address,    0   usable  (single host route)
```

### Quick Subnetting Formula

```
Number of subnets  = 2^(bits borrowed)
Hosts per subnet   = 2^(host bits) − 2   (subtract network + broadcast)
```

---

## 10. MAC Address & ARP

> **Interview Q:** *What is a MAC address? How does ARP work?*

### MAC Address

A **MAC (Media Access Control) address** is a **48-bit** hardware address burned into the NIC by the manufacturer. Used for node-to-node communication on the same network.

```
Format: 00:1A:2B:3C:4D:5E  (6 pairs of hex digits)
        └────────┘ └────────┘
        OUI (manufacturer)  Device identifier
```

- Globally unique (in theory)
- Can be spoofed in software (MAC spoofing)
- Operates at **Layer 2 (Data Link)**
- Only relevant within the same local network — routers don't forward MAC addresses

### ARP — Address Resolution Protocol

**ARP** resolves an IP address to a MAC address within a local network. Needed because switches work with MAC addresses but upper layers use IP.

```
Scenario: PC-A (192.168.1.10) wants to send to PC-B (192.168.1.20)
PC-A knows the IP but needs the MAC address.

Step 1: ARP Request (broadcast)
  PC-A broadcasts: "Who has 192.168.1.20? Tell 192.168.1.10"
  Destination MAC: FF:FF:FF:FF:FF:FF  ← broadcast to ALL devices

Step 2: ARP Reply (unicast)
  PC-B responds: "192.168.1.20 is at AA:BB:CC:DD:EE:FF"

Step 3: PC-A caches this in its ARP table
  192.168.1.20 → AA:BB:CC:DD:EE:FF  (expires after ~20 minutes)

Step 4: PC-A sends frame directly to PC-B using MAC address
```

### ARP Cache Poisoning

An attacker sends fake ARP replies associating their MAC with another device's IP. Traffic meant for the victim is redirected to the attacker (Man-in-the-Middle attack).

---

## 11. NAT & DHCP

> **Interview Q:** *What is NAT? What is DHCP?*

### NAT — Network Address Translation

**NAT** allows multiple devices on a private network to share a **single public IP address** when accessing the internet. Solves IPv4 address exhaustion.

```
Private Network:          Router/NAT          Internet
192.168.1.10 ─────────┐                ┌───→ 8.8.8.8
192.168.1.11 ─────────┤  Public IP:    │
192.168.1.12 ─────────┘  203.0.113.5 ──┘

All three devices appear to the internet as 203.0.113.5
The router maintains a translation table:
  203.0.113.5:5001 ↔ 192.168.1.10:45231
  203.0.113.5:5002 ↔ 192.168.1.11:33891
```

**Types of NAT:**
- **Static NAT** — one-to-one mapping (one private IP ↔ one public IP)
- **Dynamic NAT** — pool of public IPs assigned dynamically
- **PAT (Port Address Translation) / NAT Overload** — many-to-one using different ports (most common home router)

**Drawback of NAT:** Breaks end-to-end connectivity — external hosts can't initiate connections to internal hosts directly (requires port forwarding). IPv6 eliminates the need for NAT.

### DHCP — Dynamic Host Configuration Protocol

**DHCP** automatically assigns IP addresses and network configuration to devices when they join a network. Eliminates manual IP configuration.

```
DHCP DORA process:

1. DISCOVER  — Client broadcasts: "I need an IP address!" (source: 0.0.0.0)
2. OFFER     — DHCP server offers: "Use 192.168.1.50 for 24 hours"
3. REQUEST   — Client accepts: "I'll take 192.168.1.50"
4. ACK       — Server confirms: "192.168.1.50 is yours for 24 hours"

What DHCP assigns:
  - IP address
  - Subnet mask
  - Default gateway
  - DNS server address
  - Lease duration
```

- DHCP server is usually your router at home
- **Lease time** — how long the IP is reserved for that device
- **APIPA (169.254.x.x)** — self-assigned when DHCP server is unreachable

---

# PART 4 — TRANSPORT LAYER

---

## 12. TCP

> **Interview Q:** *How does TCP work? Explain the three-way handshake. How does TCP ensure reliability?*

**TCP (Transmission Control Protocol)** is a **connection-oriented, reliable** transport layer protocol. Every byte sent is guaranteed to arrive in order, without errors, exactly once.

### Three-Way Handshake (Connection Establishment)

```
Client                              Server
  │                                   │
  │──── SYN (seq=x) ─────────────────→│   "I want to connect; my seq starts at x"
  │                                   │
  │←─── SYN-ACK (seq=y, ack=x+1) ────│   "OK; my seq starts at y; I expect x+1 next"
  │                                   │
  │──── ACK (ack=y+1) ───────────────→│   "Got it; I expect y+1 next"
  │                                   │
  │         [Connection ESTABLISHED]  │
```

- **SYN** = Synchronise — initiate connection, send starting sequence number
- **ACK** = Acknowledge — confirm receipt
- **SYN-ACK** = Both combined

> **Why three steps and not two?** A two-way handshake can't confirm that the server's sequence number was received by the client. The third ACK confirms the server's SYN-ACK arrived, synchronising both directions.

### Four-Way Handshake (Connection Termination)

```
Client                              Server
  │──── FIN ────────────────────────→│   "I'm done sending"
  │←─── ACK ────────────────────────│   "OK, noted"
  │←─── FIN ────────────────────────│   "I'm done too"
  │──── ACK ────────────────────────→│   "OK, connection closed"
```

**TIME_WAIT state:** After sending the final ACK, the client waits 2×MSL (Maximum Segment Lifetime, ~60s) before fully closing. Ensures the final ACK arrives and prevents confusion from delayed packets.

### How TCP Ensures Reliability

| Mechanism | How |
|---|---|
| **Sequence numbers** | Every byte numbered; receiver reorders out-of-order segments |
| **Acknowledgements (ACK)** | Receiver sends ACK for each segment received |
| **Retransmission** | If ACK not received within timeout, sender retransmits |
| **Checksum** | Detects bit errors in header and data |
| **Duplicate detection** | Sequence numbers detect and discard duplicates |

### Flow Control — Sliding Window

TCP uses a **sliding window** to prevent the sender from overwhelming the receiver.

```
Receiver advertises a window size (rwnd) in every ACK:
  "I can accept 65535 bytes before I need you to wait"

Sender can have at most rwnd bytes unacknowledged at any time.
Window "slides" forward as ACKs arrive.

     Sent & ACK'd  |  Sent, not ACK'd  |  Can send  |  Can't send yet
     ─────────────── ──────────────────── ────────── ──────────────────
                    └─────────── window ────────────┘
```

---

## 13. UDP

> **Interview Q:** *What is UDP? When do you use it over TCP?*

**UDP (User Datagram Protocol)** is a **connectionless, unreliable** transport layer protocol. It sends data without establishing a connection and with no guarantee of delivery, ordering, or error recovery.

```
UDP Header (only 8 bytes vs TCP's 20 bytes):
  ┌──────────────┬──────────────┐
  │ Source Port  │  Dest Port   │  2 bytes each
  ├──────────────┴──────────────┤
  │    Length    │   Checksum   │  2 bytes each
  └─────────────────────────────┘
```

**Characteristics:**
- No connection setup — just send
- No ACKs — fire and forget
- No ordering guarantee
- No congestion control
- No flow control
- Very low overhead — just 8-byte header

**When to use UDP:**
- **Speed matters more than reliability** — video streaming, online gaming
- **Real-time** — VoIP, video calls (a dropped frame is better than a late one)
- **Small one-off queries** — DNS lookups (quick request-response, retransmit if needed)
- **Broadcast/Multicast** — UDP supports these; TCP doesn't
- **Application handles reliability itself** — QUIC (HTTP/3) is built on UDP

---

## 14. TCP vs UDP

> **Interview Q:** *What is the difference between TCP and UDP? When would you use each?*

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented (3-way handshake) | Connectionless |
| Reliability | Guaranteed delivery | No guarantee |
| Ordering | In-order delivery | No ordering |
| Error recovery | Yes (retransmission) | No (application handles) |
| Flow control | Yes (sliding window) | No |
| Congestion control | Yes (slow start, AIMD) | No |
| Speed | Slower (overhead) | Faster |
| Header size | 20–60 bytes | 8 bytes |
| Use cases | HTTP, HTTPS, FTP, SMTP, SSH | DNS, VoIP, video streaming, gaming, DHCP |

> **Interview insight:** "Use TCP when every byte must arrive correctly. Use UDP when speed matters and the application can tolerate or handle loss itself."

---

## 15. TCP Congestion Control

> **Interview Q:** *How does TCP handle congestion? Explain slow start and congestion avoidance.*

**TCP Congestion Control** prevents too much data from flooding the network, which would cause routers to drop packets and degrade performance for everyone.

TCP uses a **Congestion Window (cwnd)** — the maximum number of unacknowledged bytes the sender can have outstanding. The effective window = min(cwnd, rwnd).

### Phases

```
cwnd
  │                          * packet loss detected (ssthresh = cwnd/2)
  │                    *   *
  │               *         \
  │          *                \──── Congestion Avoidance (+1 per RTT)
  │     *                           *  *  *  *  *
  │  *  (Slow Start: doubles each RTT)
  │*
  └──────────────────────────────────────────→ Time
     ssthresh
```

### 1. Slow Start

- Begin with `cwnd = 1 MSS` (Maximum Segment Size, typically 1460 bytes)
- **Double cwnd every RTT** (exponential growth) until `cwnd >= ssthresh`
- "Slow" refers to the initial value, not the growth rate

### 2. Congestion Avoidance

- When `cwnd >= ssthresh`, switch to linear growth
- **Increase cwnd by 1 MSS per RTT** (additive increase)
- Called **AIMD: Additive Increase, Multiplicative Decrease**

### 3. Detecting Congestion — Two signals

| Signal | What it means | TCP Response |
|---|---|---|
| **Timeout** | Severe congestion; packet lost | `ssthresh = cwnd/2`, reset `cwnd = 1`, restart slow start |
| **3 Duplicate ACKs** | Mild congestion; later packets arrived | `ssthresh = cwnd/2`, `cwnd = ssthresh` — **Fast Retransmit + Fast Recovery** |

### Fast Retransmit

If 3 duplicate ACKs received, retransmit the missing segment immediately **without waiting for timeout**.

### Additive Increase, Multiplicative Decrease (AIMD)

```
Increase: cwnd += 1 MSS per RTT  (probe for more bandwidth)
Decrease: cwnd = cwnd/2          (back off quickly on congestion)
→ Converges to fair share of bandwidth for all TCP connections
```

---

## 16. Ports & Sockets

> **Interview Q:** *What is a port? What is a socket?*

### Ports

A **port** is a 16-bit number (0–65535) that identifies a specific process/service on a host. IP address identifies the host; port identifies the application on that host.

```
Communication = IP Address + Port Number
192.168.1.5:80  →  web server on that machine
192.168.1.5:22  →  SSH server on that machine
```

| Range | Type | Examples |
|---|---|---|
| 0–1023 | **Well-Known Ports** | HTTP=80, HTTPS=443, FTP=21, SSH=22, DNS=53 |
| 1024–49151 | **Registered Ports** | MySQL=3306, PostgreSQL=5432, Redis=6379 |
| 49152–65535 | **Dynamic/Ephemeral Ports** | Client-side OS-assigned ports |

### Common Well-Known Ports

| Port | Protocol | Service |
|---|---|---|
| 20, 21 | TCP | FTP (data, control) |
| 22 | TCP | SSH |
| 23 | TCP | Telnet (insecure, obsolete) |
| 25 | TCP | SMTP (email sending) |
| 53 | TCP + UDP | DNS |
| 67, 68 | UDP | DHCP (server, client) |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 (email retrieval) |
| 143 | TCP | IMAP (email) |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |
| 6379 | TCP | Redis |
| 27017 | TCP | MongoDB |

> **Can two services use the same port?** Not on the same protocol simultaneously. But TCP:80 and UDP:80 are separate, so both can coexist.

### Socket

A **socket** is the combination of IP address + port number. It is the endpoint of a network communication.

```
Socket = IP Address : Port
Client socket: 192.168.1.10:54321  (ephemeral port)
Server socket: 93.184.216.34:443   (well-known HTTPS port)

A TCP connection is uniquely identified by:
(client_IP, client_port, server_IP, server_port)
→ This is why a server can handle thousands of connections on port 443
  — each connection has a different client IP/port combination
```

---

# PART 5 — APPLICATION LAYER PROTOCOLS

---

## 17. DNS

> **Interview Q:** *What is DNS? How does it resolve a domain name? What is recursive vs iterative resolution?*

**DNS (Domain Name System)** is a distributed hierarchical database that translates human-readable domain names (google.com) into IP addresses (142.250.80.14).

It is the **phone book of the internet**.

### DNS Resolution Process

```
Browser requests: www.google.com

Step 1: Check browser cache → not found
Step 2: Check OS cache (/etc/hosts) → not found
Step 3: Query Recursive Resolver (usually your ISP or 8.8.8.8)

  Recursive Resolver asks:
    ↓ Root Nameserver: "Who handles .com?"
      → "Ask the .com TLD nameserver at 192.5.6.30"
    ↓ .com TLD Nameserver: "Who handles google.com?"
      → "Ask Google's authoritative nameserver at 216.239.32.10"
    ↓ Google's Authoritative NS: "What is www.google.com?"
      → "142.250.80.14"

Step 4: Recursive resolver caches and returns 142.250.80.14
Step 5: Browser connects to 142.250.80.14
```

### DNS Hierarchy

```
.  (root)
├── .com  (TLD — Top Level Domain)
│     └── google.com  (Second Level Domain — authoritative NS)
│               └── www.google.com  (hostname)
├── .org
├── .net
└── .in, .uk, .de  (country code TLDs)
```

### DNS Record Types

| Record | Purpose | Example |
|---|---|---|
| **A** | Maps hostname → IPv4 address | `google.com → 142.250.80.14` |
| **AAAA** | Maps hostname → IPv6 address | `google.com → 2607:f8b0::...` |
| **CNAME** | Alias — maps name → another name | `www.google.com → google.com` |
| **MX** | Mail exchange server for a domain | `google.com → smtp.google.com` |
| **NS** | Nameserver for the domain | `google.com → ns1.google.com` |
| **TXT** | Arbitrary text (used for SPF, DKIM, verification) | `"v=spf1 include:..."` |
| **PTR** | Reverse DNS — maps IP → hostname | `14.80.250.142.in-addr.arpa → google.com` |
| **SOA** | Start of Authority — zone metadata | Primary NS, admin email, TTL |

### Recursive vs Iterative Resolution

| | Recursive | Iterative |
|---|---|---|
| Who does the work | Resolver does all the work on behalf of client | Client contacts each nameserver itself |
| Used by | Client → Recursive Resolver | Recursive Resolver → Root/TLD/Auth NS |
| Client effort | Minimal — one query, one answer | Must handle each step |

**TTL (Time To Live):** DNS records have a TTL — how long to cache the answer. Low TTL = faster propagation of changes but more DNS queries.

---

## 18. HTTP & HTTPS

> **Interview Q:** *How does HTTP work? What is the difference between HTTP and HTTPS? What are HTTP methods and status codes?*

### HTTP — HyperText Transfer Protocol

**HTTP** is an application layer protocol for transmitting hypermedia (HTML, images, video, APIs). Runs over TCP. **Stateless** — each request is independent; server has no memory of previous requests.

```
Client request:
  GET /index.html HTTP/1.1
  Host: www.example.com
  User-Agent: Mozilla/5.0
  Accept: text/html

Server response:
  HTTP/1.1 200 OK
  Content-Type: text/html
  Content-Length: 1234

  <html>...</html>
```

### HTTP Methods (Verbs)

| Method | Purpose | Idempotent | Safe |
|---|---|---|---|
| **GET** | Retrieve a resource | Yes | Yes |
| **POST** | Submit data / create resource | No | No |
| **PUT** | Replace a resource entirely | Yes | No |
| **PATCH** | Partially update a resource | No | No |
| **DELETE** | Remove a resource | Yes | No |
| **HEAD** | Like GET but returns only headers | Yes | Yes |
| **OPTIONS** | List supported methods | Yes | Yes |

> **Idempotent:** Calling it multiple times has the same result as calling it once.
> **Safe:** Does not modify the resource (read-only).

### HTTP Status Codes

| Code | Category | Common Examples |
|---|---|---|
| **1xx** | Informational | 100 Continue |
| **2xx** | Success | 200 OK, 201 Created, 204 No Content |
| **3xx** | Redirection | 301 Moved Permanently, 302 Found, 304 Not Modified |
| **4xx** | Client Error | 400 Bad Request, 401 Unauthorised, 403 Forbidden, 404 Not Found, 429 Too Many Requests |
| **5xx** | Server Error | 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout |

### HTTP Versions

| Version | Key Feature |
|---|---|
| **HTTP/1.0** | New connection per request |
| **HTTP/1.1** | **Persistent connections** (keep-alive), pipelining, chunked transfer |
| **HTTP/2** | **Multiplexing** (multiple requests over one TCP connection, no head-of-line blocking), header compression (HPACK), server push, binary protocol |
| **HTTP/3** | Built on **QUIC** (UDP-based), reduces connection latency, better mobile performance |

### HTTPS

**HTTPS** = HTTP + **TLS (Transport Layer Security)**. Encrypts communication between client and server.

- Default port: **443** (HTTP uses 80)
- Provides: **Confidentiality** (encryption), **Integrity** (data not tampered), **Authentication** (server identity verified via certificate)
- Uses **asymmetric encryption** to exchange keys, then **symmetric encryption** for actual data (faster)

---

## 19. Other Key Protocols

> **Interview Q:** *What is FTP? What is the difference between SMTP, POP3, and IMAP?*

### Email Protocols

| Protocol | Port | Role |
|---|---|---|
| **SMTP** | 25, 587 | Sending email (client → server, server → server) |
| **POP3** | 110, 995 | Downloading email — removes from server (offline) |
| **IMAP** | 143, 993 | Accessing email — stays on server (sync across devices) |

### File Transfer

| Protocol | Port | Notes |
|---|---|---|
| **FTP** | 20 (data), 21 (control) | Plaintext — insecure; sends credentials unencrypted |
| **SFTP** | 22 | FTP over SSH — encrypted |
| **FTPS** | 990 | FTP over TLS |

### Remote Access

| Protocol | Port | Notes |
|---|---|---|
| **SSH** | 22 | Encrypted remote terminal access. Replaced Telnet. |
| **Telnet** | 23 | Unencrypted — obsolete and insecure |
| **RDP** | 3389 | Windows Remote Desktop |

### Network Management

| Protocol | Port | Notes |
|---|---|---|
| **SNMP** | 161 | Monitor and manage network devices |
| **ICMP** | — | Used by `ping` and `traceroute`; no port (network layer) |
| **NTP** | 123 | Network Time Protocol — synchronises clocks |

---

# PART 6 — NETWORK LAYER

---

## 20. Routing

> **Interview Q:** *What is routing? What is the difference between static and dynamic routing? Explain RIP and OSPF.*

**Routing** is the process of selecting the best path for data packets to travel from source to destination across multiple networks.

### Routing Table

Every router maintains a routing table — a map of networks and which interface/next-hop to use to reach them.

```
Destination      Subnet Mask      Gateway         Interface
0.0.0.0          0.0.0.0          192.168.1.1     eth0   ← default route
192.168.1.0      255.255.255.0    directly conn.  eth0
10.0.0.0         255.0.0.0        192.168.1.2     eth0
```

**Longest prefix match:** When multiple routes match, the most specific one (longest prefix) wins.

### Static vs Dynamic Routing

| | Static Routing | Dynamic Routing |
|---|---|---|
| Configuration | Manual — admin adds routes | Automatic — routers exchange route info |
| Scalability | Poor (manual config for every change) | Good (adapts automatically) |
| Overhead | None | Consumes bandwidth and CPU |
| Use case | Small networks, default routes | Large networks, internet |

### Routing Algorithms

#### Distance Vector (e.g., RIP)
- Each router knows the distance to its neighbours and shares its routing table with them
- Routers compute the best path based on **hop count**
- Simple but slow to converge; suffers from **count-to-infinity problem**
- **RIP:** Maximum 15 hops (16 = unreachable); updates every 30s

#### Link State (e.g., OSPF)
- Each router floods the entire network with its own link state (what it's connected to and at what cost)
- Every router builds a **complete map of the network** and runs Dijkstra's algorithm to find shortest paths
- Faster convergence; more resource-intensive
- **OSPF (Open Shortest Path First):** Uses cost (based on bandwidth) not hop count. Hierarchical (areas). Most common interior routing protocol

#### Path Vector (e.g., BGP)
- Used between autonomous systems (ISPs) on the internet
- **BGP (Border Gateway Protocol):** The routing protocol of the internet
- Routes based on AS (Autonomous System) path policies, not just metrics
- Very stable, slow to converge

### Routing Protocol Summary

| Protocol | Type | Metric | Scope |
|---|---|---|---|
| **RIP** | Distance Vector | Hop count (max 15) | Small networks |
| **OSPF** | Link State | Cost (bandwidth) | Enterprise networks |
| **BGP** | Path Vector | AS path + policy | Internet (between ISPs) |
| **EIGRP** | Hybrid (Cisco) | Bandwidth + delay | Cisco networks |

---

## 21. IP Fragmentation & TTL

> **Interview Q:** *What is IP fragmentation? What is TTL?*

### IP Fragmentation

**MTU (Maximum Transmission Unit)** is the largest packet a network link can carry. Ethernet MTU = 1500 bytes.

If an IP packet is larger than the MTU of a link, the router **fragments** it into smaller pieces. The destination reassembles them.

```
Large packet (4000 bytes)
    ↓  Router fragments it
Fragment 1: 1500 bytes (includes IP header)
Fragment 2: 1500 bytes
Fragment 3: 1000 bytes
    ↓  Destination reassembles
Original packet (4000 bytes)
```

- Each fragment has: fragment offset (position in original), MF (More Fragments) flag
- If any fragment is lost, the entire packet must be re-sent (TCP handles retransmission)
- **Path MTU Discovery (PMTUD):** Routers send ICMP "Fragmentation Needed" messages to avoid fragmentation

### TTL — Time to Live

**TTL** is an 8-bit field in the IP header that limits how long a packet can circulate the internet. Prevents infinite loops.

- Each router that forwards the packet **decrements TTL by 1**
- When TTL reaches **0**, the router drops the packet and sends an **ICMP "Time Exceeded"** message back to the sender
- `ping` uses TTL to detect routing loops
- `traceroute` works by sending packets with TTL=1, 2, 3... — each router returns ICMP Time Exceeded, revealing the path

---

# PART 7 — DATA LINK & PHYSICAL LAYER

---

## 22. Data Link Layer

> **Interview Q:** *What is the data link layer responsible for? What is framing? What is CRC?*

The **Data Link Layer** is responsible for reliable node-to-node data transfer on the **same network segment**. It packages IP packets into **frames** and handles physical addressing (MAC).

### Key Responsibilities

| Function | Description |
|---|---|
| **Framing** | Adds header + trailer to IP packet to create a frame; defines start/end |
| **MAC Addressing** | Source and destination MAC in frame header |
| **Error Detection** | CRC (Cyclic Redundancy Check) in frame trailer |
| **Flow Control** | Prevent sender from overwhelming receiver on same link |
| **Media Access Control** | Decide who gets to use the shared medium (CSMA/CD, CSMA/CA) |

### CRC — Cyclic Redundancy Check

CRC is an error-detection algorithm. Sender computes a checksum of the data and appends it to the frame. Receiver recomputes and compares — if different, frame is discarded (and TCP triggers retransmission).

### CSMA/CD — Collision Detection (Ethernet)

Used in wired Ethernet (now largely replaced by full-duplex switches):
1. Check if medium is idle before transmitting
2. If two nodes transmit simultaneously → **collision** detected
3. Both stop, wait a random **backoff time**, retry
4. With switches, each port is a separate collision domain → virtually no collisions

### CSMA/CA — Collision Avoidance (WiFi / 802.11)

WiFi can't detect collisions (can't transmit and receive simultaneously):
1. Check if medium is idle
2. Wait a random backoff before transmitting anyway
3. Send RTS (Request to Send) / CTS (Clear to Send) to reserve the medium
4. Transmit, wait for ACK

---

## 23. Switching

> **Interview Q:** *What is the difference between circuit switching, packet switching, and message switching?*

**Switching** is how data is forwarded through a network.

| Type | How | Pros | Cons | Example |
|---|---|---|---|---|
| **Circuit Switching** | Dedicated physical path established before communication | Guaranteed bandwidth, low latency | Wasted capacity if no data; setup time | PSTN (old phone network) |
| **Packet Switching** | Data split into packets; each routed independently | Efficient use of bandwidth; fault-tolerant | Variable delay (jitter); out-of-order possible | The Internet |
| **Message Switching** | Entire message stored and forwarded at each hop | No dedicated path | High delay; needs large buffer | Early telegraph, email |

> **The internet uses packet switching.** This is why it is efficient and resilient — if one router goes down, packets find another route.

---

# PART 8 — SECURITY

---

## 24. Network Security Basics

> **Interview Q:** *What is a firewall? What is the difference between symmetric and asymmetric encryption?*

### Firewall

A **firewall** is a network security device (hardware or software) that monitors and controls incoming/outgoing traffic based on predefined security rules.

| Type | How | Example |
|---|---|---|
| **Packet Filtering** | Inspects IP/port headers; stateless | Simple ACLs on routers |
| **Stateful Inspection** | Tracks connection state; allows return traffic automatically | Most modern firewalls |
| **Application Layer (WAF)** | Inspects HTTP content; understands application protocols | Web Application Firewalls |
| **Next-Gen (NGFW)** | Deep packet inspection + IDS/IPS + application awareness | Palo Alto, Fortinet |

### Encryption

**Symmetric encryption:** Same key for encryption and decryption. Fast. Problem: how to securely share the key?
- Examples: AES, DES, 3DES
- Used for: bulk data encryption (file content, database)

**Asymmetric encryption:** Two mathematically related keys — **public key** (encrypt) and **private key** (decrypt). Slow. Solves key distribution.
- Examples: RSA, ECC, Diffie-Hellman
- Used for: key exchange, digital signatures, TLS handshake

**Hybrid approach (used in TLS/HTTPS):**
1. Use asymmetric encryption to securely exchange a **session key**
2. Use that session key with symmetric encryption for the actual data transfer
3. Best of both: security of asymmetric + speed of symmetric

---

## 25. SSL/TLS & HTTPS

> **Interview Q:** *How does TLS work? What happens during the TLS handshake?*

**TLS (Transport Layer Security)** is the protocol that adds security to HTTP → HTTPS. Provides encryption, authentication, and integrity.

### TLS Handshake (TLS 1.3 simplified)

```
Client                                        Server
  │                                              │
  │──── ClientHello ────────────────────────────→│
  │     (TLS version, cipher suites, random)     │
  │                                              │
  │←─── ServerHello + Certificate + Key Exchange─│
  │     (chosen cipher, server cert, public key) │
  │                                              │
  │  Client verifies certificate with CA         │
  │  Client generates session key                │
  │──── Finished (encrypted with session key) ──→│
  │                                              │
  │←─── Finished ───────────────────────────────│
  │                                              │
  │         [All data now encrypted with         │
  │          symmetric session key]              │
```

**Certificate Authority (CA):** A trusted third party (DigiCert, Let's Encrypt, Comodo) that signs server certificates, proving the server is who it claims to be.

**Certificate chain:** Server cert → Intermediate CA cert → Root CA cert (trusted by your OS/browser)

---

## 26. Common Attacks & Defences

> **Interview Q:** *What is a DDoS attack? What is a Man-in-the-Middle attack?*

| Attack | Description | Defence |
|---|---|---|
| **DDoS** (Distributed Denial of Service) | Flood target with traffic from many sources until it crashes | Rate limiting, CDN, scrubbing centres |
| **SYN Flood** | Send many SYN packets without completing handshake; exhausts server's connection table | SYN cookies, firewalls, rate limiting |
| **Man-in-the-Middle (MITM)** | Attacker intercepts communication between two parties | TLS/HTTPS, certificate pinning |
| **ARP Poisoning** | Fake ARP replies redirect traffic to attacker | Dynamic ARP inspection, static ARP entries |
| **DNS Spoofing** | Return fake DNS answers redirecting user to malicious site | DNSSEC, use trusted DNS resolvers |
| **SQL Injection** | Inject SQL commands via input fields | Parameterised queries, input validation |
| **Phishing** | Fake website/email tricks user into giving credentials | User education, multi-factor authentication |
| **Replay Attack** | Capture and retransmit valid authentication tokens | Timestamps, nonces, TLS |

---

# PART 9 — INTERVIEW Q&A

---

## 27. Interview Q&A — Models & Addressing

**Q: What are the 7 layers of the OSI model and what does each do?**
> Physical (1) — transmits raw bits via cables/radio. Data Link (2) — MAC addressing, framing, error detection within a local network. Network (3) — IP addressing and routing across networks. Transport (4) — end-to-end delivery, TCP/UDP, ports, flow control. Session (5) — establishes and manages sessions between applications. Presentation (6) — data format translation, encryption, compression. Application (7) — user-facing protocols like HTTP, DNS, SMTP.

**Q: What is the difference between OSI and TCP/IP model?**
> OSI is a 7-layer conceptual reference model developed by ISO for teaching and troubleshooting. TCP/IP is a 4-layer practical model developed by DARPA on which the real internet is built. TCP/IP merges OSI's Application + Presentation + Session into one Application layer, and Physical + Data Link into one Network Access layer. In practice, when engineers diagnose issues they use OSI layer terminology ("this is a Layer 3 problem") but the actual protocols follow TCP/IP.

**Q: What is the difference between a MAC address and an IP address?**
> A **MAC address** is a 48-bit hardware address burned into the NIC — it identifies a device on a local network segment (Layer 2). It doesn't change when you move networks. An **IP address** is a 32-bit (IPv4) or 128-bit (IPv6) logical address assigned by the network — it identifies a device's location in the internet (Layer 3). It changes when you move to a different network. MAC is for local delivery; IP is for global routing. ARP bridges the two by mapping IP → MAC within a local network.

**Q: What is ARP and why is it needed?**
> ARP (Address Resolution Protocol) resolves an IP address to a MAC address within a local network. When PC-A wants to send to PC-B (IP: 192.168.1.20), it needs PC-B's MAC address because switches use MAC addresses for local delivery. PC-A broadcasts "Who has 192.168.1.20?" and PC-B replies with its MAC. PC-A caches this in its ARP table for future use. Without ARP, IP communication on a local network would be impossible.

**Q: What is the difference between IPv4 and IPv6?**
> IPv4 is 32-bit (~4.3 billion addresses), written in dotted decimal. IPv6 is 128-bit (~340 undecillion), written in hexadecimal. IPv6 was created to solve IPv4 address exhaustion. IPv6 also adds: built-in IPSec, no broadcast (uses multicast), fixed 40-byte header (simpler routing), auto-configuration (SLAAC), and no need for NAT.

**Q: What is NAT and why is it used?**
> NAT allows multiple devices on a private network to share a single public IP address. The router maintains a translation table mapping (private IP, private port) to (public IP, public port). Outgoing packets have their source IP replaced with the public IP; incoming responses are translated back. NAT was created to extend the life of IPv4 addresses. Downside: breaks true end-to-end connectivity — external hosts can't initiate connections to private hosts without port forwarding.

---

## 28. Interview Q&A — TCP/UDP & Transport

**Q: Explain the TCP three-way handshake.**
> The client sends **SYN** with its initial sequence number. The server responds with **SYN-ACK** — acknowledging the client's SYN and sending its own sequence number. The client sends **ACK** confirming the server's SYN-ACK. After this, the connection is established and data can flow. Purpose: synchronise sequence numbers for ordered delivery and ensure both sides are ready to communicate. A two-way handshake is insufficient because the server can't confirm its own SYN was received by the client.

**Q: What is the difference between TCP and UDP? When would you use each?**
> TCP is connection-oriented, reliable (guaranteed delivery, in-order, no duplicates), with flow and congestion control. Overhead is higher (20-byte header, connection setup). Use TCP when every byte must arrive correctly: HTTP, HTTPS, FTP, SSH, email. UDP is connectionless, unreliable (no ACKs, no ordering guarantee), with a tiny 8-byte header. Use UDP when speed matters more than reliability: DNS queries (small, quick), VoIP and video calls (lost packet better than late one), online gaming (low latency critical), live streaming.

**Q: What is TCP flow control? What is the sliding window?**
> Flow control prevents the sender from overwhelming the receiver's buffer. The receiver advertises a **window size (rwnd)** in each ACK — the maximum bytes it can accept before its buffer fills. The sender never has more than rwnd bytes unacknowledged at once. As the receiver processes data and the buffer empties, it increases the window size, allowing the sender to transmit more. This creates a "sliding" window that moves forward as ACKs arrive.

**Q: What is TCP congestion control? Explain slow start.**
> Congestion control prevents the sender from overwhelming the network (not just the receiver). TCP uses a **congestion window (cwnd)** limiting unacknowledged data. **Slow start:** begins with cwnd = 1 MSS and doubles it every RTT (exponential growth) until it reaches ssthresh. Then switches to **congestion avoidance** — increases by 1 MSS per RTT (linear). On timeout: ssthresh = cwnd/2, reset cwnd = 1, restart. On 3 duplicate ACKs: ssthresh = cwnd/2, cwnd = ssthresh (fast recovery — less drastic than timeout). AIMD (Additive Increase, Multiplicative Decrease) ensures fairness among competing TCP flows.

**Q: What is a SYN flood attack and how is it prevented?**
> An attacker sends many SYN packets without ever completing the handshake (never sends the final ACK). The server allocates resources for each half-open connection, eventually exhausting its connection table and refusing legitimate connections. Prevention: **SYN cookies** — server doesn't allocate resources until the final ACK arrives (encodes state in the SYN-ACK sequence number); rate limiting SYN packets; firewalls that detect and block abnormal SYN rates.

**Q: What is the difference between a port and a socket?**
> A **port** is a 16-bit number identifying a specific process/service on a host (e.g., port 80 for HTTP). A **socket** is the combination of IP address + port number — it's the complete endpoint of a network communication (e.g., 93.184.216.34:443). A TCP connection is uniquely identified by the 4-tuple (client IP, client port, server IP, server port), which is why a server can handle thousands of simultaneous connections all on port 443.

---

## 29. Interview Q&A — Application Layer & Protocols

**Q: How does DNS work? What is the difference between recursive and iterative resolution?**
> DNS translates domain names to IP addresses using a hierarchical distributed database. When you query google.com: (1) Check local cache; (2) Query your recursive resolver (ISP or 8.8.8.8); (3) Resolver asks root nameserver for .com; (4) Root refers to .com TLD nameserver; (5) TLD refers to Google's authoritative nameserver; (6) Authoritative NS returns the IP; (7) Resolver caches and returns to you. **Recursive resolution:** client asks the resolver once; resolver does all the legwork and returns the final answer. **Iterative resolution:** resolver asks each nameserver in turn and gets a referral to the next one — used between resolver and nameservers. The end user always uses recursive resolution.

**Q: What is the difference between HTTP and HTTPS?**
> HTTP transfers data in plaintext — anyone intercepting the traffic can read it (passwords, credit cards). HTTPS adds TLS encryption between HTTP and TCP. TLS provides: **confidentiality** (data encrypted with AES), **integrity** (HMAC prevents tampering), **authentication** (server certificate verified by trusted CA). HTTPS uses port 443; HTTP uses port 80. All modern websites use HTTPS; browsers mark HTTP sites as "Not Secure".

**Q: What are HTTP status codes? Give examples from each category.**
> 1xx — Informational (100 Continue). 2xx — Success: 200 OK (request succeeded), 201 Created (resource created), 204 No Content (success but no body). 3xx — Redirection: 301 Moved Permanently (update your bookmarks), 302 Found (temporary redirect), 304 Not Modified (use cached version). 4xx — Client Error: 400 Bad Request (malformed syntax), 401 Unauthorised (needs authentication), 403 Forbidden (authenticated but no permission), 404 Not Found, 429 Too Many Requests. 5xx — Server Error: 500 Internal Server Error, 503 Service Unavailable (server overloaded), 504 Gateway Timeout.

**Q: What is the difference between SMTP, POP3, and IMAP?**
> **SMTP** (port 25/587) is for sending email — from client to server, or server to server. **POP3** (port 110/995) downloads email to your device and typically deletes it from the server — email only on one device. **IMAP** (port 143/993) accesses email stored on the server without downloading — email synchronised across all devices. Modern email clients (Gmail, Outlook) use IMAP.

**Q: What is the difference between HTTP/1.1 and HTTP/2?**
> HTTP/1.1 opens one TCP connection per request (or persistent connections with keep-alive) but still has **head-of-line blocking** — request 2 waits for response 1 to complete. HTTP/2 introduced **multiplexing** — multiple requests and responses over a single TCP connection simultaneously, using streams. Also adds header compression (HPACK), server push (proactively send resources), and binary framing (more efficient than text). HTTP/3 goes further — uses QUIC (UDP-based) to eliminate TCP head-of-line blocking entirely.

---

## 30. Interview Q&A — Advanced & Tricky

**Q: How does traceroute work?**
> `traceroute` sends packets with TTL starting at 1, then 2, then 3, incrementing each time. When TTL = 1, the first router decrements it to 0 and drops the packet, sending back an ICMP "Time Exceeded" message revealing its IP and round-trip time. TTL = 2 reveals the second router, and so on until the destination is reached. This maps the full path a packet takes.

**Q: What happens when a packet is sent from one PC to another in a different network?**
> PC-A checks if destination IP is in its own subnet (compare with subnet mask). It's not, so PC-A sends the packet to its **default gateway** (router). PC-A uses ARP to get the gateway's MAC address and sends the frame. The router receives the frame, strips the Layer 2 header, reads the IP header, looks up its routing table for the best path to the destination network, and forwards the packet to the next hop. This continues hop by hop until the packet reaches the destination network's router, which delivers it to the final host using ARP.

**Q: Can two processes on the same machine use the same port?**
> Not on the same protocol at the same time on the same IP. Two processes can't both bind to TCP port 80 on the same IP address simultaneously — the OS will reject the second bind with "Address already in use." However, TCP port 80 and UDP port 80 are independent — one can run on each. Also, with multiple IP addresses on one machine, different IPs can each have a service on the same port number.

**Q: What is the difference between latency and bandwidth?**
> **Bandwidth** is the maximum data transfer rate of a connection — how wide the pipe is (e.g., 1 Gbps). **Latency** is the time it takes for a packet to travel from source to destination — how long the pipe is (e.g., 20ms). High bandwidth but high latency: you can download large files fast eventually, but the initial response feels slow. Low bandwidth but low latency: quick responses but slow for large transfers. For real-time applications (gaming, VoIP) latency matters most; for file transfers bandwidth matters most.

**Q: What is a VPN? How does it work?**
> A **VPN (Virtual Private Network)** creates an encrypted tunnel between your device and a VPN server over the public internet. Your traffic is encrypted before leaving your device, travels through the tunnel to the VPN server, and exits from there. To websites, your IP appears to be the VPN server's IP, not your real IP. Uses: (1) Privacy — ISP can't see your traffic; (2) Security on public WiFi; (3) Access geo-restricted content; (4) Corporate remote access to internal resources. Protocols: OpenVPN, WireGuard, IPSec, L2TP.

**Q: What is the difference between a hub and a switch? Why are hubs obsolete?**
> A **hub** is a Layer 1 device that broadcasts every incoming packet to ALL connected ports — all devices share one collision domain. If two devices transmit simultaneously, a collision occurs. Very inefficient and insecure (any device can sniff all traffic). A **switch** is a Layer 2 device that learns which MAC addresses are on which ports (MAC address table) and forwards frames only to the correct port. Each port is its own collision domain. Switches are far more efficient and secure. Hubs are obsolete — switches are used exclusively today.

**Q: What is ICMP and what is it used for?**
> **ICMP (Internet Control Message Protocol)** is a network-layer protocol used for error reporting and diagnostic purposes — not for user data. It doesn't use ports. Common uses: **ping** sends ICMP Echo Request and receives Echo Reply to test host reachability and measure RTT; **traceroute** uses ICMP Time Exceeded messages to map network paths; routers send ICMP Destination Unreachable when a packet can't be delivered; PMTUD uses ICMP Fragmentation Needed to discover the path MTU.

---

## 31. What Happens When You Type google.com?

> **The most famous interview question in computer networks. Know this end-to-end.**

```
1. URL Parsing
   Browser parses "google.com" → scheme: https, host: google.com, path: /

2. DNS Resolution
   Check: browser cache → OS cache (/etc/hosts) → OS DNS resolver
   → Recursive resolver (8.8.8.8 or ISP)
   → Root NS → .com TLD NS → Google's authoritative NS
   → Returns: 142.250.80.14
   OS caches result with TTL

3. TCP Connection (Layer 4)
   Browser initiates TCP 3-way handshake to 142.250.80.14:443
   SYN → SYN-ACK → ACK

4. TLS Handshake (Layer 6/5)
   ClientHello → ServerHello + Certificate
   Client verifies cert with CA (trusted by OS/browser)
   Key exchange → symmetric session key established
   Encrypted channel now open

5. HTTP Request (Layer 7)
   Browser sends:
     GET / HTTP/2
     Host: www.google.com
     User-Agent: ...
     Accept: text/html,...

6. Routing (Layer 3)
   Packet travels: your PC → home router → ISP → internet backbone
   At each router: longest prefix match in routing table → next hop
   TTL decremented at each hop

7. ARP (at each local segment)
   Before sending to default gateway: ARP request for gateway MAC
   Frame: src_MAC=your_NIC, dst_MAC=router_MAC, src_IP=you, dst_IP=google

8. Google's Server Receives Request
   Reverse proxy / load balancer receives HTTPS request
   TLS decrypted, HTTP request processed
   Google's backend generates HTML

9. HTTP Response
   HTTP/2 200 OK
   Content-Type: text/html
   Body: HTML of google.com homepage

10. Browser Renders Page
    HTML parsed → DOM tree
    CSS downloaded → CSSOM
    JavaScript downloaded and executed
    Render tree built → Layout → Paint
    Page displayed
```

---

## 32. Quick Reference Cheat Sheet

```
OSI LAYERS (top → bottom)
  7 Application  : HTTP, HTTPS, FTP, DNS, SMTP, SSH
  6 Presentation : TLS/SSL, JPEG, ASCII (encryption, encoding)
  5 Session      : NetBIOS, RPC (connection management)
  4 Transport    : TCP, UDP (segments, ports, end-to-end)
  3 Network      : IP, ICMP, OSPF (packets, routing)
  2 Data Link    : Ethernet, ARP (frames, MAC addresses)
  1 Physical     : Cables, WiFi (bits, signals)

TCP/IP LAYERS
  Application  = OSI 7+6+5  |  Transport = OSI 4
  Internet     = OSI 3       |  Network Access = OSI 2+1

DEVICES BY LAYER
  Layer 1: Hub, Repeater, Cable
  Layer 2: Switch, Bridge, NIC
  Layer 3: Router
  Layer 4+: Firewall, Load Balancer, Gateway

IP ADDRESSING
  IPv4: 32-bit | 4.3B addresses | Classes A/B/C/D/E
  IPv6: 128-bit | 340 undecillion | Hexadecimal, no broadcast
  Private: 10.x.x.x | 172.16-31.x.x | 192.168.x.x
  Loopback: 127.0.0.1 | APIPA: 169.254.x.x

CIDR
  /24 = 256 addresses, 254 usable
  /25 = 128 addresses, 126 usable
  /30 = 4 addresses,   2   usable (point-to-point)
  Usable = 2^(host bits) − 2

TCP vs UDP
  TCP: connection-oriented | reliable | ordered | flow+congestion control
  UDP: connectionless | unreliable | fast | 8-byte header

TCP 3-WAY HANDSHAKE: SYN → SYN-ACK → ACK
TCP 4-WAY CLOSE:     FIN → ACK → FIN → ACK

TCP CONGESTION CONTROL
  Slow Start     : cwnd doubles each RTT until ssthresh
  Cong. Avoidance: cwnd += 1 MSS per RTT
  Timeout        : ssthresh = cwnd/2, cwnd = 1, restart
  3 Dup ACKs     : ssthresh = cwnd/2, cwnd = ssthresh (fast recovery)

WELL-KNOWN PORTS
  20/21 FTP | 22 SSH | 23 Telnet | 25 SMTP | 53 DNS
  67/68 DHCP | 80 HTTP | 110 POP3 | 143 IMAP | 443 HTTPS
  3306 MySQL | 5432 Postgres | 6379 Redis | 27017 MongoDB

DNS RECORDS
  A: hostname → IPv4  |  AAAA: hostname → IPv6
  CNAME: alias → name |  MX: mail server
  NS: nameserver      |  PTR: reverse DNS (IP → hostname)
  TXT: SPF/DKIM/verification

HTTP STATUS CODES
  2xx: 200 OK | 201 Created | 204 No Content
  3xx: 301 Moved Permanently | 302 Found | 304 Not Modified
  4xx: 400 Bad Request | 401 Unauthorised | 403 Forbidden | 404 Not Found
  5xx: 500 Internal Error | 503 Service Unavailable | 504 Gateway Timeout

ROUTING PROTOCOLS
  RIP  : Distance Vector | hop count | max 15 hops
  OSPF : Link State | Dijkstra | cost (bandwidth) | enterprise
  BGP  : Path Vector | AS path | internet backbone

ENCRYPTION
  Symmetric  : same key both sides | AES, DES | fast | used for data
  Asymmetric : public/private key pair | RSA, ECC | slow | key exchange
  TLS/HTTPS  : hybrid (asymmetric for key exchange + symmetric for data)

COMMON ATTACKS
  DDoS      : flood with traffic | defend: rate limit, CDN
  SYN Flood : half-open connections | defend: SYN cookies
  MITM      : intercept traffic | defend: TLS, cert pinning
  ARP Poison: fake ARP replies | defend: dynamic ARP inspection
  DNS Spoof : fake DNS answers | defend: DNSSEC
```

---

*Sources: InterviewBit Networking Interview Questions (2026), GeeksforGeeks Computer Networks (2025),
PrepInsta Computer Network Interview Questions (2025), GeeksforGeeks TCP/IP Top 50 (2026),
Tanenbaum "Computer Networks" 5th ed., Kurose & Ross "Computer Networking: A Top-Down Approach" 8th ed.*

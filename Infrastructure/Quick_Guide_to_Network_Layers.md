# 🌐 Quick Guide to Network Layers (OSI Model)

The OSI (Open Systems Interconnection) model divides network communication into 7 layers to standardize interactions between systems.

---

## 📶 OSI Model Overview

| Layer | Name                 | Function                                 | Protocol Examples                |
|-------|----------------------|------------------------------------------|----------------------------------|
| 7     | Application          | End-user services, APIs                  | HTTP, FTP, SMTP, DNS             |
| 6     | Presentation         | Data formatting, encryption, compression | SSL/TLS, JPEG, ASCII             |
| 5     | Session              | Session management (start/stop/resume)   | NetBIOS, RPC, PPTP               |
| 4     | Transport            | Reliable transmission, flow control      | TCP, UDP                         |
| 3     | Network              | Routing, addressing                      | IP, ICMP, ARP                    |
| 2     | Data Link            | Node-to-node transfer, MAC addressing    | Ethernet, PPP, Frame Relay       |
| 1     | Physical             | Bit transmission through medium          | Cables, Fiber, Wi-Fi, Hubs       |

---

## 🔍 Layer Breakdown

### 🧑‍💻 7. Application Layer
Interface between user and network services.

```
Browser ──▶ HTTP ──▶ Server
```

- Email (SMTP), web browsing (HTTP), DNS resolution

---

### 🧰 6. Presentation Layer
Data translation, encryption/decryption, compression.

- Converts from binary to human-readable and vice versa
- Applies TLS for secure sessions

---

### 🧭 5. Session Layer
Establishes, manages, and terminates sessions.

- Useful in stateful protocols (e.g., NetBIOS, SMB)

---

### 📦 4. Transport Layer
Reliable or fast delivery. Adds ports, retransmits lost packets.

- **TCP**: reliable, ordered (web, email)
- **UDP**: faster, no guarantee (video, games)

```
TCP: 3-way handshake → SYN, SYN-ACK, ACK
UDP: Send and hope it arrives
```

---

### 🌍 3. Network Layer
Determines best path for packet delivery.

- IP addressing and routing
- ICMP for error messaging (e.g., ping)

---

### 📡 2. Data Link Layer
Direct node-to-node delivery over a physical link.

- Adds MAC addresses
- Detects and sometimes corrects physical layer errors

```
MAC address: 00:1A:2B:3C:4D:5E
```

---

### ⚙️ 1. Physical Layer
Converts data into electrical/optical signals.

- Medium: Ethernet cable, Wi-Fi, fiber optics
- Defines voltages, pinouts, etc.

---

## 🧠 Mnemonic

> **"Please Do Not Throw Sausage Pizza Away"**  
(Physical, Data Link, Network, Transport, Session, Presentation, Application)

---

## 🔀 TCP/IP vs OSI

| OSI Layer        | TCP/IP Equivalent     |
|------------------|-----------------------|
| Application (7-5)| Application            |
| Transport (4)    | Transport              |
| Network (3)      | Internet               |
| Data/Physical     | Network Access / Link |

---

## ✅ Key Takeaways

- OSI helps **structure communication protocols**.
- Understanding the layers helps with **debugging and optimization**.
- Most practical networking focuses on **Layers 1–4**.

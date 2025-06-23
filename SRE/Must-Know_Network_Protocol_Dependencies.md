# 🧭 Must-Know Network Protocol Dependencies

Understanding protocol dependencies is essential for system design, debugging, performance tuning, and secure development.

This guide covers foundational **network protocols**, how they interact, and what **higher-level services** depend on them.

---

## 🌐 1. IP Layer: The Foundation

### 🔹 IPv4 and IPv6

- **IPv4**: 32-bit addressing, still dominant
- **IPv6**: 128-bit addressing, mandatory for scale (IoT, mobile, etc.)
- Responsible for **packet delivery across networks**

```
IPv4: 192.168.1.1  
IPv6: fe80::1c2a:9fff:fe44:ab2d
```

---

## 🧪 2. Diagnostic and Security Protocols

### 🔹 ICMP / ICMPv6

- Used for diagnostics (e.g. **ping**, **traceroute**)
- ICMPv6 handles **Neighbor Discovery Protocol (NDP)** in IPv6

### 🔐 IPsec

- Protocol suite for **end-to-end encryption at the IP layer**
- Used in **VPNs** and secure tunneling

---

## 🔁 3. Transport Layer Protocols

| Protocol | Type | Use Cases                            |
|----------|------|--------------------------------------|
| TCP      | Stream | Reliable, ordered delivery (HTTP, SSH) |
| UDP      | Datagram | Fast, unreliable (DNS, RTP, NTP)   |
| SCTP     | Stream | Telecom signaling, multi-streaming   |
| DCCP     | Datagram | Multimedia over unreliable networks |

---

## 🌍 4. Application Protocols (TCP-based)

These protocols **run on top of TCP** for reliable, ordered communication.

| Protocol | Port  | Description                       |
|----------|-------|-----------------------------------|
| HTTP     | 80    | Web communication                 |
| HTTPS    | 443   | HTTP over TLS                     |
| SSH      | 22    | Secure remote shell access        |
| SMTP     | 25/587| Email sending                     |
| POP3     | 110   | Email retrieval                   |
| IMAP     | 143   | Email sync                        |
| RDP      | 3389  | Remote desktop                    |
| LDAP     | 389   | Directory services                |
| BGP      | 179   | Routing between ISPs              |

---

## ⚡ 5. Application Protocols (UDP-based)

| Protocol | Port  | Use Case                              |
|----------|--------|----------------------------------------|
| DNS      | 53     | Domain resolution                     |
| DHCP     | 67/68  | IP assignment                         |
| SIP      | 5060   | VoIP signaling                        |
| RTP      | 5004   | Media streaming                       |
| NTP      | 123    | Time synchronization                  |
| TFTP     | 69     | Lightweight file transfer             |

> ⚠️ Some of these protocols **fall back to TCP** when reliability is needed (e.g., DNS over TCP for large responses).

---

## 🔐 6. Encryption Layer: TLS/SSL

### 🔹 TLS (formerly SSL)
- **TLS (Transport Layer Security)** encrypts data over **TCP**
- Used in **HTTPS, IMAPS, SMTPS, FTPS**, etc.

```
// HTTPS = HTTP over TLS
https://example.com
```

### 🔹 LDAP vs LDAPS

- **LDAP** runs on TCP 389 (unencrypted)
- **LDAPS** runs on TCP 636 with TLS

---

## ⚡ 7. Modern Protocols

### 🔹 QUIC (Quick UDP Internet Connections)

- Developed by Google, used in **HTTP/3**
- Combines **TLS + transport reliability** on top of **UDP**
- Benefits:
  - Reduced handshake latency
  - Better performance in mobile networks

```
HTTP/3 = HTTP over QUIC (UDP + TLS)
```

### 🔹 MCP (Model Context Protocol)

- Emerging protocol for **LLM-hosting environments**
- Purpose-built for **context-aware model communication**
- Not yet standardized, but gaining traction in **AI infra stacks**

---

## 🧠 How Protocols Layer Together

Here's how these protocols build on each other:

```text
Application  ──────────────────▶ HTTP, SSH, DNS, SMTP, etc.
Transport    ──────────────────▶ TCP, UDP, QUIC
Network      ──────────────────▶ IPv4, IPv6
Link         ──────────────────▶ Ethernet, Wi-Fi, PPP
```

QUIC and MCP are exceptions:
- **QUIC** handles both transport + encryption
- **MCP** introduces **AI-focused application semantics** on existing protocols

---

## 🧩 Protocol Selection Cheat Sheet

| Requirement              | Recommended Protocol(s)   |
|--------------------------|---------------------------|
| Web API                  | HTTPS (HTTP/1.1 or HTTP/2)|
| Low-latency streaming    | RTP (UDP), QUIC           |
| File transfer            | FTP, SFTP, TFTP           |
| Directory service        | LDAP / LDAPS              |
| Remote shell             | SSH                       |
| Messaging / pub-sub      | MQTT, NATS, WebSockets    |
| VPN                      | IPsec, WireGuard, OpenVPN |
| LLM Integration          | MCP (experimental)        |

---

## 📚 Further Learning

- [IANA Port Numbers](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)
- [QUIC Protocol](https://cloudflare-quic.com/)
- [TLS 1.3 RFC](https://datatracker.ietf.org/doc/html/rfc8446)

---

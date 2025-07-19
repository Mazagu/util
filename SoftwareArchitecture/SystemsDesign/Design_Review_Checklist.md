# ✅ System Design: Design Review Checklist

A **design review checklist** ensures that your system design is comprehensive, robust, and aligned with business goals. It’s a tool for **team alignment**, **quality assurance**, and **future maintainability**.

---

## 📌 When to Use This Checklist

- Before implementing a new service or system
- During architecture reviews or RFCs
- As part of design documentation

---

## 🧠 1. Requirements

- [ ] Are **functional requirements** clear?
- [ ] Are **non-functional requirements** (latency, scale, uptime, etc.) identified?
- [ ] Are SLAs/SLOs defined and realistic?

---

## 📐 2. High-Level Architecture

- [ ] Are major components/services well defined?
- [ ] Are responsibilities of each component clear?
- [ ] Are boundaries and data flow between components documented?

---

## 🗃️ 3. Data Modeling

- [ ] Is the data model aligned with the domain?
- [ ] Are trade-offs (e.g., normalization vs denormalization) addressed?
- [ ] Is schema evolution considered?

---

## 📊 4. Scalability

- [ ] Can the system scale **horizontally** if needed?
- [ ] Are there any **single points of failure**?
- [ ] Are **resource bottlenecks** identified and mitigated?

---

## ♻️ 5. Reliability & Resilience

- [ ] Are failure modes and retries addressed?
- [ ] Is circuit breaking / backoff / fallback in place?
- [ ] Are timeouts configured reasonably?

---

## 🔐 6. Security

- [ ] Is **authentication** handled securely?
- [ ] Is **authorization** enforced consistently?
- [ ] Are secrets and sensitive data managed safely?
- [ ] Is traffic encrypted (TLS)?

---

## 🚦 7. Observability

- [ ] Are **logs**, **metrics**, and **traces** defined and implemented?
- [ ] Are alerts configured for failure scenarios?
- [ ] Can issues be debugged quickly in production?

---

## 🔁 8. Maintainability

- [ ] Is the design modular and testable?
- [ ] Are key decisions **well documented**?
- [ ] Are naming conventions and patterns consistent?
- [ ] Are there areas of **overengineering**?

---

## 🧪 9. Testing Strategy

- [ ] Are unit, integration, and load tests planned?
- [ ] Are edge cases and failure modes covered?
- [ ] Is chaos testing or fault injection considered?

---

## 🚀 10. Deployment & CI/CD

- [ ] Is deployment safe and automated (CI/CD)?
- [ ] Is rollback or canary deployment possible?
- [ ] Is configuration externalized?

---

## 🔄 11. Cost & Operations

- [ ] Is the cost of compute, storage, and traffic considered?
- [ ] Are cloud resources scalable and optimized?
- [ ] Is operational effort sustainable?

---

## 🧭 12. Trade-Offs and Alternatives

- [ ] Are design decisions justified with trade-offs?
- [ ] Are rejected alternatives documented?

---

## ✅ Final Step

- [ ] Did the team review the design together?
- [ ] Are open questions or risks documented?

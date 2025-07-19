# 🧠 System Design: Requirements Clarification (Deep Dive)

Clarifying requirements is the **first and most critical step** in system design. It ensures the system you architect solves the **right problem**, satisfies the **users' needs**, and aligns with **business goals**.

---

## 🎯 Why Requirements Clarification Matters

- Prevents overengineering or building the wrong thing.
- Reduces risk of scope creep and design rewrites.
- Helps identify edge cases and integration points early.
- Provides the foundation for architectural trade-offs.

---

## ✅ Types of Requirements

### 1. Functional Requirements
Define **what** the system should do.

```
- Users can register and log in.
- Admins can view usage analytics.
- System sends a daily summary email.
```

### 2. Non-Functional Requirements (NFRs)
Define **how** the system should behave under constraints.

```
- Availability: 99.99% uptime.
- Latency: 95% of responses < 300ms.
- Scalability: Support 10x growth in traffic.
- Security: Data must be encrypted in transit and at rest.
```

### 3. Domain Constraints
Rules and constraints that apply to the business or use case.

```
- Age must be ≥ 18 for sign-up.
- Payment must be captured within 24 hours of order.
```

---

## 🔍 Key Questions to Ask

### 🧑‍💼 Stakeholder-Oriented
- Who are the users?
- What are their pain points?
- What KPIs define success?

### 🔧 Functional Clarification
- What actions should the system support?
- What are the expected inputs and outputs?
- What’s the expected usage pattern (read-heavy, write-heavy, bursty)?

### 🚦 Constraints and Edge Cases
- What happens when service X is down?
- Is there a peak usage window?
- How should errors be handled?

### 📏 Scale and Load
- How many users are expected?
- What’s the estimated RPS (requests per second)?
- Is traffic regional or global?

---

## 🧱 Requirements Clarification Framework

Use this checklist before starting design:

```
1. Gather functional requirements.
2. Capture non-functional requirements (SLAs, SLIs).
3. Identify key business goals and KPIs.
4. Understand the data lifecycle and sensitivity.
5. Sketch basic user flows and data flows.
6. Identify potential integrations (APIs, databases, 3rd parties).
7. Validate requirements with stakeholders.
```

---

## 🧪 Tools and Techniques

- **User Stories / Use Cases** – Concrete scenarios of user interaction.
- **Event Storming** – For complex domains (DDD-based approach).
- **Service Blueprints** – For identifying back-end interactions.
- **Load Estimation Sheets** – Estimate data volume and request rates.
- **Clarification Doc** – Living document of agreed requirements.

---

## 💡 Pro Tips

- Always ask “Why?” behind every feature to avoid overbuilding.
- Quantify every NFR with real numbers.
- Identify **explicit** and **implicit** assumptions.
- Keep stakeholders involved throughout.
- Revalidate requirements before starting design reviews.

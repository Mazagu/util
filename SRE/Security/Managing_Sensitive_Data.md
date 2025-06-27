# 🛡️ Managing Sensitive Data: Best Practices for Secure Systems Design

**Sensitive data** refers to any information that must be protected from unauthorized access due to its confidential or personal nature. Managing such data is essential to maintain user trust, comply with legal regulations, and prevent security breaches.

---

## 🔍 What is Sensitive Data?

Sensitive data includes:

- **Personally Identifiable Information (PII):** Name, address, email, SSN, passport number
- **Health Information:** Medical records, prescriptions (HIPAA in the US)
- **Financial Information:** Bank accounts, credit cards, transactions
- **Legal and Educational Records**
- **Trade Secrets and Intellectual Property**

### 📜 Compliance Regulations

- **GDPR** (EU)
- **HIPAA** (US, health data)
- **CCPA** (California)
- **PCI-DSS** (payment industry)
- **SOX**, **FERPA**, **LGPD**, etc.

> ❗ Failing to comply can lead to fines, audits, and reputational damage.

---

## 🔐 Encryption & Key Management

All sensitive data **must be encrypted** at rest and in transit.

### 🔹 Encryption in Transit

Use **TLS (HTTPS)** for all data exchange over the network.

```
# Example Nginx config enforcing TLS
server {
  listen 443 ssl;
  ssl_certificate /path/to/cert.pem;
  ssl_certificate_key /path/to/key.pem;
}
```

### 🔹 Encryption at Rest

Encrypt:
- Passwords (with salted hashes: bcrypt, Argon2)
- Files (AES-256)
- Databases (column-level encryption)

### 🔐 Key Management Best Practices

- Use **Key Management Systems (KMS)** (e.g., AWS KMS, HashiCorp Vault)
- Rotate keys regularly
- Split key responsibility: e.g., `applicant`, `manager`, `auditor` roles
- Never hardcode secrets in source code or version control

```
# Key splitting via Shamir’s Secret Sharing (conceptual)
K = K1 + K2 + K3   # All 3 needed to unlock
```

---

## 🧽 Data Desensitization (Anonymization)

This involves **removing or modifying** personal data to prevent re-identification.

### Techniques

- **Masking:** Hide data partially (e.g., `****-****-1234`)
- **Tokenization:** Replace data with tokens that map to real data
- **Generalization:** Reduce precision (e.g., age = 20–30 instead of 24)
- **Pseudonymization:** Replace names with pseudonyms
- **GCM (Galois/Counter Mode):** Separates cipher and key data

### Use Cases

- Sharing datasets externally (research, analytics)
- Internal datasets with access restrictions
- Testing and development environments

---

## 🔒 Minimal Data Permissions

> "Grant the **least privilege necessary**."

### RBAC: Role-Based Access Control

Define roles with specific permissions:
- Admin: Full access
- Analyst: Read-only access to anonymized data
- Developer: Temporary access to test data

```
{
  "roles": {
    "analyst": ["read:reports"],
    "admin": ["read:all", "write:all"]
  },
  "users": {
    "alice": "analyst",
    "bob": "admin"
  }
}
```

### Best Practices

- Enforce RBAC at all layers (app, DB, API)
- Review roles regularly
- Remove access when roles change (offboarding, rotation)

---

## ♻️ Data Lifecycle Management

Data must be **securely handled through its entire lifecycle**:

### 1. **Development Phase**
- Give access only to anonymized data
- Grant temporary credentials
- Log all access

### 2. **Production Phase**
- Revoke unnecessary developer access
- Enable audit logs for access tracking
- Monitor for unusual patterns

### 3. **Retention and Deletion**
- Comply with legal retention policies
- Delete user data upon request (Right to Erasure – GDPR)
- Overwrite or shred data on deletion

---

## 📊 Auditing and Monitoring

- Log access to sensitive fields
- Track changes (writes, deletes)
- Set up alerts on suspicious patterns (e.g., mass export)
- Integrate with SIEM tools like Splunk, ELK, or CloudWatch

---

## 📚 Summary Checklist

| Area                    | Actions to Implement                              |
|-------------------------|---------------------------------------------------|
| Encryption              | TLS in transit, AES at rest, hashed passwords     |
| Key Management          | Use secure KMS, rotate, split key ownership       |
| Data Desensitization    | Apply masking, tokenization, anonymization        |
| Access Control          | RBAC, temporary dev access, log access events     |
| Lifecycle Management    | Secure dev–prod workflows, retention policies     |
| Compliance & Auditing   | GDPR/CCPA policies, audit logs, data subject rights|

---

## 📘 Recommended Resources

- [OWASP Cryptographic Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
- [NIST Data Sanitization Guidelines](https://csrc.nist.gov/publications/detail/sp/800-88/rev-1/final)
- [GDPR Summary for Developers](https://gdpr.eu/developers/)
- [AWS KMS](https://docs.aws.amazon.com/kms/)
- [HashiCorp Vault](https://www.vaultproject.io/)

---

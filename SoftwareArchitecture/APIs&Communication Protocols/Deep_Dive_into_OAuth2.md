# 🔐 Deep Dive into OAuth 2.0

OAuth 2.0 is the **industry-standard protocol** for **authorization**. It allows applications to **securely access resources** on behalf of a user, without sharing their credentials.

---

## 🧠 What is OAuth?

OAuth is a **delegation protocol** that lets users grant third-party applications limited access to their data, **without giving away passwords**.

Example: Allowing a website to access your Google Calendar or GitHub repos without sharing your login credentials.

---

## 🧩 Key Components

| Component       | Description |
|----------------|-------------|
| **Resource Owner** | The user who owns the data |
| **Client**         | The app requesting access |
| **Authorization Server** | Issues access tokens |
| **Resource Server**      | Hosts protected data (API) |

---

## 🔁 OAuth Grant Types (Flows)

### 1. 🔐 Authorization Code (with PKCE)

**Recommended for web/mobile apps.** Uses an intermediate code for higher security.

**Flow:**
1. User logs in and authorizes.
2. App gets **authorization code**.
3. App exchanges the code for an **access token**.

```
// Example exchange request
POST /token
grant_type=authorization_code
code=abc123
redirect_uri=https://app.com/callback
client_id=client123
client_secret=secret456
```

🔒 **PKCE** (Proof Key for Code Exchange) adds extra protection for public clients like mobile apps.

---

### 2. 🤖 Client Credentials

**Used for machine-to-machine communication.** No user involved.

```
POST /token
grant_type=client_credentials
client_id=abc
client_secret=xyz
```

---

### 3. 🧾 Resource Owner Password Credentials *(Deprecated)*

User provides username/password directly. Not recommended due to security risks.

---

### 4. 🔄 Refresh Token

Used to **renew access tokens** without re-prompting the user.

```
POST /token
grant_type=refresh_token
refresh_token=abc123
client_id=xyz
```

---

## 🧪 OAuth Tokens

| Token Type     | Description |
|----------------|-------------|
| **Access Token** | Used to access APIs |
| **Refresh Token** | Used to get a new access token |
| **ID Token** *(OIDC)* | Contains user identity info (JWT) |

Tokens can be **opaque** or **JWT-based**.

---

## 🛡️ Security Best Practices

✅ Always use **PKCE** for public clients (SPA, mobile)

✅ Use **HTTPS** everywhere

✅ Store tokens securely (never in localStorage)

✅ Set **short-lived access tokens** + use refresh tokens

✅ Implement **scopes** to limit access

✅ Revoke tokens if suspicious behavior is detected

---

## 🆚 OAuth vs OpenID Connect (OIDC)

- **OAuth** = Delegation (Authorization)
- **OIDC** = Identity Layer on top of OAuth (Authentication)

OIDC adds:
- **ID token** (who the user is)
- **UserInfo endpoint**
- Standard claims (email, name, etc.)

---

## 🚀 Popular Providers & Tools

| Provider       | Docs |
|----------------|------|
| Google         | https://developers.google.com/identity |
| GitHub         | https://docs.github.com/en/developers/apps |
| Auth0          | https://auth0.com/docs |
| Okta           | https://developer.okta.com |
| Keycloak       | https://www.keycloak.org |

Tools:
- Postman OAuth2 support
- OpenID Connect Playground
- OAuth 2.0 Token Introspection tools

---

## 📚 Further Reading

- [RFC 6749 - OAuth 2.0 Spec](https://datatracker.ietf.org/doc/html/rfc6749)
- [OAuth 2.0 Threat Model](https://tools.ietf.org/html/rfc6819)
- [OAuth Best Practices](https://oauth.net/2/)

---

## ✅ Summary

| Topic               | Notes |
|--------------------|-------|
| Use Authorization Code Flow | For apps with user interaction |
| Use PKCE            | For security in SPAs/mobile apps |
| Use Client Credentials | For service-to-service auth |
| Use Refresh Tokens  | For long sessions |
| Never expose secrets | In public clients or repos |
| Prefer OpenID Connect | When identity is needed |

---

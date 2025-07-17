# 🔐 Deep Dive into Authentication

## 🧭 What Is Authentication?

Authentication is the process of verifying **who a user is** before granting access to a system.  
It answers: _“Is this user who they claim to be?”_  
(Authorization comes **after** authentication.)

---

## 🧁 Cookie-Based Authentication

### How It Works:
1. User logs in → credentials are verified.
2. Server creates a session and stores it in memory/store.
3. A session ID is returned to the client as a **cookie**.
4. Browser sends cookie with each request.
5. Server retrieves session and authenticates the user.

### ✅ Pros:
- Simple and secure when using HTTPS.
- Built-in browser support.

### ⚠️ Cons:
- Requires session storage on the server.
- CSRF protection needed.
- Harder to scale in distributed systems.

```
# Laravel session-based login (simplified)
Auth::attempt([
  'email' => $request->email,
  'password' => $request->password,
]);
```

---

## 💠 Token-Based Authentication (Stateless)

### JSON Web Tokens (JWT)

JWTs are self-contained tokens that carry user info and are signed.

### How It Works:
1. User logs in → server returns a **JWT**.
2. Client stores it (e.g. localStorage or HTTP-only cookie).
3. Token is sent in every request (usually `Authorization` header).
4. Server **verifies the signature** and grants access.

### JWT Structure:
A JWT consists of three parts separated by dots:

```
<HEADER>.<PAYLOAD>.<SIGNATURE>
```

Each part is Base64URL-encoded:
- **Header**: algorithm & token type
- **Payload**: user claims (e.g. user ID, expiration)
- **Signature**: used to verify integrity

```
// Example decoded payload
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```

---

### ✅ Pros:
- Stateless and scalable
- Cross-domain capable
- Works well for APIs and SPAs

### ⚠️ Cons:
- Cannot revoke easily unless using a token blacklist
- Susceptible to **XSS** if stored in `localStorage` instead of secure cookies

---

### JWT Examples:

```
// Java JWT verification (using jjwt)
Claims claims = Jwts.parser()
  .setSigningKey(secretKey)
  .parseClaimsJws(token)
  .getBody();
```

```
// Laravel - creating a JWT (with tymondesigns/jwt-auth)
$token = Auth::attempt(['email' => $email, 'password' => $password]);
// Send token back to client
```

---

## 🔐 PASETO (Platform-Agnostic SEcurity TOkens)

A modern alternative to JWT with safer cryptographic defaults.

### Features:
- No "none" algorithm confusion
- Simpler and safer cryptographic design
- 2 token formats: local (symmetric) & public (asymmetric)

### PASETO Example:

```
// Node.js with paseto package
const { V2 } = require('paseto');
const token = await V2.encrypt({ userId: 1 }, secretKey);
```

### ✅ Pros:
- Secure defaults, no algorithm confusion
- Easier to audit

### ⚠️ Cons:
- Not as widely adopted yet
- Limited library support in some languages

---

## 🧾 Other Auth Methods

### 🔐 OAuth2 + OpenID Connect
- For third-party login (Google, GitHub, etc.)
- Common for mobile/web apps needing federated identity

### 📄 SAML
- XML-based auth, mainly used in enterprise SSO
- Heavyweight and harder to debug than modern alternatives

### 🔑 API Keys
- Simple and easy to manage
- No user identity, good for service-to-service, but **less secure** if not scoped and rate-limited

---

## 🗂️ Session vs JWT Comparison

| Feature             | Sessions           | JWT                   |
|---------------------|--------------------|------------------------|
| Storage             | Server-side        | Client-side            |
| Stateless           | ❌                 | ✅                     |
| Revocable           | ✅                 | ❌ (unless tracked)    |
| CSRF Protection     | ✅                 | Optional               |
| Susceptible to XSS  | ✅ if cookie not secure | ✅ if stored in localStorage |

---

## 🧠 Choosing the Right Auth Method

| Use Case                      | Recommended Auth                |
|------------------------------|---------------------------------|
| Web App (monolith)           | Cookies + Session               |
| SPA / Public API             | JWT or OAuth2                   |
| Microservices                | JWT or PASETO (internal tokens) |
| Enterprise / SSO             | SAML or OpenID Connect          |
| Backend-for-Frontend (BFF)   | JWT or Session in secure cookie |

---

## 🔐 Best Practices

- Always use **HTTPS**
- Store JWTs in **HTTP-only cookies** if possible
- Keep token lifetime short and rotate refresh tokens
- Avoid placing sensitive info in payloads
- Use **secure, audited libraries** (e.g., `paseto`, `jjwt`, etc.)
- Avoid black-box implementations of crypto logic
- Prefer PASETO if you're concerned with JWT complexity

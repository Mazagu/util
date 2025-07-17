# 🔐 JWT (JSON Web Token) Quick Guide

## 📌 What is JWT?
JWT is an open standard (RFC 7519) used for securely transmitting information between parties as a JSON object. It is commonly used for **authentication and authorization**.

---

## 🧱 Structure of a JWT

A JWT is composed of **three parts** separated by dots:

```
xxxxx.yyyyy.zzzzz
```

1. **Header** – contains metadata about the token (e.g., algorithm used)
2. **Payload** – contains claims (user info, permissions, etc.)
3. **Signature** – used to verify the token was not tampered with

### Example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9. 
eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJhZG1pbiJ9. 
JvVxT0WRxsf_8zdhGcG82Kg9rDUNsMWLU91vPiAqZpY
```

---

## 🔐 Common JWT Signing Algorithms

- `HS256`: HMAC + SHA-256 (symmetric key)
- `RS256`: RSA Signature with SHA-256 (asymmetric keys)

---

## 🛂 Use Cases

- Authentication (e.g., login sessions)
- Authorization (e.g., access control for APIs)
- Secure information exchange

---

## ✅ How JWT Authentication Works

1. User logs in and is authenticated by the server.
2. Server generates a JWT and returns it.
3. Client stores JWT (usually in localStorage or cookies).
4. Client sends JWT in **Authorization header** on each request:
   ```
   Authorization: Bearer <token>
   ```
5. Server validates the JWT signature and payload.

---

## 📥 Sample Payload

```
{
  "sub": "1234567890",
  "name": "John Doe",
  "role": "admin",
  "iat": 1617171723,
  "exp": 1617175323
}
```

- `sub`: Subject (user ID)
- `iat`: Issued At timestamp
- `exp`: Expiration timestamp

---

## ⚠️ Best Practices

- Always use HTTPS to transmit JWTs
- Keep expiration times short
- Use `exp` (expiration) and `iat` (issued at) claims
- Store sensitive tokens in **HttpOnly** cookies if possible
- Don’t store too much data in the payload

---

## 🧰 Popular Libraries

- **Node.js**: `jsonwebtoken`
- **Python**: `PyJWT`
- **Java**: `jjwt`, `nimbus-jose-jwt`
- **PHP**: `firebase/php-jwt`
- **Laravel**: `tymon/jwt-auth`

---

# 📎 Appendix: JWT Implementation Examples

---

## 🐘 Laravel with `tymon/jwt-auth`

### 1. Install the Package

```
composer require tymon/jwt-auth
```

### 2. Publish the Config

```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

### 3. Generate JWT Secret

```
php artisan jwt:secret
```

### 4. Add the `JWTAuth` Guard in `config/auth.php`

```
'guards' => [
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
],
```

### 5. Example: Auth Controller

```
use Illuminate\Http\Request;
use Tymon\JWTAuth\Facades\JWTAuth;

public function login(Request $request) {
    $credentials = $request->only('email', 'password');

    if (!$token = JWTAuth::attempt($credentials)) {
        return response()->json(['error' => 'Invalid Credentials'], 401);
    }

    return response()->json(compact('token'));
}
```

### 6. Middleware to Protect Routes

```
Route::middleware(['auth:api'])->get('/user', function (Request $request) {
    return $request->user();
});
```

---

## ☕ Java with `jjwt`

### 1. Add Maven Dependency

```
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-api</artifactId>
  <version>0.11.5</version>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-impl</artifactId>
  <version>0.11.5</version>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>io.jsonwebtoken</groupId>
  <artifactId>jjwt-jackson</artifactId>
  <version>0.11.5</version>
  <scope>runtime</scope>
</dependency>
```

### 2. Create JWT Token

```
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import java.util.Date;

String secretKey = "mySecretKey";
String token = Jwts.builder()
    .setSubject("user123")
    .setIssuedAt(new Date())
    .setExpiration(new Date(System.currentTimeMillis() + 3600000)) // 1 hour
    .signWith(SignatureAlgorithm.HS256, secretKey)
    .compact();
```

### 3. Parse JWT Token

```
Claims claims = Jwts.parser()
    .setSigningKey(secretKey)
    .parseClaimsJws(token)
    .getBody();

String userId = claims.getSubject();
```

---

# Authentication and Authorization

## Authentication

Authentication answers:

```text
Who are you?
```

Examples:

- username/password,
- OTP,
- MFA,
- biometric authentication,
- security key,
- OAuth login.

---

## Authorization

Authorization answers:

```text
What are you allowed to do?
```

Example:

```text
Authenticated User A
should not automatically access
User B's private data
```

---

## Authentication vs Authorization

```text
Authentication → identity
Authorization  → permissions
```

A user can be correctly authenticated while authorization is still broken.

This is how IDOR/BOLA vulnerabilities commonly happen.

---

## OTP Security

Typical flow:

```text
Registration/Login
      ↓
Generate OTP
      ↓
Send OTP
      ↓
User submits OTP
      ↓
Server verifies OTP
      ↓
Only then create/activate account
```

A broken implementation might:

```text
Invalid OTP
   ↓
Application shows error
   ↓
BUT backend still creates account
```

This is an authentication/business-logic flaw.

---

## MFA/OTP Testing Questions

During authorized testing, check:

- Is verification actually enforced server-side?
- Can the protected action continue after an invalid OTP?
- Does the OTP expire?
- Can the OTP be reused?
- Are attempts rate-limited?
- Is OTP verification tied to the correct user/session?
- Can changing the request skip a verification step?

---

## Password Storage

Passwords should generally be stored using a strong password hashing algorithm designed for passwords, such as:

- Argon2,
- bcrypt,
- scrypt,
- PBKDF2.

General-purpose hashes like MD5 are not appropriate for modern password storage because they are extremely fast and unsuitable against password-cracking attacks.

---

## Common Authentication Problems

- weak passwords,
- no rate limiting,
- insecure password reset,
- predictable recovery tokens,
- OTP bypass,
- missing MFA enforcement,
- session fixation,
- user enumeration,
- credentials exposed in logs/cookies/URLs.

---

## Authorization Testing

Ask:

```text
Can User A access User B's object?
Can a normal user perform admin actions?
Does the server verify permissions on every request?
Does hiding a button merely hide functionality in the UI?
```

Security must be enforced on the server, not only in the frontend.

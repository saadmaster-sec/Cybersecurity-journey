# Cookies and Sessions

## What is a Cookie?

A cookie is a small piece of data stored by the browser.

The server can create one using:

```http
Set-Cookie: session=abc123
```

The browser later sends it back:

```http
Cookie: session=abc123
```

Cookies are often used for:

- session IDs,
- authentication,
- preferences,
- shopping carts,
- tracking.

---

## What is a Session?

HTTP is stateless.

That means each request is separate unless the application has a way to remember the user.

A common session flow:

```text
User logs in
   ↓
Server validates credentials
   ↓
Server creates session
   ↓
Browser receives session cookie
   ↓
Browser sends cookie on future requests
   ↓
Server identifies logged-in user
```

---

## Session Cookie Example

```http
Set-Cookie: PHPSESSID=abc123; Secure; HttpOnly; SameSite=Lax
```

### Important Attributes

#### Secure

```text
Secure
```

Cookie is sent over HTTPS.

#### HttpOnly

```text
HttpOnly
```

Prevents normal JavaScript from reading the cookie.

This can reduce session-cookie theft during some XSS attacks.

#### SameSite

Controls whether cookies are sent with cross-site requests.

Possible values:

```text
Strict
Lax
None
```

Useful as one layer of CSRF protection.

---

## Weak Cookie Example

```http
Set-Cookie: session=abc123
```

Potential missing protections:

```text
Secure
HttpOnly
SameSite
```

---

## Session Security Problems

Common issues include:

- predictable session IDs,
- session IDs exposed in URLs,
- sessions not invalidated after logout,
- session fixation,
- very long session lifetime,
- insecure persistent cookies,
- sensitive data stored directly inside cookies.

Sensitive data such as raw passwords or password hashes generally should not be stored client-side in cookies.

---

## Logout Testing

After logging out:

1. Re-send an authenticated request.
2. Check whether the old session cookie still works.
3. Verify that the server invalidated the session.

Expected:

```text
Old session → rejected
```

Not just:

```text
Browser cookie deleted
```

The server-side session should also become invalid where appropriate.

---

## SOC Relevance

Session abuse may appear in:

- authentication logs,
- web proxy logs,
- WAF logs,
- unusual IP changes,
- impossible travel,
- repeated token reuse,
- abnormal session lifetimes.

# HTTP Headers

## What Are Headers?

HTTP headers are metadata sent with HTTP requests and responses.

Think of them as instructions or labels attached to the communication.

```text
HTTP Message
├── Start Line
├── Headers
└── Body
```

---

## Request Headers

Sent:

```text
Client → Server
```

Example:

```http
GET /account HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: PHPSESSID=abc123
```

Common request headers:

| Header | Purpose |
|---|---|
| Host | Requested website |
| User-Agent | Client/browser information |
| Accept | Accepted response format |
| Cookie | Sends stored cookies |
| Authorization | Sends authentication data |
| Origin | Origin that initiated the request |
| Referer | Previous page information |
| Content-Type | Format of request body |

---

## Response Headers

Sent:

```text
Server → Client
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: PHPSESSID=abc123; Secure; HttpOnly
Cache-Control: no-store
```

Common response headers:

| Header | Purpose |
|---|---|
| Content-Type | Returned content type |
| Set-Cookie | Creates/updates cookies |
| Location | Redirect target |
| Cache-Control | Caching rules |
| Server | Server information |
| Content-Length | Response size |

---

## Why Security Headers Matter

Security headers instruct the browser to apply extra protection.

Examples:

```http
Content-Security-Policy: default-src 'self'
Strict-Transport-Security: max-age=31536000
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
```

Missing security headers usually do not create an exploit by themselves.

Instead:

```text
Actual vulnerability
        +
Missing browser protection
        ↓
Potentially greater impact
```

Example:

```text
XSS + weak/no CSP → injected script may face fewer restrictions
```

---

## Important Security Headers

### Content-Security-Policy

Controls what resources/scripts the browser may load.

```http
Content-Security-Policy: default-src 'self'
```

Helps reduce the impact of some XSS attacks.

### Strict-Transport-Security

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

Tells browsers to use HTTPS.

### X-Content-Type-Options

```http
X-Content-Type-Options: nosniff
```

Prevents MIME-type sniffing.

### Frame Protection

Legacy:

```http
X-Frame-Options: DENY
```

Modern CSP:

```http
Content-Security-Policy: frame-ancestors 'none'
```

Helps prevent clickjacking.

### Referrer-Policy

```http
Referrer-Policy: strict-origin-when-cross-origin
```

Controls how much referrer information is shared.

### Permissions-Policy

```http
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

Restricts access to browser capabilities.

---

## How to Check Headers

### Browser

```text
F12
→ Network
→ Reload
→ Select request
→ Headers
```

### curl

```bash
curl -I https://example.com
```

Verbose request/response information:

```bash
curl -v https://example.com
```

### Burp Suite

Inspect both:

```text
Request
Response
```

Pay attention to:

- cookies,
- authorization tokens,
- CORS headers,
- redirects,
- security headers,
- caching,
- server information.

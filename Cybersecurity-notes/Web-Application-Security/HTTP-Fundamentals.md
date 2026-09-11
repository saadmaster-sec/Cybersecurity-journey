# HTTP Fundamentals

## What is HTTP?

HTTP stands for **Hypertext Transfer Protocol**.

It is the communication protocol used by browsers, APIs, and web servers.

Basic flow:

```text
Client → HTTP Request → Server
Client ← HTTP Response ← Server
```

Example request:

```http
GET /profile HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

Example response:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<html>...</html>
```

---

## Request Components

A request usually contains:

1. HTTP method
2. Path
3. Headers
4. Optional body

Example:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=saad&password=test123
```

---

## Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve data |
| POST | Submit/create data |
| PUT | Replace/update a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |
| OPTIONS | Ask what communication options are supported |
| HEAD | Get headers without the response body |

Methods do not enforce security by themselves.

A `POST` request is not automatically safer than a `GET` request.

---

## Status Codes

### 2xx - Success

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |

### 3xx - Redirect

| Code | Meaning |
|---|---|
| 301 | Permanent Redirect |
| 302 | Temporary Redirect |
| 307 | Temporary Redirect |
| 308 | Permanent Redirect |

### 4xx - Client-side error

| Code | Meaning |
|---|---|
| 400 | Bad Request |
| 401 | Authentication Required |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 429 | Too Many Requests |

### 5xx - Server-side error

| Code | Meaning |
|---|---|
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

---

## HTTP vs HTTPS

HTTP sends traffic without TLS encryption.

HTTPS is:

```text
HTTP + TLS
```

HTTPS provides:

- confidentiality,
- integrity,
- server authentication.

HTTPS does **not** automatically make the application secure.

A site can use HTTPS and still have:

- XSS,
- SQL injection,
- broken access control,
- weak authentication,
- insecure business logic.

---

## URL Anatomy

Example:

```text
https://example.com:443/products?id=25#reviews
```

Breakdown:

```text
https       → scheme
example.com → hostname
443         → port
/products   → path
id=25       → query parameter
#reviews    → fragment
```

Query parameters are common testing points because they contain user-controlled input.

---

## Why HTTP Matters in VAPT

Most manual web testing starts by understanding the request.

Ask:

```text
What does the browser send?
What can I control?
What does the server trust?
What does the server return?
Does behavior change if I modify the request?
```

Burp Suite makes this easier because it allows requests and responses to be inspected directly.

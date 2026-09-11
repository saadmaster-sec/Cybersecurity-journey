# Cross-Site Scripting (XSS)

## What is XSS?

XSS occurs when attacker-controlled input is returned to a browser and interpreted as executable JavaScript/HTML instead of harmless text.

Concept:

```text
User Input
   ↓
Application
   ↓
Unsafe Output
   ↓
Browser interprets it as code
```

---

## Main Types

### Reflected XSS

Payload is sent in a request and immediately reflected in the response.

Example concept:

```text
/search?q=<input>
```

The response includes the input unsafely.

### Stored XSS

Input is stored by the application and later shown to users.

Possible locations:

- comments,
- profile fields,
- product reviews,
- support tickets.

### DOM-Based XSS

The vulnerability occurs mainly in frontend JavaScript when unsafe client-side code writes attacker-controlled data into dangerous DOM locations.

---

## Harmless Test Example

In an authorized lab:

```html
<script>alert(1)</script>
```

The point is not the alert itself.

The alert demonstrates:

```text
JavaScript execution is possible
```

---

## Impact

Depending on context, XSS can enable:

- actions as the victim,
- modification of page content,
- phishing inside the trusted site,
- reading data accessible to JavaScript,
- session compromise when protections are weak.

---

## Why CSP Matters

CSP is defense-in-depth.

```text
XSS vulnerability + no strong CSP
→ potentially easier script execution
```

But:

```text
Missing CSP ≠ XSS vulnerability
```

---

## Prevention

Use:

- contextual output encoding,
- safe templating,
- input validation where appropriate,
- safe DOM APIs,
- CSP as additional protection.

Avoid inserting untrusted data directly into dangerous contexts.

---

## Reporting

Include:

```text
Affected parameter
Affected endpoint
Payload used
Observed behavior
Impact
Screenshot/request-response evidence
Recommended remediation
```

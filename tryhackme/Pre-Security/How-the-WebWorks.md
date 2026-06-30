# How The Web Works — TryHackMe Notes

Module: Pre-Security Path → How The Web Works

Rooms covered: DNS in Detail · HTTP in Detail · How Websites Work · Putting It All Together

## 1. DNS in Detail

### What DNS does

Every device on the internet has a unique IP address (e.g. 104.26.10.229), but remembering strings of numbers for every site you visit isn't practical. The Domain Name System (DNS) maps human-readable domain names (google.com) to machine-readable IP addresses, the same way a postal address lets you find a house without knowing its GPS coordinates.

### Domain name anatomy

|Part | Example | NotesTLD|
|---|---|---|
|(Top Level Domain) |.com in tryhackme.com | Right-most part of the domain. Split into gTLD (generic, e.g. .com, .org) and ccTLD (country code, e.g. .uk, .de).| 
|SLD (Second Level Domain)| tryhackme in tryhackme.com | Max 63 characters, a-z, 0-9, hyphens (can't start/end with a hyphen or have consecutive hyphens).| 
|Subdomainadmin | in admin.tryhackme.com | Sits left of the SLD. Same character rules as SLD. Multiple subdomains can be chained (jupiter.servers.tryhackme.com), total length capped at 253 characters.|
```mermaid
flowchart LR
    A["admin . tryhackme . com"] --> B["Subdomain: admin"]
    A --> C["SLD: tryhackme"]
    A --> D["TLD: .com"]
    D --> E["gTLD — generic, e.g. .com / .org"]
    D --> F["ccTLD — country code, e.g. .uk / .de"]
```
How a DNS lookup works (step by step)

Your computer checks its local DNS cache first — if you've visited the site recently, it may already know the IP.
If not cached, the query goes to a Recursive DNS Server (often run by your ISP), which checks its own cache.
If still unknown, the recursive server queries a Root DNS Server — the "backbone" of the internet, which points to the correct TLD server.
The TLD server points to the Authoritative DNS Server for that specific domain.
The authoritative server (the actual source of truth for that domain's records) returns the IP. The result is cached locally for future use, governed by the record's TTL (Time To Live) — the number of seconds the response should be stored before it's looked up again.

mermaidsequenceDiagram
    participant U as Your Computer
    participant R as Recursive Resolver
    participant Root as Root Server
    participant TLD as TLD Server (.com)
    participant Auth as Authoritative Server

    U->>U: Check local DNS cache
    Note over U: Found? Return IP immediately

    U->>R: Query: what is the IP for tryhackme.com?
    R->>R: Check resolver cache
    Note over R: Found? Return IP

    R->>Root: Who handles .com?
    Root-->>R: Ask the .com TLD server

    R->>TLD: Who is authoritative for tryhackme.com?
    TLD-->>R: Ask kip.ns.cloudflare.com

    R->>Auth: What is the A record for tryhackme.com?
    Auth-->>R: 104.26.10.229 (with TTL)

    R-->>U: 104.26.10.229
    Note over U,R: Result cached locally until TTL expires
Common DNS record types
RecordResolves toExample useAIPv4 addresswebsite.thm → 10.10.10.10AAAAIPv6 addresse.g. 2606:4700::6810:85e5CNAMEAnother domain name (alias)shop.website.thm → shops.myshopify.com (another lookup then resolves that to an IP)MXMail server address, with a priority valueUsed to route email; multiple MX records act as backupsTXTFree-text fieldSPF records, domain verification, anti-spam/anti-spoofingNSAuthoritative nameservers for the domaine.g. kip.ns.cloudflare.com
Useful command: nslookup
bashnslookup website.thm                    # default A record lookup
nslookup -type=A website.thm
nslookup -type=CNAME shop.website.thm
nslookup -type=TXT website.thm
nslookup -type=MX website.thm

2. HTTP in Detail
HTTP vs HTTPS
HyperText Transfer Protocol (HTTP) is the application-layer protocol browsers use to request and receive web content. HTTPS is the same protocol with an added layer of encryption (TLS/SSL) so data can't be read or tampered with in transit — same protocol, secure transport.
mermaidsequenceDiagram
    participant B as Browser (Client)
    participant S as Web Server

    B->>S: GET / HTTP/1.1<br/>Host: tryhackme.com<br/>User-Agent: Firefox/87.0
    Note over B,S: Blank line marks end of request
    S-->>B: HTTP/1.1 200 OK<br/>Content-Type: text/html<br/>Content-Length: 98
    Note over B,S: Blank line, then response body
    S-->>B: HTML page content
    B->>B: Parse & render page
Anatomy of a URL
scheme://user:password@host:port/path?query#fragment

Scheme — protocol to use (http, https, ftp)
User — optional credentials for services requiring login via URL
Host — domain name or IP of the server
Port — defaults to 80 (HTTP) / 443 (HTTPS) if omitted
Path / query / fragment — locates the specific resource and parameters

mermaidflowchart LR
    U["Full URL"] --> S["Scheme: https"]
    U --> C["Credentials: user:pass"]
    U --> H["Host: tryhackme.com"]
    U --> P["Port: 443"]
    U --> PA["Path: /room/dnsindetail"]
    U --> Q["Query: id=1"]
    U --> F["Fragment: task2"]
Example: https://user:pass@tryhackme.com:443/room/dnsindetail?id=1#task2
Anatomy of an HTTP request
GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com

Method, requested resource, and HTTP version
Host header — which site to serve (needed since one server can host multiple domains)
User-Agent — identifies the requesting browser/client
Referer — the page that linked here
A blank line marks the end of the request

Anatomy of an HTTP response
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>...</html>

HTTP version + status code
Server software/version
Date/time of the response
Content-Type — tells the browser how to interpret the body (HTML, JSON, image, etc.)
Content-Length — size of the response body, used to confirm nothing was cut off
Blank line marks end of headers
Response body — the actual requested content

HTTP methods
MethodPurposeGETRetrieve information from the serverPOSTSubmit data to the server, often creating a new recordPUTSubmit data to update an existing recordDELETERemove a record
HTTP status code ranges
RangeMeaning100–199Informational — request received, client should continue200–299Success300–399Redirection — resource has moved400–499Client error — something wrong with the request500–599Server error — server failed to fulfil a valid request
mermaidflowchart TD
    R[HTTP Response] --> A["1xx — Informational<br/>request received, continue"]
    R --> B["2xx — Success<br/>200 OK, 201 Created"]
    R --> C["3xx — Redirection<br/>301/302 resource moved"]
    R --> D["4xx — Client Error<br/>400, 401, 403, 404, 405"]
    R --> E["5xx — Server Error<br/>500, 503"]

    style B fill:#2e7d32,color:#fff
    style C fill:#f9a825,color:#000
    style D fill:#c62828,color:#fff
    style E fill:#6a1b9a,color:#fff
Common codes to know:

200 OK — success
201 Created — new resource created (e.g. new user, new post)
301/302 — redirect
400 Bad Request
401 Unauthorized — not authenticated
403 Forbidden — authenticated but not allowed
404 Not Found
405 Method Not Allowed — wrong method used for that resource
500 Internal Server Error — server-side crash/unhandled error (e.g. database unreachable)
503 Service Unavailable — overloaded or down for maintenance

Common request headers

Host — target domain on a multi-site server
User-Agent — client/browser info
Accept-Encoding — compression methods the browser supports
Cookie — data sent back to the server to maintain state (sessions, auth tokens)

Common response headers

Set-Cookie — tells the browser to store a cookie and resend it on future requests
Cache-Control — how long the browser should cache the response
Content-Type — type of data returned (HTML, JSON, image, etc.)

Cookies
Cookies are most commonly used for authentication/session management. The value is usually a random token rather than a plaintext password, but if a cookie isn't validated properly server-side, it can be tampered with (e.g. changing name=adam to name=admin) to escalate privileges — a key concept for later web security learning.

3. How Websites Work
Client vs Server

Front End (Client-Side) — what your browser renders and what the user interacts with directly.
Back End (Server-Side) — the server that processes the request, runs application logic, talks to the database, and sends back a response.

mermaidflowchart LR
    subgraph Client["Front End — Client Side"]
        direction TB
        HTML[HTML<br/>structure]
        CSS[CSS<br/>styling]
        JS[JavaScript<br/>interactivity / DOM]
    end

    subgraph Server["Back End — Server Side"]
        direction TB
        App[Application logic]
        DB[(Database)]
    end

    Client <-->|HTTP request / response| Server
    App --> DB
The three core front-end technologies
TechnologyRoleHTMLMarkup language — defines structure and content of the pageCSSStyling — colors, layout, typographyJavaScriptProgramming language — adds interactivity and dynamic behavior, manipulates the DOM (Document Object Model)
Basic HTML structure
html<!DOCTYPE html>
<html>
  <head>
    <title>Page Title</title>
  </head>
  <body>
    <h1>Welcome</h1>
  </body>
</html>

<!DOCTYPE html> — declares the document as HTML5
<html> — root element, everything else nests inside it
<head> — page metadata (title, linked stylesheets/scripts)
<body> — the actual visible content

mermaidflowchart TD
    DT["&lt;!DOCTYPE html&gt; — declares HTML5"] --> H["&lt;html&gt; — root element"]
    H --> HE["&lt;head&gt; — metadata"]
    H --> BO["&lt;body&gt; — visible content"]
    HE --> T["&lt;title&gt;"]
    HE --> L["&lt;link&gt; stylesheets"]
    HE --> SC["&lt;script&gt; references"]
    BO --> H1["&lt;h1&gt;, &lt;p&gt;, &lt;img&gt;, &lt;div&gt; ..."]
Static vs dynamic content

Static content — never changes; served as-is (images, CSS, fixed HTML/JS files).
Dynamic content — generated/changed per request (e.g. a blog homepage showing the latest post, search results) — handled by backend logic.

Backend & databases
The backend processes requests and often needs to store/retrieve data — this is where databases come in (MySQL, MSSQL, MongoDB, PostgreSQL, etc.). Each has different strengths (structure, scalability, query style).
Supporting infrastructure

CDN (Content Delivery Network) — hosts static assets (JS, CSS, images, video) across many geographically distributed servers, so users are served from the nearest location, reducing load and latency.
WAF (Web Application Firewall) — sits between the client and the web server to filter malicious traffic and protect against attacks like DoS.

mermaidflowchart LR
    User([User]) --> WAF["WAF<br/>filters malicious traffic"]
    WAF --> CDN{Requesting<br/>static asset?}
    CDN -->|Yes, nearest edge| Edge[CDN Edge Server]
    CDN -->|No, dynamic content| Origin[Origin Web Server]
    Origin --> DB[(Database)]
Security concepts introduced

Sensitive Data Exposure — sensitive plaintext info (credentials, internal links, API keys) accidentally left visible in front-end source code, viewable via "View Page Source."
HTML Injection — when user input is reflected on the page without sanitization, an attacker can submit their own HTML/JS, which the browser then renders/executes. Mitigation: always sanitize user input before using it in front-end or back-end logic. This is conceptually related to later topics like SQL injection on the backend.


4. Putting It All Together
This room ties DNS, HTTP, and website architecture into a single end-to-end flow of what happens when you visit a website:

DNS resolution — your browser needs the IP address of the server hosting the site, resolved via the DNS lookup chain (cache → recursive resolver → root → TLD → authoritative server).
HTTP(S) request — your browser opens a connection to that IP and sends an HTTP(S) request for the page.
Server processing — the web server (front end of the back end) handles the request, possibly querying a database for dynamic content, and may pass through a CDN or WAF along the way.
HTTP(S) response — the server returns HTML, CSS, JavaScript, images, etc., along with a status code.
Rendering — your browser parses the HTML, applies CSS, executes JavaScript, and renders the final page for you to see and interact with.

Browser → DNS lookup → IP found
Browser → HTTP(S) Request → [CDN/WAF] → Web Server → (Database if dynamic)
Web Server → HTTP(S) Response → Browser renders page
mermaidflowchart TD
    Start([You type tryhackme.com<br/>in your browser]) --> DNS{IP cached<br/>locally?}
    DNS -->|No| Resolve["DNS Resolution<br/>Recursive → Root → TLD → Authoritative"]
    Resolve --> IP[IP address obtained]
    DNS -->|Yes| IP

    IP --> Req["Browser sends HTTP/HTTPS request<br/>GET / HTTP/1.1"]
    Req --> WAF{WAF / CDN<br/>in the way?}
    WAF -->|Filters malicious traffic| Server[Web Server]
    WAF -->|Static asset cached nearby| CDN[CDN edge server] --> BrowserRender
    WAF -->|No CDN/WAF| Server

    Server --> Dynamic{Static or<br/>dynamic content?}
    Dynamic -->|Static| Resp["Server returns file as-is"]
    Dynamic -->|Dynamic| DB[(Database query)]
    DB --> Resp

    Resp --> BrowserRender["Browser receives HTTP response<br/>HTML + CSS + JS + status code"]
    BrowserRender --> Render["Browser parses HTML, applies CSS,<br/>executes JS, renders page"]
    Render --> End([Page displayed to user])

    style Start fill:#1565c0,color:#fff
    style End fill:#2e7d32,color:#fff
    style DB fill:#6a1b9a,color:#fff
Why this matters for security: every one of these steps is a potential attack surface — DNS spoofing/hijacking, intercepting/manipulating HTTP requests, exploiting backend logic or databases (e.g. injection attacks), or abusing the front end (HTML/JS injection). Understanding the full chain is the foundation for the rest of the Pre-Security / web security learning path.

Quick reference summary

DNS = name → IP address translation, layered through resolvers, root, TLD, and authoritative servers, with records like A/AAAA/CNAME/MX/TXT.
HTTP = the request/response protocol browsers and servers speak; methods (GET/POST/PUT/DELETE), status code ranges (1xx–5xx), headers, and cookies.
Website architecture = front end (HTML/CSS/JS, client-side) talks to back end (server logic + database) over HTTP.
Full picture = DNS resolves the address → HTTP carries the request/response → front end renders what the back end + database produced.

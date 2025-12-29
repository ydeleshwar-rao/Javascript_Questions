# 🔐 Frontend Web Security – Complete Learning Guide

> A comprehensive guide covering essential web security concepts for frontend developers, from browser fundamentals to advanced security patterns.

[![Made with ❤️](https://img.shields.io/badge/Made%20with-%E2%9D%A4%EF%B8%8F-red)](https://github.com)
[![Security First](https://img.shields.io/badge/Security-First-green)](https://github.com)

---

## 📋 Table of Contents

- [Phase 1: Foundations – Browser & Web Basics](#phase-1-foundations--browser--web-basics)
  - [1. Introduction to Web Security](#1-introduction-to-web-security)
  - [2. How Browsers Work](#2-how-browsers-work)
  - [3. Origin vs Site vs URL](#3-origin-vs-site-vs-url)
  - [4. Same-Origin Policy (SOP)](#4-same-origin-policy-sop)
  - [5. HTTP vs HTTPS](#5-http-vs-https)
  - [6. TLS Basics](#6-tls-basics-for-frontend-developers)
  - [7. Mixed Content Issues](#7-mixed-content-issues)
  - [8. Request-Response Lifecycle](#8-request-response-lifecycle)

---

## Phase 1: Foundations – Browser & Web Basics

### 1. Introduction to Web Security

**What is Web Security?**

Web security is the practice of protecting web applications and users from malicious attacks, data breaches, and unauthorized access. For frontend developers, this means understanding how browsers enforce security policies and how to write code that defends against common vulnerabilities.

**Threat Modeling – The STRIDE Framework**

```
┌─────────────────────────────────────────────────────┐
│                  STRIDE Model                       │
├─────────────────────────────────────────────────────┤
│  S - Spoofing Identity                              │
│  T - Tampering with Data                            │
│  R - Repudiation                                    │
│  I - Information Disclosure                         │
│  D - Denial of Service                              │
│  E - Elevation of Privilege                         │
└─────────────────────────────────────────────────────┘
```

**Real-World Example:**

```javascript
// ❌ Bad: Trusting user input without validation
function displayUserName(name) {
  document.getElementById('welcome').innerHTML = `Welcome ${name}`;
  // Vulnerable to XSS attacks!
}

// ✅ Good: Sanitizing user input
function displayUserName(name) {
  const sanitized = DOMPurify.sanitize(name);
  document.getElementById('welcome').textContent = sanitized;
  // Safe from XSS
}
```

**Key Takeaway:** Always assume user input is malicious until proven otherwise.

---

### 2. How Browsers Work

**Browser Architecture Overview**

```
┌────────────────────────────────────────────────────────┐
│                    USER INTERFACE                      │
│  (Address Bar, Back/Forward, Bookmarks)                │
├────────────────────────────────────────────────────────┤
│                   BROWSER ENGINE                       │
│  (Manages communication between UI and Rendering)      │
├────────────────────────────────────────────────────────┤
│                  RENDERING ENGINE                      │
│  ┌──────────┬──────────┬──────────┬──────────┐        │
│  │   HTML   │   CSS    │JavaScript│  Media   │        │
│  │  Parser  │  Parser  │ Engine   │  Player  │        │
│  └──────────┴──────────┴──────────┴──────────┘        │
├────────────────────────────────────────────────────────┤
│                    NETWORKING                          │
│  (HTTP/HTTPS Requests, DNS Resolution)                 │
├────────────────────────────────────────────────────────┤
│                  DATA PERSISTENCE                      │
│  (Cookies, LocalStorage, IndexedDB, Cache)             │
└────────────────────────────────────────────────────────┘
```

**The Rendering Pipeline:**

```
HTML → DOM Tree → Render Tree → Layout → Paint → Composite
  ↓
 CSS → CSSOM Tree ──┘
```

**Security Implications:**

- Each tab runs in a separate **process** (process isolation)
- **Sandbox** prevents malicious code from accessing OS resources
- **Rendering engine** validates HTML/CSS to prevent code injection

**Example – Process Isolation:**

```javascript
// Tab 1: example.com
const secretData = "user-token-12345";

// Tab 2: attacker.com
// Cannot access secretData from Tab 1 due to process isolation
console.log(window.secretData); // undefined
```

---

### 3. Origin vs Site vs URL

**Understanding the Hierarchy:**

```
┌──────────────────────────────────────────────────────┐
│                       URL                            │
│  https://api.example.com:443/users?id=123#profile    │
│  └─┬──┘ └────┬─────────┘└┬┘└───┬───┘└──┬──┘└──┬──┘  │
│  scheme    host       port path  query  fragment     │
└──────────────────────────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────┐
│                     ORIGIN                           │
│           scheme + host + port                       │
│         https://api.example.com:443                  │
└──────────────────────────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────┐
│                      SITE                            │
│              scheme + eTLD+1                         │
│              https://example.com                     │
│  (Ignores subdomain and port)                        │
└──────────────────────────────────────────────────────┘
```

**Practical Examples:**

| URL 1 | URL 2 | Same Origin? | Same Site? |
|-------|-------|--------------|------------|
| `https://example.com` | `https://example.com/page` | ✅ Yes | ✅ Yes |
| `https://example.com` | `http://example.com` | ❌ No (different scheme) | ❌ No |
| `https://api.example.com` | `https://www.example.com` | ❌ No (different subdomain) | ✅ Yes |
| `https://example.com:443` | `https://example.com:8080` | ❌ No (different port) | ✅ Yes |
| `https://example.com` | `https://example.co.uk` | ❌ No | ❌ No |

**Code Example:**

```javascript
// Current page: https://example.com

// Same Origin ✅
fetch('https://example.com/api/data'); // Works

// Cross-Origin ❌ (different subdomain)
fetch('https://api.example.com/data'); // Blocked by CORS

// Cross-Origin ❌ (different port)
fetch('https://example.com:8080/data'); // Blocked by CORS
```

---

### 4. Same-Origin Policy (SOP)

**What is SOP?**

Same-Origin Policy is the browser's most fundamental security mechanism. It prevents scripts from one origin from accessing data from another origin.

**Visual Representation:**

```
┌─────────────────────────┐       ┌─────────────────────────┐
│   https://bank.com      │       │  https://attacker.com   │
│                         │       │                         │
│  [User's Bank Account]  │       │   [Malicious Script]    │
│  Balance: $10,000       │       │                         │
│                         │       │  fetch('https://        │
│                         │       │    bank.com/account')   │
└─────────────────────────┘       └─────────────────────────┘
           ▲                                    │
           │                                    │
           └────────── ❌ BLOCKED ───────────────┘
                    (Same-Origin Policy)
```

**What SOP Protects:**

- ✅ DOM access across different origins
- ✅ Reading responses from cross-origin requests
- ✅ Cookies from different origins
- ✅ LocalStorage/SessionStorage across origins

**What SOP Allows:**

- ✅ Embedding images: `<img src="https://other-site.com/image.jpg">`
- ✅ Embedding scripts: `<script src="https://cdn.com/library.js">`
- ✅ Embedding CSS: `<link rel="stylesheet" href="https://cdn.com/style.css">`
- ✅ Submitting forms to different origins

**Code Example – SOP in Action:**

```html
<!-- Page on https://example.com -->
<iframe src="https://bank.com" id="bankFrame"></iframe>

<script>
  const iframe = document.getElementById('bankFrame');
  
  // ❌ Blocked by SOP
  try {
    console.log(iframe.contentWindow.document);
  } catch (e) {
    console.error("SOP Violation:", e);
    // DOMException: Blocked a frame from accessing a cross-origin frame
  }
</script>
```

**Bypassing SOP Safely:**

```javascript
// Using CORS (Cross-Origin Resource Sharing)
// Server must explicitly allow cross-origin requests

fetch('https://api.example.com/data', {
  method: 'GET',
  headers: {
    'Content-Type': 'application/json'
  }
})
.then(response => response.json())
.then(data => console.log(data));

// Server response must include:
// Access-Control-Allow-Origin: https://mysite.com
```

---

### 5. HTTP vs HTTPS

**The Fundamental Difference:**

```
HTTP (Insecure)                    HTTPS (Secure)
──────────────                     ──────────────

Client ─────────────► Server       Client ──🔒───► Server
  │  Plain Text Data  │              │ Encrypted  │
  │  Anyone can read  │              │ TLS/SSL    │
  └───────────────────┘              └────────────┘

┌─────────────────────────────────────────────────────┐
│  Man-in-the-Middle Attack on HTTP                   │
├─────────────────────────────────────────────────────┤
│                                                      │
│  User ────→ Attacker ────→ Server                   │
│    (reads/modifies data)                            │
│                                                      │
│  Password: "secret123" ← Attacker sees this!        │
└─────────────────────────────────────────────────────┘
```

**Why HTTPS Matters:**

1. **Encryption:** Data is encrypted in transit
2. **Authentication:** Verifies server identity via certificates
3. **Integrity:** Prevents data tampering

**Real Example – Login Form:**

```html
<!-- ❌ Dangerous: Password sent in plain text -->
<form action="http://example.com/login" method="POST">
  <input type="text" name="username">
  <input type="password" name="password">
  <button type="submit">Login</button>
</form>

<!-- ✅ Secure: Password encrypted -->
<form action="https://example.com/login" method="POST">
  <input type="text" name="username">
  <input type="password" name="password">
  <button type="submit">Login</button>
</form>
```

**Browser Security Features for HTTPS:**

```javascript
// Modern browsers enforce HTTPS for sensitive APIs

// ❌ Only works on HTTPS
if ('geolocation' in navigator) {
  navigator.geolocation.getCurrentPosition(/* ... */);
}

// ❌ Only works on HTTPS
navigator.mediaDevices.getUserMedia({ video: true });

// ❌ Service Workers require HTTPS
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}
```

---

### 6. TLS Basics for Frontend Developers

**What is TLS?**

Transport Layer Security (TLS) is the protocol that encrypts data between client and server. It's the "S" in HTTPS.

**TLS Handshake Process:**

```
Client                                  Server
  │                                       │
  │──── 1. ClientHello ──────────────────►│
  │     (Supported ciphers, TLS version)  │
  │                                       │
  │◄─── 2. ServerHello ───────────────────│
  │     (Chosen cipher, Certificate)      │
  │                                       │
  │──── 3. Key Exchange ──────────────────►│
  │     (Encrypted with server's key)     │
  │                                       │
  │◄─── 4. Finished ──────────────────────│
  │                                       │
  │──── 5. Encrypted Data ────────────────►│
  │◄──────────────────────────────────────│
```

**Certificate Validation:**

```
┌───────────────────────────────────────────────┐
│         SSL/TLS Certificate Chain             │
├───────────────────────────────────────────────┤
│                                               │
│  Root CA (e.g., DigiCert)                     │
│       ↓ signs                                 │
│  Intermediate CA                              │
│       ↓ signs                                 │
│  example.com Certificate ✅                   │
│                                               │
│  Browser trusts Root CA → validates chain     │
└───────────────────────────────────────────────┘
```

**Checking TLS in Browser DevTools:**

```javascript
// Open DevTools → Security Tab

// You can also check certificate programmatically
fetch('https://example.com')
  .then(response => {
    console.log('Connection secure:', response.ok);
    // Check response headers for security info
    console.log('Strict-Transport-Security:', 
      response.headers.get('strict-transport-security'));
  });
```

**Common TLS Issues for Frontend:**

```javascript
// Certificate errors users might see:
const TLS_ERRORS = {
  NET_ERR_CERT_AUTHORITY_INVALID: 'Certificate not trusted',
  NET_ERR_CERT_DATE_INVALID: 'Certificate expired',
  NET_ERR_CERT_COMMON_NAME_INVALID: 'Domain mismatch',
};

// ❌ Never ignore certificate errors in production!
// ✅ Always use valid certificates from trusted CAs
```

---

### 7. Mixed Content Issues

**What is Mixed Content?**

Mixed content occurs when an HTTPS page loads resources over HTTP. This creates security vulnerabilities because HTTP resources can be intercepted.

**Types of Mixed Content:**

```
┌─────────────────────────────────────────────────────┐
│              Mixed Content Types                     │
├─────────────────────────────────────────────────────┤
│                                                      │
│  🔴 Active Mixed Content (Blocked by default)        │
│     - Scripts, Stylesheets, Iframes                  │
│     - Can modify the page                            │
│                                                      │
│  🟡 Passive Mixed Content (Warning only)             │
│     - Images, Audio, Video                           │
│     - Limited modification capability                │
│                                                      │
└─────────────────────────────────────────────────────┘
```

**Visual Example:**

```
https://secure-site.com (🔒 HTTPS)
    │
    ├── ✅ https://cdn.com/script.js
    ├── ✅ https://images.com/logo.png
    ├── ❌ http://ads.com/tracking.js  ← BLOCKED!
    └── ⚠️  http://media.com/video.mp4  ← WARNING!
```

**Code Examples:**

```html
<!-- Page served over HTTPS -->
<!DOCTYPE html>
<html>
<head>
  <!-- ❌ Mixed Content: Script over HTTP -->
  <script src="http://cdn.example.com/jquery.js"></script>
  
  <!-- ✅ Secure: Script over HTTPS -->
  <script src="https://cdn.example.com/jquery.js"></script>
  
  <!-- ✅ Protocol-relative URL (inherits page protocol) -->
  <script src="//cdn.example.com/jquery.js"></script>
</head>
<body>
  <!-- ⚠️ Passive Mixed Content -->
  <img src="http://example.com/image.jpg">
  
  <!-- ✅ Secure -->
  <img src="https://example.com/image.jpg">
</body>
</html>
```

**Detecting Mixed Content:**

```javascript
// Check for mixed content warnings
const checkMixedContent = () => {
  const isSecure = location.protocol === 'https:';
  
  if (!isSecure) {
    console.warn('Page not served over HTTPS');
    return;
  }
  
  // Check all scripts
  document.querySelectorAll('script[src]').forEach(script => {
    if (script.src.startsWith('http://')) {
      console.error('Mixed Content:', script.src);
    }
  });
  
  // Check all images
  document.querySelectorAll('img[src]').forEach(img => {
    if (img.src.startsWith('http://')) {
      console.warn('Passive Mixed Content:', img.src);
    }
  });
};

// Run on page load
window.addEventListener('load', checkMixedContent);
```

**Fixing Mixed Content:**

```javascript
// Automatic upgrade using Content Security Policy
// Add this meta tag in HTML:
<meta http-equiv="Content-Security-Policy" 
      content="upgrade-insecure-requests">

// Or via HTTP header:
// Content-Security-Policy: upgrade-insecure-requests
```

---

### 8. Request-Response Lifecycle

**Complete Flow Diagram:**

```
┌──────────────────────────────────────────────────────┐
│        Request-Response Lifecycle in Browser         │
└──────────────────────────────────────────────────────┘

1. User Action
   │
   ▼
2. DNS Resolution
   │  example.com → 93.184.216.34
   ▼
3. TCP Connection
   │  Three-way handshake
   ▼
4. TLS Handshake (if HTTPS)
   │  Certificate validation, encryption setup
   ▼
5. HTTP Request
   │
   │  ┌─────────────────────────────────┐
   │  │ GET /api/users HTTP/1.1         │
   │  │ Host: example.com               │
   │  │ User-Agent: Mozilla/5.0         │
   │  │ Accept: application/json        │
   │  │ Cookie: session=abc123          │
   │  └─────────────────────────────────┘
   │
   ▼
6. Server Processing
   │
   ▼
7. HTTP Response
   │
   │  ┌─────────────────────────────────┐
   │  │ HTTP/1.1 200 OK                 │
   │  │ Content-Type: application/json  │
   │  │ Set-Cookie: token=xyz789        │
   │  │ Cache-Control: no-cache         │
   │  │                                 │
   │  │ {"users": [...]}                │
   │  └─────────────────────────────────┘
   │
   ▼
8. Browser Processing
   │  - Parse response
   │  - Update DOM
   │  - Store cookies
   ▼
9. Rendering
```

**Detailed Example with Code:**

```javascript
// Step 1: User initiates request
const fetchUsers = async () => {
  console.log('1. Starting request...');
  
  try {
    // Step 5: Browser sends HTTP request
    const response = await fetch('https://api.example.com/users', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer token123'
      },
      credentials: 'include' // Include cookies
    });
    
    // Step 7: Received response
    console.log('Status:', response.status);
    console.log('Headers:', response.headers);
    
    // Step 8: Process response
    const data = await response.json();
    
    // Step 9: Update UI
    displayUsers(data.users);
    
  } catch (error) {
    console.error('Request failed:', error);
  }
};
```

**Security Checkpoints in Lifecycle:**

```javascript
// Security checks at each stage:

// 1. DNS Resolution
// - DNSSEC validation
// - DNS over HTTPS (DoH)

// 2. TLS Handshake
// - Certificate validation
// - Cipher suite negotiation
// - Protocol version check

// 3. Request Phase
// - CORS preflight (for cross-origin)
// - Cookie validation
// - Header sanitization

// 4. Response Phase
// - Content-Type validation
// - XSS filtering
// - CORS header verification
// - Security headers check

const securityChecks = {
  request: {
    validateOrigin: (origin) => {
      const allowedOrigins = ['https://example.com'];
      return allowedOrigins.includes(origin);
    },
    
    sanitizeHeaders: (headers) => {
      // Remove sensitive headers
      delete headers['Internal-Token'];
      return headers;
    }
  },
  
  response: {
    validateContentType: (contentType) => {
      const expected = 'application/json';
      if (!contentType.includes(expected)) {
        throw new Error('Unexpected content type');
      }
    },
    
    checkSecurityHeaders: (headers) => {
      const required = [
        'X-Content-Type-Options',
        'X-Frame-Options',
        'Strict-Transport-Security'
      ];
      
      required.forEach(header => {
        if (!headers.has(header)) {
          console.warn(`Missing security header: ${header}`);
        }
      });
    }
  }
};
```

**Network Timeline Visualization:**

```
DNS Lookup       |███|
TCP Connection   |████|
TLS Handshake    |█████|
Request Sent     |██|
Waiting (TTFB)   |████████|
Content Download |███████|
─────────────────────────────────────────────→
0ms            100ms          200ms       300ms

TTFB = Time To First Byte
```

**Monitoring with Performance API:**

```javascript
// Measure request performance
const measureRequest = async (url) => {
  const startTime = performance.now();
  
  const response = await fetch(url);
  
  const endTime = performance.now();
  const duration = endTime - startTime;
  
  // Get detailed timing
  const entries = performance.getEntriesByType('navigation');
  if (entries.length > 0) {
    const timing = entries[0];
    
    console.log('Timing Breakdown:');
    console.log('DNS:', timing.domainLookupEnd - timing.domainLookupStart);
    console.log('TCP:', timing.connectEnd - timing.connectStart);
    console.log('Request:', timing.responseStart - timing.requestStart);
    console.log('Response:', timing.responseEnd - timing.responseStart);
    console.log('Total:', duration);
  }
  
  return response;
};
```

---

## 🎯 Key Takeaways

1. **Always use HTTPS** in production – no exceptions
2. **Same-Origin Policy** is your first line of defense
3. **Mixed Content** can compromise HTTPS security
4. **Understand Origins** to properly configure CORS
5. **Monitor the request lifecycle** for security issues

## 📚 Additional Resources

- [MDN Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [web.dev Security](https://web.dev/secure/)

## 🤝 Contributing

Contributions welcome! Please read our contributing guidelines before submitting PRs.

## 📝 License

This guide is available under the MIT License.

---

**Made with ❤️ for developers who care about security**
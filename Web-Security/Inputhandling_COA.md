# Frontend Security Guide

> Understanding Input Handling & Cross-Origin Security

---

## PHASE 3: Input Handling and Injection Attacks

### 🛡️ Trust Boundaries in Frontend Applications

**Simple Explanation:**  
Think of your app as a castle. Trust boundaries are the gates where outside data enters. Never trust anything coming from outside your castle walls.

**Example:**
```javascript
// ❌ BAD - Trusting user input directly
const username = urlParams.get('name');
document.getElementById('welcome').innerHTML = `Hello ${username}`;

// ✅ GOOD - Treating input as untrusted
const username = urlParams.get('name');
document.getElementById('welcome').textContent = username; // Safe
```

**Diagram:**
```
UNTRUSTED DATA          TRUST BOUNDARY          YOUR APP
┌─────────────┐              │              ┌──────────┐
│ User Input  │──────────────┼─────────────▶│ Validate │
│ URL Params  │   Validate   │              │ Sanitize │
│ External API│   Sanitize   │              │ Encode   │
└─────────────┘              │              └──────────┘
```

---

### ⚖️ Client-Side vs Server-Side Validation

**Simple Explanation:**  
- **Client-side:** Quick checks in browser (can be bypassed easily)
- **Server-side:** Real security checks on server (cannot be bypassed)

**Key Rule:** ALWAYS validate on server. Client-side is just for UX.

**Example:**
```javascript
// CLIENT-SIDE (JavaScript) - For user experience only
function validateEmail(email) {
  return email.includes('@'); // User can bypass this!
}

// SERVER-SIDE (Node.js) - Real security
app.post('/register', (req, res) => {
  const email = req.body.email;
  if (!isValidEmail(email)) {
    return res.status(400).send('Invalid email');
  }
  // This cannot be bypassed
});
```

**Diagram:**
```
    Browser                      Server
┌─────────────┐              ┌──────────┐
│ Client-Side │              │ Server   │
│ Validation  │──────────────▶│ Validate │
│ (Can bypass)│   HTTP POST  │ (SECURE) │
└─────────────┘              └──────────┘
     ❌ Weak                      ✅ Strong
```

---

### 🔄 Output Encoding vs Input Sanitization

**Simple Explanation:**  
- **Input Sanitization:** Clean bad data when it comes in (remove/escape dangerous characters)
- **Output Encoding:** Make data safe when displaying it (convert special characters)

**Example:**
```javascript
// INPUT SANITIZATION - Clean when receiving
function sanitize(input) {
  return input.replace(/<script>/gi, ''); // Remove script tags
}

// OUTPUT ENCODING - Escape when displaying
function encode(text) {
  const div = document.createElement('div');
  div.textContent = text; // Automatically encodes
  return div.innerHTML; // < becomes &lt;
}

// Usage
const userInput = '<script>alert("XSS")</script>';
const clean = sanitize(userInput); // Removes script
const safe = encode(clean); // Encodes remaining special chars
```

**Best Practice:** Prefer output encoding over input sanitization.

---

### 🚨 Cross-Site Scripting (XSS) – Overview

**Simple Explanation:**  
XSS is when an attacker injects malicious JavaScript into your website that runs in other users' browsers.

**Impact:**  
- Steal cookies/session tokens
- Redirect users to phishing sites
- Modify page content
- Steal sensitive data

**Types:** Reflected, Stored, DOM-Based

**Diagram:**
```
ATTACKER                    WEBSITE                    VICTIM
   │                           │                          │
   │──── Inject malicious ────▶│                          │
   │      JavaScript           │                          │
   │                           │◀──── Visit page ─────────│
   │                           │                          │
   │                           │──── Evil script runs ───▶│
   │◀──── Steal data ──────────┼──────────────────────────│
```

---

### 🔍 Reflected XSS

**Simple Explanation:**  
The malicious script is in the URL. When the victim clicks a crafted link, the script reflects back and executes.

**Example:**
```javascript
// Vulnerable code
const search = new URLSearchParams(window.location.search).get('q');
document.getElementById('results').innerHTML = `You searched for: ${search}`;

// Attack URL:
// https://example.com/search?q=<script>alert(document.cookie)</script>
```

**Attack Flow:**
```
1. Attacker creates malicious URL:
   https://bank.com/search?q=<script>steal()</script>

2. Victim clicks link (via email/social media)

3. Bank website reflects the script in HTML

4. Script executes in victim's browser

5. Attacker steals session cookie
```

**Fix:**
```javascript
// ✅ Safe version
const search = new URLSearchParams(window.location.search).get('q');
document.getElementById('results').textContent = `You searched for: ${search}`;
```

---

### 💾 Stored XSS

**Simple Explanation:**  
The malicious script is saved in the database (comments, profile). Every user who views it gets attacked.

**Example:**
```javascript
// Vulnerable: Storing user comment
app.post('/comment', (req, res) => {
  const comment = req.body.comment; // No sanitization
  db.save({ comment }); // Stores: <script>evil()</script>
});

// Vulnerable: Displaying comments
app.get('/comments', (req, res) => {
  const comments = db.getAll();
  res.send(`<div>${comments.join('')}</div>`); // XSS executes!
});
```

**Attack Flow:**
```
1. Attacker posts comment:
   "Great article! <script>stealCookies()</script>"

2. Comment saved to database

3. Every user who views comments gets infected

4. Script runs in all victims' browsers
```

**Fix:**
```javascript
// ✅ Escape output
app.get('/comments', (req, res) => {
  const comments = db.getAll();
  const safe = comments.map(c => escapeHtml(c));
  res.send(`<div>${safe.join('')}</div>`);
});
```

---

### 🌐 DOM-Based XSS

**Simple Explanation:**  
The vulnerability is in client-side JavaScript itself. The page never sends malicious data to the server.

**Example:**
```javascript
// Vulnerable code
const name = window.location.hash.substring(1);
document.getElementById('welcome').innerHTML = `Hello ${name}`;

// Attack URL:
// https://example.com/#<img src=x onerror=alert(document.cookie)>
```

**Key Difference:**  
The server is not involved. The attack happens entirely in the browser's DOM manipulation.

**Diagram:**
```
Traditional XSS:  Browser ──▶ Server ──▶ Browser (script)
DOM-Based XSS:    Browser ──────────────▶ Browser (script)
                           (no server)
```

**Fix:**
```javascript
// ✅ Use textContent instead of innerHTML
const name = window.location.hash.substring(1);
document.getElementById('welcome').textContent = `Hello ${name}`;
```

---

### ⚠️ Dangerous DOM APIs

**Simple Explanation:**  
Some JavaScript functions can execute code from strings. Avoid them with user input!

**Dangerous APIs:**

```javascript
// ❌ NEVER use with user input
element.innerHTML = userInput;           // Can execute scripts
element.outerHTML = userInput;           // Can execute scripts
document.write(userInput);               // Can execute scripts
eval(userInput);                         // Executes JavaScript
setTimeout(userInput, 1000);             // Executes JavaScript
new Function(userInput);                 // Executes JavaScript
element.insertAdjacentHTML('beforeend', userInput); // Can execute scripts

// ✅ SAFE alternatives
element.textContent = userInput;         // Text only, no HTML
element.setAttribute('class', userInput); // Safe for attributes
element.innerText = userInput;           // Text only
```

**Example Attack:**
```javascript
// Vulnerable
const userComment = '<img src=x onerror="alert(document.cookie)">';
div.innerHTML = userComment; // XSS executes!

// Safe
div.textContent = userComment; // Displays as text, not HTML
```

---

### ⚛️ XSS in Modern Frameworks (React, Angular, Vue)

**Simple Explanation:**  
Modern frameworks auto-escape output by default, but there are escape hatches that can be dangerous.

#### React

**Safe by Default:**
```jsx
// ✅ Safe - React escapes automatically
const UserComment = ({ text }) => {
  return <div>{text}</div>; // Even if text has <script>, it's escaped
};
```

**Dangerous Escape Hatch:**
```jsx
// ❌ DANGEROUS - Don't use unless absolutely necessary
const UserComment = ({ html }) => {
  return <div dangerouslySetInnerHTML={{ __html: html }} />;
};

// Only use with sanitized HTML
import DOMPurify from 'dompurify';
const clean = DOMPurify.sanitize(dirty);
return <div dangerouslySetInnerHTML={{ __html: clean }} />;
```

#### Angular

**Safe by Default:**
```typescript
// ✅ Safe - Angular sanitizes
@Component({
  template: `<div>{{ userInput }}</div>`
})
```

**Dangerous Bypass:**
```typescript
// ❌ DANGEROUS
import { DomSanitizer } from '@angular/platform-browser';

constructor(private sanitizer: DomSanitizer) {}

getHtml() {
  return this.sanitizer.bypassSecurityTrustHtml(this.userInput); // Risky!
}
```

#### Vue

**Safe by Default:**
```vue
<!-- ✅ Safe - Vue escapes -->
<div>{{ userInput }}</div>
```

**Dangerous Directive:**
```vue
<!-- ❌ DANGEROUS -->
<div v-html="userInput"></div>

<!-- Only use with sanitization -->
<div v-html="sanitizedInput"></div>
```

**Key Takeaway:** Trust the framework's defaults. Only bypass security when absolutely necessary and always sanitize first.

---

## PHASE 4: Cross-Origin and Request Forgery Issues

### 🍪 How Browsers Automatically Send Cookies

**Simple Explanation:**  
Browsers automatically attach cookies to every request to a domain. This is both convenient and dangerous.

**Example:**
```javascript
// User logs into bank.com
// Server sets cookie: sessionId=abc123

// Later, ANY request to bank.com includes this cookie
fetch('https://bank.com/transfer', {
  method: 'POST',
  body: JSON.stringify({ to: 'attacker', amount: 1000 })
});
// Browser automatically sends: Cookie: sessionId=abc123
```

**Diagram:**
```
User visits bank.com
        │
        ▼
Server sets cookie: sessionId=abc123
        │
        ▼
Every request to bank.com includes cookie
        │
        ▼
GET /account    → Cookie: sessionId=abc123
POST /transfer  → Cookie: sessionId=abc123
DELETE /account → Cookie: sessionId=abc123
        │
        ▼
Even if request comes from evil.com!
```

**Why This Matters:** This automatic behavior is what makes CSRF possible.

---

### 🎣 Cross-Site Request Forgery (CSRF)

**Simple Explanation:**  
CSRF tricks your browser into making unwanted requests to a site where you're logged in, using your credentials automatically.

**Real-World Analogy:**  
You give your friend (browser) a key to your house (session cookie). A burglar (attacker) tricks your friend into opening your door while you're away.

**Simple Example:**
```html
<!-- Attacker's website: evil.com -->
<img src="https://bank.com/transfer?to=attacker&amount=10000" />

<!-- When victim visits evil.com while logged into bank.com,
     the browser automatically sends bank.com cookies,
     completing the transfer! -->
```

**Diagram:**
```
VICTIM                  EVIL.COM              BANK.COM
  │                        │                     │
  │─── Logged in ─────────┼────────────────────▶│
  │◀── Cookie set ────────┼─────────────────────│
  │                        │                     │
  │─── Visits evil.com ───▶│                     │
  │◀── Hidden form ────────│                     │
  │                        │                     │
  │─── Auto-submit ────────┼────────────────────▶│
  │    (with cookies!)     │                     │
  │                        │    Transfer money   │
```

---

### 📊 CSRF Attack Flow

**Step-by-Step:**

```
Step 1: Victim logs into bank.com
   Browser stores: Cookie: session=abc123

Step 2: Victim visits attacker's site evil.com
   (while still logged into bank.com)

Step 3: evil.com contains malicious form
   <form action="https://bank.com/transfer" method="POST">
     <input name="to" value="attacker" />
     <input name="amount" value="10000" />
   </form>
   <script>document.forms[0].submit();</script>

Step 4: Browser submits form to bank.com
   Automatically includes: Cookie: session=abc123

Step 5: Bank.com sees valid session cookie
   Processes transfer: $10,000 → attacker

Step 6: Attack complete
   Victim's money stolen without their knowledge
```

**Key Point:** The bank.com server can't tell if the request came from bank.com or evil.com because the cookie is valid.

---

### 🎫 Anti-CSRF Tokens

**Simple Explanation:**  
A secret token that must be included in every state-changing request. Attackers can't get this token, so they can't forge requests.

**How It Works:**
```javascript
// Server generates unique token when user logs in
const csrfToken = generateRandomToken(); // "xyz789abc"
session.csrfToken = csrfToken;

// Server includes token in HTML forms
<form action="/transfer" method="POST">
  <input type="hidden" name="csrf_token" value="xyz789abc" />
  <input name="amount" value="100" />
  <button>Transfer</button>
</form>

// Server validates token on submission
app.post('/transfer', (req, res) => {
  if (req.body.csrf_token !== req.session.csrfToken) {
    return res.status(403).send('Invalid CSRF token');
  }
  // Process transfer
});
```

**Why Attackers Can't Get the Token:**
- Token is embedded in the page HTML
- Same-Origin Policy prevents evil.com from reading bank.com's HTML
- Attacker can't include the correct token in their forged request

**Diagram:**
```
LEGITIMATE REQUEST (from bank.com):
Form includes token → Server validates → ✅ Success

CSRF ATTACK (from evil.com):
No token (attacker can't get it) → Server rejects → ❌ Failed
```

---

### 🍪 SameSite Cookies as CSRF Mitigation

**Simple Explanation:**  
SameSite is a cookie attribute that tells the browser: "Only send this cookie if the request comes from my own website."

**Values:**

```javascript
// Strict - Cookie never sent from other sites
Set-Cookie: session=abc123; SameSite=Strict

// Lax - Cookie sent only for safe methods (GET) from other sites
Set-Cookie: session=abc123; SameSite=Lax

// None - Cookie sent everywhere (requires Secure flag)
Set-Cookie: session=abc123; SameSite=None; Secure
```

**Example:**

```javascript
// Server sets cookie with SameSite
res.cookie('sessionId', 'abc123', {
  httpOnly: true,
  secure: true,
  sameSite: 'Strict' // or 'Lax'
});
```

**Comparison:**

```
Scenario: User logged into bank.com visits evil.com

WITHOUT SameSite:
evil.com → POST bank.com/transfer
Browser sends: Cookie: session=abc123 ❌ CSRF possible

WITH SameSite=Strict:
evil.com → POST bank.com/transfer
Browser does NOT send cookie ✅ CSRF prevented
```

**Which to Use:**
- `Strict`: Maximum security, but might break legitimate cross-site flows
- `Lax`: Good balance, recommended default
- `None`: Only if you need cross-site cookies (with HTTPS)

---

### 🔑 Why JWT Alone Does Not Prevent CSRF

**Simple Explanation:**  
If you store JWT in a cookie, it's automatically sent just like session cookies, making it vulnerable to CSRF.

**Common Misconception:**
```javascript
// ❌ WRONG: "JWTs prevent CSRF"
// If stored in cookie, it's sent automatically
res.cookie('jwt', token); // Still vulnerable to CSRF!

// Attacker's site
fetch('https://api.com/transfer', {
  method: 'POST',
  body: JSON.stringify({ to: 'attacker', amount: 1000 })
});
// Browser automatically sends jwt cookie → CSRF works!
```

**Safe JWT Storage:**

```javascript
// ✅ Store JWT in memory/localStorage (not cookies)
localStorage.setItem('token', jwt);

// Manually include in requests
fetch('https://api.com/transfer', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('token')}`
  },
  body: JSON.stringify({ to: 'recipient', amount: 100 })
});
// Attacker can't access localStorage from evil.com
// So they can't include the JWT → CSRF prevented
```

**Diagram:**
```
JWT in Cookie:
evil.com → POST api.com (browser auto-sends JWT cookie) → ❌ CSRF

JWT in localStorage:
evil.com → POST api.com (no automatic JWT) → ❌ Request fails
```

**Trade-off:** localStorage is safer from CSRF but vulnerable to XSS. Choose based on your threat model.

---

### 🌍 Cross-Origin Resource Sharing (CORS)

**Simple Explanation:**  
CORS is a security feature that controls which websites can access your API. By default, browsers block cross-origin requests.

**Default Browser Behavior:**
```javascript
// On evil.com
fetch('https://api.bank.com/accounts')
  .then(res => res.json())
  .catch(err => console.log('Blocked by CORS!'));

// Browser blocks this because evil.com ≠ api.bank.com
```

**Allowing CORS:**
```javascript
// Server: api.bank.com
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', 'https://myapp.com');
  res.header('Access-Control-Allow-Methods', 'GET, POST');
  next();
});
```

**Diagram:**
```
REQUEST                      BROWSER CHECK              SERVER
myapp.com ──────────────────▶ Same origin?             
  │                           ✅ Yes → Allow            
  │                                                     
evil.com ───────────────────▶ Same origin?             
  │                           ❌ No → Check CORS ─────▶ Response
  │                           Headers                   headers
  │◀──────────────────────────                         
  │  Access-Control-Allow-Origin: evil.com?           
  │  ✅ Yes → Allow | ❌ No → Block                    
```

**Key Point:** CORS protects your API from unauthorized websites.

---

### ✈️ Preflight Requests

**Simple Explanation:**  
For complex requests (like POST with custom headers), browsers send a "preflight" OPTIONS request first to check if the server allows it.

**When Preflight Happens:**
- Non-simple methods: PUT, DELETE, PATCH
- Custom headers: `Authorization`, `X-Custom-Header`
- Content-Type other than: `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`

**Example Flow:**

```javascript
// Client makes request
fetch('https://api.com/data', {
  method: 'DELETE',
  headers: {
    'Authorization': 'Bearer token123',
    'Content-Type': 'application/json'
  }
});
```

**Preflight Request (automatic):**
```http
OPTIONS /data HTTP/1.1
Origin: https://myapp.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Authorization, Content-Type
```

**Server Response:**
```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://myapp.com
Access-Control-Allow-Methods: GET, POST, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
Access-Control-Max-Age: 86400
```

**Then Actual Request:**
```http
DELETE /data HTTP/1.1
Authorization: Bearer token123
```

**Server Code:**
```javascript
app.options('/data', (req, res) => {
  res.header('Access-Control-Allow-Origin', 'https://myapp.com');
  res.header('Access-Control-Allow-Methods', 'DELETE');
  res.header('Access-Control-Allow-Headers', 'Authorization, Content-Type');
  res.header('Access-Control-Max-Age', '86400'); // Cache preflight
  res.sendStatus(204);
});
```

---

### 🔐 Credentialed CORS Requests

**Simple Explanation:**  
By default, CORS requests don't include cookies. You must explicitly enable this on both client and server.

**Client Side:**
```javascript
// ❌ Default: No cookies sent
fetch('https://api.com/data');

// ✅ Include cookies
fetch('https://api.com/data', {
  credentials: 'include' // Send cookies
});
```

**Server Side:**
```javascript
// Server must explicitly allow credentials
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', 'https://myapp.com');
  res.header('Access-Control-Allow-Credentials', 'true'); // Required!
  next();
});
```

**Important Rules:**
```javascript
// ❌ INVALID: Can't use wildcard with credentials
res.header('Access-Control-Allow-Origin', '*');
res.header('Access-Control-Allow-Credentials', 'true');
// This combination is rejected by browsers

// ✅ VALID: Must specify exact origin
res.header('Access-Control-Allow-Origin', 'https://myapp.com');
res.header('Access-Control-Allow-Credentials', 'true');
```

**Security Note:** Only enable credentials for trusted origins!

---

### ⚠️ Common CORS Misconfigurations

**1. Wildcard with Credentials**
```javascript
// ❌ DANGEROUS: Allows any site to access with credentials
res.header('Access-Control-Allow-Origin', '*');
res.header('Access-Control-Allow-Credentials', 'true');
// Actually won't work, browser blocks it
```

**2. Reflecting Origin**
```javascript
// ❌ DANGEROUS: Trusts any origin
const origin = req.headers.origin;
res.header('Access-Control-Allow-Origin', origin);
res.header('Access-Control-Allow-Credentials', 'true');
// evil.com can now access your API with credentials!
```

**3. Insufficient Origin Validation**
```javascript
// ❌ WEAK: Substring matching
const origin = req.headers.origin;
if (origin.includes('myapp.com')) {
  res.header('Access-Control-Allow-Origin', origin);
}
// Allows: evil-myapp.com, myapp.com.evil.com

// ✅ CORRECT: Exact matching
const allowedOrigins = ['https://myapp.com', 'https://app.myapp.com'];
if (allowedOrigins.includes(origin)) {
  res.header('Access-Control-Allow-Origin', origin);
}
```

**4. Exposing Sensitive Headers**
```javascript
// ❌ RISKY: Exposing internal headers
res.header('Access-Control-Expose-Headers', 'X-Internal-Token, X-API-Key');
// Now JavaScript can read these sensitive headers
```

**5. Overly Permissive Methods**
```javascript
// ❌ TOO OPEN: Allowing all methods
res.header('Access-Control-Allow-Methods', '*');

// ✅ BETTER: Only needed methods
res.header('Access-Control-Allow-Methods', 'GET, POST');
```

**Best Practices:**
- Whitelist specific origins, never use wildcards with credentials
- Validate origins with exact string matching
- Only allow necessary HTTP methods
- Don't expose sensitive headers
- Use SameSite cookies as additional protection
- Consider authentication tokens over cookies for APIs

---

## 📚 Quick Reference

### Security Checklist

**Input Handling:**
- [ ] Validate all input on server-side
- [ ] Use `textContent` instead of `innerHTML`
- [ ] Sanitize user-generated HTML with DOMPurify
- [ ] Avoid `eval()`, `Function()`, `setTimeout(string)`
- [ ] Trust framework defaults (React, Angular, Vue)

**CSRF Protection:**
- [ ] Implement anti-CSRF tokens for state-changing operations
- [ ] Set `SameSite` attribute on cookies (`Lax` or `Strict`)
- [ ] Validate origin/referrer headers
- [ ] Use custom headers for AJAX requests

**CORS Configuration:**
- [ ] Whitelist specific origins (no wildcards with credentials)
- [ ] Validate origins with exact matching
- [ ] Only allow necessary HTTP methods
- [ ] Implement proper preflight handling
- [ ] Don't expose sensitive headers

### Safe Coding Patterns

```javascript
// ✅ Safe output
element.textContent = userInput;

// ✅ Safe HTML (if needed)
import DOMPurify from 'dompurify';
element.innerHTML = DOMPurify.sanitize(userHTML);

// ✅ Safe CORS
const allowedOrigins = ['https://app.com'];
if (allowedOrigins.includes(req.headers.origin)) {
  res.header('Access-Control-Allow-Origin', req.headers.origin);
}

// ✅ Safe cookies
res.cookie('session', token, {
  httpOnly: true,
  secure: true,
  sameSite: 'Lax'
});

// ✅ CSRF protection
if (req.body.csrfToken !== req.session.csrfToken) {
  return res.status(403).send('Invalid token');
}
```

---

## 🎯 Summary

**Phase 3 Key Takeaways:**
- Never trust user input
- Validate on server, always
- Use safe DOM APIs (`textContent` over `innerHTML`)
- XSS can steal sessions, modify pages, and compromise users
- Modern frameworks help, but can be bypassed

**Phase 4 Key Takeaways:**
- Browsers automatically send cookies (enables CSRF)
- CSRF tricks browsers into making unwanted requests
- Protect with: CSRF tokens, SameSite cookies, origin validation
- CORS controls which sites can access your API
- JWT in cookies = vulnerable to CSRF

**Remember:** Security is layers. Use multiple defenses together!

---

## 📖 Additional Resources

- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP CSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
- [MDN: CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [web.dev: SameSite Cookies](https://web.dev/samesite-cookies-explained/)

---

**Last Updated:** January 2026  
**License:** MIT
# Web Performance Guide
## Networking & Core Web Vitals

---

## Segment 3 – Networking & Loading

### 1. DNS, TCP, TLS Overview

**What it is:** The foundational protocols that establish a connection before any HTTP request can be made.

**The Journey:**
```
DNS Lookup → TCP Handshake → TLS Handshake → HTTP Request
   (20ms)       (40ms)          (80ms)          (100ms)
```

**Diagram:**
```
User types: www.example.com
     ↓
[DNS Query] → "What's the IP address?"
     ↓
IP: 93.184.216.34
     ↓
[TCP Handshake] → SYN → SYN-ACK → ACK (3-way)
     ↓
[TLS Handshake] → ClientHello → ServerHello → Certificate → Encrypted ✓
     ↓
Ready to send HTTP request!
```

**Example:**
```javascript
// First visit to a website
// DNS: 20ms (resolve domain to IP)
// TCP: 40ms (establish connection)
// TLS: 80ms (secure the connection)
// Total: 140ms before first byte!

// Optimizations:
// - DNS Prefetch: <link rel="dns-prefetch" href="//api.example.com">
// - Preconnect: <link rel="preconnect" href="//cdn.example.com">
```

**Key Takeaway:** These handshakes happen BEFORE your content loads. On slow networks, this can add 300-500ms of delay!

---

### 2. HTTP/1.1 vs HTTP/2 vs HTTP/3

**HTTP/1.1 (1997):**
- One request per connection (or waits in line)
- Head-of-line blocking
- Uncompressed headers (repeated on every request)

**HTTP/2 (2015):**
- Multiplexing: Multiple requests over single connection
- Header compression (HPACK)
- Server push capability
- Binary protocol

**HTTP/3 (2020):**
- Built on QUIC (UDP instead of TCP)
- No head-of-line blocking at transport layer
- Faster connection establishment (0-RTT)
- Better for mobile/unstable networks

**Visual Comparison:**
```
HTTP/1.1 (6 requests):
Connection 1: [====Request 1====]
Connection 2: [====Request 2====]
Connection 3: [====Request 3====]
Connection 4: [====Request 4====]
Connection 5: [====Request 5====]
Connection 6: [====Request 6====]

HTTP/2 (6 requests):
Connection 1: [R1][R2][R3][R4][R5][R6] ← All multiplexed!

HTTP/3 (6 requests):
QUIC Stream: [R1][R2][R3][R4][R5][R6] ← Even faster, no TCP!
```

**Example:**
```html
<!-- Your page needs 10 CSS files, 20 images, 5 JS files -->

<!-- HTTP/1.1: Creates 6 connections, queues requests -->
<!-- Load time: ~3.5 seconds -->

<!-- HTTP/2: One connection, all files parallel -->
<!-- Load time: ~1.2 seconds -->

<!-- HTTP/3: UDP-based, survives network switches -->
<!-- Load time: ~0.9 seconds (especially on mobile) -->
```

---

### 3. Request Waterfall Analysis

**What it is:** A visual timeline showing when each resource loads and what blocks it.

**Waterfall Diagram:**
```
Time →  0ms    500ms   1000ms  1500ms  2000ms
HTML    [████]
  ↓
CSS            [██████]
  ↓                    ⌛ (blocks render)
JS             [████████████]
  ↓
Images                         [██][██][██]
Fonts                  [████████]
API                                    [██████]

Legend:
[██] = Download time
⌛ = Blocking resource
→ = Dependency chain
```

**Real Example:**
```javascript
// BAD: Waterfall nightmare
<head>
  <link rel="stylesheet" href="style.css">  // 0ms-500ms (blocks)
  <script src="analytics.js"></script>       // 500ms-800ms (blocks)
</head>
<body>
  <img src="hero.jpg">                       // 800ms-1200ms
  <script src="app.js"></script>             // 1200ms-1800ms
</body>

// GOOD: Optimized waterfall
<head>
  <link rel="preload" href="hero.jpg" as="image">
  <link rel="stylesheet" href="style.css">
  <script defer src="app.js"></script>
</head>
<body>
  <img src="hero.jpg">  // Loads in parallel!
</body>
```

**How to Read:**
1. **Queueing** (white): Waiting for connection
2. **DNS/TCP/TLS** (light colors): Connection setup
3. **Request sent** (green): Sending
4. **Waiting (TTFB)** (yellow): Server processing
5. **Content download** (blue): Receiving data

---

### 4. Latency vs Bandwidth

**Latency:** The time for data to travel (measured in milliseconds)
**Bandwidth:** The amount of data that can travel (measured in Mbps)

**Analogy:**
```
Latency = Width of the pipe
Bandwidth = Amount of water flowing

Scenario 1: Garden hose (high latency, low bandwidth)
[You]--------------------[Server]
      200ms delay, 1 Mbps

Scenario 2: Fire hose (low latency, high bandwidth)
[You]----[Server]
   20ms, 100 Mbps
```

**Real World Impact:**
```javascript
// Downloading a 1MB file:

// Mobile 4G: 50ms latency, 10 Mbps bandwidth
// 50ms (latency) + 800ms (download) = 850ms total

// Fiber: 5ms latency, 100 Mbps bandwidth
// 5ms (latency) + 80ms (download) = 85ms total

// Key insight: For small files (<100KB), latency matters more!
// For large files, bandwidth matters more!
```

**Optimization Strategy:**
```
Small files (CSS, JS):
✓ Reduce round trips (combine files)
✓ Use HTTP/2 multiplexing
✓ Minimize redirects

Large files (images, videos):
✓ Compress aggressively
✓ Use CDN for bandwidth
✓ Implement lazy loading
```

---

### 5. Connection Reuse and Multiplexing

**Connection Reuse (Keep-Alive):**
Reusing the same TCP connection for multiple HTTP requests.

**Multiplexing:**
Sending multiple requests simultaneously over one connection.

**Diagram:**
```
HTTP/1.1 Keep-Alive:
[Client] ←——Connection 1——→ [Server]
Request 1 → Response 1
Request 2 → Response 2  (waits for 1)
Request 3 → Response 3  (waits for 2)

HTTP/2 Multiplexing:
[Client] ←——Connection 1——→ [Server]
Request 1 ————→ ←———— Response 1
Request 2 ——→   ←—————— Response 2  (parallel!)
Request 3 ————————→ ←— Response 3  (parallel!)
```

**Example:**
```javascript
// HTTP/1.1 with Keep-Alive
// Connection opened once, reused 10 times
// Saves: 10 × (TCP + TLS) = 10 × 120ms = 1.2 seconds!

fetch('https://api.example.com/user')
  .then(() => fetch('https://api.example.com/posts'))
  .then(() => fetch('https://api.example.com/comments'))
// Same connection reused ✓

// HTTP/2 Multiplexing
Promise.all([
  fetch('https://api.example.com/user'),
  fetch('https://api.example.com/posts'),
  fetch('https://api.example.com/comments')
])
// All sent simultaneously over one connection!
```

---

## Segment 4 – Core Web Vitals

### 1. Largest Contentful Paint (LCP)

**What:** Time until the largest visible element appears on screen.

**Target:** < 2.5 seconds (good), < 4.0 seconds (needs improvement)

**Visual:**
```
Page Load Timeline:
0s        1s        2s        2.5s      3s        4s
|---------|---------|---------|---------|---------|
          ↑         ↑                   ↑
       Small      Hero Image         Sidebar
       Logo       appears            loads
                  ← LCP! ←
```

**Example:**
```html
<!-- BAD: LCP at 4.2s -->
<div class="hero">
  <img src="hero-image.jpg">  <!-- 3MB, unoptimized -->
</div>

<!-- GOOD: LCP at 1.8s -->
<div class="hero">
  <img 
    src="hero-image-800w.webp"  <!-- 80KB, optimized -->
    srcset="hero-image-400w.webp 400w,
            hero-image-800w.webp 800w,
            hero-image-1200w.webp 1200w"
    sizes="(max-width: 600px) 400px, 800px"
    fetchpriority="high"
    alt="Hero image"
  >
</div>
```

**Common LCP Elements:**
- Hero images
- Video thumbnails
- Large text blocks
- Background images with text

**Improvements:**
```javascript
// 1. Preload LCP image
<link rel="preload" as="image" href="hero.jpg">

// 2. Optimize image size
// Use WebP/AVIF, compress, serve responsive sizes

// 3. Remove render-blocking resources
<link rel="stylesheet" href="critical.css">  // Inline critical CSS
<script defer src="non-critical.js"></script>

// 4. Use CDN for faster delivery
```

---

### 2. Cumulative Layout Shift (CLS)

**What:** Measures visual stability - how much content moves around unexpectedly.

**Target:** < 0.1 (good), < 0.25 (needs improvement)

**Score Calculation:**
```
CLS = Impact Fraction × Distance Fraction

Impact Fraction = % of viewport affected
Distance Fraction = Distance moved / viewport height
```

**Visual Example:**
```
User tries to click "Yes" button:

Frame 1:                    Frame 2 (ad loads):
┌─────────────┐            ┌─────────────┐
│ Read more?  │            │   [AD]      │
│             │            │ Read more?  │
│ [Yes] [No]  │ → SHIFT → │             │
│      ↑      │            │ [Yes] [No]  │
│   cursor    │            │      ↑      │
└─────────────┘            └─────────────┘
                           Clicks "No" by accident!
                           CLS = 0.25 (bad)
```

**Example:**
```html
<!-- BAD: CLS of 0.35 -->
<img src="banner.jpg">  <!-- No dimensions -->
<div class="ad-slot"></div>  <!-- Injects later -->
<p>Content jumps down!</p>

<!-- GOOD: CLS of 0.02 -->
<img src="banner.jpg" width="800" height="400">  <!-- Reserved space -->
<div class="ad-slot" style="min-height: 250px;"></div>  <!-- Reserved -->
<p>Content stays stable!</p>

<!-- CSS technique -->
<style>
.image-wrapper {
  aspect-ratio: 16 / 9;  /* Reserves space */
}
.image-wrapper img {
  width: 100%;
  height: auto;
}
</style>
```

**Common Causes:**
- Images without dimensions
- Ads/embeds/iframes without reserved space
- Web fonts causing FOIT/FOUT
- Dynamic content insertion

**Fixes:**
```css
/* 1. Set image dimensions */
img { width: 100%; height: auto; }

/* 2. Use aspect-ratio for responsive */
.video-container { aspect-ratio: 16 / 9; }

/* 3. Font loading strategy */
@font-face {
  font-family: 'CustomFont';
  font-display: swap;  /* Shows fallback immediately */
}

/* 4. Reserve space for dynamic content */
.ad-slot { min-height: 250px; }
```

---

### 3. Interaction to Next Paint (INP)

**What:** Time from user interaction (click, tap, keypress) to visual response.

**Target:** < 200ms (good), < 500ms (needs improvement)

**Timeline:**
```
User clicks button:
|
v
[Input Delay] → [Processing] → [Rendering] → [Paint]
   (50ms)         (80ms)        (30ms)       (40ms)
|←————————————— INP: 200ms ——————————————→|
```

**Example:**
```javascript
// BAD: INP of 650ms
button.addEventListener('click', () => {
  // Heavy synchronous work blocks main thread
  const result = processHugeDataset(data);  // 500ms!
  updateUI(result);
});

// GOOD: INP of 180ms
button.addEventListener('click', async () => {
  // Show immediate feedback
  button.classList.add('loading');  // 16ms
  
  // Offload heavy work
  const result = await processInWorker(data);  // Non-blocking!
  
  // Update UI
  updateUI(result);  // 50ms
  button.classList.remove('loading');
});

// Web Worker for heavy processing
// worker.js
self.addEventListener('message', (e) => {
  const result = processHugeDataset(e.data);
  self.postMessage(result);
});
```

**Common Issues:**
- Long JavaScript tasks (>50ms)
- Heavy event handlers
- Large DOM updates
- Forced synchronous layouts

**Optimizations:**
```javascript
// 1. Debounce/throttle frequent events
const debouncedSearch = debounce((query) => {
  search(query);
}, 300);

// 2. Break up long tasks
async function processLargeList(items) {
  for (let i = 0; i < items.length; i += 100) {
    await scheduler.yield();  // Let browser breathe
    processBatch(items.slice(i, i + 100));
  }
}

// 3. Use requestIdleCallback for non-urgent work
requestIdleCallback(() => {
  analyticsTrack('page_view');
});

// 4. Optimize event handlers
// Bad: Attaching handlers to 1000 items
items.forEach(item => {
  item.addEventListener('click', handleClick);
});

// Good: Event delegation
container.addEventListener('click', (e) => {
  if (e.target.matches('.item')) {
    handleClick(e);
  }
});
```

---

### 4. Lab vs Field Data

**Lab Data (Synthetic):**
Controlled tests in a simulated environment (Lighthouse, WebPageTest).

**Field Data (Real User Monitoring):**
Real metrics from actual users in production (Chrome UX Report, analytics).

**Comparison:**
```
Lab Data (Lighthouse):
┌────────────────────────────┐
│ Device: Simulated Mobile   │
│ Network: 4G (throttled)    │
│ Location: US-East          │
│ LCP: 2.1s                  │
│ CLS: 0.05                  │
│ INP: 150ms                 │
└────────────────────────────┘
  ↑
  Consistent, but not real users

Field Data (Real Users):
┌────────────────────────────┐
│ Device: Mixed              │
│ Network: Varied            │
│ Location: Global           │
│ LCP: 3.4s (P75)            │
│ CLS: 0.12 (P75)            │
│ INP: 280ms (P75)           │
└────────────────────────────┘
  ↑
  Real experience, but variable
```

**Example:**
```javascript
// Lab testing with Lighthouse
npm install -g lighthouse
lighthouse https://example.com --view

// Field data collection
// Using Web Vitals library
import {onLCP, onCLS, onINP} from 'web-vitals';

onLCP((metric) => {
  analytics.send('LCP', metric.value);
});

onCLS((metric) => {
  analytics.send('CLS', metric.value);
});

onINP((metric) => {
  analytics.send('INP', metric.value);
});

// Chrome UX Report (CrUX) - Google's field data
// Check at: https://developers.google.com/speed/pagespeed/insights/
```

**When to Use Each:**
```
Lab Data:
✓ During development
✓ CI/CD performance checks
✓ Debugging specific issues
✓ Testing before deployment

Field Data:
✓ Understanding real user experience
✓ Prioritizing optimizations
✓ Measuring business impact
✓ Geographic/device insights
```

---

### 5. User Perception Mapping

**How users perceive loading speed:**

```
Time        User Feeling           Metric
0-100ms     Instant                ✓ Perfect
100-300ms   Slight delay           ✓ Good
300-1000ms  Noticeable            ⚠ Acceptable
1000ms+     Slow/frustrating       ✗ Poor
10000ms+    Abandoned             ✗✗ Critical
```

**Perception Timeline:**
```
0ms ————————————————————————————————————————————→ 10s
    |         |          |           |           |
  Instant   Smooth    Sluggish    Broken     Abandoned
    
    ↑         ↑          ↑
   INP      LCP      Time to Interactive
 (<200ms)  (<2.5s)     (<3.8s)
```

**Psychological Tricks:**
```javascript
// 1. Show something immediately (skeleton screens)
<div class="skeleton">
  <div class="skeleton-header"></div>
  <div class="skeleton-text"></div>
  <div class="skeleton-text"></div>
</div>

// 2. Progressive enhancement
// Show basic content first, enhance later
render(basicContent);  // 200ms - Users see something!
setTimeout(() => enhance(), 0);  // 1000ms - Add fancy features

// 3. Perceived performance via optimistic UI
function likePost(postId) {
  // Update UI immediately (feels instant!)
  updateLikeCount(postId, +1);
  
  // Send request in background
  api.like(postId).catch(() => {
    // Rollback if failed
    updateLikeCount(postId, -1);
  });
}

// 4. Loading indicators for > 1s operations
if (estimatedTime > 1000) {
  showProgressBar();
}
```

**User Expectations by Action:**
```
Interaction Type        Expected Response Time
─────────────────────   ──────────────────────
Hover effect            < 50ms   (instant)
Button click            < 100ms  (immediate)
Page navigation         < 1000ms (fast)
Form submission         < 1000ms (responsive)
Search results          < 1000ms (quick)
Load new page           < 3000ms (acceptable)
Complex operation       < 5000ms (show progress)
```

---

## Quick Reference

### Performance Budget Example
```
Metric          Target      Critical
────────────────────────────────────
LCP             < 2.5s      < 4.0s
CLS             < 0.1       < 0.25
INP             < 200ms     < 500ms
FCP             < 1.8s      < 3.0s
Total JS        < 300KB     < 500KB
Total Images    < 500KB     < 1MB
```

### Testing Tools
- **Lab:** Lighthouse, WebPageTest, Chrome DevTools
- **Field:** Google Analytics, Chrome UX Report, New Relic
- **Monitoring:** Sentry, Datadog, Cloudflare Analytics

### Key Optimization Priorities
1. Optimize LCP element (usually hero image)
2. Reserve space for dynamic content (CLS)
3. Minimize JavaScript execution (INP)
4. Use HTTP/2+ for multiplexing
5. Implement proper caching strategies

---

**Remember:** Every 100ms of improvement can increase conversions by 1%!
# Web Performance – Complete Deep Topic Roadmap 🚀

> A comprehensive guide written from a Senior Frontend Engineer's perspective with real-world examples, diagrams, and battle-tested insights.
---

## 📋 Table of Contents

1. [Browser & Rendering Fundamentals](#segment-1--browser--rendering-fundamentals)
2. [Critical Render Path (CRP)](#segment-2--critical-render-path-crp)

---

## Segment 1 – Browser & Rendering Fundamentals

### 1.1 HTML Parsing and DOM Construction

**Kya hota hai?**
Jab browser HTML file receive karta hai, wo usko top-to-bottom parse karta hai aur DOM (Document Object Model) tree banata hai. Ye ek tree structure hai jisme har HTML element ek node hota hai.

**Real Example:**

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My App</title>
  </head>
  <body>
    <header>
      <h1>Welcome</h1>
    </header>
    <main>
      <p>Content here</p>
    </main>
  </body>
</html>
```

**DOM Tree:**

```
Document
└── html
    ├── head
    │   └── title
    │       └── "My App"
    └── body
        ├── header
        │   └── h1
        │       └── "Welcome"
        └── main
            └── p
                └── "Content here"
```

**Senior Engineer Insight:**
- HTML parsing is **incremental** – browser doesn't wait for entire HTML
- `<script>` tags **block** parsing (unless async/defer)
- Use semantic HTML to keep DOM shallow and meaningful

**Performance Impact:**
- Large HTML = slower parsing
- Deep nesting = more memory consumption

---

### 1.2 CSSOM Creation

**Kya hota hai?**
Jaise DOM HTML se banta hai, waise hi CSSOM (CSS Object Model) CSS se banta hai. Browser saare CSS rules ko parse karke ek tree structure banata hai.

**Real Example:**

```css
body {
  font-size: 16px;
  color: #333;
}

header {
  background: blue;
  padding: 20px;
}

h1 {
  font-size: 24px;
  color: white;
}
```

**CSSOM Tree:**

```
body
├── font-size: 16px
├── color: #333
└── header
    ├── background: blue
    ├── padding: 20px (inherited)
    ├── font-size: 16px (inherited)
    └── h1
        ├── font-size: 24px
        └── color: white
```

**Senior Engineer Insight:**
- CSSOM construction **blocks rendering** – browser won't render until CSSOM is ready
- CSS is render-blocking by default
- More CSS selectors = longer CSSOM construction time

**Optimization Tips:**
```css
/* ❌ Bad - Complex selector */
body div.container > ul li:nth-child(2) a.link { }

/* ✅ Good - Simple, specific */
.nav-link { }
```

---

### 1.3 Render Tree Generation

**Kya hota hai?**
Browser DOM aur CSSOM ko combine karke **Render Tree** banata hai. Ye tree sirf visible elements ko contain karta hai.

**Visual Diagram:**

```
DOM Tree          +    CSSOM Tree    =    Render Tree
   html                   html              html
   ├─ head                ├─ styles         └─ body
   ├─ body                └─ rules              ├─ header (visible)
   │  ├─ header                                 │  └─ h1
   │  │  └─ h1                                  └─ main (visible)
   │  └─ main                                      └─ p
   │     └─ p (display:none) ❌              
```

**Real Example:**

```html
<div style="display: none;">Hidden</div>  <!-- Not in Render Tree -->
<div style="visibility: hidden;">Still here</div>  <!-- In Render Tree -->
<div>Visible</div>  <!-- In Render Tree -->
```

**Senior Engineer Insight:**
- `display: none` elements are **not** in Render Tree
- `visibility: hidden` elements **are** in Render Tree (occupy space)
- `opacity: 0` elements **are** in Render Tree

**Performance Impact:**
```css
/* ❌ Bad - Forces unnecessary render tree nodes */
.hidden-mobile {
  display: block;
  visibility: hidden;
}

/* ✅ Good - Removes from render tree */
.hidden-mobile {
  display: none;
}
```

---

### 1.4 Layout (Reflow)

**Kya hota hai?**
Layout phase mein browser har element ki **exact position** aur **size** calculate karta hai. Isko **reflow** bhi kehte hain.

**Real Example:**

```html
<div class="container">
  <div class="box">Box 1</div>
  <div class="box">Box 2</div>
</div>
```

```css
.container {
  width: 100%;
  display: flex;
}

.box {
  width: 50%;
  padding: 20px;
}
```

**Layout Calculation:**
```
Container: width = 1920px (viewport width)
Box 1: width = 960px, height = 60px (content + padding)
Box 2: width = 960px, height = 60px
```

**Senior Engineer Insight – Reflow Triggers:**

```javascript
// ❌ Bad - Causes multiple reflows
element.style.width = '100px';    // Reflow 1
element.style.height = '100px';   // Reflow 2
element.style.margin = '10px';    // Reflow 3

// ✅ Good - Single reflow
element.style.cssText = 'width: 100px; height: 100px; margin: 10px;';

// ✅ Best - Use classes
element.classList.add('optimized-class');
```

**Expensive Layout Properties:**
- `offsetTop`, `offsetLeft`, `offsetWidth`, `offsetHeight`
- `scrollTop`, `scrollLeft`, `scrollWidth`, `scrollHeight`
- `clientTop`, `clientLeft`, `clientWidth`, `clientHeight`
- `getComputedStyle()`

**Real Performance Example:**

```javascript
// ❌ Bad - Layout thrashing (read-write-read-write)
const boxes = document.querySelectorAll('.box');
boxes.forEach(box => {
  const height = box.offsetHeight; // Read (triggers layout)
  box.style.height = height + 10 + 'px'; // Write
});

// ✅ Good - Batch reads and writes
const boxes = document.querySelectorAll('.box');
const heights = Array.from(boxes).map(box => box.offsetHeight); // Batch reads
boxes.forEach((box, i) => {
  box.style.height = heights[i] + 10 + 'px'; // Batch writes
});
```

---

### 1.5 Paint and Composite

**Kya hota hai?**
**Paint**: Browser pixels ko screen pe draw karta hai (colors, borders, shadows, text).
**Composite**: Multiple layers ko combine karke final image banata hai.

**Visual Layers Diagram:**

```
┌─────────────────────────────────┐
│   Layer 3: Modal (z-index: 1000)│
├─────────────────────────────────┤
│   Layer 2: Header (fixed)       │
├─────────────────────────────────┤
│   Layer 1: Main Content          │
└─────────────────────────────────┘
        ↓ Composite
┌─────────────────────────────────┐
│      Final Rendered Page         │
└─────────────────────────────────┘
```

**Real Example:**

```css
/* Creates a new layer - GPU accelerated */
.animated-box {
  transform: translateZ(0); /* Force GPU layer */
  will-change: transform;
}

/* Expensive paint operation */
.shadow-box {
  box-shadow: 0 10px 50px rgba(0,0,0,0.5); /* Repaints on change */
}
```

**Senior Engineer Insight – Paint Triggers:**

```css
/* ❌ These trigger paint */
color, background, border, box-shadow, border-radius, visibility, outline

/* ✅ These only trigger composite (cheapest) */
transform, opacity

/* Example optimization */
/* ❌ Bad - Triggers layout + paint */
.animate-bad {
  transition: left 0.3s;
}
.animate-bad:hover {
  left: 100px;
}

/* ✅ Good - Only triggers composite */
.animate-good {
  transition: transform 0.3s;
}
.animate-good:hover {
  transform: translateX(100px);
}
```

**Chrome DevTools mein check karo:**
```
DevTools → More Tools → Rendering → Paint Flashing
```

---

### 1.6 GPU vs CPU Rendering

**Kya hota hai?**
- **CPU**: General processing, layout calculations, JavaScript
- **GPU**: Graphics operations, 3D transforms, opacity, filters

**Visual Architecture:**

```
┌──────────────────────────────────────┐
│         Browser Rendering            │
├──────────────────┬───────────────────┤
│   CPU (Main)     │   GPU (Graphics)  │
├──────────────────┼───────────────────┤
│ - Layout         │ - Compositing     │
│ - Paint          │ - 3D transforms   │
│ - JavaScript     │ - Opacity changes │
│ - DOM updates    │ - Filters         │
└──────────────────┴───────────────────┘
```

**Real Example – Force GPU Acceleration:**

```css
/* ❌ CPU rendered (slow animations) */
.cpu-animation {
  transition: left 0.3s;
}

/* ✅ GPU rendered (smooth 60fps) */
.gpu-animation {
  transition: transform 0.3s;
  will-change: transform;
  transform: translateZ(0); /* Creates GPU layer */
}
```

**Senior Engineer Insight:**

```css
/* GPU accelerated properties */
transform: translate3d(), scale3d(), rotate3d()
opacity
filter

/* Be careful with will-change */
/* ❌ Bad - Too many layers = memory issue */
* {
  will-change: transform;
}

/* ✅ Good - Specific elements only */
.animated-card {
  will-change: transform;
}
.animated-card.finished {
  will-change: auto; /* Remove after animation */
}
```

**JavaScript Example:**

```javascript
// ✅ Smooth 60fps animation using GPU
function animateGPU(element) {
  let position = 0;
  
  function update() {
    position += 2;
    element.style.transform = `translateX(${position}px)`; // GPU
    
    if (position < 500) {
      requestAnimationFrame(update);
    }
  }
  
  requestAnimationFrame(update);
}

// ❌ Janky animation using CPU
function animateCPU(element) {
  let position = 0;
  
  function update() {
    position += 2;
    element.style.left = position + 'px'; // CPU + Layout + Paint
    
    if (position < 500) {
      requestAnimationFrame(update);
    }
  }
  
  requestAnimationFrame(update);
}
```

---

### 1.7 Impact of DOM Size and Depth

**Kya hota hai?**
Large aur deep DOM tree performance ko significantly affect karta hai.

**Performance Impact:**

```
Small DOM:  ~1000 nodes  = Fast ⚡
Medium DOM: ~5000 nodes  = OK ✓
Large DOM:  ~10000 nodes = Slow 🐌
Huge DOM:   ~20000+ nodes = Very Slow 🐌🐌
```

**Real Example – Bad vs Good:**

```html
<!-- ❌ Bad - Deep nesting (7 levels) -->
<div class="wrapper">
  <div class="container">
    <div class="section">
      <div class="content">
        <div class="box">
          <div class="inner">
            <p>Text</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ✅ Good - Flat structure (3 levels) -->
<section class="content">
  <div class="box">
    <p>Text</p>
  </div>
</section>
```

**Senior Engineer Insight – Real Metrics:**

```javascript
// Measure DOM size impact
const measureDOM = () => {
  const nodeCount = document.querySelectorAll('*').length;
  const maxDepth = getMaxDepth(document.body);
  
  console.log(`DOM Nodes: ${nodeCount}`);
  console.log(`Max Depth: ${maxDepth}`);
  
  // Google's recommendations:
  // - Total nodes < 1500
  // - Max depth < 32
  // - Parent with > 60 children is bad
};

function getMaxDepth(node) {
  let maxDepth = 0;
  
  function traverse(node, depth) {
    if (depth > maxDepth) maxDepth = depth;
    
    for (let child of node.children) {
      traverse(child, depth + 1);
    }
  }
  
  traverse(node, 0);
  return maxDepth;
}
```

**Optimization Strategies:**

```javascript
// ❌ Bad - Renders huge list at once
function renderBadList(items) {
  const container = document.getElementById('list');
  items.forEach(item => {
    container.innerHTML += `<div>${item}</div>`; // 10,000 nodes!
  });
}

// ✅ Good - Virtual scrolling (only render visible items)
function renderVirtualList(items) {
  const container = document.getElementById('list');
  const itemHeight = 50;
  const visibleItems = Math.ceil(window.innerHeight / itemHeight);
  
  let scrollTop = 0;
  
  const render = () => {
    const startIndex = Math.floor(scrollTop / itemHeight);
    const endIndex = startIndex + visibleItems;
    
    container.innerHTML = '';
    container.style.height = items.length * itemHeight + 'px';
    
    for (let i = startIndex; i < endIndex; i++) {
      if (items[i]) {
        const div = document.createElement('div');
        div.textContent = items[i];
        div.style.transform = `translateY(${i * itemHeight}px)`;
        container.appendChild(div);
      }
    }
  };
  
  container.addEventListener('scroll', (e) => {
    scrollTop = e.target.scrollTop;
    requestAnimationFrame(render);
  });
  
  render(); // Initial render - only ~20 nodes!
}
```

**Chrome DevTools mein check karo:**
```
DevTools → Performance → Record
Look for:
- Long Tasks (> 50ms)
- Layout Thrashing
- Large DOM size warnings
```

---

## Segment 2 – Critical Render Path (CRP)

### 2.1 Render-blocking CSS

**Kya hota hai?**
CSS render-blocking resource hai – browser CSSOM complete hone tak kuch bhi render nahi karta. Ye First Contentful Paint (FCP) ko delay karta hai.

**Visual Timeline:**

```
Timeline without optimization:
├─ HTML Download (200ms)
├─ CSS Download (800ms) ⚠️ BLOCKING
├─ CSSOM Construction (100ms) ⚠️ BLOCKING
└─ First Paint (1100ms total) 😢

Timeline with optimization:
├─ HTML Download (200ms)
├─ Critical CSS Inline (0ms - already in HTML)
├─ First Paint (200ms total) 🎉
└─ Full CSS Download (async, 800ms)
```

**Real Example:**

```html
<!-- ❌ Bad - Blocks rendering for 800ms -->
<head>
  <link rel="stylesheet" href="styles.css"> <!-- 50KB -->
</head>

<!-- ✅ Good - Inline critical CSS -->
<head>
  <style>
    /* Critical above-the-fold CSS only (~5KB) */
    body { margin: 0; font-family: sans-serif; }
    .header { background: blue; height: 60px; }
    .hero { min-height: 400px; background: #f5f5f5; }
  </style>
  
  <!-- Load full CSS asynchronously -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
```

**Senior Engineer Insight – Media Queries:**

```html
<!-- ❌ Bad - All CSS blocks rendering -->
<link rel="stylesheet" href="styles.css">

<!-- ✅ Good - Non-critical CSS doesn't block -->
<link rel="stylesheet" href="critical.css">
<link rel="stylesheet" href="print.css" media="print"> <!-- Only loads for print -->
<link rel="stylesheet" href="mobile.css" media="(max-width: 768px)"> <!-- Conditional -->
```

**Critical CSS Extraction (Automated):**

```javascript
// Using Critical npm package
const critical = require('critical');

critical.generate({
  inline: true,
  base: 'dist/',
  src: 'index.html',
  target: 'index-critical.html',
  width: 1300,
  height: 900,
  dimensions: [{
    width: 375,
    height: 667
  }, {
    width: 1920,
    height: 1080
  }]
});
```

**Webpack Configuration:**

```javascript
// webpack.config.js
const CriticalCssPlugin = require('critical-css-webpack-plugin');

module.exports = {
  plugins: [
    new CriticalCssPlugin({
      base: 'dist/',
      src: 'index.html',
      inline: true,
      minify: true,
      dimensions: [{
        width: 375,
        height: 667
      }]
    })
  ]
};
```

---

### 2.2 Render-blocking JavaScript

**Kya hota hai?**
`<script>` tags HTML parsing ko block karte hain. Browser script ko download, parse, aur execute karta hai before continuing HTML parsing.

**Visual Timeline:**

```
Without async/defer:
HTML Parsing ──┐
               ├── BLOCKED ⚠️
               │   (JS Download + Execute)
               └── Continue Parsing
               
With async:
HTML Parsing ────────────────────
JS Download  ─────┐
                  └─ Execute (may interrupt parsing)
                  
With defer:
HTML Parsing ────────────────────┐
JS Download  ─────────────┐      │
                          └──────┴─ Execute after parsing
```

**Real Example:**

```html
<!DOCTYPE html>
<html>
<head>
  <!-- ❌ Bad - Blocks HTML parsing -->
  <script src="analytics.js"></script> <!-- 100KB -->
  <script src="library.js"></script>   <!-- 200KB -->
</head>
<body>
  <h1>Welcome</h1>
  <!-- Content appears after JS loads -->
</body>
</html>

<!-- ✅ Good - Non-blocking -->
<!DOCTYPE html>
<html>
<head>
  <!-- Critical JS only -->
  <script>
    // Minimal inline JS for critical functionality
    window.APP_CONFIG = { apiUrl: '/api' };
  </script>
</head>
<body>
  <h1>Welcome</h1>
  <!-- Content appears immediately -->
  
  <!-- Non-critical JS at bottom with defer -->
  <script src="analytics.js" defer></script>
  <script src="library.js" defer></script>
</body>
</html>
```

---

### 2.3 Script Loading Strategies (normal, async, defer)

**Complete Comparison:**

```html
<!-- 1️⃣ Normal (Synchronous) -->
<script src="script.js"></script>
<!-- ❌ Blocks HTML parsing -->
<!-- ❌ Executes immediately -->
<!-- ✅ Maintains execution order -->

<!-- 2️⃣ Async -->
<script src="script.js" async></script>
<!-- ✅ Downloads in parallel -->
<!-- ❌ Executes ASAP (may interrupt parsing) -->
<!-- ❌ Execution order not guaranteed -->

<!-- 3️⃣ Defer -->
<script src="script.js" defer></script>
<!-- ✅ Downloads in parallel -->
<!-- ✅ Executes after HTML parsing -->
<!-- ✅ Maintains execution order -->
```

**Visual Diagram:**

```
Normal:
HTML ───────┤ (blocked) ├───────
Script      └─ Download → Execute

Async:
HTML ─────────────────────────────
Script  ──── Download ──┤ Execute

Defer:
HTML ───────────────────────┤
Script  ──── Download ──────┴─ Execute
```

**Real-World Decision Tree:**

```javascript
// When to use what?

// ❌ Normal (Avoid unless necessary)
<script src="must-run-before-html.js"></script>
// Use only for critical scripts that must run before HTML parsing

// ✅ Async - For independent scripts
<script src="analytics.js" async></script>
<script src="ads.js" async></script>
<script src="chat-widget.js" async></script>
// Perfect for: Analytics, ads, independent widgets

// ✅ Defer - For scripts that need DOM
<script src="jquery.js" defer></script>
<script src="app.js" defer></script>
<script src="ui-interactions.js" defer></script>
// Perfect for: Libraries, app code, DOM manipulation
```

**Senior Engineer Example:**

```html
<!DOCTYPE html>
<html>
<head>
  <!-- Critical: Theme must load first -->
  <script>
    localStorage.getItem('theme') === 'dark' && document.documentElement.classList.add('dark');
  </script>
  
  <!-- Analytics: Independent, use async -->
  <script src="https://www.googletagmanager.com/gtag/js?id=GA_ID" async></script>
  
  <!-- Preload important defer scripts -->
  <link rel="preload" href="app.js" as="script">
</head>
<body>
  <div id="root"></div>
  
  <!-- App code: Needs DOM, use defer -->
  <script src="react.js" defer></script>
  <script src="app.js" defer></script>
  
  <!-- Chat widget: Independent, use async -->
  <script src="intercom.js" async></script>
</body>
</html>
```

---

### 2.4 Inline vs External CSS

**Trade-offs Analysis:**

```html
<!-- ✅ Inline CSS - Pros & Cons -->
<head>
  <style>
    .header { background: blue; }
  </style>
</head>
<!-- 
PROS:
+ No additional HTTP request
+ Immediately available (no network delay)
+ Perfect for critical above-the-fold styles

CONS:
- Increases HTML size
- Not cacheable separately
- Repeated across pages
-->

<!-- ✅ External CSS - Pros & Cons -->
<head>
  <link rel="stylesheet" href="styles.css">
</head>
<!--
PROS:
+ Cached across pages
+ Keeps HTML lean
+ Better for large stylesheets

CONS:
- Additional HTTP request
- Blocks rendering until downloaded
- Network latency impact
-->
```

**Senior Engineer Strategy:**

```html
<!DOCTYPE html>
<html>
<head>
  <!-- 1️⃣ Inline Critical CSS (~5-10KB) -->
  <style>
    /* Above-the-fold styles only */
    body { margin: 0; font-family: system-ui; }
    .header { height: 60px; background: #000; color: #fff; }
    .hero { min-height: 400px; display: flex; align-items: center; }
  </style>
  
  <!-- 2️⃣ Preload full stylesheet -->
  <link rel="preload" href="styles.css" as="style">
  
  <!-- 3️⃣ Load full CSS asynchronously -->
  <link rel="stylesheet" href="styles.css" media="print" onload="this.media='all'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
<body>
  <!-- Content renders immediately with critical styles -->
</body>
</html>
```

**Build Tool Automation:**

```javascript
// Vite/Webpack configuration
// postcss.config.js
module.exports = {
  plugins: [
    require('@fullhuman/postcss-purgecss')({
      content: ['./src/**/*.html', './src/**/*.jsx'],
      safelist: ['critical-class'],
      // Remove unused CSS
    }),
    require('cssnano')({
      preset: 'default',
      // Minify CSS
    })
  ]
};

// Critical CSS extraction in build
const fs = require('fs');
const path = require('path');
const { PurgeCSS } = require('purgecss');

async function extractCriticalCSS() {
  const purgeCSSResult = await new PurgeCSS().purge({
    content: ['index.html'],
    css: ['styles.css'],
    safelist: []
  });
  
  const criticalCSS = purgeCSSResult[0].css;
  
  // Inject into HTML
  const html = fs.readFileSync('index.html', 'utf8');
  const htmlWithCritical = html.replace(
    '</head>',
    `<style>${criticalCSS}</style></head>`
  );
  
  fs.writeFileSync('dist/index.html', htmlWithCritical);
}
```

---

### 2.5 Above-the-fold Rendering

**Kya hota hai?**
Above-the-fold wo content hai jo user ko first view mein dikhta hai without scrolling. Isko priority dena chahiye.

**Visual Concept:**

```
┌──────────────────────────────┐
│      🖥️ ABOVE THE FOLD      │ ← Priority: Load first
│                              │
│  [Logo]     Navigation       │
│                              │
│  Hero Heading                │
│  Hero Description            │
│  [CTA Button]                │
│                              │
├──────────────────────────────┤ ← Fold Line
│                              │
│  🔽 BELOW THE FOLD           │ ← Can lazy load
│                              │
│  Features Section            │
│  Testimonials                │
│  Footer                      │
└──────────────────────────────┘
```

**Real Example:**

```html
<!DOCTYPE html>
<html>
<head>
  <!-- Critical CSS for above-the-fold only -->
  <style>
    /* Hero section (visible immediately) */
    .hero {
      min-height: 100vh;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
    }
    
    .hero h1 { font-size: 3rem; margin: 0; }
    .hero p { font-size: 1.5rem; }
    .cta-button {
      padding: 15px 30px;
      background: white;
      color: #667eea;
      border-radius: 5px;
      text-decoration: none;
      font-weight: bold;
    }
  </style>
  
  <!-- Preload hero image -->
  <link rel="preload" href="hero-image.jpg" as="image">
</head>
<body>
  <!-- Above-the-fold content -->
  <section class="hero">
    <div>
      <h1>Welcome to Our App</h1>
      <p>The best solution for your needs</p>
      <a href="#" class="cta-button">Get Started</a>
    </div>
  </section>
  
  <!-- Below-the-fold content - Lazy load -->
  <section class="features" data-lazy>
    <!-- Will load when user scrolls -->
  </section>
  
  <!-- Load non-critical CSS -->
  <link rel="stylesheet" href="below-fold.css" media="print" onload="this.media='all'">
  
  <!-- Lazy load script -->
  <script>
    // Intersection Observer for lazy loading
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const section = entry.target;
          // Load below-fold content
          loadBelowFoldContent(section);
          observer.unobserve(section);
        }
      });
    }, {
      rootMargin: '200px' // Start loading 200px before visible
    });
    
    document.querySelectorAll('[data-lazy]').forEach(el => {
      observer.observe(el);
    });
  </script>
</body>
</html>
```

**Senior Engineer Insight – Measuring Viewport:**

```javascript
// Calculate above-the-fold height
const getAboveFoldHeight = () => {
  return window.innerHeight || document.documentElement.clientHeight;
};

// Check if element is above fold
const isAboveFold = (element) => {
  const rect = element.getBoundingClientRect();
  const viewportHeight = getAboveFoldHeight();
  return rect.top < viewportHeight && rect.bottom > 0;
};

// Prioritize above-fold images
document.addEventListener('DOMContentLoaded', () => {
  const images = document.querySelectorAll('img[data-src]');
  
  images.forEach(img => {
    if (isAboveFold(img)) {
      // Load immediately
      img.src = img.dataset.src;
      img.removeAttribute('data-src');
    } else {
      // Lazy load
      lazyLoadObserver.observe(img);
    }
  });
});
```

**React Example:**

```jsx
import React, { Suspense, lazy } from 'react';

// Above-the-fold component (loaded immediately)
const Hero = () => (
  <section className="hero">
    <h1>Welcome</h1>
    <p>Above the fold content</p>
    <button>Get Started</button>
  </section>
);

// Below-the-fold components (lazy loaded)
const Features = lazy(() => import('./Features'));
const Testimonials = lazy(() => import('./Testimonials'));
const Footer = lazy(() => import('./Footer'));

function App() {
  return (
    <div>
      {/* Renders immediately */}
      <Hero />
      
      {/* Lazy loaded when user scrolls */}
      <Suspense fallback={<div>Loading...</div>}>
        <Features />
        <Testimonials />
        <Footer />
      </Suspense>
    </div>
  );
}
```
### 2.6 Blocking vs Non-blocking Resources

**Kya hota hai?**
Resources do types ke hote hain based on how they affect page rendering:
- **Blocking Resources**: Page rendering ko block karte hain
- **Non-blocking Resources**: Background mein load hote hain, rendering ko block nahi karte

**Complete Resource Types:**

```
BLOCKING RESOURCES (Render Blocking):
├─ CSS (stylesheets)
├─ Synchronous JavaScript (<script> without async/defer)
├─ @import in CSS
└─ Fonts (if not using font-display)

NON-BLOCKING RESOURCES:
├─ Images (<img>)
├─ Videos (<video>)
├─ Async JavaScript (<script async>)
├─ Deferred JavaScript (<script defer>)
├─ Preloaded resources (<link rel="preload">)
└─ Background images in CSS
```

**Visual Timeline Comparison:**

```
BLOCKING Resources:
HTML Parsing ──┤ BLOCKED ⚠️ ├─────────
CSS Download   └─────────────┘
                           └─ First Paint at 1000ms

NON-BLOCKING Resources:
HTML Parsing ────────────────────────────
Image Download  ─────────────┐
                            └─ First Paint at 200ms
                               Images appear at 600ms
```

**Real Example - All Resource Types:**

```html
<!DOCTYPE html>
<html>
<head>
  <!-- ❌ BLOCKING: CSS -->
  <link rel="stylesheet" href="styles.css"> <!-- Blocks rendering -->
  
  <!-- ❌ BLOCKING: Synchronous JS -->
  <script src="analytics.js"></script> <!-- Blocks HTML parsing -->
  
  <!-- ❌ BLOCKING: Font without font-display -->
  <style>
    @font-face {
      font-family: 'CustomFont';
      src: url('font.woff2');
      /* Missing font-display: swap; */
    }
  </style>
  
  <!-- ✅ NON-BLOCKING: Preload (hint to browser) -->
  <link rel="preload" href="hero-image.jpg" as="image">
  
  <!-- ✅ NON-BLOCKING: DNS Prefetch -->
  <link rel="dns-prefetch" href="https://api.example.com">
</head>
<body>
  <!-- ✅ NON-BLOCKING: Images -->
  <img src="logo.png" alt="Logo"> <!-- Loads in parallel -->
  
  <!-- ✅ NON-BLOCKING: Async Script -->
  <script src="chat-widget.js" async></script>
  
  <!-- ✅ NON-BLOCKING: Defer Script -->
  <script src="app.js" defer></script>
  
  <!-- ✅ NON-BLOCKING: Lazy-loaded Image -->
  <img data-src="lazy-image.jpg" loading="lazy" alt="Lazy">
  
  <!-- ✅ NON-BLOCKING: Video -->
  <video src="video.mp4" preload="none"></video>
</body>
</html>
```

**Senior Engineer Insight - Resource Priority:**

```html
<!DOCTYPE html>
<html>
<head>
  <!-- 1️⃣ CRITICAL (Inline to avoid blocking) -->
  <style>
    /* Critical above-the-fold CSS */
    .header { height: 60px; background: #000; }
  </style>
  
  <!-- 2️⃣ HIGH PRIORITY (Preload) -->
  <link rel="preload" href="hero-image.jpg" as="image">
  <link rel="preload" href="main.css" as="style">
  <link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin>
  
  <!-- 3️⃣ MEDIUM PRIORITY (Defer CSS) -->
  <link rel="stylesheet" href="main.css" media="print" onload="this.media='all'">
  
  <!-- 4️⃣ LOW PRIORITY (Prefetch for next page) -->
  <link rel="prefetch" href="next-page.css">
  <link rel="dns-prefetch" href="https://cdn.example.com">
</head>
<body>
  <h1>Content</h1>
  
  <!-- 5️⃣ ASYNC (Non-critical, independent) -->
  <script src="analytics.js" async></script>
  
  <!-- 6️⃣ DEFER (Non-critical, needs DOM) -->
  <script src="app.js" defer></script>
</body>
</html>
```

**Complete Optimization Strategy:**

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 1: Inline Critical CSS (Non-blocking) -->
  <!-- ═══════════════════════════════════════════ -->
  <style>
    /* Only above-the-fold styles (~5KB max) */
    body { margin: 0; font-family: system-ui; }
    .header { 
      height: 60px; 
      background: #000; 
      color: #fff;
      display: flex;
      align-items: center;
      padding: 0 20px;
    }
    .hero {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    }
  </style>
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 2: Preload Critical Resources -->
  <!-- ═══════════════════════════════════════════ -->
  <link rel="preload" href="hero-bg.jpg" as="image">
  <link rel="preload" href="main.woff2" as="font" type="font/woff2" crossorigin>
  <link rel="preload" href="app.js" as="script">
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 3: Load Full CSS Async (Non-blocking) -->
  <!-- ═══════════════════════════════════════════ -->
  <link rel="stylesheet" href="styles.css" media="print" onload="this.media='all'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 4: DNS Prefetch for Third-party -->
  <!-- ═══════════════════════════════════════════ -->
  <link rel="dns-prefetch" href="https://www.google-analytics.com">
  <link rel="dns-prefetch" href="https://cdn.jsdelivr.net">
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 5: Font Optimization -->
  <!-- ═══════════════════════════════════════════ -->
  <style>
    @font-face {
      font-family: 'CustomFont';
      src: url('font.woff2') format('woff2');
      font-display: swap; /* ✅ Prevents blocking */
      font-weight: 400;
      font-style: normal;
    }
  </style>
</head>
<body>
  <!-- ═══════════════════════════════════════════ -->
  <!-- Above-the-fold Content (Renders Immediately) -->
  <!-- ═══════════════════════════════════════════ -->
  <header class="header">
    <img src="logo.svg" alt="Logo" width="120" height="40">
  </header>
  
  <section class="hero">
    <h1>Welcome to Our App</h1>
  </section>
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- Below-the-fold Content (Lazy Load) -->
  <!-- ═══════════════════════════════════════════ -->
  <section class="features">
    <!-- ✅ Lazy-loaded images -->
    <img src="placeholder.jpg" 
         data-src="feature1.jpg" 
         loading="lazy" 
         alt="Feature 1">
    
    <img src="placeholder.jpg" 
         data-src="feature2.jpg" 
         loading="lazy" 
         alt="Feature 2">
  </section>
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 6: Async Scripts (Non-blocking) -->
  <!-- ═══════════════════════════════════════════ -->
  <script src="https://www.googletagmanager.com/gtag/js" async></script>
  <script src="chat-widget.js" async></script>
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 7: Defer Scripts (Non-blocking) -->
  <!-- ═══════════════════════════════════════════ -->
  <script src="app.js" defer></script>
  <script src="components.js" defer></script>
  
  <!-- ═══════════════════════════════════════════ -->
  <!-- STEP 8: Prefetch Next Page Resources -->
  <!-- ═══════════════════════════════════════════ -->
  <link rel="prefetch" href="about-page.css">
  <link rel="prefetch" href="about-page.js">
</body>
</html>
```

**JavaScript Example - Dynamic Resource Loading:**

```javascript
// ✅ Smart resource loader
class ResourceLoader {
  constructor() {
    this.loadedResources = new Set();
  }
  
  // Non-blocking CSS loader
  loadCSS(href, priority = 'low') {
    if (this.loadedResources.has(href)) return;
    
    const link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = href;
    
    if (priority === 'high') {
      // Blocking load for critical CSS
      document.head.appendChild(link);
    } else {
      // Non-blocking load
      link.media = 'print';
      link.onload = () => { link.media = 'all'; };
      document.head.appendChild(link);
    }
    
    this.loadedResources.add(href);
  }
  
  // Non-blocking script loader
  loadScript(src, async = true) {
    if (this.loadedResources.has(src)) return Promise.resolve();
    
    return new Promise((resolve, reject) => {
      const script = document.createElement('script');
      script.src = src;
      script.async = async;
      
      script.onload = () => {
        this.loadedResources.add(src);
        resolve();
      };
      
      script.onerror = () => reject(new Error(`Failed to load ${src}`));
      
      document.body.appendChild(script);
    });
  }
  
  // Lazy load images
  lazyLoadImages() {
    const images = document.querySelectorAll('img[data-src]');
    
    const imageObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const img = entry.target;
          img.src = img.dataset.src;
          img.removeAttribute('data-src');
          imageObserver.unobserve(img);
        }
      });
    }, {
      rootMargin: '50px' // Start loading 50px before visible
    });
    
    images.forEach(img => imageObserver.observe(img));
  }
  
  // Prefetch resources for next navigation
  prefetchNextPage(urls) {
    urls.forEach(url => {
      const link = document.createElement('link');
      link.rel = 'prefetch';
      link.href = url;
      document.head.appendChild(link);
    });
  }
}

// Usage
const loader = new ResourceLoader();

// Load critical CSS first (blocking)
loader.loadCSS('critical.css', 'high');

// Load non-critical CSS async (non-blocking)
loader.loadCSS('components.css', 'low');
loader.loadCSS('animations.css', 'low');

// Load analytics async (non-blocking)
loader.loadScript('analytics.js', true);

// Setup lazy loading for images (non-blocking)
loader.lazyLoadImages();

// Prefetch next page resources (non-blocking)
loader.prefetchNextPage([
  '/about',
  '/about.css',
  '/about.js'
]);
```

**Real Performance Metrics:**

```javascript
// Measure blocking vs non-blocking impact
const measureResourceTiming = () => {
  const resources = performance.getEntriesByType('resource');
  
  const blocking = resources.filter(r => 
    (r.initiatorType === 'link' && r.name.includes('.css')) ||
    (r.initiatorType === 'script' && !r.name.includes('async'))
  );
  
  const nonBlocking = resources.filter(r =>
    r.initiatorType === 'img' ||
    (r.initiatorType === 'script' && r.name.includes('async'))
  );
  
  console.log('⚠️ BLOCKING Resources:', blocking.length);
  blocking.forEach(r => {
    console.log(`  - ${r.name}: ${r.duration.toFixed(2)}ms`);
  });
  
  console.log('\n✅ NON-BLOCKING Resources:', nonBlocking.length);
  nonBlocking.forEach(r => {
    console.log(`  - ${r.name}: ${r.duration.toFixed(2)}ms`);
  });
  
  // Calculate First Contentful Paint
  const fcp = performance.getEntriesByName('first-contentful-paint')[0];
  console.log(`\n🎨 First Contentful Paint: ${fcp?.startTime.toFixed(2)}ms`);
};

// Run after page load
window.addEventListener('load', () => {
  setTimeout(measureResourceTiming, 1000);
});
```

**Chrome DevTools - Network Tab Analysis:**

```
DevTools → Network → Filter by:
1. CSS files (typically blocking)
2. JS files (check async/defer in Waterfall)
3. Images (should be non-blocking)

Look for:
- Resources with "High" priority = potential blocking
- Resources with "Low" priority = non-blocking
- Large "Waiting (TTFB)" = server delay
- Parallel downloads = efficient loading
```

**Resource Hints Cheat Sheet:**

```html
<!-- ═══════════════════════════════════════════ -->
<!-- Resource Hints Priority Order -->
<!-- ═══════════════════════════════════════════ -->

<!-- 1. dns-prefetch: Resolve DNS early -->
<link rel="dns-prefetch" href="https://fonts.googleapis.com">

<!-- 2. preconnect: DNS + TCP + TLS handshake -->
<link rel="preconnect" href="https://api.example.com">

<!-- 3. prefetch: Low priority, for next navigation -->
<link rel="prefetch" href="next-page.js">

<!-- 4. preload: High priority, for current page -->
<link rel="preload" href="critical.css" as="style">

<!-- 5. prerender: Pre-render entire page (use carefully) -->
<link rel="prerender" href="next-page.html">
```

**React Component Example:**

```jsx
import React, { useEffect, useState } from 'react';

const OptimizedApp = () => {
  const [belowFoldLoaded, setBelowFoldLoaded] = useState(false);
  
  useEffect(() => {
    // Load non-blocking resources after initial render
    const loadNonCriticalResources = () => {
      // Load additional CSS
      const link = document.createElement('link');
      link.rel = 'stylesheet';
      link.href = 'non-critical.css';
      link.media = 'print';
      link.onload = () => { link.media = 'all'; };
      document.head.appendChild(link);
      
      // Load analytics script (async)
      const script = document.createElement('script');
      script.src = 'analytics.js';
      script.async = true;
      document.body.appendChild(script);
    };
    
    // Load after first paint
    if (window.requestIdleCallback) {
      requestIdleCallback(loadNonCriticalResources);
    } else {
      setTimeout(loadNonCriticalResources, 1);
    }
    
    // Setup intersection observer for below-fold
    const observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting) {
        setBelowFoldLoaded(true);
      }
    });
    
    const trigger = document.querySelector('#below-fold-trigger');
    if (trigger) observer.observe(trigger);
    
    return () => observer.disconnect();
  }, []);
  
  return (
    <div>
      {/* Above-the-fold: Renders immediately */}
      <header>
        <img src="logo.svg" alt="Logo" width="120" height="40" />
      </header>
      
      <section className="hero">
        <h1>Welcome to Our App</h1>
        <p>This content loads immediately</p>
      </section>
      
      {/* Trigger point for lazy loading */}
      <div id="below-fold-trigger" />
      
      {/* Below-the-fold: Lazy loaded */}
      {belowFoldLoaded && (
        <>
          <section className="features">
            <img 
              src="feature.jpg" 
              alt="Feature" 
              loading="lazy" 
            />
          </section>
          
          <section className="testimonials">
            {/* Heavy content */}
          </section>
        </>
      )}
    </div>
  );
};

export default OptimizedApp;
```

**Webpack Configuration for Optimization:**

```javascript
// webpack.config.js
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        // Vendor code (non-blocking)
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          priority: 10
        },
        // Common code (non-blocking)
        common: {
          minChunks: 2,
          priority: 5,
          reuseExistingChunk: true
        }
      }
    }
  },
  
  plugins: [
    // Preload critical chunks
    new PreloadWebpackPlugin({
      rel: 'preload',
      include: 'initial'
    }),
    
    // Prefetch non-critical chunks
    new PreloadWebpackPlugin({
      rel: 'prefetch',
      include: 'asyncChunks'
    })
  ]
};
```

**Summary - Blocking vs Non-blocking Decision Tree:**

```
Is the resource CRITICAL for first paint?
│
├─ YES → Make it NON-BLOCKING if possible
│   ├─ CSS → Inline critical, async load rest
│   ├─ JS → Defer or async
│   └─ Fonts → Use font-display: swap
│
└─ NO → Definitely make it NON-BLOCKING
    ├─ Images → Lazy load
    ├─ Analytics → Async
    ├─ Videos → Lazy load with preload="none"
    └─ Third-party scripts → Async
```

---
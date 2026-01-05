# 🔥 JavaScript Events - Complete Interview Guide

[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Interview Ready](https://img.shields.io/badge/Interview-Ready-success?style=for-the-badge)](https://github.com)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=for-the-badge)](https://github.com)

> **A comprehensive guide to master JavaScript Events for interviews with real-world examples and tricky questions.**

---

## 📚 Table of Contents

1. [Event Flow](#1️⃣-event-flow)
2. [Event Control](#2️⃣-event-control)
3. [Event Handling Concepts](#3️⃣-event-handling-concepts)
4. [Mouse & Keyboard Events](#4️⃣-mouse--keyboard-events)
5. [Form & Input Events](#5️⃣-form--input-events)
6. [Browser & Window Events](#6️⃣-browser--window-events)
7. [Event Object](#7️⃣-event-object)

---

## 1️⃣ Event Flow

### 🎯 Core Concepts

Event Flow defines the order in which events are received on the page. There are **3 phases**:

```
Window → Document → HTML → Body → Parent → TARGET → Parent → Body → HTML → Document → Window
  ↓ Capturing Phase ↓              ↓ Bubbling Phase ↑
```

#### **Event Bubbling**
Events bubble up from the target element to its ancestors.

```javascript
// Example: Event Bubbling
document.getElementById('child').addEventListener('click', () => {
  console.log('Child clicked');
});

document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent clicked');
});

// Output when child is clicked:
// Child clicked
// Parent clicked (bubbles up!)
```

#### **Event Capturing (Trickling)**
Events trickle down from the window to the target element.

```javascript
// Example: Event Capturing
document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent clicked');
}, true); // { capture: true }

document.getElementById('child').addEventListener('click', () => {
  console.log('Child clicked');
});

// Output when child is clicked:
// Parent clicked (captured first!)
// Child clicked
```

#### **Event Phases**

1. **Capturing Phase**: Event travels from Window to target
2. **Target Phase**: Event reaches the target element
3. **Bubbling Phase**: Event bubbles back up to Window

---

### 💡 Interview Questions

#### **Q1: What happens when you click a nested button? Explain the event flow.**

```html
<div id="grandparent">
  <div id="parent">
    <button id="child">Click Me</button>
  </div>
</div>
```

```javascript
const gp = document.getElementById('grandparent');
const p = document.getElementById('parent');
const c = document.getElementById('child');

gp.addEventListener('click', () => console.log('GP Capture'), true);
p.addEventListener('click', () => console.log('P Capture'), true);
c.addEventListener('click', () => console.log('C Capture'), true);

gp.addEventListener('click', () => console.log('GP Bubble'));
p.addEventListener('click', () => console.log('P Bubble'));
c.addEventListener('click', () => console.log('C Bubble'));

// Output when button clicked:
// GP Capture (capturing down)
// P Capture
// C Capture
// C Bubble (target phase + bubbling up)
// P Bubble
// GP Bubble
```

**Why this happens**: Events follow the DOM tree path: capturing phase (window → target), then bubbling phase (target → window).

---

#### **Q2: How to determine which phase an event is in?**

```javascript
element.addEventListener('click', (event) => {
  console.log(event.eventPhase);
  // 1 = CAPTURING_PHASE
  // 2 = AT_TARGET
  // 3 = BUBBLING_PHASE
});
```

**Use case**: Useful when debugging complex event flows or implementing custom event systems.

---

#### **Q3: What is event.target vs event.currentTarget?**

```javascript
document.getElementById('parent').addEventListener('click', (event) => {
  console.log('target:', event.target.id);        // Element that triggered
  console.log('currentTarget:', event.currentTarget.id); // Element with listener
});

// Clicking child:
// target: child (actual element clicked)
// currentTarget: parent (element with the listener)
```

**Why it matters**: `event.target` = where event originated, `event.currentTarget` = where listener is attached. Critical for event delegation!

---

## 2️⃣ Event Control

### 🎯 Core Concepts

#### **stopPropagation()**
Stops the event from bubbling/capturing further.

```javascript
document.getElementById('child').addEventListener('click', (e) => {
  e.stopPropagation(); // Parent won't receive event
  console.log('Child clicked');
});

document.getElementById('parent').addEventListener('click', () => {
  console.log('Parent clicked'); // Won't execute!
});
```

#### **stopImmediatePropagation()**
Stops all other listeners on the same element AND stops propagation.

```javascript
element.addEventListener('click', (e) => {
  e.stopImmediatePropagation();
  console.log('First listener');
});

element.addEventListener('click', () => {
  console.log('Second listener'); // Won't execute!
});
```

#### **preventDefault()**
Prevents the default browser action.

```javascript
document.querySelector('a').addEventListener('click', (e) => {
  e.preventDefault(); // Link won't navigate
  console.log('Link clicked but not followed');
});
```

---

### 💡 Interview Questions

#### **Q1: What's the difference between stopPropagation and preventDefault?**

```javascript
// Example: Form with nested elements
document.querySelector('form').addEventListener('submit', (e) => {
  e.preventDefault();     // Stops form submission
  e.stopPropagation();   // Stops event bubbling
});

document.body.addEventListener('submit', () => {
  console.log('Body submit'); // Won't execute due to stopPropagation
});
```

**Key difference**:
- `preventDefault()` = stops **browser's default action** (form submit, link navigation)
- `stopPropagation()` = stops **event flow** (bubbling/capturing)

**When to use**:
- Use `preventDefault()` when you want to override browser behavior (custom form validation)
- Use `stopPropagation()` when you want to prevent parent handlers from executing

---

#### **Q2: stopPropagation vs stopImmediatePropagation - explain with example**

```javascript
const btn = document.querySelector('button');

btn.addEventListener('click', (e) => {
  console.log('Listener 1');
  e.stopPropagation(); // Other listeners on THIS element still run
});

btn.addEventListener('click', () => {
  console.log('Listener 2'); // ✅ This executes
});

// VS

btn.addEventListener('click', (e) => {
  console.log('Listener 1');
  e.stopImmediatePropagation(); // No other listeners run
});

btn.addEventListener('click', () => {
  console.log('Listener 2'); // ❌ This won't execute
});
```

**When to use**: `stopImmediatePropagation()` when you want to prevent ALL other handlers, even on the same element (e.g., preventing multiple modal triggers).

---

#### **Q3: Can you call preventDefault on a non-cancelable event?**

```javascript
document.addEventListener('scroll', (e) => {
  e.preventDefault(); // ❌ Won't work! Scroll is non-cancelable
  console.log(e.cancelable); // false
});

document.querySelector('a').addEventListener('click', (e) => {
  e.preventDefault(); // ✅ Works! Click is cancelable
  console.log(e.cancelable); // true
});
```

**Why this matters**: Some events like `scroll`, `mouseenter`, `mouseleave` are non-cancelable. Always check `event.cancelable` before calling `preventDefault()`.

---

## 3️⃣ Event Handling Concepts

### 🎯 Core Concepts

#### **Event Delegation ⭐ (Most Asked!)**
Attach one listener to a parent instead of multiple listeners to children.

```javascript
// ❌ Bad: Multiple listeners
document.querySelectorAll('.item').forEach(item => {
  item.addEventListener('click', () => {
    console.log(item.textContent);
  });
});

// ✅ Good: Single listener with delegation
document.getElementById('list').addEventListener('click', (e) => {
  if (e.target.classList.contains('item')) {
    console.log(e.target.textContent);
  }
});
```

**Benefits**:
- Better performance (fewer listeners)
- Works with dynamically added elements
- Less memory usage

#### **addEventListener Options**

```javascript
element.addEventListener('click', handler, {
  capture: true,   // Use capturing phase
  once: true,      // Auto-remove after first trigger
  passive: true    // Never calls preventDefault (better scroll perf)
});
```

---

### 💡 Interview Questions

#### **Q1: Implement a to-do list with event delegation where new items can be deleted**

```javascript
const todoList = document.getElementById('todoList');
const addBtn = document.getElementById('addBtn');

// Event Delegation for delete
todoList.addEventListener('click', (e) => {
  if (e.target.classList.contains('delete-btn')) {
    e.target.parentElement.remove();
  }
});

// Add new items dynamically
addBtn.addEventListener('click', () => {
  const li = document.createElement('li');
  li.innerHTML = `Task ${Date.now()} <button class="delete-btn">Delete</button>`;
  todoList.appendChild(li);
  // No need to attach new listeners! Delegation handles it.
});
```

**Why this works**: The click listener on `todoList` catches all clicks on descendants, even those added after page load.

**When to use**: Dynamic content (comments, chat messages, infinite scroll items).

---

#### **Q2: What's the difference between addEventListener and inline events?**

```javascript
// ❌ Inline: Only one handler allowed
<button onclick="alert('Clicked')">Click</button>

// ✅ addEventListener: Multiple handlers possible
const btn = document.querySelector('button');
btn.addEventListener('click', () => console.log('Handler 1'));
btn.addEventListener('click', () => console.log('Handler 2'));
// Both execute!
```

**Key differences**:
- **Inline**: Hard to maintain, mixes HTML/JS, only one handler
- **addEventListener**: Separates concerns, multiple handlers, can remove listeners
- **addEventListener** supports options (`capture`, `once`, `passive`)

---

#### **Q3: Explain `{ passive: true }` and why it matters**

```javascript
// ❌ Without passive: Browser waits to check preventDefault
document.addEventListener('scroll', (e) => {
  // e.preventDefault(); might be called
  console.log('Scrolling...');
});

// ✅ With passive: Browser scrolls immediately (smoother)
document.addEventListener('scroll', (e) => {
  console.log('Scrolling...');
}, { passive: true });
// Can't call e.preventDefault() here
```

**Why it triggers**: Passive listeners promise not to call `preventDefault()`, so the browser doesn't need to wait to see if you will—it can scroll immediately.

**When to use**: Scroll/touch events for better performance (especially on mobile).

---

## 4️⃣ Mouse & Keyboard Events

### 🎯 Core Concepts

#### **Mouse Events**

```javascript
// Click Events
element.addEventListener('click', () => {}); // Single click
element.addEventListener('dblclick', () => {}); // Double click

// Mouse Button Events
element.addEventListener('mousedown', () => {}); // Button pressed
element.addEventListener('mouseup', () => {}); // Button released

// Mouse Movement
element.addEventListener('mouseenter', () => {}); // Enters element (no bubble)
element.addEventListener('mouseleave', () => {}); // Leaves element (no bubble)
element.addEventListener('mouseover', () => {}); // Enters element (bubbles)
element.addEventListener('mouseout', () => {}); // Leaves element (bubbles)
```

#### **Keyboard Events**

```javascript
document.addEventListener('keydown', (e) => {
  console.log(e.key);      // 'a', 'Enter', 'Shift'
  console.log(e.keyCode);  // 65, 13, 16 (deprecated)
  console.log(e.code);     // 'KeyA', 'Enter', 'ShiftLeft'
});
```

---

### 💡 Interview Questions

#### **Q1: mouseenter vs mouseover - what's the difference?**

```javascript
// mouseenter: Triggers only when entering parent (no bubble)
parent.addEventListener('mouseenter', () => {
  console.log('Entered parent'); // Fires once
});

// mouseover: Triggers when entering parent OR children (bubbles)
parent.addEventListener('mouseover', (e) => {
  console.log('Over:', e.target.id); // Fires for parent AND children!
});
```

```html
<div id="parent" style="padding: 50px; background: lightblue;">
  Parent
  <div id="child" style="padding: 20px; background: lightcoral;">Child</div>
</div>
```

**Why this happens**: 
- `mouseenter/leave` don't bubble (only fire for the element itself)
- `mouseover/out` bubble (fire for element and descendants)

**When to use**: Use `mouseenter/leave` for tooltips, `mouseover/out` when you need to detect child hovers.

---

#### **Q2: How to detect specific key combinations (Ctrl+S)?**

```javascript
document.addEventListener('keydown', (e) => {
  // Save shortcut
  if (e.ctrlKey && e.key === 's') {
    e.preventDefault(); // Prevent browser save dialog
    console.log('Custom save triggered!');
  }
  
  // Alt + Shift + K
  if (e.altKey && e.shiftKey && e.key === 'K') {
    console.log('Secret shortcut!');
  }
});
```

**Why this matters**: Essential for building keyboard shortcuts in web apps (Gmail, Notion, VS Code).

**What triggers it**: `e.ctrlKey`, `e.altKey`, `e.shiftKey`, `e.metaKey` (Cmd on Mac) are boolean modifiers.

---

#### **Q3: event.key vs event.keyCode vs event.code**

```javascript
document.addEventListener('keydown', (e) => {
  console.log('key:', e.key);      // 'A' or 'a' (depends on Shift)
  console.log('keyCode:', e.keyCode); // 65 (deprecated!)
  console.log('code:', e.code);    // 'KeyA' (physical key)
});
```

**Key differences**:
- `e.key`: Character value ('a', 'A', 'Enter', '1', '!')
- `e.keyCode`: Numeric code (deprecated, avoid!)
- `e.code`: Physical key location ('KeyA', 'Digit1', 'ArrowUp')

**When to use**: Use `e.key` for character detection, `e.code` for game controls (WASD regardless of keyboard layout).

---

## 5️⃣ Form & Input Events

### 🎯 Core Concepts

```javascript
// onChange: Fires when value changes AND focus is lost
input.addEventListener('change', (e) => {
  console.log('Final value:', e.target.value);
});

// onInput: Fires on every keystroke
input.addEventListener('input', (e) => {
  console.log('Current value:', e.target.value);
});

// onSubmit: Form submission
form.addEventListener('submit', (e) => {
  e.preventDefault(); // Prevent page reload
  const formData = new FormData(e.target);
  console.log(Object.fromEntries(formData));
});

// onFocus / onBlur
input.addEventListener('focus', () => console.log('Input focused'));
input.addEventListener('blur', () => console.log('Input lost focus'));
```

---

### 💡 Interview Questions

#### **Q1: onChange vs onInput - when does each fire?**

```javascript
const input = document.querySelector('input');

input.addEventListener('input', () => {
  console.log('Input event'); // Fires on EVERY change
});

input.addEventListener('change', () => {
  console.log('Change event'); // Fires when FOCUS IS LOST
});

// User types "abc":
// Input event (a)
// Input event (ab)
// Input event (abc)
// [User clicks outside]
// Change event
```

**Why this happens**:
- `input`: Real-time updates (use for search, live validation)
- `change`: Confirms final value (use for dropdowns, checkboxes)

**When to use**: Use `input` for instant feedback (character counter), `change` for batch updates (form validation on blur).

---

#### **Q2: How to prevent form submission and validate data?**

```javascript
const form = document.querySelector('form');

form.addEventListener('submit', (e) => {
  e.preventDefault(); // Stop page reload
  
  const email = form.querySelector('#email').value;
  
  if (!email.includes('@')) {
    alert('Invalid email!');
    return; // Stop submission
  }
  
  // Submit via AJAX
  fetch('/api/submit', {
    method: 'POST',
    body: new FormData(form)
  });
});
```

**What triggers it**: Form submission via Enter key, submit button, or `form.submit()` programmatically.

**Why this matters**: Modern SPAs prevent default submission to handle data with JavaScript (AJAX, fetch, validation).

---

#### **Q3: Controlled vs Uncontrolled inputs (React context)**

```javascript
// ❌ Uncontrolled: DOM manages state
<input type="text" defaultValue="Hello" />

// ✅ Controlled: React state manages value
const [value, setValue] = useState('');
<input 
  type="text" 
  value={value} 
  onChange={(e) => setValue(e.target.value)} 
/>
```

**Key difference**:
- **Uncontrolled**: DOM is source of truth (use refs to access)
- **Controlled**: React state is source of truth (full control)

**When to use**: Controlled for validation/formatting, uncontrolled for simple forms or file inputs.

---

## 6️⃣ Browser & Window Events

### 🎯 Core Concepts

```javascript
// DOMContentLoaded: HTML parsed, DOM ready (no images/styles)
document.addEventListener('DOMContentLoaded', () => {
  console.log('DOM ready!');
});

// load: Everything loaded (images, styles, scripts)
window.addEventListener('load', () => {
  console.log('Page fully loaded!');
});

// resize: Window size changes
window.addEventListener('resize', () => {
  console.log('Width:', window.innerWidth);
});

// scroll: Page scrolls
window.addEventListener('scroll', () => {
  console.log('Scroll position:', window.scrollY);
});

// beforeunload: User leaving page
window.addEventListener('beforeunload', (e) => {
  e.preventDefault();
  e.returnValue = ''; // Show confirmation dialog
});
```

---

### 💡 Interview Questions

#### **Q1: DOMContentLoaded vs load - what's the difference?**

```javascript
console.log('1. Script started');

document.addEventListener('DOMContentLoaded', () => {
  console.log('2. DOMContentLoaded - DOM ready');
});

window.addEventListener('load', () => {
  console.log('3. Load - Everything ready');
});

// Output:
// 1. Script started
// 2. DOMContentLoaded - DOM ready (HTML parsed)
// 3. Load - Everything ready (images, CSS loaded)
```

**Timeline**:
1. HTML parsing → `DOMContentLoaded`
2. Images/CSS loading → `load`

**When to use**: Use `DOMContentLoaded` to manipulate DOM early, `load` when you need image dimensions.

---

#### **Q2: How to implement infinite scroll with scroll event?**

```javascript
let page = 1;
let loading = false;

window.addEventListener('scroll', () => {
  // Check if near bottom
  const { scrollTop, scrollHeight, clientHeight } = document.documentElement;
  
  if (scrollTop + clientHeight >= scrollHeight - 100 && !loading) {
    loading = true;
    loadMoreItems();
  }
});

function loadMoreItems() {
  fetch(`/api/items?page=${page}`)
    .then(res => res.json())
    .then(data => {
      appendItems(data);
      page++;
      loading = false;
    });
}
```

**What triggers it**: When user scrolls within 100px of bottom, fetch more items.

**Why this matters**: Common in social media feeds (Twitter, Instagram). Use `IntersectionObserver` for better performance.

---

#### **Q3: How to warn users before leaving with unsaved changes?**

```javascript
let hasUnsavedChanges = false;

document.querySelector('textarea').addEventListener('input', () => {
  hasUnsavedChanges = true;
});

document.querySelector('form').addEventListener('submit', () => {
  hasUnsavedChanges = false; // Saved!
});

window.addEventListener('beforeunload', (e) => {
  if (hasUnsavedChanges) {
    e.preventDefault();
    e.returnValue = ''; // Modern browsers show generic message
    return ''; // For older browsers
  }
});
```

**When to use**: Forms, text editors, any app where users might lose work.

**What triggers it**: User closes tab, navigates away, refreshes page.

---

## 7️⃣ Event Object

### 🎯 Core Concepts

```javascript
element.addEventListener('click', (event) => {
  console.log('target:', event.target);           // Element clicked
  console.log('currentTarget:', event.currentTarget); // Element with listener
  console.log('type:', event.type);               // 'click'
  console.log('timeStamp:', event.timeStamp);     // Time of event
  console.log('bubbles:', event.bubbles);         // true/false
  console.log('cancelable:', event.cancelable);   // Can preventDefault?
  
  event.preventDefault();  // Stop default action
  event.stopPropagation(); // Stop bubbling
});
```

---

### 💡 Interview Questions

#### **Q1: event.target vs event.currentTarget in event delegation**

```javascript
document.getElementById('list').addEventListener('click', (e) => {
  console.log('Clicked:', e.target.tagName);        // LI or BUTTON
  console.log('Listener on:', e.currentTarget.id);  // list
  
  // Useful for delegation
  if (e.target.tagName === 'BUTTON') {
    console.log('Button clicked!');
  }
});
```

```html
<ul id="list">
  <li>Item 1 <button>Delete</button></li>
  <li>Item 2 <button>Delete</button></li>
</ul>
```

**Key insight**: In delegation, `target` changes (clicked element), `currentTarget` stays the same (listener element).

---

#### **Q2: How to get mouse position from event?**

```javascript
document.addEventListener('click', (e) => {
  console.log('Client X/Y:', e.clientX, e.clientY);   // Relative to viewport
  console.log('Page X/Y:', e.pageX, e.pageY);         // Relative to document
  console.log('Screen X/Y:', e.screenX, e.screenY);   // Relative to screen
  console.log('Offset X/Y:', e.offsetX, e.offsetY);   // Relative to target
});
```

**Use cases**:
- `clientX/Y`: Custom tooltips, context menus
- `pageX/Y`: Drawing apps, drag-and-drop
- `offsetX/Y`: Heatmaps, click tracking

---

#### **Q3: How to distinguish between different mouse buttons?**

```javascript
document.addEventListener('mousedown', (e) => {
  switch(e.button) {
    case 0: console.log('Left click'); break;
    case 1: console.log('Middle click'); break;
    case 2: console.log('Right click'); break;
  }
  
  // Or check multiple buttons
  if (e.buttons === 3) {
    console.log('Left + Right pressed together!');
  }
});
```

**What triggers it**: `e.button` = which button pressed, `e.buttons` = bitmask of all buttons currently pressed.

---

## 🎓 Bonus: Common Interview Patterns

### Pattern 1: Debounced Search

```javascript
let timeout;
searchInput.addEventListener('input', (e) => {
  clearTimeout(timeout);
  timeout = setTimeout(() => {
    console.log('Search:', e.target.value);
    // API call here
  }, 500);
});
```

### Pattern 2: Custom Event

```javascript
// Create and dispatch
const myEvent = new CustomEvent('userLogin', {
  detail: { userId: 123 }
});
document.dispatchEvent(myEvent);

// Listen
document.addEventListener('userLogin', (e) => {
  console.log('User logged in:', e.detail.userId);
});
```

### Pattern 3: Once Listener

```javascript
button.addEventListener('click', () => {
  console.log('Clicked once!');
}, { once: true }); // Auto-removed after first click
```

---

## 📝 Quick Reference Cheatsheet

| Event | When It Fires | Bubbles | Cancelable |
|-------|--------------|---------|------------|
| `click` | Mouse click | ✅ | ✅ |
| `input` | Value changes | ✅ | ❌ |
| `change` | Value changes + blur | ✅ | ❌ |
| `submit` | Form submitted | ✅ | ✅ |
| `scroll` | Element scrolls | ❌ | ❌ |
| `keydown` | Key pressed | ✅ | ✅ |
| `mouseenter` | Mouse enters | ❌ | ❌ |
| `mouseover` | Mouse enters | ✅ | ❌ |
| `DOMContentLoaded` | DOM ready | ❌ | ❌ |
| `load` | Everything loaded | ❌ | ❌ |

---

## 🚀 Pro Tips for Interviews

1. **Always explain WHY**: Don't just say "bubbling happens" - explain it goes from target → ancestors
2. **Use visual examples**: Draw the DOM tree when explaining event flow
3. **Mention performance**: Event delegation reduces memory, passive listeners improve scroll
4. **Know when NOT to use**: `stopPropagation()` can break third-party libraries
5. **Real-world context**: "In production, I'd use debounce for search inputs"

---

## 🤝 Contributing

Found a mistake? Want to add more examples? PRs are welcome!

---

## 📧 Contact

Questions? Open an issue or reach out!

---

**⭐ Star this repo if it helped you crack your interview!**
# JavaScript Execution Context, Event Loop & Error Types - Complete Guide

## Table of Contents

- [Execution Context](#execution-context)
- [Call Stack](#call-stack)
- [Event Loop Architecture](#event-loop-architecture)
- [Microtask Queue vs Task Queue](#microtask-queue-vs-task-queue)
- [Hoisting & TDZ](#hoisting--tdz)
- [JavaScript Error Types](#javascript-error-types)
- [Execution Order Examples](#execution-order-examples)
- [Practice Questions](#practice-questions)
- [Key Takeaways](#key-takeaways)

---

## Execution Context

**Execution Context** JavaScript code ke execute hone ka environment hai. Jab bhi function run hota hai, ek naya execution context create hota hai.

### Types of Execution Context:

1. **Global Execution Context (GEC)** - Default context, code start hone par
2. **Function Execution Context (FEC)** - Har function call par
3. **Eval Execution Context** - eval() function ke andar

### Execution Context ke 2 Phases:

#### 1. Creation Phase (Memory Allocation)
- Variables ko `undefined` assign hota hai
- Functions pura code memory mein store hota hai
- `this` binding hoti hai

#### 2. Execution Phase
- Code line by line execute hota hai
- Variables ko actual values milti hai

---

## Call Stack

**Call Stack** ek data structure hai jo **LIFO (Last In First Out)** principle pe kaam karta hai.

```javascript
function first() {
    console.log("First function");
    second();
    console.log("First function end");
}

function second() {
    console.log("Second function");
    third();
}

function third() {
    console.log("Third function");
}

first();

// Call Stack Flow:
// 1. Global Execution Context
// 2. first() pushed
// 3. second() pushed
// 4. third() pushed
// 5. third() popped (completed)
// 6. second() popped
// 7. first() popped
```

---

## Event Loop Architecture

**Event Loop** JavaScript ko **asynchronous** banata hai, jabki JavaScript **single-threaded** hai.

### Components:
1. **Call Stack** - Synchronous code execute hota hai
2. **Microtask Queue** - High priority async tasks
3. **Task Queue (Callback Queue)** - Low priority async tasks
4. **Web APIs** - Browser provide karta hai (setTimeout, fetch, DOM events)

### Event Loop ka Kaam:
```
1. Call Stack check karo - Empty hai?
2. Agar empty hai, to Microtask Queue check karo
3. Saare microtasks execute karo (queue empty ho jaye)
4. Ab Task Queue se EK task lo aur execute karo
5. Repeat
```

### Visual Representation
```
┌───────────────────────────┐
│      Call Stack           │ ← Synchronous code
└───────────────────────────┘
            ↓
┌───────────────────────────┐
│   process.nextTick()      │ ← Highest Priority (Node.js)
└───────────────────────────┘
            ↓
┌───────────────────────────┐
│   Microtask Queue         │ ← Promises, queueMicrotask
│   - Promise.then()        │
│   - async/await           │
└───────────────────────────┘
            ↓
┌───────────────────────────┐
│   Task Queue              │ ← setTimeout, setImmediate, I/O
│   - setTimeout()          │
│   - setImmediate()        │
│   - fs.readFile()         │
└───────────────────────────┘
```

---

## Microtask Queue vs Task Queue

### **Microtask Queue (High Priority)**
Ye tasks **Task Queue se pehle** execute hote hain:

1. **`Promise.then()/.catch()/.finally()`**
2. **`process.nextTick()`** (Node.js - highest priority)
3. **`queueMicrotask()`**
4. **`MutationObserver`** (Browser)
5. **`async/await`** (internally Promise use karta hai)

### **Task Queue / Callback Queue (Low Priority)**
Ye tasks **microtasks ke baad** execute hote hain:

1. **`setTimeout()`**
2. **`setInterval()`**
3. **`setImmediate()`** (Node.js - next event loop iteration)
4. **I/O operations** (fs.readFile, network requests)
5. **UI rendering events** (click, scroll)
6. **`requestAnimationFrame()`** (Browser)

### Priority Order (Highest to Lowest):
```
1. Call Stack (Synchronous code)
2. process.nextTick() (Node.js only)
3. Microtask Queue (Promises, queueMicrotask)
4. Task Queue (setTimeout, setImmediate, I/O)
```

---

## Hoisting & TDZ

### Hoisting kya hai?
JavaScript mein variables aur functions ko unke declaration se pehle access kar sakte hain (creation phase mein memory allocate hoti hai).

```javascript
console.log(x); // undefined (hoisted)
var x = 10;

greet(); // "Hello" (function hoisted with body)
function greet() {
    console.log("Hello");
}
```

### let aur const mein Hoisting hoti hai ya nahi?
**Jawab:** Haan, hoisting hoti hai! Lekin **TDZ (Temporal Dead Zone)** ke karan access nahi kar sakte.

### TDZ (Temporal Dead Zone)
TDZ wo time period hai jab variable declare ho chuka hai (hoisted) lekin initialize nahi hua hai.

```javascript
// TDZ starts
console.log(a); // ❌ ReferenceError: Cannot access 'a' before initialization
// TDZ continues
let a = 10; // TDZ ends
console.log(a); // ✅ 10
```

### Hoisting Comparison:

| Feature | var | let/const |
|---------|-----|-----------|
| Hoisting | ✅ Yes | ✅ Yes |
| Default Value | `undefined` | No value (TDZ) |
| Access before declaration | ✅ Returns `undefined` | ❌ ReferenceError |
| TDZ | ❌ No | ✅ Yes |

```javascript
// var example
console.log(x); // undefined
var x = 5;

// let example
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 5;

// const example
console.log(z); // ReferenceError: Cannot access 'z' before initialization
const z = 5;
```

---

## JavaScript Error Types

### 1. ReferenceError
Jab koi variable ya function exist hi nahi karta ya TDZ mein access karte hain.

#### 5 Common Reasons:
```javascript
// Reason 1: Variable declare hi nahi kiya
console.log(nonExistent); // ReferenceError: nonExistent is not defined

// Reason 2: TDZ - let/const ko initialize se pehle access
console.log(x); // ReferenceError: Cannot access 'x' before initialization
let x = 10;

// Reason 3: Scope ke bahar access
function test() {
    let localVar = 5;
}
console.log(localVar); // ReferenceError: localVar is not defined

// Reason 4: Typo in variable name
let myVariable = 10;
console.log(myVariabel); // ReferenceError: myVariabel is not defined

// Reason 5: const variable ko reassign karne ki koshish
const PI = 3.14;
PI = 3.14159; // TypeError (but often confused with ReferenceError)
```

### 2. TypeError
Jab koi operation wrong data type pe perform karte hain.

#### 5 Common Reasons:
```javascript
// Reason 1: null/undefined pe property access
let obj = null;
console.log(obj.name); // TypeError: Cannot read property 'name' of null

// Reason 2: Non-function ko call karna
let notAFunction = 10;
notAFunction(); // TypeError: notAFunction is not a function

// Reason 3: const variable ko reassign
const x = 10;
x = 20; // TypeError: Assignment to constant variable

// Reason 4: Primitive value ko modify karna
let str = "hello";
str.toUpperCase = function() {}; // TypeError in strict mode

// Reason 5: Wrong argument type
let num = 123;
num.toUpperCase(); // TypeError: num.toUpperCase is not a function
```

### 3. SyntaxError
Jab code grammatically incorrect hai (parsing time pe error).

#### 5 Common Reasons:
```javascript
// Reason 1: Missing brackets/parentheses
function test() {
    console.log("Hello"
} // SyntaxError: missing ) after argument list

// Reason 2: Invalid variable name
let 123abc = 10; // SyntaxError: Invalid or unexpected token

// Reason 3: Duplicate parameter names (strict mode)
"use strict";
function test(a, a) {} // SyntaxError: Duplicate parameter name

// Reason 4: Missing colon in object
let obj = {
    name "John"  // SyntaxError: Missing colon
};

// Reason 5: Invalid JSON
JSON.parse("{name: 'John'}"); // SyntaxError: Unexpected token
```

---

## Execution Order Examples

### Example 1: Node.js Mixed Async Operations

```javascript
const fs = require("fs");
const a = 100;

setImmediate(() => console.log("setImmediate"));

Promise.resolve("Promise").then(console.log);

fs.readFile("./data.txt", "utf8", () => {
    console.log("File Reading CB");
});

setTimeout(() => console.log("Timer expired"), 0);

process.nextTick(() => console.log("process.nextTick"));

function printA() {
    console.log("a=", a);
}

printA();
console.log("Last line of the file.");
```

**Output:**
```
a= 100
Last line of the file.
process.nextTick
Promise
Timer expired
setImmediate
File Reading CB
```

**Explanation:**
1. **Synchronous code pehle (Call Stack):**
   - `printA()` → `a= 100`
   - `console.log("Last line of the file.")`

2. **Microtask Queue:**
   - `process.nextTick()` → Highest priority in Node.js
   - `Promise.then()` → Microtask

3. **Task Queue:**
   - `setTimeout(0)` → Next event loop
   - `setImmediate()` → Check phase (after timers)
   - `fs.readFile()` → I/O operation (takes time)

### Example 2: Promise and setTimeout Mix

```javascript
console.log("Start");

setTimeout(() => console.log("setTimeout 1"), 0);

Promise.resolve()
    .then(() => console.log("Promise 1"))
    .then(() => console.log("Promise 2"));

queueMicrotask(() => console.log("queueMicrotask"));

setTimeout(() => console.log("setTimeout 2"), 0);

console.log("End");
```

**Output:**
```
Start
End
Promise 1
queueMicrotask
Promise 2
setTimeout 1
setTimeout 2
```

### Example 3: Nested Async Operations

```javascript
console.log("A");

setTimeout(() => {
    console.log("B");
    Promise.resolve().then(() => console.log("C"));
}, 0);

Promise.resolve().then(() => {
    console.log("D");
    setTimeout(() => console.log("E"), 0);
});

console.log("F");
```

**Output:**
```
A
F
D
B
C
E
```

---

## Practice Questions

### Question 1
```javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
```
**Answer:** `1, 4, 3, 2`

---

### Question 2
```javascript
async function test() {
    console.log("A");
    await Promise.resolve();
    console.log("B");
}

test();
console.log("C");
```
**Answer:** `A, C, B`  
*(await ke baad ka code microtask ban jata hai)*

---

### Question 3
```javascript
console.log("Start");
process.nextTick(() => console.log("nextTick"));
Promise.resolve().then(() => console.log("Promise"));
setTimeout(() => console.log("setTimeout"), 0);
console.log("End");
```
**Answer:** `Start, End, nextTick, Promise, setTimeout`

---

### Question 4
```javascript
setTimeout(() => console.log("1"), 0);

Promise.resolve().then(() => {
    console.log("2");
    setTimeout(() => console.log("3"), 0);
});

Promise.resolve().then(() => console.log("4"));
console.log("5");
```
**Answer:** `5, 2, 4, 1, 3`

---

### Question 5
```javascript
console.log("A");

setTimeout(() => {
    console.log("B");
    Promise.resolve().then(() => console.log("C"));
}, 0);

new Promise((resolve) => {
    console.log("D");
    resolve();
}).then(() => console.log("E"));

console.log("F");
```
**Answer:** `A, D, F, E, B, C`

---

### Question 6
```javascript
const promise = new Promise((resolve) => {
    console.log("1");
    resolve();
    console.log("2");
});

promise.then(() => console.log("3"));
console.log("4");
```
**Answer:** `1, 2, 4, 3`  
*(Promise executor synchronous hai)*

---

### Question 7
```javascript
setTimeout(() => console.log("timeout"), 0);
queueMicrotask(() => console.log("microtask"));
Promise.resolve().then(() => console.log("promise"));
console.log("sync");
```
**Answer:** `sync, microtask, promise, timeout`

---

### Question 8
```javascript
async function async1() {
    console.log("A");
    await async2();
    console.log("B");
}

async function async2() {
    console.log("C");
}

console.log("D");
async1();
console.log("E");
```
**Answer:** `D, A, C, E, B`

---

### Question 9
```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timeout");
    Promise.resolve().then(() => console.log("Promise in Timeout"));
}, 0);

Promise.resolve()
    .then(() => console.log("Promise 1"))
    .then(() => console.log("Promise 2"));

console.log("End");
```
**Answer:** `Start, End, Promise 1, Promise 2, Timeout, Promise in Timeout`

---

### Question 10
```javascript
console.log("1");

process.nextTick(() => {
    console.log("2");
    process.nextTick(() => console.log("3"));
});

Promise.resolve().then(() => console.log("4"));
process.nextTick(() => console.log("5"));
console.log("6");
```
**Answer:** `1, 6, 2, 5, 3, 4`  
*(process.nextTick recursive bhi ho sakta hai)*

---

## Key Takeaways

✅ **Synchronous code** sabse pehle execute hota hai  
✅ **process.nextTick()** sabse high priority (Node.js)  
✅ **Promises** microtask queue mein jate hain  
✅ **setTimeout/setImmediate** task queue mein jate hain  
✅ **let/const hoisted** hote hain but TDZ ke karan access nahi kar sakte  
✅ **Event Loop**: Call Stack → Microtask Queue → Task Queue  

---

## Contributing

Feel free to open issues or submit pull requests to improve this guide!

## License

MIT License - Feel free to use this guide for learning purposes.

---

**Happy Learning! 🚀**
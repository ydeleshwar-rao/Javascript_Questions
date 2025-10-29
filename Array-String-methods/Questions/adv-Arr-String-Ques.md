## 🔥 Advanced Level (Questions 26-35)

### 26. **Deep clone array/object without JSON methods**
```javascript
// Complex nested structure ko properly clone karo
const complexObj = {
    name: "John",
    skills: ["JS", "React"],
    address: { city: "Delhi", pincode: 110001 }
};
```

### 27. **Array rotation implement karo**
```javascript
// Example: [1,2,3,4,5] rotate by 2 → [4,5,1,2,3]
const rotateArr = [1, 2, 3, 4, 5, 6, 7];
const rotateBy = 3;
// Methods: slice + spread, manual shifting
```

### 28. **String compression implement karo**
```javascript
// Example: "aaabbccccaa" → "a3b2c4a2"
const compressStr = "aaabbccccaaddd";
```

### 29. **Array mein kth smallest/largest element find karo**
```javascript
// Example: [7,10,4,3,20,15] mein 3rd smallest → 7
const unsortedArr = [7, 10, 4, 3, 20, 15, 25, 1];
const k = 3; // 3rd smallest
```

### 30. **Implement custom map, filter, reduce functions**
```javascript
// Apne khud ke higher-order functions banao
const testArray = [1, 2, 3, 4, 5];

// Custom implementations:
// myMap(), myFilter(), myReduce()
```

### 31. **Matrix (2D array) operations**
```javascript
// Matrix transpose, rotation, spiral traversal
const matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];
// Tasks: Transpose, 90° rotation, spiral order
```

### 32. **Debounce function for array operations**
```javascript
// Function jo frequently called hone par delay kare
const searchArray = (query) => {
    // Search logic here
    console.log("Searching for:", query);
};
// Implement debounce wrapper
```

### 33. **Array permutations generate karo**
```javascript
// Example: [1,2,3] → [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
const permuteArr = [1, 2, 3];
```

### 34. **LCS (Longest Common Subsequence) in strings**
```javascript
// Example: "ABCDGH" aur "AEDFHR" → "ADH" (length 3)
const lcsStr1 = "ABCDGH";
const lcsStr2 = "AEDFHR";
```

### 35. **Memory-efficient array operations (Generator functions)**
```javascript
// Large arrays ke liye memory-efficient solutions
function* fibonacciGenerator(n) {
    // Generate Fibonacci sequence efficiently
}

function* primeGenerator(limit) {
    // Generate prime numbers efficiently
}
```

---

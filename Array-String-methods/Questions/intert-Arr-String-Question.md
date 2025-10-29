## 🚀 Intermediate Level (Questions 11-25)

### 11. **Two arrays ko merge aur sort karo**
```javascript
// Example: [1, 3, 5] + [2, 4, 6] → [1, 2, 3, 4, 5, 6]
const arr1 = [1, 3, 5, 7];
const arr2 = [2, 4, 6, 8];
// Methods: concat(), spread operator, sort()
```

### 12. **Array mein most frequent element find karo**    Done
```javascript
// Example: [1, 2, 2, 3, 2, 4] → Output: 2
const frequencyArr = [1, 2, 2, 3, 2, 4, 3, 2];
// Logic: Object/Map for counting
```

### 13. **String mein character frequency count karo**  Done /Half
```javascript
// Example: "hello" → {h: 1, e: 1, l: 2, o: 1}
const charString = "programming";
// Methods: Object, Map, reduce()
```

### 14. **Array ko chunks mein divide karo**  Done
```javascript
// Example: [1,2,3,4,5,6,7,8] chunk size 3 → [[1,2,3], [4,5,6], [7,8]]
const longArr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const chunkSize = 3;
```

### 15. **Palindrome string check karo**  Done
```javascript
// Example: "racecar" → true, "hello" → false
const testStr1 = "racecar";
const testStr2 = "level";
const testStr3 = "hello";
```

### 16. **Array mein missing number find karo (1 to n sequence)**
```javascript
// Example: [1, 2, 4, 5, 6] → Missing: 3
const incompleteArr = [1, 2, 4, 5, 6, 7, 9, 10];
// Logic: Math sum formula ya Set
```

### 17. **Two strings anagram hain ya nahi check karo**   Done
```javascript
// Example: "listen" aur "silent" → true
const str1 = "listen";
const str2 = "silent";
const str3 = "hello";
const str4 = "world";   note when ever you use string ass array u need split("") for sort() to joint the array to string ==> join();
```

### 18. **Array ko flatten karo (nested arrays)**
```javascript
// Example: [1, [2, 3], [4, [5, 6]]] → [1, 2, 3, 4, 5, 6]
const nestedArr = [1, [2, 3], [4, [5, 6]], 7];
 

### 19. **Array mein common elements find karo (intersection)**
```javascript
// Example: [1,2,3,4] ∩ [3,4,5,6] → [3, 4]
const setA = [1, 2, 3, 4, 5];
const setB = [3, 4, 5, 6, 7];
// Methods: filter + includes, Set
```

### 20. **String mein longest word find karo**
```javascript
// Example: "The quick brown fox jumps" → "quick" ya "brown" (length 5)
const sentence = "JavaScript is a powerful programming language";
```

### 21. **Array elements ko group karo (specific condition)**
```javascript
// Example: Group by even/odd: [1,2,3,4,5,6] → {even: [2,4,6], odd: [1,3,5]}
const numbersToGroup = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
// Methods: reduce(), filter()
```

### 22. **Binary search implement karo (sorted array mein)**
```javascript
// Example: [1,3,5,7,9,11] mein 7 find karo → index 3
const sortedArr = [1, 3, 5, 7, 9, 11, 13, 15];
const searchValue = 7;
```

### 23. **String mein words ko reverse karo (order maintain)**
```javascript
// Example: "Hello World JavaScript" → "olleH dlroW tpircSavaJ"
const wordString = "Hello World JavaScript Programming";
```

### 24. **Array mein subarray ka maximum sum find karo (Kadane's Algorithm)**
```javascript
// Example: [-2,1,-3,4,-1,2,1,-5,4] → Maximum sum: 6 (subarray: [4,-1,2,1])
const kadaneArr = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
```

### 25. **Two arrays ka difference nikalo**
```javascript
// Example: [1,2,3,4] - [2,4,5] → [1, 3]
const mainArr = [1, 2, 3, 4, 5];
const removeArr = [2, 4, 6];
```

---


### String Methods:
- **split()** - Convert to array
- **charAt()** - Get character at index
- **substring()** - Extract portion
- **toLowerCase()** - Convert to lowercase
- **toUpperCase()** - Convert to uppercase
- **trim()** - Remove whitespace
- **replace()** - Replace text
- **match()** - Pattern matching
- **search()** - Find pattern index
- **includes()** - Check if contains text

# JavaScript String Methods - Complete Reference Guide

## 1. split()

**Description**
The `split()` method splits a string into an array of substrings.
The `split()` method returns the new array.
The `split()` method does not change the original string.
If (" ") is used as separator, the string is split between words.

**Syntax**
```javascript
string.split(separator, limit)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| separator | Optional. A string or regular expression to use for splitting. If omitted, an array with the original string is returned. |
| limit | Optional. An integer that limits the number of splits. Items after the split limit will not be included in the array. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | An array of strings, split at each point where the separator occurs in the given string. |

**Example**
```javascript
const str = "How are you doing today?";
const words = str.split(" ");
console.log(words); // ["How", "are", "you", "doing", "today?"]

const chars = str.split("");
console.log(chars); // ["H", "o", "w", " ", "a", "r", "e", ...]

const limited = str.split(" ", 3);
console.log(limited); // ["How", "are", "you"]
```

---

## 2. charAt()

**Description**
The `charAt()` method returns the character at a specified index (position) in a string.
The index of the first character is 0, the second 1, ...
The `charAt()` method returns an empty string if the index number is not found.

**Syntax**
```javascript
string.charAt(index)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| index | Required. The index of the character you want to return. |

**Return Value**
| Type | Description |
|------|-------------|
| String | The character at the specified index. An empty string ("") if the index is out of range. |

**Example**
```javascript
const str = "HELLO WORLD";
console.log(str.charAt(0)); // "H"
console.log(str.charAt(6)); // "W"
console.log(str.charAt(20)); // "" (empty string)

// Using in loop
for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i));
}
```

---

## 3. substring()

**Description**
The `substring()` method extracts characters from a string, between two specified indices.
The `substring()` method returns the substring.
The `substring()` method does not change the original string.
If start is greater than end, arguments are swapped: (4, 1) = (1, 4).
Arguments below 0 are treated as 0.

**Syntax**
```javascript
string.substring(start, end)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| start | Required. Start position. First character is at index 0. |
| end | Optional. End position (up to, but not including). If omitted, extracts the rest of the string. |

**Return Value**
| Type | Description |
|------|-------------|
| String | A string containing the extracted characters. |

**Example**
```javascript
const str = "Hello World";
console.log(str.substring(0, 5)); // "Hello"
console.log(str.substring(6)); // "World"
console.log(str.substring(6, 11)); // "World"
console.log(str.substring(3, 0)); // "Hel" (arguments swapped)

// Extract last 3 characters
console.log(str.substring(str.length - 3)); // "rld"
```

---

## 4. toLowerCase()

**Description**
The `toLowerCase()` method converts a string to lowercase letters.
The `toLowerCase()` method returns the string converted to lowercase.
The `toLowerCase()` method does not change the original string.

**Syntax**
```javascript
string.toLowerCase()
```

**Parameters**
None

**Return Value**
| Type | Description |
|------|-------------|
| String | The string converted to lowercase. |

**Example**
```javascript
const str = "Hello World";
console.log(str.toLowerCase()); // "hello world"

const mixedCase = "JavaScript Programming";
console.log(mixedCase.toLowerCase()); // "javascript programming"

// Case-insensitive comparison
const str1 = "HELLO";
const str2 = "hello";
console.log(str1.toLowerCase() === str2.toLowerCase()); // true
```

---

## 5. toUpperCase()

**Description**
The `toUpperCase()` method converts a string to uppercase letters.
The `toUpperCase()` method returns the string converted to uppercase.
The `toUpperCase()` method does not change the original string.

**Syntax**
```javascript
string.toUpperCase()
```

**Parameters**
None

**Return Value**
| Type | Description |
|------|-------------|
| String | The string converted to uppercase. |

**Example**
```javascript
const str = "Hello World";
console.log(str.toUpperCase()); // "HELLO WORLD"

const lowerCase = "javascript programming";
console.log(lowerCase.toUpperCase()); // "JAVASCRIPT PROGRAMMING"

// Creating initials
const name = "john doe";
const initials = name.split(" ").map(word => word.charAt(0).toUpperCase()).join("");
console.log(initials); // "JD"
```

---

## 6. trim()

**Description**
The `trim()` method removes whitespace from both sides of a string.
The `trim()` method returns a new string.
The `trim()` method does not change the original string.

**Syntax**
```javascript
string.trim()
```

**Parameters**
None

**Return Value**
| Type | Description |
|------|-------------|
| String | A string with removed whitespace from both ends. |

**Example**
```javascript
const str = "   Hello World   ";
console.log(str.trim()); // "Hello World"
console.log(str.length); // 17
console.log(str.trim().length); // 11

// Useful for form validation
const userInput = "  john@example.com  ";
const cleanEmail = userInput.trim().toLowerCase();
console.log(cleanEmail); // "john@example.com"

// Also removes tabs and newlines
const messyString = "\n\t  Hello  \t\n";
console.log(messyString.trim()); // "Hello"
```

---

## 7. replace()

**Description**
The `replace()` method searches a string for a value or regular expression.
The `replace()` method returns a new string with the value(s) replaced.
The `replace()` method does not change the original string.
By default, the `replace()` method replaces only the first match.

**Syntax**
```javascript
string.replace(searchValue, newValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| searchValue | Required. The value, or regular expression, to search for. |
| newValue | Required. The value to replace with. |

**Return Value**
| Type | Description |
|------|-------------|
| String | A new string where the specified value(s) has been replaced. |

**Example**
```javascript
const str = "Hello World";
console.log(str.replace("World", "JavaScript")); // "Hello JavaScript"

// Replace only first occurrence
const text = "cat dog cat bird cat";
console.log(text.replace("cat", "pet")); // "pet dog cat bird cat"

// Replace all occurrences using regex
console.log(text.replace(/cat/g, "pet")); // "pet dog pet bird pet"

// Case-insensitive replace
const message = "Hello hello HELLO";
console.log(message.replace(/hello/gi, "Hi")); // "Hi Hi Hi"
```

---

## 8. match()

**Description**
The `match()` method matches a string against a regular expression.
The `match()` method returns an array with the match(es).
The `match()` method returns null if no match is found.

**Syntax**
```javascript
string.match(regexp)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| regexp | Required. A regular expression object, or any object that has a Symbol.match method. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | An array containing the match(es). null if no match is found. |

**Example**
```javascript
const str = "The rain in SPAIN stays mainly in the plain";

// Find word containing "ain"
console.log(str.match(/ain/g)); // ["ain", "AIN", "ain", "ain"]

// Case-insensitive match
console.log(str.match(/ain/gi)); // ["ain", "AIN", "ain", "ain"]

// Match digits
const text = "Call me at 123-456-7890 or 987-654-3210";
console.log(text.match(/\d{3}-\d{3}-\d{4}/g)); // ["123-456-7890", "987-654-3210"]

// Match email pattern
const email = "Contact us at info@example.com or sales@test.org";
console.log(email.match(/\w+@\w+\.\w+/g)); // ["info@example.com", "sales@test.org"]
```

---

## 9. search()

**Description**
The `search()` method matches a string against a regular expression.
The `search()` method returns the index (position) of the first match.
The `search()` method returns -1 if no match is found.
The `search()` method is case sensitive.

**Syntax**
```javascript
string.search(regexp)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| regexp | Required. A regular expression object, or a string that will be converted to a regular expression. |

**Return Value**
| Type | Description |
|------|-------------|
| Number | The index (position) of the first match. -1 if not found. |

**Example**
```javascript
const str = "Mr. Blue has a blue house";

console.log(str.search("Blue")); // 4
console.log(str.search("blue")); // 15 (first lowercase "blue")
console.log(str.search("red")); // -1 (not found)

// Using regular expression
console.log(str.search(/blue/i)); // 4 (case-insensitive, finds first "Blue")

// Find digit
const text = "Hello123World456";
console.log(text.search(/\d/)); // 5 (first digit position)

// Find email position
const message = "Contact info@example.com for details";
console.log(message.search(/\w+@\w+\.\w+/)); // 8
```

---

## 10. includes()

**Description**
The `includes()` method returns true if a string contains a specified string.
Otherwise it returns false.
The `includes()` method is case sensitive.

**Syntax**
```javascript
string.includes(searchString, start)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| searchString | Required. The string to search for. |
| start | Optional. Default 0. The position to start from. |

**Return Value**
| Type | Description |
|------|-------------|
| Boolean | true if the string contains the value, otherwise false. |

**Example**
```javascript
const str = "Hello world, welcome to the universe.";

console.log(str.includes("world")); // true
console.log(str.includes("World")); // false (case sensitive)
console.log(str.includes("world", 12)); // false (start from position 12)

// Practical use cases
const email = "user@example.com";
console.log(email.includes("@")); // true (valid email check)

const url = "https://www.example.com";
console.log(url.includes("https")); // true (secure URL check)

// Check multiple conditions
const userInput = "JavaScript Programming";
const keywords = ["JavaScript", "Python", "Java"];
const hasKeyword = keywords.some(keyword => userInput.includes(keyword));
console.log(hasKeyword); // true
```

---

## Additional String Methods (Bonus)

### 11. slice()

**Description**
The `slice()` method extracts a part of a string and returns the extracted part in a new string.
The method takes 2 parameters: start position, and end position (end not included).

**Syntax**
```javascript
string.slice(start, end)
```

**Example**
```javascript
const str = "Apple, Banana, Kiwi";
console.log(str.slice(7, 13)); // "Banana"
console.log(str.slice(-4)); // "Kiwi" (negative index)
console.log(str.slice(0, 5)); // "Apple"
```

### 12. indexOf()

**Description**
The `indexOf()` method returns the index (position) the first occurrence of a specified text in a string.
JavaScript counts positions from zero. Returns -1 if the text is not found.

**Syntax**
```javascript
string.indexOf(searchValue, start)
```

**Example**
```javascript
const str = "Please locate where 'locate' occurs!";
console.log(str.indexOf("locate")); // 7
console.log(str.indexOf("locate", 15)); // 21
console.log(str.indexOf("John")); // -1 (not found)
```

### 13. lastIndexOf()

**Description**
The `lastIndexOf()` method returns the index of the last occurrence of a specified text in a string.
The method searches backwards from the end of the string.

**Syntax**
```javascript
string.lastIndexOf(searchValue, start)
```

**Example**
```javascript
const str = "Please locate where 'locate' occurs!";
console.log(str.lastIndexOf("locate")); // 21
console.log(str.lastIndexOf("e")); // 36
```

### 14. startsWith()

**Description**
The `startsWith()` method returns true if a string starts with a specified string.
Otherwise it returns false. The method is case sensitive.

**Syntax**
```javascript
string.startsWith(searchString, start)
```

**Example**
```javascript
const str = "Hello world";
console.log(str.startsWith("Hello")); // true
console.log(str.startsWith("world", 6)); // true
console.log(str.startsWith("hello")); // false (case sensitive)
```

### 15. endsWith()

**Description**
The `endsWith()` method returns true if a string ends with a specified string.
Otherwise it returns false. The method is case sensitive.

**Syntax**
```javascript
string.endsWith(searchString, length)
```

**Example**
```javascript
const str = "Hello world";
console.log(str.endsWith("world")); // true
console.log(str.endsWith("Hello", 5)); // true
console.log(str.endsWith("World")); // false (case sensitive)
```

---

## Summary

JavaScript String methods ko categories mein divide kar sakte hain:

**Text Transformation:**
- `toLowerCase()`, `toUpperCase()` - Case conversion
- `trim()` - Whitespace removal
- `replace()` - Text replacement

**Text Extraction:**
- `charAt()` - Single character
- `substring()`, `slice()` - Substring extraction
- `split()` - Convert to array

**Text Search & Validation:**
- `includes()` - Check if text exists
- `indexOf()`, `lastIndexOf()` - Find position
- `search()` - Pattern search
- `match()` - Pattern matching
- `startsWith()`, `endsWith()` - Position-specific checks

**Practical Tips:**
1. Use `trim()` for user input validation
2. Use `toLowerCase()` for case-insensitive comparisons  
3. Use `includes()` for simple text checking
4. Use `match()` with regex for complex pattern matching
5. Use `split()` to convert CSV or space-separated strings to arrays

Ye methods daily programming mein bahut useful hain. Practice karte rahen!
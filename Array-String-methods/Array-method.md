## 📚 Important JavaScript Built-in Methods Reference

### Array Methods:
- **map()** - Transform each element
- **filter()** - Select elements based on condition
- **reduce()** - Aggregate to single value
- **forEach()** - Iterate over elements
- **find()** - Find first matching element
- **findIndex()** - Find index of first match
- **some()** - Check if any element matches
- **every()** - Check if all elements match
- **sort()** - Sort array elements
- **reverse()** - Reverse array order
- **slice()** - Extract portion of array
- **splice()** - Add/remove elements
- **concat()** - Merge arrays
- **includes()** - Check if element exists
- **indexOf()** - Find element index
- **join()** - Convert to string
- **flat()** - Flatten nested arrays
- **pop()** - Remove last element
- **push()** - Add element to end
- **length** - Show the array size
- **shift()** - Remove first element
- **unshift()** - Add element to beginning

# JavaScript Array Methods - Complete Extended Reference Guide

## 1. map()

**Description**
The `map()` method creates a new array filled with the results of calling a function for every array element.
The `map()` method calls a function once for each element in an array.
The `map()` method does not execute the function for empty elements.
The `map()` method does not change the original array.

**Syntax**
```javascript
array.length
```

**Parameters**
None

**Return Value**
| Type | Description |
|------|-------------|
| Number | The number of elements in the array. |

**Examples**
```javascript
// Example 1: Get array length
const fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(fruits.length); // 4

// Example 2: Use length in loop
const numbers = [1, 2, 3, 4, 5];
for(let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]); // 1, 2, 3, 4, 5
}

// Example 3: Truncate array using length
const colors = ['red', 'green', 'blue', 'yellow'];
colors.length = 2;
console.log(colors); // ['red', 'green']
```

---

## 21. shift()

**Description**
The `shift()` method removes the first element from an array and returns that element.
The `shift()` method changes the original array.
The `shift()` method returns `undefined` if the array is empty.

**Syntax**
```javascript
array.shift()
```

**Parameters**
None

**Return Value**
| Type | Description |
|------|-------------|
| Any | The removed element from the array. `undefined` if the array is empty. |

**Examples**
```javascript
// Example 1: Remove first fruit
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const firstFruit = fruits.shift();
console.log(firstFruit); // "Banana"
console.log(fruits); // ["Orange", "Apple", "Mango"]

// Example 2: Process queue (FIFO)
const queue = ['first', 'second', 'third'];
while(queue.length > 0) {
  const item = queue.shift();
  console.log('Processing:', item); // first, second, third
}

// Example 3: Shift from empty array
const emptyArray = [];
const result = emptyArray.shift();
console.log(result); // undefined
console.log(emptyArray); // []
```

---

## 22. unshift()

**Description**
The `unshift()` method adds one or more elements to the beginning of an array and returns the new length of the array.
The `unshift()` method changes the original array.

**Syntax**
```javascript
array.unshift(item1, item2, ..., itemX)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| item1, item2, ..., itemX | Required. The item(s) to add to the beginning of the array. |

**Return Value**
| Type | Description |
|------|-------------|
| Number | The new length of the array. |

**Examples**
```javascript
// Example 1: Add single element to beginning
const fruits = ["Orange", "Apple", "Mango"];
const newLength = fruits.unshift("Banana");
console.log(newLength); // 4
console.log(fruits); // ["Banana", "Orange", "Apple", "Mango"]

// Example 2: Add multiple elements to beginning
const numbers = [4, 5, 6];
numbers.unshift(1, 2, 3);
console.log(numbers); // [1, 2, 3, 4, 5, 6]

// Example 3: Build priority queue
const priorities = ['medium', 'low'];
priorities.unshift('high');
priorities.unshift('critical', 'urgent');
console.log(priorities); // ['critical', 'urgent', 'high', 'medium', 'low']
```

---

## Summary

Ye sab JavaScript Array methods hai jo aap daily development mein use kar sakte hai. Har method ka apna specific purpose hai:

- **Iteration**: `forEach()`, `map()`
- **Filtering**: `filter()`, `find()`, `findIndex()`
- **Testing**: `some()`, `every()`, `includes()`
- **Aggregation**: `reduce()`
- **Manipulation**: `sort()`, `reverse()`, `splice()`
- **Creation**: `slice()`, `concat()`, `flat()`
- **Conversion**: `join()`
- **Search**: `indexOf()`
- **Add/Remove**: `push()`, `pop()`, `shift()`, `unshift()`
- **Properties**: `length`

**Key Points to Remember:**

1. **Mutating vs Non-mutating methods:**
   - **Mutating** (change original array): `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `sort()`, `reverse()`
   - **Non-mutating** (return new array): `map()`, `filter()`, `slice()`, `concat()`, `flat()`

2. **Return values:**
   - New array: `map()`, `filter()`, `slice()`, `concat()`, `flat()`
   - Boolean: `some()`, `every()`, `includes()`
   - Single value: `find()`, `reduce()`, `pop()`, `shift()`
   - Index: `findIndex()`, `indexOf()`
   - Length: `push()`, `unshift()`

3. **Performance tips:**
   - Use `for` loop for simple iterations when performance is critical
   - `some()` and `every()` stop early when condition is met
   - `find()` and `findIndex()` stop at first match

Reference ke liye ye guide save kar len aur practice karte rahen!
array.map(function(currentValue, index, arr), thisValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| currentValue | Required. The value of the current element. |
| index | Optional. The index of the current element. |
| arr | Optional. The array of the current element. |
| thisValue | Optional. Default `undefined`. A value passed to the function as its `this` value. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | A new array with the results of calling a function for each array element. |

**Examples**
```javascript
// Example 1: Double each number
const numbers = [1, 4, 9, 16];
const doubled = numbers.map(x => x * 2);
console.log(doubled); // [2, 8, 18, 32]

// Example 2: Convert to uppercase
const fruits = ['apple', 'banana', 'cherry'];
const upperFruits = fruits.map(fruit => fruit.toUpperCase());
console.log(upperFruits); // ['APPLE', 'BANANA', 'CHERRY']

// Example 3: Extract property from objects
const users = [{name: 'Ali', age: 25}, {name: 'Sara', age: 30}];
const names = users.map(user => user.name);
console.log(names); // ['Ali', 'Sara']
```

---

## 2. filter()

**Description**
The `filter()` method creates a new array filled with elements that pass a test provided by a function.
The `filter()` method does not execute the function for empty elements.
The `filter()` method does not change the original array.

**Syntax**
```javascript
array.filter(function(currentValue, index, arr), thisValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| currentValue | Required. The value of the current element. |
| index | Optional. The index of the current element. |
| arr | Optional. The array of the current element. |
| thisValue | Optional. Default `undefined`. A value passed to the function as its `this` value. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | An array of elements that pass the test. An empty array if no elements pass the test. |

**Examples**
```javascript
// Example 1: Filter adults
const ages = [32, 33, 16, 40];
const adults = ages.filter(age => age >= 18);
console.log(adults); // [32, 33, 40]

// Example 2: Filter even numbers
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8]

// Example 3: Filter strings by length
const words = ['hello', 'hi', 'javascript', 'code'];
const longWords = words.filter(word => word.length > 4);
console.log(longWords); // ['hello', 'javascript']
```

---

## 3. reduce()

**Description**
The `reduce()` method executes a reducer function for array element.
The `reduce()` method returns a single value: the function's accumulated result.
The `reduce()` method does not execute the function for empty array elements.
The `reduce()` method does not change the original array.

**Syntax**
```javascript
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| total | Required. The initialValue, or the previously returned value of the function. |
| currentValue | Required. The value of the current element. |
| currentIndex | Optional. The index of the current element. |
| arr | Optional. The array the current element belongs to. |
| initialValue | Optional. A value to be passed to the function as the initial value. |

**Return Value**
| Type | Description |
|------|-------------|
| Any | The accumulated result from the last call of the callback function. |

**Examples**
```javascript
// Example 1: Sum all numbers
const numbers = [175, 50, 25];
const sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum); // 250

// Example 2: Find maximum value
const nums = [3, 7, 2, 9, 1];
const max = nums.reduce((max, current) => current > max ? current : max);
console.log(max); // 9

// Example 3: Count occurrences
const fruits = ['apple', 'banana', 'apple', 'orange', 'banana'];
const count = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
console.log(count); // {apple: 2, banana: 2, orange: 1}
```

---

## 4. forEach()

**Description**
The `forEach()` method calls a function once for each array element.
The `forEach()` method is not executed for empty elements.

**Syntax**
```javascript
array.forEach(function(currentValue, index, arr), thisValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| currentValue | Required. The value of the current element. |
| index | Optional. The index of the current element. |
| arr | Optional. The array of the current element. |
| thisValue | Optional. Default `undefined`. A value passed to the function as its `this` value. |

**Return Value**
| Type | Description |
|------|-------------|
| undefined | undefined |

**Examples**
```javascript
// Example 1: Log each fruit with index
const fruits = ["apple", "orange", "cherry"];
fruits.forEach((fruit, index) => {
  console.log(index + ": " + fruit);
});
// 0: apple
// 1: orange
// 2: cherry

// Example 2: Update array elements (not recommended, use map instead)
const numbers = [1, 2, 3, 4, 5];
numbers.forEach((num, index, arr) => {
  arr[index] = num * 2;
});
console.log(numbers); // [2, 4, 6, 8, 10]

// Example 3: Calculate total using forEach
const prices = [10, 25, 30, 15];
let total = 0;
prices.forEach(price => total += price);
console.log(total); // 80
```

---

## 5. find()

**Description**
The `find()` method returns the value of the first array element that passes a test function.
The `find()` method executes a function for each array element.
The `find()` method returns `undefined` if no elements are found.
The `find()` method does not execute the function for empty elements.
The `find()` method does not change the original array.

**Syntax**
```javascript
array.find(function(currentValue, index, arr), thisValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| currentValue | Required. The value of the current element. |
| index | Optional. The index of the current element. |
| arr | Optional. The array of the current element. |
| thisValue | Optional. Default `undefined`. A value passed to the function as its `this` value. |

**Return Value**
| Type | Description |
|------|-------------|
| Any | The value of the first element that pass the test. Otherwise it returns `undefined`. |

**Examples**
```javascript
// Example 1: Find first adult
const ages = [3, 10, 18, 20];
const adult = ages.find(age => age >= 18);
console.log(adult); // 18

// Example 2: Find user by name
const users = [
  {name: 'Ali', age: 25},
  {name: 'Sara', age: 30},
  {name: 'Ahmad', age: 35}
];
const user = users.find(u => u.name === 'Sara');
console.log(user); // {name: 'Sara', age: 30}

// Example 3: Find even number
const numbers = [1, 3, 5, 8, 9, 10];
const firstEven = numbers.find(num => num % 2 === 0);
console.log(firstEven); // 8
```

---

## 6. findIndex()

**Description**
The `findIndex()` method returns the index of the first array element that passes a test function.
The `findIndex()` method executes a function for each array element.
The `findIndex()` method returns -1 if no match is found.
The `findIndex()` method does not execute the function for empty elements.
The `findIndex()` method does not change the original array.

**Syntax**
```javascript
array.findIndex(function(currentValue, index, arr), thisValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| currentValue | Required. The value of the current element. |
| index | Optional. The index of the current element. |
| arr | Optional. The array of the current element. |
| thisValue | Optional. Default `undefined`. A value passed to the function as its `this` value. |

**Return Value**
| Type | Description |
|------|-------------|
| Number | The index of the first element that passes the test. Otherwise -1. |

**Examples**
```javascript
// Example 1: Find index of first adult
const ages = [3, 10, 18, 20];
const adultIndex = ages.findIndex(age => age >= 18);
console.log(adultIndex); // 2

// Example 2: Find index of user
const users = ['Ali', 'Sara', 'Ahmad'];
const saraIndex = users.findIndex(name => name === 'Sara');
console.log(saraIndex); // 1

// Example 3: Find index of negative number
const numbers = [1, 5, 8, -2, 10];
const negativeIndex = numbers.findIndex(num => num < 0);
console.log(negativeIndex); // 3
```

---

## 7. some()

**Description**
The `some()` method tests whether at least one element in the array passes the test implemented by the provided function.
The `some()` method executes the function once for each array element.
The `some()` method returns true (and stops) if the function returns true for one of the array elements.
The `some()` method returns false if the function returns false for all of the array elements.
The `some()` method does not execute the function for empty elements.
The `some()` method does not change the original array.

**Syntax**
```javascript
array.some(function(currentValue, index, arr), thisValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| currentValue | Required. The value of the current element. |
| index | Optional. The index of the current element. |
| arr | Optional. The array of the current element. |
| thisValue | Optional. Default `undefined`. A value passed to the function as its `this` value. |

**Return Value**
| Type | Description |
|------|-------------|
| Boolean | true if any of the array elements pass the test, otherwise false. |

**Examples**
```javascript
// Example 1: Check if any adult exists
const ages = [3, 10, 18, 20];
const hasAdult = ages.some(age => age >= 18);
console.log(hasAdult); // true

// Example 2: Check if any even number exists
const numbers = [1, 3, 5, 7, 9];
const hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven); // false

// Example 3: Check if any string is long
const words = ['hi', 'hello', 'bye'];
const hasLongWord = words.some(word => word.length > 4);
console.log(hasLongWord); // true
```

---

## 8. every()

**Description**
The `every()` method tests whether all elements in the array pass the test implemented by the provided function.
The `every()` method executes the function once for each array element.
The `every()` method returns false (and stops) if the function returns false for one of the array elements.
The `every()` method returns true if the function returns true for all of the array elements.
The `every()` method does not execute the function for empty elements.
The `every()` method does not change the original array.

**Syntax**
```javascript
array.every(function(currentValue, index, arr), thisValue)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| function() | Required. A function to run for each array element. |
| currentValue | Required. The value of the current element. |
| index | Optional. The index of the current element. |
| arr | Optional. The array of the current element. |
| thisValue | Optional. Default `undefined`. A value passed to the function as its `this` value. |

**Return Value**
| Type | Description |
|------|-------------|
| Boolean | true if all the array elements pass the test, otherwise false. |

**Examples**
```javascript
// Example 1: Check if all are adults
const ages = [32, 33, 16, 40];
const allAdults = ages.every(age => age >= 18);
console.log(allAdults); // false

// Example 2: Check if all numbers are positive
const numbers = [1, 5, 8, 12, 3];
const allPositive = numbers.every(num => num > 0);
console.log(allPositive); // true

// Example 3: Check if all strings are short
const words = ['hi', 'ok', 'bye'];
const allShort = words.every(word => word.length <= 3);
console.log(allShort); // true
```

---

## 9. sort()

**Description**
The `sort()` method sorts the elements of an array.
The `sort()` method sorts the elements as strings in alphabetical and ascending order.
The `sort()` method changes the original array.

**Syntax**
```javascript
array.sort(compareFunction)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| compareFunction | Optional. A function that defines the sort order. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | The array with the items sorted. |

**Examples**
```javascript
// Example 1: Sort strings alphabetically
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();
console.log(fruits); // ["Apple", "Banana", "Mango", "Orange"]

// Example 2: Sort numbers ascending
const numbers = [40, 100, 1, 5, 25, 10];
numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 5, 10, 25, 40, 100]

// Example 3: Sort objects by property
const students = [
  {name: 'Ali', marks: 85},
  {name: 'Sara', marks: 92},
  {name: 'Ahmad', marks: 78}
];
students.sort((a, b) => b.marks - a.marks);
console.log(students); 
// [{name: 'Sara', marks: 92}, {name: 'Ali', marks: 85}, {name: 'Ahmad', marks: 78}]
```

---

## 10. reverse()

**Description**
The `reverse()` method reverses the order of the elements in an array.
The `reverse()` method overwrites the original array.

**Syntax**
```javascript
array.reverse()
```

**Parameters**
None

**Return Value**
| Type | Description |
|------|-------------|
| Array | The array after it has been reversed. |

**Examples**
```javascript
// Example 1: Reverse fruits array
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.reverse();
console.log(fruits); // ["Mango", "Apple", "Orange", "Banana"]

// Example 2: Reverse numbers
const numbers = [1, 2, 3, 4, 5];
numbers.reverse();
console.log(numbers); // [5, 4, 3, 2, 1]

// Example 3: Reverse and then sort
const letters = ['z', 'a', 'm', 'b'];
letters.reverse().sort();
console.log(letters); // ['a', 'b', 'm', 'z']
```

---

## 11. slice()

**Description**
The `slice()` method returns selected elements in an array, as a new array.
The `slice()` method selects from a given start, up to a (not inclusive) given end.
The `slice()` method does not change the original array.

**Syntax**
```javascript
array.slice(start, end)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| start | Optional. Start position. Default is 0. Negative numbers select from the end. |
| end | Optional. End position. Default is last element. Negative numbers select from the end. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | A new array containing the selected elements. |

**Examples**
```javascript
// Example 1: Get middle elements
const fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
const citrus = fruits.slice(1, 3);
console.log(citrus); // ["Orange", "Lemon"]
console.log(fruits); // Original array unchanged

// Example 2: Get last 3 elements
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];
const lastThree = numbers.slice(-3);
console.log(lastThree); // [6, 7, 8]

// Example 3: Copy entire array
const original = ['a', 'b', 'c'];
const copy = original.slice();
console.log(copy); // ['a', 'b', 'c']
```

---

## 12. splice()

**Description**
The `splice()` method adds and/or removes array elements.
The `splice()` method overwrites the original array.

**Syntax**
```javascript
array.splice(index, deleteCount, item1, ....., itemX)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| index | Required. The position to add/remove items. |
| deleteCount | Optional. Number of items to be removed. |
| item1, ..., itemX | Optional. New elements to be added. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | An array containing the removed elements (if any). |

**Examples**
```javascript
// Example 1: Replace elements
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const removed = fruits.splice(2, 1, "Lemon", "Kiwi");
console.log(fruits); // ["Banana", "Orange", "Lemon", "Kiwi", "Mango"]
console.log(removed); // ["Apple"]

// Example 2: Remove elements only
const numbers = [1, 2, 3, 4, 5, 6];
const deleted = numbers.splice(2, 2);
console.log(numbers); // [1, 2, 5, 6]
console.log(deleted); // [3, 4]

// Example 3: Add elements only
const colors = ['red', 'blue'];
colors.splice(1, 0, 'green', 'yellow');
console.log(colors); // ['red', 'green', 'yellow', 'blue']
```

---

## 13. concat()

**Description**
The `concat()` method concatenates (joins) two or more arrays.
The `concat()` method returns a new array, containing the joined arrays.
The `concat()` method does not change the existing arrays.

**Syntax**
```javascript
array1.concat(array2, array3, ..., arrayX)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| array2, array3, ..., arrayX | Required. The arrays to be joined. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | A new array, containing the joined arrays. |

**Examples**
```javascript
// Example 1: Concatenate two arrays
const arr1 = ["a", "b", "c"];
const arr2 = ["d", "e", "f"];
const arr3 = arr1.concat(arr2);
console.log(arr3); // ["a", "b", "c", "d", "e", "f"]

// Example 2: Concatenate multiple arrays
const nums1 = [1, 2];
const nums2 = [3, 4];
const nums3 = [5, 6];
const allNums = nums1.concat(nums2, nums3);
console.log(allNums); // [1, 2, 3, 4, 5, 6]

// Example 3: Concatenate with values
const fruits = ['apple', 'banana'];
const moreFruits = fruits.concat('orange', ['mango', 'grape']);
console.log(moreFruits); // ['apple', 'banana', 'orange', 'mango', 'grape']
```

---

## 14. includes()

**Description**
The `includes()` method returns true if an array contains a specified element, otherwise false.
The `includes()` method is case sensitive.

**Syntax**
```javascript
array.includes(searchElement, start)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| searchElement | Required. The element to search for. |
| start | Optional. Default 0. At which position in the array to start the search. |

**Return Value**
| Type | Description |
|------|-------------|
| Boolean | true if the element is found, otherwise false. |

**Examples**
```javascript
// Example 1: Check if fruit exists
const fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(fruits.includes("Mango")); // true
console.log(fruits.includes("mango")); // false (case sensitive)

// Example 2: Check from specific position
const numbers = [1, 2, 3, 2, 1];
console.log(numbers.includes(2)); // true
console.log(numbers.includes(2, 3)); // true (starts from index 3)

// Example 3: Check object references
const obj1 = {name: 'Ali'};
const obj2 = {name: 'Sara'};
const users = [obj1, obj2];
console.log(users.includes(obj1)); // true
console.log(users.includes({name: 'Ali'})); // false (different object)
```

---

## 15. indexOf()

**Description**
The `indexOf()` method returns the first index (position) of a specified element.
The `indexOf()` method returns -1 if the element is not found.
The `indexOf()` method starts at a specified index and searches from left to right.

**Syntax**
```javascript
array.indexOf(searchElement, start)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| searchElement | Required. The element to locate. |
| start | Optional. Default 0. Where to start the search. |

**Return Value**
| Type | Description |
|------|-------------|
| Number | The index (position) of the first element found. -1 if not found. |

**Examples**
```javascript
// Example 1: Find index of fruits
const fruits = ["Apple", "Orange", "Apple", "Mango"];
console.log(fruits.indexOf("Apple")); // 0
console.log(fruits.indexOf("Apple", 1)); // 2
console.log(fruits.indexOf("Banana")); // -1

// Example 2: Find index of numbers
const numbers = [10, 20, 30, 20, 40];
console.log(numbers.indexOf(20)); // 1
console.log(numbers.indexOf(20, 2)); // 3

// Example 3: Check if element exists
const colors = ['red', 'green', 'blue'];
const blueExists = colors.indexOf('blue') !== -1;
console.log(blueExists); // true
```

---

## 16. join()

**Description**
The `join()` method returns an array as a string.
The `join()` method does not change the original array.
Any separator can be specified. The default is comma (,).

**Syntax**
```javascript
array.join(separator)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| separator | Optional. The separator to be used. Default is a comma. |

**Return Value**
| Type | Description |
|------|-------------|
| String | A string with all array elements joined. |

**Examples**
```javascript
// Example 1: Join with different separators
const fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(fruits.join()); // "Banana,Orange,Apple,Mango"
console.log(fruits.join(" - ")); // "Banana - Orange - Apple - Mango"
console.log(fruits.join("")); // "BananaOrangeAppleMango"

// Example 2: Create readable sentence
const words = ['JavaScript', 'is', 'awesome'];
const sentence = words.join(' ') + '!';
console.log(sentence); // "JavaScript is awesome!"

// Example 3: Join numbers
const numbers = [1, 2, 3, 4, 5];
console.log(numbers.join(' + ')); // "1 + 2 + 3 + 4 + 5"
console.log(numbers.join('')); // "12345"
```

---

## 17. flat()

**Description**
The `flat()` method creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.
The `flat()` method does not change the original array.

**Syntax**
```javascript
array.flat(depth)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| depth | Optional. Default 1. How deep a nested array structure should be flattened. |

**Return Value**
| Type | Description |
|------|-------------|
| Array | A new array with the sub-array elements concatenated into it. |

**Examples**
```javascript
// Example 1: Flatten one level
const arr1 = [0, 1, 2, [3, 4]];
console.log(arr1.flat()); // [0, 1, 2, 3, 4]

// Example 2: Flatten multiple levels
const arr2 = [0, 1, 2, [[[3, 4]]]];
console.log(arr2.flat(2)); // [0, 1, 2, [3, 4]]
console.log(arr2.flat(Infinity)); // [0, 1, 2, 3, 4]

// Example 3: Mixed nested array
const mixed = [1, [2, 3], [4, [5, 6]], 7];
console.log(mixed.flat()); // [1, 2, 3, 4, [5, 6], 7]
console.log(mixed.flat(2)); // [1, 2, 3, 4, 5, 6, 7]
```

---

## 18. pop()

**Description**
The `pop()` method removes the last element from an array and returns that element.
The `pop()` method changes the length of the original array.
The `pop()` method returns `undefined` if the array is empty.

**Syntax**
```javascript
array.pop()
```

**Parameters**
None

**Return Value**
| Type | Description |
|------|-------------|
| Any | The removed element from the array. `undefined` if the array is empty. |

**Examples**
```javascript
// Example 1: Remove last fruit
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const lastFruit = fruits.pop();
console.log(lastFruit); // "Mango"
console.log(fruits); // ["Banana", "Orange", "Apple"]

// Example 2: Remove numbers one by one
const numbers = [1, 2, 3, 4, 5];
while(numbers.length > 0) {
  console.log(numbers.pop()); // 5, 4, 3, 2, 1
}
console.log(numbers); // []

// Example 3: Pop from empty array
const emptyArray = [];
const result = emptyArray.pop();
console.log(result); // undefined
console.log(emptyArray); // []
```

---

## 19. push()

**Description**
The `push()` method adds one or more elements to the end of an array and returns the new length of the array.
The `push()` method changes the original array.

**Syntax**
```javascript
array.push(item1, item2, ..., itemX)
```

**Parameters**
| Parameter | Description |
|-----------|-------------|
| item1, item2, ..., itemX | Required. The item(s) to add to the array. |

**Return Value**
| Type | Description |
|------|-------------|
| Number | The new length of the array. |

**Examples**
```javascript
// Example 1: Add single element
const fruits = ["Banana", "Orange", "Apple"];
const newLength = fruits.push("Mango");
console.log(newLength); // 4
console.log(fruits); // ["Banana", "Orange", "Apple", "Mango"]

// Example 2: Add multiple elements
const numbers = [1, 2, 3];
numbers.push(4, 5, 6);
console.log(numbers); // [1, 2, 3, 4, 5, 6]

// Example 3: Build array dynamically
const colors = [];
colors.push('red');
colors.push('green', 'blue');
colors.push('yellow');
console.log(colors); // ['red', 'green', 'blue', 'yellow']
```

---

## 20. length

**Description**
The `length` property sets or returns the number of elements in an array.
The `length` property is always one more than the index of the last element.

**Syntax**
```javascript
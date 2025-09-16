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
- **pop()** -  Remove last element
- **push()** -Add element to end
- **length()** - show the array size
- **shift()** - Remove first element
- **unshift()** - Add element to beginning

# JavaScript Array Methods - Complete Reference Guide

## 1. map()

**Description**
The `map()` method creates a new array filled with the results of calling a function for every array element.
The `map()` method calls a function once for each element in an array.
The `map()` method does not execute the function for empty elements.
The `map()` method does not change the original array.

**Syntax**
```javascript
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

**Example**
```javascript
const numbers = [1, 4, 9, 16];
const doubled = numbers.map(x => x * 2);
console.log(doubled); // [2, 8, 18, 32]
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

**Example**
```javascript
const ages = [32, 33, 16, 40];
const adults = ages.filter(age => age >= 18);
console.log(adults); // [32, 33, 40]
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

**Example**
```javascript
const numbers = [175, 50, 25];
const sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum); // 250
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

**Example**
```javascript
const fruits = ["apple", "orange", "cherry"];
fruits.forEach((fruit, index) => {
  console.log(index + ": " + fruit);
});
// 0: apple
// 1: orange
// 2: cherry
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

**Example**
```javascript
const ages = [3, 10, 18, 20];
const adult = ages.find(age => age >= 18);
console.log(adult); // 18
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

**Example**
```javascript
const ages = [3, 10, 18, 20];
const adultIndex = ages.findIndex(age => age >= 18);
console.log(adultIndex); // 2
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

**Example**
```javascript
const ages = [3, 10, 18, 20];
const hasAdult = ages.some(age => age >= 18);
console.log(hasAdult); // true
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

**Example**
```javascript
const ages = [32, 33, 16, 40];
const allAdults = ages.every(age => age >= 18);
console.log(allAdults); // false
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

**Example**
```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();
console.log(fruits); // ["Apple", "Banana", "Mango", "Orange"]

const numbers = [40, 100, 1, 5, 25, 10];
numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 5, 10, 25, 40, 100]
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

**Example**
```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.reverse();
console.log(fruits); // ["Mango", "Apple", "Orange", "Banana"]
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

**Example**
```javascript
const fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
const citrus = fruits.slice(1, 3);
console.log(citrus); // ["Orange", "Lemon"]
console.log(fruits); // Original array unchanged
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

**Example**
```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const removed = fruits.splice(2, 1, "Lemon", "Kiwi");
console.log(fruits); // ["Banana", "Orange", "Lemon", "Kiwi", "Mango"]
console.log(removed); // ["Apple"]
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

**Example**
```javascript
const arr1 = ["a", "b", "c"];
const arr2 = ["d", "e", "f"];
const arr3 = arr1.concat(arr2);
console.log(arr3); // ["a", "b", "c", "d", "e", "f"]
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

**Example**
```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(fruits.includes("Mango")); // true
console.log(fruits.includes("mango")); // false (case sensitive)
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

**Example**
```javascript
const fruits = ["Apple", "Orange", "Apple", "Mango"];
console.log(fruits.indexOf("Apple")); // 0
console.log(fruits.indexOf("Apple", 1)); // 2
console.log(fruits.indexOf("Banana")); // -1
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

**Example**
```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
console.log(fruits.join()); // "Banana,Orange,Apple,Mango"
console.log(fruits.join(" - ")); // "Banana - Orange - Apple - Mango"
console.log(fruits.join("")); // "BananaOrangeAppleMango"
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

**Example**
```javascript
const arr1 = [0, 1, 2, [3, 4]];
console.log(arr1.flat()); // [0, 1, 2, 3, 4]

const arr2 = [0, 1, 2, [[[3, 4]]]];
console.log(arr2.flat(2)); // [0, 1, 2, [3, 4]]
console.log(arr2.flat(Infinity)); // [0, 1, 2, 3, 4]
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

Reference ke liye ye guide save kar len aur practice karte rahen!
# Unit and Type Conversions

A comprehensive React utility library for common type and unit conversions used in modern web development projects.

## Overview

This library provides a collection of pure functions for converting between different data types and units commonly encountered in React applications, including CSS units, strings, numbers, dates, and data structures.

## Installation

Copy the `UnitAndTypeConversions.jsx` file into your project's utility folder:

```bash
src/utils/UnitAndTypeConversions.jsx
```

## Usage

Import the functions you need:

```javascript
import { 
  pxToNumber, 
  numberToPx, 
  stringToBoolean,
  dateToString 
} from './utils/UnitAndTypeConversions';
```

## API Reference

### String to Number Conversions

Convert CSS units and strings to numeric values.

**`pxToNumber(value: string): number`**
```javascript
pxToNumber("1280px") // Returns: 1280
```

**`remToNumber(value: string): number`**
```javascript
remToNumber("1.5rem") // Returns: 1.5
```

**`percentToNumber(value: string): number`**
```javascript
percentToNumber("75%") // Returns: 75
```

**`stringToInt(value: string): number`**
```javascript
stringToInt("00123") // Returns: 123
```

**`stringToFloat(value: string): number`**
```javascript
stringToFloat("12.45") // Returns: 12.45
```

### Number to String with Units

Convert numeric values to formatted CSS unit strings.

**`numberToPx(value: number): string`**
```javascript
numberToPx(1280) // Returns: "1280px"
```

**`numberToRem(value: number): string`**
```javascript
numberToRem(1.5) // Returns: "1.5rem"
```

**`numberToPercent(value: number): string`**
```javascript
numberToPercent(75) // Returns: "75%"
```

### Boolean Conversions

Handle boolean values in forms and URL parameters.

**`stringToBoolean(value: string): boolean`**
```javascript
stringToBoolean("true") // Returns: true
stringToBoolean("false") // Returns: false
```

**`booleanToString(value: boolean): string`**
```javascript
booleanToString(true) // Returns: "true"
```

### Date Conversions

Convert between date strings and Date objects.

**`stringToDate(value: string): Date`**
```javascript
stringToDate("2026-01-08") // Returns: Date object
```

**`dateToString(date: Date): string`**
```javascript
dateToString(new Date()) // Returns: "2026-01-08"
```

### Array and CSV Conversions

Convert between arrays and comma-separated values.

**`csvToNumberArray(value: string): number[]`**
```javascript
csvToNumberArray("1,2,3") // Returns: [1, 2, 3]
```

**`arrayToCsv(arr: any[]): string`**
```javascript
arrayToCsv([1, 2, 3]) // Returns: "1,2,3"
```

### JSON Conversions

Parse and stringify JSON for API interactions.

**`jsonToObject(value: string): object`**
```javascript
jsonToObject('{"name":"John"}') // Returns: { name: "John" }
```

**`objectToJson(value: object): string`**
```javascript
objectToJson({ name: "John" }) // Returns: '{"name":"John"}'
```

### Local Storage Helpers

Simplified localStorage operations with automatic JSON handling.

**`saveToLocalStorage(key: string, value: any): void`**
```javascript
saveToLocalStorage("user", { name: "John", age: 30 })
```

**`getFromLocalStorage(key: string): any`**
```javascript
getFromLocalStorage("user") // Returns: { name: "John", age: 30 }
```

## Common Use Cases

### Responsive Design

```javascript
const breakpoint = "1280px";
const width = pxToNumber(breakpoint);

if (window.innerWidth < width) {
  // Mobile layout
}
```

### Form Handling

```javascript
const formData = {
  isActive: stringToBoolean(urlParams.get('active')),
  startDate: stringToDate(formValues.startDate)
};
```

### CSS-in-JS

```javascript
const spacing = 16;
const styles = {
  padding: numberToPx(spacing),
  fontSize: numberToRem(1.25),
  width: numberToPercent(100)
};
```

### API Data Processing

```javascript
const apiResponse = '{"users":[1,2,3]}';
const data = jsonToObject(apiResponse);
```

## Demo Component

The library includes a demo component showing basic usage:

```javascript
import ConversionDemo from './utils/UnitAndTypeConversions';

function App() {
  return <ConversionDemo />;
}
```

## Features

- Zero dependencies (React only)
- Pure functions with no side effects
- TypeScript-ready (add type definitions as needed)
- Lightweight and tree-shakeable
- Comprehensive coverage of common conversions

## Browser Support

Works in all modern browsers that support ES6+.

## Contributing

Contributions are welcome! Feel free to submit issues or pull requests.

## License

MIT License - feel free to use in personal and commercial projects.
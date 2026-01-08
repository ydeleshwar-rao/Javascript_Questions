# JavaScript Tricky Questions - Interview Prep 🚀

Quick reference guide for JavaScript interview questions with explanations.

---

## 📌 Type Conversion & Coercion

### Q1: What is the output of `1 + 2 + "2"`?
**Answer:** `"32"`

**Why?** 
- First `1 + 2` = `3` (number addition)
- Then `3 + "2"` = `"32"` (string concatenation)
- JS evaluates left to right, converts number to string when it meets a string

### Q2: What is the output of `"1" + 2 + 2`?
**Answer:** `"122"`

**Why?**
- First operand is string `"1"`
- All `+` operations become concatenation
- `"1" + 2` = `"12"`, then `"12" + 2` = `"122"`

### Q3: What does `+true` return?
**Answer:** `1`

**Why?**
- Unary `+` operator converts boolean to number
- `true` → `1`, `false` → `0`
- **Avoid this in production code!** It's confusing and not readable

### Q4: What is `Number("33abc")`?
**Answer:** `NaN`

**Why?**
- Can't convert alphanumeric string to valid number
- Returns `NaN` (Not a Number)
- **Tricky part:** `typeof NaN` is `"number"` ⚠️

### Q5: What does `Number(null)` return?
**Answer:** `0`

**Why?**
- `null` is explicitly converted to `0`
- But `Number(undefined)` returns `NaN`
- Different handling for null vs undefined

---

## ⚖️ Comparison Operators

### Q6: What is the result of `null >= 0`?
**Answer:** `true`

**Why?**
- Comparison operators convert `null` to `0`
- So it becomes `0 >= 0` which is `true`
- But `null == 0` is `false` (equality works differently!)

### Q7: Why is `null == 0` false but `null >= 0` true?
**Answer:** Different mechanisms

**Why?**
- `==` (equality) doesn't convert null to number
- `>=` (comparison) converts null to `0`
- This is a major JavaScript inconsistency!

### Q8: What does `undefined == null` return?
**Answer:** `true`

**Why?**
- Special case in JS equality rules
- They are loosely equal but not strictly equal
- `undefined === null` returns `false`

### Q9: What is `"2" == 2`?
**Answer:** `true`

**Why?**
- `==` performs type coercion
- String `"2"` is converted to number `2`
- Use `===` to avoid this: `"2" === 2` returns `false`

### Q10: What does `undefined > 0` return?
**Answer:** `false`

**Why?**
- `undefined` in comparisons always returns `false`
- More predictable than `null`
- All comparisons with undefined: `>`, `<`, `==` return `false`

---

## 🔢 Operations & Precedence

### Q11: What is `3 + 4 * 5 % 3`?
**Answer:** `5`

**Why?**
- Order: `5 % 3` = `2`, then `4 * 2` = `8`, then `3 + 8` = `11`
- Wait, actually: `4 * 5` = `20`, then `20 % 3` = `2`, then `3 + 2` = `5`
- **Always use parentheses!** Makes intent clear

### Q12: What happens in `let a = b = c = 5`?
**Answer:** All variables = `5`, but b and c become global!

**Why?**
- Chained assignment evaluates right to left
- Without `let/const` for b and c, they become global
- **Bad practice!** Declare each variable separately

### Q13: What is `++x` vs `x++`?
**Answer:** Prefix vs Postfix increment

**Why?**
- `++x`: increment first, then return (pre-increment)
- `x++`: return first, then increment (post-increment)
- Example: `let x = 5; console.log(++x)` → `6`, `console.log(x++)` → `5` (then x becomes 6)

### Q14: What is `+""`?
**Answer:** `0`

**Why?**
- Unary `+` converts empty string to number
- Empty string → `0`
- **Tricky and confusing!** Avoid in real code

### Q15: What is the result of `true + true`?
**Answer:** `2`

**Why?**
- Both `true` values convert to `1`
- `1 + 1` = `2`
- Type coercion in action

---

## 🔄 Boolean Conversion

### Q16: What does `Boolean("")` return?
**Answer:** `false`

**Why?**
- Empty string is falsy value
- Falsy values: `0`, `""`, `null`, `undefined`, `NaN`, `false`

### Q17: What is `Boolean("0")`?
**Answer:** `true`

**Why?**
- Non-empty string, even with "0", is truthy
- Only empty string `""` is falsy
- Common mistake in conditionals!

### Q18: What does `Boolean([])`?
**Answer:** `true`

**Why?**
- Empty array is still an object
- All objects are truthy
- Even `Boolean({})` is `true`

### Q19: What is `Boolean(NaN)`?
**Answer:** `false`

**Why?**
- `NaN` is one of the falsy values
- Even though `typeof NaN` is `"number"`

### Q20: What does `1 == true` return?
**Answer:** `true`

**Why?**
- `true` converts to `1` in loose comparison
- `1 == 1` becomes `true`
- But `2 == true` is `false` (2 ≠ 1)

---

## 🎯 Strict vs Loose Equality

### Q21: What is `"2" === 2`?
**Answer:** `false`

**Why?**
- `===` checks value AND type
- String vs Number: different types
- No type coercion with strict equality

### Q22: What does `null === undefined` return?
**Answer:** `false`

**Why?**
- Different types in strict comparison
- `null` is type "object", `undefined` is type "undefined"
- Use `===` for predictable code

### Q23: What is `0 == false`?
**Answer:** `true`

**Why?**
- `false` converts to `0`
- `0 == 0` is `true`
- Use `===` to avoid: `0 === false` is `false`

### Q24: What does `[] == false` return?
**Answer:** `true` 😱

**Why?**
- Array converts to empty string `""`
- Empty string converts to `0`
- `false` converts to `0`
- `0 == 0` is `true`
- **This is why we use `===`!**

### Q25: What is `null == undefined`?
**Answer:** `true`

**Why?**
- Special rule in JS: they are loosely equal
- But not strictly equal: `null === undefined` is `false`
- Only equal to each other, nothing else

---

## 💡 Pro Tips for Interviews

### Best Practices:
1. **Always use `===` instead of `==`** - Avoid type coercion bugs
2. **Use parentheses** in complex expressions - Makes intent clear
3. **Avoid tricky conversions** like `+true` or `+""`- Write readable code
4. **Check data types** with `typeof` before operations
5. **Never compare different types** - Convert explicitly first

### Red Flags to Avoid:
- ❌ Chained assignments: `a = b = c = 5`
- ❌ Implicit conversions: `+true`, `+""`
- ❌ Complex precedence: `3 + 4 * 5 % 3`
- ❌ Comparing null/undefined without strict equality
- ❌ Loose equality `==` in production code

### Quick Reference:
```javascript
// Falsy values (6 total)
false, 0, "", null, undefined, NaN

// Everything else is truthy
true, 1, "0", " ", [], {}, function(){}

// Type conversion shortcuts
Number("33")    → 33
Number("33abc") → NaN
Number(null)    → 0
Number(undefined) → NaN
Number(true)    → 1
Number(false)   → 0

String(33)      → "33"
Boolean(1)      → true
Boolean(0)      → false
Boolean("")     → false
Boolean("0")    → true
```

---

## 🎓 Remember:
**"Clean code > Clever code"**

In interviews, explain WHY something happens, not just the answer. Show you understand JavaScript's type coercion and comparison mechanisms!

---

*Good luck with your interviews! 🚀*
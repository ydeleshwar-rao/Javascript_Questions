# 🎯 Story-Based Coding Interview Problems

> Real-world scenarios from actual company interviews with detailed constraints and edge cases

[![Difficulty](https://img.shields.io/badge/Difficulty-Easy%20to%20Hard-orange)]()
[![Problems](https://img.shields.io/badge/Problems-11-blue)]()
[![JavaScript](https://img.shields.io/badge/Language-JavaScript-yellow)]()

## 📚 Table of Contents

- [About](#about)
- [Problem List](#problem-list)
- [How to Use](#how-to-use)
- [Problem Format](#problem-format)
- [Solutions](#solutions)
- [Contributing](#contributing)

---

## 🎓 About

This repository contains **11 story-based coding problems** that simulate real interview scenarios from various companies. Each problem is presented with:

- **Real-world context** from actual companies
- **Detailed constraints** and edge cases
- **Multiple test cases** with expected outputs
- **Hints** for solving
- **Clean solution** with explanations

These problems focus on **date manipulation**, **array processing**, and **business logic** - common themes in technical interviews.

---

## 📋 Problem List

| # | Problem | Difficulty | Topics |
|---|---------|-----------|---------|
| 0 | [HR Recruitment Portal](#problem-0-hr-recruitment-portal) | Easy | Date Comparison, Arrays |
| 1 | [Birthday Reminder System](#problem-1-birthday-reminder-system) | Easy | Date Arithmetic, Days Calculation |
| 2 | [School Admission System](#problem-2-school-admission-system) | Easy | Age Calculation, Cutoff Dates |
| 3 | [Sports Tournament Registration](#problem-3-sports-tournament-registration) | Medium | Age Categories, Validation |
| 4 | [Credit Card Expiry Validator](#problem-4-credit-card-expiry-validator) | Easy | Date Validation, Month-End Logic |
| 5 | [Leave Management System](#problem-5-leave-management-system) | Medium | Future Date Validation, Range Check |
| 6 | [Event Ticket Booking](#problem-6-event-ticket-booking) | Medium | Age Restrictions, Multiple Criteria |
| 7 | [Vaccination Schedule Checker](#problem-7-vaccination-schedule-checker) | Medium | Age in Months, Array Filtering |
| 8 | [Scholarship Deadline Tracker](#problem-8-scholarship-deadline-tracker) | Hard | Multi-Criteria Filtering, Objects |
| 9 | [Driver License Renewal](#problem-9-driver-license-renewal) | Medium | Date Arithmetic, Renewal Logic |
| 10 | [Loan Eligibility Checker](#problem-10-loan-eligibility-checker) | Hard | Complex Multi-Factor Validation |

---

## 🔥 Problem #0: HR Recruitment Portal

**Difficulty:** `Easy` | **Topics:** `Date Comparison`, `Arrays`, `Range Check`

### Problem Statement

You are building an automated screening system for a campus recruitment drive. The HR department needs to filter candidates based on their date of birth. 

You will receive a candidate's date of birth as an array of 8 digits in the format `[D, D, M, M, Y, Y, Y, Y]` where:
- First 2 digits represent the day
- Next 2 digits represent the month  
- Last 4 digits represent the year

**Your task:** Determine if the candidate is eligible based on their birth date falling within a specific range.

### Constraints

- `dobArr.length == 8`
- `1 ≤ day ≤ 31`
- `1 ≤ month ≤ 12`
- `1900 ≤ year ≤ 2100`
- Eligible range: **01/02/1990** to **20/09/1995** (inclusive)
- Input is always a valid date

### Input Format

```javascript
dobArr = [D, D, M, M, Y, Y, Y, Y]
```

### Output Format

Return a string: `"Eligible"` or `"Not Eligible"`

### Examples

**Example 1:**
```javascript
Input: dobArr = [0,1,1,2,1,9,9,5]
Output: "Eligible"
Explanation: Date is 01/12/1995, which falls within the range.
```

**Example 2:**
```javascript
Input: dobArr = [0,1,0,2,1,9,9,0]
Output: "Eligible"
Explanation: Date is 01/02/1990, which is the start boundary (inclusive).
```

**Example 3:**
```javascript
Input: dobArr = [2,1,0,9,1,9,9,5]
Output: "Not Eligible"
Explanation: Date is 21/09/1995, which exceeds the end boundary of 20/09/1995.
```

**Example 4:**
```javascript
Input: dobArr = [3,1,0,1,1,9,9,0]
Output: "Not Eligible"
Explanation: Date is 31/01/1990, which is before the start date.
```

### Function Signature

```javascript
/**
 * @param {number[]} dobArr - Date of birth as [D,D,M,M,Y,Y,Y,Y]
 * @return {string} - "Eligible" or "Not Eligible"
 */
function checkEligibility(dobArr) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Convert the array into a comparable format (YYYYMMDD works well for numerical comparison)
2. Define your boundary dates in the same format
3. Use simple numerical comparison for range checking
4. Remember: both boundaries are inclusive

</details>

---

## 🎂 Problem #1: Birthday Reminder System

**Difficulty:** `Easy` | **Topics:** `Date Objects`, `Milliseconds`, `Modular Arithmetic`

### Problem Statement

You are developing a birthday reminder feature for a social media application. The system needs to calculate how many days remain until a user's next birthday to send timely notifications.

Given:
- User's date of birth as an array: `[D, D, M, M, Y, Y, Y, Y]`
- Today's date as an array: `[D, D, M, M, Y, Y, Y, Y]`

**Your task:** Calculate the number of days remaining until the user's next birthday.

### Constraints

- `dobArr.length == 8`
- `todayArr.length == 8`
- Both inputs represent valid dates
- If today is the user's birthday, return `0`
- If the birthday has already occurred this year, calculate days until next year's birthday
- `1 ≤ day ≤ 31`
- `1 ≤ month ≤ 12`
- `1900 ≤ year ≤ 2100`

### Input Format

```javascript
dobArr = [D, D, M, M, Y, Y, Y, Y]  // User's birth date
todayArr = [D, D, M, M, Y, Y, Y, Y]  // Current date
```

### Output Format

Return an integer representing days until next birthday.

### Examples

**Example 1:**
```javascript
Input: dobArr = [1,5,0,6,1,9,9,5], todayArr = [1,0,0,6,2,0,2,4]
Output: 5
Explanation: Birthday is June 15th, today is June 10th, 2024. 5 days remaining.
```

**Example 2:**
```javascript
Input: dobArr = [0,1,0,1,2,0,0,0], todayArr = [3,1,1,2,2,0,2,4]
Output: 1
Explanation: Birthday is January 1st, today is Dec 31st, 2024. Next birthday is tomorrow.
```

**Example 3:**
```javascript
Input: dobArr = [1,0,0,6,1,9,9,5], todayArr = [1,0,0,6,2,0,2,4]
Output: 0
Explanation: Today is the birthday!
```

**Example 4:**
```javascript
Input: dobArr = [0,1,0,3,1,9,9,5], todayArr = [1,5,0,6,2,0,2,4]
Output: 260
Explanation: Birthday (March 1st) already passed this year. Calculate days until next year.
```

### Function Signature

```javascript
/**
 * @param {number[]} dobArr - User's date of birth
 * @param {number[]} todayArr - Current date
 * @return {number} - Days until next birthday
 */
function daysUntilBirthday(dobArr, todayArr) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Create Date objects from both arrays
2. Set the birthday to the current year initially
3. If birthday has already passed this year, add 1 to the year
4. Calculate difference in milliseconds: `nextBirthday.getTime() - today.getTime()`
5. Convert milliseconds to days: `ms / (1000 * 60 * 60 * 24)`
6. Use `Math.ceil()` or `Math.floor()` appropriately

</details>

---

## 🏫 Problem #2: School Admission System

**Difficulty:** `Easy` | **Topics:** `Age Calculation`, `Date Comparison`, `Cutoff Logic`

### Problem Statement

An EdTech platform needs to determine grade eligibility for school admissions. Schools have age requirements with specific cutoff dates (typically August 1st of the admission year).

A child must be a certain age **on or before** the cutoff date to be eligible for that grade.

**Your task:** Given a child's DOB, cutoff date, and required minimum age, determine if the child is eligible.

### Constraints

- `childDOB.length == 8`
- `cutoffDate.length == 8`
- `0 ≤ requiredAge ≤ 18`
- All inputs represent valid dates
- Child's DOB will always be before the cutoff date
- `1900 ≤ year ≤ 2100`

### Input Format

```javascript
childDOB = [D, D, M, M, Y, Y, Y, Y]
cutoffDate = [D, D, M, M, Y, Y, Y, Y]
requiredAge = integer
```

### Output Format

Return a boolean: `true` if eligible, `false` otherwise.

### Examples

**Example 1:**
```javascript
Input: childDOB = [3,1,0,7,2,0,1,9], cutoffDate = [0,1,0,8,2,0,2,4], requiredAge = 5
Output: true
Explanation: Child turns 5 on July 31, 2019. By Aug 1, 2024 cutoff, child is 5 years old.
```

**Example 2:**
```javascript
Input: childDOB = [0,2,0,8,2,0,1,9], cutoffDate = [0,1,0,8,2,0,2,4], requiredAge = 5
Output: false
Explanation: Child turns 5 on August 2, 2019. On Aug 1, 2024 cutoff, child is still 4.
```

**Example 3:**
```javascript
Input: childDOB = [0,1,0,8,2,0,1,9], cutoffDate = [0,1,0,8,2,0,2,4], requiredAge = 5
Output: true
Explanation: Child turns 5 exactly on the cutoff date. Boundary is inclusive.
```

**Example 4:**
```javascript
Input: childDOB = [1,5,0,9,2,0,1,8], cutoffDate = [0,1,0,8,2,0,2,4], requiredAge = 6
Output: true
Explanation: Child born Sept 15, 2018 is 5 years old on cutoff, but required age is 6. False.
Output: false
```

### Function Signature

```javascript
/**
 * @param {number[]} childDOB - Child's date of birth
 * @param {number[]} cutoffDate - School year cutoff date
 * @param {number} requiredAge - Minimum age requirement
 * @return {boolean} - Eligibility status
 */
function isEligibleForGrade(childDOB, cutoffDate, requiredAge) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Calculate child's age as of the cutoff date, not today's date
2. Age calculation: `cutoffYear - birthYear`
3. Adjust if birthday hasn't occurred yet by cutoff: if `(cutoffMonth < birthMonth)` OR `(cutoffMonth == birthMonth && cutoffDay < birthDay)`, subtract 1 from age
4. Compare calculated age with required age

</details>

---

## 🏆 Problem #3: Sports Tournament Registration

**Difficulty:** `Medium` | **Topics:** `Age Categories`, `Date Validation`, `Mapping`

### Problem Statement

A sports league organizes age-group tournaments. Players must register for the correct age category based on their birth date.

Age categories are defined as:
- **U15 (Under-15):** Born on or after January 1, 2009
- **U18 (Under-18):** Born on or after January 1, 2006
- **U21 (Under-21):** Born on or after January 1, 2003

**Important:** "Under-X" means the player must be **younger than X** on the tournament date. A player who turns X years old on the tournament date is **NOT** eligible for Under-X category.

**Your task:** Validate if a player is eligible for their registered category.

### Constraints

- `playerDOB.length == 8`
- `tournamentDate.length == 8`
- `category ∈ {"U15", "U18", "U21"}`
- All inputs represent valid dates
- Tournament date is always after player's DOB
- `1990 ≤ birth year ≤ 2015`

### Input Format

```javascript
playerDOB = [D, D, M, M, Y, Y, Y, Y]
category = "U15" | "U18" | "U21"
tournamentDate = [D, D, M, M, Y, Y, Y, Y]
```

### Output Format

Return a boolean: `true` if eligible, `false` otherwise.

### Examples

**Example 1:**
```javascript
Input: playerDOB = [3,1,1,2,2,0,0,5], category = "U18", tournamentDate = [0,1,0,1,2,0,2,4]
Output: true
Explanation: Player born Dec 31, 2005. On Jan 1, 2024, they are 18 years 1 day old. 
Still under 18 years on tournament day, eligible for U18.
```

**Example 2:**
```javascript
Input: playerDOB = [0,1,0,1,2,0,0,6], category = "U18", tournamentDate = [0,1,0,1,2,0,2,4]
Output: false
Explanation: Player born Jan 1, 2006. On Jan 1, 2024, they turn exactly 18. 
No longer "under 18", ineligible for U18.
```

**Example 3:**
```javascript
Input: playerDOB = [1,5,0,6,2,0,0,3], category = "U21", tournamentDate = [0,1,0,1,2,0,2,4]
Output: true
Explanation: Player born June 15, 2003. On Jan 1, 2024, they are 20 years old. Eligible for U21.
```

**Example 4:**
```javascript
Input: playerDOB = [0,1,0,1,2,0,0,9], category = "U15", tournamentDate = [0,2,0,1,2,0,2,4]
Output: false
Explanation: Player born Jan 1, 2009. On Jan 2, 2024, they are 15 years 1 day old. Not eligible for U15.
```

### Function Signature

```javascript
/**
 * @param {number[]} playerDOB - Player's date of birth
 * @param {string} category - Age category ("U15", "U18", or "U21")
 * @param {number[]} tournamentDate - Tournament date
 * @return {boolean} - Eligibility status
 */
function isEligibleForCategory(playerDOB, category, tournamentDate) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Map categories to cutoff years: `{U15: 2009, U18: 2006, U21: 2003}`
2. Convert dates to YYYYMMDD format for comparison
3. Player eligible if: `playerDateNum >= cutoffDateNum`
4. Remember: "Under-X" means strictly less than X years old

</details>

---

## 💳 Problem #4: Credit Card Expiry Validator

**Difficulty:** `Easy` | **Topics:** `Date Validation`, `Month-End Logic`, `Comparison`

### Problem Statement

A fintech payment gateway needs to validate credit card expiry dates. Cards are valid through the **last day** of the expiry month shown.

The legacy system stores expiry dates as: `[M, M, Y, Y]` where:
- First 2 digits: Month (01-12)
- Last 2 digits: Year (00-99, representing 2000-2099)

**Important:** A card showing "03/25" is valid throughout March 2025 and expires at 11:59 PM on March 31, 2025.

**Your task:** Determine if a card is still valid on a given date.

### Constraints

- `expiryArr.length == 4`
- `currentDateArr.length == 8`
- `1 ≤ expiryMonth ≤ 12`
- `0 ≤ expiryYear ≤ 99` (represents 2000-2099)
- All inputs represent valid dates
- Current date year is between 2000-2099

### Input Format

```javascript
expiryArr = [M, M, Y, Y]
currentDateArr = [D, D, M, M, Y, Y, Y, Y]
```

### Output Format

Return a boolean: `true` if card is valid, `false` if expired.

### Examples

**Example 1:**
```javascript
Input: expiryArr = [0,3,2,5], currentDateArr = [1,5,0,2,2,0,2,5]
Output: true
Explanation: Card expires March 2025. Today is Feb 15, 2025. Still valid.
```

**Example 2:**
```javascript
Input: expiryArr = [0,3,2,5], currentDateArr = [3,1,0,3,2,0,2,5]
Output: true
Explanation: Card expires March 2025. Today is March 31, 2025. Valid until end of day.
```

**Example 3:**
```javascript
Input: expiryArr = [0,3,2,5], currentDateArr = [0,1,0,4,2,0,2,5]
Output: false
Explanation: Card expired end of March 2025. Today is April 1, 2025. Expired.
```

**Example 4:**
```javascript
Input: expiryArr = [1,2,2,4], currentDateArr = [0,1,1,2,2,0,2,4]
Output: true
Explanation: Card expires December 2024. Today is Dec 1, 2024. Still valid for the month.
```

### Function Signature

```javascript
/**
 * @param {number[]} expiryArr - Card expiry [M,M,Y,Y]
 * @param {number[]} currentDateArr - Current date [D,D,M,M,Y,Y,Y,Y]
 * @return {boolean} - Is card valid?
 */
function isCardValid(expiryArr, currentDateArr) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Extract expiry month and year from the array
2. Add 2000 to the year (YY → YYYY)
3. Extract current month and year
4. Card valid if: `expiryYear > currentYear` OR `(expiryYear == currentYear AND expiryMonth >= currentMonth)`
5. Day of the month doesn't matter for validation

</details>

---

## 📅 Problem #5: Leave Management System

**Difficulty:** `Medium` | **Topics:** `Date Arithmetic`, `Future Date Validation`, `Milliseconds`

### Problem Statement

An HR software system has a business rule: employees cannot apply for leave more than a specified number of days in advance (default: 90 days).

**Your task:** Validate if a requested leave date falls within the allowed future range.

### Constraints

- `todayArr.length == 8`
- `leaveDateArr.length == 8`
- `1 ≤ maxDaysAhead ≤ 365`
- Both inputs represent valid dates
- Leave date is always in the future (after today)
- `2000 ≤ year ≤ 2100`

### Input Format

```javascript
todayArr = [D, D, M, M, Y, Y, Y, Y]
leaveDateArr = [D, D, M, M, Y, Y, Y, Y]
maxDaysAhead = integer (default: 90)
```

### Output Format

Return a boolean: `true` if valid (within range), `false` if too far in future.

### Examples

**Example 1:**
```javascript
Input: todayArr = [0,1,0,1,2,0,2,4], leaveDateArr = [3,1,0,1,2,0,2,4], maxDaysAhead = 90
Output: true
Explanation: Leave is 30 days ahead (Jan 31 from Jan 1). Within 90-day limit.
```

**Example 2:**
```javascript
Input: todayArr = [0,1,0,1,2,0,2,4], leaveDateArr = [3,1,0,3,2,0,2,4], maxDaysAhead = 90
Output: true
Explanation: Leave is exactly 90 days ahead (March 31 from Jan 1). Boundary inclusive.
```

**Example 3:**
```javascript
Input: todayArr = [0,1,0,1,2,0,2,4], leaveDateArr = [0,1,0,4,2,0,2,4], maxDaysAhead = 90
Output: false
Explanation: Leave is 91 days ahead (April 1 from Jan 1). Exceeds 90-day limit.
```

**Example 4:**
```javascript
Input: todayArr = [1,5,0,6,2,0,2,4], leaveDateArr = [0,1,0,7,2,0,2,4], maxDaysAhead = 30
Output: true
Explanation: Leave is 16 days ahead. Within 30-day custom limit.
```

### Function Signature

```javascript
/**
 * @param {number[]} todayArr - Current date
 * @param {number[]} leaveDateArr - Requested leave date
 * @param {number} maxDaysAhead - Maximum days in advance (default 90)
 * @return {boolean} - Is leave date valid?
 */
function isValidLeaveDate(todayArr, leaveDateArr, maxDaysAhead = 90) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Create Date objects from both arrays
2. Calculate difference: `leaveDateObj.getTime() - todayObj.getTime()`
3. Convert milliseconds to days: `diffMs / (1000 * 60 * 60 * 24)`
4. Check if `daysDifference <= maxDaysAhead`
5. Use `Math.floor()` to handle partial days

</details>

---

## 🎟️ Problem #6: Event Ticket Booking

**Difficulty:** `Medium` | **Topics:** `Age Calculation`, `Multi-Criteria Validation`, `Objects`

### Problem Statement

A ticketing platform sells concert tickets with age restrictions and youth discounts. 

Rules:
- Events may have minimum age requirements (18+ or 21+)
- Youth discount applies to attendees under 25 years old **on the event date**

**Your task:** Determine both eligibility and discount status for a customer.

### Constraints

- `dobArr.length == 8`
- `eventDateArr.length == 8`
- `minAge ∈ {18, 21}`
- All inputs represent valid dates
- Event date is always in the future
- `1950 ≤ birth year ≤ 2010`

### Input Format

```javascript
dobArr = [D, D, M, M, Y, Y, Y, Y]
eventDateArr = [D, D, M, M, Y, Y, Y, Y]
minAge = 18 | 21
```

### Output Format

Return an object: `{eligible: boolean, youthDiscount: boolean}`

### Examples

**Example 1:**
```javascript
Input: dobArr = [0,1,0,1,2,0,0,7], eventDateArr = [3,1,1,2,2,0,2,4], minAge = 18
Output: {eligible: false, youthDiscount: false}
Explanation: Customer born Jan 1, 2007 will be 17 on Dec 31, 2024. Not eligible for 18+ event.
```

**Example 2:**
```javascript
Input: dobArr = [0,1,0,1,2,0,0,0], eventDateArr = [3,1,1,2,2,0,2,4], minAge = 18
Output: {eligible: true, youthDiscount: true}
Explanation: Customer born Jan 1, 2000 will be 24 on event date. Eligible and gets youth discount.
```

**Example 3:**
```javascript
Input: dobArr = [0,1,0,1,1,9,9,8], eventDateArr = [3,1,1,2,2,0,2,4], minAge = 21
Output: {eligible: true, youthDiscount: false}
Explanation: Customer born Jan 1, 1998 will be 26 on event date. Eligible but no discount.
```

**Example 4:**
```javascript
Input: dobArr = [3,1,1,2,2,0,0,3], eventDateArr = [3,1,1,2,2,0,2,4], minAge = 21
Output: {eligible: true, youthDiscount: true}
Explanation: Customer turns 21 exactly on event date. Eligible for 21+ and under 25 for discount.
```

### Function Signature

```javascript
/**
 * @param {number[]} dobArr - Customer's date of birth
 * @param {number[]} eventDateArr - Event date
 * @param {number} minAge - Minimum age requirement
 * @return {Object} - {eligible: boolean, youthDiscount: boolean}
 */
function checkTicketEligibility(dobArr, eventDateArr, minAge) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Calculate customer's age **on the event date**, not today
2. Use the same age calculation logic from previous problems
3. Check: `age >= minAge` for eligibility
4. Check: `age < 25` for youth discount
5. Both checks are independent - handle separately

</details>

---

## 🏥 Problem #7: Vaccination Schedule Checker

**Difficulty:** `Medium` | **Topics:** `Age in Months`, `Array Filtering`, `Multiple Milestones`

### Problem Statement

A healthcare app helps parents track childhood vaccination schedules. Vaccines are due at specific age milestones:
- 6 months
- 12 months
- 18 months
- 24 months

A vaccine is considered "due" if the child has reached that age (or is older).

**Your task:** Return all vaccine milestones that are currently due for a child.

### Constraints

- `childDOB.length == 8`
- `todayArr.length == 8`
- All inputs represent valid dates
- Child's age is between 0-36 months
- Vaccine schedule: `[6, 12, 18, 24]` months
- Today's date is always after child's DOB

### Input Format

```javascript
childDOB = [D, D, M, M, Y, Y, Y, Y]
todayArr = [D, D, M, M, Y, Y, Y, Y]
```

### Output Format

Return an array of integers representing due vaccine ages in months: `number[]`

### Examples

**Example 1:**
```javascript
Input: childDOB = [1,5,0,1,2,0,2,3], todayArr = [2,0,0,8,2,0,2,3]
Output: [6]
Explanation: Child born Jan 15, 2023. Today is Aug 20, 2023. Age = 7 months. Only 6-month vaccine is due.
```

**Example 2:**
```javascript
Input: childDOB = [1,5,0,1,2,0,2,3], todayArr = [2,0,0,3,2,0,2,4]
Output: [6, 12]
Explanation: Child born Jan 15, 2023. Today is March 20, 2024. Age = 14 months. 6 and 12-month vaccines due.
```

**Example 3:**
```javascript
Input: childDOB = [1,5,0,1,2,0,2,2], todayArr = [2,0,0,2,2,0,2,4]
Output: [6, 12, 18, 24]
Explanation: Child born Jan 15, 2022. Today is Feb 20, 2024. Age = 25 months. All vaccines due.
```

**Example 4:**
```javascript
Input: childDOB = [0,1,0,6,2,0,2,3], todayArr = [3,0,1,1,2,0,2,3]
Output: []
Explanation: Child born June 1, 2023. Today is Nov 30, 2023. Age = 5 months. No vaccines due yet.
```

### Function Signature

```javascript
/**
 * @param {number[]} childDOB - Child's date of birth
 * @param {number[]} todayArr - Current date
 * @return {number[]} - Array of due vaccine ages in months
 */
function getDueVaccines(childDOB, todayArr) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Calculate child's age in months: `(yearDiff * 12) + monthDiff`
2. Adjust for day of month if needed
3. Define vaccine schedule: `const schedule = [6, 12, 18, 24]`
4. Use `filter()` to return only milestones where `child's age >= milestone`
5. Return the filtered array

</details>

---

## 🎓 Problem #8: Scholarship Deadline Tracker

**Difficulty:** `Hard` | **Topics:** `Multi-Criteria Filtering`, `Objects`, `Complex Logic`

### Problem Statement

A university portal lists scholarships with varying eligibility criteria. Each scholarship has:
- Age range (minimum and maximum age)
- Application deadline

A student is eligible for a scholarship if:
1. Their age falls within the scholarship's age range (inclusive)
2. The application deadline has not passed

**Your task:** Return names of all scholarships the student is eligible for.

### Constraints

- `studentDOB.length == 8`
- `todayArr.length == 8`
- `1 ≤ scholarships.length ≤ 20`
- Each scholarship has: `{name: string, deadline: number[8], minAge: number, maxAge: number}`
- `16 ≤ minAge ≤ maxAge ≤ 35`
- All dates are valid
- Scholarship names are unique

### Input Format

```javascript
studentDOB = [D, D, M, M, Y, Y, Y, Y]
todayArr = [D, D, M, M, Y, Y, Y, Y]
scholarships = [
    {name: "Name", deadline: [D,D,M,M,Y,Y,Y,Y], minAge: num, maxAge: num},
    ...
]
```

### Output Format

Return a string array of eligible scholarship names: `string[]`

### Examples

**Example 1:**
```javascript
Input: 
studentDOB = [1,5,0,3,2,0,0,2]
todayArr = [1,0,1,1,2,0,2,4]
scholarships = [
    {name: "Merit Award", deadline: [3,1,1,2,2,0,2,4], minAge: 18, maxAge: 25},
    {name: "Sports Scholarship", deadline: [1,5,1,1,2,0,2,4], minAge: 16, maxAge: 22},
    {name: "Need-Based Grant", deadline: [0,1,0,1,2,0,2,5], minAge: 18, maxAge: 30}
]
Output: ["Merit Award", "Need-Based Grant"]
Explanation: Student is 22 years old on Nov 10, 2024.
- Merit Award: Age ✓ (18-25), Deadline Dec 31, 2024 ✓ → Eligible
- Sports: Age ✓ (16-22), but deadline Nov 15, 2024 already passed ✗ → Not eligible
- Need-Based: Age ✓ (18-30), Deadline Jan 1, 2025 ✓ → Eligible
```

**Example 2:**
```javascript
Input: 
studentDOB = [1,0,0,6,2,0,0,7]
todayArr = [1,0,1,1,2,0,2,4]
scholarships = [
    {name: "Merit Award", deadline: [3,1,1,2,2,0,2,4], minAge: 18, maxAge: 25},
    {name: "Youth Grant", deadline: [3,1,1,2,2,0,2,4], minAge: 16, maxAge: 18}
]
Output: ["Youth Grant"]
Explanation: Student is 17 years old. Only eligible for Youth Grant (age 16-18).
```

**Example 3:**
```javascript
Input: 
studentDOB = [1,0,0,6,1,9,9,8]
todayArr = [1,0,1,1,2,0,2,4]
scholarships = [
    {name: "Merit Award", deadline: [3,1,1,2,2,0,2,4], minAge: 18, maxAge: 25},
    {name: "Need-Based Grant", deadline: [0,1,0,1,2,0,2,5], minAge: 18, maxAge: 30}
]
Output: ["Need-Based Grant"]
Explanation: Student is 26 years old. Too old for Merit Award (max 25), eligible for Need-Based (max 30).
```

### Function Signature

```javascript
/**
 * @param {number[]} studentDOB - Student's date of birth
 * @param {number[]} todayArr - Current date
 * @param {Object[]} scholarships - Array of scholarship objects
 * @return {string[]} - Names of eligible scholarships
 */
function getEligibleScholarships(studentDOB, todayArr, scholarships) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. First, calculate the student's current age
2. For each scholarship, check TWO conditions:
   - Age: `minAge <= studentAge <= maxAge`
   - Deadline: `deadlineDate >= today`
3. Use `filter()` to get eligible scholarships
4. Use `map()` to extract just the names
5. Remember to handle date comparison properly

</details>

---

## 🚗 Problem #9: Driver License Renewal

**Difficulty:** `Medium` | **Topics:** `Date Arithmetic`, `Multi-Output`, `Renewal Logic`

### Problem Statement

A DMV system manages driver license renewals. Licenses are valid for a specific period (default: 10 years) from the issue date.

The system must:
1. Check if the license is still valid
2. Determine if a renewal notice should be sent (90 days before expiry)

**Important:** License issued on March 15, 2015 expires on March 14, 2025 (day before the 10-year anniversary).

**Your task:** Return both the validity status and renewal notice flag.

### Constraints

- `issueDateArr.length == 8`
- `todayArr.length == 8`
- `1 ≤ validityYears ≤ 50`
- All inputs represent valid dates
- Issue date is always before today
- `2000 ≤ year ≤ 2100`

### Input Format

```javascript
issueDateArr = [D, D, M, M, Y, Y, Y, Y]
todayArr = [D, D, M, M, Y, Y, Y, Y]
validityYears = integer (default: 10)
```

### Output Format

Return an object: `{isValid: boolean, renewalNotice: boolean}`

### Examples

**Example 1:**
```javascript
Input: issueDateArr = [1,5,0,3,2,0,1,5], todayArr = [1,0,0,6,2,0,2,4], validityYears = 10
Output: {isValid: true, renewalNotice: false}
Explanation: Issued March 15, 2015. Expires March 14, 2025. Today is June 10, 2024.
Still valid and more than 90 days until expiry.
```

**Example 2:**
```javascript
Input: issueDateArr = [1,5,0,3,2,0,1,5], todayArr = [2,5,1,2,2,0,2,4], validityYears = 10
Output: {isValid: true, renewalNotice: true}
Explanation: Issued March 15, 2015. Expires March 14, 2025. Today is Dec 25, 2024.
Still valid but only 79 days until expiry - send renewal notice!
```

**Example 3:**
```javascript
Input: issueDateArr = [1,5,0,3,2,0,1,5], todayArr = [2,0,0,4,2,0,2,5], validityYears = 10
Output: {isValid: false, renewalNotice: false}
Explanation: Issued March 15, 2015. Expires March 14, 2025. Today is April 20, 2025.
License expired, no point in renewal notice.
```

**Example 4:**
```javascript
Input: issueDateArr = [0,1,0,1,2,0,2,0], todayArr = [3,0,1,2,2,0,2,4], validityYears = 5
Output: {isValid: true, renewalNotice: true}
Explanation: 5-year validity. Issued Jan 1, 2020, expires Dec 31, 2024. Today is Dec 30, 2024.
Valid but only 1 day left - definitely send notice!
```

### Function Signature

```javascript
/**
 * @param {number[]} issueDateArr - License issue date
 * @param {number[]} todayArr - Current date
 * @param {number} validityYears - Validity period in years (default 10)
 * @return {Object} - {isValid: boolean, renewalNotice: boolean}
 */
function checkLicenseStatus(issueDateArr, todayArr, validityYears = 10) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Calculate expiry date: issue date + validity years - 1 day
2. Use `setFullYear()` to add years to a Date object
3. Subtract 1 day: `new Date(date.getTime() - 24*60*60*1000)`
4. Check if today < expiry date for validity
5. Calculate days until expiry, if < 90 and still valid, set renewalNotice to true
6. If already expired, renewalNotice should be false

</details>

---

## 🏦 Problem #10: Loan Eligibility Checker

**Difficulty:** `Hard` | **Topics:** `Complex Multi-Factor Validation`, `Business Logic`, `Edge Cases`

### Problem Statement

A bank's loan eligibility system has complex rules:

**Eligibility Criteria:**
1. Applicant age on application date: **21-60 years** (inclusive)
2. Loan completion date: Applicant must be **≤ 65 years old** when loan ends
3. Retirement buffer: Applicant must have **at least 2 years until retirement** (age 60)

**Your task:** Determine eligibility, provide rejection reason if ineligible, and calculate maximum allowable tenure.

### Constraints

- `dobArr.length == 8`
- `applicationDateArr.length == 8`
- `1 ≤ tenureYears ≤ 40`
- All inputs represent valid dates
- Application date is always after DOB
- `1940 ≤ birth year ≤ 2010`

### Input Format

```javascript
dobArr = [D, D, M, M, Y, Y, Y, Y]
applicationDateArr = [D, D, M, M, Y, Y, Y, Y]
tenureYears = integer
```

### Output Format

Return an object: `{eligible: boolean, reason: string, maxTenure: number}`

Where:
- `eligible`: Whether the loan is approved
- `reason`: "Approved" or specific rejection reason
- `maxTenure`: Maximum years of loan applicant can take

### Examples

**Example 1:**
```javascript
Input: dobArr = [0,1,0,1,1,9,7,0], applicationDateArr = [0,1,0,1,2,0,2,4], tenureYears = 15
Output: {eligible: false, reason: "Loan extends beyond age 65", maxTenure: 11}
Explanation: Applicant born Jan 1, 1970. On Jan 1, 2024, age is 54.
15-year loan means completion at age 69 (exceeds 65).
Max tenure: 65 - 54 = 11 years.
```

**Example 2:**
```javascript
Input: dobArr = [0,1,0,1,1,9,9,4], applicationDateArr = [0,1,0,1,2,0,2,4], tenureYears = 20
Output: {eligible: true, reason: "Approved", maxTenure: 35}
Explanation: Applicant born Jan 1, 1994. On Jan 1, 2024, age is 30.
20-year loan means completion at age 50 (within 65).
Has 30 years until retirement (> 2 years required).
Max tenure: 65 - 30 = 35 years.
```

**Example 3:**
```javascript
Input: dobArr = [0,1,0,1,1,9,6,5], applicationDateArr = [0,1,0,1,2,0,2,4], tenureYears = 5
Output: {eligible: false, reason: "Less than 2 years until retirement", maxTenure: 0}
Explanation: Applicant born Jan 1, 1965. On Jan 1, 2024, age is 59.
Only 1 year until retirement (age 60), needs at least 2 years.
Cannot take any loan.
```

**Example 4:**
```javascript
Input: dobArr = [0,1,0,1,2,0,0,4], applicationDateArr = [0,1,0,1,2,0,2,4], tenureYears = 10
Output: {eligible: false, reason: "Applicant age must be between 21-60", maxTenure: 0}
Explanation: Applicant born Jan 1, 2004. On Jan 1, 2024, age is 20.
Too young (under 21). Cannot apply.
```

**Example 5:**
```javascript
Input: dobArr = [0,1,0,1,1,9,6,3], applicationDateArr = [0,1,0,1,2,0,2,4], tenureYears = 5
Output: {eligible: false, reason: "Applicant age must be between 21-60", maxTenure: 0}
Explanation: Applicant born Jan 1, 1963. On Jan 1, 2024, age is 61.
Too old (over 60). Cannot apply.
```

### Function Signature

```javascript
/**
 * @param {number[]} dobArr - Applicant's date of birth
 * @param {number[]} applicationDateArr - Loan application date
 * @param {number} tenureYears - Requested loan tenure in years
 * @return {Object} - {eligible: boolean, reason: string, maxTenure: number}
 */
function checkLoanEligibility(dobArr, applicationDateArr, tenureYears) {
    // Your code here
}
```

### Hints

<details>
<summary>Click to reveal hints</summary>

1. Calculate age on application date first
2. Check conditions in priority order:
   - First: Age must be 21-60
   - Second: Must have ≥2 years until retirement (age 60)
   - Third: Loan end age must be ≤65
3. For maxTenure calculation:
   - If age < 21 or age > 60: maxTenure = 0
   - If age ≥ 58: maxTenure = 0 (less than 2 years to retirement)
   - Otherwise: maxTenure = 65 - currentAge
4. Return immediately when first failing condition is found
5. If all checks pass: `{eligible: true, reason: "Approved", maxTenure: calculated}`

</details>

---

## 🎯 How to Approach These Problems

### Step 1: Read and Understand
- Read the problem statement carefully
- Identify the real-world context
- Note all constraints and edge cases
- Understand the input/output format

### Step 2: Plan Your Solution
- Break down the problem into smaller steps
- Identify what calculations are needed
- Think about edge cases
- Plan your data transformations

### Step 3: Code Incrementally
```javascript
// 1. Parse input
// 2. Perform calculations
// 3. Apply business rules
// 4. Handle edge cases
// 5. Format output
```

### Step 4: Test Thoroughly
- Test all provided examples
- Test edge cases (boundary values)
- Test invalid scenarios
- Verify output format

---

## 💡 Common Patterns & Techniques

### Array to Date Conversion
```javascript
function arrayToDate(arr) {
    const day = parseInt(`${arr[0]}${arr[1]}`);
    const month = parseInt(`${arr[2]}${arr[3]}`);
    const year = parseInt(`${arr[4]}${arr[5]}${arr[6]}${arr[7]}`);
    return new Date(year, month - 1, day); // month is 0-indexed
}
```

### YYYYMMDD Comparison
```javascript
function arrayToYYYYMMDD(arr) {
    const year = `${arr[4]}${arr[5]}${arr[6]}${arr[7]}`;
    const month = `${arr[2]}${arr[3]}`;
    const day = `${arr[0]}${arr[1]}`;
    return parseInt(year + month + day);
}
```

### Age Calculation
```javascript
function calculateAge(dobDate, referenceDate) {
    let age = referenceDate.getFullYear() - dobDate.getFullYear();
    const monthDiff = referenceDate.getMonth() - dobDate.getMonth();
    
    if (monthDiff < 0 || (monthDiff === 0 && referenceDate.getDate() < dobDate.getDate())) {
        age--;
    }
    return age;
}
```

### Days Between Dates
```javascript
function daysBetween(date1, date2) {
    const diffMs = date2.getTime() - date1.getTime();
    return Math.floor(diffMs / (1000 * 60 * 60 * 24));
}
```

---

## 📚 JavaScript Methods Reference

| Method | Usage | Example |
|--------|-------|---------|
| `Date()` | Create date object | `new Date(2024, 5, 15)` |
| `getTime()` | Get milliseconds | `date.getTime()` |
| `getFullYear()` | Get year | `date.getFullYear()` |
| `getMonth()` | Get month (0-11) | `date.getMonth()` |
| `getDate()` | Get day | `date.getDate()` |
| `setFullYear()` | Set year | `date.setFullYear(2025)` |
| `parseInt()` | String to number | `parseInt("42")` |
| `filter()` | Filter array | `arr.filter(x => x > 5)` |
| `map()` | Transform array | `arr.map(x => x * 2)` |
| `Math.floor()` | Round down | `Math.floor(4.9)` → 4 |

---

## 🎓 Interview Tips

### What Interviewers Look For:

✅ **Ask Clarifying Questions**
- "What should I return if the date is invalid?"
- "Are boundary dates inclusive or exclusive?"
- "Should I handle leap years?"

✅ **Think Out Loud**
- Explain your approach before coding
- Verbalize your thought process
- Discuss trade-offs

✅ **Handle Edge Cases**
- Boundary values (min/max dates)
- Leap years
- Month-end scenarios
- Exact matches

✅ **Write Clean Code**
- Meaningful variable names
- Consistent formatting
- Comments for complex logic
- Modular functions

✅ **Test Your Solution**
- Walk through examples
- Test edge cases
- Verify output format

### Common Mistakes to Avoid:

❌ Not asking about date format assumptions  
❌ Forgetting leap years in calculations  
❌ Off-by-one errors in date ranges  
❌ Not handling boundary conditions  
❌ Hardcoding values instead of parameters  
❌ Ignoring time zones (use UTC if needed)  
❌ Month indexing errors (JS months are 0-11)  

---

## 🚀 Practice Strategy

### Week 1: Fundamentals (Problems #0-3)
- Focus on date parsing and basic comparisons
- Master array-to-date conversion
- Practice age calculations

### Week 2: Intermediate (Problems #4-6)
- Work on multi-criteria validation
- Handle month-end logic
- Practice object returns

### Week 3: Advanced (Problems #7-9)
- Complex date arithmetic
- Array filtering and mapping
- Multi-output scenarios

### Week 4: Expert (Problem #10 + Review)
- Multi-factor business logic
- Comprehensive testing
- Review all previous problems

### Daily Routine:
1. ⏰ **5 min:** Read problem carefully
2. 📝 **5 min:** Plan solution on paper
3. 💻 **15 min:** Code solution
4. ✅ **5 min:** Test with all examples
5. 🔍 **5 min:** Try to break your code

---

## 📖 Solutions

Solutions are provided in the `/solutions` directory with detailed explanations for each problem.

```
solutions/
├── problem-00-hr-recruitment.js
├── problem-01-birthday-reminder.js
├── problem-02-school-admission.js
├── problem-03-sports-tournament.js
├── problem-04-credit-card-expiry.js
├── problem-05-leave-management.js
├── problem-06-event-ticket.js
├── problem-07-vaccination-schedule.js
├── problem-08-scholarship-tracker.js
├── problem-09-license-renewal.js
└── problem-10-loan-eligibility.js
```

Each solution includes:
- Complete working code
- Step-by-step explanation
- Time and space complexity analysis
- Alternative approaches

---

## 🤝 Contributing

Found a bug? Have a better solution? Want to add more problems?

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/new-problem`)
3. Commit your changes (`git commit -m 'Add new problem'`)
4. Push to the branch (`git push origin feature/new-problem`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Problems inspired by real interviews at startups and tech companies
- Special thanks to all contributors
- Interview preparation resources from LeetCode, HackerRank, and InterviewBit

---

## 📬 Contact

Have questions? Found an issue?

- Create an issue on GitHub
- Submit a pull request
- Reach out via discussions

---

## ⭐ Star History

If you find this repository helpful, please consider giving it a star! ⭐

---

**Good Luck with Your Interviews! 🎉**

Remember: Companies don't just test *what* you know, they test *how* you think!

---

<p align="center">
  Made with ❤️ for aspiring developers
</p>

<p align="center">
  <sub>Happy Coding! 💻</sub>
</p>
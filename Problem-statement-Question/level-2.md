# 🧠 Advanced JavaScript Interview Problems - Critical Thinking Edition

High-Level Thinking Problems - Complex scenarios with lengthy requirements, multiple edge cases, and real-world business logic


## 📖 Table of Contents

- Problems 11-20: Advanced Scenarios
- Critical Thinking Framework
- How to Read Complex Problems


## 🎯 Advanced Scenario-Based Problems

### Problem #11: 🏨 Hotel Dynamic Pricing System
**Hints:**
- Maintain inventory with FIFO queue
- Track purchase date for each lot
- Calculate holding period for each sale
- Apply tax rates based on holding period
- Handle corporate actions by adjusting inventory

**JS Methods:**
- `filter()`, `reduce()`, queue operations
- Date difference calculations
- Financial math (percentages, rounding)
- Object state management
- Complex conditional logic

---

### Problem #16: 🚗 **Ride-Sharing Surge Pricing Algorithm**

**📋 Real-World Dynamic Pricing:**

Ride-sharing company. The pricing team explains:

> *"Our surge pricing has multiple factors:*
> 
> **Base Calculation:**
> - *Base fare: ₹50 + ₹12/km + ₹2/min*
> - *Time multipliers: Peak hours (8-10 AM, 6-9 PM) = 1.5x, Night (11 PM-6 AM) = 2x*
> - *Day multipliers: Weekend = 1.2x, Holidays = 1.5x*
> 
> **Dynamic Surge:**
> - *Demand/Supply ratio: >3 = 2x, 2-3 = 1.5x, 1-2 = 1.2x*
> - *Weather: Rain = 1.3x, Heavy rain = 1.8x*
> - *Events nearby: Concert/Match = 1.5x (3 hours before/after)*
> 
> **Discounts:**
> - *First ride: 50% off (max ₹100)*
> - *Referral: 30% off*
> - *Loyalty points: ₹1 = 1 point, max 20% discount*
> - *Promo codes: Various percentages*
> 
> **Rules:**
> - *All surge multipliers compound (multiply)*
> - *Only one user discount (highest value)*
> - *Platform fee: 10% on final price (non-negotiable)*
> - *Driver gets 80% of fare after platform fee*
> - *Minimum fare: ₹99, Maximum surge: 3x*
> 
> *Calculate final price and driver earnings."*

---

**Problem Statement:**
```javascript
/**
 * Calculate ride fare with surge and discounts
 * @param {Object} rideDetails - {
 *   distance: number, // km
 *   duration: number, // minutes
 *   startTime: [D,D,M,M,Y,Y,Y,Y,H,H,M,M],
 *   pickupLocation: {lat: number, lng: number},
 *   weather: 'clear'|'rain'|'heavy-rain'
 * }
 * @param {Object} userData - {
 *   isFirstRide: boolean,
 *   hasReferral: boolean,
 *   loyaltyPoints: number,
 *   promoCode: {code: string, discount: number} | null
 * }
 * @param {Object[]} nearbyEvents - [{
 *   name: string,
 *   location: {lat, lng},
 *   startTime: [...],
 *   endTime: [...]
 * }]
 * @param {number} demandSupplyRatio
 * @param {number[]} holidays - [[D,D,M,M,Y,Y,Y,Y], ...]
 * @returns {Object} {
 *   baseFare: number,
 *   surgeMultiplier: number,
 *   surgeBreakdown: {time, day, demand, weather, event},
 *   fareBeforeDiscount: number,
 *   discount: {type: string, amount: number},
 *   platformFee: number,
 *   finalFare: number,
 *   driverEarnings: number,
 *   breakdown: string[]
 * }
 */
function calculateRideFare(rideDetails, userData, nearbyEvents, demandSupplyRatio, holidays) {
    // Your code here
}
```

**Test Cases:**
```javascript
// Test 1: Peak hour, rainy, high demand
console.log(calculateRideFare(
    {
        distance: 10,
        duration: 25,
        startTime: [1,5,1,2,2,0,2,4,0,8,3,0], // 8:30 AM
        pickupLocation: {lat: 28.6139, lng: 77.2090},
        weather: 'rain'
    },
    {
        isFirstRide: false,
        hasReferral: false,
        loyaltyPoints: 500,
        promoCode: null
    },
    [],
    3.5, // High demand
    []
));
/* Expected: {
    baseFare: 220, // 50 + (10*12) + (25*2)
    surgeMultiplier: 5.85, // 1.5(peak) * 2(demand) * 1.3(rain) = 3.9, capped at 3x
    surgeBreakdown: {
        time: 1.5,
        day: 1.0,
        demand: 2.0,
        weather: 1.3,
        event: 1.0
    },
    fareBeforeDiscount: 660, // 220 * 3 (max surge)
    discount: {type: 'loyalty', amount: 100}, // 500 points, max 20% = 132, but capped
    platformFee: 56, // (660-100) * 0.10
    finalFare: 616,
    driverEarnings: 403.2, // (560 - 56) * 0.8
    breakdown: [...]
} */

// Test 2: First ride with promo on weekend
console.log(calculateRideFare(
    {
        distance: 5,
        duration: 15,
        startTime: [1,4,1,2,2,0,2,4,1,5,0,0], // Saturday 3 PM
        pickupLocation: {lat: 28.6139, lng: 77.2090},
        weather: 'clear'
    },
    {
        isFirstRide: true,
        hasReferral: true,
        loyaltyPoints: 0,
        promoCode: {code: 'SAVE40', discount: 40}
    },
    [],
    1.5,
    []
));
/* Expected:
    - Base: 50 + 60 + 30 = 140
    - Surge: 1.2(weekend) * 1.2(demand) = 1.44
    - Before discount: 201.6
    - Best discount: First ride 50% (100.8) vs Referral 30% (60.48) vs Promo 40% (80.64)
    - Choose: First ride 50% but max 100, so 100
    - Final: ~111 (after minimum fare check)
*/

// Test 3: Concert event nearby at night
console.log(calculateRideFare(
    {
        distance: 3,
        duration: 20,
        startTime: [1,5,1,2,2,0,2,4,2,2,0,0], // 10 PM
        pickupLocation: {lat: 28.6139, lng: 77.2090},
        weather: 'clear'
    },
    {
        isFirstRide: false,
        hasReferral: false,
        loyaltyPoints: 0,
        promoCode: null
    },
    [{
        name: 'Rock Concert',
        location: {lat: 28.6141, lng: 77.2095}, // Very close
        startTime: [1,5,1,2,2,0,2,4,1,9,0,0],
        endTime: [1,5,1,2,2,0,2,4,2,3,0,0]
    }],
    2.2,
    []
));
/* Expected:
    - Night 2x, Event 1.5x, Demand 1.5x
    - Surge: 2 * 1.5 * 1.5 = 4.5, capped at 3x
*/
```

**Hints:**
- Calculate base fare first
- Multiply surge factors, then cap at 3x
- Compare all discount options, take maximum
- Enforce minimum fare ₹99
- Use Haversine formula for event proximity

**JS Methods:**
- Math operations, `Math.min()`, `Math.max()`
- Object destructuring
- Distance calculation (geo formulas)
- Time parsing and comparison
- Conditional chaining

---

### Problem #17: 📚 **Library Fine Calculation System**

**📋 Complex Multi-Factor Penalty System:**

University library. The librarian explains:

> *"Our fine system is educational but strict:*
> 
> **Book Categories:**
> - *Reference (can't issue): In-library only*
> - *Regular: 14-day loan, ₹2/day fine*
> - *Reserved: 3-day loan, ₹5/day fine*
> - *Magazine: 7-day loan, ₹1/day fine*
> 
> **Student Categories:**
> - *Undergraduate: Max 3 books*
> - *Postgraduate: Max 5 books*
> - *Faculty: Max 10 books, no fines (grace)*
> 
> **Fine Rules:**
> - *Weekends/holidays don't count as overdue days*
> - *First offense: 50% discount on fine*
> - *Repeat offender (3+ times): No new issues until paid*
> - *Lost book: Book cost + ₹500 processing + pending fines*
> - *Damaged book: 50% cost + repair estimate*
> 
> **Grace Periods:**
> - *Exam week: +7 days automatic extension*
> - *Festival: +3 days*
> - *Medical leave: Full waiver with certificate*
> 
> **Payment:**
> - *Pay within 7 days: No penalty*
> - *7-30 days: +10% penalty*
> - *>30 days: +25% penalty + library access suspended*
> 
> *Calculate fine with grace periods and penalties."*

---

**Problem Statement:**
```javascript
/**
 * Calculate library fine with all factors
 * @param {Object} studentData - {
 *   id: string,
 *   category: 'UG'|'PG'|'Faculty',
 *   previousOffenses: number,
 *   medicalLeave: {start: [...], end: [...]} | null
 * }
 * @param {Object[]} issuedBooks - [{
 *   bookId: string,
 *   category: 'regular'|'reserved'|'magazine',
 *   issueDate: [D,D,M,M,Y,Y,Y,Y],
 *   dueDate: [D,D,M,M,Y,Y,Y,Y],
 *   returnDate: [D,D,M,M,Y,Y,Y,Y] | null,
 *   status: 'returned'|'lost'|'damaged',
 *   cost: number,
 *   damageEstimate: number
 * }]
 * @param {number[]} currentDate - [D,D,M,M,Y,Y,Y,Y]
 * @param {Object} calendar - {
 *   holidays: [[D,D,M,M,Y,Y,Y,Y], ...],
 *   examWeek: {start: [...], end: [...]},
 *   festivals: [{date: [...], name: string}, ...]
 * }
 * @returns {Object} {
 *   totalFine: number,
 *   breakdown: [{bookId, overdueDays, baseFine, adjustments, finalFine}],
 *   penalties: {latePayment: number},
 *   canIssueNew: boolean,
 *   warnings: string[]
 * }
 */
function calculateLibraryFine(studentData, issuedBooks, currentDate, calendar) {
    // Your code here
}
```

**Test Cases:**
```javascript
// Test 1: UG student, first offense, returned late
console.log(calculateLibraryFine(
    {
        id: 's1',
        category: 'UG',
        previousOffenses: 0,
        medicalLeave: null
    },
    [{
        bookId: 'b1',
        category: 'regular',
        issueDate: [0,1,1,1,2,0,2,4],
        dueDate: [1,5,1,1,2,0,2,4],
        returnDate: [2,5,1,1,2,0,2,4], // 10 days late
        status: 'returned',
        cost: 500,
        damageEstimate: 0
    }],
    [3,0,1,1,2,0,2,4],
    {
        holidays: [[1,6,1,1,2,0,2,4], [1,7,1,1,2,0,2,4]], // Weekend
        examWeek: null,
        festivals: []
    }
));
/* Expected: {
    totalFine: 8, // 10 days - 2 (weekend) = 8 days * ₹2 * 50% (first offense)
    breakdown: [{
        bookId: 'b1',
        overdueDays: 8,
        baseFine: 16,
        adjustments: [{type: 'first-offense', discount: -8}],
        finalFine: 8
    }],
    penalties: {latePayment: 0},
    canIssueNew: true,
    warnings: []
} */

// Test 2: Lost book by repeat offender
console.log(calculateLibraryFine(
    {
        id: 's2',
        category: 'PG',
        previousOffenses: 4,
        medicalLeave: null
    },
    [{
        bookId: 'b2',
        category: 'reserved',
        issueDate: [0,1,1,0,2,0,2,4],
        dueDate: [0,3,1,0,2,0,2,4],
        returnDate: null,
        status: 'lost',
        cost: 1200,
        damageEstimate: 0
    }],
    [1,5,1,1,2,0,2,4], // 45 days late
    {
        holidays: [],
        examWeek: null,
        festivals: []
    }
));
/* Expected: {
    totalFine: 2675, // 1200 + 500 + (42 days * ₹5) + 25% penalty
    breakdown: [{
        bookId: 'b2',
        overdueDays: 42,
        baseFine: 210,
        adjustments: [
            {type: 'lost-book', charge: 1200},
            {type: 'processing', charge: 500},
            {type: 'late-payment', penalty: 477.5}
        ],
        finalFine: 2387.5
    }],
    penalties: {latePayment: 477.5},
    canIssueNew: false,
    warnings: ['Repeat offender - no new issues until paid']
} */

// Test 3: Exam week extension + medical leave
console.log(calculateLibraryFine(
    {
        id: 's3',
        category: 'UG',
        previousOffenses: 1,
        medicalLeave: {
            start: [1,8,1,1,2,0,2,4],
            end: [2,5,1,1,2,0,2,4]
        }
    },
    [{
        bookId: 'b3',
        category: 'regular',
        issueDate: [0,1,1,1,2,0,2,4],
        dueDate: [1,5,1,1,2,0,2,4],
        returnDate: [2,6,1,1,2,0,2,4],
        status: 'returned',
        cost: 800,
        damageEstimate: 0
    }],
    [2,6,1,1,2,0,2,4],
    {
        holidays: [],
        examWeek: {
            start: [1,6,1,1,2,0,2,4],
            end: [2,3,1,1,2,0,2,4]
        },
        festivals: []
    }
));
/* Expected: {
    totalFine: 0, // Exam week +7 days, medical leave covers rest
    breakdown: [{
        bookId: 'b3',
        overdueDays: 0,
        baseFine: 0,
        adjustments: [
            {type: 'exam-extension', days: 7},
            {type: 'medical-waiver', days: 4}
        ],
        finalFine: 0
    }],
    canIssueNew: true,
    warnings: []
} */
```

**Hints:**
- Count overdue days excluding weekends/holidays
- Check if dates overlap with grace periods
- Apply faculty exemption first
- Calculate penalties based on payment delay
- Handle lost/damaged separately

**JS Methods:**
- Date arithmetic with exclusions
- `filter()` for calendar checks
- Conditional discount application
- Object building for breakdown
- Array `reduce()` for totals

---

### Problem #18: 🎫 **Movie Theater Dynamic Seat Pricing**

**📋 Complex Theater Revenue Optimization:**

Multiplex chain. The revenue manager explains:

> *"Seat pricing is algorithmic:*
> 
> **Base Prices (by seat type):**
> - *Recliner: ₹500*
> - *Premium: ₹300*
> - *Normal: ₹200*
> 
> **Show Time Multipliers:**
> - *Morning (before 12 PM): 0.7x*
> - *Matinee (12-3 PM): 0.8x*
> - *Evening (3-6 PM): 1.0x*
> - *Prime (6-9 PM): 1.5x*
> - *Night (after 9 PM): 1.2x*
> 
> **Dynamic Factors:**
> - *Occupancy: <30% = 0.8x, 30-60% = 1.0x, 60-80% = 1.3x, >80% = 1.8x*
> - *Movie rating: <6.0 = 0.9x, 6-7.5 = 1.0x, 7.5-9 = 1.2x, >9 = 1.5x*
> - *Release: First week = 1.3x, Second week = 1.1x, Third+ = 1.0x*
> - *Day: Weekend = 1.4x, Friday = 1.2x, Holiday = 1.5x*
> 
> **Position Premium:**
> - *Center seats (middle 40%): +₹50*
> - *Aisle seats: +₹20*
> - *First/last row: -₹30*
> - *Corner seats: -₹40*
> 
> **Special Offers:**
> - *Couple seats (selected pairs): +₹100 but includes ₹100 F&B voucher*
> - *Student discount: 20% off on weekdays before 6 PM (ID required)*
> - *Senior citizen: 30% off always*
> - *Credit card offers: Varies by card*
> 
> **Rules:**
> - *All multipliers compound*
> - *Position premium added after multipliers*
> - *Only one personal discount (student/senior)*
> - *Minimum price: ₹99, Maximum: ₹1500*
> - *Once 90% full, enable waitlist at +₹100*
> 
> *Calculate price for seat selection."*

---

**Problem Statement:**
```javascript
/**
 * Calculate movie ticket price
 * @param {Object} showDetails - {
 *   startTime: [D,D,M,M,Y,Y,Y,Y,H,H,M,M],
 *   totalSeats: number,
 *   occupiedSeats: number,
 *   movie: {
 *     name: string,
 *     rating: number,
 *     releaseDate: [D,D,M,M,Y,Y,Y,Y]
 *   }
 * }
 * @param {Object} seatDetails - {
 *   type: 'recliner'|'premium'|'normal',
 *   position: {row: number, col: number},
 *   isCouple: boolean,
 *   isAisle: boolean,
 *   totalRows: number,
 *   totalCols: number
 * }
 * @param {Object} userProfile - {
 *   type: 'student'|'senior'|'regular',
 *   cardOffer: {bank: string, discount: number} | null
 * }
 * @param {number[]} holidays
 * @returns {Object} {
 *   basePrice: number,
 *   multiplier: number,
 *   multiplierBreakdown: {time, occupancy, rating, release, day},
 *   positionAdjustment: number,
 *   priceBeforeDiscount: number,
 *   discount: {type, amount},
 *   finalPrice: number,
 *   fbVoucher: number,
 *   breakdown: string[]
 * }
 */
function calculateTicketPrice(showDetails, seatDetails, userProfile, holidays) {
    // Your code here
}
```

**Test Cases:**
```javascript
// Test 1: Prime time weekend, high occupancy, blockbuster
console.log(calculateTicketPrice(
    {
        startTime: [1,4,1,2,2,0,2,4,1,9,0,0], // Saturday 7 PM
        totalSeats: 200,
        occupiedSeats: 170, // 85% occupied
        movie: {
            name: 'Blockbuster',
            rating: 8.5,
            releaseDate: [0,8,1,2,2,0,2,4] // Released 6 days ago
        }
    },
    {
        type: 'premium',
        position: {row: 10, col: 15}, // Center row, center column
        isCouple: false,
        isAisle: false,
        totalRows: 20,
        totalCols: 30
    },
    {
        type: 'regular',
        cardOffer: null
    },
    []
));
/* Expected: {
    basePrice: 300,
    multiplier: 5.616, // 1.5(prime) * 1.8(occupancy) * 1.2(rating) * 1.3(first week) * 1.4(weekend)
    multiplierBreakdown: {
        time: 1.5,
        occupancy: 1.8,
        rating: 1.2,
        release: 1.3,
        day: 1.4
    },
    positionAdjustment: 50, // Center seat
    priceBeforeDiscount: 1500, // Capped at max
    discount: {type: 'none', amount: 0},
    finalPrice: 1500,
    fbVoucher: 0,
    breakdown: [...]
} */

// Test 2: Student morning show weekday
console.log(calculateTicketPrice(
    {
        startTime: [1,3,1,2,2,0,2,4,1,0,3,0], // Monday 10:30 AM
        totalSeats: 150,
        occupiedSeats: 20, // 13% occupied
        movie: {
            name: 'Old Movie',
            rating: 6.2,
            releaseDate: [0,1,1,0,2,0,2,4] // Released 6 weeks ago
        }
    },
    {
        type: 'normal',
        position: {row: 1, col: 5}, // First row
        isCouple: false,
        isAisle: true,
        totalRows: 15,
        totalCols: 20
    },
    {
        type: 'student',
        cardOffer: {bank: 'HDFC', discount: 15}
    },
    []
));
/* Expected: {
    basePrice: 200,
    multiplier: 0.56, // 0.7(morning) * 0.8(low occupancy) * 1.0(rating) * 1.0(old movie) * 1.0(weekday)
    positionAdjustment: -10, // -30(first row) + 20(aisle)
    priceBeforeDiscount: 102,
    discount: {type: 'student', amount: 20.4}, // 20% better than card 15%
    finalPrice: 99, // Enforced minimum
    breakdown: [...]
} */

// Test 3: Couple seat senior citizen on holiday
console.log(calculateTicketPrice(
    {
        startTime: [2,6,1,2,2,0,2,4,2,0,0,0], // Dec 26 (holiday) 8 PM
        totalSeats: 180,
        occupiedSeats: 125, // 69%
        movie: {
            name: 'Family Drama',
            rating: 7.8,
            releaseDate: [1,9,1,2,2,0,2,4] // Second week
        }
    },
    {
        type: 'recliner',
        position: {row: 12, col: 16}, // Center
        isCouple: true,
        isAisle: false,
        totalRows: 18,
        totalCols: 28
    },
    {
        type: 'senior',
        cardOffer: null
    },
    [[2,5,1,2,2,0,2,4], [2,6,1,2,2,0,2,4]]
));
/* Expected: {
    basePrice: 500,
    multiplier: 3.432, // 1.2(night) * 1.3(occupancy) * 1.2(rating) * 1.1(week 2) * 1.5(holiday)
    positionAdjustment: 150, // +50(center) + 100(couple)
    priceBeforeDiscount: 1866,
    discount: {type: 'senior', amount: 559.8}, // 30% off
    finalPrice: 1306.2,
    fbVoucher: 100,
    breakdown: [...]
} */
```

**Hints:**
- Calculate time slot from HH:MM
- Determine day of week and holiday status
- Check seat position (center = middle 40% of rows/cols)
- Multiply all factors, then add position premium
- Compare discounts, apply best one
- Enforce min/max price caps

**JS Methods:**
- Date `getDay()`, `getHours()`
- Percentage calculations in middle ranges
- `Math.min()`, `Math.max()` for caps
- Object building for breakdown
- Conditional chaining

---

### Problem #19: 🏃 **Marathon Registration Fee Calculator**

**📋 Ultra-Complex Event Registration:**

Marathon organizer. The event manager explains:

> *"Registration pricing is tiered and time-sensitive:*
> 
> **Race Categories:**
> - *5K Fun Run: Base ₹500*
> - *10K: Base ₹1000*
> - *21K Half Marathon: Base ₹2000*
> - *42K Full Marathon: Base ₹3500*
> 
> **Early Bird Tiers:**
> - *Super Early (6+ months): 40% off*
> - *Early (3-6 months): 25% off*
> - *Regular (1-3 months): 10% off*
> - *Late (<1 month): No discount*
> - *Last minute (<1 week): +₹500 surcharge*
> 
> **Age Categories:**
> - *Kids (under 15): 50% off (5K only)*
> - *Youth (15-25): 20% off*
> - *Senior (60+): 30% off*
> - *Differently-abled: 40% off all categories*
> 
> **Group Registration:**
> - *5-9 people: 10% off per person*
> - *10-19: 15% off*
> - *20+: 20% off*
> - *Corporate (50+): 25% off + branded bib*
> 
> **Add-ons:**
> - *Timing chip: ₹300 (mandatory for 21K/42K)*
> - *T-shirt: ₹400 (optional)*
> - *Medal: ₹200 (optional, free for 21K+)*
> - *Refreshment pack: ₹150*
> - *Photo service: ₹500*
> 
> **Previous Participation:**
> - *Loyalty: Ran last year = 15% off*
> - *Veteran: Ran 3+ times = 25% off*
> - *Streak: Ran last 5 years = Free entry (pay add-ons only)*
> 
> **Special:**
> - *Charity runner: Pay minimum ₹5000, goes to charity*
> - *Virtual run option: 60% of regular price*
> - *International runner: +₹500 processing*
> 
> **Rules:**
> - *Best age discount applies*
> - *Group discount doesn't stack with early bird*
> - *Loyalty stacks with age*
> - *Add-ons at full price always*
> - *Refunds: >30 days = 50%, <30 days = no refund*
> 
> *Calculate registration fee with full breakdown."*

---

**Problem Statement:**
```javascript
/**
 * Calculate marathon registration fee
 * @param {Object} runnerProfile - {
 *   age: number,
 *   category: '5K'|'10K'|'21K'|'42K',
 *   isVirtual: boolean,
 *   isDifferentlyAbled: boolean,
 *   isInternational: boolean,
 *   previousRuns: number[], // Years they participated
 *   isCharityRunner: boolean
 * }
 * @param {Object} registrationDetails - {
 *   registrationDate: [D,D,M,M,Y,Y,Y,Y],
 *   eventDate: [D,D,M,M,Y,Y,Y,Y],
 *   groupSize: number,
 *   addOns: {
 *     tShirt: boolean,
 *     refreshments: boolean,
 *     photo: boolean
 *   }
 * }
 * @param {number} currentYear
 * @returns {Object} {
 *   basePrice: number,
 *   earlyBirdDiscount: {tier: string, percentage: number, amount: number},
 *   ageDiscount: {type: string, amount: number},
 *   groupDiscount: {amount: number},
 *   loyaltyDiscount: {type: string, amount: number},
 *   mandatoryAddOns: {chip: number, medal: number},
 *   optionalAddOns: number,
 *   surcharges: {late: number, international: number, virtual: number},
 *   totalBeforeCharity: number,
 *   finalAmount: number,
 *   charityPortion: number,
 *   breakdown: string[],
 *   refundPolicy: string
 * }
 */
function calculateMarathonFee(runnerProfile, registrationDetails, currentYear) {
    // Your code here
}
```

**Test Cases:**
```javascript
// Test 1: Super early bird, group registration, youth
console.log(calculateMarathonFee(
    {
        age: 22,
        category: '10K',
        isVirtual: false,
        isDifferentlyAbled: false,
        isInternational: false,
        previousRuns: [2023],
        isCharityRunner: false
    },
    {
        registrationDate: [0,1,0,6,2,0,2,4], // June 1
        eventDate: [1,5,1,2,2,0,2,4], // Dec 15 (6.5 months)
        groupSize: 12,
        addOns: {
            tShirt: true,
            refreshments: true,
            photo: false
        }
    },
    2024
));
/* Expected: {
    basePrice: 1000,
    earlyBirdDiscount: {tier: 'super-early', percentage: 40, amount: 0}, // Overridden by group
    ageDiscount: {type: 'youth', amount: 0}, // Doesn't stack
    groupDiscount: {amount: 150}, // 15% off for 10-19 people
    loyaltyDiscount: {type: 'single', amount: 127.5}, // 15% of discounted base
    mandatoryAddOns: {chip: 0, medal: 0},
    optionalAddOns: 550, // T-shirt + refreshments
    surcharges: {late: 0, international: 0, virtual: 0},
    totalBeforeCharity: 1272.5,
    finalAmount: 1272.5,
    charityPortion: 0,
    breakdown: [...],
    refundPolicy: '50% refund available until Nov 15'
} */

// Test 2: Veteran runner, 42K, late registration
console.log(calculateMarathonFee(
    {
        age: 45,
        category: '42K',
        isVirtual: false,
        isDifferentlyAbled: false,
        isInternational: false,
        previousRuns: [2020, 2021, 2022, 2023],
        isCharityRunner: false
    },
    {
        registrationDate: [1,0,1,2,2,0,2,4], // Dec 10
        eventDate: [1,5,1,2,2,0,2,4], // Dec 15 (5 days)
        groupSize: 1,
        addOns: {
            tShirt: true,
            refreshments: true,
            photo: true
        }
    },
    2024
));
/* Expected: {
    basePrice: 3500,
    earlyBirdDiscount: {tier: 'none', percentage: 0, amount: 0},
    ageDiscount: {type: 'none', amount: 0},
    groupDiscount: {amount: 0},
    loyaltyDiscount: {type: 'veteran', amount: 875}, // 25% off
    mandatoryAddOns: {chip: 300, medal: 0}, // Medal free for 42K
    optionalAddOns: 1050, // All add-ons
    surcharges: {late: 500, international: 0, virtual: 0},
    totalBeforeCharity: 4475,
    finalAmount: 4475,
    charityPortion: 0,
    breakdown: [...],
    refundPolicy: 'No refund available'
} */

// Test 3: 5-year streak runner (free entry)
console.log(calculateMarathonFee(
    {
        age: 35,
        category: '21K',
        isVirtual: false,
        isDifferentlyAbled: false,
        isInternational: true,
        previousRuns: [2019, 2020, 2021, 2022, 2023],
        isCharityRunner: false
    },
    {
        registrationDate: [0,1,0,9,2,0,2,4],
        eventDate: [1,5,1,2,2,0,2,4],
        groupSize: 1,
        addOns: {
            tShirt: false,
            refreshments: true,
            photo: true
        }
    },
    2024
));
/* Expected: {
    basePrice: 2000,
    loyaltyDiscount: {type: 'streak', amount: 2000}, // Free entry!
    mandatoryAddOns: {chip: 300, medal: 0},
    optionalAddOns: 650,
    surcharges: {late: 0, international: 500, virtual: 0},
    finalAmount: 1450, // Only add-ons + international fee
    breakdown: ['5-year streak - Free entry', ...]
} */
```

**Hints:**
- Calculate months between registration and event date
- Determine best discount combination (group vs early bird)
- Check streak: last N consecutive years
- Add mandatory add-ons based on category
- Handle charity minimum separately

**JS Methods:**
- Date difference in months
- `filter()` for checking consecutive years
- Conditional discount stacking logic
- Object building for detailed breakdown
- `Math.max()` for charity minimum

---

### Problem #20: 🏠 **Property Tax Calculator - Municipal**

**📋 Extremely Complex Government Tax System:**

Municipal corporation. The tax officer explains:

> *"Property tax calculation involves multiple factors:*
> 
> **Property Valuation:**
> - *Carpet area × Location factor × Age factor × Floor factor*
> - *Location: Prime = 5x, Sub-urban = 3x, Rural = 1x*
> - *Age: <5 years = 1.2x, 5-10 = 1.0x, 10-20 = 0.9x, >20 = 0.8x*
> - *Floor: Ground = 1.0x, 1-3 = 1.1x, 4-7 = 1.2x, 8+ = 1.3x*
> 
> **Base Tax Rate:**
> - *Residential: 0.8% of valuation*
> - *Commercial: 1.5% of valuation*
> - *Industrial: 2.0% of valuation*
> - *Mixed use: Weighted average*
> 
> **Cess & Surcharges:**
> - *Water: ₹500/quarter*
> - *Sewerage: 10% of base tax*
> - *Street light: ₹200/quarter*
> - *Education cess: 2% of base tax*
> - *Health cess: 1% of base tax*
> 
> **Rebates:**
> - *Early payment (before April 30): 5%*
> - *Senior citizen owner: 30%*
> - *Women ownership: 10%*
> - *Ex-serviceman: 20%*
> - *Differently-abled: 25%*
> 
> **Penalties:**
> - *Late payment: 1% per month*
> - *No payment last year: +50% surcharge*
> - *Illegal construction: 3x regular tax*
> - *Commercial in residential zone: +100%*
> 
> **Special Cases:**
> - *Under construction: 50% exemption*
> - *Heritage property: 70% exemption*
> - *Rain water harvesting: 10% rebate*
> - *Solar panels: 15% rebate*
> - *Waste segregation: 5% rebate*
> 
> **Payment:**
> - *Quarterly: Full amount/4*
> - *Bi-annual: 2% discount*
> - *Annual: 5% discount*
> - *Arrears: Compound at 12% annually*
> 
> *Calculate annual tax with all factors."*

---

**Problem Statement:**
```javascript
/**
 * Calculate municipal property tax
 * @param {Object} propertyDetails - {
 *   carpetArea: number, // sq ft
 *   location: 'prime'|'suburban'|'rural',
 *   age: number, // years
 *   floor: number,
 *   usage: {residential: number, commercial: number, industrial: number}, // percentages
 *   features: {rainwater: boolean, solar: boolean, wasteSegregation: boolean},
 *   status: 'complete'|'under-construction'|'heritage',
 *   zoning: {actual: string, permitted: string}
 * }
 * @param {Object} ownerDetails - {
 *   isSenior: boolean,
 *   isWomen: boolean,
 *   isExServiceman: boolean,
 *   isDifferentlyAbled: boolean
 * }
 * @param {Object} paymentDetails - {
 *   paymentDate: [D,D,M,M,Y,Y,Y,Y],
 *   frequency: 'quarterly'|'bi-annual'|'annual',
 *   arrears: number,
 *   lastYearPaid: boolean
 * }
 * @param {number} financialYear
 * @returns {Object} {
 *   valuation: number,
 *   baseTax: number,
 *   cess: {water, sewerage, streetLight, education, health},
 *   rebates: [{type, amount}],
 *   penalties: [{type, amount}],
 *   greenRebates: number,
 *   arrearsWithInterest: number,
 *   grossTax: number,
 *   netTax: number,
 *   paymentSchedule: [{quarter, amount, dueDate}],
 *   breakdown: string[]
 * }
 */
function calculatePropertyTax(propertyDetails, ownerDetails, paymentDetails, financialYear) {
    // Your code here
}
```

**Test Cases:**
```javascript
// Test 1: Residential, prime location, senior citizen, early payment
console.log(calculatePropertyTax(
    {
        carpetArea: 1500,
        location: 'prime',
        age: 8,
        floor: 5,
        usage: {residential: 100, commercial: 0, industrial: 0},
        features: {rainwater: true, solar: true, wasteSegregation: true},
        status: 'complete',
        zoning: {actual: 'residential', permitted: 'residential'}
    },
    {
        isSenior: true,
        isWomen: false,
        isExServiceman: false,
        isDifferentlyAbled: false
    },
    {
        paymentDate: [1,5,0,4,2,0,2,4], // April 15
        frequency: 'annual',
        arrears: 0,
        lastYearPaid: true
    },
    2024
));
/* Expected: {
    valuation: 9000000, // 1500 * 5(prime) * 1.0(age) * 1.2(floor) * 1000
    baseTax: 72000, // 0.8% of valuation
    cess: {
        water: 2000,
        sewerage: 7200,
        streetLight: 800,
        education: 1440,
        health: 720
    },
    rebates: [
        {type: 'senior-citizen', amount: 21600}, // 30%
        {type: 'early-payment', amount: 3960}, // 5% after senior
        {type: 'annual-payment', amount: 3920} // 5% additional
    ],
    greenRebates: 21600, // 30% total (10+15+5)
    penalties: [],
    arrearsWithInterest: 0,
    grossTax: 84160,
    netTax: 33080,
    paymentSchedule: [{quarter: 'FY2024-25', amount: 33080, dueDate: 'April 30, 2024'}],
    breakdown: [...]
} */

// Test 2: Mixed use, commercial in residential zone, late payment, arrears
console.log(calculatePropertyTax(
    {
        carpetArea: 2000,
        location: 'suburban',
        age: 3,
        floor: 2,
        usage: {residential: 40, commercial: 60, industrial: 0},
        features: {rainwater: false, solar: false, wasteSegregation: false},
        status: 'complete',
        zoning: {actual: 'commercial', permitted: 'residential'}
    },
    {
        isSenior: false,
        isWomen: true,
        isExServiceman: false,
        isDifferentlyAbled: false
    },
    {
        paymentDate: [1,5,0,8,2,0,2,4], // August 15 (4 months late)
        frequency: 'quarterly',
        arrears: 50000,
        lastYearPaid: false
    },
    2024
));
/* Expected: Complex calculation with:
    - Valuation: 2000 * 3 * 1.2 * 1.1 * 1000 = 7,920,000
    - Weighted tax: (0.8% * 40%) + (1.5% * 60%) = 1.22%
    - Base tax: 96,624
    - Zone violation: +100% = additional 96,624
    - Last year non-payment: +50% surcharge
    - Late payment: 4% (4 months * 1%)
    - Arrears with 12% annual interest: 56,000
    - Women rebate: 10%
    - Net tax: Very high due to penalties
} */

// Test 3: Heritage property under construction
console.log(calculatePropertyTax(
    {
        carpetArea: 3000,
        location: 'prime',
        age: 150,
        floor: 0,
        usage: {residential: 100, commercial: 0, industrial: 0},
        features: {rainwater: true, solar: false, wasteSegregation: true},
        status: 'heritage',
        zoning: {actual: 'residential', permitted: 'residential'}
    },
    {
        isSenior: false,
        isWomen: false,
        isExServiceman: true,
        isDifferentlyAbled: false
    },
    {
        paymentDate: [1,0,0,5,2,0,2,4],
        frequency: 'bi-annual',
        arrears: 0,
        lastYearPaid: true
    },
    2024
));
/* Expected: {
    valuation: 12000000, // 3000 * 5 * 0.8 * 1.0 * 1000
    baseTax: 96000,
    rebates: [
        {type: 'heritage', amount: 67200}, // 70% exemption
        {type: 'ex-serviceman', amount: 5760}, // 20% of remaining
        {type: 'bi-annual', amount: 465} // 2%
    ],
    greenRebates: 3270, // 15% (10+5, no solar)
    netTax: 19305,
    paymentSchedule: [
        {quarter: 'H1 FY2024-25', amount: 9652.5, dueDate: 'Sept 30'},
        {quarter: 'H2 FY2024-25', amount: 9652.5, dueDate: 'Mar 31'}
    ]
} */
```

**Hints:**
- Calculate valuation step-by-step with all multipliers
- For mixed use, calculate weighted average tax rate
- Apply exemptions before rebates
- Compound arrears interest annually
- Check payment date against Apr 30 deadline
- Sum all cess separately

**JS Methods:**
- Complex math operations
- Weighted averages
- Compound interest calculation
- Date comparison for deadlines
- Object building for schedules
- `reduce()` for summing components

---

## 🎯 Critical Thinking Framework

### How to Approach These Complex Problems:

**Step 1: Information Extraction (5 minutes)**
- Read the entire scenario once
- Highlight key numbers and rules
- Note all edge cases mentioned
- List all calculation dependencies

**Step 2: Problem Decomposition (5 minutes)**
- Break into sub-problems
- Identify calculation order (what depends on what?)
- Map business rules to code logic
- Plan helper functions

**Step 3: Data Structure Design (3 minutes)**
- Decide input parsing strategy
- Plan intermediate data storage
- Design output object structure
- Consider lookup tables for constants

**Step 4: Algorithm Design (5 minutes)**
- Write pseudocode for main flow
- Plan error handling
- Consider optimization opportunities
- Think about testability

**Step 5: Implementation (20 minutes)**
- Code incrementally
- Test each component
- Handle edge cases
- Refactor for clarity

**Step 6: Validation (5 minutes)**
- Run all test cases
- Check boundary conditions
- Verify business logic
- Review for bugs

---

## 📚 How to Read Complex Problem Statements

### Technique 1: **Color Coding (Mental)**
- **Rules** (blue): Hard constraints that must be followed
- **Calculations** (green): Mathematical operations
- **Exceptions** (red): Special cases and edge cases
- **Examples** (yellow): Use cases to understand context

### Technique 2: **Progressive Summarization**
1. **First Read**: Get the general idea
2. **Second Read**: Extract all numbers and formulas
3. **Third Read**: Note all conditions and rules
4. **Fourth Read**: Identify dependencies

### Technique 3: **Question Mapping**
Ask yourself:
- What is the main output?
- What are the required inputs?
- What intermediate calculations are needed?
- What are the edge cases?
- What rules can conflict?

### Technique 4: **Rule Hierarchy**
Organize rules by priority:
1. **Hard constraints**: Must always apply
2. **Soft constraints**: Apply when possible
3. **Defaults**: Fallback values
4. **Optimizations**: Nice to have

---

## 🚀 Practice Strategy

**Week 1-2:** Problems #11-13
- Focus on multi-factor calculations
- Practice breaking down complex scenarios
- Master date/time manipulation

**Week 3-4:** Problems #14-16
- Advanced conditional logic
- State management across operations
- Priority-based decision making

**Week 5-6:** Problems #17-19
- Financial calculations
- Discount/penalty stacking
- Complex eligibility criteria

**Week 7:** Problem #20 + Review
- Government/regulatory logic
- Comprehensive tax calculations
- Full integration of all concepts

---

## 💡 Key Learnings from These Problems

### Mathematical Concepts:
- Weighted averages
- Compound multipliers
- Percentage cascading
- Range-based calculations
- Time-based calculations

### Logic Patterns:
- Multi-tier conditionals
- Priority queues
- Constraint satisfaction
- Rule conflict resolution
- State machines

### Data Handling:
- Date arithmetic
- Array transformations
- Object manipulation
- Lookup tables
- Nested structures

### JS Methods Mastery:
- `filter()`, `map()`, `reduce()` - Data transformation
- `some()`, `every()` - Validation
- `Date()` methods - Time calculations
- `Math` methods - Numerical operations
- Object/Array destructuring - Clean code

---

## 🎯 Interview Success Tips

### During Problem Reading:
1. ✅ Take notes while reading
2. ✅ Ask clarifying questions
3. ✅ Repeat back your understanding
4. ✅ Confirm edge cases
5. ✅ Request examples if unclear

### During Coding:
1. ✅ Think aloud - explain your approach
2. ✅ Start with simple case
3. ✅ Build incrementally
4. ✅ Test as you go
5. ✅ Handle errors gracefully

### Common Mistakes to Avoid:
1. ❌ Rushing without understanding
2. ❌ Not asking about edge cases
3. ❌ Hardcoding values
4. ❌ Ignoring rule priorities
5. ❌ Not testing boundary conditions

---

**Master These, Crack Any Interview! 💪**

*The difference between junior and senior developers is how they handle complexity. These problems teach you to think like a senior!*📋 Complex Business Scenario:**
You're interviewing at a hotel booking platform. The revenue manager presents a detailed scenario:

"Our pricing algorithm is sophisticated. Room rates vary based on multiple factors:

- Base price depends on season: Summer (June-Aug) = $200, Winter (Dec-Feb) = $150, Spring/Fall = $180
- Weekend markup: Friday-Sunday bookings get 30% surcharge
- Advance booking discount: Book 30+ days ahead = 15% off, 60+ days = 25% off
- Peak dates (holidays): New Year (Dec 31-Jan 2), Christmas (Dec 24-26), Independence Day (July 4) = additional $50
- Long stay discount: 7+ nights = 10% off, 14+ nights = 20% off

All discounts and surcharges stack, but apply in this order: base price → season → weekend → peak → advance booking → long stay. Given check-in date, checkout date, and booking date (all as arrays), calculate the total price."

**Critical Thinking Required:**

- How do discounts combine? Multiplicative or additive?
- What if checkout is on a weekday but checkin was on weekend?
- How to handle multi-week stays crossing seasons?


**Problem Statement:**
```javascript
/**
 * Calculate hotel booking total price
 * @param {number[]} checkinDate - [D,D,M,M,Y,Y,Y,Y]
 * @param {number[]} checkoutDate - [D,D,M,M,Y,Y,Y,Y]
 * @param {number[]} bookingDate - [D,D,M,M,Y,Y,Y,Y]
 * @returns {Object} {totalPrice: number, breakdown: {base, season, weekend, peak, advance, longStay}}
 */
function calculateHotelPrice(checkinDate, checkoutDate, bookingDate) {
    // Your code here
    // Must return breakdown of how price was calculated
}
```

**Test Cases:**
```javascript
// Test 1: Simple summer weekend, no discounts
console.log(calculateHotelPrice(
    [1,5,0,7,2,0,2,4], // Checkin: July 15, 2024 (Monday)
    [1,7,0,7,2,0,2,4], // Checkout: July 17, 2024 (Wednesday)
    [1,0,0,7,2,0,2,4]  // Booked: July 10, 2024
));
/* Expected: {
    totalPrice: 400,
    breakdown: {
        base: 200,
        nights: 2,
        season: 'summer',
        weekendNights: 0,
        peakDates: 0,
        advanceDays: 5,
        advanceDiscount: 0,
        longStayDiscount: 0
    }
} */

// Test 2: New Year peak period with advance booking
console.log(calculateHotelPrice(
    [3,1,1,2,2,0,2,4], // Dec 31, 2024
    [0,2,0,1,2,0,2,5], // Jan 2, 2025
    [0,1,1,1,2,0,2,4]  // Booked Nov 1, 2024 (60 days ahead)
));
/* Expected: Complex calculation with:
    - 2 nights
    - Winter season base ($150)
    - Weekend nights (Tue-Wed-Thu, no weekend)
    - Peak dates: 2 nights * $50 = $100 extra
    - 60 days advance = 25% off
    - Total: [($150 * 2) + $100] * 0.75 = ~$300
} */

// Test 3: Long summer stay crossing into fall
console.log(calculateHotelPrice(
    [2,5,0,8,2,0,2,4], // Aug 25, 2024
    [0,5,0,9,2,0,2,4], // Sep 5, 2024 (11 nights)
    [1,0,0,8,2,0,2,4]  // Booked Aug 10 (15 days ahead)
));
/* Expected: 
    - 11 nights (6 in summer, 5 in fall)
    - Need to calculate each night's rate
    - 2 weekends included
    - 15 days advance = 15% off
    - 11 nights = 10% long stay discount
    - Discounts multiply: 0.85 * 0.90 = 0.765
} */
```

**Hints:**

- Loop through each night individually
- Determine day of week for each date
- Track which season each night falls in
- Apply discounts as percentages that multiply
- Use helper functions: getDayOfWeek(), getSeason(), isPeakDate()

**JS Methods to Master:**

- Date(), getDay(), getMonth()
- Loops with date iteration
- Object building for breakdown
- Percentage calculations
- Array methods for counting nights

---

### Problem #12: 📦 E-Commerce Flash Sale Eligibility Engine
**📋 Complex Business Scenario:**
Major e-commerce platform. The product manager explains a multi-layered flash sale system:

"Our flash sales are complex. Here's the eligibility criteria:

**User Eligibility:**

- Account must be at least 30 days old (from registration date)
- User must have made at least 3 purchases in last 90 days
- No returns/refunds in last 60 days
- Prime members get early access (6 hours before sale)

**Sale Timing:**

- Regular sale: 12 PM to 6 PM on sale date
- Prime early access: 6 AM to 12 PM
- After 6 PM, sale continues but only for items in cart (30 min checkout window)

**Purchase Limits:**

- New users (< 90 days): max 1 item per sale
- Regular users: max 3 items
- Prime members: max 5 items
- VIP (Prime + 50+ lifetime orders): max 10 items

Given user data and current timestamp, determine: Can they access the sale now? What's their purchase limit? When does their access expire?"

**Critical Thinking Challenge:**

- Multiple time windows to check
- Nested conditions (VIP requires Prime AND order count)
- Edge case: What if user's Prime expires during the sale?


**Problem Statement:**
```javascript
/**
 * Determine flash sale access and limits
 * @param {Object} userData - {
 *   registrationDate: [D,D,M,M,Y,Y,Y,Y],
 *   isPrime: boolean,
 *   primeExpiry: [D,D,M,M,Y,Y,Y,Y],
 *   purchaseHistory: [[date, returned], ...],
 *   lifetimeOrders: number
 * }
 * @param {number[]} saleDate - [D,D,M,M,Y,Y,Y,Y]
 * @param {number[]} currentDateTime - [D,D,M,M,Y,Y,Y,Y,H,H,M,M]
 * @returns {Object} {
 *   canAccess: boolean,
 *   accessType: 'prime-early' | 'regular' | 'cart-only' | 'denied',
 *   purchaseLimit: number,
 *   accessExpiresAt: string,
 *   denialReasons: string[]
 * }
 */
function checkFlashSaleAccess(userData, saleDate, currentDateTime) {
    // Your code here
}
```

**Test Cases:**
```javascript
// Test 1: Prime VIP user during early access
console.log(checkFlashSaleAccess(
    {
        registrationDate: [0,1,0,1,2,0,2,0],
        isPrime: true,
        primeExpiry: [3,1,1,2,2,0,2,5],
        purchaseHistory: [
            [[0,1,1,1,2,0,2,4], false],
            [[1,5,1,1,2,0,2,4], false],
            [[2,0,1,1,2,0,2,4], false],
            [[0,5,1,2,2,0,2,4], false]
        ],
        lifetimeOrders: 75
    },
    [1,5,1,2,2,0,2,4], // Sale date: Dec 15, 2024
    [1,5,1,2,2,0,2,4,0,8,3,0] // Current: Dec 15, 8:30 AM
));
/* Expected: {
    canAccess: true,
    accessType: 'prime-early',
    purchaseLimit: 10,
    accessExpiresAt: '12:00 PM',
    denialReasons: []
} */

// Test 2: New user trying to access during regular hours
console.log(checkFlashSaleAccess(
    {
        registrationDate: [2,0,1,1,2,0,2,4], // Registered Nov 20
        isPrime: false,
        primeExpiry: null,
        purchaseHistory: [
            [[2,5,1,1,2,0,2,4], false],
            [[0,1,1,2,2,0,2,4], false]
        ],
        lifetimeOrders: 2
    },
    [1,5,1,2,2,0,2,4],
    [1,5,1,2,2,0,2,4,1,4,0,0] // Dec 15, 2:00 PM
));
/* Expected: {
    canAccess: true,
    accessType: 'regular',
    purchaseLimit: 1,
    accessExpiresAt: '6:00 PM',
    denialReasons: []
} */

// Test 3: User with recent return trying to access
console.log(checkFlashSaleAccess(
    {
        registrationDate: [0,1,0,5,2,0,2,4],
        isPrime: true,
        primeExpiry: [3,1,0,3,2,0,2,5],
        purchaseHistory: [
            [[0,1,1,0,2,0,2,4], false],
            [[1,5,1,0,2,0,2,4], true], // Returned!
            [[0,1,1,1,2,0,2,4], false]
        ],
        lifetimeOrders: 25
    },
    [1,5,1,2,2,0,2,4],
    [1,5,1,2,2,0,2,4,1,0,0,0] // Dec 15, 10:00 AM
));
/* Expected: {
    canAccess: false,
    accessType: 'denied',
    purchaseLimit: 0,
    accessExpiresAt: null,
    denialReasons: ['Recent return within 60 days']
} */
```

**Hints:**

- Create helper function to check days between dates
- Check conditions in order: account age → returns → timing → limits
- Parse time from extended array (last 4 digits are HHMM)
- Use switch/case for access type determination
- Build denial reasons array as you validate

**JS Methods:**

- Date(), date arithmetic
- Array filter(), some(), every()
- String formatting for time display
- Nested conditionals
- Object destructuring

---

### Problem #13: 🎮 Gaming Tournament Bracket Generator
**📋 Complex Scenario:**
Esports platform. The tournament coordinator explains:

"We run tournaments with complex seeding rules:

**Player Eligibility & Seeding:**

- Players register with rank (Bronze/Silver/Gold/Platinum/Diamond)
- Registration must close 48 hours before tournament date
- Players need minimum 10 ranked games in last 30 days
- Seeding based on: 60% win rate + 40% recent performance (last 10 games)

**Bracket Rules:**

- Tournament only runs if 8, 16, 32, or 64 qualified players
- If not exact power of 2, highest seeds get 'bye' rounds
- No two players from same region in first round (if possible)
- Top 25% seeds cannot face each other before semifinals

**Match Timing:**

- Matches scheduled every 2 hours starting tournament date 10 AM
- Finals always on tournament date + 1 at 6 PM
- Each player must have minimum 4-hour rest between matches

Given array of player objects and tournament date, generate complete bracket with match timings."

**Critical Thinking:**

- How to handle odd numbers of players?
- Region distribution might be impossible - what's the fallback?
- What if a player's rest time conflicts with bracket structure?


**Problem Statement:**
```javascript
/**
 * Generate tournament bracket with schedule
 * @param {Object[]} players - [{
 *   id: string,
 *   name: string,
 *   rank: 'Bronze'|'Silver'|'Gold'|'Platinum'|'Diamond',
 *   region: string,
 *   registrationDate: [D,D,M,M,Y,Y,Y,Y],
 *   recentGames: [{date: [], won: boolean, opponent: string}],
 *   winRate: number (0-100)
 * }]
 * @param {number[]} tournamentDate - [D,D,M,M,Y,Y,Y,Y]
 * @param {number[]} currentDate - [D,D,M,M,Y,Y,Y,Y]
 * @returns {Object} {
 *   qualified: string[],
 *   disqualified: [{id, reason}],
 *   bracket: {round1: [{p1, p2, time}], round2: [...], ...},
 *   warnings: string[]
 * }
 */
function generateTournamentBracket(players, tournamentDate, currentDate) {
    // Your code here
}
```

**Test Cases:**
```javascript
const players = [
    {
        id: 'p1',
        name: 'ProGamer1',
        rank: 'Diamond',
        region: 'NA',
        registrationDate: [0,1,1,2,2,0,2,4],
        recentGames: Array(15).fill(null).map((_, i) => ({
            date: [i+1, 0, 1, 2, 2, 0, 2, 4],
            won: i % 2 === 0,
            opponent: 'random'
        })),
        winRate: 75
    },
    {
        id: 'p2',
        name: 'Player2',
        rank: 'Gold',
        region: 'EU',
        registrationDate: [2,8,1,1,2,0,2,4],
        recentGames: Array(8).fill(null).map((_, i) => ({
            date: [i+1, 0, 1, 2, 2, 0, 2, 4],
            won: true,
            opponent: 'test'
        })),
        winRate: 80
    },
    // ... more players
];

// Test 1: Perfect 8 players, all eligible
console.log(generateTournamentBracket(
    players.slice(0, 8),
    [2,0,1,2,2,0,2,4], // Dec 20, 2024
    [1,5,1,2,2,0,2,4]  // Dec 15, 2024
));
/* Expected: {
    qualified: ['p1', 'p2', ...],
    disqualified: [],
    bracket: {
        round1: [
            {p1: 'p1', p2: 'p8', time: 'Dec 20, 10:00 AM'},
            {p1: 'p2', p2: 'p7', time: 'Dec 20, 10:00 AM'},
            // Different regions paired
        ],
        semifinals: [...],
        finals: [{p1: 'winner1', p2: 'winner2', time: 'Dec 21, 6:00 PM'}]
    },
    warnings: []
} */

// Test 2: 10 players (need byes)
// Expected: Top 6 seeds get byes, bottom 4 play in first round

// Test 3: Player registered too late
// Expected: disqualified with reason 'Late registration'
```

**Hints:**

- Filter eligible players first (games count, registration time)
- Calculate seeding score: (winRate * 0.6) + (recentWinRate * 0.4)
- Use Math.log2(n) to check if power of 2
- Distribute regions using greedy algorithm
- Track match times and enforce 4-hour rest

**JS Methods:**

- filter(), sort(), reduce()
- Math.log2(), Math.ceil(), Math.pow()
- Date arithmetic for scheduling
- Object/array deep manipulation
- Recursive bracket building

---

### Problem #14: 🏥 Hospital Appointment Scheduling System
**📋 Ultra-Complex Healthcare Scenario:**
Hospital management system. The head administrator presents:

"Our appointment system is intricate due to medical regulations:

**Doctor Availability:**

- Each doctor has different schedules: working days, hours, lunch breaks
- Specialists require 60-min slots, GPs require 30-min slots
- Emergency slots reserved: 2 slots per doctor per day
- Doctors can't see more than 12 patients per day (burnout prevention)

**Patient Priority:**

- Emergency: Book same day, override normal limits
- Urgent: Book within 7 days, priority over routine
- Routine: Book 7-60 days in advance
- Follow-up: Must be 2-4 weeks after previous appointment

**Business Rules:**

- No appointments on public holidays
- Seniors (65+) get morning slots priority (better for health)
- Children under 12 must book double slots (parent present)
- Same patient can't book multiple doctors on same day
- Lab test appointments need 3-day gap from consultation

**Special Constraints:**

- Diabetic patients need fasting appointments (before 10 AM)
- Vaccination: walk-in allowed if <5 people waiting
- Teleconsultation: allowed only for follow-ups, not new patients

Given doctor schedules, patient details, and requested date, find next available slot matching all criteria."

**This Tests:**

- Multi-layer conditional logic
- Time slot management
- Priority queuing
- Constraint satisfaction
- Edge case handling


**Problem Statement:**
```javascript
/**
 * Find next available appointment slot
 * @param {Object} patientData - {
 *   age: number,
 *   type: 'emergency'|'urgent'|'routine'|'followup',
 *   condition: string[],
 *   lastAppointment: [D,D,M,M,Y,Y,Y,Y] | null,
 *   preferredDoctor: string | null,
 *   needsLabTest: boolean
 * }
 * @param {Object[]} doctorSchedules - [{
 *   id: string,
 *   name: string,
 *   specialty: 'GP'|'Specialist',
 *   workingDays: number[], // 1=Mon, 7=Sun
 *   hours: {start: 'HH:MM', end: 'HH:MM', lunch: 'HH:MM-HH:MM'},
 *   bookedSlots: [[D,D,M,M,Y,Y,Y,Y,H,H,M,M], ...],
 *   maxPatientsPerDay: number
 * }]
 * @param {number[]} requestDate - [D,D,M,M,Y,Y,Y,Y]
 * @param {number[]} holidays - [[D,D,M,M,Y,Y,Y,Y], ...]
 * @returns {Object} {
 *   slot: [D,D,M,M,Y,Y,Y,Y,H,H,M,M] | null,
 *   doctor: string,
 *   duration: number,
 *   type: 'in-person'|'teleconsult',
 *   warnings: string[],
 *   alternativeSlots: [...] // if preferred slot unavailable
 * }
 */
function findAppointmentSlot(patientData, doctorSchedules, requestDate, holidays) {
    // Your code here
}
```

**Test Cases:**
```javascript
const doctors = [
    {
        id: 'd1',
        name: 'Dr. Smith',
        specialty: 'GP',
        workingDays: [1, 2, 3, 4, 5], // Mon-Fri
        hours: {start: '09:00', end: '17:00', lunch: '13:00-14:00'},
        bookedSlots: [
            [2,0,1,2,2,0,2,4,0,9,0,0],
            [2,0,1,2,2,0,2,4,0,9,3,0]
        ],
        maxPatientsPerDay: 12
    },
    {
        id: 'd2',
        name: 'Dr. Jones',
        specialty: 'Specialist',
        workingDays: [1, 3, 5],
        hours: {start: '10:00', end: '18:00', lunch: '14:00-15:00'},
        bookedSlots: [],
        maxPatientsPerDay: 8
    }
];

const holidays = [
    [2,5,1,2,2,0,2,4] // Dec 25, 2024
];

// Test 1: Senior diabetic patient needs morning fasting slot
console.log(findAppointmentSlot(
    {
        age: 68,
        type: 'routine',
        condition: ['diabetes'],
        lastAppointment: null,
        preferredDoctor: 'd1',
        needsLabTest: false
    },
    doctors,
    [2,0,1,2,2,0,2,4], // Dec 20, 2024
    holidays
));
/* Expected: {
    slot: [2,0,1,2,2,0,2,4,0,9,0,0], // 9:00 AM
    doctor: 'd1',
    duration: 30,
    type: 'in-person',
    warnings: ['Fasting required - no food/drink after midnight'],
    alternativeSlots: []
} */

// Test 2: Child appointment (needs double slot)
console.log(findAppointmentSlot(
    {
        age: 8,
        type: 'urgent',
        condition: [],
        lastAppointment: null,
        preferredDoctor: 'd1',
        needsLabTest: false
    },
    doctors,
    [2,2,1,2,2,0,2,4],
    holidays
));
/* Expected: {
    slot: [2,2,1,2,2,0,2,4,1,0,0,0],
    doctor: 'd1',
    duration: 60, // Double slot for child
    type: 'in-person',
    warnings: ['Parent/guardian must accompany child'],
    alternativeSlots: [...]
} */

// Test 3: Follow-up with lab test requirement
console.log(findAppointmentSlot(
    {
        age: 45,
        type: 'followup',
        condition: [],
        lastAppointment: [0,1,1,2,2,0,2,4], // Dec 1
        preferredDoctor: 'd2',
        needsLabTest: true
    },
    doctors,
    [1,8,1,2,2,0,2,4], // Dec 18
    holidays
));
/* Expected: {
    slot: [2,3,1,2,2,0,2,4,1,0,0,0], // Dec 23 (3 days after lab)
    doctor: 'd2',
    duration: 60,
    type: 'in-person',
    warnings: ['Lab test appointment needed 3 days prior'],
    alternativeSlots: [...]
} */
```

**Hints:**

- Filter doctors by specialty and working days
- Generate time slots accounting for lunch breaks
- Check multiple constraints in priority order
- Use helper function isSlotAvailable(doctor, slot, constraints)
- Return multiple alternatives sorted by suitability

**JS Methods:**

- filter(), map(), reduce(), sort()
- Date manipulation with Date(), setHours()
- String parsing for time ('HH:MM')
- Array some(), every() for constraint checking
- Object deep cloning for alternatives

---

### Problem #15: 💰 Stock Portfolio Tax Calculator
**📋 Extremely Complex Financial Scenario:**
Wealth management firm. The tax specialist explains:

"Tax calculation for stock portfolios is multi-layered:

**Tax Rules (India LTCG/STCG):**

- Short-term (< 1 year): 15% tax on gains
- Long-term (> 1 year): 10% on gains above ₹1 lakh, 0% below
- Loss harvesting: Short-term loss offsets short-term gain first, then long-term
- Long-term loss only offsets long-term gain
- Carry forward losses up to 8 years

**Transaction Matching (FIFO):**

- First In First Out: Oldest purchase matched to sale first
- If partial sale, split the purchase lot
- Track remaining quantity after each sale

**Dividend Tax:**

- Dividends added to income, taxed at slab rate
- TDS already deducted at 10%

**Special Cases:**

- Bonus shares: Cost basis is zero but holding period from original purchase
- Stock split: Adjust quantity and price, holding period unchanged
- STT (Securities Transaction Tax): Deductible from gains

Given transaction history and financial year, calculate total tax liability with complete breakdown."

**Why This is Hard:**

- Multiple tax categories
- FIFO matching across years
- Loss carry-forward tracking
- Special corporate actions


**Problem Statement:**
```javascript
/**
 * Calculate comprehensive tax liability
 * @param {Object[]} transactions - [{
 *   date: [D,D,M,M,Y,Y,Y,Y],
 *   type: 'buy'|'sell'|'dividend'|'bonus'|'split',
 *   stock: string,
 *   quantity: number,
 *   price: number,
 *   stt: number,
 *   splitRatio?: number // for split: 1:2 means 1->2
 * }]
 * @param {number} financialYearStart - Year (e.g., 2024 for FY 2024-25)
 * @param {Object} previousLosses - {stcg: number, ltcg: number, year: number}
 * @returns {Object} {
 *   stcgTax: number,
 *   ltcgTax: number,
 *   dividendTax: number,
 *   totalTax: number,
 *   breakdown: {
 *     stcgGain: number,
 *     stcgLoss: number,
 *     ltcgGain: number,
 *     ltcgLoss: number,
 *     netStcg: number,
 *     netLtcg: number,
 *     ltcgExemption: 100000
 *   },
 *   carryForwardLosses: {stcg: number, ltcg: number},
 *   transactions: [{buy, sell, gain, holding, taxType}]
 * }
 */
function calculateStockTax(transactions, financialYearStart, previousLosses) {
    // Your code here
}
```

**Test Cases:**
```javascript
// Test 1: Simple LTCG above exemption
console.log(calculateStockTax(
    [
        {date: [0,1,0,4,2,0,2,3], type: 'buy', stock: 'RELIANCE', quantity: 100, price: 2000, stt: 200},
        {date: [0,1,0,6,2,0,2,4], type: 'sell', stock: 'RELIANCE', quantity: 100, price: 2500, stt: 250}
    ],
    2024,
    {stcg: 0, ltcg: 0, year: 2024}
));
/* Expected: {
    stcgTax: 0,
    ltcgTax: 5000, // (250000-200000-100000) * 0.10
    dividendTax: 0,
    totalTax: 5000,
    breakdown: {
        stcgGain: 0,
        ltcgGain: 49550, // After STT deduction
        netLtcg: 49550,
        ltcgExemption: 100000
    },
    carryForwardLosses: {stcg: 0, ltcg: 0},
    transactions: [...]
} */

// Test 2: STCG with loss harvesting
console.log(calculateStockTax(
    [
        {date: [0,1,0,1,2,0,2,4], type: 'buy', stock: 'TCS', quantity: 50, price: 3000, stt: 150},
        {date: [0,1,0,3,2,0,2,4], type: 'sell', stock: 'TCS', quantity: 50, price: 3500, stt: 175},
        {date: [0,1,0,2,2,0,2,4], type: 'buy', stock: 'INFY', quantity: 100, price: 1500, stt: 150},
        {date: [0,1,0,4,2,0,2,4], type: 'sell', stock: 'INFY', quantity: 100, price: 1300, stt: 130}
    ],
    2024,
    {stcg: 0, ltcg: 0, year: 2024}
));
/* Expected: {
    stcgTax: 2253.75, // (25000-20000) * 0.15 after loss offset
    breakdown: {
        stcgGain: 24675, // TCS gain after STT
        stcgLoss: -20280, // INFY loss after STT
        netStcg: 4395
    }
} */

// Test 3: Bonus shares and stock split
console.log(calculateStockTax(
    [
        {date: [0,1,0,1,2,0,2,2], type: 'buy', stock: 'HDFC', quantity: 100, price: 2000, stt: 200},
        {date: [0,1,0,6,2,0,2,3], type: 'bonus', stock: 'HDFC', quantity: 50, price: 0, stt: 0},
        {date: [0,1,0,1,2,0,2,4], type: 'split', stock: 'HDFC', quantity: 150, splitRatio: 2, stt: 0},
        {date: [0,1,0,8,2,0,2,4], type: 'sell', stock: 'HDFC', quantity: 300, price: 1100, stt: 330}
    ],
    2024,
    {stcg: 0, ltcg: 0, year: 2024}
));
/* Expected: Complex calculation
- Original 100 shares @ 2000
- Bonus 50 @ 0
- Split 2:1 makes it 300 shares
- Adjusted cost basis per share
- Holding from 2022, so LTCG
*/
```

**
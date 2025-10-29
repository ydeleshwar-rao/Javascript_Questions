# React Hooks - Complete Deep Dive Guide 🎣

Comprehensive guide covering every React hook with tricky concepts, loopholes, and advanced patterns.

---

## 1. useState 🔄

### Basic Concept
State management hook that returns current state and updater function.

### Signature
```javascript
const [state, setState] = useState(initialValue)
```

### What it Returns
- **Array with 2 elements:**
  1. Current state value
  2. Setter function to update state

### Parameters
- `initialValue`: Can be any type (primitive, object, array, function)

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Lazy Initialization - Performance Trap**
```javascript
// ❌ BAD - Expensive calculation runs on every render
const [state, setState] = useState(expensiveCalculation())

// ✅ GOOD - Runs only once on mount
const [state, setState] = useState(() => expensiveCalculation())
```
**Why:** Without function, calculation runs every render even though initial value is used only once.

#### 2. **Batching Behavior - Async Updates**
```javascript
const [count, setCount] = useState(0)

// ❌ Unreliable - May not update 3 times
setCount(count + 1)
setCount(count + 1)
setCount(count + 1)
// Result: count = 1 (not 3!)

// ✅ Correct - Uses functional updates
setCount(prev => prev + 1)
setCount(prev => prev + 1)
setCount(prev => prev + 1)
// Result: count = 3
```
**Why:** React batches state updates. All updates use same `count` value if not using functional form.

#### 3. **Object/Array Reference Trap**
```javascript
const [user, setUser] = useState({ name: 'John', age: 25 })

// ❌ BAD - Mutates original state (React may not re-render)
user.age = 26
setUser(user)

// ✅ GOOD - Creates new reference
setUser({ ...user, age: 26 })
// OR
setUser(prev => ({ ...prev, age: 26 }))
```
**Why:** React uses `Object.is` comparison. Same reference = no re-render.

#### 4. **Stale Closure Problem**
```javascript
const [count, setCount] = useState(0)

useEffect(() => {
  const interval = setInterval(() => {
    console.log(count) // ❌ Always logs 0 (stale closure)
    setCount(count + 1) // ❌ Always sets to 1
  }, 1000)
  return () => clearInterval(interval)
}, []) // Empty dependency array

// ✅ Solution 1: Use functional update
setCount(prev => prev + 1)

// ✅ Solution 2: Add count to dependencies
}, [count])
```
**Why:** Closure captures initial `count` value (0) and never updates.

#### 5. **Initial State Type Mismatch**
```javascript
// ❌ Type can change - causes bugs
const [value, setValue] = useState(null)
setValue(42) // Now it's a number
setValue("hello") // Now it's a string

// ✅ Better - consistent typing
const [value, setValue] = useState<number | null>(null)
```
**Why:** TypeScript can't infer consistent type from `null`, leading to type errors.

---

## 2. useEffect 🎭

### Basic Concept
Handles side effects (API calls, subscriptions, timers, DOM mutations).

### Signature
```javascript
useEffect(callback, dependencies?)
```

### What it Returns
- **Nothing** (undefined)
- Callback can return cleanup function

### Parameters
1. **callback**: Function containing side effect logic
2. **dependencies**: Optional array controlling when effect runs

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Dependency Array Types & Triggers**
```javascript
// Type 1: No dependency array - Runs after EVERY render
useEffect(() => {
  console.log('Runs on every render')
})

// Type 2: Empty array [] - Runs ONCE on mount only
useEffect(() => {
  console.log('Runs only on mount')
}, [])

// Type 3: With dependencies - Runs when ANY dependency changes
useEffect(() => {
  console.log('Runs when count or name changes')
}, [count, name])
```

**Supported Dependency Types:**
- ✅ Primitives (number, string, boolean)
- ✅ Props
- ✅ State variables
- ✅ Context values
- ⚠️ Objects/Arrays (by reference, not deep equality)
- ❌ Functions (unless wrapped in useCallback)

**Trigger Rules:**
```javascript
// Primitives - triggers on value change
const [count, setCount] = useState(0)
useEffect(() => {}, [count]) // Triggers when count value changes

// Objects - triggers on reference change
const [user, setUser] = useState({ name: 'John' })
useEffect(() => {}, [user]) // Triggers only if user reference changes

// ❌ This WON'T trigger effect (same reference)
user.name = 'Jane'
setUser(user)

// ✅ This WILL trigger (new reference)
setUser({ ...user, name: 'Jane' })
```

#### 2. **Cleanup Function Timing**
```javascript
useEffect(() => {
  console.log('Effect runs')
  
  return () => {
    console.log('Cleanup runs')
  }
}, [dependency])

// Order of execution:
// 1. Component renders
// 2. DOM updates
// 3. Effect runs
// 4. Dependency changes
// 5. Cleanup runs FIRST
// 6. New effect runs
// 7. Component unmounts
// 8. Final cleanup runs
```

**Common Mistake:**
```javascript
// ❌ BAD - Cleanup runs before new API call
useEffect(() => {
  const controller = new AbortController()
  fetchData(controller.signal)
  
  return () => controller.abort() // Cancels current request
}, [id])

// When id changes rapidly:
// 1. Cleanup aborts request for id=1
// 2. New effect starts request for id=2
// 3. If id changes again, request for id=2 is aborted
```

#### 3. **Infinite Loop Trap**
```javascript
const [data, setData] = useState([])

// ❌ INFINITE LOOP
useEffect(() => {
  setData([...data, 'new item']) // Creates new array reference
}, [data]) // Triggers effect because data changed!

// ✅ Solution 1: Remove from dependencies
useEffect(() => {
  setData(prev => [...prev, 'new item'])
}, []) // Runs once

// ✅ Solution 2: Use different trigger
useEffect(() => {
  setData(prev => [...prev, 'new item'])
}, [someOtherTrigger])
```

#### 4. **Object/Function Dependencies**
```javascript
// ❌ BAD - Creates new object every render
const [data, setData] = useState(null)
const config = { url: '/api', method: 'GET' } // New object every render!

useEffect(() => {
  fetchData(config)
}, [config]) // Runs on EVERY render

// ✅ Solution 1: Move inside effect
useEffect(() => {
  const config = { url: '/api', method: 'GET' }
  fetchData(config)
}, [])

// ✅ Solution 2: useMemo
const config = useMemo(() => ({ url: '/api', method: 'GET' }), [])

// ✅ Solution 3: Primitive dependencies only
useEffect(() => {
  fetchData({ url, method })
}, [url, method])
```

#### 5. **Race Condition with Async**
```javascript
// ❌ BAD - Race condition
useEffect(() => {
  async function fetchData() {
    const result = await fetch(`/api/user/${userId}`)
    const data = await result.json()
    setUser(data) // May set stale data if userId changed
  }
  fetchData()
}, [userId])

// ✅ Solution: Ignore flag
useEffect(() => {
  let ignore = false
  
  async function fetchData() {
    const result = await fetch(`/api/user/${userId}`)
    const data = await result.json()
    if (!ignore) setUser(data) // Only update if still relevant
  }
  
  fetchData()
  return () => { ignore = true }
}, [userId])
```

---

## 3. useContext 🌐

### Basic Concept
Reads value from React Context without wrapping components in Consumer.

### Signature
```javascript
const value = useContext(MyContext)
```

### What it Returns
- Current context value from nearest Provider above in tree

### Parameters
- `MyContext`: Context object created by `React.createContext()`

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Re-render on Any Context Change**
```javascript
const ThemeContext = createContext()

// Provider with object value
<ThemeContext.Provider value={{ theme: 'dark', user: 'John' }}>
  <ComponentA />
  <ComponentB />
</ThemeContext.Provider>

// ❌ BAD - Both components re-render when ANY value changes
function ComponentA() {
  const { theme } = useContext(ThemeContext)
  return <div>{theme}</div>
}

function ComponentB() {
  const { user } = useContext(ThemeContext)
  return <div>{user}</div>
}

// ✅ Solution: Split contexts
const ThemeContext = createContext()
const UserContext = createContext()
```
**Why:** useContext subscribes to entire context object. Even if you use only one property, component re-renders when any property changes.

#### 2. **Provider Value Reference Trap**
```javascript
function App() {
  const [theme, setTheme] = useState('dark')
  
  // ❌ BAD - Creates new object every render
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Child />
    </ThemeContext.Provider>
  )
  // Every render creates new value object = all consumers re-render
}

// ✅ Solution: useMemo
function App() {
  const [theme, setTheme] = useState('dark')
  const value = useMemo(() => ({ theme, setTheme }), [theme])
  
  return (
    <ThemeContext.Provider value={value}>
      <Child />
    </ThemeContext.Provider>
  )
}
```

#### 3. **Missing Provider Error**
```javascript
const MyContext = createContext()

function Child() {
  const value = useContext(MyContext)
  console.log(value) // undefined (not error!)
  return <div>{value}</div> // May cause runtime error
}

// ❌ No provider in tree
<Child />

// ✅ Solution: Default value
const MyContext = createContext('default value')

// ✅ Better: Error boundary
function Child() {
  const value = useContext(MyContext)
  if (value === undefined) {
    throw new Error('useMyContext must be used within MyProvider')
  }
}
```

#### 4. **Context Selector Problem (No Built-in)**
```javascript
// React context doesn't support selectors
const value = useContext(MyContext) // Gets entire object

// ❌ Can't do this:
const theme = useContext(MyContext, state => state.theme)

// ✅ Solutions:
// 1. Split contexts (best for simple cases)
// 2. Use Zustand/Redux for complex state
// 3. Custom hook with useSyncExternalStore
```

#### 5. **Dynamic Context Values**
```javascript
// ❌ Context value changes but component doesn't re-render
const ValueContext = createContext()

function Parent() {
  let value = 'initial'
  
  setTimeout(() => {
    value = 'updated' // ❌ Doesn't trigger re-render
  }, 1000)
  
  return (
    <ValueContext.Provider value={value}>
      <Child />
    </ValueContext.Provider>
  )
}

// ✅ Must use state
function Parent() {
  const [value, setValue] = useState('initial')
  
  useEffect(() => {
    setTimeout(() => setValue('updated'), 1000)
  }, [])
}
```

---

## 4. useReducer ⚙️

### Basic Concept
Alternative to useState for complex state logic. Similar to Redux pattern.

### Signature
```javascript
const [state, dispatch] = useReducer(reducer, initialState, init?)
```

### What it Returns
- **Array with 2 elements:**
  1. Current state
  2. Dispatch function to send actions

### Parameters
1. **reducer**: Function `(state, action) => newState`
2. **initialState**: Initial state value
3. **init** (optional): Function for lazy initialization `(initialState) => computedInitialState`

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Reducer Must Be Pure Function**
```javascript
// ❌ BAD - Mutates state directly
function reducer(state, action) {
  switch (action.type) {
    case 'ADD':
      state.items.push(action.item) // ❌ Mutation!
      return state // ❌ Same reference
    default:
      return state
  }
}

// ✅ GOOD - Returns new state
function reducer(state, action) {
  switch (action.type) {
    case 'ADD':
      return {
        ...state,
        items: [...state.items, action.item]
      }
    default:
      return state
  }
}
```

#### 2. **Lazy Initialization (Third Parameter)**
```javascript
// ❌ Expensive calculation runs every render
const [state, dispatch] = useReducer(
  reducer,
  expensiveCalculation(props)
)

// ✅ Runs only once - pass function as third argument
const [state, dispatch] = useReducer(
  reducer,
  props, // Pass props as second arg
  (props) => expensiveCalculation(props) // Runs once
)
```

#### 3. **Dispatch Identity is Stable**
```javascript
function Component() {
  const [state, dispatch] = useReducer(reducer, initialState)
  
  // ✅ dispatch never changes - safe to omit from dependencies
  useEffect(() => {
    dispatch({ type: 'FETCH' })
  }, []) // No need to add dispatch
  
  // ✅ Safe to pass to children without causing re-renders
  return <Child onAction={dispatch} />
}
```

#### 4. **Bail Out of State Update**
```javascript
function reducer(state, action) {
  switch (action.type) {
    case 'RESET':
      // ❌ Always returns new object - component re-renders
      return { ...initialState }
    
    case 'UPDATE':
      if (state.value === action.value) {
        return state // ✅ Returns same reference - no re-render!
      }
      return { ...state, value: action.value }
  }
}
```

#### 5. **Action Object vs Function**
```javascript
// Most people use objects
dispatch({ type: 'ADD', payload: data })

// ✅ But actions can be functions too (like Redux Thunk)
function reducer(state, action) {
  if (typeof action === 'function') {
    return action(state) // Execute function with current state
  }
  // Regular action handling
}

// Usage:
dispatch(currentState => ({
  ...currentState,
  count: currentState.count + 1
}))

// ⚠️ This pattern is NOT standard - use with caution
```

---

## 5. useCallback 🔗

### Basic Concept
Memoizes function references to prevent unnecessary re-creation.

### Signature
```javascript
const memoizedCallback = useCallback(callback, dependencies)
```

### What it Returns
- Memoized version of callback that only changes if dependencies change

### Parameters
1. **callback**: The function to memoize
2. **dependencies**: Array of values that callback depends on

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **When NOT to Use useCallback**
```javascript
// ❌ USELESS - No child component, no effect dependency
function Parent() {
  const handleClick = useCallback(() => {
    console.log('clicked')
  }, [])
  
  return <button onClick={handleClick}>Click</button>
  // Native DOM elements don't care about function reference
}

// ✅ USEFUL - Prevents child re-render
const MemoizedChild = memo(({ onClick }) => {
  return <button onClick={onClick}>Click</button>
})

function Parent() {
  const handleClick = useCallback(() => {
    console.log('clicked')
  }, [])
  
  return <MemoizedChild onClick={handleClick} />
}
```

#### 2. **Dependencies in Closure**
```javascript
function Component() {
  const [count, setCount] = useState(0)
  const [multiplier, setMultiplier] = useState(2)
  
  // ❌ BAD - Stale closure (count never updates)
  const handleClick = useCallback(() => {
    console.log(count * multiplier) // Uses initial values
  }, []) // Missing dependencies!
  
  // ✅ GOOD - Include all used values
  const handleClick = useCallback(() => {
    console.log(count * multiplier)
  }, [count, multiplier])
  
  // ✅ BETTER - Use functional update (fewer dependencies)
  const incrementCount = useCallback(() => {
    setCount(prev => prev + 1)
  }, []) // setCount is stable, no other dependencies needed
}
```

#### 3. **useCallback with Objects/Arrays**
```javascript
// ❌ BAD - Creates new object every render anyway
function Component() {
  const [id, setId] = useState(1)
  
  const config = { id, type: 'user' } // New object every render
  
  const fetchData = useCallback(() => {
    fetch('/api', { body: JSON.stringify(config) })
  }, [config]) // config changes every render!
}

// ✅ Solution 1: Destructure dependencies
const fetchData = useCallback(() => {
  const config = { id, type: 'user' }
  fetch('/api', { body: JSON.stringify(config) })
}, [id]) // Only id as dependency

// ✅ Solution 2: useMemo for config
const config = useMemo(() => ({ id, type: 'user' }), [id])
const fetchData = useCallback(() => {
  fetch('/api', { body: JSON.stringify(config) })
}, [config])
```

#### 4. **Circular Dependency Problem**
```javascript
// ❌ Problem: A needs B, B needs A
const functionA = useCallback(() => {
  functionB()
}, [functionB])

const functionB = useCallback(() => {
  functionA()
}, [functionA])

// ✅ Solution 1: Combine into one function
const combined = useCallback(() => {
  // Logic here
}, [dependencies])

// ✅ Solution 2: Use refs
const functionARef = useRef()
const functionBRef = useRef()

functionARef.current = () => functionBRef.current()
functionBRef.current = () => functionARef.current()

const functionA = useCallback(() => functionARef.current(), [])
const functionB = useCallback(() => functionBRef.current(), [])
```

#### 5. **Performance Cost**
```javascript
// ❌ Premature optimization - useCallback has overhead
function SimpleComponent() {
  const handleClick = useCallback(() => {
    console.log('clicked')
  }, [])
  
  return <button onClick={handleClick}>Click</button>
}

// Cost breakdown:
// 1. Create function
// 2. useCallback internal logic
// 3. Dependency comparison
// 4. Store in memory
// vs just creating new function (which is very cheap)

// ✅ Only use when:
// 1. Passed to memoized child components
// 2. Used in effect dependencies
// 3. Passed to custom hooks that optimize
// 4. Expensive function creation (rare)
```

---

## 6. useMemo 💎

### Basic Concept
Memoizes computed values to avoid expensive recalculations.

### Signature
```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])
```

### What it Returns
- Memoized result of the computation

### Parameters
1. **callback**: Function that returns computed value
2. **dependencies**: Array of values computation depends on

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **When NOT to Use useMemo**
```javascript
// ❌ USELESS - Simple calculation
const doubled = useMemo(() => count * 2, [count])
// Overhead of useMemo > cost of multiplication

// ❌ USELESS - Already memoized by React
const element = useMemo(() => <Child />, [])
// JSX already doesn't recreate unnecessarily

// ✅ USEFUL - Expensive calculation
const sortedData = useMemo(() => {
  return data.slice().sort((a, b) => {
    // Complex sorting logic
  })
}, [data])

// ✅ USEFUL - Referential equality for dependencies
const style = useMemo(() => ({
  color: 'red',
  fontSize: size
}), [size])
```

#### 2. **useMemo vs useCallback Relationship**
```javascript
// These are equivalent:
const memoizedCallback = useCallback(() => {
  doSomething(a, b)
}, [a, b])

const memoizedCallback = useMemo(() => {
  return () => doSomething(a, b)
}, [a, b])

// useCallback(fn, deps) = useMemo(() => fn, deps)
```

#### 3. **Dependency Array Gotchas**
```javascript
const [items, setItems] = useState([1, 2, 3])

// ❌ BAD - Array as dependency (compares by reference)
const total = useMemo(() => {
  return items.reduce((sum, item) => sum + item, 0)
}, [items])
// Recalculates only when items reference changes, not content

// If you do: items[0] = 10, total won't update!

// ✅ Solution 1: Ensure immutable updates
setItems([...items]) // Creates new reference

// ✅ Solution 2: Use items.length + hash
const total = useMemo(() => {
  return items.reduce((sum, item) => sum + item, 0)
}, [items.length, JSON.stringify(items)]) // ⚠️ Heavy for large arrays

// ✅ Solution 3: Don't memoize if you mutate
// Just recalculate - it's cheaper than memoization overhead
```

#### 4. **Memory Leak Potential**
```javascript
// ❌ BAD - Large data cached forever
const [filter, setFilter] = useState('')
const [data] = useState(() => generateHugeDataset())

const filtered = useMemo(() => {
  return data.filter(item => item.name.includes(filter))
}, [filter, data])
// Both original data AND filtered copy in memory!

// ✅ Solution: Consider if memoization is worth it
// Sometimes re-filtering is cheaper than memory cost
```

#### 5. **useMemo Doesn't Guarantee Cache**
```javascript
// ⚠️ React may discard memoized values under memory pressure
const expensive = useMemo(() => {
  return expensiveCalculation()
}, [dep])

// React can:
// 1. Keep the cache (normal case)
// 2. Discard cache and recalculate (low memory)

// This is by design! useMemo is optimization, not semantic guarantee

// ✅ If you need guarantee, use state:
const [cached, setCached] = useState(null)
useEffect(() => {
  setCached(expensiveCalculation())
}, [dep])
```

---

## 7. useRef 📌

### Basic Concept
Creates mutable reference that persists across renders without causing re-renders.

### Signature
```javascript
const refContainer = useRef(initialValue)
```

### What it Returns
- Object with `.current` property: `{ current: initialValue }`

### Parameters
- `initialValue`: Initial value for `ref.current`

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Mutating .current Doesn't Trigger Re-render**
```javascript
function Component() {
  const countRef = useRef(0)
  
  const handleClick = () => {
    countRef.current = countRef.current + 1
    console.log(countRef.current) // Updates
    // ❌ Component doesn't re-render! UI shows old value
  }
  
  return <div>{countRef.current}</div>
}

// ✅ Use state if you need re-render
const [count, setCount] = useState(0)

// ✅ Use ref for values you DON'T want to trigger re-render
const renderCount = useRef(0)
useEffect(() => {
  renderCount.current += 1
})
```

#### 2. **Accessing DOM Elements**
```javascript
function Component() {
  const inputRef = useRef(null)
  
  useEffect(() => {
    // ❌ BAD - null on first render
    inputRef.current.focus()
    
    // ✅ GOOD - Check for null
    if (inputRef.current) {
      inputRef.current.focus()
    }
  }, [])
  
  return <input ref={inputRef} />
}

// ⚠️ Ref attaches AFTER render
// Sequence:
// 1. Component renders, inputRef.current = null
// 2. DOM updates
// 3. Ref attaches, inputRef.current = DOM node
// 4. useEffect runs
```

#### 3. **Ref Callback Pattern**
```javascript
// Standard ref
<div ref={myRef} />

// ✅ Ref callback - called when ref attaches/detaches
<div ref={(element) => {
  console.log('Ref attached:', element)
  // Called with DOM node on mount
  // Called with null on unmount
}} />

// ✅ Useful for dynamic refs
function Component({ items }) {
  const itemRefs = useRef({})
  
  return items.map(item => (
    <div
      key={item.id}
      ref={(el) => {
        if (el) {
          itemRefs.current[item.id] = el
        } else {
          delete itemRefs.current[item.id]
        }
      }}
    />
  ))
}
```

#### 4. **Storing Previous Values**
```javascript
// ✅ Store previous prop/state value
function Component({ value }) {
  const prevValueRef = useRef()
  
  useEffect(() => {
    prevValueRef.current = value
  }, [value])
  
  const prevValue = prevValueRef.current
  
  console.log('Previous:', prevValue, 'Current:', value)
}

// Execution order:
// 1. Render phase: prevValue is old value
// 2. Commit phase: useEffect updates ref to new value
// 3. Next render: prevValue has previous render's value
```

#### 5. **Ref with Functions (Advanced Pattern)**
```javascript
// ❌ Common mistake - function recreated every render
function Component() {
  const handleClick = () => {
    console.log('clicked')
  }
  
  useEffect(() => {
    document.addEventListener('click', handleClick)
    return () => document.removeEventListener('click', handleClick)
  }, [handleClick]) // Changes every render!
}

// ✅ Solution: Store in ref
function Component() {
  const handleClickRef = useRef()
  
  handleClickRef.current = () => {
    console.log('clicked')
  }
  
  useEffect(() => {
    const handler = () => handleClickRef.current()
    document.addEventListener('click', handler)
    return () => document.removeEventListener('click', handler)
  }, []) // Empty deps - handler always uses latest function
}
```

---

## 8. useImperativeHandle 🎯

### Basic Concept
Customizes the instance value exposed to parent when using `ref`.

### Signature
```javascript
useImperativeHandle(ref, createHandle, dependencies?)
```

### What it Returns
- Nothing (undefined)

### Parameters
1. **ref**: Ref passed from parent via `forwardRef`
2. **createHandle**: Function returning object with methods to expose
3. **dependencies**: Optional array controlling when handle is recreated

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Must Use with forwardRef**
```javascript
// ❌ DOESN'T WORK
function Child() {
  useImperativeHandle(???, () => ({
    focus: () => {}
  }))
}

// ✅ CORRECT
const Child = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    focus: () => {
      // Custom focus logic
    }
  }))
  
  return <input />
})

// Usage in parent:
const childRef = useRef()
<Child ref={childRef} />
childRef.current.focus()
```

#### 2. **Hiding Internal Implementation**
```javascript
const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef()
  const [isValid, setIsValid] = useState(true)
  
  // ❌ BAD - Exposes entire DOM element
  // ref = inputRef
  
  // ✅ GOOD - Only expose what parent should use
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus()
    },
    validate: () => {
      const valid = inputRef.current.value.length > 0
      setIsValid(valid)
      return valid
    }
    // Parent CAN'T access: inputRef.current.value directly
    // Parent CAN'T access: setIsValid
  }))
  
  return <input ref={inputRef} />
})

// ✅ Encapsulation achieved
```

#### 3. **Dependencies Control Recreation**
```javascript
const Child = forwardRef((props, ref) => {
  const [count, setCount] = useState(0)
  
  // ❌ No dependencies - recreates every render
  useImperativeHandle(ref, () => ({
    getCount: () => count
  }))
  
  // ✅ With dependencies - stable reference
  useImperativeHandle(ref, () => ({
    getCount: () => count
  }), [count]) // Recreates only when count changes
  
  // ⚠️ Empty array - stale closure
  useImperativeHandle(ref, () => ({
    getCount: () => count // Always returns initial count!
  }), [])
})
```

#### 4. **Async Methods**
```javascript
const AsyncComponent = forwardRef((props, ref) => {
  const [data, setData] = useState(null)
  
  useImperativeHandle(ref, () => ({
    // ✅ Can expose async methods
    async fetchData() {
      const response = await fetch('/api/data')
      const result = await response.json()
      setData(result)
      return result
    }
  }), [])
  
  return <div>{data}</div>
})

// Usage:
const ref = useRef()
await ref.current.fetchData()
```

#### 5. **Anti-Pattern Warning**
```javascript
// ⚠️ useImperativeHandle is escape hatch - use sparingly!

// ❌ DON'T use for simple prop passing
const BadChild = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    setValue: (val) => setValue(val)
  }))
})
// ✅ Just use props: <Child onChange={val => setVal(val)} />

// ✅ DO use for:
// 1. Exposing DOM methods (focus, scroll, play)
// 2. Complex imperative APIs (form validation, animation controls)
// 3. Third-party library integration
```

---

## 9. useLayoutEffect ⚡

### Basic Concept
Identical to useEffect but fires synchronously after DOM mutations, before browser paint.

### Signature
```javascript
useLayoutEffect(callback, dependencies?)
```

### What it Returns
- Nothing (undefined)
- Callback can return cleanup function

### Parameters
- Same as useEffect

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Timing Difference from useEffect**
```javascript
// Execution order:
// 1. React updates DOM
// 2. useLayoutEffect runs (BLOCKS paint)
// 3. Browser paints screen
// 4. useEffect runs

function Component() {
  useLayoutEffect(() => {
    console.log('1. Layout effect')
  })
  
  useEffect(() => {
    console.log('2. Effect')
  })
  
  console.log('0. Render')
}

// Output:
// 0. Render
// 1. Layout effect
// 2. Effect
```

#### 2. **When to Use useLayoutEffect**
```javascript
// ❌ BAD - useEffect causes flicker
function Tooltip() {
  const [position, setPosition] = useState({ x: 0, y: 0 })
  const ref = useRef()
  
  useEffect(() => {
    const rect = ref.current.getBoundingClientRect()
    setPosition({ x: rect.x, y: rect.y })
  }, [])
  
  // Flicker happens:
  // 1. Component renders at (0, 0)
  // 2. Browser paints tooltip at wrong position
  // 3. useEffect calculates correct position
  // 4. Component re-renders
  // 5. Browser paints again at correct position
  
  return <div ref={ref} style={{ left: position.x, top: position.y }} />
}

// ✅ GOOD - No flicker
function Tooltip() {
  const [position, setPosition] = useState({ x: 0, y: 0 })
  const ref = useRef()
  
  useLayoutEffect(() => {
    const rect = ref.current.getBoundingClientRect()
    setPosition({ x: rect.x, y: rect.y })
  }, [])
  
  // No flicker:
  // 1. Component renders at (0, 0)
  // 2. useLayoutEffect calculates correct position
  // 3. Component re-renders
  // 4. Browser paints ONCE at correct position
  
  return <div ref={ref} style={{ left: position.x, top: position.y }} />
}
```

#### 3. **Performance Impact**
```javascript
// ⚠️ useLayoutEffect BLOCKS painting

function BadComponent() {
  useLayoutEffect(() => {
    // ❌ Heavy calculation blocks paint
    for (let i = 0; i < 1000000000; i++) {}
  })
  // User sees frozen UI!
}

// ✅ Use useEffect for non-visual side effects
function GoodComponent() {
  useEffect(() => {
    // Heavy work doesn't block paint
    for (let i = 0; i < 1000000000; i++) {}
  })
}

// Rule: Only use useLayoutEffect if:
// 1. You need DOM measurements
// 2. You're preventing visual flicker
// 3. You're mutating DOM before paint
```

#### 4. **Server-Side Rendering Warning**
```javascript
// ⚠️ useLayoutEffect doesn't run on server

// This causes hydration mismatch:
function Component() {
  const [isClient, setIsClient] = useState(false)
  
  useLayoutEffect(() => {
    setIsClient(true) // Only runs on client
  }, [])
  
  return <div>{isClient ? 'Client' : 'Server'}</div>
  // Server renders: <div>Server</div>
  // Client hydrates: expects <div>Server</div>
  // But immediately shows: <div>Client</div>
  // ❌ Hydration mismatch warning!
}

// ✅ Solution: useEffect + isomorphic check
function Component() {
  const [isClient, setIsClient] = useState(false)
  
  useEffect(() => {
    setIsClient(true)
  }, [])
  
  return <div>{isClient ? 'Client' : 'Server'}</div>
}
```

#### 5. **Nested Layout Effects**
```javascript
function Parent() {
  useLayoutEffect(() => {
    console.log('1. Parent layout effect')
  })
  
  return <Child />
}

function Child() {
  useLayoutEffect(() => {
    console.log('2. Child layout effect')
  })
  
  return <GrandChild />
}

function GrandChild() {
  useLayoutEffect(() => {
    console.log('3. GrandChild layout effect')
  })
  
  return <div />
}

// Output (bottom-up order):
// 3. GrandChild layout effect
// 2. Child layout effect
// 1. Parent layout effect

// ⚠️ Child effects run before parent effects!
// This is opposite of component mounting order
```

---

## 10. useDebugValue 🐛

### Basic Concept
Shows custom label in React DevTools for custom hooks.

### Signature
```javascript
useDebugValue(value, format?)
```

### What it Returns
- Nothing (undefined)

### Parameters
1. **value**: Value to display in DevTools
2. **format** (optional): Function to format display value (only called when DevTools is open)

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Only for Custom Hooks**
```javascript
// ❌ USELESS - In component
function Component() {
  const [count, setCount] = useState(0)
  useDebugValue(count) // Doesn't show anywhere
  return <div>{count}</div>
}

// ✅ USEFUL - In custom hook
function useCustomCounter() {
  const [count, setCount] = useState(0)
  useDebugValue(count) // Shows in DevTools
  return [count, setCount]
}
```

#### 2. **Format Function for Expensive Formatting**
```javascript
// ❌ Formats every render (expensive)
function useUser(userId) {
  const user = fetchUser(userId)
  useDebugValue(JSON.stringify(user)) // Always stringifies
  return user
}

// ✅ Only formats when DevTools open
function useUser(userId) {
  const user = fetchUser(userId)
  useDebugValue(user, u => JSON.stringify(u))
  // Format function only called in DevTools
  return user
}
```

#### 3. **Multiple Debug Values**
```javascript
// ❌ Only last one shows
function useCustomHook() {
  useDebugValue('First')
  useDebugValue('Second')
  useDebugValue('Third')
  // DevTools shows: "Third"
}

// ✅ Combine into object or string
function useCustomHook() {
  const state1 = useState(0)
  const state2 = useState('')
  
  useDebugValue({
    count: state1[0],
    text: state2[0]
  })
  // DevTools shows: { count: 0, text: "" }
}
```

#### 4. **Conditional Debug Values**
```javascript
function useDataFetching(url) {
  const [data, setData] = useState(null)
  const [isLoading, setIsLoading] = useState(true)
  const [error, setError] = useState(null)
  
  // ✅ Show relevant debug info based on state
  useDebugValue(
    isLoading ? 'Loading...' :
    error ? `Error: ${error.message}` :
    data ? `Loaded (${Object.keys(data).length} keys)` :
    'No data'
  )
  
  return { data, isLoading, error }
}
```

#### 5. **Production Build Optimization**
```javascript
// ⚠️ In production builds, useDebugValue is removed
// by React's dead code elimination

// This is safe - no performance impact in production
function useExpensiveHook() {
  const value = expensiveCalculation()
  
  useDebugValue(value, v => {
    // This expensive formatting only happens in:
    // 1. Development builds
    // 2. When React DevTools is open
    return JSON.stringify(v)
  })
  
  return value
}
```

---

## 11. useDeferredValue ⏳

### Basic Concept
Defers updating part of UI to keep interface responsive during expensive renders.

### Signature
```javascript
const deferredValue = useDeferredValue(value)
```

### What it Returns
- Deferred version of value (may lag behind actual value)

### Parameters
- **value**: The value you want to defer

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Not a Debounce/Throttle**
```javascript
// ❌ Common misconception - NOT a timer-based delay
function Search() {
  const [query, setQuery] = useState('')
  const deferredQuery = useDeferredValue(query)
  
  // deferredQuery doesn't wait X milliseconds
  // It updates based on React's priority system
  
  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <Results query={deferredQuery} />
    </>
  )
}

// Behavior:
// User types "a" -> query = "a", deferredQuery = ""
// User types "b" -> query = "ab", deferredQuery = "a"
// User stops -> deferredQuery catches up = "ab"
```

#### 2. **Works with Suspense Boundary**
```javascript
// ✅ Best results with Suspense
function App() {
  const [query, setQuery] = useState('')
  const deferredQuery = useDeferredValue(query)
  
  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <Suspense fallback={<div>Loading...</div>}>
        <Results query={deferredQuery} />
      </Suspense>
    </>
  )
}

// When query changes:
// 1. Input updates immediately (high priority)
// 2. Old results stay visible (doesn't show fallback)
// 3. New results render in background (low priority)
// 4. When ready, swap to new results
```

#### 3. **Primitive Values Only (Effectively)**
```javascript
// ⚠️ Works but creates new reference
const [obj, setObj] = useState({ name: 'John' })
const deferredObj = useDeferredValue(obj)

// Every update creates new deferred object
// Not ideal for object comparisons

// ✅ Better: Defer primitive values
const [name, setName] = useState('John')
const deferredName = useDeferredValue(name)
```

#### 4. **Memoization Required for Optimization**
```javascript
// ❌ BAD - Results always re-renders
function Search() {
  const [query, setQuery] = useState('')
  const deferredQuery = useDeferredValue(query)
  
  return <Results query={deferredQuery} />
  // Results re-renders even when deferredQuery hasn't changed
}

// ✅ GOOD - Memoize expensive component
const MemoizedResults = memo(function Results({ query }) {
  // Expensive render
  return <div>{/* ... */}</div>
})

function Search() {
  const [query, setQuery] = useState('')
  const deferredQuery = useDeferredValue(query)
  
  return <MemoizedResults query={deferredQuery} />
  // Only re-renders when deferredQuery actually changes
}
```

#### 5. **Initial Value Behavior**
```javascript
// ⚠️ Initial render: value === deferredValue
function Component() {
  const [value, setValue] = useState('initial')
  const deferredValue = useDeferredValue(value)
  
  console.log(value === deferredValue) // true on first render
  
  // After setValue:
  console.log(value === deferredValue) // false (lag)
  
  // After React finishes deferred update:
  console.log(value === deferredValue) // true again
}

// You can use this to show loading state:
const isStale = value !== deferredValue
return (
  <div style={{ opacity: isStale ? 0.5 : 1 }}>
    {/* Content */}
  </div>
)
```

---

## 12. useTransition 🔄

### Basic Concept
Marks state updates as low-priority transitions to keep UI responsive.

### Signature
```javascript
const [isPending, startTransition] = useTransition()
```

### What it Returns
- **Array with 2 elements:**
  1. `isPending`: Boolean indicating if transition is in progress
  2. `startTransition`: Function to wrap low-priority updates

### Parameters
- None

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Only Wraps State Updates**
```javascript
// ❌ WRONG - API call is not a state update
startTransition(() => {
  fetch('/api/data') // Does nothing!
})

// ❌ WRONG - DOM manipulation
startTransition(() => {
  document.title = 'New Title' // Not affected
})

// ✅ CORRECT - State updates only
startTransition(() => {
  setQuery(newQuery) // This is deferred
  setFilter(newFilter) // This too
})
```

#### 2. **Synchronous State Updates Only**
```javascript
// ❌ Async updates don't work
startTransition(async () => {
  const data = await fetchData()
  setData(data) // Not in transition! Too late
})

// ✅ Solution 1: Update state synchronously
startTransition(() => {
  setData(cachedData) // Immediate update
})
fetchData().then(data => setData(data)) // Later update

// ✅ Solution 2: Use isPending separately
const [isPending, startTransition] = useTransition()
const [isLoading, setIsLoading] = useState(false)

async function handleClick() {
  setIsLoading(true)
  const data = await fetchData()
  startTransition(() => {
    setData(data)
    setIsLoading(false)
  })
}
```

#### 3. **Difference from useDeferredValue**
```javascript
// useDeferredValue: Defer a VALUE
function Component({ value }) {
  const deferredValue = useDeferredValue(value)
  // Use deferredValue instead of value
}

// useTransition: Defer an UPDATE
function Component() {
  const [value, setValue] = useState('')
  const [isPending, startTransition] = useTransition()
  
  function handleChange(newValue) {
    startTransition(() => {
      setValue(newValue) // This update is deferred
    })
  }
}

// Rule: Use useTransition when you control the update
// Use useDeferredValue when you receive the value as prop
```

#### 4. **isPending UI Patterns**
```javascript
function Search() {
  const [query, setQuery] = useState('')
  const [isPending, startTransition] = useTransition()
  
  function handleChange(e) {
    const newQuery = e.target.value
    setQuery(newQuery) // Immediate (for input responsiveness)
    
    startTransition(() => {
      setSearchResults(newQuery) // Deferred (expensive update)
    })
  }
  
  return (
    <>
      <input value={query} onChange={handleChange} />
      
      {/* Pattern 1: Overlay spinner */}
      {isPending && <Spinner />}
      
      {/* Pattern 2: Opacity fade */}
      <div style={{ opacity: isPending ? 0.5 : 1 }}>
        <Results />
      </div>
      
      {/* Pattern 3: Disable interactions */}
      <button disabled={isPending}>Submit</button>
    </>
  )
}
```

#### 5. **Multiple Transitions**
```javascript
// ⚠️ New transition interrupts previous one
function Component() {
  const [isPending, startTransition] = useTransition()
  
  function slowUpdate1() {
    startTransition(() => {
      // Expensive operation 1
      for (let i = 0; i < 10000; i++) {
        setState(/* ... */)
      }
    })
  }
  
  function slowUpdate2() {
    startTransition(() => {
      // Expensive operation 2
      for (let i = 0; i < 10000; i++) {
        setState(/* ... */)
      }
    })
  }
  
  // If user calls slowUpdate2() while slowUpdate1() is pending:
  // React ABANDONS slowUpdate1 and starts slowUpdate2
  // This is good! User's latest input takes priority
}
```

---

## 13. useId 🔑

### Basic Concept
Generates unique IDs that are consistent across server and client.

### Signature
```javascript
const id = useId()
```

### What it Returns
- Unique string ID consistent across server/client

### Parameters
- None

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Not for List Keys!**
```javascript
// ❌ WRONG - Don't use for list keys
function List({ items }) {
  const id = useId()
  return items.map((item, index) => (
    <div key={`${id}-${index}`}>{item}</div>
  ))
}
// Problem: All items get same base ID, only index differs
// Keys should be stable and unique to the data

// ✅ CORRECT - Use data-based keys
function List({ items }) {
  return items.map(item => (
    <div key={item.id}>{item}</div>
  ))
}
```

#### 2. **For Accessibility Attributes**
```javascript
// ✅ Perfect use case
function FormField({ label, type }) {
  const id = useId()
  
  return (
    <>
      <label htmlFor={id}>{label}</label>
      <input id={id} type={type} />
    </>
  )
}

// Multiple instances work correctly:
<>
  <FormField label="Email" type="email" />
  {/* id: :r1: */}
  <FormField label="Password" type="password" />
  {/* id: :r2: */}
</>
```

#### 3. **SSR Consistency**
```javascript
// ⚠️ Math.random() causes hydration mismatch
function BadComponent() {
  const id = `field-${Math.random()}`
  // Server: field-0.123
  // Client: field-0.456
  // ❌ Mismatch!
  
  return <input id={id} />
}

// ✅ useId is consistent
function GoodComponent() {
  const id = useId()
  // Server: :r1:
  // Client: :r1:
  // ✅ Match!
  
  return <input id={id} />
}
```

#### 4. **Creating Multiple Related IDs**
```javascript
// ✅ Generate multiple IDs from same base
function FormField() {
  const id = useId()
  
  return (
    <div>
      <label htmlFor={id}>Name</label>
      <input id={id} aria-describedby={`${id}-hint`} />
      <div id={`${id}-hint`}>Enter your full name</div>
    </div>
  )
}

// All IDs are related and unique:
// :r1: (input)
// :r1:-hint (description)
```

#### 5. **Not Stable Across Renders**
```javascript
// ❌ ID changes if component unmounts and remounts
function Component() {
  const id = useId()
  // First mount: :r1:
  // Unmount then remount: :r5: (different!)
}

// ⚠️ Don't rely on ID persistence
// IDs are unique, not stable

// ✅ If you need persistence, use state:
const [id] = useState(() => crypto.randomUUID())
// OR use a data-based identifier
```

---

## 14. useSyncExternalStore 🔌

### Basic Concept
Subscribes to external stores (non-React state) safely.

### Signature
```javascript
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```

### What it Returns
- Current snapshot of the store

### Parameters
1. **subscribe**: Function that registers a callback and returns unsubscribe function
2. **getSnapshot**: Function that returns current store value
3. **getServerSnapshot** (optional): Function that returns initial value for SSR

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Subscribe Function Must Return Unsubscribe**
```javascript
// ❌ BAD - No cleanup
const count = useSyncExternalStore(
  (callback) => {
    externalStore.on('change', callback)
    // ❌ Missing return!
  },
  () => externalStore.getCount()
)

// ✅ GOOD - Returns cleanup
const count = useSyncExternalStore(
  (callback) => {
    externalStore.on('change', callback)
    return () => externalStore.off('change', callback)
  },
  () => externalStore.getCount()
)
```

#### 2. **getSnapshot Must Return Immutable Value**
```javascript
const externalStore = {
  state: { count: 0 },
  listeners: new Set(),
  
  getState() {
    return this.state // ❌ Returns same object reference
  },
  
  increment() {
    this.state.count++ // ❌ Mutates object
    this.listeners.forEach(cb => cb())
  }
}

// Component won't re-render! (same reference)
const state = useSyncExternalStore(
  (cb) => {
    externalStore.listeners.add(cb)
    return () => externalStore.listeners.delete(cb)
  },
  () => externalStore.getState() // Always same reference
)

// ✅ Solution: Return new object or primitive
getSnapshot() {
  return { ...this.state } // New object each time
  // OR
  return this.state.count // Primitive value
}
```

#### 3. **SSR getServerSnapshot Required**
```javascript
// ❌ Causes hydration mismatch
const count = useSyncExternalStore(
  subscribe,
  () => externalStore.getCount() // Returns 0 on client
  // Missing getServerSnapshot - returns undefined on server
)
// Server renders: 0
// Client hydrates: undefined initially
// ❌ Mismatch!

// ✅ Provide server snapshot
const count = useSyncExternalStore(
  subscribe,
  () => externalStore.getCount(),
  () => 0 // Server snapshot
)
```

#### 4. **Browser API Integration**
```javascript
// ✅ Subscribe to window size
function useWindowSize() {
  return useSyncExternalStore(
    (callback) => {
      window.addEventListener('resize', callback)
      return () => window.removeEventListener('resize', callback)
    },
    () => ({ width: window.innerWidth, height: window.innerHeight }),
    () => ({ width: 0, height: 0 }) // SSR fallback
  )
}

// ✅ Subscribe to online status
function useOnlineStatus() {
  return useSyncExternalStore(
    (callback) => {
      window.addEventListener('online', callback)
      window.addEventListener('offline', callback)
      return () => {
        window.removeEventListener('online', callback)
        window.removeEventListener('offline', callback)
      }
    },
    () => navigator.onLine,
    () => true // Assume online on server
  )
}
```

#### 5. **Performance Consideration**
```javascript
// ⚠️ getSnapshot called VERY frequently
// Must be fast!

// ❌ BAD - Expensive calculation
const value = useSyncExternalStore(
  subscribe,
  () => {
    // Called on every potential update check
    return expensiveCalculation(store.getState())
  }
)

// ✅ GOOD - Store pre-calculated value
class Store {
  constructor() {
    this.cachedValue = this.calculate()
  }
  
  update() {
    this.cachedValue = this.calculate() // Calculate once
    this.notify()
  }
  
  getSnapshot() {
    return this.cachedValue // Return cached
  }
}
```

---

## 15. Custom Hooks 🎨

### Basic Concept
Functions starting with "use" that can use other hooks.

### Signature
```javascript
function useCustomHook(params) {
  // Use built-in hooks
  return value
}
```

### What it Returns
- Anything you want (value, array, object, function)

### Parameters
- Any parameters you define

### ⚠️ 5 Tricky Concepts & Loopholes

#### 1. **Must Follow Hook Rules**
```javascript
// ❌ BAD - Conditional hook call
function useConditional(condition) {
  if (condition) {
    const [state, setState] = useState(0) // ❌ Conditional!
    return state
  }
  return null
}

// ✅ GOOD - Hook always called
function useConditional(condition) {
  const [state, setState] = useState(0)
  return condition ? state : null
}
```

#### 2. **Return Value Patterns**
```javascript
// Pattern 1: Single value
function useUser(id) {
  const [user, setUser] = useState(null)
  // ...
  return user
}

// Pattern 2: Array (like useState)
function useToggle(initial) {
  const [value, setValue] = useState(initial)
  const toggle = () => setValue(v => !v)
  return [value, toggle]
}
// Usage: const [isOpen, toggle] = useToggle(false)

// Pattern 3: Object
function useAsync(asyncFunction) {
  const [state, setState] = useState({ loading: false, data: null, error: null })
  // ...
  return { ...state, execute: run }
}
// Usage: const { data, loading, error, execute } = useAsync(fetchData)

// ⚠️ Choose based on:
// - Array: When order matters, similar to built-in hooks
// - Object: When you have many return values
// - Single value: When there's only one value
```

#### 3. **Sharing Hook Logic Doesn't Share State**
```javascript
function useCounter() {
  const [count, setCount] = useState(0)
  return [count, () => setCount(c => c + 1)]
}

function App() {
  const [count1, increment1] = useCounter()
  const [count2, increment2] = useCounter()
  
  increment1() // count1 = 1, count2 = 0
  // ⚠️ Each hook call has independent state!
}

// ✅ If you need shared state, use Context:
const CountContext = createContext()

function useSharedCounter() {
  return useContext(CountContext)
}
```

#### 4. **Async Hook Patterns**
```javascript
// ✅ Pattern 1: useEffect with cleanup
function useData(url) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
  
  useEffect(() => {
    let ignore = false
    
    fetch(url)
      .then(res => res.json())
      .then(data => {
        if (!ignore) {
          setData(data)
          setLoading(false)
        }
      })
    
    return () => { ignore = true }
  }, [url])
  
  return { data, loading }
}

// ✅ Pattern 2: Execute function
function useAsyncFunction() {
  const [state, setState] = useState({ loading: false })
  
  const execute = useCallback(async (asyncFn) => {
    setState({ loading: true })
    try {
      const result = await asyncFn()
      setState({ loading: false, data: result })
    } catch (error) {
      setState({ loading: false, error })
    }
  }, [])
  
  return { ...state, execute }
}
```

#### 5. **Custom Hook Dependencies**
```javascript
// ⚠️ Tricky: What should be in dependency array?

// ❌ BAD - Missing callback dependency
function useInterval(callback, delay) {
  useEffect(() => {
    const id = setInterval(callback, delay)
    return () => clearInterval(id)
  }, [delay]) // ❌ Missing callback!
}
// If callback changes, interval uses stale callback

// ✅ Solution 1: Add callback (interval restarts on change)
function useInterval(callback, delay) {
  useEffect(() => {
    const id = setInterval(callback, delay)
    return () => clearInterval(id)
  }, [callback, delay])
}

// ✅ Solution 2: Use ref (interval doesn't restart)
function useInterval(callback, delay) {
  const savedCallback = useRef(callback)
  
  useEffect(() => {
    savedCallback.current = callback
  }, [callback])
  
  useEffect(() => {
    const id = setInterval(() => savedCallback.current(), delay)
    return () => clearInterval(id)
  }, [delay])
}
```

---

## Summary Table 📊

| Hook | Primary Use Case | Common Pitfall |
|------|------------------|----------------|
| useState | Local component state | Batching / stale closures |
| useEffect | Side effects | Infinite loops / missing deps |
| useContext | Global state access | Unnecessary re-renders |
| useReducer | Complex state logic | Mutation instead of immutability |
| useCallback | Memoize functions | Over-optimization |
| useMemo | Memoize values | Premature optimization |
| useRef | Mutable values / DOM refs | Expecting re-render on change |
| useImperativeHandle | Expose child methods | Overuse / breaking encapsulation |
| useLayoutEffect | DOM measurements | Blocking paint / SSR issues |
| useDebugValue | Custom hook debugging | Using outside custom hooks |
| useDeferredValue | Defer non-urgent updates | Treating as debounce |
| useTransition | Mark updates as transitions | Using with async operations |
| useId | Generate unique IDs | Using for list keys |
| useSyncExternalStore | Subscribe to external stores | Not returning cleanup |
| Custom Hooks | Reusable logic | Not following hook rules |

---

## Advanced Patterns & Best Practices 🚀

### Pattern 1: Compound Hooks Pattern
```javascript
// ✅ Combine multiple hooks for complex behavior
function useFormField(initialValue) {
  const [value, setValue] = useState(initialValue)
  const [error, setError] = useState(null)
  const [touched, setTouched] = useState(false)
  
  const onChange = useCallback((e) => {
    setValue(e.target.value)
    setError(null)
  }, [])
  
  const onBlur = useCallback(() => {
    setTouched(true)
  }, [])
  
  const validate = useCallback((validator) => {
    if (touched) {
      const errorMsg = validator(value)
      setError(errorMsg)
      return !errorMsg
    }
    return true
  }, [value, touched])
  
  return {
    value,
    onChange,
    onBlur,
    error,
    touched,
    validate
  }
}

// Usage:
function Form() {
  const email = useFormField('')
  const password = useFormField('')
  
  const handleSubmit = () => {
    const isEmailValid = email.validate(val => 
      val.includes('@') ? null : 'Invalid email'
    )
    const isPasswordValid = password.validate(val =>
      val.length >= 8 ? null : 'Password too short'
    )
    
    if (isEmailValid && isPasswordValid) {
      // Submit
    }
  }
  
  return (
    <form>
      <input {...email} />
      {email.error && <span>{email.error}</span>}
      
      <input {...password} type="password" />
      {password.error && <span>{password.error}</span>}
      
      <button onClick={handleSubmit}>Submit</button>
    </form>
  )
}
```

### Pattern 2: State Machine with useReducer
```javascript
// ✅ Model complex state transitions
const stateMachine = {
  idle: {
    FETCH: 'loading'
  },
  loading: {
    SUCCESS: 'success',
    ERROR: 'error'
  },
  success: {
    FETCH: 'loading'
  },
  error: {
    RETRY: 'loading'
  }
}

function reducer(state, action) {
  const nextState = stateMachine[state.status]?.[action.type]
  
  if (!nextState) return state
  
  switch (action.type) {
    case 'FETCH':
      return { status: 'loading', data: null, error: null }
    case 'SUCCESS':
      return { status: 'success', data: action.data, error: null }
    case 'ERROR':
      return { status: 'error', data: null, error: action.error }
    case 'RETRY':
      return { status: 'loading', data: null, error: null }
    default:
      return state
  }
}

function useDataFetch(url) {
  const [state, dispatch] = useReducer(reducer, {
    status: 'idle',
    data: null,
    error: null
  })
  
  const fetch = useCallback(() => {
    dispatch({ type: 'FETCH' })
    
    fetchData(url)
      .then(data => dispatch({ type: 'SUCCESS', data }))
      .catch(error => dispatch({ type: 'ERROR', error }))
  }, [url])
  
  return { ...state, fetch, retry: () => dispatch({ type: 'RETRY' }) }
}
```

### Pattern 3: Lazy State Initialization with Factory
```javascript
// ✅ Expensive initial state computation
function useExpensiveState() {
  const [data, setData] = useState(() => {
    // This only runs once on mount
    const stored = localStorage.getItem('data')
    const parsed = stored ? JSON.parse(stored) : null
    return parsed || computeExpensiveDefault()
  })
  
  // Sync to localStorage
  useEffect(() => {
    localStorage.setItem('data', JSON.stringify(data))
  }, [data])
  
  return [data, setData]
}
```

### Pattern 4: Refs for Previous Values
```javascript
// ✅ Track previous props/state
function usePrevious(value) {
  const ref = useRef()
  
  useEffect(() => {
    ref.current = value
  }, [value])
  
  return ref.current
}

// Usage:
function Component({ count }) {
  const prevCount = usePrevious(count)
  
  useEffect(() => {
    if (prevCount !== undefined) {
      console.log(`Changed from ${prevCount} to ${count}`)
    }
  }, [count, prevCount])
}
```

### Pattern 5: Stable Callback with Latest State
```javascript
// ✅ Callback always uses latest state without re-creating
function useEvent(handler) {
  const handlerRef = useRef()
  
  useLayoutEffect(() => {
    handlerRef.current = handler
  })
  
  return useCallback((...args) => {
    const fn = handlerRef.current
    return fn(...args)
  }, [])
}

// Usage:
function Chat() {
  const [text, setText] = useState('')
  
  // ✅ sendMessage is stable but uses latest text
  const sendMessage = useEvent(() => {
    console.log('Sending:', text)
  })
  
  return (
    <>
      <input value={text} onChange={e => setText(e.target.value)} />
      <ChatRoom onSend={sendMessage} />
    </>
  )
}
```

---

## Common Anti-Patterns to Avoid ⚠️

### 1. Using useState for Derived State
```javascript
// ❌ BAD - Duplicate state
function Component({ items }) {
  const [filteredItems, setFilteredItems] = useState([])
  
  useEffect(() => {
    setFilteredItems(items.filter(item => item.active))
  }, [items])
  
  return <List items={filteredItems} />
}

// ✅ GOOD - Compute during render
function Component({ items }) {
  const filteredItems = items.filter(item => item.active)
  return <List items={filteredItems} />
}

// ✅ GOOD - Memoize if expensive
function Component({ items }) {
  const filteredItems = useMemo(
    () => items.filter(item => item.active),
    [items]
  )
  return <List items={filteredItems} />
}
```

### 2. Effect Dependency Hell
```javascript
// ❌ BAD - Too many dependencies
function Component() {
  const [a, setA] = useState(0)
  const [b, setB] = useState(0)
  const [c, setC] = useState(0)
  
  useEffect(() => {
    // Complex logic using a, b, c
  }, [a, b, c]) // Effect runs too often
}

// ✅ GOOD - Extract to reducer
function Component() {
  const [state, dispatch] = useReducer(reducer, initialState)
  
  useEffect(() => {
    // Logic using state
  }, [state]) // Single dependency
}
```

### 3. Unnecessary Effect for Event Handlers
```javascript
// ❌ BAD - Effect for user interaction
function Component() {
  const [count, setCount] = useState(0)
  
  useEffect(() => {
    const button = document.getElementById('btn')
    button.addEventListener('click', () => setCount(c => c + 1))
  }, [])
}

// ✅ GOOD - Direct event handler
function Component() {
  const [count, setCount] = useState(0)
  
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </button>
  )
}
```

### 4. Storing Props in State
```javascript
// ❌ BAD - Props in state (out of sync)
function Component({ initialValue }) {
  const [value, setValue] = useState(initialValue)
  // If initialValue changes, state doesn't update!
}

// ✅ GOOD - Use prop directly
function Component({ value, onChange }) {
  return <input value={value} onChange={onChange} />
}

// ✅ GOOD - Reset state when key changes
function Component({ userId }) {
  const [data, setData] = useState(null)
  
  useEffect(() => {
    fetchData(userId).then(setData)
  }, [userId])
}
// OR
<Component key={userId} userId={userId} />
```

### 5. Creating Functions Inside Render
```javascript
// ❌ BAD - New function every render
function Component() {
  return (
    <ExpensiveChild
      onClick={() => console.log('clicked')}
    />
  )
}

// ✅ GOOD - Memoized callback
function Component() {
  const handleClick = useCallback(() => {
    console.log('clicked')
  }, [])
  
  return <ExpensiveChild onClick={handleClick} />
}
```

---

## Performance Optimization Checklist ✅

### 1. **Identify Bottlenecks First**
```javascript
// Use React DevTools Profiler
// Measure before optimizing!

// ✅ Add custom labels
function Component() {
  // React DevTools will show this name
  return <div>Content</div>
}
```

### 2. **Memoization Strategy**
```javascript
// Priority order:
// 1. Fix expensive calculations first
// 2. Memoize components that render large lists
// 3. Memoize components with expensive children
// 4. Last resort: memoize everything

// ❌ Don't do this
const OverMemoized = memo(({ value }) => {
  const double = useMemo(() => value * 2, [value])
  const handleClick = useCallback(() => {}, [])
  return <div>{double}</div>
})

// ✅ Do this
function Optimized({ value }) {
  return <div>{value * 2}</div>
}
```

### 3. **List Rendering Optimization**
```javascript
// ✅ Key best practices
const List = ({ items }) => {
  return items.map(item => (
    <MemoizedItem
      key={item.id} // Stable, unique key
      item={item}
    />
  ))
}

const MemoizedItem = memo(({ item }) => {
  return <div>{item.name}</div>
})
```

### 4. **Context Optimization**
```javascript
// ❌ Single context with all data
const AppContext = createContext()

// ✅ Split contexts by update frequency
const ThemeContext = createContext() // Rarely changes
const UserContext = createContext()  // Changes sometimes
const NotificationsContext = createContext() // Changes often

// ✅ Use useMemo for provider value
function Provider({ children }) {
  const [state, setState] = useState(initialState)
  const value = useMemo(() => ({ state, setState }), [state])
  
  return <Context.Provider value={value}>{children}</Context.Provider>
}
```

### 5. **Bundle Size Optimization**
```javascript
// ✅ Code split large components
const HeavyComponent = lazy(() => import('./HeavyComponent'))

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <HeavyComponent />
    </Suspense>
  )
}
```

---

## Debugging Tips 🐛

### 1. **Use React DevTools**
- Install React Developer Tools browser extension
- Profiler tab shows component render times
- Components tab shows props, state, hooks

### 2. **Add Logging to Hooks**
```javascript
function useDebugValue(value, name = 'Value') {
  useEffect(() => {
    console.log(`${name} changed:`, value)
  }, [value, name])
}

// Usage:
const [count, setCount] = useState(0)
useDebugValue(count, 'Count')
```

### 3. **Why Did You Render**
```javascript
function useWhyDidYouUpdate(name, props) {
  const previousProps = useRef()
  
  useEffect(() => {
    if (previousProps.current) {
      const allKeys = Object.keys({ ...previousProps.current, ...props })
      const changedProps = {}
      
      allKeys.forEach(key => {
        if (previousProps.current[key] !== props[key]) {
          changedProps[key] = {
            from: previousProps.current[key],
            to: props[key]
          }
        }
      })
      
      if (Object.keys(changedProps).length) {
        console.log('[why-did-you-update]', name, changedProps)
      }
    }
    
    previousProps.current = props
  })
}

// Usage:
function Component(props) {
  useWhyDidYouUpdate('Component', props)
  return <div>...</div>
}
```

### 4. **Strict Mode for Double Rendering**
```javascript
// ✅ Wrap app in StrictMode during development
<React.StrictMode>
  <App />
</React.StrictMode>

// StrictMode:
// - Renders components twice to detect side effects
// - Runs effects twice to ensure cleanup works
// - Only in development mode
```

### 5. **ESLint Plugin**
```json
// .eslintrc.json
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

---

## TypeScript Tips for Hooks 📘

### 1. **Generic Hooks**
```typescript
// ✅ Type-safe generic hook
function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key)
      return item ? JSON.parse(item) : initialValue
    } catch (error) {
      return initialValue
    }
  })
  
  const setValue = (value: T | ((val: T) => T)) => {
    const valueToStore = value instanceof Function ? value(storedValue) : value
    setStoredValue(valueToStore)
    window.localStorage.setItem(key, JSON.stringify(valueToStore))
  }
  
  return [storedValue, setValue] as const
}

// Usage:
const [user, setUser] = useLocalStorage<User>('user', { name: 'John' })
```

### 2. **Ref Types**
```typescript
// DOM element ref
const inputRef = useRef<HTMLInputElement>(null)

// Mutable value ref
const countRef = useRef<number>(0)

// Timer ref
const timerRef = useRef<NodeJS.Timeout | null>(null)
```

### 3. **Event Handler Types**
```typescript
function Component() {
  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    console.log(e.currentTarget)
  }
  
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value)
  }
  
  return (
    <>
      <button onClick={handleClick}>Click</button>
      <input onChange={handleChange} />
    </>
  )
}
```

### 4. **Context Types**
```typescript
interface ThemeContextType {
  theme: 'light' | 'dark'
  setTheme: (theme: 'light' | 'dark') => void
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined)

function useTheme() {
  const context = useContext(ThemeContext)
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider')
  }
  return context
}
```

---

## Testing Hooks 🧪

### 1. **Testing Library**
```javascript
import { renderHook, act } from '@testing-library/react'

test('useCounter increment', () => {
  const { result } = renderHook(() => useCounter())
  
  act(() => {
    result.current.increment()
  })
  
  expect(result.current.count).toBe(1)
})
```

### 2. **Testing Effects**
```javascript
test('useEffect runs on mount', () => {
  const callback = jest.fn()
  
  renderHook(() => {
    useEffect(() => {
      callback()
    }, [])
  })
  
  expect(callback).toHaveBeenCalledTimes(1)
})
```

### 3. **Testing Async Hooks**
```javascript
test('useAsync fetches data', async () => {
  const { result, waitForNextUpdate } = renderHook(() => 
    useAsync(() => fetchData())
  )
  
  expect(result.current.loading).toBe(true)
  
  await waitForNextUpdate()
  
  expect(result.current.loading).toBe(false)
  expect(result.current.data).toEqual(expectedData)
})
```

---

## Quick Reference Card 🎴

### Hook Selection Guide
```
Need to...                        → Use...
─────────────────────────────────────────────────────
Store component state             → useState
Perform side effects              → useEffect
Access context                    → useContext
Complex state logic               → useReducer
Memoize expensive calculations    → useMemo
Memoize callback functions        → useCallback
Store mutable value               → useRef
Access DOM elements               → useRef
Sync with external store          → useSyncExternalStore
Generate unique IDs               → useId
Defer non-urgent updates          → useDeferredValue
Mark updates as transitions       → useTransition
Measure/mutate DOM before paint   → useLayoutEffect
Customize ref behavior            → useImperativeHandle
Debug custom hooks                → useDebugValue
```

### Common Hook Combinations
```javascript
// Form handling
useState + useCallback + useEffect

// Data fetching
useState + useEffect + cleanup

// Optimized child components
useMemo + useCallback + memo

// Complex state management
useReducer + useContext

// External store sync
useSyncExternalStore + useCallback

// Performance optimization
useMemo + memo + useCallback
```

---

## Final Notes 📝

### Rules of Hooks (MUST FOLLOW)
1. ✅ Only call hooks at top level (not in loops, conditions, nested functions)
2. ✅ Only call hooks from React functions (components or custom hooks)
3. ✅ Custom hooks must start with "use"
4. ✅ Dependencies should be exhaustive (include all used values)

### When to Extract Custom Hooks
- Logic used in 2+ components
- Complex hook combinations
- Stateful logic that's testable in isolation
- Clear, reusable abstraction

### Performance Philosophy
> "Premature optimization is the root of all evil"
> - Donald Knuth

1. **Make it work** (functionality first)
2. **Make it right** (clean code)
3. **Make it fast** (optimize bottlenecks)

Measure first, optimize second!

---

## Additional Resources 📚

- **Official Docs**: https://react.dev/reference/react
- **React Hooks RFC**: https://github.com/reactjs/rfcs/pull/68
- **React DevTools**: Browser extension for debugging
- **ESLint Plugin**: `eslint-plugin-react-hooks`
- **TypeScript**: `@types/react` for type definitions

---

*Keep this guide handy and refer to specific hook sections when you encounter issues. Happy coding! 🚀*
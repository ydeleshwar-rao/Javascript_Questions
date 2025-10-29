🎯 Machine Coding Round Strategy
Interview mein ye cheezein dekhi jaati hai:

Clean Code - Readable aur organized
Component Structure - Proper breakdown
State Management - useState, useEffect sahi use
UI/UX - Basic styling acchi ho
Time Management - 30-45 min mein complete


📚 Level 1: Basic Concepts (Yahan se shuru kar)
Question 1: Counter App (Sabse Easy)
Concept: useState hook
jsx// YE SAMAJH - useState hook value store karta hai
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
Concept Samjho:

useState(0) - Initial value 0 hai
count - Current value
setCount - Value change karne ke liye function


Question 2: Todo List (Must Practice)
Ye question bohot common hai interviews mein GitHubMedium
Concept: Array manipulation, map function
jsximport { useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');

  const addTodo = () => {
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input, done: false }]);
      setInput('');
    }
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  const toggleTodo = (id) => {
    setTodos(todos.map(todo => 
      todo.id === id ? { ...todo, done: !todo.done } : todo
    ));
  };

  return (
    <div>
      <input 
        value={input} 
        onChange={(e) => setInput(e.target.value)}
        placeholder="Enter task"
      />
      <button onClick={addTodo}>Add</button>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input 
              type="checkbox" 
              checked={todo.done}
              onChange={() => toggleTodo(todo.id)}
            />
            <span style={{ textDecoration: todo.done ? 'line-through' : 'none' }}>
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
Key Concepts:

Spread operator (...todos) - Existing array ko copy kar raha hai
filter() - Delete ke liye
map() - Update ke liye
key prop - React ko batata hai konsa item unique hai


📚 Level 2: API Integration (India Startups bohot puchte hain)
Question 3: Search/Filter Users
Ye functional requirement common hai - search aur filter functionality Frontendgeek
Concept: API call, useEffect, filter
jsximport { useState, useEffect } from 'react';

function UserSearch() {
  const [users, setUsers] = useState([]);
  const [search, setSearch] = useState('');
  const [loading, setLoading] = useState(true);

  // API call - Component load hote hi
  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => res.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      });
  }, []); // Empty array = sirf ek baar chalega

  // Filter logic
  const filteredUsers = users.filter(user =>
    user.name.toLowerCase().includes(search.toLowerCase())
  );

  if (loading) return <div>Loading...</div>;

  return (
    <div>
      <input
        type="text"
        placeholder="Search users..."
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />
      
      <ul>
        {filteredUsers.map(user => (
          <li key={user.id}>
            {user.name} - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}
Concepts:

useEffect - Side effects ke liye (API calls, timers)
Dependency Array [] - Kab chalega wo decide karta hai
Loading state - User experience ke liye


Question 4: Pagination (Important!)
Pagination aur infinite scroll common requirements hai Frontendgeek
Concept: Slice array, page calculation
jsximport { useState } from 'react';

function Pagination() {
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 5;
  
  // Dummy data
  const items = Array.from({ length: 50 }, (_, i) => `Item ${i + 1}`);
  
  // Calculate
  const totalPages = Math.ceil(items.length / itemsPerPage);
  const startIndex = (currentPage - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  const currentItems = items.slice(startIndex, endIndex);

  return (
    <div>
      <ul>
        {currentItems.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
      
      <div>
        <button 
          onClick={() => setCurrentPage(prev => Math.max(1, prev - 1))}
          disabled={currentPage === 1}
        >
          Previous
        </button>
        
        <span>Page {currentPage} of {totalPages}</span>
        
        <button 
          onClick={() => setCurrentPage(prev => Math.min(totalPages, prev + 1))}
          disabled={currentPage === totalPages}
        >
          Next
        </button>
      </div>
    </div>
  );
}
Math samjho:

slice(start, end) - Array ka part nikalta hai
Math.ceil() - Round up karta hai
disabled - Button ko disable karna


📚 Level 3: Startup Favorites (Ye pakka puchte hain!)
Question 5: Accordion (Collapsible List)
Nested tree view with expand/collapse common requirement hai Frontendgeek
Concept: Toggle state, conditional rendering
jsximport { useState } from 'react';

function Accordion() {
  const [openIndex, setOpenIndex] = useState(null);

  const data = [
    { title: 'Section 1', content: 'Content for section 1' },
    { title: 'Section 2', content: 'Content for section 2' },
    { title: 'Section 3', content: 'Content for section 3' }
  ];

  const toggle = (index) => {
    setOpenIndex(openIndex === index ? null : index);
  };

  return (
    <div>
      {data.map((item, index) => (
        <div key={index} style={{ border: '1px solid #ccc', margin: '10px' }}>
          <div 
            onClick={() => toggle(index)}
            style={{ padding: '10px', cursor: 'pointer', background: '#f0f0f0' }}
          >
            {item.title} {openIndex === index ? '▲' : '▼'}
          </div>
          
          {openIndex === index && (
            <div style={{ padding: '10px' }}>
              {item.content}
            </div>
          )}
        </div>
      ))}
    </div>
  );
}
Concept:

Conditional rendering - && operator use karke
Ternary operator - ? : for icon toggle


Question 6: Debouncing in Search
Concept: Performance optimization
jsximport { useState, useEffect } from 'react';

function DebouncedSearch() {
  const [search, setSearch] = useState('');
  const [debouncedSearch, setDebouncedSearch] = useState('');

  // Debounce logic
  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedSearch(search);
    }, 500); // 500ms delay

    return () => clearTimeout(timer); // Cleanup
  }, [search]);

  // API call sirf debounced value change hone par
  useEffect(() => {
    if (debouncedSearch) {
      console.log('API call with:', debouncedSearch);
      // Yahan API call karo
    }
  }, [debouncedSearch]);

  return (
    <input
      type="text"
      value={search}
      onChange={(e) => setSearch(e.target.value)}
      placeholder="Search (debounced)..."
    />
  );
}
Why Debouncing?

Har keystroke pe API call nahi hoga
500ms ruk kar hi call hoga
Performance improve hogi


Question 7: Modal/Popup
Concept: Conditional rendering, event handling
jsximport { useState } from 'react';

function Modal() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsOpen(true)}>Open Modal</button>

      {isOpen && (
        <div style={{
          position: 'fixed',
          top: 0,
          left: 0,
          right: 0,
          bottom: 0,
          background: 'rgba(0,0,0,0.5)',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center'
        }}>
          <div style={{
            background: 'white',
            padding: '20px',
            borderRadius: '8px',
            minWidth: '300px'
          }}>
            <h2>Modal Title</h2>
            <p>Modal content here...</p>
            <button onClick={() => setIsOpen(false)}>Close</button>
          </div>
        </div>
      )}
    </div>
  );
}

🎯 Top 10 Questions India Startups Puchte Hain
Based on search results, ye questions most common hain:

✅ Todo List (Must do!)
✅ Search with Filter/Debounce
✅ Pagination
✅ Accordion/Collapsible
Form Validation
Infinite Scroll
Star Rating Component
Image Gallery/Carousel
Grid with Click Animation (Startup favorite - color change on click) Medium
Weather App (API integration)


💡 Interview Tips (Bohot Important!)
Practice Strategy:

Pehle concept samjho - Blindly code mat karo
Hardcode se shuru karo - Pehle static data, phir dynamic
Console.log use karo - Debug karne ke liye
Small components banao - Ek bada component nahi

Interview Mein:

Requirements clarify karo - "Should I add styling?" "Any edge cases?"
Loud think karo - Jo soch rahe ho bol do
Start simple - Pehle basic, phir features add karo
Time management - 30 min mein basic working chahiye

Common Mistakes (Avoid karo!):

Complex se shuru karna ❌
Styling mein zyada time waste ❌
Error handling na karna ❌
Key props bhoolna (warnings aayengi) ❌


Bhai, agar tu chahta hai toh main tere liye:

Practice ke liye aur detailed examples bana sakta hoon
Specific question ka complete solution dikha sakta hoon
Common interview patterns aur tricks bata sakta hoon

Bol kahan se shuru karein? Konsa question pehle practice karna hai? 🚀🎯 Machine Coding Round Strategy
Interview mein ye cheezein dekhi jaati hai:

Clean Code - Readable aur organized
Component Structure - Proper breakdown
State Management - useState, useEffect sahi use
UI/UX - Basic styling acchi ho
Time Management - 30-45 min mein complete


📚 Level 1: Basic Concepts (Yahan se shuru kar)
Question 1: Counter App (Sabse Easy)
Concept: useState hook
jsx// YE SAMAJH - useState hook value store karta hai
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
Concept Samjho:

useState(0) - Initial value 0 hai
count - Current value
setCount - Value change karne ke liye function


Question 2: Todo List (Must Practice)
Ye question bohot common hai interviews mein GitHubMedium
Concept: Array manipulation, map function
jsximport { useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');

  const addTodo = () => {
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input, done: false }]);
      setInput('');
    }
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  const toggleTodo = (id) => {
    setTodos(todos.map(todo => 
      todo.id === id ? { ...todo, done: !todo.done } : todo
    ));
  };

  return (
    <div>
      <input 
        value={input} 
        onChange={(e) => setInput(e.target.value)}
        placeholder="Enter task"
      />
      <button onClick={addTodo}>Add</button>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input 
              type="checkbox" 
              checked={todo.done}
              onChange={() => toggleTodo(todo.id)}
            />
            <span style={{ textDecoration: todo.done ? 'line-through' : 'none' }}>
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
Key Concepts:

Spread operator (...todos) - Existing array ko copy kar raha hai
filter() - Delete ke liye
map() - Update ke liye
key prop - React ko batata hai konsa item unique hai


📚 Level 2: API Integration (India Startups bohot puchte hain)
Question 3: Search/Filter Users
Ye functional requirement common hai - search aur filter functionality Frontendgeek
Concept: API call, useEffect, filter
jsximport { useState, useEffect } from 'react';

function UserSearch() {
  const [users, setUsers] = useState([]);
  const [search, setSearch] = useState('');
  const [loading, setLoading] = useState(true);

  // API call - Component load hote hi
  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => res.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      });
  }, []); // Empty array = sirf ek baar chalega

  // Filter logic
  const filteredUsers = users.filter(user =>
    user.name.toLowerCase().includes(search.toLowerCase())
  );

  if (loading) return <div>Loading...</div>;

  return (
    <div>
      <input
        type="text"
        placeholder="Search users..."
        value={search}
        onChange={(e) => setSearch(e.target.value)}
      />
      
      <ul>
        {filteredUsers.map(user => (
          <li key={user.id}>
            {user.name} - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}
Concepts:

useEffect - Side effects ke liye (API calls, timers)
Dependency Array [] - Kab chalega wo decide karta hai
Loading state - User experience ke liye


Question 4: Pagination (Important!)
Pagination aur infinite scroll common requirements hai Frontendgeek
Concept: Slice array, page calculation
jsximport { useState } from 'react';

function Pagination() {
  const [currentPage, setCurrentPage] = useState(1);
  const itemsPerPage = 5;
  
  // Dummy data
  const items = Array.from({ length: 50 }, (_, i) => `Item ${i + 1}`);
  
  // Calculate
  const totalPages = Math.ceil(items.length / itemsPerPage);
  const startIndex = (currentPage - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;
  const currentItems = items.slice(startIndex, endIndex);

  return (
    <div>
      <ul>
        {currentItems.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
      
      <div>
        <button 
          onClick={() => setCurrentPage(prev => Math.max(1, prev - 1))}
          disabled={currentPage === 1}
        >
          Previous
        </button>
        
        <span>Page {currentPage} of {totalPages}</span>
        
        <button 
          onClick={() => setCurrentPage(prev => Math.min(totalPages, prev + 1))}
          disabled={currentPage === totalPages}
        >
          Next
        </button>
      </div>
    </div>
  );
}
Math samjho:

slice(start, end) - Array ka part nikalta hai
Math.ceil() - Round up karta hai
disabled - Button ko disable karna


📚 Level 3: Startup Favorites (Ye pakka puchte hain!)
Question 5: Accordion (Collapsible List)
Nested tree view with expand/collapse common requirement hai Frontendgeek
Concept: Toggle state, conditional rendering
jsximport { useState } from 'react';

function Accordion() {
  const [openIndex, setOpenIndex] = useState(null);

  const data = [
    { title: 'Section 1', content: 'Content for section 1' },
    { title: 'Section 2', content: 'Content for section 2' },
    { title: 'Section 3', content: 'Content for section 3' }
  ];

  const toggle = (index) => {
    setOpenIndex(openIndex === index ? null : index);
  };

  return (
    <div>
      {data.map((item, index) => (
        <div key={index} style={{ border: '1px solid #ccc', margin: '10px' }}>
          <div 
            onClick={() => toggle(index)}
            style={{ padding: '10px', cursor: 'pointer', background: '#f0f0f0' }}
          >
            {item.title} {openIndex === index ? '▲' : '▼'}
          </div>
          
          {openIndex === index && (
            <div style={{ padding: '10px' }}>
              {item.content}
            </div>
          )}
        </div>
      ))}
    </div>
  );
}
Concept:

Conditional rendering - && operator use karke
Ternary operator - ? : for icon toggle


Question 6: Debouncing in Search
Concept: Performance optimization
jsximport { useState, useEffect } from 'react';

function DebouncedSearch() {
  const [search, setSearch] = useState('');
  const [debouncedSearch, setDebouncedSearch] = useState('');

  // Debounce logic
  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedSearch(search);
    }, 500); // 500ms delay

    return () => clearTimeout(timer); // Cleanup
  }, [search]);

  // API call sirf debounced value change hone par
  useEffect(() => {
    if (debouncedSearch) {
      console.log('API call with:', debouncedSearch);
      // Yahan API call karo
    }
  }, [debouncedSearch]);

  return (
    <input
      type="text"
      value={search}
      onChange={(e) => setSearch(e.target.value)}
      placeholder="Search (debounced)..."
    />
  );
}
Why Debouncing?

Har keystroke pe API call nahi hoga
500ms ruk kar hi call hoga
Performance improve hogi


Question 7: Modal/Popup
Concept: Conditional rendering, event handling
jsximport { useState } from 'react';

function Modal() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsOpen(true)}>Open Modal</button>

      {isOpen && (
        <div style={{
          position: 'fixed',
          top: 0,
          left: 0,
          right: 0,
          bottom: 0,
          background: 'rgba(0,0,0,0.5)',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center'
        }}>
          <div style={{
            background: 'white',
            padding: '20px',
            borderRadius: '8px',
            minWidth: '300px'
          }}>
            <h2>Modal Title</h2>
            <p>Modal content here...</p>
            <button onClick={() => setIsOpen(false)}>Close</button>
          </div>
        </div>
      )}
    </div>
  );
}

🎯 Top 10 Questions India Startups Puchte Hain
Based on search results, ye questions most common hain:

✅ Todo List (Must do!)
✅ Search with Filter/Debounce
✅ Pagination
✅ Accordion/Collapsible
Form Validation
Infinite Scroll
Star Rating Component
Image Gallery/Carousel
Grid with Click Animation (Startup favorite - color change on click) Medium
Weather App (API integration)


💡 Interview Tips (Bohot Important!)
Practice Strategy:

Pehle concept samjho - Blindly code mat karo
Hardcode se shuru karo - Pehle static data, phir dynamic
Console.log use karo - Debug karne ke liye
Small components banao - Ek bada component nahi

Interview Mein:

Requirements clarify karo - "Should I add styling?" "Any edge cases?"
Loud think karo - Jo soch rahe ho bol do
Start simple - Pehle basic, phir features add karo
Time management - 30 min mein basic working chahiye

Common Mistakes (Avoid karo!):

Complex se shuru karna ❌
Styling mein zyada time waste ❌
Error handling na karna ❌
Key props bhoolna (warnings aayengi) ❌


Bhai, agar tu chahta hai toh main tere liye:

Practice ke liye aur detailed examples bana sakta hoon
Specific question ka complete solution dikha sakta hoon
Common interview patterns aur tricks bata sakta hoon

Bol kahan se shuru karein? Konsa question pehle practice karna hai? 🚀
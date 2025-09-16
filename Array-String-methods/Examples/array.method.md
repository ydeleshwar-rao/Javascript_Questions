# 📚 Complete JavaScript Array Methods Reference Guide

## Quick Reference
### Array Methods Overview:
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
- **length** - Array size property
- **shift()** - Remove first element
- **unshift()** - Add element to beginning

---

## 1. map() - Transform Each Element

**Description**
Creates a new array by transforming each element with a provided function. Never mutates the original array.

**Syntax**
```javascript
array.map(function(currentValue, index, arr), thisValue)
```

### 5 Advanced Examples:

#### Example 1: E-commerce Product Price Calculator
```javascript
const products = [
  { name: 'Laptop', price: 50000, discount: 10 },
  { name: 'Phone', price: 30000, discount: 15 },
  { name: 'Tablet', price: 20000, discount: 5 }
];

// Calculate final prices after discount
const finalPrices = products.map(product => ({
  ...product,
  finalPrice: product.price - (product.price * product.discount / 100),
  savings: product.price * product.discount / 100
}));

console.log(finalPrices);
// Output: Array with original data + finalPrice and savings
```

#### Example 2: Student Grade Analysis
```javascript
const students = [
  { name: 'Ahmed', marks: [85, 92, 78, 88] },
  { name: 'Sara', marks: [95, 87, 91, 94] },
  { name: 'Ali', marks: [76, 82, 85, 79] }
];

// Calculate average and grade for each student
const studentResults = students.map(student => {
  const average = student.marks.reduce((sum, mark) => sum + mark, 0) / student.marks.length;
  return {
    name: student.name,
    average: Math.round(average),
    grade: average >= 90 ? 'A' : average >= 80 ? 'B' : average >= 70 ? 'C' : 'D',
    totalMarks: student.marks.reduce((sum, mark) => sum + mark, 0)
  };
});

console.log(studentResults);
```

#### Example 3: API Data Transformation
```javascript
const apiUsers = [
  { id: 1, first_name: 'John', last_name: 'Doe', email: 'john@email.com', is_active: 1 },
  { id: 2, first_name: 'Jane', last_name: 'Smith', email: 'jane@email.com', is_active: 0 }
];

// Transform API response to frontend format
const frontendUsers = apiUsers.map((user, index) => ({
  userId: user.id,
  fullName: `${user.first_name} ${user.last_name}`,
  email: user.email,
  status: user.is_active ? 'Active' : 'Inactive',
  displayOrder: index + 1,
  initials: user.first_name[0] + user.last_name[0]
}));

console.log(frontendUsers);
```

#### Example 4: Complex Nested Data Processing
```javascript
const orders = [
  { orderId: 'ORD001', items: [{ name: 'Book', qty: 2, price: 500 }] },
  { orderId: 'ORD002', items: [{ name: 'Pen', qty: 10, price: 50 }, { name: 'Notebook', qty: 3, price: 100 }] }
];

// Process orders with item calculations
const processedOrders = orders.map(order => ({
  ...order,
  totalItems: order.items.reduce((sum, item) => sum + item.qty, 0),
  totalAmount: order.items.reduce((sum, item) => sum + (item.qty * item.price), 0),
  itemDetails: order.items.map(item => `${item.name} x${item.qty}`)
}));

console.log(processedOrders);
```

#### Example 5: Form Data Validation and Formatting
```javascript
const formInputs = [
  { field: 'name', value: '  john doe  ', required: true },
  { field: 'email', value: 'JOHN@EMAIL.COM', required: true },
  { field: 'phone', value: '03001234567', required: false }
];

// Clean and validate form data
const cleanedData = formInputs.map(input => ({
  field: input.field,
  originalValue: input.value,
  cleanValue: input.value.trim().toLowerCase(),
  isValid: input.required ? input.value.trim().length > 0 : true,
  charCount: input.value.trim().length,
  formatted: input.field === 'phone' ? input.value.replace(/(\d{4})(\d{7})/, '$1-$2') : input.value.trim()
}));

console.log(cleanedData);
```

---

## 2. filter() - Select Elements Based on Condition

**Description**
Creates a new array with elements that pass a test function. Perfect for data filtering and search functionality.

**Syntax**
```javascript
array.filter(function(currentValue, index, arr), thisValue)
```

### 5 Advanced Examples:

#### Example 1: Advanced E-commerce Filtering
```javascript
const products = [
  { name: 'Gaming Laptop', price: 80000, category: 'Electronics', rating: 4.5, inStock: true },
  { name: 'Office Chair', price: 15000, category: 'Furniture', rating: 4.2, inStock: false },
  { name: 'Smartphone', price: 45000, category: 'Electronics', rating: 4.7, inStock: true },
  { name: 'Desk', price: 12000, category: 'Furniture', rating: 3.9, inStock: true }
];

// Multi-criteria filtering
const premiumElectronics = products.filter(product => 
  product.category === 'Electronics' && 
  product.price > 40000 && 
  product.rating >= 4.5 && 
  product.inStock
);

// Filter with complex conditions
const affordableQualityItems = products.filter(product => {
  const isPriceGood = product.price >= 10000 && product.price <= 50000;
  const isQualityGood = product.rating >= 4.0;
  return isPriceGood && isQualityGood && product.inStock;
});

console.log(premiumElectronics);
console.log(affordableQualityItems);
```

#### Example 2: User Permission & Role Filtering
```javascript
const users = [
  { id: 1, name: 'Admin', roles: ['admin', 'user'], permissions: ['read', 'write', 'delete'], lastLogin: '2025-01-15' },
  { id: 2, name: 'Editor', roles: ['editor'], permissions: ['read', 'write'], lastLogin: '2025-01-10' },
  { id: 3, name: 'Viewer', roles: ['user'], permissions: ['read'], lastLogin: '2024-12-20' }
];

// Filter active admins with delete permission
const activeAdmins = users.filter(user => {
  const hasAdminRole = user.roles.includes('admin');
  const hasDeletePermission = user.permissions.includes('delete');
  const isRecentlyActive = new Date(user.lastLogin) > new Date('2025-01-01');
  return hasAdminRole && hasDeletePermission && isRecentlyActive;
});

// Filter users who can edit content
const editors = users.filter(user => 
  user.permissions.includes('write') && 
  (user.roles.includes('admin') || user.roles.includes('editor'))
);

console.log(activeAdmins);
console.log(editors);
```

#### Example 3: Search and Filter Functionality
```javascript
const employees = [
  { name: 'Ahmed Khan', department: 'IT', salary: 75000, skills: ['JavaScript', 'React', 'Node.js'], experience: 5 },
  { name: 'Sara Ali', department: 'Marketing', salary: 60000, skills: ['SEO', 'Content Writing'], experience: 3 },
  { name: 'Ali Hassan', department: 'IT', salary: 90000, skills: ['Python', 'Django', 'JavaScript'], experience: 7 }
];

function searchEmployees(searchTerm, filters = {}) {
  return employees.filter(employee => {
    // Text search
    const matchesSearch = searchTerm ? 
      employee.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
      employee.department.toLowerCase().includes(searchTerm.toLowerCase()) ||
      employee.skills.some(skill => skill.toLowerCase().includes(searchTerm.toLowerCase()))
      : true;

    // Department filter
    const matchesDepartment = filters.department ? 
      employee.department === filters.department : true;

    // Salary range filter
    const matchesSalary = filters.minSalary ? 
      employee.salary >= filters.minSalary : true;

    // Experience filter
    const matchesExperience = filters.minExperience ? 
      employee.experience >= filters.minExperience : true;

    return matchesSearch && matchesDepartment && matchesSalary && matchesExperience;
  });
}

// Usage examples
console.log(searchEmployees('javascript'));
console.log(searchEmployees('', { department: 'IT', minSalary: 80000 }));
```

#### Example 4: Date and Time Filtering
```javascript
const orders = [
  { id: 'ORD001', date: '2025-01-10', status: 'completed', amount: 5000 },
  { id: 'ORD002', date: '2025-01-15', status: 'pending', amount: 3000 },
  { id: 'ORD003', date: '2025-01-08', status: 'cancelled', amount: 2000 },
  { id: 'ORD004', date: '2025-01-12', status: 'completed', amount: 7000 }
];

// Filter orders from last 7 days
const recentOrders = orders.filter(order => {
  const orderDate = new Date(order.date);
  const weekAgo = new Date();
  weekAgo.setDate(weekAgo.getDate() - 7);
  return orderDate >= weekAgo;
});

// Filter high-value completed orders
const highValueCompleted = orders.filter(order => 
  order.status === 'completed' && 
  order.amount > 5000
);

// Complex date filtering with multiple conditions
const thisMonthPendingOrders = orders.filter(order => {
  const orderDate = new Date(order.date);
  const now = new Date();
  const isThisMonth = orderDate.getMonth() === now.getMonth() && 
                      orderDate.getFullYear() === now.getFullYear();
  return isThisMonth && order.status === 'pending';
});

console.log(recentOrders);
console.log(highValueCompleted);
```

#### Example 5: Nested Array Filtering with Complex Logic
```javascript
const courses = [
  { 
    title: 'React Development', 
    instructor: 'John Doe', 
    students: [
      { name: 'Ahmed', grade: 85, attendance: 90 },
      { name: 'Sara', grade: 92, attendance: 95 }
    ],
    duration: 40,
    level: 'intermediate'
  },
  {
    title: 'Python Basics',
    instructor: 'Jane Smith',
    students: [
      { name: 'Ali', grade: 78, attendance: 80 },
      { name: 'Fatima', grade: 88, attendance: 92 }
    ],
    duration: 30,
    level: 'beginner'
  }
];

// Filter courses with high-performing students
const qualityCourses = courses.filter(course => {
  const avgGrade = course.students.reduce((sum, student) => sum + student.grade, 0) / course.students.length;
  const avgAttendance = course.students.reduce((sum, student) => sum + student.attendance, 0) / course.students.length;
  const hasGoodMetrics = avgGrade >= 80 && avgAttendance >= 85;
  const isSuitableLength = course.duration >= 30 && course.duration <= 50;
  
  return hasGoodMetrics && isSuitableLength;
});

console.log(qualityCourses);
```

---

## 3. reduce() - Aggregate to Single Value

**Description**
Executes a reducer function on each element, resulting in a single output value. Most powerful array method for data aggregation.

**Syntax**
```javascript
array.reduce(function(accumulator, currentValue, currentIndex, arr), initialValue)
```

### 5 Advanced Examples:

#### Example 1: Financial Data Analysis
```javascript
const transactions = [
  { type: 'income', amount: 50000, category: 'salary', date: '2025-01-01' },
  { type: 'expense', amount: 15000, category: 'rent', date: '2025-01-02' },
  { type: 'expense', amount: 5000, category: 'food', date: '2025-01-03' },
  { type: 'income', amount: 10000, category: 'freelance', date: '2025-01-05' }
];

// Create comprehensive financial summary
const financialSummary = transactions.reduce((summary, transaction) => {
  if (transaction.type === 'income') {
    summary.totalIncome += transaction.amount;
    summary.incomeByCategory[transaction.category] = 
      (summary.incomeByCategory[transaction.category] || 0) + transaction.amount;
  } else {
    summary.totalExpense += transaction.amount;
    summary.expenseByCategory[transaction.category] = 
      (summary.expenseByCategory[transaction.category] || 0) + transaction.amount;
  }
  
  summary.netAmount = summary.totalIncome - summary.totalExpense;
  summary.transactionCount++;
  
  return summary;
}, {
  totalIncome: 0,
  totalExpense: 0,
  netAmount: 0,
  transactionCount: 0,
  incomeByCategory: {},
  expenseByCategory: {}
});

console.log(financialSummary);
```

#### Example 2: Inventory Management System
```javascript
const inventory = [
  { product: 'Laptop', quantity: 5, price: 50000, category: 'Electronics' },
  { product: 'Mouse', quantity: 20, price: 1500, category: 'Electronics' },
  { product: 'Chair', quantity: 10, price: 8000, category: 'Furniture' },
  { product: 'Desk', quantity: 7, price: 15000, category: 'Furniture' }
];

// Create detailed inventory analysis
const inventoryAnalysis = inventory.reduce((analysis, item) => {
  const itemValue = item.quantity * item.price;
  
  // Update totals
  analysis.totalItems += item.quantity;
  analysis.totalValue += itemValue;
  
  // Category-wise analysis
  if (!analysis.categories[item.category]) {
    analysis.categories[item.category] = {
      items: 0,
      quantity: 0,
      value: 0,
      products: []
    };
  }
  
  analysis.categories[item.category].items++;
  analysis.categories[item.category].quantity += item.quantity;
  analysis.categories[item.category].value += itemValue;
  analysis.categories[item.category].products.push(item.product);
  
  // Track high/low value items
  if (itemValue > 50000) {
    analysis.highValueItems.push(item);
  }
  
  if (item.quantity < 10) {
    analysis.lowStockItems.push(item);
  }
  
  return analysis;
}, {
  totalItems: 0,
  totalValue: 0,
  categories: {},
  highValueItems: [],
  lowStockItems: []
});

console.log(inventoryAnalysis);
```

#### Example 3: Student Performance Analytics
```javascript
const studentData = [
  { name: 'Ahmed', subjects: { math: 85, science: 92, english: 78 }, attendance: 95 },
  { name: 'Sara', subjects: { math: 91, science: 87, english: 94 }, attendance: 90 },
  { name: 'Ali', subjects: { math: 76, science: 82, english: 85 }, attendance: 88 }
];

// Comprehensive class performance analysis
const classAnalysis = studentData.reduce((analysis, student) => {
  const subjectMarks = Object.values(student.subjects);
  const studentAvg = subjectMarks.reduce((sum, mark) => sum + mark, 0) / subjectMarks.length;
  
  // Update overall statistics
  analysis.totalStudents++;
  analysis.totalAttendance += student.attendance;
  
  // Subject-wise analysis
  Object.entries(student.subjects).forEach(([subject, marks]) => {
    if (!analysis.subjectStats[subject]) {
      analysis.subjectStats[subject] = { total: 0, count: 0, highest: 0, lowest: 100 };
    }
    
    analysis.subjectStats[subject].total += marks;
    analysis.subjectStats[subject].count++;
    analysis.subjectStats[subject].highest = Math.max(analysis.subjectStats[subject].highest, marks);
    analysis.subjectStats[subject].lowest = Math.min(analysis.subjectStats[subject].lowest, marks);
  });
  
  // Performance categories
  if (studentAvg >= 90) analysis.excellentStudents++;
  else if (studentAvg >= 80) analysis.goodStudents++;
  else if (studentAvg >= 70) analysis.averageStudents++;
  else analysis.needsImprovement++;
  
  // Student details with calculated fields
  analysis.studentDetails.push({
    name: student.name,
    average: Math.round(studentAvg),
    totalMarks: subjectMarks.reduce((sum, mark) => sum + mark, 0),
    attendance: student.attendance,
    grade: studentAvg >= 90 ? 'A' : studentAvg >= 80 ? 'B' : studentAvg >= 70 ? 'C' : 'D'
  });
  
  return analysis;
}, {
  totalStudents: 0,
  totalAttendance: 0,
  excellentStudents: 0,
  goodStudents: 0,
  averageStudents: 0,
  needsImprovement: 0,
  subjectStats: {},
  studentDetails: []
});

// Calculate final averages
classAnalysis.averageAttendance = Math.round(classAnalysis.totalAttendance / classAnalysis.totalStudents);
Object.keys(classAnalysis.subjectStats).forEach(subject => {
  const stats = classAnalysis.subjectStats[subject];
  stats.average = Math.round(stats.total / stats.count);
});

console.log(classAnalysis);
```

#### Example 4: E-commerce Order Processing
```javascript
const orders = [
  {
    id: 'ORD001',
    items: [
      { name: 'Laptop', price: 50000, quantity: 1, category: 'Electronics' },
      { name: 'Mouse', price: 1500, quantity: 2, category: 'Electronics' }
    ],
    customer: 'Ahmed',
    status: 'completed',
    date: '2025-01-10'
  },
  {
    id: 'ORD002',
    items: [
      { name: 'Book', price: 500, quantity: 3, category: 'Education' }
    ],
    customer: 'Sara',
    status: 'pending',
    date: '2025-01-12'
  }
];

// Process all orders for business insights
const businessInsights = orders.reduce((insights, order) => {
  const orderTotal = order.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  // Revenue tracking
  insights.totalRevenue += orderTotal;
  insights.totalOrders++;
  
  // Customer analysis
  if (!insights.customerData[order.customer]) {
    insights.customerData[order.customer] = {
      totalSpent: 0,
      orderCount: 0,
      orders: []
    };
  }
  
  insights.customerData[order.customer].totalSpent += orderTotal;
  insights.customerData[order.customer].orderCount++;
  insights.customerData[order.customer].orders.push(order.id);
  
  // Status tracking
  insights.statusCount[order.status] = (insights.statusCount[order.status] || 0) + 1;
  
  // Category analysis
  order.items.forEach(item => {
    if (!insights.categoryStats[item.category]) {
      insights.categoryStats[item.category] = {
        revenue: 0,
        quantity: 0,
        uniqueProducts: new Set()
      };
    }
    
    insights.categoryStats[item.category].revenue += item.price * item.quantity;
    insights.categoryStats[item.category].quantity += item.quantity;
    insights.categoryStats[item.category].uniqueProducts.add(item.name);
  });
  
  return insights;
}, {
  totalRevenue: 0,
  totalOrders: 0,
  customerData: {},
  statusCount: {},
  categoryStats: {}
});

// Convert Sets to arrays for final output
Object.keys(businessInsights.categoryStats).forEach(category => {
  businessInsights.categoryStats[category].uniqueProducts = 
    Array.from(businessInsights.categoryStats[category].uniqueProducts);
});

console.log(businessInsights);
```

#### Example 5: Data Transformation and Grouping
```javascript
const employees = [
  { name: 'Ahmed', department: 'IT', salary: 75000, projects: ['Web App', 'Mobile App'] },
  { name: 'Sara', department: 'Marketing', salary: 60000, projects: ['Campaign A'] },
  { name: 'Ali', department: 'IT', salary: 90000, projects: ['Web App', 'API Development'] },
  { name: 'Fatima', department: 'HR', salary: 65000, projects: ['Recruitment'] }
];

// Create complex organizational structure
const organizationData = employees.reduce((org, employee) => {
  const dept = employee.department;
  
  // Initialize department if doesn't exist
  if (!org.departments[dept]) {
    org.departments[dept] = {
      employees: [],
      totalSalary: 0,
      avgSalary: 0,
      employeeCount: 0,
      projects: new Set(),
      salaryRange: { min: Infinity, max: 0 }
    };
  }
  
  const deptData = org.departments[dept];
  
  // Update department data
  deptData.employees.push(employee.name);
  deptData.totalSalary += employee.salary;
  deptData.employeeCount++;
  deptData.avgSalary = Math.round(deptData.totalSalary / deptData.employeeCount);
  
  // Salary range
  deptData.salaryRange.min = Math.min(deptData.salaryRange.min, employee.salary);
  deptData.salaryRange.max = Math.max(deptData.salaryRange.max, employee.salary);
  
  // Project tracking
  employee.projects.forEach(project => {
    deptData.projects.add(project);
    
    // Global project tracking
    if (!org.projectAssignments[project]) {
      org.projectAssignments[project] = [];
    }
    org.projectAssignments[project].push(employee.name);
  });
  
  // Overall organization stats
  org.totalEmployees++;
  org.totalSalaryBudget += employee.salary;
  
  return org;
}, {
  departments: {},
  projectAssignments: {},
  totalEmployees: 0,
  totalSalaryBudget: 0
});

// Convert Sets to Arrays
Object.keys(organizationData.departments).forEach(dept => {
  organizationData.departments[dept].projects = 
    Array.from(organizationData.departments[dept].projects);
});

organizationData.avgOrganizationSalary = Math.round(
  organizationData.totalSalaryBudget / organizationData.totalEmployees
);

console.log(organizationData);
```

---

## 4. forEach() - Iterate Over Elements

**Description**
Executes a function for each array element. Unlike map, it doesn't return a new array. Best for side effects like logging, DOM manipulation, or API calls.

**Syntax**
```javascript
array.forEach(function(currentValue, index, arr), thisValue)
```

### 5 Advanced Examples:

#### Example 1: DOM Manipulation and Event Handling
```javascript
const menuItems = [
  { id: 'home', text: 'Home', active: true },
  { id: 'about', text: 'About', active: false },
  { id: 'services', text: 'Services', active: false },
  { id: 'contact', text: 'Contact', active: false }
];

// Create navigation menu with event handlers
menuItems.forEach((item, index) => {
  // Create DOM elements
  const li = document.createElement('li');
  const a = document.createElement('a');
  
  // Set attributes and content
  a.href = `#${item.id}`;
  a.textContent = item.text;
  a.id = `menu-${item.id}`;
  
  // Add active class
  if (item.active) {
    li.classList.add('active');
  }
  
  // Add click event listener
  a.addEventListener('click', function(e) {
    e.preventDefault();
    console.log(`Navigating to ${item.text} section`);
    
    // Remove active from all items
    document.querySelectorAll('.nav-menu li').forEach(navItem => {
      navItem.classList.remove('active');
    });
    
    // Add active to clicked item
    li.classList.add('active');
  });
  
  // Add keyboard navigation
  a.addEventListener('keydown', function(e) {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      a.click();
    }
  });
  
  li.appendChild(a);
  document.querySelector('.nav-menu')?.appendChild(li);
  
  console.log(`Menu item ${index + 1}: ${item.text} created`);
});
```

#### Example 2: Form Validation with Error Display
```javascript
const formFields = [
  { name: 'email', value: 'invalid-email', rules: ['required', 'email'] },
  { name: 'password', value: '123', rules: ['required', 'minLength:8'] },
  { name: 'confirmPassword', value: '456', rules: ['required', 'matches:password'] },
  { name: 'age', value: '15', rules: ['required', 'min:18'] }
];

const formData = {
  email: 'invalid-email',
  password: '123',
  confirmPassword: '456',
  age: '15'
};

// Validate each field and display errors
formFields.forEach((field, index) => {
  const errors = [];
  const value = field.value;
  
  console.log(`Validating field ${index + 1}: ${field.name}`);
  
  field.rules.forEach(rule => {
    if (rule === 'required' && !value.trim()) {
      errors.push(`${field.name} is required`);
    } else if (rule === 'email' && !/\S+@\S+\.\S+/.test(value)) {
      errors.push(`${field.name} must be a valid email`);
    } else if (rule.startsWith('minLength:')) {
      const minLength = parseInt(rule.split(':')[1]);
      if (value.length < minLength) {
        errors.push(`${field.name} must be at least ${minLength} characters`);
      }
    } else if (rule.startsWith('min:')) {
      const minValue = parseInt(rule.split(':')[1]);
      if (parseInt(value) < minValue) {
        errors.push(`${field.name} must be at least ${minValue}`);
      }
    } else if (rule.startsWith('matches:')) {
      const matchField = rule.split(':')[1];
      if (value !== formData[matchField]) {
        errors.push(`${field.name} must match ${matchField}`);
      }
    }
  });
  
  // Display errors in DOM
  const errorContainer = document.getElementById(`${field.name}-errors`);
  if (errorContainer) {
    errorContainer.innerHTML = '';
    errors.forEach(error => {
      const errorElement = document.createElement('div');
      errorElement.className = 'error-message';
      errorElement.textContent = error;
      errorContainer.appendChild(errorElement);
    });
  }
  
  console.log(`Field ${field.name} has ${errors.length} errors:`, errors);
});
```

#### Example 3: API Data Processing and Caching
```javascript
const apiEndpoints = [
  { name: 'users', url: '/api/users', priority: 1 },
  { name: 'products', url: '/api/products', priority: 2 },
  { name: 'orders', url: '/api/orders', priority: 3 },
  { name: 'analytics', url: '/api/analytics', priority: 4 }
];

const cache = new Map();
const loadingStates = new Map();

// Process each API endpoint
apiEndpoints.forEach(async (endpoint, index) => {
  console.log(`Processing endpoint ${index + 1}: ${endpoint.name}`);
  
  // Set loading state
  loadingStates.set(endpoint.name, true);
  
  // Simulate API call delay based on priority
  const delay = endpoint.priority * 500;
  
  setTimeout(async () => {
    try {
      // Simulate API call
      console.log(`Fetching data from ${endpoint.url}`);
      
      // Mock API response
      const mockData = {
        users: [{ id: 1, name: 'Ahmed' }, { id: 2, name: 'Sara' }],
        products: [{ id: 1, name: 'Laptop', price: 50000 }],
        orders: [{ id: 1, userId: 1, total: 50000 }],
        analytics: { totalUsers: 2, totalRevenue: 50000 }
      };
      
      const data = mockData[endpoint.name] || {};
      
      // Cache the data
      cache.set(endpoint.name, {
        data: data,
        timestamp: Date.now(),
        ttl: 300000 // 5 minutes
      });
      
      // Update loading state
      loadingStates.set(endpoint.name, false);
      
      console.log(`✓ ${endpoint.name} data loaded and cached`);
      
      // Update UI or trigger callbacks
      if (typeof window !== 'undefined') {
        const event = new CustomEvent(`${endpoint.name}Loaded`, {
          detail: { data, endpoint: endpoint.name }
        });
        window.dispatchEvent(event);
      }
      
    } catch (error) {
      console.error(`✗ Failed to load ${endpoint.name}:`, error.message);
      loadingStates.set(endpoint.name, false);
      
      // Handle error in UI
      const errorEvent = new CustomEvent('apiError', {
        detail: { endpoint: endpoint.name, error: error.message }
      });
      if (typeof window !== 'undefined') {
        window.dispatchEvent(errorEvent);
      }
    }
  }, delay);
});

// Log cache status periodically
setTimeout(() => {
  console.log('\n=== Cache Status ===');
  cache.forEach((value, key) => {
    const age = (Date.now() - value.timestamp) / 1000;
    console.log(`${key}: ${Object.keys(value.data).length} items, ${age.toFixed(1)}s old`);
  });
}, 3000);
```

#### Example 4: File Processing and Progress Tracking
```javascript
const files = [
  { name: 'document.pdf', size: 1024000, type: 'pdf' },
  { name: 'image.jpg', size: 512000, type: 'image' },
  { name: 'video.mp4', size: 10240000, type: 'video' },
  { name: 'data.csv', size: 256000, type: 'csv' }
];

const processedFiles = [];
let totalSize = files.reduce((sum, file) => sum + file.size, 0);
let processedSize = 0;

// Process each file with progress tracking
files.forEach((file, index) => {
  console.log(`\nProcessing file ${index + 1}/${files.length}: ${file.name}`);
  
  const startTime = Date.now();
  
  // Simulate file processing based on size and type
  const processingTime = Math.ceil(file.size / 100000) * 100;
  
  // Update progress immediately
  const progressPercent = Math.round((processedSize / totalSize) * 100);
  console.log(`Progress: ${progressPercent}% (${formatBytes(processedSize)}/${formatBytes(totalSize)})`);
  
  setTimeout(() => {
    const endTime = Date.now();
    const duration = endTime - startTime;
    
    // File-specific processing
    let processedData = {};
    
    if (file.type === 'image') {
      processedData = {
        ...file,
        compressed: true,
        thumbnailGenerated: true,
        metadata: {
          dimensions: '1920x1080',
          format: 'JPEG'
        }
      };
    } else if (file.type === 'pdf') {
      processedData = {
        ...file,
        textExtracted: true,
        pageCount: Math.ceil(file.size / 50000),
        searchable: true
      };
    } else if (file.type === 'video') {
      processedData = {
        ...file,
        transcoded: true,
        duration: '00:05:30',
        formats: ['mp4', 'webm']
      };
    } else {
      processedData = {
        ...file,
        parsed: true,
        rowCount: Math.ceil(file.size / 100)
      };
    }
    
    processedData.processingTime = duration;
    processedData.status = 'completed';
    processedData.processedAt = new Date().toISOString();
    
    processedFiles.push(processedData);
    processedSize += file.size;
    
    const finalProgress = Math.round((processedSize / totalSize) * 100);
    console.log(`✓ ${file.name} processed in ${duration}ms`);
    console.log(`Progress: ${finalProgress}%`);
    
    // Trigger completion callback if all files processed
    if (processedFiles.length === files.length) {
      console.log('\n🎉 All files processed successfully!');
      console.log('Processing Summary:', {
        totalFiles: files.length,
        totalSize: formatBytes(totalSize),
        avgProcessingTime: Math.round(
          processedFiles.reduce((sum, f) => sum + f.processingTime, 0) / files.length
        ) + 'ms'
      });
    }
  }, processingTime);
});

function formatBytes(bytes) {
  if (bytes === 0) return '0 Bytes';
  const k = 1024;
  const sizes = ['Bytes', 'KB', 'MB', 'GB'];
  const i = Math.floor(Math.log(bytes) / Math.log(k));
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
}
```

#### Example 5: Database Migration and Logging
```javascript
const migrationTasks = [
  {
    name: 'Create Users Table',
    sql: 'CREATE TABLE users (id INT PRIMARY KEY, name VARCHAR(255))',
    dependencies: [],
    estimatedTime: 1000
  },
  {
    name: 'Add User Indexes',
    sql: 'CREATE INDEX idx_user_name ON users(name)',
    dependencies: ['Create Users Table'],
    estimatedTime: 2000
  },
  {
    name: 'Insert Sample Data',
    sql: 'INSERT INTO users VALUES (1, "Ahmed"), (2, "Sara")',
    dependencies: ['Create Users Table'],
    estimatedTime: 1500
  },
  {
    name: 'Create Orders Table',
    sql: 'CREATE TABLE orders (id INT PRIMARY KEY, user_id INT, FOREIGN KEY (user_id) REFERENCES users(id))',
    dependencies: ['Create Users Table'],
    estimatedTime: 1200
  }
];

const executionLog = [];
const completedTasks = new Set();

// Execute migration tasks with dependency checking
migrationTasks.forEach((task, index) => {
  console.log(`\n📋 Preparing migration ${index + 1}: ${task.name}`);
  
  // Check dependencies
  const canExecute = task.dependencies.every(dep => completedTasks.has(dep));
  
  if (!canExecute) {
    const missingDeps = task.dependencies.filter(dep => !completedTasks.has(dep));
    console.log(`⏳ Waiting for dependencies: ${missingDeps.join(', ')}`);
    
    // Schedule for later execution (simplified)
    const checkInterval = setInterval(() => {
      const nowCanExecute = task.dependencies.every(dep => completedTasks.has(dep));
      if (nowCanExecute) {
        clearInterval(checkInterval);
        executeMigration(task, index);
      }
    }, 100);
  } else {
    executeMigration(task, index);
  }
});

function executeMigration(task, index) {
  const startTime = Date.now();
  
  console.log(`🚀 Executing migration ${index + 1}: ${task.name}`);
  console.log(`   SQL: ${task.sql}`);
  
  // Simulate database execution
  setTimeout(() => {
    const endTime = Date.now();
    const actualTime = endTime - startTime;
    
    // Log execution details
    const logEntry = {
      taskName: task.name,
      sql: task.sql,
      startTime: new Date(startTime).toISOString(),
      endTime: new Date(endTime).toISOString(),
      executionTime: actualTime,
      estimatedTime: task.estimatedTime,
      timeDifference: actualTime - task.estimatedTime,
      status: 'success',
      dependencies: task.dependencies
    };
    
    executionLog.push(logEntry);
    completedTasks.add(task.name);
    
    console.log(`✅ Migration completed: ${task.name}`);
    console.log(`   Time taken: ${actualTime}ms (estimated: ${task.estimatedTime}ms)`);
    
    // Check if all migrations are complete
    if (completedTasks.size === migrationTasks.length) {
      console.log('\n🎉 All migrations completed successfully!');
      
      // Generate summary report
      const totalEstimated = migrationTasks.reduce((sum, t) => sum + t.estimatedTime, 0);
      const totalActual = executionLog.reduce((sum, log) => sum + log.executionTime, 0);
      
      console.log('\n📊 Migration Summary:');
      console.log(`   Total tasks: ${migrationTasks.length}`);
      console.log(`   Estimated time: ${totalEstimated}ms`);
      console.log(`   Actual time: ${totalActual}ms`);
      console.log(`   Time variance: ${totalActual - totalEstimated}ms`);
      
      // Log detailed execution order
      console.log('\n📝 Execution Log:');
      executionLog.forEach((log, i) => {
        console.log(`   ${i + 1}. ${log.taskName} (${log.executionTime}ms)`);
      });
    }
  }, task.estimatedTime + Math.random() * 500); // Add some randomness
}
```

---

## 5. find() - Find First Matching Element

**Description**
Returns the first element that satisfies the testing function. Returns `undefined` if no element is found.

**Syntax**
```javascript
array.find(function(currentValue, index, arr), thisValue)
```

### 5 Advanced Examples:

#### Example 1: User Authentication and Role Management
```javascript
const users = [
  { 
    id: 1, 
    username: 'ahmed123', 
    email: 'ahmed@email.com', 
    roles: ['user', 'moderator'], 
    permissions: ['read', 'write', 'moderate'],
    lastLogin: '2025-01-15T10:30:00',
    isActive: true,
    profile: { firstName: 'Ahmed', lastName: 'Khan', department: 'IT' }
  },
  { 
    id: 2, 
    username: 'sara456', 
    email: 'sara@email.com', 
    roles: ['user'], 
    permissions: ['read'],
    lastLogin: '2025-01-10T15:20:00',
    isActive: true,
    profile: { firstName: 'Sara', lastName: 'Ali', department: 'Marketing' }
  },
  { 
    id: 3, 
    username: 'admin', 
    email: 'admin@email.com', 
    roles: ['admin', 'user'], 
    permissions: ['read', 'write', 'delete', 'admin'],
    lastLogin: '2025-01-16T09:00:00',
    isActive: true,
    profile: { firstName: 'Admin', lastName: 'User', department: 'Management' }
  }
];

// Find user with complex authentication logic
function authenticateUser(identifier, requiredPermission = null) {
  const user = users.find(user => {
    // Check multiple identification methods
    const matchesIdentifier = 
      user.username === identifier || 
      user.email === identifier ||
      user.id === parseInt(identifier);
    
    // Check if user is active
    const isActiveUser = user.isActive;
    
    // Check permission if required
    const hasPermission = requiredPermission ? 
      user.permissions.includes(requiredPermission) : true;
    
    // Check if user logged in recently (within 30 days)
    const lastLoginDate = new Date(user.lastLogin);
    const thirtyDaysAgo = new Date();
    thirtyDaysAgo.setDate(thirtyDaysAgo.getDate() - 30);
    const isRecentlyActive = lastLoginDate > thirtyDaysAgo;
    
    return matchesIdentifier && isActiveUser && hasPermission && isRecentlyActive;
  });
  
  if (user) {
    console.log(`✅ User authenticated: ${user.profile.firstName} ${user.profile.lastName}`);
    return {
      ...user,
      fullName: `${user.profile.firstName} ${user.profile.lastName}`,
      canModerate: user.roles.includes('moderator') || user.roles.includes('admin'),
      isAdmin: user.roles.includes('admin'),
      daysSinceLogin: Math.floor((new Date() - new Date(user.lastLogin)) / (1000 * 60 * 60 * 24))
    };
  } else {
    console.log('❌ Authentication failed');
    return null;
  }
}

// Usage examples
const moderatorUser = authenticateUser('ahmed123', 'moderate');
const adminUser = authenticateUser('admin@email.com', 'admin');
const regularUser = authenticateUser('sara456');

console.log('Moderator User:', moderatorUser?.fullName);
console.log('Admin User:', adminUser?.fullName);
console.log('Regular User:', regularUser?.fullName);
```

#### Example 2: E-commerce Product Search with Filters
```javascript
const products = [
  {
    id: 'P001',
    name: 'Gaming Laptop Pro',
    category: 'Electronics',
    subcategory: 'Laptops',
    price: 150000,
    brand: 'TechBrand',
    specs: {
      ram: '16GB',
      storage: '1TB SSD',
      processor: 'Intel i7',
      graphics: 'RTX 3070'
    },
    ratings: { average: 4.5, count: 245 },
    availability: { inStock: true, quantity: 8 },
    tags: ['gaming', 'high-performance', 'portable'],
    seller: { name: 'TechStore', rating: 4.8, verified: true }
  },
  {
    id: 'P002',
    name: 'Professional Office Chair',
    category: 'Furniture',
    subcategory: 'Chairs',
    price: 25000,
    brand: 'ComfortPlus',
    specs: {
      material: 'Leather',
      adjustable: true,
      warranty: '5 years'
    },
    ratings: { average: 4.2, count: 89 },
    availability: { inStock: false, quantity: 0 },
    tags: ['ergonomic', 'professional', 'adjustable'],
    seller: { name: 'FurnitureHub', rating: 4.3, verified: true }
  },
  {
    id: 'P003',
    name: 'Wireless Bluetooth Headphones',
    category: 'Electronics',
    subcategory: 'Audio',
    price: 8000,
    brand: 'SoundTech',
    specs: {
      battery: '30 hours',
      noiseCancellation: true,
      wireless: true
    },
    ratings: { average: 4.7, count: 1250 },
    availability: { inStock: true, quantity: 25 },
    tags: ['wireless', 'noise-cancelling', 'premium-sound'],
    seller: { name: 'AudioWorld', rating: 4.9, verified: true }
  }
];

// Advanced product search function
function findProduct(searchCriteria) {
  const product = products.find(product => {
    // Text search in name and tags
    if (searchCriteria.searchText) {
      const searchLower = searchCriteria.searchText.toLowerCase();
      const matchesText = 
        product.name.toLowerCase().includes(searchLower) ||
        product.tags.some(tag => tag.toLowerCase().includes(searchLower)) ||
        product.brand.toLowerCase().includes(searchLower);
      
      if (!matchesText) return false;
    }
    
    // Category filter
    if (searchCriteria.category && product.category !== searchCriteria.category) {
      return false;
    }
    
    // Price range filter
    if (searchCriteria.priceRange) {
      const { min = 0, max = Infinity } = searchCriteria.priceRange;
      if (product.price < min || product.price > max) {
        return false;
      }
    }
    
    // Rating filter
    if (searchCriteria.minRating && product.ratings.average < searchCriteria.minRating) {
      return false;
    }
    
    // Stock availability
    if (searchCriteria.inStockOnly && !product.availability.inStock) {
      return false;
    }
    
    // Verified seller filter
    if (searchCriteria.verifiedSellersOnly && !product.seller.verified) {
      return false;
    }
    
    // Specific spec requirements
    if (searchCriteria.requiredSpecs) {
      const hasRequiredSpecs = Object.entries(searchCriteria.requiredSpecs).every(([key, value]) => {
        return product.specs[key] && product.specs[key].toString().toLowerCase().includes(value.toLowerCase());
      });
      if (!hasRequiredSpecs) return false;
    }
    
    return true;
  });
  
  if (product) {
    return {
      ...product,
      priceFormatted: `₹${product.price.toLocaleString()}`,
      discountPrice: Math.round(product.price * 0.9), // 10% discount
      estimatedDelivery: product.availability.inStock ? '2-3 days' : '7-10 days',
      totalReviews: product.ratings.count,
      isPopular: product.ratings.count > 100,
      isPremium: product.price > 50000
    };
  }
  
  return null;
}

// Usage examples
const gamingLaptop = findProduct({
  searchText: 'gaming',
  category: 'Electronics',
  priceRange: { min: 100000, max: 200000 },
  inStockOnly: true,
  requiredSpecs: { graphics: 'RTX' }
});

const affordableHeadphones = findProduct({
  category: 'Electronics',
  priceRange: { max: 10000 },
  minRating: 4.5,
  verifiedSellersOnly: true
});

console.log('Gaming Laptop Found:', gamingLaptop?.name);
console.log('Affordable Headphones:', affordableHeadphones?.name);
```

#### Example 3: Task Management and Priority Finding
```javascript
const tasks = [
  {
    id: 'T001',
    title: 'Complete Project Documentation',
    description: 'Write comprehensive documentation for the new feature',
    priority: 'high',
    status: 'in-progress',
    assignee: 'Ahmed Khan',
    dueDate: '2025-01-20',
    estimatedHours: 8,
    completedHours: 3,
    tags: ['documentation', 'project'],
    dependencies: ['T002'],
    createdAt: '2025-01-10',
    project: { name: 'Web Application', client: 'TechCorp' }
  },
  {
    id: 'T002',
    title: 'Fix Critical Bug in Payment System',
    description: 'Resolve the payment processing issue affecting checkout',
    priority: 'critical',
    status: 'completed',
    assignee: 'Sara Ali',
    dueDate: '2025-01-15',
    estimatedHours: 4,
    completedHours: 5,
    tags: ['bugfix', 'payment', 'critical'],
    dependencies: [],
    createdAt: '2025-01-08',
    project: { name: 'E-commerce Platform', client: 'ShopMart' }
  },
  {
    id: 'T003',
    title: 'Implement User Dashboard',
    description: 'Create responsive dashboard with analytics',
    priority: 'medium',
    status: 'pending',
    assignee: 'Ali Hassan',
    dueDate: '2025-01-25',
    estimatedHours: 12,
    completedHours: 0,
    tags: ['feature', 'dashboard', 'ui'],
    dependencies: ['T001'],
    createdAt: '2025-01-12',
    project: { name: 'Admin Panel', client: 'DataCorp' }
  }
];

// Find next task to work on based on complex criteria
function findNextTask(assignee, currentDate = new Date()) {
  const nextTask = tasks.find(task => {
    // Must be assigned to the person
    if (task.assignee !== assignee) return false;
    
    // Must not be completed
    if (task.status === 'completed') return false;
    
    // Check if dependencies are completed
    const dependenciesCompleted = task.dependencies.every(depId => {
      const depTask = tasks.find(t => t.id === depId);
      return depTask && depTask.status === 'completed';
    });
    if (!dependenciesCompleted) return false;
    
    // Check if due date is approaching (within 7 days)
    const dueDate = new Date(task.dueDate);
    const daysDiff = Math.ceil((dueDate - currentDate) / (1000 * 60 * 60 * 24));
    const isDueSoon = daysDiff <= 7;
    
    // Priority scoring
    const priorityScore = {
      'critical': 100,
      'high': 75,
      'medium': 50,
      'low': 25
    }[task.priority] || 0;
    
    // Calculate urgency score
    const urgencyScore = isDueSoon ? 50 : 0;
    
    // Calculate progress penalty (tasks in progress get priority)
    const progressBonus = task.status === 'in-progress' ? 25 : 0;
    
    // Store total score for comparison
    task.calculatedScore = priorityScore + urgencyScore + progressBonus;
    
    return true;
  });
  
  // If multiple tasks found, return the one with highest score
  const eligibleTasks = tasks.filter(task => {
    return task.assignee === assignee && 
           task.status !== 'completed' && 
           task.dependencies.every(depId => {
             const depTask = tasks.find(t => t.id === depId);
             return depTask && depTask.status === 'completed';
           });
  });
  
  if (eligibleTasks.length === 0) return null;
  
  // Calculate scores for all eligible tasks
  eligibleTasks.forEach(task => {
    const dueDate = new Date(task.dueDate);
    const daysDiff = Math.ceil((dueDate - currentDate) / (1000 * 60 * 60 * 24));
    const isDueSoon = daysDiff <= 7;
    const isOverdue = daysDiff < 0;
    
    const priorityScore = { 'critical': 100, 'high': 75, 'medium': 50, 'low': 25 }[task.priority] || 0;
    const urgencyScore = isOverdue ? 100 : isDueSoon ? 50 : 0;
    const progressBonus = task.status === 'in-progress' ? 25 : 0;
    
    task.calculatedScore = priorityScore + urgencyScore + progressBonus;
    task.daysUntilDue = daysDiff;
    task.isOverdue = isOverdue;
  });
  
  // Find task with highest score
  const bestTask = eligibleTasks.reduce((best, current) => 
    current.calculatedScore > best.calculatedScore ? current : best
  );
  
  return {
    ...bestTask,
    completionPercentage: Math.round((bestTask.completedHours / bestTask.estimatedHours) * 100),
    remainingHours: bestTask.estimatedHours - bestTask.completedHours,
    isUrgent: bestTask.daysUntilDue <= 2,
    recommendation: bestTask.isOverdue ? 'IMMEDIATE ATTENTION' : 
                   bestTask.daysUntilDue <= 2 ? 'URGENT' :
                   bestTask.priority === 'critical' ? 'HIGH PRIORITY' : 'NORMAL'
  };
}

// Usage examples
const ahmedNextTask = findNextTask('Ahmed Khan');
const saraNextTask = findNextTask('Sara Ali');
const aliNextTask = findNextTask('Ali Hassan');

console.log('Ahmed should work on:', ahmedNextTask?.title);
console.log('Priority level:', ahmedNextTask?.recommendation);
console.log('Days until due:', ahmedNextTask?.daysUntilDue);
```

#### Example 4: Configuration and Settings Management
```javascript
const configurations = [
  {
    id: 'database',
    environment: 'production',
    service: 'postgresql',
    version: '13.4',
    settings: {
      host: 'prod-db.company.com',
      port: 5432,
      maxConnections: 100,
      ssl: true,
      backup: { enabled: true, frequency: 'daily' }
    },
    metadata: {
      owner: 'DevOps Team',
      lastUpdated: '2025-01-15',
      critical: true
    },
    healthCheck: {
      status: 'healthy',
      lastChecked: '2025-01-16T08:00:00',
      responseTime: 45
    }
  },
  {
    id: 'cache',
    environment: 'production',
    service: 'redis',
    version: '6.2',
    settings: {
      host: 'prod-cache.company.com',
      port: 6379,
      maxMemory: '2GB',
      evictionPolicy: 'allkeys-lru'
    },
    metadata: {
      owner: 'Backend Team',
      lastUpdated: '2025-01-12',
      critical: false
    },
    healthCheck: {
      status: 'healthy',
      lastChecked: '2025-01-16T07:55:00',
      responseTime: 12
    }
  },
  {
    id: 'api',
    environment: 'staging',
    service: 'nodejs',
    version: '18.0',
    settings: {
      host: 'staging-api.company.com',
      port: 3000,
      rateLimit: 1000,
      cors: { enabled: true, origins: ['*.company.com'] }
    },
    metadata: {
      owner: 'API Team',
      lastUpdated: '2025-01-10',
      critical: true
    },
    healthCheck: {
      status: 'warning',
      lastChecked: '2025-01-16T07:50:00',
      responseTime: 150
    }
  }
];

// Find configuration with complex matching
function findConfiguration(criteria) {
  const config = configurations.find(config => {
    // Environment match
    if (criteria.environment && config.environment !== criteria.environment) {
      return false;
    }
    
    // Service type match
    if (criteria.service && config.service !== criteria.service) {
      return false;
    }
    
    // Version compatibility check
    if (criteria.minVersion) {
      const configVersion = parseFloat(config.version);
      const minVersion = parseFloat(criteria.minVersion);
      if (configVersion < minVersion) {
        return false;
      }
    }
    
    // Health status check
    if (criteria.healthStatus && config.healthCheck.status !== criteria.healthStatus) {
      return false;
    }
    
    // Performance threshold
    if (criteria.maxResponseTime && config.healthCheck.responseTime > criteria.maxResponseTime) {
      return false;
    }
    
    // Critical systems only
    if (criteria.criticalOnly && !config.metadata.critical) {
      return false;
    }
    
    // Owner/team match
    if (criteria.owner && !config.metadata.owner.toLowerCase().includes(criteria.owner.toLowerCase())) {
      return false;
    }
    
    // Recent updates check
    if (criteria.recentlyUpdated) {
      const updateDate = new Date(config.metadata.lastUpdated);
      const daysAgo = (new Date() - updateDate) / (1000 * 60 * 60 * 24);
      if (daysAgo > criteria.recentlyUpdated) {
        return false;
      }
    }
    
    // Custom setting checks
    if (criteria.customChecks) {
      return criteria.customChecks.every(check => {
        const value = getNestedValue(config.settings, check.path);
        return check.condition(value);
      });
    }
    
    return true;
  });
  
  if (config) {
    const lastUpdateDays = Math.floor((new Date() - new Date(config.metadata.lastUpdated)) / (1000 * 60 * 60 * 24));
    const lastHealthCheckMinutes = Math.floor((new Date() - new Date(config.healthCheck.lastChecked)) / (1000 * 60));
    
    return {
      ...config,
      analysis: {
        isHealthy: config.healthCheck.status === 'healthy',
        performanceGrade: config.healthCheck.responseTime < 50 ? 'A' : 
                         config.healthCheck.responseTime < 100 ? 'B' : 'C',
        isUpToDate: lastUpdateDays <= 7,
        daysSinceUpdate: lastUpdateDays,
        minutesSinceHealthCheck: lastHealthCheckMinutes,
        configurationScore: calculateConfigScore(config)
      }
    };
  }
  
  return null;
}

function getNestedValue(obj, path) {
  return path.split('.').reduce((current, key) => current && current[key], obj);
}

function calculateConfigScore(config) {
  let score = 0;
  
  // Health score
  score += config.healthCheck.status === 'healthy' ? 30 : 
           config.healthCheck.status === 'warning' ? 15 : 0;
  
  // Performance score
  score += config.healthCheck.responseTime < 50 ? 25 : 
           config.healthCheck.responseTime < 100 ? 15 : 5;
  
  // Update recency score
  const daysSinceUpdate = (new Date() - new Date(config.metadata.lastUpdated)) / (1000 * 60 * 60 * 24);
  score += daysSinceUpdate <= 7 ? 25 : daysSinceUpdate <= 30 ? 15 : 5;
  
  // Critical system bonus
  score += config.metadata.critical ? 20 : 10;
  
  return score;
}

// Usage examples
const productionDatabase = findConfiguration({
  environment: 'production',
  service: 'postgresql',
  healthStatus: 'healthy',
  criticalOnly: true
});

const fastPerformingConfigs = findConfiguration({
  maxResponseTime: 50,
  healthStatus: 'healthy'
});

const recentlyUpdatedConfigs = findConfiguration({
  recentlyUpdated: 10, // Within last 10 days
  customChecks: [
    {
      path: 'ssl',
      condition: (value) => value === true
    }
  ]
});

console.log('Production Database Config:', productionDatabase?.id);
console.log('Performance Grade:', productionDatabase?.analysis?.performanceGrade);
console.log('Configuration Score:', productionDatabase?.analysis?.configurationScore);
```

#### Example 5: Advanced Data Mining and Pattern Recognition
```javascript
const customerBehaviorData = [
  {
    customerId: 'C001',
    profile: {
      age: 28,
      location: 'Karachi',
      membershipLevel: 'gold',
      joinDate: '2023-06-15'
    },
    purchaseHistory: [
      { date: '2025-01-10', amount: 5000, category: 'electronics' },
      { date: '2025-01-08', amount: 2000, category: 'books' },
      { date: '2024-12-25', amount: 8000, category: 'electronics' }
    ],
    behaviorMetrics: {
      avgOrderValue: 5000,
      purchaseFrequency: 2.3, // purchases per month
      returnRate: 0.05,
      supportTickets: 1,
      referrals: 3
    },
    preferences: {
      communicationChannel: 'email',
      paymentMethod: 'card',
      deliveryTime: 'evening',
      categories: ['electronics', 'books', 'gadgets']
    },
    engagement: {
      emailOpenRate: 0.75,
      clickThroughRate: 0.15,
      socialMediaFollower: true,
      reviewsWritten: 8
    }
  },
  {
    customerId: 'C002',
    profile: {
      age: 35,
      location: 'Lahore',
      membershipLevel: 'platinum',
      joinDate: '2022-03-20'
    },
    purchaseHistory: [
      { date: '2025-01-12', amount: 12000, category: 'fashion' },
      { date: '2025-01-05', amount: 15000, category: 'home' },
      { date: '2024-12-30', amount: 9000, category: 'fashion' }
    ],
    behaviorMetrics: {
      avgOrderValue: 12000,
      purchaseFrequency: 3.1,
      returnRate: 0.02,
      supportTickets: 0,
      referrals: 7
    },
    preferences: {
      communicationChannel: 'sms',
      paymentMethod: 'wallet',
      deliveryTime: 'morning',
      categories: ['fashion', 'home', 'beauty']
    },
    engagement: {
      emailOpenRate: 0.85,
      clickThroughRate: 0.25,
      socialMediaFollower: true,
      reviewsWritten: 15
    }
  },
  {
    customerId: 'C003',
    profile: {
      age: 42,
      location: 'Islamabad',
      membershipLevel: 'silver',
      joinDate: '2024-01-10'
    },
    purchaseHistory: [
      { date: '2025-01-14', amount: 3500, category: 'books' },
      { date: '2025-01-01', amount: 4500, category: 'sports' }
    ],
    behaviorMetrics: {
      avgOrderValue: 4000,
      purchaseFrequency: 1.8,
      returnRate: 0.08,
      supportTickets: 2,
      referrals: 1
    },
    preferences: {
      communicationChannel: 'phone',
      paymentMethod: 'cod',
      deliveryTime: 'afternoon',
      categories: ['books', 'sports', 'health']
    },
    engagement: {
      emailOpenRate: 0.45,
      clickThroughRate: 0.08,
      socialMediaFollower: false,
      reviewsWritten: 3
    }
  }
];

// Find ideal customer for targeted marketing campaign
function findTargetCustomer(campaignCriteria) {
  const targetCustomer = customerBehaviorData.find(customer => {
    // Demographic targeting
    if (campaignCriteria.ageRange) {
      const { min, max } = campaignCriteria.ageRange;
      if (customer.profile.age < min || customer.profile.age > max) {
        return false;
      }
    }
    
    if (campaignCriteria.locations && !campaignCriteria.locations.includes(customer.profile.location)) {
      return false;
    }
    
    if (campaignCriteria.membershipLevels && !campaignCriteria.membershipLevels.includes(customer.profile.membershipLevel)) {
      return false;
    }
    
    // Purchase behavior targeting
    if (campaignCriteria.minAvgOrderValue && customer.behaviorMetrics.avgOrderValue < campaignCriteria.minAvgOrderValue) {
      return false;
    }
    
    if (campaignCriteria.minPurchaseFrequency && customer.behaviorMetrics.purchaseFrequency < campaignCriteria.minPurchaseFrequency) {
      return false;
    }
    
    if (campaignCriteria.maxReturnRate && customer.behaviorMetrics.returnRate > campaignCriteria.maxReturnRate) {
      return false;
    }
    
    // Category interest matching
    if (campaignCriteria.targetCategories) {
      const hasInterestInCategories = campaignCriteria.targetCategories.some(category => 
        customer.preferences.categories.includes(category)
      );
      if (!hasInterestInCategories) {
        return false;
      }
    }
    
    // Engagement level requirements
    if (campaignCriteria.minEngagementScore) {
      const engagementScore = calculateEngagementScore(customer.engagement);
      if (engagementScore < campaignCriteria.minEngagementScore) {
        return false;
      }
    }
    
    // Communication channel preference
    if (campaignCriteria.communicationChannel && 
        customer.preferences.communicationChannel !== campaignCriteria.communicationChannel) {
      return false;
    }
    
    // Recent purchase activity
    if (campaignCriteria.recentPurchaseRequired) {
      const hasRecentPurchase = customer.purchaseHistory.some(purchase => {
        const purchaseDate = new Date(purchase.date);
        const daysSince = (new Date() - purchaseDate) / (1000 * 60 * 60 * 24);
        return daysSince <= campaignCriteria.recentPurchaseRequired;
      });
      if (!hasRecentPurchase) {
        return false;
      }
    }
    
    // Referral potential
    if (campaignCriteria.minReferrals && customer.behaviorMetrics.referrals < campaignCriteria.minReferrals) {
      return false;
    }
    
    return true;
  });
  
  if (targetCustomer) {
    // Calculate customer lifetime value
    const membershipMonths = Math.floor((new Date() - new Date(targetCustomer.profile.joinDate)) / (1000 * 60 * 60 * 24 * 30));
    const totalPurchases = targetCustomer.purchaseHistory.reduce((sum, purchase) => sum + purchase.amount, 0);
    const monthlyValue = membershipMonths > 0 ? totalPurchases / membershipMonths : 0;
    
    // Calculate engagement score
    const engagementScore = calculateEngagementScore(targetCustomer.engagement);
    
    // Predict campaign success probability
    const successProbability = calculateCampaignSuccessProbability(targetCustomer, campaignCriteria);
    
    return {
      ...targetCustomer,
      analysis: {
        customerLifetimeValue: totalPurchases,
        monthlyAverageSpend: Math.round(monthlyValue),
        membershipDuration: membershipMonths,
        engagementScore: engagementScore,
        campaignSuccessProbability: successProbability,
        riskLevel: targetCustomer.behaviorMetrics.returnRate > 0.05 ? 'high' : 
                  targetCustomer.behaviorMetrics.returnRate > 0.03 ? 'medium' : 'low',
        customerSegment: classifyCustomerSegment(targetCustomer),
        recommendedCampaignType: suggestCampaignType(targetCustomer),
        nextBestAction: suggestNextAction(targetCustomer)
      }
    };
  }
  
  return null;
}

function calculateEngagementScore(engagement) {
  const emailScore = engagement.emailOpenRate * 30;
  const clickScore = engagement.clickThroughRate * 40;
  const socialScore = engagement.socialMediaFollower ? 15 : 0;
  const reviewScore = Math.min(engagement.reviewsWritten * 2, 15);
  
  return Math.round(emailScore + clickScore + socialScore + reviewScore);
}

function calculateCampaignSuccessProbability(customer, criteria) {
  let probability = 0.5; // Base probability
  
  // High-value customers more likely to respond
  if (customer.behaviorMetrics.avgOrderValue > 8000) probability += 0.2;
  
  // Frequent purchasers
  if (customer.behaviorMetrics.purchaseFrequency > 2.5) probability += 0.15;
  
  // Low return rate indicates satisfaction
  if (customer.behaviorMetrics.returnRate < 0.05) probability += 0.1;
  
  // High engagement
  const engagementScore = calculateEngagementScore(customer.engagement);
  if (engagementScore > 70) probability += 0.15;
  
  // Premium membership
  if (customer.profile.membershipLevel === 'platinum') probability += 0.1;
  else if (customer.profile.membershipLevel === 'gold') probability += 0.05;
  
  return Math.min(probability, 0.95); // Cap at 95%
}

function classifyCustomerSegment(customer) {
  const clv = customer.purchaseHistory.reduce((sum, purchase) => sum + purchase.amount, 0);
  const engagementScore = calculateEngagementScore(customer.engagement);
  
  if (clv > 20000 && engagementScore > 70) return 'VIP Champions';
  if (clv > 15000 && customer.behaviorMetrics.purchaseFrequency > 2.5) return 'High-Value Frequent';
  if (engagementScore > 70 && customer.behaviorMetrics.referrals > 3) return 'Brand Advocates';
  if (clv > 10000) return 'Premium Customers';
  if (customer.behaviorMetrics.purchaseFrequency > 2) return 'Regular Buyers';
  return 'Occasional Shoppers';
}

function suggestCampaignType(customer) {
  const segment = classifyCustomerSegment(customer);
  const preferredCategories = customer.preferences.categories;
  
  switch (segment) {
    case 'VIP Champions': return 'Exclusive Early Access';
    case 'High-Value Frequent': return 'Personalized Recommendations';
    case 'Brand Advocates': return 'Referral Rewards Program';
    case 'Premium Customers': return 'Premium Product Showcase';
    case 'Regular Buyers': return 'Loyalty Points Multiplier';
    default: return 'Welcome Back Discount';
  }
}

function suggestNextAction(customer) {
  const daysSinceLastPurchase = Math.floor(
    (new Date() - new Date(customer.purchaseHistory[0].date)) / (1000 * 60 * 60 * 24)
  );
  
  if (daysSinceLastPurchase > 30) return 'Re-engagement Campaign';
  if (customer.behaviorMetrics.supportTickets > 1) return 'Customer Satisfaction Survey';
  if (customer.engagement.reviewsWritten < 3) return 'Review Request';
  if (customer.behaviorMetrics.referrals === 0) return 'Referral Program Introduction';
  return 'Product Recommendation';
}

// Usage examples
const premiumCustomer = findTargetCustomer({
  ageRange: { min: 25, max: 40 },
  membershipLevels: ['gold', 'platinum'],
  minAvgOrderValue: 7000,
  minPurchaseFrequency: 2.0,
  maxReturnRate: 0.06,
  targetCategories: ['electronics', 'fashion'],
  minEngagementScore: 60,
  communicationChannel: 'email',
  recentPurchaseRequired: 30,
  minReferrals: 2
});

const loyalCustomer = findTargetCustomer({
  minPurchaseFrequency: 3.0,
  maxReturnRate: 0.03,
  minEngagementScore: 80,
  minReferrals: 5
});

console.log('Premium Customer Found:', premiumCustomer?.customerId);
console.log('Customer Segment:', premiumCustomer?.analysis?.customerSegment);
console.log('Campaign Success Probability:', premiumCustomer?.analysis?.campaignSuccessProbability);
console.log('Recommended Campaign:', premiumCustomer?.analysis?.recommendedCampaignType);
console.log('Next Best Action:', premiumCustomer?.analysis?.nextBestAction);
```

---
# JavaScript Array Methods - Complete Guide with Advanced Examples

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

**Advanced Examples**

**Example 1: Finding User by Complex Condition**
```javascript
const users = [
  { id: 1, name: 'Ali', age: 25, city: 'Karachi', isActive: true },
  { id: 2, name: 'Sara', age: 30, city: 'Lahore', isActive: false },
  { id: 3, name: 'Ahmed', age: 22, city: 'Islamabad', isActive: true },
  { id: 4, name: 'Fatima', age: 28, city: 'Karachi', isActive: true }
];

// Find index of first active user from Karachi above age 24
const userIndex = users.findIndex(user => 
  user.city === 'Karachi' && user.isActive && user.age > 24
);
console.log(userIndex); // 0 (Ali)
console.log(users[userIndex]); // { id: 1, name: 'Ali', age: 25, city: 'Karachi', isActive: true }
```

**Example 2: Finding Index in Nested Array Structure**
```javascript
const orders = [
  { orderId: 101, items: [{ product: 'laptop', price: 50000 }, { product: 'mouse', price: 1500 }] },
  { orderId: 102, items: [{ product: 'phone', price: 30000 }] },
  { orderId: 103, items: [{ product: 'tablet', price: 25000 }, { product: 'keyboard', price: 3000 }] }
];

// Find index of order that has item with price > 40000
const expensiveOrderIndex = orders.findIndex(order => 
  order.items.some(item => item.price > 40000)
);
console.log(expensiveOrderIndex); // 0
console.log(orders[expensiveOrderIndex].orderId); // 101
```

**Example 3: Using with Index Parameter for Pattern Matching**
```javascript
const sequence = [2, 4, 8, 15, 32, 64];

// Find index where number is NOT double of previous number (breaking pattern)
const patternBreakIndex = sequence.findIndex((num, index) => {
  if (index === 0) return false;
  return num !== sequence[index - 1] * 2;
});
console.log(patternBreakIndex); // 3 (15 breaks the doubling pattern)
```

**Example 4: Finding Index Using thisValue Parameter**
```javascript
const products = [
  { name: 'Laptop', price: 50000, category: 'electronics' },
  { name: 'Book', price: 500, category: 'education' },
  { name: 'Phone', price: 30000, category: 'electronics' }
];

const searchCriteria = {
  minPrice: 25000,
  category: 'electronics'
};

// Using thisValue to access external object
const productIndex = products.findIndex(function(product) {
  return product.price >= this.minPrice && product.category === this.category;
}, searchCriteria);

console.log(productIndex); // 0 (Laptop)
```

**Example 5: Complex Date-Based Index Finding**
```javascript
const tasks = [
  { id: 1, title: 'Meeting', dueDate: new Date('2024-01-15'), priority: 'high', completed: false },
  { id: 2, title: 'Report', dueDate: new Date('2024-01-10'), priority: 'medium', completed: true },
  { id: 3, title: 'Review', dueDate: new Date('2024-01-20'), priority: 'high', completed: false },
  { id: 4, title: 'Training', dueDate: new Date('2024-01-08'), priority: 'low', completed: false }
];

const today = new Date('2024-01-12');

// Find index of first overdue high-priority incomplete task
const urgentTaskIndex = tasks.findIndex(task => 
  !task.completed && 
  task.priority === 'high' && 
  task.dueDate < today
);
console.log(urgentTaskIndex); // -1 (no overdue high-priority tasks)

// Find index of first incomplete task due within 7 days
const upcomingTaskIndex = tasks.findIndex(task => {
  if (task.completed) return false;
  const daysUntilDue = (task.dueDate - today) / (1000 * 60 * 60 * 24);
  return daysUntilDue <= 7 && daysUntilDue >= 0;
});
console.log(upcomingTaskIndex); // 0 (Meeting due in 3 days)
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

**Advanced Examples**

**Example 1: Permission System Validation**
```javascript
const userPermissions = ['read', 'write', 'delete'];
const requiredPermissions = ['admin', 'moderator', 'delete'];

// Check if user has ANY of the required permissions
const hasRequiredAccess = requiredPermissions.some(permission => 
  userPermissions.includes(permission)
);
console.log(hasRequiredAccess); // true (user has 'delete' permission)

// Check if any permission is administrative
const hasAdminAccess = userPermissions.some(permission => 
  permission === 'admin' || permission === 'superuser'
);
console.log(hasAdminAccess); // false
```

**Example 2: Complex Object Validation**
```javascript
const students = [
  { name: 'Ali', grades: [85, 90, 78], attendance: 95, hasScholarship: false },
  { name: 'Sara', grades: [92, 88, 95], attendance: 98, hasScholarship: true },
  { name: 'Ahmed', grades: [70, 75, 68], attendance: 80, hasScholarship: false }
];

// Check if any student qualifies for honor roll (avg > 85 AND attendance > 90)
const hasHonorStudent = students.some(student => {
  const avgGrade = student.grades.reduce((sum, grade) => sum + grade, 0) / student.grades.length;
  return avgGrade > 85 && student.attendance > 90;
});
console.log(hasHonorStudent); // true (Sara qualifies)

// Check if any scholarship student is at risk (avg < 80)
const scholarshipAtRisk = students.some(student => {
  if (!student.hasScholarship) return false;
  const avgGrade = student.grades.reduce((sum, grade) => sum + grade, 0) / student.grades.length;
  return avgGrade < 80;
});
console.log(scholarshipAtRisk); // false
```

**Example 3: Network Security Check**
```javascript
const networkConnections = [
  { ip: '192.168.1.100', port: 80, protocol: 'HTTP', isEncrypted: false, attempts: 5 },
  { ip: '10.0.0.50', port: 443, protocol: 'HTTPS', isEncrypted: true, attempts: 2 },
  { ip: '172.16.0.25', port: 22, protocol: 'SSH', isEncrypted: true, attempts: 15 },
  { ip: '192.168.1.200', port: 21, protocol: 'FTP', isEncrypted: false, attempts: 3 }
];

const suspiciousIPs = ['172.16.0.25', '192.168.1.999'];

// Check if any connection has security vulnerabilities
const hasSecurityRisk = networkConnections.some(conn => 
  !conn.isEncrypted || 
  conn.attempts > 10 || 
  suspiciousIPs.includes(conn.ip) ||
  (conn.protocol === 'FTP' && conn.port === 21)
);
console.log(hasSecurityRisk); // true (multiple vulnerabilities found)
```

**Example 4: E-commerce Inventory Check**
```javascript
const products = [
  { id: 1, name: 'Laptop', stock: 5, price: 50000, category: 'electronics', onSale: false },
  { id: 2, name: 'Mouse', stock: 0, price: 1500, category: 'accessories', onSale: true },
  { id: 3, name: 'Keyboard', stock: 12, price: 3000, category: 'accessories', onSale: false }
];

const cart = [
  { productId: 1, quantity: 2 },
  { productId: 2, quantity: 1 },
  { productId: 3, quantity: 5 }
];

// Check if any item in cart is out of stock
const hasOutOfStock = cart.some(cartItem => {
  const product = products.find(p => p.id === cartItem.productId);
  return product && product.stock < cartItem.quantity;
});
console.log(hasOutOfStock); // true (Mouse is out of stock)

// Check if cart qualifies for free shipping (any item > 40000 OR total > 60000)
const qualifiesForFreeShipping = cart.some(cartItem => {
  const product = products.find(p => p.id === cartItem.productId);
  return product && product.price > 40000;
}) || cart.reduce((total, cartItem) => {
  const product = products.find(p => p.id === cartItem.productId);
  return total + (product ? product.price * cartItem.quantity : 0);
}, 0) > 60000;
console.log(qualifiesForFreeShipping); // true (Laptop > 40000)
```

**Example 5: Weather Alert System**
```javascript
const weatherData = [
  { city: 'Karachi', temp: 35, humidity: 80, windSpeed: 15, pressure: 1012, alert: null },
  { city: 'Lahore', temp: 42, humidity: 45, windSpeed: 25, pressure: 1008, alert: null },
  { city: 'Islamabad', temp: 28, humidity: 65, windSpeed: 35, pressure: 995, alert: 'storm' },
  { city: 'Peshawar', temp: 38, humidity: 55, windSpeed: 20, pressure: 1015, alert: null }
];

// Check if any city needs weather warning
const needsWeatherWarning = weatherData.some((data, index, arr) => {
  // Heat warning: temp > 40
  const heatWarning = data.temp > 40;
  
  // Storm warning: windSpeed > 30 OR pressure < 1000
  const stormWarning = data.windSpeed > 30 || data.pressure < 1000;
  
  // High humidity warning: humidity > 75 AND temp > 30
  const humidityWarning = data.humidity > 75 && data.temp > 30;
  
  // Existing alert
  const hasAlert = data.alert !== null;
  
  return heatWarning || stormWarning || humidityWarning || hasAlert;
});
console.log(needsWeatherWarning); // true (multiple cities need warnings)
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

**Advanced Examples**

**Example 1: Form Validation System**
```javascript
const formFields = [
  { name: 'email', value: 'user@example.com', required: true, type: 'email', minLength: 5 },
  { name: 'password', value: 'SecurePass123!', required: true, type: 'password', minLength: 8 },
  { name: 'confirmPassword', value: 'SecurePass123!', required: true, type: 'password', minLength: 8 },
  { name: 'phone', value: '+923001234567', required: true, type: 'phone', minLength: 10 }
];

// Validate all required fields are filled and meet criteria
const isFormValid = formFields.every(field => {
  if (!field.required) return true;
  
  // Check if field has value and meets minimum length
  if (!field.value || field.value.length < field.minLength) return false;
  
  // Type-specific validation
  switch (field.type) {
    case 'email':
      return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(field.value);
    case 'password':
      return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*])/.test(field.value);
    case 'phone':
      return /^\+92[0-9]{10}$/.test(field.value);
    default:
      return true;
  }
});
console.log(isFormValid); // true (all fields valid)

// Check if passwords match
const passwordsMatch = formFields.every(field => {
  if (field.name === 'confirmPassword') {
    const passwordField = formFields.find(f => f.name === 'password');
    return field.value === passwordField.value;
  }
  return true;
});
console.log(passwordsMatch); // true
```

**Example 2: Data Integrity Validation**
```javascript
const transactions = [
  { id: 'T001', amount: 1500, currency: 'PKR', timestamp: 1642780800000, verified: true, hash: 'abc123' },
  { id: 'T002', amount: 2500, currency: 'PKR', timestamp: 1642781200000, verified: true, hash: 'def456' },
  { id: 'T003', amount: 3000, currency: 'PKR', timestamp: 1642781600000, verified: true, hash: 'ghi789' }
];

// Validate all transactions meet business rules
const allTransactionsValid = transactions.every((transaction, index, arr) => {
  // Basic validation
  if (!transaction.id || !transaction.amount || !transaction.hash) return false;
  
  // Amount validation (must be positive and reasonable)
  if (transaction.amount <= 0 || transaction.amount > 100000) return false;
  
  // Currency validation
  if (transaction.currency !== 'PKR') return false;
  
  // Timestamp validation (must be chronological)
  if (index > 0 && transaction.timestamp <= arr[index - 1].timestamp) return false;
  
  // Verification status
  if (!transaction.verified) return false;
  
  // Hash validation (simplified - should be unique)
  const duplicateHash = arr.some((t, i) => i !== index && t.hash === transaction.hash);
  return !duplicateHash;
});
console.log(allTransactionsValid); // true
```

**Example 3: Quality Assurance Testing**
```javascript
const testCases = [
  { id: 'TC001', status: 'passed', executionTime: 120, coverage: 95, criticalBugs: 0 },
  { id: 'TC002', status: 'passed', executionTime: 85, coverage: 88, criticalBugs: 0 },
  { id: 'TC003', status: 'passed', executionTime: 200, coverage: 92, criticalBugs: 0 },
  { id: 'TC004', status: 'passed', executionTime: 150, coverage: 90, criticalBugs: 0 }
];

const qualityStandards = {
  maxExecutionTime: 300,
  minCoverage: 85,
  maxCriticalBugs: 0,
  requiredStatus: 'passed'
};

// Check if all test cases meet quality standards
const meetsQualityStandards = testCases.every(testCase => {
  return testCase.status === qualityStandards.requiredStatus &&
         testCase.executionTime <= qualityStandards.maxExecutionTime &&
         testCase.coverage >= qualityStandards.minCoverage &&
         testCase.criticalBugs <= qualityStandards.maxCriticalBugs;
});
console.log(meetsQualityStandards); // true

// Check if all tests have consistent performance (execution time within 50% of average)
const avgExecutionTime = testCases.reduce((sum, tc) => sum + tc.executionTime, 0) / testCases.length;
const hasConsistentPerformance = testCases.every(testCase => {
  const deviation = Math.abs(testCase.executionTime - avgExecutionTime) / avgExecutionTime;
  return deviation <= 0.5; // Within 50% of average
});
console.log(hasConsistentPerformance); // true
```

**Example 4: Permissions and Access Control**
```javascript
const users = [
  { id: 1, name: 'Admin', roles: ['admin', 'user'], permissions: ['read', 'write', 'delete'], isActive: true, mfaEnabled: true },
  { id: 2, name: 'Editor', roles: ['editor', 'user'], permissions: ['read', 'write'], isActive: true, mfaEnabled: true },
  { id: 3, name: 'Viewer', roles: ['user'], permissions: ['read'], isActive: true, mfaEnabled: false }
];

const securityRequirements = {
  mustBeActive: true,
  requireMFA: false, // Changed to false for this example
  minimumPermissions: ['read'],
  blockedRoles: ['guest', 'suspended']
};

// Check if all users meet security requirements
const allUsersCompliant = users.every(user => {
  // Must be active
  if (securityRequirements.mustBeActive && !user.isActive) return false;
  
  // MFA requirement
  if (securityRequirements.requireMFA && !user.mfaEnabled) return false;
  
  // Must have minimum permissions
  const hasMinPermissions = securityRequirements.minimumPermissions.every(perm => 
    user.permissions.includes(perm)
  );
  if (!hasMinPermissions) return false;
  
  // Must not have blocked roles
  const hasBlockedRoles = user.roles.some(role => 
    securityRequirements.blockedRoles.includes(role)
  );
  if (hasBlockedRoles) return false;
  
  return true;
});
console.log(allUsersCompliant); // true

// Check if all users with admin role have full permissions
const adminUsersCompliant = users.every(user => {
  if (user.roles.includes('admin')) {
    const requiredAdminPerms = ['read', 'write', 'delete'];
    return requiredAdminPerms.every(perm => user.permissions.includes(perm));
  }
  return true;
});
console.log(adminUsersCompliant); // true
```

**Example 5: Distributed System Health Check**
```javascript
const servers = [
  { name: 'web-1', cpu: 65, memory: 78, disk: 45, uptime: 99.9, responseTime: 120, connections: 450 },
  { name: 'web-2', cpu: 72, memory: 82, disk: 52, uptime: 99.8, responseTime: 135, connections: 520 },
  { name: 'db-1', cpu: 58, memory: 85, disk: 68, uptime: 99.95, responseTime: 45, connections: 120 },
  { name: 'cache-1', cpu: 45, memory: 65, disk: 25, uptime: 99.7, responseTime: 15, connections: 200 }
];

const healthThresholds = {
  maxCPU: 80,
  maxMemory: 90,
  maxDisk: 85,
  minUptime: 99.5,
  maxResponseTime: { web: 200, db: 100, cache: 50 },
  maxConnections: { web: 1000, db: 500, cache: 1000 }
};

// Check if all servers are healthy
const allServersHealthy = servers.every(server => {
  // Basic resource checks
  if (server.cpu > healthThresholds.maxCPU) return false;
  if (server.memory > healthThresholds.maxMemory) return false;
  if (server.disk > healthThresholds.maxDisk) return false;
  if (server.uptime < healthThresholds.minUptime) return false;
  
  // Server-type specific checks
  const serverType = server.name.includes('web') ? 'web' : 
                    server.name.includes('db') ? 'db' : 'cache';
  
  if (server.responseTime > healthThresholds.maxResponseTime[serverType]) return false;
  if (server.connections > healthThresholds.maxConnections[serverType]) return false;
  
  return true;
});
console.log(allServersHealthy); // true (all servers within thresholds)
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

**Advanced Examples**

**Example 1: Multi-Level Sorting (Priority, Date, Name)**
```javascript
const tasks = [
  { id: 1, name: 'Fix Bug', priority: 1, dueDate: new Date('2024-01-15'), assignee: 'Ali', status: 'pending' },
  { id: 2, name: 'Code Review', priority: 2, dueDate: new Date('2024-01-15'), assignee: 'Sara', status: 'in-progress' },
  { id: 3, name: 'Deploy Feature', priority: 1, dueDate: new Date('2024-01-10'), assignee: 'Ahmed', status: 'pending' },
  { id: 4, name: 'Write Tests', priority: 2, dueDate: new Date('2024-01-12'), assignee: 'Fatima', status: 'completed' },
  { id: 5, name: 'Update Docs', priority: 3, dueDate: new Date('2024-01-15'), assignee: 'Ali', status: 'pending' }
];

// Sort by priority (ascending), then by due date (ascending), then by name (alphabetically)
const sortedTasks = tasks.sort((a, b) => {
  // First: sort by priority
  if (a.priority !== b.priority) {
    return a.priority - b.priority;
  }
  
  // Second: sort by due date
  if (a.dueDate.getTime() !== b.dueDate.getTime()) {
    return a.dueDate.getTime() - b.dueDate.getTime();
  }
  
  // Third: sort by name alphabetically
  return a.name.localeCompare(b.name);
});

console.log(sortedTasks.map(t => `${t.priority}-${t.dueDate.toDateString()}-${t.name}`));
// Output shows tasks sorted by priority first, then date, then name
```

**Example 2: Custom Object Sorting with Performance Metrics**
```javascript
const employees = [
  { name: 'Ali Khan', department: 'Engineering', salary: 75000, performance: 8.5, yearsExperience: 5, projects: 12 },
  { name: 'Sara Ahmed', department: 'Marketing', salary: 65000, performance: 9.2, yearsExperience: 3, projects: 8 },
  { name: 'Ahmed Ali', department: 'Engineering', salary: 85000, performance: 7.8, yearsExperience: 7, projects: 15 },
  { name: 'Fatima Shah', department: 'Sales', salary: 70000, performance: 9.0, yearsExperience: 4, projects: 10 },
  { name: 'Hassan Sheikh', department: 'Engineering', salary: 80000, performance: 8.8, yearsExperience: 6, projects: 14 }
];

// Sort by performance score (descending), then by years of experience (descending)
const topPerformers = [...employees].sort((a, b) => {
  if (b.performance !== a.performance) {
    return b.performance - a.performance; // Descending performance
  }
  return b.yearsExperience - a.yearsExperience; // Descending experience
});

console.log('Top Performers:');
topPerformers.forEach((emp, index) => {
  console.log(`${index + 1}. ${emp.name} - Performance: ${emp.performance}, Experience: ${emp.yearsExperience} years`);
});

// Sort by department, then by salary (descending within department)
const byDepartmentAndSalary = [...employees].sort((a, b) => {
  if (a.department !== b.department) {
    return a.department.localeCompare(b.department);
  }
  return b.salary - a.salary; // Descending salary within same department
});
```

**Example 3: Complex E-commerce Product Sorting**
```javascript
const products = [
  { id: 1, name: 'iPhone 15', price: 120000, rating: 4.8, reviews: 1250, inStock: true, category: 'Electronics', brand: 'Apple', discount: 0 },
  { id: 2, name: 'Samsung Galaxy S24', price: 110000, rating: 4.6, reviews: 980, inStock: true, category: 'Electronics', brand: 'Samsung', discount: 10 },
  { id: 3, name: 'MacBook Pro', price: 250000, rating: 4.9, reviews: 750, inStock: false, category: 'Electronics', brand: 'Apple', discount: 5 },
  { id: 4, name: 'Dell XPS 13', price: 180000, rating: 4.5, reviews: 680, inStock: true, category: 'Electronics', brand: 'Dell', discount: 15 },
  { id: 5, name: 'AirPods Pro', price: 35000, rating: 4.7, reviews: 2100, inStock: true, category: 'Electronics', brand: 'Apple', discount: 0 }
];

// Smart sorting: In stock first, then by popularity (rating * reviews), then by price
const smartSorted = [...products].sort((a, b) => {
  // Priority 1: In stock items first
  if (a.inStock !== b.inStock) {
    return b.inStock - a.inStock; // true (1) comes before false (0)
  }
  
  // Priority 2: Popularity score (rating * number of reviews)
  const popularityA = a.rating * Math.log(a.reviews + 1); // Log to prevent review count dominance
  const popularityB = b.rating * Math.log(b.reviews + 1);
  
  if (Math.abs(popularityA - popularityB) > 0.5) {
    return popularityB - popularityA; // Higher popularity first
  }
  
  // Priority 3: Price (ascending for similar popularity)
  return a.price - b.price;
});

console.log('Smart Sorted Products:');
smartSorted.forEach(product => {
  const popularity = (product.rating * Math.log(product.reviews + 1)).toFixed(2);
  console.log(`${product.name} - Stock: ${product.inStock}, Popularity: ${popularity}, Price: ${product.price}`);
});
```

**Example 4: Date and Time Sorting with Multiple Criteria**
```javascript
const events = [
  { title: 'Team Meeting', startTime: new Date('2024-01-15T09:00:00'), duration: 60, importance: 'high', attendees: 8 },
  { title: 'Code Review', startTime: new Date('2024-01-15T14:00:00'), duration: 45, importance: 'medium', attendees: 4 },
  { title: 'Client Call', startTime: new Date('2024-01-15T09:00:00'), duration: 30, importance: 'high', attendees: 3 },
  { title: 'Lunch Break', startTime: new Date('2024-01-15T12:00:00'), duration: 60, importance: 'low', attendees: 1 },
  { title: 'Project Planning', startTime: new Date('2024-01-15T10:30:00'), duration: 90, importance: 'high', attendees: 6 }
];

const importanceWeight = { high: 3, medium: 2, low: 1 };

// Sort by start time, but prioritize high importance events at same time
const scheduleSorted = [...events].sort((a, b) => {
  const timeA = a.startTime.getTime();
  const timeB = b.startTime.getTime();
  
  // If same start time, sort by importance
  if (timeA === timeB) {
    return importanceWeight[b.importance] - importanceWeight[a.importance];
  }
  
  // Otherwise sort by time
  return timeA - timeB;
});

console.log('Schedule (sorted by time, importance):');
scheduleSorted.forEach(event => {
  console.log(`${event.startTime.toLocaleTimeString()} - ${event.title} (${event.importance})`);
});

// Smart calendar sorting: Consider conflicts and importance
const calendarOptimized = [...events].sort((a, b) => {
  // Calculate end times
  const endA = new Date(a.startTime.getTime() + a.duration * 60000);
  const endB = new Date(b.startTime.getTime() + b.duration * 60000);
  
  // Priority score: importance + attendee factor
  const scoreA = importanceWeight[a.importance] * (1 + a.attendees * 0.1);
  const scoreB = importanceWeight[b.importance] * (1 + b.attendees * 0.1);
  
  // If events overlap, prioritize by score
  const aOverlapsB = (a.startTime < endB && endA > b.startTime);
  if (aOverlapsB && Math.abs(scoreA - scoreB) > 0.5) {
    return scoreB - scoreA;
  }
  
  // Default: sort by start time
  return a.startTime.getTime() - b.startTime.getTime();
});
```

**Example 5: Geographic and Distance-Based Sorting**
```javascript
const locations = [
  { name: 'Karachi Office', lat: 24.8607, lng: 67.0011, type: 'office', employees: 150, established: 2015 },
  { name: 'Lahore Branch', lat: 31.5204, lng: 74.3587, type: 'branch', employees: 80, established: 2018 },
  { name: 'Islamabad HQ', lat: 33.6844, lng: 73.0479, type: 'headquarters', employees: 200, established: 2010 },
  { name: 'Faisalabad Center', lat: 31.4504, lng: 73.1350, type: 'center', employees: 45, established: 2020 },
  { name: 'Peshawar Office', lat: 34.0151, lng: 71.5249, type: 'office', employees: 60, established: 2019 }
];

const userLocation = { lat: 31.5204, lng: 74.3587 }; // User in Lahore

// Calculate distance using Haversine formula
function calculateDistance(loc1, loc2) {
  const R = 6371; // Earth's radius in kilometers
  const dLat = (loc2.lat - loc1.lat) * Math.PI / 180;
  const dLng = (loc2.lng - loc1.lng) * Math.PI / 180;
  const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
    Math.cos(loc1.lat * Math.PI / 180) * Math.cos(loc2.lat * Math.PI / 180) *
    Math.sin(dLng/2) * Math.sin(dLng/2);
  const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
  return R * c;
}

// Sort by proximity to user, but prioritize by type importance
const typeImportance = { headquarters: 4, office: 3, branch: 2, center: 1 };

const proximitySort = [...locations].sort((a, b) => {
  const distanceA = calculateDistance(userLocation, a);
  const distanceB = calculateDistance(userLocation, b);
  
  // If distances are very close (within 50km), sort by type importance
  if (Math.abs(distanceA - distanceB) < 50) {
    if (typeImportance[a.type] !== typeImportance[b.type]) {
      return typeImportance[b.type] - typeImportance[a.type];
    }
    // Then by employee count (larger offices first)
    return b.employees - a.employees;
  }
  
  // Otherwise sort by distance
  return distanceA - distanceB;
});

console.log('Locations sorted by proximity and importance:');
proximitySort.forEach(location => {
  const distance = calculateDistance(userLocation, location).toFixed(1);
  console.log(`${location.name} - ${distance}km away, ${location.type}, ${location.employees} employees`);
});
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

**Advanced Examples**

**Example 1: Reversing Complex Data Structures**
```javascript
const chatMessages = [
  { id: 1, user: 'Ali', message: 'Hello everyone!', timestamp: '09:00:00', type: 'text' },
  { id: 2, user: 'Sara', message: 'Hi Ali! How are you?', timestamp: '09:02:00', type: 'text' },
  { id: 3, user: 'Ahmed', message: 'Good morning!', timestamp: '09:05:00', type: 'text' },
  { id: 4, user: 'System', message: 'Fatima joined the chat', timestamp: '09:07:00', type: 'system' },
  { id: 5, user: 'Fatima', message: 'Thanks for the warm welcome!', timestamp: '09:08:00', type: 'text' }
];

// Display messages in reverse chronological order (newest first) - common in chat apps
const recentMessages = [...chatMessages].reverse();
console.log('Recent Messages (newest first):');
recentMessages.forEach(msg => {
  console.log(`[${msg.timestamp}] ${msg.user}: ${msg.message}`);
});

// Reverse only user messages, keeping system messages in place
const messagesReversed = [...chatMessages];
const userMessages = messagesReversed.filter(msg => msg.type === 'text');
const systemMessages = messagesReversed.filter(msg => msg.type === 'system');

userMessages.reverse();

// Reconstruct array maintaining system message positions
const reconstructed = chatMessages.map(originalMsg => {
  if (originalMsg.type === 'system') {
    return originalMsg;
  } else {
    return userMessages.shift();
  }
});
```

**Example 2: Palindrome and Pattern Analysis**
```javascript
const dnaSequence = ['A', 'T', 'G', 'C', 'G', 'T', 'A'];
const userInputs = ['hello', 'world', 'level', 'radar', 'python', 'madam'];
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];

// Check if DNA sequence is palindromic
function isPalindromic(sequence) {
  const reversed = [...sequence].reverse();
  return sequence.every((base, index) => {
    // DNA complement rules: A-T, G-C
    const complement = { 'A': 'T', 'T': 'A', 'G': 'C', 'C': 'G' };
    return complement[base] === reversed[index];
  });
}

console.log('Is DNA sequence palindromic?', isPalindromic(dnaSequence)); // false

// Find palindromes in user inputs
const palindromes = userInputs.filter(word => {
  const reversed = word.split('').reverse().join('');
  return word.toLowerCase() === reversed.toLowerCase();
});
console.log('Palindromes found:', palindromes); // ['level', 'radar', 'madam']

// Create mirror pattern
const mirrorPattern = [...numbers, ...numbers.slice(0, -1).reverse()];
console.log('Mirror pattern:', mirrorPattern); // [1,2,3,4,5,6,7,8,9,8,7,6,5,4,3,2,1]

// Analyze symmetry in data
function analyzeSymmetry(data) {
  const midpoint = Math.floor(data.length / 2);
  const firstHalf = data.slice(0, midpoint);
  const secondHalf = data.slice(-midpoint).reverse();
  
  const isSymmetric = firstHalf.every((item, index) => item === secondHalf[index]);
  return { isSymmetric, firstHalf, secondHalf, midpoint };
}

const symmetryAnalysis = analyzeSymmetry([1, 2, 3, 2, 1]);
console.log('Symmetry analysis:', symmetryAnalysis);
```

**Example 3: Undo/Redo System Implementation**
```javascript
class UndoRedoManager {
  constructor() {
    this.history = [];
    this.currentIndex = -1;
    this.maxHistorySize = 50;
  }
  
  executeCommand(command, data) {
    // Remove any redo history when new command is executed
    this.history = this.history.slice(0, this.currentIndex + 1);
    
    // Add new command to history
    this.history.push({ command, data, timestamp: Date.now() });
    this.currentIndex++;
    
    // Limit history size
    if (this.history.length > this.maxHistorySize) {
      this.history.shift();
      this.currentIndex--;
    }
    
    console.log(`Executed: ${command}`, data);
    return this.getCurrentState();
  }
  
  undo() {
    if (this.currentIndex > 0) {
      this.currentIndex--;
      const previousCommand = this.history[this.currentIndex];
      console.log(`Undone: ${this.history[this.currentIndex + 1].command}`);
      return previousCommand;
    }
    console.log('Nothing to undo');
    return null;
  }
  
  redo() {
    if (this.currentIndex < this.history.length - 1) {
      this.currentIndex++;
      const nextCommand = this.history[this.currentIndex];
      console.log(`Redone: ${nextCommand.command}`);
      return nextCommand;
    }
    console.log('Nothing to redo');
    return null;
  }
  
  getHistoryReversed() {
    // Get history in reverse order (most recent first)
    return [...this.history].reverse();
  }
  
  getCurrentState() {
    return this.currentIndex >= 0 ? this.history[this.currentIndex] : null;
  }
}

// Usage example
const undoManager = new UndoRedoManager();
undoManager.executeCommand('CREATE_DOCUMENT', { name: 'Report.docx' });
undoManager.executeCommand('ADD_TEXT', { text: 'Hello World' });
undoManager.executeCommand('FORMAT_TEXT', { style: 'bold' });
undoManager.executeCommand('ADD_IMAGE', { src: 'chart.png' });

console.log('\nHistory (newest first):');
undoManager.getHistoryReversed().forEach((item, index) => {
  console.log(`${index + 1}. ${item.command} - ${new Date(item.timestamp).toLocaleTimeString()}`);
});

// Undo last two operations
undoManager.undo(); // Undo ADD_IMAGE
undoManager.undo(); // Undo FORMAT_TEXT
undoManager.redo(); // Redo FORMAT_TEXT
```

**Example 4: Data Stream Processing (LIFO - Last In, First Out)**
```javascript
const stockPrices = [
  { symbol: 'KTL', price: 150, timestamp: '09:00:00', volume: 1000 },
  { symbol: 'KTL', price: 152, timestamp: '09:15:00', volume: 1200 },
  { symbol: 'KTL', price: 148, timestamp: '09:30:00', volume: 800 },
  { symbol: 'KTL', price: 155, timestamp: '09:45:00', volume: 1500 },
  { symbol: 'KTL', price: 153, timestamp: '10:00:00', volume: 1100 }
];

class TradingAnalyzer {
  constructor() {
    this.priceHistory = [];
    this.alerts = [];
  }
  
  addPrice(priceData) {
    this.priceHistory.push(priceData);
    this.checkForAlerts(priceData);
  }
  
  checkForAlerts(currentPrice) {
    if (this.priceHistory.length < 2) return;
    
    // Get recent prices (reversed to check latest first)
    const recentPrices = [...this.priceHistory].reverse();
    
    // Check for rapid price movement (5% in last 2 readings)
    if (recentPrices.length >= 2) {
      const change = Math.abs(recentPrices[0].price - recentPrices[1].price);
      const percentChange = (change / recentPrices[1].price) * 100;
      
      if (percentChange > 5) {
        this.alerts.push({
          type: 'RAPID_MOVEMENT',
          message: `Rapid ${percentChange.toFixed(1)}% change detected`,
          timestamp: currentPrice.timestamp
        });
      }
    }
    
    // Check for trend reversal (3 consecutive increases/decreases)
    if (recentPrices.length >= 3) {
      const trend = this.detectTrend(recentPrices.slice(0, 3).reverse());
      if (trend) {
        this.alerts.push({
          type: 'TREND_DETECTED',
          message: `${trend} trend detected`,
          timestamp: currentPrice.timestamp
        });
      }
    }
  }
  
  detectTrend(prices) {
    if (prices.length < 3) return null;
    
    const isIncreasing = prices.every((price, index) => 
      index === 0 || price.price > prices[index - 1].price
    );
    
    const isDecreasing = prices.every((price, index) => 
      index === 0 || price.price < prices[index - 1].price
    );
    
    if (isIncreasing) return 'UPWARD';
    if (isDecreasing) return 'DOWNWARD';
    return null;
  }
  
  getRecentAlerts(count = 5) {
    return [...this.alerts].reverse().slice(0, count);
  }
  
  getLatestPrices(count = 10) {
    return [...this.priceHistory].reverse().slice(0, count);
  }
}

const analyzer = new TradingAnalyzer();
stockPrices.forEach(price => analyzer.addPrice(price));

console.log('Latest Prices (newest first):');
analyzer.getLatestPrices(3).forEach(price => {
  console.log(`[${price.timestamp}] ${price.symbol}: ${price.price} (Vol: ${price.volume})`);
});

console.log('\nRecent Alerts:');
analyzer.getRecentAlerts().forEach(alert => {
  console.log(`[${alert.timestamp}] ${alert.type}: ${alert.message}`);
});
```

**Example 5: Breadcrumb Navigation System**
```javascript
class NavigationManager {
  constructor() {
    this.navigationHistory = [];
    this.maxHistorySize = 20;
  }
  
  navigateTo(page) {
    // Add current page to history
    this.navigationHistory.push({
      page: page.name,
      url: page.url,
      params: page.params || {},
      timestamp: Date.now(),
      title: page.title
    });
    
    // Limit history size
    if (this.navigationHistory.length > this.maxHistorySize) {
      this.navigationHistory.shift();
    }
    
    console.log(`Navigated to: ${page.name}`);
    this.updateBreadcrumbs();
  }
  
  goBack(steps = 1) {
    if (this.navigationHistory.length <= steps) {
      console.log('Cannot go back that many steps');
      return null;
    }
    
    // Remove current and previous steps
    const removed = this.navigationHistory.splice(-steps);
    const currentPage = this.navigationHistory[this.navigationHistory.length - 1];
    
    console.log(`Went back ${steps} step(s) to: ${currentPage.page}`);
    this.updateBreadcrumbs();
    return currentPage;
  }
  
  getBreadcrumbs() {
    // Return navigation path (oldest to newest)
    return this.navigationHistory.map(item => ({
      name: item.page,
      title: item.title,
      url: item.url
    }));
  }
  
  getReverseBreadcrumbs() {
    // Return navigation path (newest to oldest) - useful for "Recent Pages"
    return [...this.navigationHistory].reverse().map(item => ({
      name: item.page,
      title: item.title,
      url: item.url,
      timestamp: item.timestamp
    }));
  }
  
  updateBreadcrumbs() {
    const breadcrumbs = this.getBreadcrumbs();
    const reverseBreadcrumbs = this.getReverseBreadcrumbs().slice(0, 5);
    
    console.log('Breadcrumb Path:', breadcrumbs.map(b => b.name).join(' > '));
    console.log('Recent Pages:', reverseBreadcrumbs.map(b => b.name).join(', '));
  }
  
  findInHistory(pageName) {
    // Search history in reverse order (find most recent occurrence first)
    const reversedHistory = [...this.navigationHistory].reverse();
    const found = reversedHistory.find(item => item.page === pageName);
    return found || null;
  }
  
  getNavigationStats() {
    const reversedHistory = [...this.navigationHistory].reverse();
    const uniquePages = [...new Set(reversedHistory.map(item => item.page))];
    const mostRecentUniquePages = [];
    
    // Get most recent visit for each unique page
    uniquePages.forEach(pageName => {
      const mostRecent = reversedHistory.find(item => item.page === pageName);
      if (mostRecent) {
        mostRecentUniquePages.push(mostRecent);
      }
    });
    
    return {
      totalVisits: this.navigationHistory.length,
      uniquePages: uniquePages.length,
      mostRecentUniquePages: mostRecentUniquePages.slice(0, 10)
    };
  }
}

// Usage example
const navManager = new NavigationManager();

// Simulate user navigation
navManager.navigateTo({ name: 'Home', url: '/', title: 'Welcome Home' });
navManager.navigateTo({ name: 'Products', url: '/products', title: 'Our Products' });
navManager.navigateTo({ name: 'Laptop', url: '/products/laptop', title: 'Laptops' });
navManager.navigateTo({ name: 'Gaming Laptop', url: '/products/laptop/gaming', title: 'Gaming Laptops' });
navManager.navigateTo({ name: 'Product Details', url: '/products/laptop/gaming/asus-rog', title: 'ASUS ROG Laptop' });

console.log('\n--- Navigation Stats ---');
const stats = navManager.getNavigationStats();
console.log(`Total visits: ${stats.totalVisits}`);
console.log(`Unique pages: ${stats.uniquePages}`);

// Go back 2 steps
navManager.goBack(2);

// Navigate to a new page
navManager.navigateTo({ name: 'Cart', url: '/cart', title: 'Shopping Cart' });
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

**Advanced Examples**

**Example 1: Data Pagination and Chunking**
```javascript
const products = [
  { id: 1, name: 'Laptop', price: 50000 }, { id: 2, name: 'Phone', price: 30000 },
  { id: 3, name: 'Tablet', price: 25000 }, { id: 4, name: 'Watch', price: 15000 },
  { id: 5, name: 'Headphones', price: 8000 }, { id: 6, name: 'Mouse', price: 2000 },
  { id: 7, name: 'Keyboard', price: 5000 }, { id: 8, name: 'Monitor', price: 35000 },
  { id: 9, name: 'Printer', price: 20000 }, { id: 10, name: 'Scanner', price: 12000 },
  { id: 11, name: 'Webcam', price: 6000 }, { id: 12, name: 'Speaker', price: 10000 }
];

class PaginationManager {
  constructor(data, itemsPerPage = 5) {
    this.data = data;
    this.itemsPerPage = itemsPerPage;
    this.currentPage = 1;
    this.totalPages = Math.ceil(data.length / itemsPerPage);
  }
  
  getCurrentPage() {
    const startIndex = (this.currentPage - 1) * this.itemsPerPage;
    const endIndex = startIndex + this.itemsPerPage;
    return this.data.slice(startIndex, endIndex);
  }
  
  getPage(pageNumber) {
    if (pageNumber < 1 || pageNumber > this.totalPages) {
      return null;
    }
    
    const startIndex = (pageNumber - 1) * this.itemsPerPage;
    const endIndex = startIndex + this.itemsPerPage;
    return this.data.slice(startIndex, endIndex);
  }
  
  nextPage() {
    if (this.currentPage < this.totalPages) {
      this.currentPage++;
      return this.getCurrentPage();
    }
    return null;
  }
  
  previousPage() {
    if (this.currentPage > 1) {
      this.currentPage--;
      return this.getCurrentPage();
    }
    return null;
  }
  
  getPageRange(startPage, endPage) {
    const results = [];
    for (let page = startPage; page <= endPage && page <= this.totalPages; page++) {
      results.push(...this.getPage(page));
    }
    return results;
  }
  
  getLastNPages(n) {
    const startPage = Math.max(1, this.totalPages - n + 1);
    return this.getPageRange(startPage, this.totalPages);
  }
}

const paginator = new PaginationManager(products, 4);
console.log('Page 1:', paginator.getCurrentPage().map(p => p.name));
console.log('Page 2:', paginator.getPage(2).map(p => p.name));
console.log('Last 2 pages:', paginator.getLastNPages(2).map(p => p.name));
```

**Example 2: Time-Based Data Slicing**
```javascript
const salesData = [
  { date: '2024-01-01', sales: 12000, region: 'North' },
  { date: '2024-01-02', sales: 15000, region: 'South' },
  { date: '2024-01-03', sales: 8000, region: 'East' },
  { date: '2024-01-04', sales: 18000, region: 'West' },
  { date: '2024-01-05', sales: 14000, region: 'North' },
  { date: '2024-01-06', sales: 22000, region: 'South' },
  { date: '2024-01-07', sales: 16000, region: 'East' },
  { date: '2024-01-08', sales: 19000, region: 'West' },
  { date: '2024-01-09', sales: 13000, region: 'North' },
  { date: '2024-01-10', sales: 25000, region: 'South' }
];

class TimeSeriesAnalyzer {
  constructor(data) {
    this.data = data.sort((a, b) => new Date(a.date) - new Date(b.date));
  }
  
  getRecentDays(days) {
    return this.data.slice(-days);
  }
  
  getDateRange(startDate, endDate) {
    const startIdx = this.data.findIndex(item => item.date >= startDate);
    const endIdx = this.data.findIndex(item => item.date > endDate);
    
    if (startIdx === -1) return [];
    if (endIdx === -1) return this.data.slice(startIdx);
    
    return this.data.slice(startIdx, endIdx);
  }
  
  getMovingWindow(windowSize) {
    const windows = [];
    for (let i = 0; i <= this.data.length - windowSize; i++) {
      const window = this.data.slice(i, i + windowSize);
      const avgSales = window.reduce((sum, item) => sum + item.sales, 0) / windowSize;
      windows.push({
        startDate: window[0].date,
        endDate: window[window.length - 1].date,
        data: window,
        averageSales: avgSales
      });
    }
    return windows;
  }
  
  getWeeklySlices() {
    const weeks = [];
    for (let i = 0; i < this.data.length; i += 7) {
      const weekData = this.data.slice(i, i + 7);
      if (weekData.length > 0) {
        weeks.push({
          week: Math.floor(i / 7) + 1,
          startDate: weekData[0].date,
          endDate: weekData[weekData.length - 1].date,
          data: weekData,
          totalSales: weekData.reduce((sum, item) => sum + item.sales, 0)
        });
      }
    }
    return weeks;
  }
  
  getTopPerformingDays(count) {
    const sortedData = [...this.data].sort((a, b) => b.sales - a.sales);
    return sortedData.slice(0, count);
  }
}

const analyzer = new TimeSeriesAnalyzer(salesData);
console.log('Last 3 days:', analyzer.getRecentDays(3));
console.log('Jan 3-6 range:', analyzer.getDateRange('2024-01-03', '2024-01-06'));
console.log('3-day moving averages:', analyzer.getMovingWindow(3).map(w => 
  `${w.startDate} to ${w.endDate}: ${w.averageSales.toFixed(0)}`
));
```

**Example 3: Array Manipulation for Data Processing**
```javascript
const logEntries = [
  'ERROR 09:15:23 Database connection failed',
  'INFO 09:15:45 User login successful',
  'WARNING 09:16:12 Memory usage high',
  'ERROR 09:16:30 API timeout',
  'INFO 09:17:01 Cache cleared',
  'ERROR 09:17:22 File not found',
  'INFO 09:17:45 Backup completed',
  'WARNING 09:18:10 Disk space low',
  'ERROR 09:18:33 Network unreachable',
  'INFO 09:19:05 System restart initiated'
];

class LogAnalyzer {
  constructor(logs) {
    this.logs = logs.map(log => {
      const [level, time, ...messageParts] = log.split(' ');
      return {
        level,
        time,
        message: messageParts.join(' '),
        timestamp: this.parseTime(time)
      };
    });
  }
  
  parseTime(timeStr) {
    const [hours, minutes, seconds] = timeStr.split(':').map(Number);
    return hours * 3600 + minutes * 60 + seconds;
  }
  
  getRecentLogs(count) {
    return this.logs.slice(-count);
  }
  
  getLogsByLevel(level) {
    return this.logs.filter(log => log.level === level);
  }
  
  getLogsInTimeRange(startTime, endTime) {
    const startTimestamp = this.parseTime(startTime);
    const endTimestamp = this.parseTime(endTime);
    
    return this.logs.filter(log => 
      log.timestamp >= startTimestamp && log.timestamp <= endTimestamp
    );
  }
  
  getErrorSequences(windowSize = 3) {
    const sequences = [];
    for (let i = 0; i <= this.logs.length - windowSize; i++) {
      const window = this.logs.slice(i, i + windowSize);
      const errorCount = window.filter(log => log.level === 'ERROR').length;
      
      if (errorCount >= 2) {
        sequences.push({
          startIndex: i,
          endIndex: i + windowSize - 1,
          logs: window,
          errorCount
        });
      }
    }
    return sequences;
  }
  
  getSummaryByTimeSlices(sliceMinutes = 2) {
    const slices = [];
    const firstTimestamp = this.logs[0]?.timestamp || 0;
    const lastTimestamp = this.logs[this.logs.length - 1]?.timestamp || 0;
    const sliceSeconds = sliceMinutes * 60;
    
    for (let start = firstTimestamp; start <= lastTimestamp; start += sliceSeconds) {
      const end = start + sliceSeconds;
      const sliceLogs = this.logs.filter(log => 
        log.timestamp >= start && log.timestamp < end
      );
      
      if (sliceLogs.length > 0) {
        slices.push({
          startTime: this.formatTime(start),
          endTime: this.formatTime(end),
          logs: sliceLogs,
          summary: this.getSummary(sliceLogs)
        });
      }
    }
    return slices;
  }
  
  formatTime(timestamp) {
    const hours = Math.floor(timestamp / 3600);
    const minutes = Math.floor((timestamp % 3600) / 60);
    const seconds = timestamp % 60;
    return `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
  }
  
  getSummary(logs) {
    const summary = { ERROR: 0, WARNING: 0, INFO: 0 };
    logs.forEach(log => summary[log.level]++);
    return summary;
  }
}

const logAnalyzer = new LogAnalyzer(logEntries);
console.log('Recent 3 logs:', logAnalyzer.getRecentLogs(3).map(l => `${l.level}: ${l.message}`));
console.log('Error sequences:', logAnalyzer.getErrorSequences(4).length);
console.log('Time slice summaries:', logAnalyzer.getSummaryByTimeSlices(2).map(s => 
  `${s.startTime}-${s.endTime}: ${s.summary.ERROR} errors, ${s.summary.WARNING} warnings`
));
```

**Example 4: Advanced String and Text Processing**
```javascript
const documentText = [
  'The', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy', 'dog',
  'in', 'the', 'forest', 'near', 'the', 'river', 'where', 'many', 'animals',
  'live', 'peacefully', 'together', 'in', 'harmony', 'with', 'nature'
];

const sentences = [
  'Artificial intelligence is transforming the world.',
  'Machine learning algorithms can process vast amounts of data.',
  'Natural language processing enables computers to understand human language.',
  'Computer vision allows machines to interpret visual information.',
  'Robotics combines AI with physical automation.'
];

class TextAnalyzer {
  constructor(words) {
    this.words = words;
  }
  
  getWordSlice(start, length) {
    return this.words.slice(start, start + length).join(' ');
  }
  
  extractNGrams(n) {
    const ngrams = [];
    for (let i = 0; i <= this.words.length - n; i++) {
      ngrams.push(this.words.slice(i, i + n));
    }
    return ngrams;
  }
  
  findPatterns(patternLength) {
    const patterns = new Map();
    const ngrams = this.extractNGrams(patternLength);
    
    ngrams.forEach((gram, index) => {
      const pattern = gram.join(' ');
      if (!patterns.has(pattern)) {
        patterns.set(pattern, []);
      }
      patterns.get(pattern).push(index);
    });
    
    // Return only patterns that appear more than once
    return Array.from(patterns.entries())
      .filter(([pattern, positions]) => positions.length > 1)
      .map(([pattern, positions]) => ({
        pattern,
        frequency: positions.length,
        positions
      }));
  }
  
  getContextWindow(wordIndex, windowSize = 2) {
    const start = Math.max(0, wordIndex - windowSize);
    const end = Math.min(this.words.length, wordIndex + windowSize + 1);
    
    return {
      context: this.words.slice(start, end),
      wordPosition: wordIndex - start,
      fullContext: this.words.slice(start, end).join(' ')
    };
  }
  
  extractKeyPhrases(minLength = 2, maxLength = 4) {
    const phrases = [];
    
    for (let length = minLength; length <= maxLength; length++) {
      for (let i = 0; i <= this.words.length - length; i++) {
        const phrase = this.words.slice(i, i + length);
        const phraseText = phrase.join(' ');
        
        // Simple heuristic: avoid phrases with too many common words
        const commonWords = ['the', 'a', 'an', 'and', 'or', 'but', 'in', 'on', 'at'];
        const commonWordCount = phrase.filter(word => 
          commonWords.includes(word.toLowerCase())
        ).length;
        
        if (commonWordCount < phrase.length * 0.6) {
          phrases.push({
            phrase: phraseText,
            words: phrase,
            startIndex: i,
            length: length
          });
        }
      }
    }
    
    return phrases;
  }
  
  getSlidingWindowAnalysis(windowSize) {
    const windows = [];
    for (let i = 0; i <= this.words.length - windowSize; i++) {
      const window = this.words.slice(i, i + windowSize);
      windows.push({
        startIndex: i,
        window: window,
        text: window.join(' '),
        wordCount: window.length,
        uniqueWords: [...new Set(window)].length,
        avgWordLength: window.reduce((sum, word) => sum + word.length, 0) / window.length
      });
    }
    return windows;
  }
}

const textAnalyzer = new TextAnalyzer(documentText);
console.log('3-grams with patterns:');
textAnalyzer.findPatterns(3).forEach(p => 
  console.log(`"${p.pattern}" appears ${p.frequency} times at positions: ${p.positions.join(', ')}`)
);

console.log('\nContext window for word "the" (first occurrence):');
const theIndex = documentText.indexOf('the');
const context = textAnalyzer.getContextWindow(theIndex, 3);
console.log(`Context: "${context.fullContext}"`);

console.log('\nKey phrases (2-3 words):');
textAnalyzer.extractKeyPhrases(2, 3).slice(0, 5).forEach(kp => 
  console.log(`"${kp.phrase}" (${kp.length} words, starts at ${kp.startIndex})`)
);
```

**Example 5: Financial Data Windowing and Analysis**
```javascript
const stockPrices = [
  { date: '2024-01-01', open: 100, high: 105, low: 98, close: 103, volume: 10000 },
  { date: '2024-01-02', open: 103, high: 108, low: 101, close: 106, volume: 12000 },
  { date: '2024-01-03', open: 106, high: 110, low: 104, close: 107, volume: 8000 },
  { date: '2024-01-04', open: 107, high: 109, low: 105, close: 105, volume: 9000 },
  { date: '2024-01-05', open: 105, high: 107, low: 102, close: 104, volume: 11000 },
  { date: '2024-01-08', open: 104, high: 106, low: 100, close: 101, volume: 15000 },
  { date: '2024-01-09', open: 101, high: 103, low: 99, close: 102, volume: 13000 },
  { date: '2024-01-10', open: 102, high: 105, low: 100, close: 104, volume: 10500 }
];

class TechnicalAnalyzer {
  constructor(data) {
    this.data = data;
  }
  
  calculateSMA(period) {
    if (period > this.data.length) return [];
    
    const smaValues = [];
    for (let i = period - 1; i < this.data.length; i++) {
      const window = this.data.slice(i - period + 1, i + 1);
      const average = window.reduce((sum, candle) => sum + candle.close, 0) / period;
      smaValues.push({
        date: this.data[i].date,
        sma: average,
        period: period
      });
    }
    return smaValues;
  }
  
  calculateEMA(period, smoothingFactor = 2) {
    if (period > this.data.length) return [];
    
    const multiplier = smoothingFactor / (period + 1);
    const emaValues = [];
    
    // Start with SMA for first value
    const firstSMA = this.data.slice(0, period)
      .reduce((sum, candle) => sum + candle.close, 0) / period;
    
    emaValues.push({
      date: this.data[period - 1].date,
      ema: firstSMA,
      period: period
    });
    
    // Calculate EMA for remaining values
    for (let i = period; i < this.data.length; i++) {
      const previousEMA = emaValues[emaValues.length - 1].ema;
      const currentPrice = this.data[i].close;
      const ema = (currentPrice * multiplier) + (previousEMA * (1 - multiplier));
      
      emaValues.push({
        date: this.data[i].date,
        ema: ema,
        period: period
      });
    }
    
    return emaValues;
  }
  
  findSupportResistanceLevels(lookbackPeriod = 3) {
    const levels = { support: [], resistance: [] };
    
    for (let i = lookbackPeriod; i < this.data.length - lookbackPeriod; i++) {
      const window = this.data.slice(i - lookbackPeriod, i + lookbackPeriod + 1);
      const current = this.data[i];
      
      // Check if current low is the lowest in the window (support)
      const isSupport = window.every(candle => current.low <= candle.low);
      if (isSupport) {
        levels.support.push({
          date: current.date,
          price: current.low,
          type: 'support'
        });
      }
      
      // Check if current high is the highest in the window (resistance)
      const isResistance = window.every(candle => current.high >= candle.high);
      if (isResistance) {
        levels.resistance.push({
          date: current.date,
          price: current.high,
          type: 'resistance'
        });
      }
    }
    
    return levels;
  }
  
  detectPatterns(patternSize = 3) {
    const patterns = [];
    
    for (let i = 0; i <= this.data.length - patternSize; i++) {
      const window = this.data.slice(i, i + patternSize);
      const pattern = this.analyzePattern(window);
      
      if (pattern.type !== 'neutral') {
        patterns.push({
          startDate: window[0].date,
          endDate: window[window.length - 1].date,
          data: window,
          pattern: pattern,
          strength: pattern.strength
        });
      }
    }
    
    return patterns;
  }
  
  analyzePattern(candles) {
    const closes = candles.map(c => c.close);
    const changes = [];
    
    for (let i = 1; i < closes.length; i++) {
      changes.push(closes[i] - closes[i - 1]);
    }
    
    const positiveChanges = changes.filter(c => c > 0).length;
    const negativeChanges = changes.filter(c => c < 0).length;
    const totalChange = closes[closes.length - 1] - closes[0];
    const avgChange = totalChange / (closes.length - 1);
    
    let type = 'neutral';
    let strength = 0;
    
    if (positiveChanges > negativeChanges && totalChange > 0) {
      type = 'bullish';
      strength = (positiveChanges / changes.length) * Math.abs(avgChange);
    } else if (negativeChanges > positiveChanges && totalChange < 0) {
      type = 'bearish';
      strength = (negativeChanges / changes.length) * Math.abs(avgChange);
    }
    
    return { type, strength, totalChange, avgChange };
  }
  
  getVolumeProfile(priceRanges = 5) {
    const minPrice = Math.min(...this.data.map(d => d.low));
    const maxPrice = Math.max(...this.data.map(d => d.high));
    const rangeSize = (maxPrice - minPrice) / priceRanges;
    
    const profile = Array(priceRanges).fill(0).map((_, i) => ({
      priceRange: {
        min: minPrice + (i * rangeSize),
        max: minPrice + ((i + 1) * rangeSize)
      },
      volume: 0,
      transactions: 0
    }));
    
    this.data.forEach(candle => {
      const avgPrice = (candle.high + candle.low + candle.close) / 3;
      const rangeIndex = Math.min(
        Math.floor((avgPrice - minPrice) / rangeSize),
        priceRanges - 1
      );
      
      profile[rangeIndex].volume += candle.volume;
      profile[rangeIndex].transactions += 1;
    });
    
    return profile;
  }
}

const analyzer = new TechnicalAnalyzer(stockPrices);

console.log('5-period SMA:');
analyzer.calculateSMA(5).forEach(sma => 
  console.log(`${sma.date}: ${sma.sma.toFixed(2)}`)
);

console.log('\nSupport/Resistance Levels:');
const levels = analyzer.findSupportResistanceLevels(2);
[...levels.support, ...levels.resistance].forEach(level => 
  console.log(`${level.date}: ${level.price} (${level.type})`)
);

console.log('\nBullish/Bearish Patterns:');
analyzer.detectPatterns(3).forEach(pattern => 
  console.log(`${pattern.startDate} to ${pattern.endDate}: ${pattern.pattern.type} (strength: ${pattern.pattern.strength.toFixed(2)})`)
);
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

**Advanced Examples**

**Example 1: Dynamic List Management System**
```javascript
class TaskManager {
  constructor() {
    this.tasks = [
      { id: 1, title: 'Review code', priority: 2, status: 'pending', assignee: 'Ali' },
      { id: 2, title: 'Fix bug #123', priority: 1, status: 'in-progress', assignee: 'Sara' },
      { id: 3, title: 'Update docs', priority: 3, status: 'pending', assignee: 'Ahmed' },
      { id: 4, title: 'Deploy to staging', priority: 1, status: 'pending', assignee: 'Fatima' }
    ];
    this.nextId = 5;
  }
  
  addTask(title, priority = 3, assignee = 'Unassigned') {
    const newTask = {
      id: this.nextId++,
      title,
      priority,
      status: 'pending',
      assignee
    };
    
    // Insert task based on priority (higher priority = lower number)
    const insertIndex = this.tasks.findIndex(task => task.priority > priority);
    if (insertIndex === -1) {
      this.tasks.splice(this.tasks.length, 0, newTask);
    } else {
      this.tasks.splice(insertIndex, 0, newTask);
    }
    
    console.log(`Added task: "${title}" at position ${insertIndex === -1 ? this.tasks.length - 1 : insertIndex}`);
    return newTask;
  }
  
  removeTask(taskId) {
    const taskIndex = this.tasks.findIndex(task => task.id === taskId);
    if (taskIndex !== -1) {
      const removed = this.tasks.splice(taskIndex, 1);
      console.log(`Removed task: "${removed[0].title}"`);
      return removed[0];
    }
    return null;
  }
  
  updateTaskStatus(taskId, newStatus) {
    const taskIndex = this.tasks.findIndex(task => task.id === taskId);
    if (taskIndex !== -1) {
      const oldTask = { ...this.tasks[taskIndex] };
      this.tasks[taskIndex].status = newStatus;
      
      // If task is completed, move to end
      if (newStatus === 'completed') {
        const completedTask = this.tasks.splice(taskIndex, 1)[0];
        this.tasks.splice(this.tasks.length, 0, completedTask);
        console.log(`Moved completed task "${completedTask.title}" to end`);
      }
      
      return { old: oldTask, new: this.tasks[taskIndex] };
    }
    return null;
  }
  
  reorganizeByPriority() {
    // Remove all tasks and re-insert them sorted by priority
    const allTasks = this.tasks.splice(0, this.tasks.length);
    const sortedTasks = allTasks.sort((a, b) => {
      if (a.status === 'completed' && b.status !== 'completed') return 1;
      if (b.status === 'completed' && a.status !== 'completed') return -1;
      return a.priority - b.priority;
    });
    
    this.tasks.splice(0, 0, ...sortedTasks);
    console.log('Tasks reorganized by priority');
    return this.tasks;
  }
  
  bulkUpdate(updates) {
    const results = [];
    updates.forEach(({ taskId, changes }) => {
      const taskIndex = this.tasks.findIndex(task => task.id === taskId);
      if (taskIndex !== -1) {
        const originalTask = { ...this.tasks[taskIndex] };
        
        // Remove original task
        const [removedTask] = this.tasks.splice(taskIndex, 1);
        
        // Apply changes
        const updatedTask = { ...removedTask, ...changes };
        
        // Find new position based on priority
        const newIndex = this.tasks.findIndex(task => 
          task.priority > updatedTask.priority && task.status !== 'completed'
        );
        
        // Insert at new position
        if (newIndex === -1) {
          const insertPos = this.tasks.findIndex(task => task.status === 'completed');
          this.tasks.splice(insertPos === -1 ? this.tasks.length : insertPos, 0, updatedTask);
        } else {
          this.tasks.splice(newIndex, 0, updatedTask);
        }
        
        results.push({ original: originalTask, updated: updatedTask });
      }
    });
    
    return results;
  }
  
  getTasks() {
    return [...this.tasks];
  }
}

const taskManager = new TaskManager();
console.log('Initial tasks:', taskManager.getTasks().map(t => `${t.title} (P${t.priority})`));

taskManager.addTask('Critical Bug Fix', 1, 'Hassan');
taskManager.updateTaskStatus(2, 'completed');
taskManager.addTask('Code Optimization', 2, 'Zara');

console.log('After updates:', taskManager.getTasks().map(t => `${t.title} (P${t.priority}, ${t.status})`));
```

**Example 2: Advanced Array Editing and Undo System**
```javascript
class ArrayEditor {
  constructor(initialArray = []) {
    this.array = [...initialArray];
    this.history = [{ operation: 'initialize', state: [...initialArray], timestamp: Date.now() }];
    this.maxHistorySize = 50;
  }
  
  recordOperation(operation, details) {
    this.history.push({
      operation,
      details,
      state: [...this.array],
      timestamp: Date.now()
    });
    
    // Limit history size
    if (this.history.length > this.maxHistorySize) {
      this.history.splice(0, this.history.length - this.maxHistorySize);
    }
  }
  
  insert(index, ...items) {
    if (index < 0) index = this.array.length + index;
    if (index < 0) index = 0;
    if (index > this.array.length) index = this.array.length;
    
    const result = this.array.splice(index, 0, ...items);
    this.recordOperation('insert', { index, items: [...items] });
    
    return {
      insertedAt: index,
      insertedItems: items,
      newLength: this.array.length
    };
  }
  
  remove(startIndex, count = 1) {
    if (startIndex < 0) startIndex = this.array.length + startIndex;
    startIndex = Math.max(0, Math.min(startIndex, this.array.length));
    count = Math.max(0, Math.min(count, this.array.length - startIndex));
    
    const removed = this.array.splice(startIndex, count);
    this.recordOperation('remove', { startIndex, count, removed: [...removed] });
    
    return {
      removedFrom: startIndex,
      removedItems: removed,
      newLength: this.array.length
    };
  }
  
  replace(startIndex, deleteCount, ...newItems) {
    if (startIndex < 0) startIndex = this.array.length + startIndex;
    startIndex = Math.max(0, Math.min(startIndex, this.array.length));
    deleteCount = Math.max(0, Math.min(deleteCount, this.array.length - startIndex));
    
    const removed = this.array.splice(startIndex, deleteCount, ...newItems);
    this.recordOperation('replace', { 
      startIndex, 
      deleteCount, 
      newItems: [...newItems], 
      removed: [...removed] 
    });
    
    return {
      position: startIndex,
      removed: removed,
      inserted: newItems,
      newLength: this.array.length
    };
  }
  
  move(fromIndex, toIndex, count = 1) {
    if (fromIndex < 0) fromIndex = this.array.length + fromIndex;
    if (toIndex < 0) toIndex = this.array.length + toIndex;
    
    fromIndex = Math.max(0, Math.min(fromIndex, this.array.length - 1));
    toIndex = Math.max(0, Math.min(toIndex, this.array.length));
    count = Math.max(1, Math.min(count, this.array.length - fromIndex));
    
    // Remove items from source
    const items = this.array.splice(fromIndex, count);
    
    // Adjust destination index if it was after the removed items
    if (toIndex > fromIndex) {
      toIndex -= items.length;
    }
    
    // Insert items at destination
    this.array.splice(toIndex, 0, ...items);
    
    this.recordOperation('move', { fromIndex, toIndex, count, items: [...items] });
    
    return {
      from: fromIndex,
      to: toIndex,
      movedItems: items,
      count: count
    };
  }
  
  swap(index1, index2) {
    if (index1 < 0) index1 = this.array.length + index1;
    if (index2 < 0) index2 = this.array.length + index2;
    
    if (index1 >= 0 && index1 < this.array.length && 
        index2 >= 0 && index2 < this.array.length && 
        index1 !== index2) {
      
      // Use splice to swap elements
      const item1 = this.array.splice(index1, 1)[0];
      const item2 = this.array.splice(index2 - (index2 > index1 ? 1 : 0), 1)[0];
      
      this.array.splice(index1 < index2 ? index1 : index1 - 1, 0, item2);
      this.array.splice(index1 < index2 ? index2 : index2 + 1, 0, item1);
      
      this.recordOperation('swap', { index1, index2, item1, item2 });
      
      return {
        swapped: [{ index: index1, item: item2 }, { index: index2, item: item1 }]
      };
    }
    
    return null;
  }
  
  undo() {
    if (this.history.length > 1) {
      this.history.pop(); // Remove current state
      const previousState = this.history[this.history.length - 1];
      this.array = [...previousState.state];
      
      return {
        operation: previousState.operation,
        restoredState: [...this.array]
      };
    }
    return null;
  }
  
  getArray() {
    return [...this.array];
  }
  
  getHistory() {
    return this.history.map(h => ({
      operation: h.operation,
      details: h.details,
      timestamp: new Date(h.timestamp).toLocaleTimeString()
    }));
  }
}

const editor = new ArrayEditor(['a', 'b', 'c', 'd', 'e']);
console.log('Initial:', editor.getArray());

editor.insert(2, 'X', 'Y');
console.log('After insert:', editor.getArray()); // ['a', 'b', 'X', 'Y', 'c', 'd', 'e']

editor.remove(1, 2);
console.log('After remove:', editor.getArray()); // ['a', 'Y', 'c', 'd', 'e']

editor.replace(1, 1, 'REPLACED');
console.log('After replace:', editor.getArray()); // ['a', 'REPLACED', 'c', 'd', 'e']

editor.move(0, 3, 1);
console.log('After move:', editor.getArray()); // ['REPLACED', 'c', 'd', 'a', 'e']

editor.swap(0, 4);
console.log('After swap:', editor.getArray()); // ['e', 'c', 'd', 'a', 'REPLACED']

console.log('\nHistory:', editor.getHistory());
editor.undo();
console.log('After undo:', editor.getArray());
```

**Example 3: Real-time Data Stream Management**
```javascript
class DataStreamProcessor {
  constructor(maxBufferSize = 1000) {
    this.buffer = [];
    this.maxBufferSize = maxBufferSize;
    this.alerts = [];
    this.statistics = {
      totalProcessed: 0,
      alertCount: 0,
      bufferOverflows: 0
    };
  }
  
  addDataPoint(dataPoint) {
    const timestamp = Date.now();
    const enrichedData = { ...dataPoint, timestamp, id: this.generateId() };
    
    // Add to buffer
    this.buffer.splice(this.buffer.length, 0, enrichedData);
    
    // Manage buffer size
    if (this.buffer.length > this.maxBufferSize) {
      const removed = this.buffer.splice(0, this.buffer.length - this.maxBufferSize);
      this.statistics.bufferOverflows++;
      console.log(`Buffer overflow: removed ${removed.length} old records`);
    }
    
    this.statistics.totalProcessed++;
    this.processDataPoint(enrichedData);
    
    return enrichedData;
  }
  
  processDataPoint(dataPoint) {
    // Check for anomalies
    if (this.isAnomalous(dataPoint)) {
      const alert = {
        id: this.generateId(),
        timestamp: dataPoint.timestamp,
        type: 'ANOMALY',
        data: dataPoint,
        severity: this.calculateSeverity(dataPoint)
      };
      
      this.alerts.splice(this.alerts.length, 0, alert);
      this.statistics.alertCount++;
      
      // Keep only last 100 alerts
      if (this.alerts.length > 100) {
        this.alerts.splice(0, this.alerts.length - 100);
      }
    }
  }
  
  isAnomalous(dataPoint) {
    if (this.buffer.length <
```javascript
const taskQueue = [
  {
    id: 'T001',
    name: 'Process Payment',
    priority: 'high',
    estimatedTime: 120, // seconds
    dependencies: [],
    resources: ['payment-service', 'database'],
    status: 'queued',
    createdAt: '2025-01-16T09:00:00',
    deadline: '2025-01-16T09:10:00',
    retryCount: 0,
    maxRetries: 3,
    executorType: 'payment-worker'
  },
  {
    id: 'T002',
    name: 'Send Email Notification',
    priority: 'medium',
    estimatedTime: 45,
    dependencies: ['T001'],
    resources: ['email-service'],
    status: 'queued',
    createdAt: '2025-01-16T09:01:00',
    deadline: '2025-01-16T09:20:00',
    retryCount: 1,
    maxRetries: 5,
    executorType: 'notification-worker'
  },
  {
    id: 'T003',
    name: 'Update Inventory',
    priority: 'low',
    estimatedTime: 90,
    dependencies: [],
    resources: ['inventory-service', 'database'],
    status: 'running',
    createdAt: '2025-01-16T08:55:00',
    deadline: '2025-01-16T10:00:00',
    retryCount: 0,
    maxRetries: 2,
    executorType: 'inventory-worker'
  },
  {
    id: 'T004',
    name: 'Generate Report',
    priority: 'critical',
    estimatedTime: 300,
    dependencies: ['T003'],
    resources: ['report-service', 'database'],
    status: 'queued',
    createdAt: '2025-01-16T09:02:00',
    deadline: '2025-01-16T09:15:00',
    retryCount: 0,
    maxRetries: 1,
    executorType: 'report-worker'
  }
];

// Find next task to execute based on complex scheduling logic
function findNextTaskToExecute(availableWorker, currentTime = new Date()) {
  const taskIndex = taskQueue.findIndex((task, index) => {
    // Task must be queued
    if (task.status !== 'queued') return false;
    
    // Worker type compatibility
    if (availableWorker && task.executorType !== availableWorker.type) return false;
    
    // Check if dependencies are completed
    const dependenciesMet = task.dependencies.every(depId => {
      const depTask = taskQueue.find(t => t.id === depId);
      return depTask && depTask.status === 'completed';
    });
    if (!dependenciesMet) return false;
    
    // Check resource availability
    if (availableWorker && availableWorker.availableResources) {
      const resourcesAvailable = task.resources.every(resource => 
        availableWorker.availableResources.includes(resource)
      );
      if (!resourcesAvailable) return false;
    }
    
    // Check retry limits
    if (task.retryCount >= task.maxRetries) return false;
    
    // Check if deadline hasn't passed
    const deadline = new Date(task.deadline);
    if (currentTime > deadline) return false;
    
    // Priority-based scoring
    const priorityScore = {
      'critical': 100,
      'high': 75,
      'medium': 50,
      'low': 25
    }[task.priority] || 0;
    
    // Urgency based on deadline
    const timeToDeadline = (deadline - currentTime) / (1000 * 60); // minutes
    const urgencyScore = timeToDeadline < 5 ? 50 : timeToDeadline < 15 ? 25 : 0;
    
    // Age of task (older tasks get slight priority)
    const taskAge = (currentTime - new Date(task.createdAt)) / (1000 * 60); // minutes
    const ageScore = Math.min(taskAge * 0.5, 10);
    
    task.calculatedScore = priorityScore + urgencyScore + ageScore;
    
    return true;
  });
  
  // Find the highest scoring eligible task
  const eligibleTasks = taskQueue
    .map((task, index) => ({ task, index }))
    .filter(({ task, index }) => {
      // Repeat the same checks as above
      if (task.status !== 'queued') return false;
      if (availableWorker && task.executorType !== availableWorker.type) return false;
      
      const dependenciesMet = task.dependencies.every(depId => {
        const depTask = taskQueue.find(t => t.id === depId);
        return depTask && depTask.status === 'completed';
      });
      if (!dependenciesMet) return false;
      
      if (availableWorker && availableWorker.availableResources) {
        const resourcesAvailable = task.resources.every(resource => 
          availableWorker.availableResources.includes(resource)
        );
        if (!resourcesAvailable) return false;
      }
      
      if (task.retryCount >= task.maxRetries) return false;
      
      const deadline = new Date(task.deadline);
      if (currentTime > deadline) return false;
      
      // Calculate scores
      const priorityScore = { 'critical': 100, 'high': 75, 'medium': 50, 'low': 25 }[task.priority] || 0;
      const timeToDeadline = (deadline - currentTime) / (1000 * 60);
      const urgencyScore = timeToDeadline < 5 ? 50 : timeToDeadline < 15 ? 25 : 0;
      const taskAge = (currentTime - new Date(task.createdAt)) / (1000 * 60);
      const ageScore = Math.min(taskAge * 0.5, 10);
      
      task.calculatedScore = priorityScore + urgencyScore + ageScore;
      
      return true;
    });
  
  if (eligibleTasks.length === 0) return null;
  
  // Find task with highest score
  const bestTask = eligibleTasks.reduce((best, current) => 
    current.task.calculatedScore > best.task.calculatedScore ? current : best
  );
  
  const task = bestTask.task;
  const timeToDeadline = (new Date(task.deadline) - currentTime) / (1000 * 60);
  
  return {
    index: bestTask.index,
    task: task,
    executionDetails: {
      estimatedExecutionTime: task.estimatedTime,
      timeToDeadline: Math.round(timeToDeadline),
      isUrgent: timeToDeadline < 10,
      isCritical: task.priority === 'critical',
      riskLevel: task.retryCount > 0 ? 'high' : timeToDeadline < 5 ? 'medium' : 'low',
      resourceRequirements: task.resources,
      dependencyStatus: task.dependencies.map(depId => {
        const depTask = taskQueue.find(t => t.id === depId);
        return { id: depId, status: depTask?.status || 'not-found' };
      })
    }
  };
}

// Find task to pause/kill due to resource constraints
function findTaskToPause(resourceConstraint, currentTime = new Date()) {
  const taskIndex = taskQueue.findIndex(task => {
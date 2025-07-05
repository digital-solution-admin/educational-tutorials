# Modern JavaScript Best Practices

A comprehensive guide to writing clean, efficient, and maintainable JavaScript code in 2025.

## Table of Contents

1. [Introduction](#introduction)
2. [Modern JavaScript Syntax](#modern-javascript-syntax)
3. [Asynchronous JavaScript](#asynchronous-javascript)
4. [Functions and Scope](#functions-and-scope)
5. [Objects and Data Structures](#objects-and-data-structures)
6. [Error Handling](#error-handling)
7. [Module Systems](#module-systems)
8. [Performance Optimization](#performance-optimization)
9. [Testing](#testing)
10. [Tooling](#tooling)

## Introduction

JavaScript has evolved significantly over the years. This guide focuses on modern JavaScript practices that help you write code that is:

- Maintainable and readable
- Performant and efficient
- Secure and robust
- Easier to debug and test

Following these best practices will help you avoid common pitfalls and leverage the full power of modern JavaScript.

## Modern JavaScript Syntax

### Use `const` and `let` instead of `var`

```javascript
// Bad
var name = 'John';
var age = 30;
age = 31;

// Good
const name = 'John'; // Immutable variable
let age = 30;        // Mutable variable
age = 31;
```

### Template Literals

```javascript
// Bad
const greeting = 'Hello, ' + name + '! You are ' + age + ' years old.';

// Good
const greeting = `Hello, ${name}! You are ${age} years old.`;
```

### Optional Chaining and Nullish Coalescing

```javascript
// Bad
const street = user && user.address && user.address.street;
const username = user.username || 'Guest';

// Good
const street = user?.address?.street;
const username = user.username ?? 'Guest'; // Only falls back if username is null or undefined
```

### Destructuring

```javascript
// Bad
const firstName = user.firstName;
const lastName = user.lastName;
const item = array[0];

// Good
const { firstName, lastName } = user;
const [item] = array;

// Function parameters
function processUser({ firstName, lastName, age }) {
  console.log(`${firstName} ${lastName} is ${age} years old`);
}
```

### Spread and Rest Operators

```javascript
// Spread (expanding)
const newArray = [...oldArray, newItem];
const newObject = { ...oldObject, newProperty: value };

// Rest (collecting)
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}
```

## Asynchronous JavaScript

### Promises over Callbacks

```javascript
// Bad
function fetchData(callback) {
  setTimeout(() => {
    callback(null, 'Data');
  }, 1000);
}

fetchData((error, data) => {
  if (error) {
    console.error(error);
  } else {
    console.log(data);
  }
});

// Good
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Data');
    }, 1000);
  });
}

fetchData()
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

### Async/Await for Cleaner Asynchronous Code

```javascript
// Using promises with .then()
function processData() {
  return fetchData()
    .then(data => processResult(data))
    .then(result => saveResult(result))
    .catch(error => handleError(error));
}

// Better with async/await
async function processData() {
  try {
    const data = await fetchData();
    const result = await processResult(data);
    return await saveResult(result);
  } catch (error) {
    handleError(error);
  }
}
```

### Promise Methods

```javascript
// Promise.all - wait for all promises to resolve
const results = await Promise.all([
  fetchUsers(),
  fetchPosts(),
  fetchComments()
]);

// Promise.race - use the first promise to resolve
const result = await Promise.race([
  fetchFromAPI1(),
  fetchFromAPI2()
]);

// Promise.allSettled - wait for all promises to settle (resolve or reject)
const outcomes = await Promise.allSettled([
  fetchCriticalData(),
  fetchNonCriticalData()
]);
```

## Functions and Scope

### Arrow Functions

```javascript
// Traditional function
function add(a, b) {
  return a + b;
}

// Arrow function
const add = (a, b) => a + b;

// Preserving 'this' context
// Bad
function Person() {
  this.age = 0;
  setInterval(function() {
    this.age++; // 'this' refers to the wrong context
  }, 1000);
}

// Good
function Person() {
  this.age = 0;
  setInterval(() => {
    this.age++; // 'this' is correctly bound
  }, 1000);
}
```

### Default Parameters

```javascript
// Bad
function createUser(name, age, isPremium) {
  name = name || 'Anonymous';
  age = age || 30;
  isPremium = isPremium !== undefined ? isPremium : false;
  // ...
}

// Good
function createUser(name = 'Anonymous', age = 30, isPremium = false) {
  // ...
}
```

### Partial Application and Currying

```javascript
// Currying
const multiply = a => b => a * b;
const double = multiply(2);
double(5); // 10

// Partial application
const baseUrl = 'https://api.example.com';
const fetchFromApi = endpoint => fetch(`${baseUrl}${endpoint}`);

fetchFromApi('/users');
fetchFromApi('/posts');
```

## Objects and Data Structures

### Object Shorthand

```javascript
// Bad
function createUser(name, age) {
  return {
    name: name,
    age: age,
    getName: function() {
      return this.name;
    }
  };
}

// Good
function createUser(name, age) {
  return {
    name,     // Property shorthand
    age,      // Property shorthand
    getName() { // Method shorthand
      return this.name;
    }
  };
}
```

### Object Immutability

```javascript
// Bad - mutating objects
function updateUser(user) {
  user.age += 1;
  return user;
}

// Good - creating new objects with changes
function updateUser(user) {
  return { ...user, age: user.age + 1 };
}

// With nested objects
function updateUserAddress(user, street) {
  return {
    ...user,
    address: {
      ...user.address,
      street
    }
  };
}
```

### Use Maps for Dynamic Keys

```javascript
// Bad - using object as a map
const userRoles = {};
userRoles[userId] = 'admin';

// Good - using Map
const userRoles = new Map();
userRoles.set(userId, 'admin');

// Maps preserve key order and allow any type as keys
```

### Use Sets for Unique Collections

```javascript
// Bad
const uniqueIds = [];
function addId(id) {
  if (!uniqueIds.includes(id)) {
    uniqueIds.push(id);
  }
}

// Good
const uniqueIds = new Set();
function addId(id) {
  uniqueIds.add(id); // Automatically handles uniqueness
}
```

## Error Handling

### Try-Catch Blocks

```javascript
// Bad
function processData(data) {
  // No error handling
  const result = JSON.parse(data);
  return result.value;
}

// Good
function processData(data) {
  try {
    const result = JSON.parse(data);
    return result.value;
  } catch (error) {
    console.error('Invalid JSON:', error);
    return null;
  }
}
```

### Custom Error Types

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
  }
}

try {
  if (!isValid) {
    throw new ValidationError('Data is invalid');
  }
} catch (error) {
  if (error instanceof ValidationError) {
    // Handle validation errors
  } else {
    // Handle other errors
  }
}
```

### Prefer Early Returns

```javascript
// Bad
function processUser(user) {
  let result;
  if (user) {
    if (user.isActive) {
      result = user.name;
    } else {
      result = 'Inactive user';
    }
  } else {
    result = 'No user';
  }
  return result;
}

// Good
function processUser(user) {
  if (!user) return 'No user';
  if (!user.isActive) return 'Inactive user';
  return user.name;
}
```

## Module Systems

### ES Modules

```javascript
// Exporting
export const PI = 3.14159;
export function square(x) {
  return x * x;
}
export default class Calculator { /* ... */ }

// Importing
import Calculator, { PI, square } from './math.js';
import * as math from './math.js';
```

### Dynamic Imports

```javascript
// Static import (always loaded)
import { sortArray } from './utils.js';

// Dynamic import (loaded on demand)
async function loadSortModule() {
  if (needsSort) {
    const { sortArray } = await import('./utils.js');
    return sortArray(data);
  }
  return data;
}
```

## Performance Optimization

### Debouncing and Throttling

```javascript
// Debounce (delays execution until after a quiet period)
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// For search inputs, window resizing
const debouncedSearch = debounce(searchAPI, 300);

// Throttle (limits execution to once per time period)
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}

// For scroll events, mouse movements
const throttledScroll = throttle(handleScroll, 100);
```

### Memoization

```javascript
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Example: Expensive calculation
const memoizedCalculate = memoize(expensiveCalculation);
```

### Use Web Workers for CPU-Intensive Tasks

```javascript
// main.js
const worker = new Worker('worker.js');

worker.onmessage = function(event) {
  console.log('Result:', event.data);
};

worker.postMessage({ data: complexData });

// worker.js
self.onmessage = function(event) {
  const result = processComplexData(event.data.data);
  self.postMessage(result);
};
```

## Testing

### Unit Testing with Jest

```javascript
// math.js
export function add(a, b) {
  return a + b;
}

// math.test.js
import { add } from './math';

describe('Math functions', () => {
  test('adds two numbers correctly', () => {
    expect(add(2, 3)).toBe(5);
    expect(add(-1, 1)).toBe(0);
    expect(add(0, 0)).toBe(0);
  });
});
```

### Mocking

```javascript
// user-service.js
export async function getUser(id) {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
}

// user-service.test.js
import { getUser } from './user-service';

// Mock the fetch function
global.fetch = jest.fn(() =>
  Promise.resolve({
    json: () => Promise.resolve({ id: 1, name: 'John' })
  })
);

test('getUser returns user data', async () => {
  const user = await getUser(1);
  expect(user).toEqual({ id: 1, name: 'John' });
  expect(fetch).toHaveBeenCalledWith('/api/users/1');
});
```

## Tooling

### Essential Development Tools

1. **Package Managers**
   - npm or Yarn for dependency management
   
2. **Bundlers**
   - Webpack, Rollup, or Vite for bundling modules
   
3. **Transpilers**
   - Babel for transpiling modern JavaScript to older versions

4. **Linters and Formatters**
   - ESLint for catching errors and enforcing code style
   - Prettier for automatic code formatting

5. **Type Checking**
   - TypeScript or JSDoc with TypeScript checking

### ESLint Configuration Example

```javascript
// .eslintrc.js
module.exports = {
  "env": {
    "browser": true,
    "es2022": true,
    "node": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:import/errors"
  ],
  "parserOptions": {
    "ecmaVersion": 2022,
    "sourceType": "module"
  },
  "rules": {
    "no-console": "warn",
    "prefer-const": "error",
    "no-unused-vars": ["error", { "argsIgnorePattern": "^_" }]
  }
};
```

### Using TypeScript for Type Safety

```typescript
// Without TypeScript
function calculateTotal(price, quantity, discount) {
  return price * quantity * (1 - discount);
}

// With TypeScript
function calculateTotal(
  price: number,
  quantity: number,
  discount: number = 0
): number {
  return price * quantity * (1 - discount);
}

// Interface definitions
interface User {
  id: number;
  name: string;
  email: string;
  isActive: boolean;
}

function processUser(user: User): void {
  // TypeScript ensures user has all required properties
}
```

## Conclusion

Following these modern JavaScript best practices will help you write code that is more maintainable, efficient, and less error-prone. Remember that the JavaScript ecosystem continues to evolve rapidly, so staying up-to-date with the latest patterns and practices is essential for any developer.

## Additional Resources

- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [JavaScript.info](https://javascript.info/)
- [Eloquent JavaScript](https://eloquentjavascript.net/)
- [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
- [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)

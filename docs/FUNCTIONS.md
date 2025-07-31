# Functions Documentation

## Overview

This document provides comprehensive documentation for all public functions and utilities in the project.

## Table of Contents

- [Utility Functions](#utility-functions)
- [Validation Functions](#validation-functions)
- [Data Processing Functions](#data-processing-functions)
- [Authentication Functions](#authentication-functions)
- [Database Functions](#database-functions)
- [Helper Functions](#helper-functions)

## Utility Functions

### `formatDate(date, format)`

Formats a date according to the specified format string.

**Parameters:**
- `date` (Date | string): The date to format
- `format` (string): Format string (e.g., 'YYYY-MM-DD', 'MM/DD/YYYY')

**Returns:**
- `string`: Formatted date string

**Example:**
```javascript
import { formatDate } from './utils/date';

const date = new Date('2024-01-15');
const formatted = formatDate(date, 'YYYY-MM-DD'); // '2024-01-15'
const formatted2 = formatDate(date, 'MM/DD/YYYY'); // '01/15/2024'
```

**Supported Formats:**
- `YYYY`: 4-digit year
- `MM`: 2-digit month
- `DD`: 2-digit day
- `HH`: 24-hour format hour
- `mm`: Minutes
- `ss`: Seconds

### `generateId(length, prefix)`

Generates a unique identifier with optional prefix.

**Parameters:**
- `length` (number, optional): Length of the ID (default: 12)
- `prefix` (string, optional): Prefix to add to the ID

**Returns:**
- `string`: Generated unique identifier

**Example:**
```javascript
import { generateId } from './utils/id';

const id = generateId(); // 'abc123def456'
const userId = generateId(8, 'user_'); // 'user_a1b2c3d4'
const sessionId = generateId(16); // 'a1b2c3d4e5f6g7h8'
```

### `deepClone(obj)`

Creates a deep copy of an object or array.

**Parameters:**
- `obj` (any): The object or array to clone

**Returns:**
- `any`: Deep copy of the input

**Example:**
```javascript
import { deepClone } from './utils/object';

const original = { user: { name: 'John', age: 30 } };
const copy = deepClone(original);
copy.user.name = 'Jane'; // original.user.name remains 'John'
```

### `debounce(func, delay)`

Creates a debounced version of a function that delays execution.

**Parameters:**
- `func` (function): The function to debounce
- `delay` (number): Delay in milliseconds

**Returns:**
- `function`: Debounced function

**Example:**
```javascript
import { debounce } from './utils/function';

const handleSearch = debounce((query) => {
  console.log('Searching for:', query);
}, 300);

// Multiple rapid calls will only execute once after 300ms
handleSearch('hello');
handleSearch('hello world'); // Only this will execute
```

## Validation Functions

### `validateEmail(email)`

Validates an email address format.

**Parameters:**
- `email` (string): Email address to validate

**Returns:**
- `boolean`: True if valid, false otherwise

**Example:**
```javascript
import { validateEmail } from './utils/validation';

validateEmail('user@example.com'); // true
validateEmail('invalid-email'); // false
validateEmail('test@'); // false
```

### `validatePassword(password, options)`

Validates password strength based on criteria.

**Parameters:**
- `password` (string): Password to validate
- `options` (object, optional): Validation options

**Options:**
- `minLength` (number): Minimum length (default: 8)
- `requireUppercase` (boolean): Require uppercase letter (default: true)
- `requireLowercase` (boolean): Require lowercase letter (default: true)
- `requireNumbers` (boolean): Require numbers (default: true)
- `requireSpecialChars` (boolean): Require special characters (default: true)

**Returns:**
- `object`: Validation result with `isValid` boolean and `errors` array

**Example:**
```javascript
import { validatePassword } from './utils/validation';

const result = validatePassword('MyPass123!');
// { isValid: true, errors: [] }

const result2 = validatePassword('weak');
// { 
//   isValid: false, 
//   errors: ['Too short', 'Missing uppercase', 'Missing numbers', 'Missing special characters'] 
// }
```

### `sanitizeInput(input, type)`

Sanitizes user input to prevent XSS and injection attacks.

**Parameters:**
- `input` (string): Input to sanitize
- `type` (string): Type of sanitization ('html', 'sql', 'general')

**Returns:**
- `string`: Sanitized input

**Example:**
```javascript
import { sanitizeInput } from './utils/validation';

const userInput = '<script>alert("xss")</script>Hello';
const safe = sanitizeInput(userInput, 'html'); // 'Hello'

const sqlInput = "'; DROP TABLE users; --";
const safeSql = sanitizeInput(sqlInput, 'sql'); // "&#39;; DROP TABLE users; --"
```

## Data Processing Functions

### `transformData(data, schema)`

Transforms data according to a schema definition.

**Parameters:**
- `data` (object | array): Data to transform
- `schema` (object): Transformation schema

**Returns:**
- `any`: Transformed data

**Example:**
```javascript
import { transformData } from './utils/transform';

const data = { firstName: 'John', lastName: 'Doe', age: '30' };
const schema = {
  name: data => `${data.firstName} ${data.lastName}`,
  age: data => parseInt(data.age),
  isAdult: data => parseInt(data.age) >= 18
};

const result = transformData(data, schema);
// { name: 'John Doe', age: 30, isAdult: true }
```

### `filterData(data, filters)`

Filters an array of objects based on criteria.

**Parameters:**
- `data` (array): Array of objects to filter
- `filters` (object): Filter criteria

**Returns:**
- `array`: Filtered data

**Example:**
```javascript
import { filterData } from './utils/filter';

const users = [
  { name: 'John', age: 25, active: true },
  { name: 'Jane', age: 35, active: false },
  { name: 'Bob', age: 30, active: true }
];

const activeAdults = filterData(users, {
  active: true,
  age: { $gte: 25 }
});
// [{ name: 'John', age: 25, active: true }, { name: 'Bob', age: 30, active: true }]
```

### `sortData(data, sortBy, order)`

Sorts an array of objects by specified field.

**Parameters:**
- `data` (array): Array to sort
- `sortBy` (string): Field name to sort by
- `order` (string): 'asc' or 'desc' (default: 'asc')

**Returns:**
- `array`: Sorted data

**Example:**
```javascript
import { sortData } from './utils/sort';

const users = [
  { name: 'John', age: 30 },
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 35 }
];

const sortedByAge = sortData(users, 'age', 'asc');
// [{ name: 'Alice', age: 25 }, { name: 'John', age: 30 }, { name: 'Bob', age: 35 }]
```

## Authentication Functions

### `hashPassword(password, saltRounds)`

Hashes a password using bcrypt.

**Parameters:**
- `password` (string): Plain text password
- `saltRounds` (number, optional): Number of salt rounds (default: 12)

**Returns:**
- `Promise<string>`: Hashed password

**Example:**
```javascript
import { hashPassword } from './auth/password';

const hashedPassword = await hashPassword('mySecretPassword');
// '$2b$12$...'
```

### `verifyPassword(password, hash)`

Verifies a password against its hash.

**Parameters:**
- `password` (string): Plain text password
- `hash` (string): Hashed password

**Returns:**
- `Promise<boolean>`: True if password matches, false otherwise

**Example:**
```javascript
import { verifyPassword } from './auth/password';

const isValid = await verifyPassword('mySecretPassword', hashedPassword);
// true or false
```

### `generateJWT(payload, secret, options)`

Generates a JSON Web Token.

**Parameters:**
- `payload` (object): JWT payload
- `secret` (string): Secret key for signing
- `options` (object, optional): JWT options

**Returns:**
- `string`: Generated JWT token

**Example:**
```javascript
import { generateJWT } from './auth/jwt';

const token = generateJWT(
  { userId: '123', email: 'user@example.com' },
  process.env.JWT_SECRET,
  { expiresIn: '24h' }
);
```

### `verifyJWT(token, secret)`

Verifies and decodes a JWT token.

**Parameters:**
- `token` (string): JWT token to verify
- `secret` (string): Secret key for verification

**Returns:**
- `object | null`: Decoded payload if valid, null otherwise

**Example:**
```javascript
import { verifyJWT } from './auth/jwt';

const payload = verifyJWT(token, process.env.JWT_SECRET);
if (payload) {
  console.log('User ID:', payload.userId);
}
```

## Database Functions

### `connectDB(connectionString, options)`

Establishes database connection.

**Parameters:**
- `connectionString` (string): Database connection string
- `options` (object, optional): Connection options

**Returns:**
- `Promise<Connection>`: Database connection object

**Example:**
```javascript
import { connectDB } from './db/connection';

const db = await connectDB(process.env.DATABASE_URL, {
  maxConnections: 10,
  timeout: 5000
});
```

### `executeQuery(query, params)`

Executes a database query with parameters.

**Parameters:**
- `query` (string): SQL query string
- `params` (array, optional): Query parameters

**Returns:**
- `Promise<array>`: Query results

**Example:**
```javascript
import { executeQuery } from './db/query';

const users = await executeQuery(
  'SELECT * FROM users WHERE age > ? AND active = ?',
  [25, true]
);
```

### `createTransaction(callback)`

Executes multiple queries in a transaction.

**Parameters:**
- `callback` (function): Function containing database operations

**Returns:**
- `Promise<any>`: Transaction result

**Example:**
```javascript
import { createTransaction } from './db/transaction';

const result = await createTransaction(async (trx) => {
  await trx.query('INSERT INTO users (name) VALUES (?)', ['John']);
  await trx.query('INSERT INTO profiles (user_id) VALUES (?)', [userId]);
  return { success: true };
});
```

## Helper Functions

### `retry(fn, maxAttempts, delay)`

Retries a function with exponential backoff.

**Parameters:**
- `fn` (function): Function to retry
- `maxAttempts` (number): Maximum number of attempts (default: 3)
- `delay` (number): Initial delay in milliseconds (default: 1000)

**Returns:**
- `Promise<any>`: Function result or throws after max attempts

**Example:**
```javascript
import { retry } from './utils/retry';

const result = await retry(
  () => fetch('/api/data'),
  3, // max 3 attempts
  1000 // start with 1 second delay
);
```

### `logger(level, message, meta)`

Logs messages with different levels and metadata.

**Parameters:**
- `level` (string): Log level ('debug', 'info', 'warn', 'error')
- `message` (string): Log message
- `meta` (object, optional): Additional metadata

**Returns:**
- `void`

**Example:**
```javascript
import { logger } from './utils/logger';

logger('info', 'User logged in', { userId: '123' });
logger('error', 'Database connection failed', { error: err.message });
```

### `cache(key, value, ttl)`

Simple in-memory caching function.

**Parameters:**
- `key` (string): Cache key
- `value` (any, optional): Value to cache (if undefined, retrieves cached value)
- `ttl` (number, optional): Time to live in milliseconds (default: 300000)

**Returns:**
- `any`: Cached value or undefined

**Example:**
```javascript
import { cache } from './utils/cache';

// Set cache
cache('user:123', { name: 'John', age: 30 }, 60000); // Cache for 1 minute

// Get cache
const user = cache('user:123'); // { name: 'John', age: 30 }

// Cache miss
const missing = cache('user:456'); // undefined
```

## Error Handling

### Custom Error Classes

```javascript
import { ValidationError, DatabaseError, AuthenticationError } from './utils/errors';

// ValidationError
throw new ValidationError('Invalid email format', { field: 'email' });

// DatabaseError
throw new DatabaseError('Connection timeout', { query: 'SELECT * FROM users' });

// AuthenticationError
throw new AuthenticationError('Invalid credentials');
```

### Error Handler Function

```javascript
import { handleError } from './utils/errorHandler';

try {
  // Some operation
} catch (error) {
  const response = handleError(error);
  // Returns standardized error response
}
```

## Best Practices

### Function Documentation

```javascript
/**
 * Calculates the distance between two geographical points
 * @param {number} lat1 - Latitude of first point
 * @param {number} lon1 - Longitude of first point
 * @param {number} lat2 - Latitude of second point
 * @param {number} lon2 - Longitude of second point
 * @param {string} unit - Unit of measurement ('km' or 'miles')
 * @returns {number} Distance between points
 * @throws {ValidationError} When coordinates are invalid
 * @example
 * const distance = calculateDistance(40.7128, -74.0060, 34.0522, -118.2437, 'miles');
 * console.log(distance); // 2445.55
 */
function calculateDistance(lat1, lon1, lat2, lon2, unit = 'km') {
  // Implementation
}
```

### Type Definitions (TypeScript)

```typescript
interface UserData {
  id: string;
  name: string;
  email: string;
  createdAt: Date;
}

interface ValidationOptions {
  minLength?: number;
  requireUppercase?: boolean;
  requireLowercase?: boolean;
  requireNumbers?: boolean;
  requireSpecialChars?: boolean;
}

type LogLevel = 'debug' | 'info' | 'warn' | 'error';
type SortOrder = 'asc' | 'desc';
```

---

*Last updated: 2024-01-01*
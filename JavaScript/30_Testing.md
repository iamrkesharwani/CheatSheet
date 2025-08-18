# JavaScript Testing Fundamentals `v 0.07.25.01`

## Essentials
```javascript
// AAA Pattern - Arrange, Act, Assert
test('should calculate tax correctly', () => {
  // Arrange
  const price = 100, rate = 0.1;
  
  // Act
  const result = calculateTax(price, rate);
  
  // Assert
  expect(result).toBe(10);
});

// TDD Red-Green-Refactor cycle
// 1. Write failing test (Red)
// 2. Write minimal code to pass (Green)  
// 3. Improve code quality (Refactor)
```

**Key Concepts**: Unit Testing | TDD | Mocking | Test Pyramid | AAA Pattern
**When to use**: Every function, feature, and user workflow in your application

## Syntax Reference

### Test Structure (AAA Pattern)
```javascript
// Arrange-Act-Assert pattern
describe('Calculator', () => {
  test('should add two numbers correctly', () => {
    // Arrange - Set up test data
    const a = 5;
    const b = 3;
    const calculator = new Calculator();
    
    // Act - Execute the function
    const result = calculator.add(a, b);
    
    // Assert - Verify expected outcome
    expect(result).toBe(8);
  });
});

// Given-When-Then pattern (BDD style)
describe('User Authentication', () => {
  test('should login with valid credentials', () => {
    // Given a user with valid credentials
    const user = { username: 'john', password: 'secret123' };
    
    // When attempting to login
    const result = authenticateUser(user);
    
    // Then should return success
    expect(result.success).toBe(true);
    expect(result.token).toBeDefined();
  });
});
```

### Test Setup and Teardown
```javascript
describe('Database Tests', () => {
  beforeAll(() => {
    // Run once before all tests
    connectToDatabase();
  });
  
  beforeEach(() => {
    // Run before each test
    seedTestData();
  });
  
  afterEach(() => {
    // Run after each test
    cleanupTestData();
  });
  
  afterAll(() => {
    // Run once after all tests
    disconnectFromDatabase();
  });
  
  test('should create user', () => {
    // Test implementation
  });
});
```

### Mocking and Stubbing
```javascript
// Jest function mocks
const mockCallback = jest.fn();
mockCallback.mockReturnValue(42);
mockCallback.mockResolvedValue({ id: 1, name: 'John' });

// Mock implementation
const mockFn = jest.fn();
mockFn.mockImplementation((x) => x * 2);
expect(mockFn(5)).toBe(10);

// Module mocking
jest.mock('./userService', () => ({
  getUser: jest.fn().mockResolvedValue({ id: 1, name: 'John' }),
  createUser: jest.fn().mockResolvedValue({ id: 2, name: 'Jane' })
}));

// Mock verification
expect(mockFn).toHaveBeenCalled();
expect(mockFn).toHaveBeenCalledTimes(2);
expect(mockFn).toHaveBeenCalledWith('expectedArg');
expect(mockFn).toHaveBeenNthCalledWith(1, 'firstCall');
```

### Test Data Creation
```javascript
// Test data factory
function createUser(overrides = {}) {
  return {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    role: 'user',
    createdAt: new Date('2024-01-01'),
    ...overrides
  };
}

// Usage in tests
const adminUser = createUser({ role: 'admin' });
const testUser = createUser({ 
  name: 'Test User', 
  email: 'test@example.com' 
});

// Parameterized tests
const testCases = [
  { input: 5, expected: 25 },
  { input: 10, expected: 100 },
  { input: -3, expected: 9 }
];

testCases.forEach(({ input, expected }) => {
  test(`should square ${input} to get ${expected}`, () => {
    expect(square(input)).toBe(expected);
  });
});
```

### TDD Cycle Example
```javascript
// Step 1: Write failing test first (Red)
describe('Calculator', () => {
  test('should add two numbers', () => {
    const calculator = new Calculator();
    expect(calculator.add(2, 3)).toBe(5);
  });
});

// Step 2: Write minimal code to pass (Green)
class Calculator {
  add(a, b) {
    return a + b;
  }
}

// Step 3: Refactor to improve quality
class Calculator {
  add(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
      throw new Error('Arguments must be numbers');
    }
    return a + b;
  }
}

// Step 4: Add more tests and repeat cycle
test('should throw error for non-numeric inputs', () => {
  const calculator = new Calculator();
  expect(() => calculator.add('2', 3)).toThrow('Arguments must be numbers');
});
```

## Common Patterns

### Dependency Injection for Testing
```javascript
// Constructor injection
class UserService {
  constructor(apiClient) {
    this.apiClient = apiClient;
  }
  
  async getUser(id) {
    return this.apiClient.get(`/users/${id}`);
  }
}

// Test with injected mock
test('should fetch user data', async () => {
  const mockApiClient = {
    get: jest.fn().mockResolvedValue({
      data: { id: 1, name: 'John Doe' }
    })
  };
  
  const userService = new UserService(mockApiClient);
  const userData = await userService.getUser(1);
  
  expect(mockApiClient.get).toHaveBeenCalledWith('/users/1');
  expect(userData.data.name).toBe('John Doe');
});
```

### Async Testing
```javascript
// Testing promises
test('should fetch user data', async () => {
  const userData = await fetchUser(1);
  expect(userData.name).toBe('John Doe');
});

// Testing with resolves/rejects
test('should resolve with user data', () => {
  return expect(fetchUser(1)).resolves.toHaveProperty('name', 'John Doe');
});

test('should reject with error', () => {
  return expect(fetchUser(999)).rejects.toThrow('User not found');
});

// Testing callbacks
test('should call callback with result', (done) => {
  fetchUserCallback(1, (error, user) => {
    expect(error).toBeNull();
    expect(user.name).toBe('John Doe');
    done();
  });
});
```

### Error Testing
```javascript
// Testing thrown errors
test('should throw error for invalid input', () => {
  expect(() => {
    divide(10, 0);
  }).toThrow('Cannot divide by zero');
});

// Testing async errors
test('should handle API errors', async () => {
  const mockApiClient = {
    get: jest.fn().mockRejectedValue(new Error('Network error'))
  };
  
  await expect(getUserData(1, mockApiClient)).rejects.toThrow('Network error');
});
```

### Integration Testing
```javascript
// Testing multiple components together
describe('User Registration Flow', () => {
  test('should register new user successfully', async () => {
    // Arrange - Set up test database
    await setupTestDatabase();
    
    const userData = {
      username: 'newuser',
      email: 'new@example.com',
      password: 'securePassword123'
    };
    
    // Act - Execute the full registration flow
    const userService = new UserService(realDatabase);
    const emailService = new EmailService(realEmailProvider);
    const registrationService = new RegistrationService(userService, emailService);
    
    const result = await registrationService.registerUser(userData);
    
    // Assert - Verify all expected outcomes
    expect(result.success).toBe(true);
    expect(result.user.id).toBeDefined();
    
    // Verify user was saved to database
    const savedUser = await userService.findByEmail(userData.email);
    expect(savedUser).toBeDefined();
    
    // Verify welcome email was sent
    expect(emailService.getSentEmails()).toHaveLength(1);
    
    // Cleanup
    await cleanupTestDatabase();
  });
});
```

## Assertion Types
```javascript
// Equality assertions
expect(value).toBe(4); // Strict equality (===)
expect(value).toEqual({a: 1, b: 2}); // Deep equality
expect(value).toStrictEqual({a: 1, b: 2}); // Strict deep equality

// Truthiness
expect(value).toBeTruthy();
expect(value).toBeFalsy();
expect(value).toBeNull();
expect(value).toBeUndefined();
expect(value).toBeDefined();

// Numbers
expect(value).toBeGreaterThan(3);
expect(value).toBeLessThan(5);
expect(value).toBeCloseTo(3.14, 2);

// Strings
expect(value).toMatch(/pattern/);
expect(value).toContain('substring');

// Arrays and Objects
expect(array).toContain(item);
expect(array).toHaveLength(3);
expect(object).toHaveProperty('key');
expect(object).toHaveProperty('key', 'value');

// Functions
expect(fn).toThrow();
expect(fn).toThrow('Error message');
expect(fn).toHaveBeenCalled();
expect(fn).toHaveBeenCalledWith(arg1, arg2);
```

## Do's & Don'ts
| ✅ Do | ❌ Don't |
|-------|----------|
| Write tests before or during implementation | Write tests only after implementation |
| Test behavior, not implementation details | Test internal implementation |
| Use descriptive test names that explain what's being tested | Use vague names like "test1" or "should work" |
| Keep tests simple and focused on one thing | Test multiple unrelated things in one test |
| Mock external dependencies and side effects | Use real external services in unit tests |
| Clean up test data and state after each test | Let tests affect each other |
| Use specific assertions over generic ones | Rely on toBeTruthy() for everything |

## Quick Fixes
- **Tests are slow** → Mock external dependencies, use test doubles
- **Tests are flaky** → Remove shared state, fix timing issues, avoid random data
- **Tests are hard to understand** → Use better names, simplify setup, focus on one thing
- **Tests break with refactoring** → Test behavior not implementation details
- **Hard to test code** → Improve design with dependency injection
- **Too much test setup** → Use test data factories and builders
- **Tests don't catch bugs** → Improve assertions, test edge cases and error conditions

## Gotchas
⚠️ **High test coverage ≠ good tests** - Focus on testing critical paths and edge cases
⚠️ **Testing implementation details makes tests brittle** - Test public interfaces and behavior
⚠️ **Shared mutable state between tests causes flakiness** - Keep tests isolated
⚠️ **Not testing error conditions** - Test both happy path and error scenarios  
⚠️ **Mocking everything makes tests worthless** - Balance mocking with real object testing
⚠️ **Complex test setup indicates design problems** - Simplify dependencies and coupling

## Checklist
- [ ] Tests follow AAA (Arrange-Act-Assert) pattern
- [ ] Test names clearly describe what's being tested
- [ ] Each test focuses on one specific behavior
- [ ] External dependencies are mocked or stubbed
- [ ] Tests are independent and can run in any order
- [ ] Both happy path and error cases are tested
- [ ] Test data is minimal and relevant to the test
- [ ] Setup and teardown properly manage test state
- [ ] Assertions are specific and meaningful
- [ ] Tests run fast and provide quick feedback
- [ ] Code coverage focuses on critical business logic
- [ ] Integration tests verify component interactions
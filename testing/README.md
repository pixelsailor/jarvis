# Testing Strategies Guide

## 🎯 AI Agent Instructions for Testing

When working with testing in web development projects, follow these guidelines to ensure comprehensive test coverage, maintainable test code, and reliable applications.

## 🧪 Testing Pyramid

### Testing Levels
```
E2E Tests (Few)
    ↑
Integration Tests (Some)
    ↑
Unit Tests (Many)
```

### Testing Strategy
1. **Unit Tests (70%)** - Test individual functions and components
2. **Integration Tests (20%)** - Test component interactions and API integration
3. **E2E Tests (10%)** - Test complete user workflows

## 🔬 Unit Testing

### Component Testing (React)
```typescript
// Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders with default props', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button')).toBeInTheDocument();
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('handles click events', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('applies variant classes correctly', () => {
    render(<Button variant="destructive">Delete</Button>);
    const button = screen.getByRole('button');
    expect(button).toHaveClass('bg-destructive');
  });

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>Disabled Button</Button>);
    const button = screen.getByRole('button');
    expect(button).toBeDisabled();
    expect(button).toHaveClass('disabled:opacity-50');
  });

  it('forwards ref correctly', () => {
    const ref = jest.fn();
    render(<Button ref={ref}>Button with ref</Button>);
    expect(ref).toHaveBeenCalled();
  });

  it('renders with custom className', () => {
    render(<Button className="custom-class">Custom Button</Button>);
    const button = screen.getByRole('button');
    expect(button).toHaveClass('custom-class');
  });
});
```

### Hook Testing (React)
```typescript
// useUser.test.ts
import { renderHook, waitFor } from '@testing-library/react';
import { useUser } from './useUser';
import { UserService } from '../services/UserService';

// Mock the service
jest.mock('../services/UserService');
const mockUserService = UserService as jest.Mocked<typeof UserService>;

describe('useUser', () => {
  const mockUser = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com'
  };

  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('fetches user data on mount', async () => {
    mockUserService.getUserById.mockResolvedValue(mockUser);

    const { result } = renderHook(() => useUser('1'));

    expect(result.current.loading).toBe(true);
    expect(result.current.user).toBe(null);

    await waitFor(() => {
      expect(result.current.loading).toBe(false);
      expect(result.current.user).toEqual(mockUser);
    });

    expect(mockUserService.getUserById).toHaveBeenCalledWith('1');
  });

  it('handles fetch errors', async () => {
    const errorMessage = 'Failed to fetch user';
    mockUserService.getUserById.mockRejectedValue(new Error(errorMessage));

    const { result } = renderHook(() => useUser('1'));

    await waitFor(() => {
      expect(result.current.loading).toBe(false);
      expect(result.current.error).toBe(errorMessage);
      expect(result.current.user).toBe(null);
    });
  });

  it('updates user data', async () => {
    const updatedUser = { ...mockUser, name: 'Jane Doe' };
    mockUserService.getUserById.mockResolvedValue(mockUser);
    mockUserService.updateUser.mockResolvedValue(updatedUser);

    const { result } = renderHook(() => useUser('1'));

    await waitFor(() => {
      expect(result.current.user).toEqual(mockUser);
    });

    await result.current.updateUser({ name: 'Jane Doe' });

    await waitFor(() => {
      expect(result.current.user).toEqual(updatedUser);
    });
  });
});
```

### Service Testing
```typescript
// UserService.test.ts
import { UserService } from './UserService';
import { ApiClient } from './ApiClient';

// Mock the API client
jest.mock('./ApiClient');
const mockApiClient = ApiClient as jest.Mocked<typeof ApiClient>;

describe('UserService', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  describe('getUserById', () => {
    it('returns user data when API call succeeds', async () => {
      const mockUser = {
        id: '1',
        name: 'John Doe',
        email: 'john@example.com'
      };
      
      mockApiClient.get.mockResolvedValue({ data: mockUser });

      const result = await UserService.getUserById('1');

      expect(result).toEqual(mockUser);
      expect(mockApiClient.get).toHaveBeenCalledWith('/users/1');
    });

    it('throws error when API call fails', async () => {
      const errorMessage = 'User not found';
      mockApiClient.get.mockRejectedValue(new Error(errorMessage));

      await expect(UserService.getUserById('1')).rejects.toThrow(errorMessage);
    });
  });

  describe('createUser', () => {
    it('creates user successfully', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com'
      };
      
      const createdUser = { id: '1', ...userData };
      mockApiClient.post.mockResolvedValue({ data: createdUser });

      const result = await UserService.createUser(userData);

      expect(result).toEqual(createdUser);
      expect(mockApiClient.post).toHaveBeenCalledWith('/users', userData);
    });

    it('validates required fields', async () => {
      const invalidUserData = { name: '' };

      await expect(UserService.createUser(invalidUserData)).rejects.toThrow(
        'Name is required'
      );
    });
  });
});
```

### Utility Function Testing
```typescript
// dateUtils.test.ts
import { formatDate, parseDate, isValidDate } from './dateUtils';

describe('dateUtils', () => {
  describe('formatDate', () => {
    it('formats date correctly', () => {
      const date = new Date('2024-01-15T10:30:00Z');
      const result = formatDate(date, 'YYYY-MM-DD');
      expect(result).toBe('2024-01-15');
    });

    it('handles invalid date', () => {
      const invalidDate = new Date('invalid');
      expect(() => formatDate(invalidDate, 'YYYY-MM-DD')).toThrow(
        'Invalid date'
      );
    });
  });

  describe('parseDate', () => {
    it('parses valid date string', () => {
      const result = parseDate('2024-01-15');
      expect(result).toEqual(new Date('2024-01-15'));
    });

    it('returns null for invalid date string', () => {
      const result = parseDate('invalid-date');
      expect(result).toBeNull();
    });
  });

  describe('isValidDate', () => {
    it('returns true for valid date', () => {
      const result = isValidDate(new Date('2024-01-15'));
      expect(result).toBe(true);
    });

    it('returns false for invalid date', () => {
      const result = isValidDate(new Date('invalid'));
      expect(result).toBe(false);
    });
  });
});
```

## 🔗 Integration Testing

### API Integration Testing
```typescript
// api.integration.test.ts
import { UserService } from '../services/UserService';
import { ApiClient } from '../services/ApiClient';

describe('UserService Integration', () => {
  let apiClient: ApiClient;
  let userService: UserService;

  beforeAll(() => {
    apiClient = new ApiClient('http://localhost:3000/api');
    userService = new UserService(apiClient);
  });

  it('should create and retrieve a user', async () => {
    const userData = {
      name: 'Integration Test User',
      email: 'integration@test.com'
    };

    // Create user
    const createdUser = await userService.createUser(userData);
    expect(createdUser).toMatchObject(userData);
    expect(createdUser.id).toBeDefined();

    // Retrieve user
    const retrievedUser = await userService.getUserById(createdUser.id);
    expect(retrievedUser).toEqual(createdUser);

    // Clean up
    await userService.deleteUser(createdUser.id);
  });

  it('should handle authentication errors', async () => {
    // Test with invalid credentials
    await expect(userService.getUserById('invalid-id')).rejects.toThrow();
  });
});
```

### Component Integration Testing
```typescript
// UserProfile.integration.test.tsx
import { render, screen, waitFor } from '@testing-library/react';
import { UserProfile } from './UserProfile';
import { UserService } from '../services/UserService';

// Mock the service
jest.mock('../services/UserService');
const mockUserService = UserService as jest.Mocked<typeof UserService>;

describe('UserProfile Integration', () => {
  const mockUser = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com'
  };

  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('should display user information and handle updates', async () => {
    mockUserService.getUserById.mockResolvedValue(mockUser);
    mockUserService.updateUser.mockResolvedValue({
      ...mockUser,
      name: 'Jane Doe'
    });

    render(<UserProfile userId="1" />);

    // Wait for user data to load
    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument();
      expect(screen.getByText('john@example.com')).toBeInTheDocument();
    });

    // Test update functionality
    const updateButton = screen.getByText('Update User');
    fireEvent.click(updateButton);

    await waitFor(() => {
      expect(mockUserService.updateUser).toHaveBeenCalled();
    });
  });

  it('should handle loading and error states', async () => {
    mockUserService.getUserById.mockRejectedValue(new Error('User not found'));

    render(<UserProfile userId="1" />);

    // Should show loading initially
    expect(screen.getByText('Loading...')).toBeInTheDocument();

    // Should show error after failure
    await waitFor(() => {
      expect(screen.getByText('Error: User not found')).toBeInTheDocument();
    });
  });
});
```

## 🎭 End-to-End Testing

### Playwright E2E Tests
```typescript
// user-management.e2e.ts
import { test, expect } from '@playwright/test';

test.describe('User Management', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
    await page.fill('[data-testid="email"]', 'admin@example.com');
    await page.fill('[data-testid="password"]', 'password');
    await page.click('[data-testid="login-button"]');
    await page.waitForURL('/dashboard');
  });

  test('should create a new user', async ({ page }) => {
    // Navigate to users page
    await page.click('[data-testid="users-nav"]');
    await page.waitForURL('/users');

    // Click create user button
    await page.click('[data-testid="create-user-button"]');

    // Fill user form
    await page.fill('[data-testid="user-name"]', 'Test User');
    await page.fill('[data-testid="user-email"]', 'test@example.com');
    await page.selectOption('[data-testid="user-role"]', 'user');

    // Submit form
    await page.click('[data-testid="submit-button"]');

    // Verify user was created
    await expect(page.locator('[data-testid="user-list"]')).toContainText('Test User');
    await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
  });

  test('should edit an existing user', async ({ page }) => {
    // Navigate to users page
    await page.click('[data-testid="users-nav"]');
    await page.waitForURL('/users');

    // Click edit button for first user
    await page.click('[data-testid="edit-user-button"]:first-child');

    // Update user name
    await page.fill('[data-testid="user-name"]', 'Updated User Name');
    await page.click('[data-testid="submit-button"]');

    // Verify user was updated
    await expect(page.locator('[data-testid="user-list"]')).toContainText('Updated User Name');
    await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
  });

  test('should delete a user', async ({ page }) => {
    // Navigate to users page
    await page.click('[data-testid="users-nav"]');
    await page.waitForURL('/users');

    // Click delete button for first user
    await page.click('[data-testid="delete-user-button"]:first-child');

    // Confirm deletion
    await page.click('[data-testid="confirm-delete-button"]');

    // Verify user was deleted
    await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
  });
});
```

### Cypress E2E Tests
```typescript
// user-management.cy.ts
describe('User Management', () => {
  beforeEach(() => {
    cy.login('admin@example.com', 'password');
    cy.visit('/users');
  });

  it('should create a new user', () => {
    cy.get('[data-testid="create-user-button"]').click();
    
    cy.get('[data-testid="user-name"]').type('Test User');
    cy.get('[data-testid="user-email"]').type('test@example.com');
    cy.get('[data-testid="user-role"]').select('user');
    
    cy.get('[data-testid="submit-button"]').click();
    
    cy.get('[data-testid="user-list"]').should('contain', 'Test User');
    cy.get('[data-testid="success-message"]').should('be.visible');
  });

  it('should edit an existing user', () => {
    cy.get('[data-testid="edit-user-button"]').first().click();
    
    cy.get('[data-testid="user-name"]').clear().type('Updated User Name');
    cy.get('[data-testid="submit-button"]').click();
    
    cy.get('[data-testid="user-list"]').should('contain', 'Updated User Name');
    cy.get('[data-testid="success-message"]').should('be.visible');
  });

  it('should delete a user', () => {
    cy.get('[data-testid="delete-user-button"]').first().click();
    cy.get('[data-testid="confirm-delete-button"]').click();
    
    cy.get('[data-testid="success-message"]').should('be.visible');
  });
});
```

## 🧪 Test Utilities and Helpers

### Test Setup and Teardown
```typescript
// test-utils.tsx
import { render, RenderOptions } from '@testing-library/react';
import { ReactElement } from 'react';
import { BrowserRouter } from 'react-router-dom';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ThemeProvider } from '@/contexts/ThemeContext';

// Create a custom render function
const AllTheProviders = ({ children }: { children: React.ReactNode }) => {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: {
        retry: false,
      },
    },
  });

  return (
    <QueryClientProvider client={queryClient}>
      <BrowserRouter>
        <ThemeProvider>
          {children}
        </ThemeProvider>
      </BrowserRouter>
    </QueryClientProvider>
  );
};

const customRender = (
  ui: ReactElement,
  options?: Omit<RenderOptions, 'wrapper'>
) => render(ui, { wrapper: AllTheProviders, ...options });

export * from '@testing-library/react';
export { customRender as render };
```

### Mock Data and Factories
```typescript
// mock-data.ts
export const mockUser: User = {
  id: '1',
  name: 'John Doe',
  email: 'john@example.com',
  role: 'user',
  createdAt: new Date('2024-01-01'),
  updatedAt: new Date('2024-01-01'),
};

export const mockUsers: User[] = [
  mockUser,
  {
    id: '2',
    name: 'Jane Smith',
    email: 'jane@example.com',
    role: 'admin',
    createdAt: new Date('2024-01-02'),
    updatedAt: new Date('2024-01-02'),
  },
];

// Factory function for creating test data
export const createMockUser = (overrides: Partial<User> = {}): User => ({
  id: Math.random().toString(36).substr(2, 9),
  name: 'Test User',
  email: 'test@example.com',
  role: 'user',
  createdAt: new Date(),
  updatedAt: new Date(),
  ...overrides,
});

// Factory for creating multiple users
export const createMockUsers = (count: number): User[] => {
  return Array.from({ length: count }, (_, index) =>
    createMockUser({
      id: (index + 1).toString(),
      name: `User ${index + 1}`,
      email: `user${index + 1}@example.com`,
    })
  );
};
```

### Custom Matchers
```typescript
// custom-matchers.ts
import { expect } from '@jest/globals';

// Custom matcher for checking if a date is valid
expect.extend({
  toBeValidDate(received: any) {
    const pass = received instanceof Date && !isNaN(received.getTime());
    
    if (pass) {
      return {
        message: () => `expected ${received} not to be a valid date`,
        pass: true,
      };
    } else {
      return {
        message: () => `expected ${received} to be a valid date`,
        pass: false,
      };
    }
  },
});

// Custom matcher for checking if a string is a valid email
expect.extend({
  toBeValidEmail(received: string) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    const pass = emailRegex.test(received);
    
    if (pass) {
      return {
        message: () => `expected ${received} not to be a valid email`,
        pass: true,
      };
    } else {
      return {
        message: () => `expected ${received} to be a valid email`,
        pass: false,
      };
    }
  },
});

// Usage in tests
describe('User validation', () => {
  it('should validate email format', () => {
    expect('user@example.com').toBeValidEmail();
    expect('invalid-email').not.toBeValidEmail();
  });

  it('should validate date format', () => {
    expect(new Date('2024-01-01')).toBeValidDate();
    expect(new Date('invalid')).not.toBeValidDate();
  });
});
```

## 📊 Test Coverage and Reporting

### Coverage Configuration
```json
// jest.config.js
module.exports = {
  collectCoverage: true,
  coverageDirectory: 'coverage',
  coverageReporters: ['text', 'lcov', 'html'],
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
    '!src/index.tsx',
    '!src/reportWebVitals.ts',
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

### Coverage Reports
```typescript
// coverage-report.ts
import { generateCoverageReport } from './coverage-utils';

describe('Coverage Report', () => {
  it('should generate coverage report', async () => {
    const report = await generateCoverageReport();
    
    expect(report.total.coverage).toBeGreaterThanOrEqual(80);
    expect(report.components.coverage).toBeGreaterThanOrEqual(80);
    expect(report.services.coverage).toBeGreaterThanOrEqual(80);
    expect(report.utils.coverage).toBeGreaterThanOrEqual(80);
  });
});
```

## 🎯 Testing Best Practices

### General Guidelines
1. **Write tests first** - Use TDD when possible
2. **Test behavior, not implementation** - Focus on what the code does
3. **Keep tests simple** - One assertion per test
4. **Use descriptive test names** - Clear and specific
5. **Mock external dependencies** - Isolate units under test
6. **Test edge cases** - Handle error conditions
7. **Maintain test data** - Keep test data up to date
8. **Run tests frequently** - Catch issues early

### Test Organization
1. **Group related tests** - Use describe blocks
2. **Use consistent naming** - Follow naming conventions
3. **Keep tests focused** - Test one thing at a time
4. **Use setup and teardown** - Prepare test environment
5. **Clean up after tests** - Remove test data
6. **Use test utilities** - Reuse common test code
7. **Document test purpose** - Explain complex tests
8. **Review test coverage** - Ensure adequate coverage

### Performance Testing
1. **Test loading times** - Measure page load performance
2. **Test API response times** - Monitor API performance
3. **Test memory usage** - Check for memory leaks
4. **Test bundle size** - Monitor bundle size growth
5. **Test rendering performance** - Measure component render times
6. **Test database queries** - Optimize query performance
7. **Test caching** - Verify cache effectiveness
8. **Test scalability** - Test under load

## 🎯 AI Agent Best Practices

When assisting with testing:

1. **Write comprehensive tests** for all code components
2. **Use appropriate testing frameworks** for the technology stack
3. **Follow testing best practices** and patterns
4. **Implement proper test organization** and structure
5. **Use meaningful test data** and mock objects
6. **Test edge cases and error conditions** thoroughly
7. **Maintain high test coverage** across the codebase
8. **Write clear and descriptive test names** and descriptions
9. **Use proper test utilities** and helper functions
10. **Follow the established testing patterns** in the project

## 🔗 Additional Resources

- [Testing Guide](https://testing-guide.org/)
- [Jest Documentation](https://jestjs.io/docs/getting-started)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [Playwright Documentation](https://playwright.dev/)
- [Cypress Documentation](https://docs.cypress.io/)

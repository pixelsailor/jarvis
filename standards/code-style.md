# Code Style Guide

## 🎯 AI Agent Instructions for Code Style

When working with code style in web development projects, follow these guidelines to ensure consistency, readability, and maintainability across the codebase.

## 📝 General Code Style Principles

### Core Principles
1. **Consistency** - Use the same style throughout the project
2. **Readability** - Code should be easy to read and understand
3. **Maintainability** - Code should be easy to modify and extend
4. **Simplicity** - Prefer simple, clear solutions over complex ones
5. **Documentation** - Document complex logic and important decisions
6. **Performance** - Consider performance implications of style choices

### Code Organization
1. **Logical grouping** - Group related code together
2. **Separation of concerns** - Keep different responsibilities separate
3. **Single responsibility** - Each function/component should have one purpose
4. **DRY principle** - Don't repeat yourself
5. **KISS principle** - Keep it simple, stupid
6. **YAGNI principle** - You aren't gonna need it

## 🎨 Formatting Standards

### Indentation and Spacing
```typescript
// Use 2 spaces for indentation (not tabs)
function calculateTotal(items: Item[]): number {
  let total = 0;
  
  for (const item of items) {
    if (item.isActive) {
      total += item.price;
    }
  }
  
  return total;
}

// Use consistent spacing around operators
const result = a + b * c;
const isActive = status === 'active' && user.isLoggedIn;

// Use consistent spacing in function calls
const user = await getUserById(userId);
const posts = await getPostsByUserId(userId, { limit: 10 });

// Use consistent spacing in object literals
const user = {
  id: 1,
  name: 'John Doe',
  email: 'john@example.com',
  isActive: true
};

// Use consistent spacing in arrays
const items = [
  'item1',
  'item2',
  'item3'
];
```

### Line Length and Wrapping
```typescript
// Keep lines under 100 characters when possible
const longVariableName = someVeryLongFunctionCall(
  parameter1,
  parameter2,
  parameter3
);

// Break long function calls across multiple lines
const result = await apiClient.post(
  '/api/users',
  {
    name: user.name,
    email: user.email,
    role: user.role
  },
  {
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`
    }
  }
);

// Break long conditionals across multiple lines
if (
  user.isActive &&
  user.hasPermission('read') &&
  user.role !== 'guest'
) {
  // Handle active user with read permission
}
```

### Semicolons and Commas
```typescript
// Always use semicolons
const name = 'John Doe';
const age = 25;
const isActive = true;

// Use trailing commas in objects and arrays
const user = {
  id: 1,
  name: 'John Doe',
  email: 'john@example.com', // trailing comma
};

const items = [
  'item1',
  'item2',
  'item3', // trailing comma
];

// Use trailing commas in function parameters
function createUser(
  name: string,
  email: string,
  role: string, // trailing comma
) {
  return { name, email, role };
}
```

## 🔤 Naming Conventions

### Variables and Functions
```typescript
// Use camelCase for variables and functions
const userName = 'John Doe';
const userAge = 25;
const isUserActive = true;

// Use descriptive names
const getUserById = (id: string) => { /* ... */ };
const calculateTotalPrice = (items: Item[]) => { /* ... */ };
const validateUserInput = (input: UserInput) => { /* ... */ };

// Use boolean prefixes
const isActive = true;
const hasPermission = false;
const canEdit = true;
const shouldUpdate = false;

// Use event handler prefixes
const handleUserClick = (user: User) => { /* ... */ };
const handleFormSubmit = (data: FormData) => { /* ... */ };
const handleInputChange = (event: ChangeEvent) => { /* ... */ };
```

### Classes and Interfaces
```typescript
// Use PascalCase for classes and interfaces
class UserService {
  // class implementation
}

interface User {
  id: string;
  name: string;
  email: string;
}

// Use descriptive names for types
type UserRole = 'admin' | 'user' | 'guest';
type ApiResponse<T> = {
  data: T;
  message: string;
  success: boolean;
};

// Use generic type parameters
interface Repository<T> {
  findById(id: string): Promise<T>;
  save(entity: T): Promise<T>;
  delete(id: string): Promise<void>;
}
```

### Constants
```typescript
// Use UPPER_CASE for constants
const API_BASE_URL = 'https://api.example.com';
const MAX_RETRY_ATTEMPTS = 3;
const DEFAULT_PAGE_SIZE = 10;

// Use camelCase for object constants
const config = {
  apiUrl: 'https://api.example.com',
  timeout: 5000,
  retries: 3
} as const;

// Use descriptive names for enums
enum UserRole {
  ADMIN = 'admin',
  USER = 'user',
  GUEST = 'guest'
}
```

## 🏗️ Function and Method Style

### Function Declarations
```typescript
// Use function declarations for main functions
function calculateTotal(items: Item[]): number {
  return items.reduce((total, item) => total + item.price, 0);
}

// Use arrow functions for callbacks and short functions
const items = data.map(item => ({
  ...item,
  displayName: `${item.firstName} ${item.lastName}`
}));

// Use async/await for asynchronous functions
async function fetchUserData(userId: string): Promise<User> {
  try {
    const response = await apiClient.get(`/users/${userId}`);
    return response.data;
  } catch (error) {
    throw new Error(`Failed to fetch user: ${error.message}`);
  }
}

// Use proper error handling
async function createUser(userData: CreateUserRequest): Promise<User> {
  try {
    const response = await apiClient.post('/users', userData);
    return response.data;
  } catch (error) {
    if (error.status === 409) {
      throw new Error('User already exists');
    }
    throw new Error(`Failed to create user: ${error.message}`);
  }
}
```

### Method Organization
```typescript
class UserService {
  // Public methods first
  async getUserById(id: string): Promise<User> {
    return this.fetchUser(id);
  }

  async createUser(userData: CreateUserRequest): Promise<User> {
    this.validateUserData(userData);
    return this.saveUser(userData);
  }

  async updateUser(id: string, updates: UpdateUserRequest): Promise<User> {
    const user = await this.getUserById(id);
    return this.saveUser({ ...user, ...updates });
  }

  // Private methods last
  private async fetchUser(id: string): Promise<User> {
    const response = await apiClient.get(`/users/${id}`);
    return response.data;
  }

  private validateUserData(userData: CreateUserRequest): void {
    if (!userData.email || !userData.name) {
      throw new Error('Email and name are required');
    }
  }

  private async saveUser(userData: any): Promise<User> {
    const response = await apiClient.post('/users', userData);
    return response.data;
  }
}
```

## 🎨 Component Style

### React Component Style
```typescript
// Component structure
import React, { useState, useEffect, useCallback } from 'react';
import { Button } from '@/components/ui/Button';
import { UserService } from '@/services/UserService';
import type { User, UserProps } from '@/types/User';

// Component interface
interface UserProfileProps {
  userId: string;
  onUserUpdate?: (user: User) => void;
  className?: string;
}

// Component implementation
export const UserProfile: React.FC<UserProfileProps> = ({
  userId,
  onUserUpdate,
  className
}) => {
  // State declarations
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  // Event handlers
  const handleUserUpdate = useCallback((updatedUser: User) => {
    setUser(updatedUser);
    onUserUpdate?.(updatedUser);
  }, [onUserUpdate]);

  // Effects
  useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        setError(null);
        const userData = await UserService.getUserById(userId);
        setUser(userData);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Failed to fetch user');
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [userId]);

  // Render
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>User not found</div>;

  return (
    <div className={className}>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
      <Button onClick={() => handleUserUpdate(user)}>
        Update User
      </Button>
    </div>
  );
};
```

### Vue Component Style
```vue
<template>
  <div class="user-profile" :class="className">
    <h2 v-if="user">{{ user.name }}</h2>
    <p v-if="user">{{ user.email }}</p>
    <button @click="handleUserUpdate" :disabled="loading">
      Update User
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { UserService } from '@/services/UserService';
import type { User } from '@/types/User';

// Props
interface Props {
  userId: string;
  onUserUpdate?: (user: User) => void;
  className?: string;
}

const props = defineProps<Props>();

// State
const user = ref<User | null>(null);
const loading = ref(true);
const error = ref<string | null>(null);

// Methods
const handleUserUpdate = () => {
  if (user.value) {
    props.onUserUpdate?.(user.value);
  }
};

const fetchUser = async () => {
  try {
    loading.value = true;
    error.value = null;
    const userData = await UserService.getUserById(props.userId);
    user.value = userData;
  } catch (err) {
    error.value = err instanceof Error ? err.message : 'Failed to fetch user';
  } finally {
    loading.value = false;
  }
};

// Lifecycle
onMounted(() => {
  fetchUser();
});
</script>

<style scoped>
.user-profile {
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
}

.user-profile h2 {
  margin: 0 0 0.5rem 0;
  color: #333;
}

.user-profile p {
  margin: 0 0 1rem 0;
  color: #666;
}
</style>
```

## 🎨 CSS Style

### CSS Organization
```css
/* Use consistent indentation (2 spaces) */
.user-profile {
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
  background-color: #fff;
}

.user-profile__header {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 1rem;
  color: #333;
}

.user-profile__content {
  padding: 1rem 0;
  line-height: 1.6;
}

.user-profile__footer {
  border-top: 1px solid #eee;
  padding-top: 1rem;
  margin-top: 1rem;
}

/* Use consistent spacing */
.user-profile--featured {
  border-color: #007bff;
  background-color: #f8f9fa;
}

.user-profile.is-active {
  opacity: 1;
  transform: translateY(0);
}

.user-profile.is-loading {
  opacity: 0.5;
  pointer-events: none;
}

/* Use consistent property ordering */
.button {
  /* Layout */
  display: inline-block;
  position: relative;
  
  /* Box model */
  padding: 0.5rem 1rem;
  margin: 0;
  border: 1px solid #ccc;
  border-radius: 0.25rem;
  
  /* Typography */
  font-size: 1rem;
  font-weight: 500;
  line-height: 1.5;
  text-align: center;
  
  /* Visual */
  background-color: #fff;
  color: #333;
  
  /* Misc */
  cursor: pointer;
  transition: all 0.2s ease;
}
```

### CSS Custom Properties
```css
/* Use consistent naming for CSS variables */
:root {
  /* Colors */
  --color-primary: #007bff;
  --color-primary-dark: #0056b3;
  --color-primary-light: #66b3ff;
  
  --color-secondary: #6c757d;
  --color-secondary-dark: #545b62;
  --color-secondary-light: #adb5bd;
  
  --color-success: #28a745;
  --color-danger: #dc3545;
  --color-warning: #ffc107;
  --color-info: #17a2b8;
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  
  /* Typography */
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  
  /* Layout */
  --container-max-width: 1200px;
  --header-height: 4rem;
  --sidebar-width: 16rem;
  
  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  
  /* Transitions */
  --transition-fast: 0.15s ease;
  --transition-normal: 0.2s ease;
  --transition-slow: 0.3s ease;
}
```

## 🧪 Test Style

### Test Organization
```typescript
// Test file structure
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { UserProfile } from './UserProfile';
import { UserService } from '@/services/UserService';
import type { User } from '@/types/User';

// Mock services
jest.mock('@/services/UserService');
const mockUserService = UserService as jest.Mocked<typeof UserService>;

// Test data
const mockUser: User = {
  id: '1',
  name: 'John Doe',
  email: 'john@example.com',
  role: 'user'
};

// Test suite
describe('UserProfile', () => {
  // Setup and teardown
  beforeEach(() => {
    jest.clearAllMocks();
  });

  // Test cases
  describe('rendering', () => {
    it('renders user information correctly', async () => {
      // Arrange
      mockUserService.getUserById.mockResolvedValue(mockUser);

      // Act
      render(<UserProfile userId="1" />);

      // Assert
      await waitFor(() => {
        expect(screen.getByText('John Doe')).toBeInTheDocument();
        expect(screen.getByText('john@example.com')).toBeInTheDocument();
      });
    });

    it('shows loading state initially', () => {
      // Arrange
      mockUserService.getUserById.mockImplementation(
        () => new Promise(() => {}) // Never resolves
      );

      // Act
      render(<UserProfile userId="1" />);

      // Assert
      expect(screen.getByText('Loading...')).toBeInTheDocument();
    });

    it('shows error state when fetch fails', async () => {
      // Arrange
      mockUserService.getUserById.mockRejectedValue(
        new Error('Failed to fetch user')
      );

      // Act
      render(<UserProfile userId="1" />);

      // Assert
      await waitFor(() => {
        expect(screen.getByText('Error: Failed to fetch user')).toBeInTheDocument();
      });
    });
  });

  describe('interactions', () => {
    it('calls onUserUpdate when update button is clicked', async () => {
      // Arrange
      const onUserUpdate = jest.fn();
      mockUserService.getUserById.mockResolvedValue(mockUser);

      // Act
      render(<UserProfile userId="1" onUserUpdate={onUserUpdate} />);
      
      await waitFor(() => {
        expect(screen.getByText('John Doe')).toBeInTheDocument();
      });

      fireEvent.click(screen.getByText('Update User'));

      // Assert
      expect(onUserUpdate).toHaveBeenCalledWith(mockUser);
    });
  });
});
```

## 📝 Documentation Style

### Code Comments
```typescript
/**
 * Calculates the total price of items with tax applied
 * @param items - Array of items to calculate total for
 * @param taxRate - Tax rate as a decimal (e.g., 0.08 for 8%)
 * @returns Total price including tax
 */
function calculateTotalWithTax(items: Item[], taxRate: number): number {
  const subtotal = items.reduce((total, item) => total + item.price, 0);
  const tax = subtotal * taxRate;
  return subtotal + tax;
}

// Single line comment for simple explanations
const user = await getUserById(userId); // Fetch user data

// Multi-line comment for complex logic
/*
 * This function handles the complex logic of determining
 * whether a user can access a specific resource based on
 * their role, permissions, and resource ownership.
 */
function canUserAccessResource(user: User, resource: Resource): boolean {
  // Implementation here
}
```

### JSDoc Comments
```typescript
/**
 * User service for managing user operations
 */
class UserService {
  /**
   * Retrieves a user by their ID
   * @param id - The user ID
   * @returns Promise that resolves to the user data
   * @throws {Error} When user is not found
   */
  async getUserById(id: string): Promise<User> {
    // Implementation
  }

  /**
   * Creates a new user
   * @param userData - The user data to create
   * @returns Promise that resolves to the created user
   * @throws {Error} When user creation fails
   */
  async createUser(userData: CreateUserRequest): Promise<User> {
    // Implementation
  }
}
```

## 🎯 Code Style Best Practices

### General Guidelines
1. **Use consistent formatting** - Use Prettier or similar tools
2. **Follow language conventions** - Use established patterns for each language
3. **Write self-documenting code** - Use clear, descriptive names
4. **Keep functions small** - Aim for single responsibility
5. **Use meaningful comments** - Explain why, not what
6. **Handle errors gracefully** - Use proper error handling patterns
7. **Use TypeScript** - Leverage type safety when possible
8. **Write tests** - Ensure code quality with comprehensive tests

### Performance Considerations
1. **Avoid premature optimization** - Focus on readability first
2. **Use appropriate data structures** - Choose the right tool for the job
3. **Minimize re-renders** - Use React.memo, useMemo, useCallback appropriately
4. **Lazy load components** - Load components only when needed
5. **Optimize bundle size** - Use code splitting and tree shaking
6. **Use efficient algorithms** - Choose algorithms with good time complexity

### Security Considerations
1. **Validate inputs** - Always validate user inputs
2. **Sanitize data** - Clean data before processing
3. **Use secure defaults** - Implement secure-by-default patterns
4. **Handle sensitive data** - Protect sensitive information
5. **Use HTTPS** - Always use secure connections
6. **Implement proper authentication** - Use secure auth patterns

## 🎯 AI Agent Best Practices

When assisting with code style:

1. **Follow established patterns** in the project
2. **Use consistent formatting** across all code
3. **Choose descriptive names** for all code elements
4. **Follow language-specific conventions** for the technology stack
5. **Use proper error handling** patterns
6. **Write self-documenting code** with clear logic
7. **Use appropriate comments** to explain complex logic
8. **Follow the established style guide** for the project
9. **Use TypeScript** for better type safety when possible
10. **Write comprehensive tests** for all code

## 🔗 Additional Resources

- [Code Style Guide](https://code-style.org/)
- [Prettier Configuration](https://prettier.io/docs/en/configuration.html)
- [ESLint Rules](https://eslint.org/docs/rules/)
- [TypeScript Style Guide](https://typescript-eslint.io/rules/)
- [React Style Guide](https://react-style-guide.org/)

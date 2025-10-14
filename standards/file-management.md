# File Management Standards

## 🎯 AI Agent Instructions for File Management

When working with file management in web development projects, follow these guidelines to ensure optimal organization, maintainability, and scalability.

## 📁 Directory Structure Standards

### Standard Web Application Structure
```
project-root/
├── public/                     # Static assets served directly
│   ├── favicon.ico
│   ├── robots.txt
│   ├── sitemap.xml
│   └── images/
│       ├── icons/
│       └── logos/
├── src/                        # Source code
│   ├── components/             # Reusable UI components
│   │   ├── ui/                 # Basic UI components
│   │   ├── forms/              # Form-specific components
│   │   ├── layout/             # Layout components
│   │   └── features/           # Feature-specific components
│   ├── pages/                  # Page components (if using file-based routing)
│   ├── hooks/                  # Custom React hooks
│   ├── services/               # API and business logic
│   ├── store/                  # State management
│   ├── utils/                  # Utility functions
│   ├── types/                  # TypeScript type definitions
│   ├── constants/              # Application constants
│   ├── styles/                 # Global styles and themes
│   ├── assets/                 # Static assets processed by build system
│   │   ├── images/
│   │   ├── icons/
│   │   └── fonts/
│   └── __tests__/              # Test files
├── docs/                       # Documentation
├── scripts/                    # Build and deployment scripts
├── tests/                      # Integration and E2E tests
├── .github/                    # GitHub workflows and templates
├── .vscode/                    # VS Code configuration
├── .cursor/                    # Cursor configuration
└── config files                # Package.json, tsconfig.json, etc.
```

### Framework-Specific Structures

#### React/Next.js Structure
```
src/
├── app/                        # Next.js 13+ app directory
│   ├── (auth)/                 # Route groups
│   ├── api/                    # API routes
│   ├── globals.css
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── ui/                     # shadcn/ui components
│   ├── forms/                  # Form components
│   └── layout/                 # Layout components
├── lib/                        # Utility libraries
├── hooks/                      # Custom hooks
└── types/                      # TypeScript types
```

#### Vue.js Structure
```
src/
├── components/
│   ├── base/                   # Base components
│   ├── common/                 # Common components
│   └── features/               # Feature components
├── views/                      # Page components
├── router/                     # Vue Router configuration
├── store/                      # Vuex/Pinia store
├── composables/                # Vue 3 composables
├── utils/                      # Utility functions
└── types/                      # TypeScript types
```

#### Angular Structure
```
src/
├── app/
│   ├── core/                   # Singleton services, guards
│   ├── shared/                 # Shared components, pipes
│   ├── features/               # Feature modules
│   │   └── user/
│   │       ├── components/
│   │       ├── services/
│   │       └── models/
│   └── layout/                 # Layout components
├── assets/                     # Static assets
└── environments/               # Environment configurations
```

## 📄 File Naming Conventions

### General Naming Rules
1. **Use kebab-case** for file and directory names
2. **Use PascalCase** for component files
3. **Use camelCase** for utility and service files
4. **Use UPPER_CASE** for constants and environment files
5. **Use descriptive names** that clearly indicate purpose
6. **Avoid abbreviations** unless they're widely understood
7. **Use consistent suffixes** for similar file types

### File Type Naming Patterns
```
# Components
UserProfile.tsx                 # React component
user-profile.vue                # Vue component
user-profile.component.ts       # Angular component

# Services
userService.ts                  # Service file
apiClient.ts                    # API client
authService.ts                  # Authentication service

# Utilities
dateUtils.ts                    # Date utilities
stringUtils.ts                  # String utilities
validationUtils.ts              # Validation utilities

# Types
user.types.ts                   # User-related types
api.types.ts                    # API-related types
common.types.ts                 # Common types

# Constants
apiEndpoints.ts                 # API endpoints
appConstants.ts                 # Application constants
errorMessages.ts                # Error messages

# Hooks (React)
useUser.ts                      # User hook
useApi.ts                       # API hook
useLocalStorage.ts              # Local storage hook

# Styles
globals.css                     # Global styles
components.css                  # Component styles
variables.css                   # CSS variables
```

### Directory Naming Patterns
```
# Feature-based organization
src/
├── features/
│   ├── authentication/
│   ├── user-management/
│   ├── product-catalog/
│   └── order-processing/

# Component-based organization
src/
├── components/
│   ├── ui/                     # Basic UI components
│   ├── forms/                  # Form components
│   ├── layout/                 # Layout components
│   └── features/               # Feature-specific components

# Service-based organization
src/
├── services/
│   ├── api/                    # API services
│   ├── auth/                   # Authentication services
│   ├── storage/                # Storage services
│   └── utils/                  # Utility services
```

## 🗂️ File Organization Patterns

### Component Organization
```
components/
├── ui/                         # Reusable UI components
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.test.tsx
│   │   ├── Button.stories.tsx
│   │   └── index.ts
│   ├── Input/
│   │   ├── Input.tsx
│   │   ├── Input.test.tsx
│   │   ├── Input.stories.tsx
│   │   └── index.ts
│   └── index.ts                # Barrel export
├── forms/                      # Form components
│   ├── LoginForm/
│   ├── RegistrationForm/
│   └── ContactForm/
└── layout/                     # Layout components
    ├── Header/
    ├── Footer/
    ├── Sidebar/
    └── MainLayout/
```

### Service Organization
```
services/
├── api/                        # API services
│   ├── base/
│   │   ├── apiClient.ts
│   │   ├── httpClient.ts
│   │   └── types.ts
│   ├── auth/
│   │   ├── authService.ts
│   │   ├── tokenService.ts
│   │   └── types.ts
│   └── user/
│       ├── userService.ts
│       └── types.ts
├── storage/                    # Storage services
│   ├── localStorage.ts
│   ├── sessionStorage.ts
│   └── indexedDB.ts
└── utils/                      # Utility services
    ├── dateService.ts
    ├── validationService.ts
    └── notificationService.ts
```

### Type Organization
```
types/
├── api/                        # API-related types
│   ├── auth.types.ts
│   ├── user.types.ts
│   └── common.types.ts
├── components/                 # Component types
│   ├── ui.types.ts
│   ├── form.types.ts
│   └── layout.types.ts
├── services/                   # Service types
│   ├── api.types.ts
│   ├── storage.types.ts
│   └── utils.types.ts
└── global/                     # Global types
    ├── app.types.ts
    ├── env.types.ts
    └── index.ts
```

## 📋 File Content Standards

### Component File Structure
```typescript
// components/ui/Button/Button.tsx
import React from 'react';
import { type VariantProps, cva } from 'class-variance-authority';
import { cn } from '@/lib/utils';

// Types and interfaces
interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
}

// Constants and variants
const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground hover:bg-primary/90',
        destructive: 'bg-destructive text-destructive-foreground hover:bg-destructive/90',
        outline: 'border border-input hover:bg-accent hover:text-accent-foreground',
        secondary: 'bg-secondary text-secondary-foreground hover:bg-secondary/80',
        ghost: 'hover:bg-accent hover:text-accent-foreground',
        link: 'underline-offset-4 hover:underline text-primary',
      },
      size: {
        default: 'h-10 py-2 px-4',
        sm: 'h-9 px-3 rounded-md',
        lg: 'h-11 px-8 rounded-md',
        icon: 'h-10 w-10',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  }
);

// Main component
const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, ...props }, ref) => {
    return (
      <button
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...props}
      />
    );
  }
);

Button.displayName = 'Button';

// Exports
export { Button, buttonVariants };
export type { ButtonProps };
```

### Service File Structure
```typescript
// services/api/userService.ts
import { apiClient } from './base/apiClient';
import type { User, CreateUserRequest, UpdateUserRequest } from '@/types/api/user.types';

// Constants
const ENDPOINTS = {
  USERS: '/users',
  USER_BY_ID: (id: string) => `/users/${id}`,
  USER_PROFILE: '/users/profile',
} as const;

// Types
interface UserServiceResponse<T> {
  data: T;
  message: string;
  success: boolean;
}

// Service class
export class UserService {
  // Get all users
  static async getUsers(params?: {
    page?: number;
    limit?: number;
    search?: string;
  }): Promise<UserServiceResponse<User[]>> {
    try {
      const response = await apiClient.get(ENDPOINTS.USERS, { params });
      return response.data;
    } catch (error) {
      throw new Error(`Failed to fetch users: ${error.message}`);
    }
  }

  // Get user by ID
  static async getUserById(id: string): Promise<UserServiceResponse<User>> {
    try {
      const response = await apiClient.get(ENDPOINTS.USER_BY_ID(id));
      return response.data;
    } catch (error) {
      throw new Error(`Failed to fetch user: ${error.message}`);
    }
  }

  // Create user
  static async createUser(userData: CreateUserRequest): Promise<UserServiceResponse<User>> {
    try {
      const response = await apiClient.post(ENDPOINTS.USERS, userData);
      return response.data;
    } catch (error) {
      throw new Error(`Failed to create user: ${error.message}`);
    }
  }

  // Update user
  static async updateUser(id: string, userData: UpdateUserRequest): Promise<UserServiceResponse<User>> {
    try {
      const response = await apiClient.put(ENDPOINTS.USER_BY_ID(id), userData);
      return response.data;
    } catch (error) {
      throw new Error(`Failed to update user: ${error.message}`);
    }
  }

  // Delete user
  static async deleteUser(id: string): Promise<UserServiceResponse<void>> {
    try {
      const response = await apiClient.delete(ENDPOINTS.USER_BY_ID(id));
      return response.data;
    } catch (error) {
      throw new Error(`Failed to delete user: ${error.message}`);
    }
  }
}

// Default export
export default UserService;
```

### Hook File Structure
```typescript
// hooks/useUser.ts
import { useState, useEffect, useCallback } from 'react';
import { UserService } from '@/services/api/userService';
import type { User, CreateUserRequest, UpdateUserRequest } from '@/types/api/user.types';

// Types
interface UseUserReturn {
  user: User | null;
  loading: boolean;
  error: string | null;
  createUser: (userData: CreateUserRequest) => Promise<void>;
  updateUser: (userData: UpdateUserRequest) => Promise<void>;
  deleteUser: () => Promise<void>;
  refetch: () => Promise<void>;
}

// Hook
export const useUser = (userId?: string): UseUserReturn => {
  // State
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  // Functions
  const fetchUser = useCallback(async () => {
    if (!userId) return;
    
    try {
      setLoading(true);
      setError(null);
      const response = await UserService.getUserById(userId);
      setUser(response.data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch user');
    } finally {
      setLoading(false);
    }
  }, [userId]);

  const createUser = useCallback(async (userData: CreateUserRequest) => {
    try {
      setLoading(true);
      setError(null);
      const response = await UserService.createUser(userData);
      setUser(response.data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to create user');
      throw err;
    } finally {
      setLoading(false);
    }
  }, []);

  const updateUser = useCallback(async (userData: UpdateUserRequest) => {
    if (!userId) return;
    
    try {
      setLoading(true);
      setError(null);
      const response = await UserService.updateUser(userId, userData);
      setUser(response.data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to update user');
      throw err;
    } finally {
      setLoading(false);
    }
  }, [userId]);

  const deleteUser = useCallback(async () => {
    if (!userId) return;
    
    try {
      setLoading(true);
      setError(null);
      await UserService.deleteUser(userId);
      setUser(null);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to delete user');
      throw err;
    } finally {
      setLoading(false);
    }
  }, [userId]);

  // Effects
  useEffect(() => {
    fetchUser();
  }, [fetchUser]);

  // Return
  return {
    user,
    loading,
    error,
    createUser,
    updateUser,
    deleteUser,
    refetch: fetchUser,
  };
};
```

## 🔄 File Import/Export Standards

### Import Organization
```typescript
// 1. Node modules (external libraries)
import React from 'react';
import { useState, useEffect } from 'react';
import { Router, Route, Switch } from 'react-router-dom';
import axios from 'axios';

// 2. Internal modules (absolute imports)
import { Button } from '@/components/ui/Button';
import { UserService } from '@/services/api/userService';
import { useUser } from '@/hooks/useUser';
import type { User } from '@/types/api/user.types';

// 3. Relative imports
import './Button.css';
import { validateUser } from '../utils/validation';
import type { ButtonProps } from './Button.types';
```

### Export Patterns
```typescript
// Named exports (preferred for most cases)
export const Button = () => { /* ... */ };
export const Input = () => { /* ... */ };
export type { ButtonProps, InputProps };

// Default exports (for main component or service)
const UserProfile = () => { /* ... */ };
export default UserProfile;

// Barrel exports (index.ts files)
export { Button } from './Button';
export { Input } from './Input';
export { Form } from './Form';
export type { ButtonProps, InputProps, FormProps } from './types';

// Re-exports with renaming
export { Button as PrimaryButton } from './Button';
export { Input as TextInput } from './Input';
```

## 📁 Asset Management

### Image Organization
```
src/assets/images/
├── icons/                      # Icon files
│   ├── ui/                     # UI icons
│   ├── social/                 # Social media icons
│   └── brand/                  # Brand icons
├── illustrations/              # Illustration files
├── photos/                     # Photo files
│   ├── hero/                   # Hero images
│   ├── gallery/                # Gallery images
│   └── avatars/                # User avatars
└── backgrounds/                # Background images
```

### Font Organization
```
src/assets/fonts/
├── primary/                    # Primary font family
│   ├── regular.woff2
│   ├── medium.woff2
│   └── bold.woff2
├── secondary/                  # Secondary font family
│   ├── regular.woff2
│   └── italic.woff2
└── monospace/                  # Monospace font
    └── regular.woff2
```

## 🧪 Test File Organization

### Test File Naming
```
# Unit tests
Button.test.tsx                 # Component tests
userService.test.ts             # Service tests
dateUtils.test.ts               # Utility tests

# Integration tests
userFlow.test.ts                # User flow tests
apiIntegration.test.ts          # API integration tests

# E2E tests
login.e2e.ts                    # E2E test files
checkout.e2e.ts                 # E2E test files

# Test utilities
testUtils.ts                    # Test utilities
mockData.ts                     # Mock data
testSetup.ts                    # Test setup
```

### Test File Structure
```
__tests__/
├── components/                 # Component tests
│   ├── ui/
│   │   ├── Button.test.tsx
│   │   └── Input.test.tsx
│   └── forms/
│       └── LoginForm.test.tsx
├── services/                   # Service tests
│   ├── api/
│   │   └── userService.test.ts
│   └── utils/
│       └── dateUtils.test.ts
├── hooks/                      # Hook tests
│   └── useUser.test.ts
├── integration/                # Integration tests
│   └── userFlow.test.ts
└── utils/                      # Test utilities
    ├── testUtils.ts
    ├── mockData.ts
    └── testSetup.ts
```

## 🔧 Configuration Files

### Configuration File Organization
```
project-root/
├── .env                        # Environment variables
├── .env.local                  # Local environment overrides
├── .env.development            # Development environment
├── .env.production             # Production environment
├── .env.test                   # Test environment
├── .gitignore                  # Git ignore rules
├── .eslintrc.js                # ESLint configuration
├── .prettierrc                 # Prettier configuration
├── .stylelintrc.js             # Stylelint configuration
├── tsconfig.json               # TypeScript configuration
├── tsconfig.test.json          # TypeScript test configuration
├── jest.config.js              # Jest configuration
├── vitest.config.ts            # Vitest configuration
├── tailwind.config.js          # Tailwind CSS configuration
├── postcss.config.js           # PostCSS configuration
├── webpack.config.js           # Webpack configuration
├── vite.config.ts              # Vite configuration
├── next.config.js              # Next.js configuration
├── nuxt.config.ts              # Nuxt.js configuration
└── angular.json                # Angular configuration
```

## 🎯 File Management Best Practices

### Organization Principles
1. **Group by feature** rather than file type when possible
2. **Use consistent naming conventions** across the project
3. **Keep related files together** in the same directory
4. **Use barrel exports** to simplify imports
5. **Separate concerns** between different types of files
6. **Use descriptive names** that clearly indicate purpose
7. **Avoid deep nesting** (max 3-4 levels deep)
8. **Keep files focused** on a single responsibility

### Maintenance Guidelines
1. **Regular cleanup** of unused files and imports
2. **Consistent refactoring** to maintain organization
3. **Documentation updates** when structure changes
4. **Version control** for configuration changes
5. **Automated tools** for formatting and linting
6. **Regular reviews** of file organization
7. **Team alignment** on naming conventions
8. **Tooling integration** for consistency

## 🎯 AI Agent Best Practices

When assisting with file management:

1. **Follow established patterns** in the project
2. **Use consistent naming conventions** across all files
3. **Organize files logically** by feature or functionality
4. **Keep related files together** in appropriate directories
5. **Use proper import/export patterns** for better maintainability
6. **Follow framework-specific conventions** for the technology stack
7. **Maintain clean separation** between different types of files
8. **Use descriptive names** that clearly indicate file purpose
9. **Follow the established directory structure** for the project
10. **Ensure proper file organization** for scalability and maintainability

## 🔗 Additional Resources

- [File Organization Best Practices](https://file-organization.org/)
- [Naming Conventions Guide](https://naming-conventions.org/)
- [Project Structure Patterns](https://project-structure.org/)
- [Import/Export Best Practices](https://import-export.org/)

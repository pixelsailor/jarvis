# React Development Guide

## 🎯 AI Agent Instructions for React Development

When working with React projects, follow these guidelines to ensure optimal AI assistance and code quality.

## 📁 Project Structure

```
src/
├── components/
│   ├── ui/               # Reusable UI components
│   ├── forms/            # Form-specific components
│   ├── layout/           # Layout components
│   └── features/         # Feature-specific components
├── hooks/                # Custom React hooks
├── services/             # API and external services
├── store/                # State management (Redux/Zustand)
├── utils/                # Utility functions
├── types/                # TypeScript type definitions
├── constants/            # Application constants
├── styles/               # Global styles and themes
└── __tests__/            # Test files
```

## ⚛️ Component Development

### Component Naming
- Use PascalCase for component files: `UserProfile.tsx`
- Use descriptive names that indicate purpose
- Prefix with `Base` for fundamental components: `BaseButton.tsx`
- Use feature prefixes for domain-specific components: `UserProfileCard.tsx`

### Functional Component Template
```tsx
import React, { useState, useEffect, useCallback } from 'react';
import type { ComponentProps } from './types';

interface UserProfileProps {
  userId: string;
  onUpdate?: (user: User) => void;
  className?: string;
}

export const UserProfile: React.FC<UserProfileProps> = ({
  userId,
  onUpdate,
  className = ''
}) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchUser = useCallback(async () => {
    try {
      setLoading(true);
      const userData = await userService.getUser(userId);
      setUser(userData);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch user');
    } finally {
      setLoading(false);
    }
  }, [userId]);

  useEffect(() => {
    fetchUser();
  }, [fetchUser]);

  const handleUpdate = useCallback((updatedUser: User) => {
    setUser(updatedUser);
    onUpdate?.(updatedUser);
  }, [onUpdate]);

  if (loading) return <div className="loading">Loading...</div>;
  if (error) return <div className="error">Error: {error}</div>;
  if (!user) return <div className="not-found">User not found</div>;

  return (
    <div className={`user-profile ${className}`}>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
      {/* Additional user details */}
    </div>
  );
};

export default UserProfile;
```

## 🎣 Custom Hooks

### Hook Patterns
```typescript
// hooks/useUser.ts
import { useState, useEffect, useCallback } from 'react';
import { userService } from '../services/userService';

interface UseUserReturn {
  user: User | null;
  loading: boolean;
  error: string | null;
  refetch: () => Promise<void>;
  updateUser: (updates: Partial<User>) => Promise<void>;
}

export const useUser = (userId: string): UseUserReturn => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchUser = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      const userData = await userService.getUser(userId);
      setUser(userData);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch user');
    } finally {
      setLoading(false);
    }
  }, [userId]);

  const updateUser = useCallback(async (updates: Partial<User>) => {
    if (!user) return;
    
    try {
      const updatedUser = await userService.updateUser(user.id, updates);
      setUser(updatedUser);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to update user');
    }
  }, [user]);

  useEffect(() => {
    fetchUser();
  }, [fetchUser]);

  return {
    user,
    loading,
    error,
    refetch: fetchUser,
    updateUser
  };
};
```

## 🏪 State Management

### Redux Toolkit Pattern
```typescript
// store/slices/userSlice.ts
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
import { userService } from '../../services/userService';

interface UserState {
  users: User[];
  currentUser: User | null;
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  users: [],
  currentUser: null,
  loading: false,
  error: null
};

export const fetchUsers = createAsyncThunk(
  'users/fetchUsers',
  async () => {
    const response = await userService.getUsers();
    return response;
  }
);

export const userSlice = createSlice({
  name: 'users',
  initialState,
  reducers: {
    setCurrentUser: (state, action) => {
      state.currentUser = action.payload;
    },
    clearError: (state) => {
      state.error = null;
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.users = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message || 'Failed to fetch users';
      });
  }
});

export const { setCurrentUser, clearError } = userSlice.actions;
export default userSlice.reducer;
```

### Zustand Pattern
```typescript
// store/userStore.ts
import { create } from 'zustand';
import { devtools } from 'zustand/middleware';

interface UserStore {
  users: User[];
  currentUser: User | null;
  loading: boolean;
  error: string | null;
  fetchUsers: () => Promise<void>;
  setCurrentUser: (user: User | null) => void;
  updateUser: (id: string, updates: Partial<User>) => Promise<void>;
}

export const useUserStore = create<UserStore>()(
  devtools(
    (set, get) => ({
      users: [],
      currentUser: null,
      loading: false,
      error: null,

      fetchUsers: async () => {
        set({ loading: true, error: null });
        try {
          const users = await userService.getUsers();
          set({ users, loading: false });
        } catch (error) {
          set({ 
            error: error instanceof Error ? error.message : 'Failed to fetch users',
            loading: false 
          });
        }
      },

      setCurrentUser: (user) => set({ currentUser: user }),

      updateUser: async (id, updates) => {
        try {
          const updatedUser = await userService.updateUser(id, updates);
          set((state) => ({
            users: state.users.map(user => 
              user.id === id ? updatedUser : user
            ),
            currentUser: state.currentUser?.id === id ? updatedUser : state.currentUser
          }));
        } catch (error) {
          set({ 
            error: error instanceof Error ? error.message : 'Failed to update user'
          });
        }
      }
    }),
    { name: 'user-store' }
  )
);
```

## 🎨 Styling Guidelines

### CSS Modules Pattern
```tsx
// UserProfile.module.css
.userProfile {
  padding: 1rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.5rem;
}

.header {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 200px;
}

// UserProfile.tsx
import styles from './UserProfile.module.css';

export const UserProfile = ({ user }: { user: User }) => (
  <div className={styles.userProfile}>
    <h2 className={styles.header}>{user.name}</h2>
    {/* Component content */}
  </div>
);
```

### Styled Components Pattern
```tsx
import styled from 'styled-components';

const UserProfileContainer = styled.div`
  padding: 1rem;
  border: 1px solid ${props => props.theme.colors.border};
  border-radius: 0.5rem;
  background: ${props => props.theme.colors.background};
`;

const Header = styled.h2`
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: ${props => props.theme.colors.text};
`;

export const UserProfile = ({ user }: { user: User }) => (
  <UserProfileContainer>
    <Header>{user.name}</Header>
    {/* Component content */}
  </UserProfileContainer>
);
```

## ⚡ Performance Optimization

### Memoization Patterns
```tsx
import React, { memo, useMemo, useCallback } from 'react';

// Memoized component
export const UserCard = memo<UserCardProps>(({ user, onEdit }) => {
  const handleEdit = useCallback(() => {
    onEdit(user.id);
  }, [user.id, onEdit]);

  const userStats = useMemo(() => ({
    totalPosts: user.posts?.length || 0,
    joinDate: new Date(user.createdAt).toLocaleDateString()
  }), [user.posts, user.createdAt]);

  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>Posts: {userStats.totalPosts}</p>
      <p>Joined: {userStats.joinDate}</p>
      <button onClick={handleEdit}>Edit</button>
    </div>
  );
});

UserCard.displayName = 'UserCard';
```

### Code Splitting
```tsx
import React, { lazy, Suspense } from 'react';

// Lazy load heavy components
const HeavyComponent = lazy(() => import('./HeavyComponent'));
const AdminPanel = lazy(() => import('./AdminPanel'));

export const App = () => (
  <div>
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
    
    {user.isAdmin && (
      <Suspense fallback={<div>Loading admin panel...</div>}>
        <AdminPanel />
      </Suspense>
    )}
  </div>
);
```

## 🧪 Testing Patterns

### Component Testing
```tsx
// UserProfile.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { UserProfile } from './UserProfile';
import { userService } from '../services/userService';

// Mock the service
jest.mock('../services/userService');
const mockUserService = userService as jest.Mocked<typeof userService>;

describe('UserProfile', () => {
  const mockUser: User = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com'
  };

  beforeEach(() => {
    mockUserService.getUser.mockResolvedValue(mockUser);
  });

  test('renders user information', async () => {
    render(<UserProfile userId="1" />);
    
    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument();
      expect(screen.getByText('john@example.com')).toBeInTheDocument();
    });
  });

  test('handles update callback', async () => {
    const onUpdate = jest.fn();
    render(<UserProfile userId="1" onUpdate={onUpdate} />);
    
    // Simulate user interaction that triggers update
    const updateButton = screen.getByRole('button', { name: /update/i });
    fireEvent.click(updateButton);
    
    await waitFor(() => {
      expect(onUpdate).toHaveBeenCalledWith(expect.objectContaining({
        id: '1'
      }));
    });
  });
});
```

### Hook Testing
```tsx
// useUser.test.ts
import { renderHook, waitFor } from '@testing-library/react';
import { useUser } from './useUser';
import { userService } from '../services/userService';

jest.mock('../services/userService');
const mockUserService = userService as jest.Mocked<typeof userService>;

describe('useUser', () => {
  test('fetches user data', async () => {
    const mockUser: User = { id: '1', name: 'John Doe' };
    mockUserService.getUser.mockResolvedValue(mockUser);

    const { result } = renderHook(() => useUser('1'));

    await waitFor(() => {
      expect(result.current.user).toEqual(mockUser);
      expect(result.current.loading).toBe(false);
    });
  });
});
```

## 🔧 Common Patterns

### Error Boundary
```tsx
// ErrorBoundary.tsx
import React, { Component, ErrorInfo, ReactNode } from 'react';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <div className="error-boundary">
          <h2>Something went wrong.</h2>
          <details>
            {this.state.error?.message}
          </details>
        </div>
      );
    }

    return this.props.children;
  }
}
```

### Context Pattern
```tsx
// AuthContext.tsx
import React, { createContext, useContext, useReducer } from 'react';

interface AuthState {
  user: User | null;
  isAuthenticated: boolean;
  loading: boolean;
}

interface AuthContextType extends AuthState {
  login: (user: User) => void;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

const authReducer = (state: AuthState, action: any): AuthState => {
  switch (action.type) {
    case 'LOGIN':
      return {
        ...state,
        user: action.payload,
        isAuthenticated: true
      };
    case 'LOGOUT':
      return {
        ...state,
        user: null,
        isAuthenticated: false
      };
    case 'SET_LOADING':
      return {
        ...state,
        loading: action.payload
      };
    default:
      return state;
  }
};

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [state, dispatch] = useReducer(authReducer, {
    user: null,
    isAuthenticated: false,
    loading: false
  });

  const login = (user: User) => dispatch({ type: 'LOGIN', payload: user });
  const logout = () => dispatch({ type: 'LOGOUT' });

  return (
    <AuthContext.Provider value={{ ...state, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
```

## 🚨 Common Pitfalls

1. **Don't mutate state directly**: Always use setState or state updater functions
2. **Avoid unnecessary re-renders**: Use memo, useMemo, and useCallback appropriately
3. **Don't forget cleanup**: Use useEffect cleanup functions for subscriptions
4. **Avoid prop drilling**: Use Context or state management for deeply nested props
5. **Don't use array index as key**: Use stable, unique identifiers

## 📚 Recommended Libraries

- **State Management**: Redux Toolkit, Zustand, Jotai
- **Routing**: React Router, Next.js Router
- **Styling**: Styled Components, Emotion, CSS Modules, TailwindCSS
- **Testing**: Jest, React Testing Library, Cypress
- **Forms**: React Hook Form, Formik
- **Data Fetching**: React Query, SWR, Apollo Client

## 🎯 AI Agent Best Practices

When assisting with React development:

1. **Always use TypeScript** for better type safety and developer experience
2. **Follow the component structure template** above
3. **Prefer functional components** with hooks over class components
4. **Use proper state management** patterns (Context for local, Redux/Zustand for global)
5. **Optimize for performance** with memoization and code splitting
6. **Write comprehensive tests** for components and hooks
7. **Follow accessibility guidelines** (ARIA attributes, semantic HTML)
8. **Use proper error handling** with Error Boundaries
9. **Implement proper loading states** and error states
10. **Follow React best practices** for hooks and lifecycle management

## 🔗 Additional Resources

- [React Documentation](https://react.dev/)
- [React Router](https://reactrouter.com/)
- [Redux Toolkit](https://redux-toolkit.js.org/)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [React Patterns](https://reactpatterns.com/)

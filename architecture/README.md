# Architecture Patterns Guide

## 🎯 AI Agent Instructions for Architecture

When working with architecture patterns in web development projects, follow these guidelines to ensure scalable, maintainable, and robust applications.

## 🏗️ Architecture Principles

### Core Principles
1. **Separation of Concerns** - Each component has a single responsibility
2. **Single Responsibility Principle** - One reason to change
3. **Open/Closed Principle** - Open for extension, closed for modification
4. **Liskov Substitution Principle** - Subtypes must be substitutable for base types
5. **Interface Segregation Principle** - Many specific interfaces are better than one general
6. **Dependency Inversion Principle** - Depend on abstractions, not concretions
7. **Don't Repeat Yourself (DRY)** - Avoid code duplication
8. **Keep It Simple, Stupid (KISS)** - Prefer simple solutions
9. **You Aren't Gonna Need It (YAGNI)** - Don't over-engineer
10. **Fail Fast** - Detect and handle errors early

### Design Patterns
1. **Creational Patterns** - Object creation patterns
2. **Structural Patterns** - Object composition patterns
3. **Behavioral Patterns** - Object interaction patterns
4. **Architectural Patterns** - High-level system organization

## 🎨 Frontend Architecture Patterns

### Component Architecture
```typescript
// Component hierarchy structure
interface ComponentArchitecture {
  // Presentation Layer
  components: {
    ui: Component[];           // Reusable UI components
    forms: Component[];        // Form-specific components
    layout: Component[];       // Layout components
    features: Component[];     // Feature-specific components
  };
  
  // Business Logic Layer
  services: {
    api: Service[];            // API services
    state: Service[];          // State management
    utils: Service[];          // Utility services
  };
  
  // Data Layer
  data: {
    models: Model[];           // Data models
    stores: Store[];           // Data stores
    hooks: Hook[];             // Data hooks
  };
}
```

### Container/Presentational Pattern
```typescript
// Container Component (Smart Component)
interface UserListContainerProps {
  onUserSelect: (user: User) => void;
}

export const UserListContainer: React.FC<UserListContainerProps> = ({
  onUserSelect
}) => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchUsers = async () => {
      try {
        setLoading(true);
        const userData = await UserService.getUsers();
        setUsers(userData);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Failed to fetch users');
      } finally {
        setLoading(false);
      }
    };

    fetchUsers();
  }, []);

  const handleUserClick = (user: User) => {
    onUserSelect(user);
  };

  return (
    <UserListPresentation
      users={users}
      loading={loading}
      error={error}
      onUserClick={handleUserClick}
    />
  );
};

// Presentational Component (Dumb Component)
interface UserListPresentationProps {
  users: User[];
  loading: boolean;
  error: string | null;
  onUserClick: (user: User) => void;
}

export const UserListPresentation: React.FC<UserListPresentationProps> = ({
  users,
  loading,
  error,
  onUserClick
}) => {
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div className="user-list">
      {users.map(user => (
        <UserCard
          key={user.id}
          user={user}
          onClick={() => onUserClick(user)}
        />
      ))}
    </div>
  );
};
```

### Higher-Order Component (HOC) Pattern
```typescript
// HOC for authentication
interface WithAuthProps {
  isAuthenticated: boolean;
  user: User | null;
}

export function withAuth<P extends object>(
  Component: React.ComponentType<P & WithAuthProps>
) {
  return function AuthenticatedComponent(props: P) {
    const { isAuthenticated, user } = useAuth();

    if (!isAuthenticated) {
      return <LoginPage />;
    }

    return <Component {...props} isAuthenticated={isAuthenticated} user={user} />;
  };
}

// Usage
const ProtectedUserProfile = withAuth(UserProfile);

// HOC for loading states
interface WithLoadingProps {
  loading: boolean;
}

export function withLoading<P extends object>(
  Component: React.ComponentType<P & WithLoadingProps>
) {
  return function LoadingComponent(props: P) {
    const [loading, setLoading] = useState(true);

    useEffect(() => {
      // Simulate loading
      const timer = setTimeout(() => setLoading(false), 1000);
      return () => clearTimeout(timer);
    }, []);

    if (loading) {
      return <div>Loading...</div>;
    }

    return <Component {...props} loading={loading} />;
  };
}
```

### Render Props Pattern
```typescript
// Render props for data fetching
interface DataFetcherProps<T> {
  url: string;
  children: (data: {
    data: T | null;
    loading: boolean;
    error: string | null;
    refetch: () => void;
  }) => React.ReactNode;
}

export function DataFetcher<T>({ url, children }: DataFetcherProps<T>) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchData = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      const response = await fetch(url);
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch data');
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  return <>{children({ data, loading, error, refetch: fetchData })}</>;
}

// Usage
<DataFetcher<User[]> url="/api/users">
  {({ data, loading, error, refetch }) => (
    <div>
      {loading && <div>Loading...</div>}
      {error && <div>Error: {error}</div>}
      {data && (
        <ul>
          {data.map(user => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      )}
      <button onClick={refetch}>Refresh</button>
    </div>
  )}
</DataFetcher>
```

### Compound Component Pattern
```typescript
// Compound component for modal
interface ModalContextType {
  isOpen: boolean;
  open: () => void;
  close: () => void;
}

const ModalContext = createContext<ModalContextType | null>(null);

export const Modal: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [isOpen, setIsOpen] = useState(false);

  const open = () => setIsOpen(true);
  const close = () => setIsOpen(false);

  return (
    <ModalContext.Provider value={{ isOpen, open, close }}>
      {children}
    </ModalContext.Provider>
  );
};

export const ModalTrigger: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const context = useContext(ModalContext);
  if (!context) throw new Error('ModalTrigger must be used within Modal');

  return (
    <div onClick={context.open}>
      {children}
    </div>
  );
};

export const ModalContent: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const context = useContext(ModalContext);
  if (!context) throw new Error('ModalContent must be used within Modal');

  if (!context.isOpen) return null;

  return (
    <div className="modal-overlay" onClick={context.close}>
      <div className="modal-content" onClick={e => e.stopPropagation()}>
        {children}
      </div>
    </div>
  );
};

export const ModalHeader: React.FC<{ children: React.ReactNode }> = ({ children }) => (
  <div className="modal-header">{children}</div>
);

export const ModalBody: React.FC<{ children: React.ReactNode }> = ({ children }) => (
  <div className="modal-body">{children}</div>
);

export const ModalFooter: React.FC<{ children: React.ReactNode }> = ({ children }) => (
  <div className="modal-footer">{children}</div>
);

// Usage
<Modal>
  <ModalTrigger>
    <button>Open Modal</button>
  </ModalTrigger>
  <ModalContent>
    <ModalHeader>
      <h2>Modal Title</h2>
    </ModalHeader>
    <ModalBody>
      <p>Modal content goes here</p>
    </ModalBody>
    <ModalFooter>
      <button>Close</button>
    </ModalFooter>
  </ModalContent>
</Modal>
```

## 🏪 State Management Patterns

### Redux Pattern
```typescript
// Store structure
interface RootState {
  user: UserState;
  posts: PostsState;
  ui: UIState;
}

// Action types
enum UserActionTypes {
  FETCH_USERS_REQUEST = 'FETCH_USERS_REQUEST',
  FETCH_USERS_SUCCESS = 'FETCH_USERS_SUCCESS',
  FETCH_USERS_FAILURE = 'FETCH_USERS_FAILURE',
  CREATE_USER_REQUEST = 'CREATE_USER_REQUEST',
  CREATE_USER_SUCCESS = 'CREATE_USER_SUCCESS',
  CREATE_USER_FAILURE = 'CREATE_USER_FAILURE',
}

// Actions
interface FetchUsersRequestAction {
  type: UserActionTypes.FETCH_USERS_REQUEST;
}

interface FetchUsersSuccessAction {
  type: UserActionTypes.FETCH_USERS_SUCCESS;
  payload: User[];
}

interface FetchUsersFailureAction {
  type: UserActionTypes.FETCH_USERS_FAILURE;
  payload: string;
}

type UserAction = 
  | FetchUsersRequestAction 
  | FetchUsersSuccessAction 
  | FetchUsersFailureAction;

// Reducer
const userReducer = (state: UserState = initialState, action: UserAction): UserState => {
  switch (action.type) {
    case UserActionTypes.FETCH_USERS_REQUEST:
      return {
        ...state,
        loading: true,
        error: null,
      };
    case UserActionTypes.FETCH_USERS_SUCCESS:
      return {
        ...state,
        loading: false,
        users: action.payload,
        error: null,
      };
    case UserActionTypes.FETCH_USERS_FAILURE:
      return {
        ...state,
        loading: false,
        error: action.payload,
      };
    default:
      return state;
  }
};

// Action creators
export const fetchUsersRequest = (): FetchUsersRequestAction => ({
  type: UserActionTypes.FETCH_USERS_REQUEST,
});

export const fetchUsersSuccess = (users: User[]): FetchUsersSuccessAction => ({
  type: UserActionTypes.FETCH_USERS_SUCCESS,
  payload: users,
});

export const fetchUsersFailure = (error: string): FetchUsersFailureAction => ({
  type: UserActionTypes.FETCH_USERS_FAILURE,
  payload: error,
});

// Thunk action creator
export const fetchUsers = (): ThunkAction<void, RootState, unknown, UserAction> => {
  return async (dispatch) => {
    dispatch(fetchUsersRequest());
    try {
      const users = await UserService.getUsers();
      dispatch(fetchUsersSuccess(users));
    } catch (error) {
      dispatch(fetchUsersFailure(error instanceof Error ? error.message : 'Failed to fetch users'));
    }
  };
};
```

### Context API Pattern
```typescript
// Context definition
interface AuthContextType {
  user: User | null;
  isAuthenticated: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  loading: boolean;
  error: string | null;
}

const AuthContext = createContext<AuthContextType | null>(null);

// Provider component
export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const isAuthenticated = !!user;

  const login = async (email: string, password: string) => {
    try {
      setLoading(true);
      setError(null);
      const userData = await AuthService.login(email, password);
      setUser(userData);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Login failed');
      throw err;
    } finally {
      setLoading(false);
    }
  };

  const logout = () => {
    setUser(null);
    AuthService.logout();
  };

  useEffect(() => {
    const checkAuth = async () => {
      try {
        const userData = await AuthService.getCurrentUser();
        setUser(userData);
      } catch (err) {
        setUser(null);
      } finally {
        setLoading(false);
      }
    };

    checkAuth();
  }, []);

  const value: AuthContextType = {
    user,
    isAuthenticated,
    login,
    logout,
    loading,
    error,
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
};

// Custom hook
export const useAuth = (): AuthContextType => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
```

### Zustand Pattern
```typescript
// Store definition
interface UserStore {
  users: User[];
  currentUser: User | null;
  loading: boolean;
  error: string | null;
  
  // Actions
  fetchUsers: () => Promise<void>;
  createUser: (userData: CreateUserRequest) => Promise<void>;
  updateUser: (id: string, updates: UpdateUserRequest) => Promise<void>;
  deleteUser: (id: string) => Promise<void>;
  setCurrentUser: (user: User | null) => void;
  clearError: () => void;
}

export const useUserStore = create<UserStore>((set, get) => ({
  users: [],
  currentUser: null,
  loading: false,
  error: null,

  fetchUsers: async () => {
    set({ loading: true, error: null });
    try {
      const users = await UserService.getUsers();
      set({ users, loading: false });
    } catch (error) {
      set({ 
        error: error instanceof Error ? error.message : 'Failed to fetch users',
        loading: false 
      });
    }
  },

  createUser: async (userData) => {
    set({ loading: true, error: null });
    try {
      const newUser = await UserService.createUser(userData);
      set(state => ({ 
        users: [...state.users, newUser], 
        loading: false 
      }));
    } catch (error) {
      set({ 
        error: error instanceof Error ? error.message : 'Failed to create user',
        loading: false 
      });
    }
  },

  updateUser: async (id, updates) => {
    set({ loading: true, error: null });
    try {
      const updatedUser = await UserService.updateUser(id, updates);
      set(state => ({
        users: state.users.map(user => 
          user.id === id ? updatedUser : user
        ),
        currentUser: state.currentUser?.id === id ? updatedUser : state.currentUser,
        loading: false
      }));
    } catch (error) {
      set({ 
        error: error instanceof Error ? error.message : 'Failed to update user',
        loading: false 
      });
    }
  },

  deleteUser: async (id) => {
    set({ loading: true, error: null });
    try {
      await UserService.deleteUser(id);
      set(state => ({
        users: state.users.filter(user => user.id !== id),
        currentUser: state.currentUser?.id === id ? null : state.currentUser,
        loading: false
      }));
    } catch (error) {
      set({ 
        error: error instanceof Error ? error.message : 'Failed to delete user',
        loading: false 
      });
    }
  },

  setCurrentUser: (user) => set({ currentUser: user }),
  clearError: () => set({ error: null }),
}));
```

## 🔧 Service Layer Patterns

### Repository Pattern
```typescript
// Base repository interface
interface Repository<T, ID = string> {
  findById(id: ID): Promise<T | null>;
  findAll(): Promise<T[]>;
  create(entity: Omit<T, 'id'>): Promise<T>;
  update(id: ID, updates: Partial<T>): Promise<T>;
  delete(id: ID): Promise<void>;
  findWhere(conditions: Partial<T>): Promise<T[]>;
}

// User repository implementation
class UserRepository implements Repository<User> {
  constructor(private apiClient: ApiClient) {}

  async findById(id: string): Promise<User | null> {
    try {
      const response = await this.apiClient.get(`/users/${id}`);
      return response.data;
    } catch (error) {
      if (error.status === 404) return null;
      throw error;
    }
  }

  async findAll(): Promise<User[]> {
    const response = await this.apiClient.get('/users');
    return response.data;
  }

  async create(userData: Omit<User, 'id'>): Promise<User> {
    const response = await this.apiClient.post('/users', userData);
    return response.data;
  }

  async update(id: string, updates: Partial<User>): Promise<User> {
    const response = await this.apiClient.put(`/users/${id}`, updates);
    return response.data;
  }

  async delete(id: string): Promise<void> {
    await this.apiClient.delete(`/users/${id}`);
  }

  async findWhere(conditions: Partial<User>): Promise<User[]> {
    const queryParams = new URLSearchParams();
    Object.entries(conditions).forEach(([key, value]) => {
      if (value !== undefined) {
        queryParams.append(key, value.toString());
      }
    });
    
    const response = await this.apiClient.get(`/users?${queryParams}`);
    return response.data;
  }
}
```

### Service Layer Pattern
```typescript
// Base service interface
interface Service<T, CreateRequest, UpdateRequest> {
  getById(id: string): Promise<T>;
  getAll(options?: QueryOptions): Promise<PaginatedResponse<T>>;
  create(data: CreateRequest): Promise<T>;
  update(id: string, data: UpdateRequest): Promise<T>;
  delete(id: string): Promise<void>;
  search(query: string, options?: QueryOptions): Promise<PaginatedResponse<T>>;
}

// User service implementation
class UserService implements Service<User, CreateUserRequest, UpdateUserRequest> {
  constructor(private userRepository: UserRepository) {}

  async getById(id: string): Promise<User> {
    const user = await this.userRepository.findById(id);
    if (!user) {
      throw new Error('User not found');
    }
    return user;
  }

  async getAll(options: QueryOptions = {}): Promise<PaginatedResponse<User>> {
    const { page = 1, limit = 10, sortBy, sortOrder } = options;
    
    const users = await this.userRepository.findAll();
    
    // Apply sorting
    if (sortBy) {
      users.sort((a, b) => {
        const aValue = a[sortBy as keyof User];
        const bValue = b[sortBy as keyof User];
        
        if (sortOrder === 'desc') {
          return aValue > bValue ? -1 : aValue < bValue ? 1 : 0;
        }
        return aValue > bValue ? 1 : aValue < bValue ? -1 : 0;
      });
    }
    
    // Apply pagination
    const startIndex = (page - 1) * limit;
    const endIndex = startIndex + limit;
    const paginatedUsers = users.slice(startIndex, endIndex);
    
    return {
      data: paginatedUsers,
      pagination: {
        page,
        limit,
        total: users.length,
        totalPages: Math.ceil(users.length / limit),
      },
    };
  }

  async create(data: CreateUserRequest): Promise<User> {
    // Validate input
    this.validateCreateUserRequest(data);
    
    // Check if user already exists
    const existingUsers = await this.userRepository.findWhere({ email: data.email });
    if (existingUsers.length > 0) {
      throw new Error('User with this email already exists');
    }
    
    // Create user
    const userData: Omit<User, 'id'> = {
      ...data,
      createdAt: new Date(),
      updatedAt: new Date(),
    };
    
    return this.userRepository.create(userData);
  }

  async update(id: string, data: UpdateUserRequest): Promise<User> {
    // Validate input
    this.validateUpdateUserRequest(data);
    
    // Check if user exists
    const existingUser = await this.userRepository.findById(id);
    if (!existingUser) {
      throw new Error('User not found');
    }
    
    // Update user
    const updateData: Partial<User> = {
      ...data,
      updatedAt: new Date(),
    };
    
    return this.userRepository.update(id, updateData);
  }

  async delete(id: string): Promise<void> {
    // Check if user exists
    const existingUser = await this.userRepository.findById(id);
    if (!existingUser) {
      throw new Error('User not found');
    }
    
    await this.userRepository.delete(id);
  }

  async search(query: string, options: QueryOptions = {}): Promise<PaginatedResponse<User>> {
    const users = await this.userRepository.findAll();
    
    // Filter users based on search query
    const filteredUsers = users.filter(user => 
      user.name.toLowerCase().includes(query.toLowerCase()) ||
      user.email.toLowerCase().includes(query.toLowerCase())
    );
    
    // Apply pagination
    const { page = 1, limit = 10 } = options;
    const startIndex = (page - 1) * limit;
    const endIndex = startIndex + limit;
    const paginatedUsers = filteredUsers.slice(startIndex, endIndex);
    
    return {
      data: paginatedUsers,
      pagination: {
        page,
        limit,
        total: filteredUsers.length,
        totalPages: Math.ceil(filteredUsers.length / limit),
      },
    };
  }

  private validateCreateUserRequest(data: CreateUserRequest): void {
    if (!data.name || data.name.trim().length === 0) {
      throw new Error('Name is required');
    }
    if (!data.email || !this.isValidEmail(data.email)) {
      throw new Error('Valid email is required');
    }
  }

  private validateUpdateUserRequest(data: UpdateUserRequest): void {
    if (data.email && !this.isValidEmail(data.email)) {
      throw new Error('Valid email is required');
    }
  }

  private isValidEmail(email: string): boolean {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
  }
}
```

## 🎯 Architecture Best Practices

### General Guidelines
1. **Start simple** - Begin with basic patterns and evolve
2. **Follow SOLID principles** - Maintain good object-oriented design
3. **Use composition over inheritance** - Prefer composition
4. **Implement proper error handling** - Handle errors gracefully
5. **Use TypeScript** - Leverage type safety
6. **Write tests** - Ensure code quality with tests
7. **Document decisions** - Explain architectural choices
8. **Refactor regularly** - Keep code clean and maintainable

### Performance Considerations
1. **Lazy loading** - Load components and data when needed
2. **Code splitting** - Split code into smaller chunks
3. **Memoization** - Cache expensive calculations
4. **Virtual scrolling** - Handle large lists efficiently
5. **Image optimization** - Optimize images for web
6. **Bundle optimization** - Minimize bundle size
7. **Caching strategies** - Implement proper caching
8. **Database optimization** - Optimize queries and indexes

### Security Considerations
1. **Input validation** - Validate all user inputs
2. **Authentication** - Implement proper authentication
3. **Authorization** - Control access to resources
4. **Data sanitization** - Clean data before processing
5. **HTTPS** - Use secure connections
6. **CORS** - Configure cross-origin requests properly
7. **Rate limiting** - Prevent abuse
8. **Error handling** - Don't expose sensitive information

## 🎯 AI Agent Best Practices

When assisting with architecture patterns:

1. **Follow established patterns** in the project
2. **Use appropriate patterns** for the specific use case
3. **Maintain consistency** across the codebase
4. **Consider scalability** when choosing patterns
5. **Implement proper error handling** in all patterns
6. **Use TypeScript** for better type safety
7. **Follow SOLID principles** in all implementations
8. **Write comprehensive tests** for all patterns
9. **Document architectural decisions** clearly
10. **Consider performance implications** of pattern choices

## 🔗 Additional Resources

- [Architecture Patterns Guide](https://architecture-patterns.org/)
- [Design Patterns](https://design-patterns.org/)
- [SOLID Principles](https://solid-principles.org/)
- [Clean Architecture](https://clean-architecture.org/)
- [Domain-Driven Design](https://domain-driven-design.org/)

# Naming Conventions Guide

## 🎯 AI Agent Instructions for Naming Conventions

When working with naming conventions in web development projects, follow these guidelines to ensure consistency, readability, and maintainability across the codebase.

## 📝 General Naming Principles

### Core Principles
1. **Be descriptive** - Names should clearly indicate purpose and functionality
2. **Be consistent** - Use the same pattern throughout the project
3. **Be concise** - Avoid unnecessarily long names while maintaining clarity
4. **Be meaningful** - Avoid abbreviations unless they're widely understood
5. **Be searchable** - Use names that are easy to find and replace
6. **Be future-proof** - Consider how names will scale with the project

### Naming Case Conventions
```
camelCase          # Variables, functions, methods, properties
PascalCase         # Classes, components, interfaces, types
kebab-case         # Files, directories, URLs, CSS classes
snake_case         # Constants, environment variables
UPPER_CASE         # Global constants, enum values
```

## 🏗️ Component Naming

### React Components
```typescript
// Component files
UserProfile.tsx                 # PascalCase for component files
UserProfileCard.tsx             # Descriptive component names
UserProfileSettings.tsx         # Feature-specific components

// Component names
const UserProfile = () => {     # PascalCase for component names
  return <div>...</div>;
};

const UserProfileCard = () => { # Descriptive component names
  return <div>...</div>;
};

// Component props
interface UserProfileProps {    # ComponentName + Props suffix
  userId: string;               # camelCase for prop names
  userName: string;
  userEmail: string;
  onUserUpdate: (user: User) => void;  # on + EventName pattern
  isUserActive: boolean;        # is/has/can prefixes for booleans
}

// Component variants
const UserProfile = () => {     # Main component
  return <div>...</div>;
};

const UserProfileSkeleton = () => {  # Loading state
  return <div>...</div>;
};

const UserProfileError = () => {     # Error state
  return <div>...</div>;
};
```

### Vue Components
```vue
<!-- Component files -->
UserProfile.vue                 # PascalCase for component files
UserProfileCard.vue             # Descriptive component names
UserProfileSettings.vue         # Feature-specific components

<!-- Component names -->
<script setup lang="ts">
// Component name in PascalCase
defineOptions({
  name: 'UserProfile'
});

// Props in camelCase
interface Props {
  userId: string;
  userName: string;
  userEmail: string;
  onUserUpdate: (user: User) => void;
  isUserActive: boolean;
}

const props = defineProps<Props>();
</script>
```

### Angular Components
```typescript
// Component files
user-profile.component.ts        # kebab-case for component files
user-profile-card.component.ts   # Descriptive component names
user-profile-settings.component.ts # Feature-specific components

// Component classes
@Component({
  selector: 'app-user-profile',  # app- prefix for app components
  templateUrl: './user-profile.component.html',
  styleUrls: ['./user-profile.component.scss']
})
export class UserProfileComponent {  # ComponentName + Component suffix
  @Input() userId: string;       # camelCase for properties
  @Input() userName: string;
  @Input() userEmail: string;
  @Output() userUpdate = new EventEmitter<User>(); // camelCase for events
  @Input() isUserActive: boolean;
}
```

## 🎨 CSS and Styling Naming

### CSS Classes
```css
/* BEM Methodology */
.user-profile {                  /* Block */
  padding: 1rem;
  border: 1px solid #ccc;
}

.user-profile__header {          /* Element */
  font-size: 1.5rem;
  font-weight: bold;
}

.user-profile__header--large {   /* Modifier */
  font-size: 2rem;
}

.user-profile--featured {        /* Block modifier */
  border-color: #007bff;
}

/* Utility classes */
.text-center {                   /* kebab-case for utility classes */
  text-align: center;
}

.bg-primary {                    /* bg- prefix for background */
  background-color: var(--primary-color);
}

.text-primary {                  /* text- prefix for text color */
  color: var(--primary-color);
}

/* Component classes */
.btn {                          /* Base component class */
  display: inline-block;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0.25rem;
}

.btn--primary {                 /* Component variant */
  background-color: #007bff;
  color: white;
}

.btn--large {                   /* Component size */
  padding: 0.75rem 1.5rem;
  font-size: 1.125rem;
}
```

### CSS Custom Properties
```css
/* CSS Variables */
:root {
  /* Color variables */
  --color-primary: #007bff;
  --color-primary-dark: #0056b3;
  --color-primary-light: #66b3ff;
  
  /* Spacing variables */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  
  /* Typography variables */
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  
  /* Layout variables */
  --container-max-width: 1200px;
  --header-height: 4rem;
  --sidebar-width: 16rem;
}
```

## 🔧 Function and Method Naming

### JavaScript/TypeScript Functions
```typescript
// Function names in camelCase
const getUserById = (id: string) => {     // get + Entity + By + Field
  return userService.getUser(id);
};

const createUser = (userData: User) => {  // create + Entity
  return userService.createUser(userData);
};

const updateUser = (id: string, data: User) => { // update + Entity
  return userService.updateUser(id, data);
};

const deleteUser = (id: string) => {      // delete + Entity
  return userService.deleteUser(id);
};

// Event handlers
const handleUserClick = (user: User) => { // handle + Event + Entity
  console.log('User clicked:', user);
};

const handleFormSubmit = (data: FormData) => { // handle + Event + Entity
  console.log('Form submitted:', data);
};

// Boolean functions
const isUserActive = (user: User) => {    // is + Entity + State
  return user.status === 'active';
};

const hasUserPermission = (user: User, permission: string) => { // has + Entity + Property
  return user.permissions.includes(permission);
};

const canUserEdit = (user: User, resource: Resource) => { // can + Entity + Action
  return user.role === 'admin' || resource.ownerId === user.id;
};

// Utility functions
const formatDate = (date: Date) => {      // format + Entity
  return date.toLocaleDateString();
};

const validateEmail = (email: string) => { // validate + Entity
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
};

const parseJson = (json: string) => {     // parse + Entity
  return JSON.parse(json);
};
```

### Async Functions
```typescript
// Async function naming
const fetchUserData = async (id: string) => { // fetch + Entity + Data
  const response = await api.get(`/users/${id}`);
  return response.data;
};

const loadUserPosts = async (userId: string) => { // load + Entity + Entity
  const response = await api.get(`/users/${userId}/posts`);
  return response.data;
};

const saveUserSettings = async (settings: UserSettings) => { // save + Entity + Entity
  const response = await api.post('/users/settings', settings);
  return response.data;
};
```

## 📊 Variable Naming

### JavaScript/TypeScript Variables
```typescript
// Primitive variables
const userName = 'John Doe';           // camelCase for variables
const userAge = 25;                    // descriptive names
const isUserLoggedIn = true;           // boolean with is/has/can prefix
const userPermissions = ['read', 'write']; // descriptive names

// Object variables
const userProfile = {                  // camelCase for objects
  name: 'John Doe',
  email: 'john@example.com',
  age: 25
};

const userSettings = {                 // descriptive names
  theme: 'dark',
  language: 'en',
  notifications: true
};

// Array variables
const userList = [];                   // camelCase for arrays
const activeUsers = [];                // descriptive names
const userEmails = [];                 // descriptive names

// Function variables
const handleUserClick = (user: User) => { // descriptive function names
  console.log('User clicked:', user);
};

const processUserData = (data: User[]) => { // descriptive function names
  return data.map(user => ({
    ...user,
    displayName: `${user.firstName} ${user.lastName}`
  }));
};
```

### Constants
```typescript
// Global constants
const API_BASE_URL = 'https://api.example.com'; // UPPER_CASE for constants
const MAX_RETRY_ATTEMPTS = 3;                    // descriptive names
const DEFAULT_PAGE_SIZE = 10;                    // descriptive names

// Enum values
enum UserRole {
  ADMIN = 'admin',                    // UPPER_CASE for enum values
  USER = 'user',
  MODERATOR = 'moderator'
}

// Configuration constants
const CONFIG = {
  apiUrl: 'https://api.example.com',  // camelCase for object properties
  timeout: 5000,
  retries: 3
} as const;

// Environment variables
const env = {
  NODE_ENV: process.env.NODE_ENV,     // UPPER_CASE for env vars
  API_URL: process.env.API_URL,
  DATABASE_URL: process.env.DATABASE_URL
};
```

## 🗂️ File and Directory Naming

### File Naming
```
# Component files
UserProfile.tsx                 # PascalCase for component files
user-profile.vue                # kebab-case for Vue components
user-profile.component.ts       # kebab-case for Angular components

# Service files
userService.ts                  # camelCase for service files
apiClient.ts                    # camelCase for utility files
authService.ts                  # camelCase for service files

# Type files
user.types.ts                   # camelCase.types.ts
api.types.ts                    # camelCase.types.ts
common.types.ts                 # camelCase.types.ts

# Utility files
dateUtils.ts                    # camelCase + Utils suffix
stringUtils.ts                  # camelCase + Utils suffix
validationUtils.ts              # camelCase + Utils suffix

# Hook files (React)
useUser.ts                      # use + PascalCase
useApi.ts                       # use + PascalCase
useLocalStorage.ts              # use + PascalCase

# Test files
Button.test.tsx                 # ComponentName.test.tsx
userService.test.ts             # serviceName.test.ts
dateUtils.test.ts               # utilityName.test.ts

# Configuration files
.env                            # .env for environment variables
.env.local                      # .env.local for local overrides
.env.development                # .env.development for dev environment
.env.production                 # .env.production for prod environment
```

### Directory Naming
```
# Feature-based directories
src/
├── features/                   # kebab-case for directories
│   ├── user-management/        # descriptive names
│   ├── product-catalog/        # descriptive names
│   └── order-processing/       # descriptive names

# Component directories
src/
├── components/                 # kebab-case for directories
│   ├── ui/                     # short, clear names
│   ├── forms/                  # descriptive names
│   └── layout/                 # descriptive names

# Service directories
src/
├── services/                   # kebab-case for directories
│   ├── api/                    # short, clear names
│   ├── auth/                   # descriptive names
│   └── storage/                # descriptive names

# Asset directories
src/
├── assets/                     # kebab-case for directories
│   ├── images/                 # descriptive names
│   ├── icons/                  # descriptive names
│   └── fonts/                  # descriptive names
```

## 🎯 API and Endpoint Naming

### REST API Endpoints
```
# Resource-based URLs
GET    /users                   # Get all users
GET    /users/:id               # Get user by ID
POST   /users                   # Create new user
PUT    /users/:id               # Update user
DELETE /users/:id               # Delete user

# Nested resources
GET    /users/:id/posts         # Get user's posts
POST   /users/:id/posts         # Create post for user
GET    /users/:id/posts/:postId # Get specific post
PUT    /users/:id/posts/:postId # Update specific post
DELETE /users/:id/posts/:postId # Delete specific post

# Action-based endpoints
POST   /users/:id/activate      # Activate user
POST   /users/:id/deactivate    # Deactivate user
POST   /users/:id/reset-password # Reset user password
POST   /users/:id/upload-avatar # Upload user avatar

# Query parameters
GET    /users?page=1&limit=10   # Pagination
GET    /users?search=john       # Search
GET    /users?role=admin        # Filter by role
GET    /users?active=true       # Filter by status
```

### GraphQL Naming
```graphql
# Queries
type Query {
  user(id: ID!): User           # camelCase for field names
  users(filter: UserFilter): [User] # camelCase for field names
  userPosts(userId: ID!): [Post] # camelCase for field names
}

# Mutations
type Mutation {
  createUser(input: CreateUserInput!): User # camelCase for field names
  updateUser(id: ID!, input: UpdateUserInput!): User # camelCase for field names
  deleteUser(id: ID!): Boolean # camelCase for field names
}

# Types
type User {                     # PascalCase for type names
  id: ID!                       # camelCase for field names
  firstName: String!            # camelCase for field names
  lastName: String!             # camelCase for field names
  email: String!                # camelCase for field names
  isActive: Boolean!            # camelCase for field names
  createdAt: DateTime!          # camelCase for field names
  updatedAt: DateTime!          # camelCase for field names
}

# Input types
input CreateUserInput {         # PascalCase for input types
  firstName: String!            # camelCase for field names
  lastName: String!             # camelCase for field names
  email: String!                # camelCase for field names
  password: String!             # camelCase for field names
}
```

## 🗄️ Database Naming

### Table Names
```sql
-- Table names in snake_case
users                          -- Plural, descriptive names
user_profiles                  -- Descriptive names
user_permissions               -- Descriptive names
posts                          -- Plural, descriptive names
post_comments                  -- Descriptive names
post_likes                     -- Descriptive names
categories                     -- Plural, descriptive names
category_posts                 -- Junction tables
```

### Column Names
```sql
-- Column names in snake_case
CREATE TABLE users (
  id SERIAL PRIMARY KEY,       -- Primary key
  first_name VARCHAR(100),     -- Descriptive names
  last_name VARCHAR(100),      -- Descriptive names
  email VARCHAR(255),          -- Descriptive names
  password_hash VARCHAR(255),  -- Descriptive names
  is_active BOOLEAN DEFAULT true, -- Boolean with is_ prefix
  created_at TIMESTAMP DEFAULT NOW(), -- Timestamp with _at suffix
  updated_at TIMESTAMP DEFAULT NOW()  -- Timestamp with _at suffix
);

-- Foreign key columns
CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  title VARCHAR(255),
  content TEXT,
  author_id INTEGER REFERENCES users(id), -- Entity_id for foreign keys
  category_id INTEGER REFERENCES categories(id), -- Entity_id for foreign keys
  published_at TIMESTAMP,      -- Timestamp with _at suffix
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Index Names
```sql
-- Index names in snake_case
CREATE INDEX idx_users_email ON users(email);           -- idx_ + table + column
CREATE INDEX idx_users_created_at ON users(created_at); -- idx_ + table + column
CREATE INDEX idx_posts_author_id ON posts(author_id);   -- idx_ + table + column
CREATE INDEX idx_posts_published_at ON posts(published_at); -- idx_ + table + column

-- Unique index names
CREATE UNIQUE INDEX uq_users_email ON users(email);     -- uq_ + table + column
CREATE UNIQUE INDEX uq_users_username ON users(username); -- uq_ + table + column

-- Composite index names
CREATE INDEX idx_posts_author_published ON posts(author_id, published_at); -- idx_ + table + columns
```

## 🎨 CSS Class Naming

### BEM Methodology
```css
/* Block */
.user-profile {                 /* Block name in kebab-case */
  padding: 1rem;
  border: 1px solid #ccc;
}

/* Element */
.user-profile__header {         /* Block__Element */
  font-size: 1.5rem;
  font-weight: bold;
}

.user-profile__content {        /* Block__Element */
  padding: 1rem 0;
}

.user-profile__footer {         /* Block__Element */
  border-top: 1px solid #eee;
  padding-top: 1rem;
}

/* Modifier */
.user-profile--featured {       /* Block--Modifier */
  border-color: #007bff;
  background-color: #f8f9fa;
}

.user-profile__header--large {  /* Block__Element--Modifier */
  font-size: 2rem;
}

/* State */
.user-profile.is-active {       /* Block.is-state */
  opacity: 1;
}

.user-profile.is-loading {      /* Block.is-state */
  opacity: 0.5;
  pointer-events: none;
}
```

### Utility Classes
```css
/* Spacing utilities */
.m-0 { margin: 0; }             /* m- + value */
.m-1 { margin: 0.25rem; }       /* m- + value */
.m-2 { margin: 0.5rem; }        /* m- + value */
.m-3 { margin: 0.75rem; }       /* m- + value */
.m-4 { margin: 1rem; }          /* m- + value */

.p-0 { padding: 0; }            /* p- + value */
.p-1 { padding: 0.25rem; }      /* p- + value */
.p-2 { padding: 0.5rem; }       /* p- + value */
.p-3 { padding: 0.75rem; }      /* p- + value */
.p-4 { padding: 1rem; }         /* p- + value */

/* Text utilities */
.text-left { text-align: left; }    /* text- + alignment */
.text-center { text-align: center; } /* text- + alignment */
.text-right { text-align: right; }   /* text- + alignment */

.text-xs { font-size: 0.75rem; }    /* text- + size */
.text-sm { font-size: 0.875rem; }   /* text- + size */
.text-md { font-size: 1rem; }       /* text- + size */
.text-lg { font-size: 1.125rem; }   /* text- + size */
.text-xl { font-size: 1.25rem; }    /* text- + size */

/* Color utilities */
.text-primary { color: var(--primary-color); }     /* text- + color */
.text-secondary { color: var(--secondary-color); } /* text- + color */
.text-success { color: var(--success-color); }     /* text- + color */
.text-danger { color: var(--danger-color); }       /* text- + color */
.text-warning { color: var(--warning-color); }     /* text- + color */
.text-info { color: var(--info-color); }           /* text- + color */

.bg-primary { background-color: var(--primary-color); }     /* bg- + color */
.bg-secondary { background-color: var(--secondary-color); } /* bg- + color */
.bg-success { background-color: var(--success-color); }     /* bg- + color */
.bg-danger { background-color: var(--danger-color); }       /* bg- + color */
.bg-warning { background-color: var(--warning-color); }     /* bg- + color */
.bg-info { background-color: var(--info-color); }           /* bg- + color */
```

## 🎯 Naming Convention Best Practices

### General Guidelines
1. **Be consistent** - Use the same pattern throughout the project
2. **Be descriptive** - Names should clearly indicate purpose
3. **Be concise** - Avoid unnecessarily long names
4. **Be meaningful** - Avoid abbreviations unless widely understood
5. **Be searchable** - Use names that are easy to find and replace
6. **Be future-proof** - Consider how names will scale

### Language-Specific Guidelines
1. **JavaScript/TypeScript** - Use camelCase for variables and functions, PascalCase for classes
2. **CSS** - Use kebab-case for classes, BEM methodology for components
3. **HTML** - Use kebab-case for attributes and data attributes
4. **SQL** - Use snake_case for tables and columns
5. **Files** - Use kebab-case for files and directories
6. **APIs** - Use kebab-case for URLs, camelCase for JSON properties

### Team Guidelines
1. **Document conventions** - Keep naming conventions documented
2. **Use linters** - Configure linters to enforce naming conventions
3. **Code reviews** - Review naming conventions in code reviews
4. **Training** - Train team members on naming conventions
5. **Consistency** - Maintain consistency across the project
6. **Evolution** - Update conventions as the project evolves

## 🎯 AI Agent Best Practices

When assisting with naming conventions:

1. **Follow established patterns** in the project
2. **Use consistent naming** across all code elements
3. **Choose descriptive names** that clearly indicate purpose
4. **Follow language-specific conventions** for the technology stack
5. **Use appropriate case conventions** for different types of elements
6. **Avoid abbreviations** unless they're widely understood
7. **Consider searchability** when choosing names
8. **Maintain consistency** with existing code patterns
9. **Use meaningful prefixes and suffixes** for clarity
10. **Follow the established naming standards** for the project

## 🔗 Additional Resources

- [Naming Conventions Guide](https://naming-conventions.org/)
- [BEM Methodology](https://bem.info/)
- [CSS Naming Conventions](https://css-naming.org/)
- [JavaScript Naming Conventions](https://js-naming.org/)
- [API Naming Conventions](https://api-naming.org/)

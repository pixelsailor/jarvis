# Dexie Development Guide

## 🎯 AI Agent Instructions for Dexie

When working with Dexie projects, follow these guidelines to ensure optimal AI assistance and efficient client-side database management.

## 📁 Project Structure

```
src/
├── db/
│   ├── index.ts             # Main database configuration
│   ├── schema.ts            # Database schema definitions
│   ├── migrations/          # Database migrations
│   │   ├── 001-initial.ts
│   │   ├── 002-add-users.ts
│   │   └── 003-add-posts.ts
│   └── types.ts             # TypeScript type definitions
├── services/
│   ├── userService.ts       # User-related database operations
│   ├── postService.ts       # Post-related database operations
│   └── index.ts             # Service exports
├── hooks/
│   ├── useUsers.ts          # React hooks for user operations
│   ├── usePosts.ts          # React hooks for post operations
│   └── index.ts             # Hook exports
└── utils/
    ├── dbUtils.ts           # Database utility functions
    └── validation.ts        # Data validation utilities
```

## 🗄️ Database Configuration

### Main Database Setup
```typescript
// db/index.ts
import Dexie, { Table } from 'dexie';
import { User, Post, Comment, Category } from './types';

export class AppDatabase extends Dexie {
  users!: Table<User>;
  posts!: Table<Post>;
  comments!: Table<Comment>;
  categories!: Table<Category>;

  constructor() {
    super('AppDatabase');
    
    this.version(1).stores({
      users: '++id, email, username, createdAt, updatedAt',
      posts: '++id, title, slug, content, authorId, categoryId, published, createdAt, updatedAt',
      comments: '++id, content, authorId, postId, parentId, createdAt, updatedAt',
      categories: '++id, name, slug, description, createdAt, updatedAt'
    });

    // Add hooks for data validation and transformation
    this.users.hook('creating', (primKey, obj, trans) => {
      obj.createdAt = new Date();
      obj.updatedAt = new Date();
    });

    this.users.hook('updating', (modifications, primKey, obj, trans) => {
      modifications.updatedAt = new Date();
    });

    this.posts.hook('creating', (primKey, obj, trans) => {
      obj.createdAt = new Date();
      obj.updatedAt = new Date();
      obj.slug = obj.slug || generateSlug(obj.title);
    });

    this.posts.hook('updating', (modifications, primKey, obj, trans) => {
      modifications.updatedAt = new Date();
      if (modifications.title && !modifications.slug) {
        modifications.slug = generateSlug(modifications.title);
      }
    });
  }
}

export const db = new AppDatabase();

// Utility function for generating slugs
function generateSlug(title: string): string {
  return title
    .toLowerCase()
    .replace(/[^a-z0-9 -]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-')
    .trim();
}
```

### Type Definitions
```typescript
// db/types.ts
export interface User {
  id?: number;
  email: string;
  username: string;
  firstName?: string;
  lastName?: string;
  avatar?: string;
  role: 'admin' | 'user' | 'moderator';
  isActive: boolean;
  preferences: UserPreferences;
  createdAt?: Date;
  updatedAt?: Date;
}

export interface UserPreferences {
  theme: 'light' | 'dark' | 'auto';
  language: string;
  notifications: {
    email: boolean;
    push: boolean;
    sms: boolean;
  };
  privacy: {
    profileVisibility: 'public' | 'private' | 'friends';
    showEmail: boolean;
    showLastSeen: boolean;
  };
}

export interface Post {
  id?: number;
  title: string;
  slug: string;
  content: string;
  excerpt?: string;
  authorId: number;
  categoryId: number;
  tags: string[];
  featuredImage?: string;
  published: boolean;
  publishedAt?: Date;
  viewCount: number;
  likeCount: number;
  commentCount: number;
  createdAt?: Date;
  updatedAt?: Date;
}

export interface Comment {
  id?: number;
  content: string;
  authorId: number;
  postId: number;
  parentId?: number;
  isApproved: boolean;
  likeCount: number;
  createdAt?: Date;
  updatedAt?: Date;
}

export interface Category {
  id?: number;
  name: string;
  slug: string;
  description?: string;
  color?: string;
  icon?: string;
  postCount: number;
  createdAt?: Date;
  updatedAt?: Date;
}

// Index types for better query performance
export interface UserIndexes {
  email: string;
  username: string;
  role: string;
  isActive: boolean;
  createdAt: Date;
}

export interface PostIndexes {
  title: string;
  slug: string;
  authorId: number;
  categoryId: number;
  published: boolean;
  publishedAt: Date;
  createdAt: Date;
}

export interface CommentIndexes {
  authorId: number;
  postId: number;
  parentId: number;
  isApproved: boolean;
  createdAt: Date;
}
```

## 🔧 Service Layer Patterns

### User Service
```typescript
// services/userService.ts
import { db } from '../db';
import { User, UserPreferences } from '../db/types';
import { validateUser, validateUserPreferences } from '../utils/validation';

export class UserService {
  // Create a new user
  static async createUser(userData: Omit<User, 'id' | 'createdAt' | 'updatedAt'>): Promise<number> {
    try {
      // Validate user data
      const validatedData = validateUser(userData);
      
      // Check if user already exists
      const existingUser = await db.users
        .where('email')
        .equals(validatedData.email)
        .first();
      
      if (existingUser) {
        throw new Error('User with this email already exists');
      }

      // Create user
      const userId = await db.users.add(validatedData);
      return userId;
    } catch (error) {
      console.error('Error creating user:', error);
      throw error;
    }
  }

  // Get user by ID
  static async getUserById(id: number): Promise<User | undefined> {
    try {
      return await db.users.get(id);
    } catch (error) {
      console.error('Error getting user by ID:', error);
      throw error;
    }
  }

  // Get user by email
  static async getUserByEmail(email: string): Promise<User | undefined> {
    try {
      return await db.users
        .where('email')
        .equals(email)
        .first();
    } catch (error) {
      console.error('Error getting user by email:', error);
      throw error;
    }
  }

  // Get all users with pagination
  static async getUsers(options: {
    page?: number;
    limit?: number;
    role?: string;
    isActive?: boolean;
    search?: string;
  } = {}): Promise<{ users: User[]; total: number; page: number; limit: number }> {
    try {
      const { page = 1, limit = 10, role, isActive, search } = options;
      const offset = (page - 1) * limit;

      let query = db.users.orderBy('createdAt');

      // Apply filters
      if (role) {
        query = query.filter(user => user.role === role);
      }
      
      if (isActive !== undefined) {
        query = query.filter(user => user.isActive === isActive);
      }
      
      if (search) {
        query = query.filter(user => 
          user.username.toLowerCase().includes(search.toLowerCase()) ||
          user.email.toLowerCase().includes(search.toLowerCase()) ||
          (user.firstName && user.firstName.toLowerCase().includes(search.toLowerCase())) ||
          (user.lastName && user.lastName.toLowerCase().includes(search.toLowerCase()))
        );
      }

      const users = await query.offset(offset).limit(limit).toArray();
      const total = await query.count();

      return {
        users,
        total,
        page,
        limit
      };
    } catch (error) {
      console.error('Error getting users:', error);
      throw error;
    }
  }

  // Update user
  static async updateUser(id: number, updates: Partial<User>): Promise<void> {
    try {
      const existingUser = await db.users.get(id);
      if (!existingUser) {
        throw new Error('User not found');
      }

      // Validate updates
      const validatedUpdates = validateUser({ ...existingUser, ...updates });
      
      await db.users.update(id, validatedUpdates);
    } catch (error) {
      console.error('Error updating user:', error);
      throw error;
    }
  }

  // Update user preferences
  static async updateUserPreferences(id: number, preferences: Partial<UserPreferences>): Promise<void> {
    try {
      const user = await db.users.get(id);
      if (!user) {
        throw new Error('User not found');
      }

      const validatedPreferences = validateUserPreferences({
        ...user.preferences,
        ...preferences
      });

      await db.users.update(id, { preferences: validatedPreferences });
    } catch (error) {
      console.error('Error updating user preferences:', error);
      throw error;
    }
  }

  // Delete user
  static async deleteUser(id: number): Promise<void> {
    try {
      const user = await db.users.get(id);
      if (!user) {
        throw new Error('User not found');
      }

      // Delete related data
      await db.transaction('rw', [db.users, db.posts, db.comments], async () => {
        // Delete user's posts
        await db.posts.where('authorId').equals(id).delete();
        
        // Delete user's comments
        await db.comments.where('authorId').equals(id).delete();
        
        // Delete user
        await db.users.delete(id);
      });
    } catch (error) {
      console.error('Error deleting user:', error);
      throw error;
    }
  }

  // Search users
  static async searchUsers(query: string, limit: number = 10): Promise<User[]> {
    try {
      const searchTerm = query.toLowerCase();
      
      return await db.users
        .filter(user => 
          user.username.toLowerCase().includes(searchTerm) ||
          user.email.toLowerCase().includes(searchTerm) ||
          (user.firstName && user.firstName.toLowerCase().includes(searchTerm)) ||
          (user.lastName && user.lastName.toLowerCase().includes(searchTerm))
        )
        .limit(limit)
        .toArray();
    } catch (error) {
      console.error('Error searching users:', error);
      throw error;
    }
  }

  // Get user statistics
  static async getUserStats(id: number): Promise<{
    postCount: number;
    commentCount: number;
    totalViews: number;
    totalLikes: number;
  }> {
    try {
      const [postCount, commentCount, posts] = await Promise.all([
        db.posts.where('authorId').equals(id).count(),
        db.comments.where('authorId').equals(id).count(),
        db.posts.where('authorId').equals(id).toArray()
      ]);

      const totalViews = posts.reduce((sum, post) => sum + post.viewCount, 0);
      const totalLikes = posts.reduce((sum, post) => sum + post.likeCount, 0);

      return {
        postCount,
        commentCount,
        totalViews,
        totalLikes
      };
    } catch (error) {
      console.error('Error getting user stats:', error);
      throw error;
    }
  }
}
```

### Post Service
```typescript
// services/postService.ts
import { db } from '../db';
import { Post, User, Category } from '../db/types';
import { validatePost } from '../utils/validation';

export class PostService {
  // Create a new post
  static async createPost(postData: Omit<Post, 'id' | 'createdAt' | 'updatedAt'>): Promise<number> {
    try {
      const validatedData = validatePost(postData);
      
      // Check if slug already exists
      const existingPost = await db.posts
        .where('slug')
        .equals(validatedData.slug)
        .first();
      
      if (existingPost) {
        throw new Error('Post with this slug already exists');
      }

      const postId = await db.posts.add(validatedData);
      
      // Update category post count
      await db.categories.update(validatedData.categoryId, {
        postCount: await db.posts.where('categoryId').equals(validatedData.categoryId).count()
      });

      return postId;
    } catch (error) {
      console.error('Error creating post:', error);
      throw error;
    }
  }

  // Get post by ID
  static async getPostById(id: number): Promise<Post | undefined> {
    try {
      return await db.posts.get(id);
    } catch (error) {
      console.error('Error getting post by ID:', error);
      throw error;
    }
  }

  // Get post by slug
  static async getPostBySlug(slug: string): Promise<Post | undefined> {
    try {
      return await db.posts
        .where('slug')
        .equals(slug)
        .first();
    } catch (error) {
      console.error('Error getting post by slug:', error);
      throw error;
    }
  }

  // Get posts with pagination and filters
  static async getPosts(options: {
    page?: number;
    limit?: number;
    categoryId?: number;
    authorId?: number;
    published?: boolean;
    search?: string;
    sortBy?: 'createdAt' | 'updatedAt' | 'publishedAt' | 'viewCount' | 'likeCount';
    sortOrder?: 'asc' | 'desc';
  } = {}): Promise<{ posts: Post[]; total: number; page: number; limit: number }> {
    try {
      const {
        page = 1,
        limit = 10,
        categoryId,
        authorId,
        published,
        search,
        sortBy = 'createdAt',
        sortOrder = 'desc'
      } = options;

      const offset = (page - 1) * limit;

      let query = db.posts.orderBy(sortBy);

      // Apply filters
      if (categoryId) {
        query = query.filter(post => post.categoryId === categoryId);
      }
      
      if (authorId) {
        query = query.filter(post => post.authorId === authorId);
      }
      
      if (published !== undefined) {
        query = query.filter(post => post.published === published);
      }
      
      if (search) {
        query = query.filter(post => 
          post.title.toLowerCase().includes(search.toLowerCase()) ||
          post.content.toLowerCase().includes(search.toLowerCase()) ||
          post.tags.some(tag => tag.toLowerCase().includes(search.toLowerCase()))
        );
      }

      // Apply sorting
      if (sortOrder === 'desc') {
        query = query.reverse();
      }

      const posts = await query.offset(offset).limit(limit).toArray();
      const total = await query.count();

      return {
        posts,
        total,
        page,
        limit
      };
    } catch (error) {
      console.error('Error getting posts:', error);
      throw error;
    }
  }

  // Update post
  static async updatePost(id: number, updates: Partial<Post>): Promise<void> {
    try {
      const existingPost = await db.posts.get(id);
      if (!existingPost) {
        throw new Error('Post not found');
      }

      const validatedUpdates = validatePost({ ...existingPost, ...updates });
      
      await db.posts.update(id, validatedUpdates);
    } catch (error) {
      console.error('Error updating post:', error);
      throw error;
    }
  }

  // Increment view count
  static async incrementViewCount(id: number): Promise<void> {
    try {
      await db.posts.update(id, { viewCount: await db.posts.get(id).then(post => (post?.viewCount || 0) + 1) });
    } catch (error) {
      console.error('Error incrementing view count:', error);
      throw error;
    }
  }

  // Like/Unlike post
  static async toggleLike(id: number): Promise<{ liked: boolean; likeCount: number }> {
    try {
      const post = await db.posts.get(id);
      if (!post) {
        throw new Error('Post not found');
      }

      // This is a simplified implementation
      // In a real app, you'd track which users liked which posts
      const newLikeCount = post.likeCount + 1;
      await db.posts.update(id, { likeCount: newLikeCount });

      return {
        liked: true,
        likeCount: newLikeCount
      };
    } catch (error) {
      console.error('Error toggling like:', error);
      throw error;
    }
  }

  // Delete post
  static async deletePost(id: number): Promise<void> {
    try {
      const post = await db.posts.get(id);
      if (!post) {
        throw new Error('Post not found');
      }

      await db.transaction('rw', [db.posts, db.comments, db.categories], async () => {
        // Delete related comments
        await db.comments.where('postId').equals(id).delete();
        
        // Delete post
        await db.posts.delete(id);
        
        // Update category post count
        await db.categories.update(post.categoryId, {
          postCount: await db.posts.where('categoryId').equals(post.categoryId).count()
        });
      });
    } catch (error) {
      console.error('Error deleting post:', error);
      throw error;
    }
  }

  // Get related posts
  static async getRelatedPosts(id: number, limit: number = 5): Promise<Post[]> {
    try {
      const post = await db.posts.get(id);
      if (!post) {
        throw new Error('Post not found');
      }

      return await db.posts
        .where('categoryId')
        .equals(post.categoryId)
        .and(relatedPost => relatedPost.id !== id && relatedPost.published)
        .limit(limit)
        .toArray();
    } catch (error) {
      console.error('Error getting related posts:', error);
      throw error;
    }
  }

  // Get popular posts
  static async getPopularPosts(limit: number = 10): Promise<Post[]> {
    try {
      return await db.posts
        .where('published')
        .equals(true)
        .orderBy('viewCount')
        .reverse()
        .limit(limit)
        .toArray();
    } catch (error) {
      console.error('Error getting popular posts:', error);
      throw error;
    }
  }
}
```

## 🎣 React Hooks Integration

### User Hooks
```typescript
// hooks/useUsers.ts
import { useState, useEffect, useCallback } from 'react';
import { UserService } from '../services/userService';
import { User, UserPreferences } from '../db/types';

export const useUsers = (options: {
  page?: number;
  limit?: number;
  role?: string;
  isActive?: boolean;
  search?: string;
} = {}) => {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [total, setTotal] = useState(0);
  const [page, setPage] = useState(options.page || 1);
  const [limit, setLimit] = useState(options.limit || 10);

  const fetchUsers = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      
      const result = await UserService.getUsers({
        page,
        limit,
        role: options.role,
        isActive: options.isActive,
        search: options.search
      });
      
      setUsers(result.users);
      setTotal(result.total);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch users');
    } finally {
      setLoading(false);
    }
  }, [page, limit, options.role, options.isActive, options.search]);

  useEffect(() => {
    fetchUsers();
  }, [fetchUsers]);

  const createUser = useCallback(async (userData: Omit<User, 'id' | 'createdAt' | 'updatedAt'>) => {
    try {
      const userId = await UserService.createUser(userData);
      await fetchUsers(); // Refresh the list
      return userId;
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to create user');
      throw err;
    }
  }, [fetchUsers]);

  const updateUser = useCallback(async (id: number, updates: Partial<User>) => {
    try {
      await UserService.updateUser(id, updates);
      await fetchUsers(); // Refresh the list
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to update user');
      throw err;
    }
  }, [fetchUsers]);

  const deleteUser = useCallback(async (id: number) => {
    try {
      await UserService.deleteUser(id);
      await fetchUsers(); // Refresh the list
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to delete user');
      throw err;
    }
  }, [fetchUsers]);

  return {
    users,
    loading,
    error,
    total,
    page,
    limit,
    setPage,
    setLimit,
    createUser,
    updateUser,
    deleteUser,
    refetch: fetchUsers
  };
};

export const useUser = (id: number) => {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchUser = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      
      const userData = await UserService.getUserById(id);
      setUser(userData || null);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch user');
    } finally {
      setLoading(false);
    }
  }, [id]);

  useEffect(() => {
    if (id) {
      fetchUser();
    }
  }, [id, fetchUser]);

  const updateUser = useCallback(async (updates: Partial<User>) => {
    try {
      await UserService.updateUser(id, updates);
      await fetchUser(); // Refresh user data
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to update user');
      throw err;
    }
  }, [id, fetchUser]);

  const updatePreferences = useCallback(async (preferences: Partial<UserPreferences>) => {
    try {
      await UserService.updateUserPreferences(id, preferences);
      await fetchUser(); // Refresh user data
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to update preferences');
      throw err;
    }
  }, [id, fetchUser]);

  return {
    user,
    loading,
    error,
    updateUser,
    updatePreferences,
    refetch: fetchUser
  };
};
```

### Post Hooks
```typescript
// hooks/usePosts.ts
import { useState, useEffect, useCallback } from 'react';
import { PostService } from '../services/postService';
import { Post } from '../db/types';

export const usePosts = (options: {
  page?: number;
  limit?: number;
  categoryId?: number;
  authorId?: number;
  published?: boolean;
  search?: string;
  sortBy?: 'createdAt' | 'updatedAt' | 'publishedAt' | 'viewCount' | 'likeCount';
  sortOrder?: 'asc' | 'desc';
} = {}) => {
  const [posts, setPosts] = useState<Post[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [total, setTotal] = useState(0);
  const [page, setPage] = useState(options.page || 1);
  const [limit, setLimit] = useState(options.limit || 10);

  const fetchPosts = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      
      const result = await PostService.getPosts({
        page,
        limit,
        categoryId: options.categoryId,
        authorId: options.authorId,
        published: options.published,
        search: options.search,
        sortBy: options.sortBy,
        sortOrder: options.sortOrder
      });
      
      setPosts(result.posts);
      setTotal(result.total);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch posts');
    } finally {
      setLoading(false);
    }
  }, [page, limit, options]);

  useEffect(() => {
    fetchPosts();
  }, [fetchPosts]);

  const createPost = useCallback(async (postData: Omit<Post, 'id' | 'createdAt' | 'updatedAt'>) => {
    try {
      const postId = await PostService.createPost(postData);
      await fetchPosts(); // Refresh the list
      return postId;
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to create post');
      throw err;
    }
  }, [fetchPosts]);

  const updatePost = useCallback(async (id: number, updates: Partial<Post>) => {
    try {
      await PostService.updatePost(id, updates);
      await fetchPosts(); // Refresh the list
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to update post');
      throw err;
    }
  }, [fetchPosts]);

  const deletePost = useCallback(async (id: number) => {
    try {
      await PostService.deletePost(id);
      await fetchPosts(); // Refresh the list
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to delete post');
      throw err;
    }
  }, [fetchPosts]);

  return {
    posts,
    loading,
    error,
    total,
    page,
    limit,
    setPage,
    setLimit,
    createPost,
    updatePost,
    deletePost,
    refetch: fetchPosts
  };
};

export const usePost = (id: number) => {
  const [post, setPost] = useState<Post | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchPost = useCallback(async () => {
    try {
      setLoading(true);
      setError(null);
      
      const postData = await PostService.getPostById(id);
      setPost(postData || null);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to fetch post');
    } finally {
      setLoading(false);
    }
  }, [id]);

  useEffect(() => {
    if (id) {
      fetchPost();
    }
  }, [id, fetchPost]);

  const updatePost = useCallback(async (updates: Partial<Post>) => {
    try {
      await PostService.updatePost(id, updates);
      await fetchPost(); // Refresh post data
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to update post');
      throw err;
    }
  }, [id, fetchPost]);

  const incrementViewCount = useCallback(async () => {
    try {
      await PostService.incrementViewCount(id);
      await fetchPost(); // Refresh post data
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to increment view count');
      throw err;
    }
  }, [id, fetchPost]);

  const toggleLike = useCallback(async () => {
    try {
      const result = await PostService.toggleLike(id);
      await fetchPost(); // Refresh post data
      return result;
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Failed to toggle like');
      throw err;
    }
  }, [id, fetchPost]);

  return {
    post,
    loading,
    error,
    updatePost,
    incrementViewCount,
    toggleLike,
    refetch: fetchPost
  };
};
```

## 🔄 Database Migrations

### Migration System
```typescript
// db/migrations/001-initial.ts
import { Dexie } from 'dexie';

export const migration001 = {
  version: 1,
  description: 'Initial database schema',
  up: (db: Dexie) => {
    db.version(1).stores({
      users: '++id, email, username, createdAt, updatedAt',
      posts: '++id, title, slug, content, authorId, categoryId, published, createdAt, updatedAt',
      comments: '++id, content, authorId, postId, parentId, createdAt, updatedAt',
      categories: '++id, name, slug, description, createdAt, updatedAt'
    });
  },
  down: (db: Dexie) => {
    // Rollback logic if needed
  }
};

// db/migrations/002-add-users.ts
export const migration002 = {
  version: 2,
  description: 'Add user preferences and role fields',
  up: (db: Dexie) => {
    db.version(2).stores({
      users: '++id, email, username, role, isActive, preferences, createdAt, updatedAt',
      posts: '++id, title, slug, content, authorId, categoryId, published, createdAt, updatedAt',
      comments: '++id, content, authorId, postId, parentId, createdAt, updatedAt',
      categories: '++id, name, slug, description, createdAt, updatedAt'
    });
  },
  down: (db: Dexie) => {
    // Rollback logic
  }
};

// db/migrations/003-add-posts.ts
export const migration003 = {
  version: 3,
  description: 'Add post statistics and tags',
  up: (db: Dexie) => {
    db.version(3).stores({
      users: '++id, email, username, role, isActive, preferences, createdAt, updatedAt',
      posts: '++id, title, slug, content, authorId, categoryId, tags, published, viewCount, likeCount, commentCount, createdAt, updatedAt',
      comments: '++id, content, authorId, postId, parentId, isApproved, likeCount, createdAt, updatedAt',
      categories: '++id, name, slug, description, color, icon, postCount, createdAt, updatedAt'
    });
  },
  down: (db: Dexie) => {
    // Rollback logic
  }
};
```

## 🛠️ Utility Functions

### Database Utilities
```typescript
// utils/dbUtils.ts
import { db } from '../db';

export class DatabaseUtils {
  // Clear all data
  static async clearAllData(): Promise<void> {
    try {
      await db.transaction('rw', [db.users, db.posts, db.comments, db.categories], async () => {
        await db.users.clear();
        await db.posts.clear();
        await db.comments.clear();
        await db.categories.clear();
      });
    } catch (error) {
      console.error('Error clearing database:', error);
      throw error;
    }
  }

  // Export all data
  static async exportData(): Promise<{
    users: any[];
    posts: any[];
    comments: any[];
    categories: any[];
  }> {
    try {
      const [users, posts, comments, categories] = await Promise.all([
        db.users.toArray(),
        db.posts.toArray(),
        db.comments.toArray(),
        db.categories.toArray()
      ]);

      return {
        users,
        posts,
        comments,
        categories
      };
    } catch (error) {
      console.error('Error exporting data:', error);
      throw error;
    }
  }

  // Import data
  static async importData(data: {
    users?: any[];
    posts?: any[];
    comments?: any[];
    categories?: any[];
  }): Promise<void> {
    try {
      await db.transaction('rw', [db.users, db.posts, db.comments, db.categories], async () => {
        if (data.users) {
          await db.users.bulkAdd(data.users);
        }
        if (data.posts) {
          await db.posts.bulkAdd(data.posts);
        }
        if (data.comments) {
          await db.comments.bulkAdd(data.comments);
        }
        if (data.categories) {
          await db.categories.bulkAdd(data.categories);
        }
      });
    } catch (error) {
      console.error('Error importing data:', error);
      throw error;
    }
  }

  // Get database statistics
  static async getDatabaseStats(): Promise<{
    totalUsers: number;
    totalPosts: number;
    totalComments: number;
    totalCategories: number;
    databaseSize: number;
  }> {
    try {
      const [totalUsers, totalPosts, totalComments, totalCategories] = await Promise.all([
        db.users.count(),
        db.posts.count(),
        db.comments.count(),
        db.categories.count()
      ]);

      // Estimate database size (this is approximate)
      const databaseSize = await db.estimate();

      return {
        totalUsers,
        totalPosts,
        totalComments,
        totalCategories,
        databaseSize
      };
    } catch (error) {
      console.error('Error getting database stats:', error);
      throw error;
    }
  }

  // Optimize database
  static async optimizeDatabase(): Promise<void> {
    try {
      // This would typically involve:
      // 1. Rebuilding indexes
      // 2. Compacting the database
      // 3. Removing unused data
      
      // For now, we'll just trigger a simple optimization
      await db.transaction('rw', [db.users, db.posts, db.comments, db.categories], async () => {
        // Force index rebuild by querying all tables
        await Promise.all([
          db.users.toArray(),
          db.posts.toArray(),
          db.comments.toArray(),
          db.categories.toArray()
        ]);
      });
    } catch (error) {
      console.error('Error optimizing database:', error);
      throw error;
    }
  }
}
```

### Validation Utilities
```typescript
// utils/validation.ts
import { User, Post, Comment, Category, UserPreferences } from '../db/types';

export function validateUser(user: Partial<User>): User {
  if (!user.email || !isValidEmail(user.email)) {
    throw new Error('Valid email is required');
  }
  
  if (!user.username || user.username.length < 3) {
    throw new Error('Username must be at least 3 characters long');
  }
  
  if (!user.role || !['admin', 'user', 'moderator'].includes(user.role)) {
    throw new Error('Valid role is required');
  }

  return {
    id: user.id,
    email: user.email,
    username: user.username,
    firstName: user.firstName,
    lastName: user.lastName,
    avatar: user.avatar,
    role: user.role,
    isActive: user.isActive ?? true,
    preferences: user.preferences || getDefaultUserPreferences(),
    createdAt: user.createdAt,
    updatedAt: user.updatedAt
  };
}

export function validatePost(post: Partial<Post>): Post {
  if (!post.title || post.title.length < 1) {
    throw new Error('Title is required');
  }
  
  if (!post.content || post.content.length < 1) {
    throw new Error('Content is required');
  }
  
  if (!post.authorId || post.authorId < 1) {
    throw new Error('Valid author ID is required');
  }
  
  if (!post.categoryId || post.categoryId < 1) {
    throw new Error('Valid category ID is required');
  }

  return {
    id: post.id,
    title: post.title,
    slug: post.slug || generateSlug(post.title),
    content: post.content,
    excerpt: post.excerpt,
    authorId: post.authorId,
    categoryId: post.categoryId,
    tags: post.tags || [],
    featuredImage: post.featuredImage,
    published: post.published ?? false,
    publishedAt: post.publishedAt,
    viewCount: post.viewCount || 0,
    likeCount: post.likeCount || 0,
    commentCount: post.commentCount || 0,
    createdAt: post.createdAt,
    updatedAt: post.updatedAt
  };
}

export function validateUserPreferences(preferences: Partial<UserPreferences>): UserPreferences {
  return {
    theme: preferences.theme || 'light',
    language: preferences.language || 'en',
    notifications: {
      email: preferences.notifications?.email ?? true,
      push: preferences.notifications?.push ?? true,
      sms: preferences.notifications?.sms ?? false
    },
    privacy: {
      profileVisibility: preferences.privacy?.profileVisibility || 'public',
      showEmail: preferences.privacy?.showEmail ?? false,
      showLastSeen: preferences.privacy?.showLastSeen ?? true
    }
  };
}

function isValidEmail(email: string): boolean {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
}

function generateSlug(title: string): string {
  return title
    .toLowerCase()
    .replace(/[^a-z0-9 -]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-')
    .trim();
}

function getDefaultUserPreferences(): UserPreferences {
  return {
    theme: 'light',
    language: 'en',
    notifications: {
      email: true,
      push: true,
      sms: false
    },
    privacy: {
      profileVisibility: 'public',
      showEmail: false,
      showLastSeen: true
    }
  };
}
```

## 🎯 Dexie Best Practices

### Performance Optimization
1. **Use proper indexes** for frequently queried fields
2. **Implement pagination** for large datasets
3. **Use transactions** for related operations
4. **Optimize queries** by using appropriate methods
5. **Implement caching** for frequently accessed data
6. **Use bulk operations** when possible
7. **Monitor database size** and implement cleanup strategies

### Data Management
1. **Implement proper validation** for all data
2. **Use TypeScript** for type safety
3. **Handle errors gracefully** with proper error messages
4. **Implement data migration** strategies
5. **Use hooks for data transformation** when needed
6. **Implement proper cleanup** for deleted data
7. **Use transactions** for data consistency

### Security Considerations
1. **Validate all inputs** before storing
2. **Implement proper access control** in services
3. **Use secure data types** for sensitive information
4. **Implement data encryption** for sensitive data
5. **Use proper error handling** to avoid data leaks
6. **Implement audit logging** for important operations
7. **Use proper data sanitization** techniques

## 🎯 AI Agent Best Practices

When assisting with Dexie development:

1. **Use proper TypeScript types** for all database operations
2. **Implement comprehensive error handling** for all database operations
3. **Use transactions** for related operations to maintain data consistency
4. **Implement proper validation** for all data before storing
5. **Use appropriate indexes** for better query performance
6. **Implement pagination** for large datasets
7. **Use React hooks** for data management in UI components
8. **Follow the established service patterns** for database operations
9. **Implement proper cleanup** for deleted data and related records
10. **Use bulk operations** when possible for better performance

## 🔗 Additional Resources

- [Dexie Documentation](https://dexie.org/)
- [Dexie GitHub](https://github.com/dexie/Dexie.js)
- [Dexie Examples](https://dexie.org/docs/Tutorial/Getting-started)
- [IndexedDB Guide](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
- [Dexie Best Practices](https://dexie.org/docs/Best-Practices)

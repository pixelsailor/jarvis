# Angular Development Guide

## 🎯 AI Agent Instructions for Angular Development

When working with Angular projects, follow these guidelines to ensure optimal AI assistance and code quality.

## 📁 Project Structure

```
src/
├── app/
│   ├── core/             # Singleton services, guards, interceptors
│   ├── shared/           # Shared components, pipes, directives
│   ├── features/         # Feature modules
│   │   └── user/
│   │       ├── components/
│   │       ├── services/
│   │       ├── models/
│   │       └── user-routing.module.ts
│   ├── layout/           # Layout components
│   └── app-routing.module.ts
├── assets/               # Static assets
├── environments/         # Environment configurations
└── styles/               # Global styles
```

## 🏗️ Component Development

### Component Naming
- Use PascalCase for component files: `UserProfileComponent`
- Use kebab-case for selectors: `app-user-profile`
- Follow Angular naming conventions: `ComponentNameComponent`

### Component Template
```typescript
// user-profile.component.ts
import { Component, OnInit, OnDestroy, Input, Output, EventEmitter } from '@angular/core';
import { Subject, takeUntil } from 'rxjs';
import { User } from '../models/user.model';
import { UserService } from '../services/user.service';

@Component({
  selector: 'app-user-profile',
  templateUrl: './user-profile.component.html',
  styleUrls: ['./user-profile.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserProfileComponent implements OnInit, OnDestroy {
  @Input() userId!: string;
  @Input() showActions: boolean = true;
  @Output() userUpdated = new EventEmitter<User>();
  @Output() userDeleted = new EventEmitter<string>();

  user: User | null = null;
  loading = false;
  error: string | null = null;

  private destroy$ = new Subject<void>();

  constructor(
    private userService: UserService,
    private cdr: ChangeDetectorRef
  ) {}

  ngOnInit(): void {
    this.loadUser();
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }

  private loadUser(): void {
    this.loading = true;
    this.error = null;

    this.userService.getUser(this.userId)
      .pipe(takeUntil(this.destroy$))
      .subscribe({
        next: (user) => {
          this.user = user;
          this.loading = false;
          this.cdr.markForCheck();
        },
        error: (error) => {
          this.error = error.message || 'Failed to load user';
          this.loading = false;
          this.cdr.markForCheck();
        }
      });
  }

  onUpdateUser(updates: Partial<User>): void {
    if (!this.user) return;

    this.userService.updateUser(this.user.id, updates)
      .pipe(takeUntil(this.destroy$))
      .subscribe({
        next: (updatedUser) => {
          this.user = updatedUser;
          this.userUpdated.emit(updatedUser);
          this.cdr.markForCheck();
        },
        error: (error) => {
          this.error = error.message || 'Failed to update user';
          this.cdr.markForCheck();
        }
      });
  }

  onDeleteUser(): void {
    if (!this.user) return;

    this.userService.deleteUser(this.user.id)
      .pipe(takeUntil(this.destroy$))
      .subscribe({
        next: () => {
          this.userDeleted.emit(this.user!.id);
        },
        error: (error) => {
          this.error = error.message || 'Failed to delete user';
          this.cdr.markForCheck();
        }
      });
  }
}
```

### Component Template
```html
<!-- user-profile.component.html -->
<div class="user-profile" *ngIf="user; else loadingTemplate">
  <div class="user-header">
    <h2>{{ user.name }}</h2>
    <p class="user-email">{{ user.email }}</p>
  </div>

  <div class="user-details">
    <div class="detail-item">
      <label>Role:</label>
      <span>{{ user.role }}</span>
    </div>
    <div class="detail-item">
      <label>Created:</label>
      <span>{{ user.createdAt | date:'medium' }}</span>
    </div>
  </div>

  <div class="user-actions" *ngIf="showActions">
    <button 
      class="btn btn-primary" 
      (click)="onUpdateUser({ role: 'admin' })"
      [disabled]="loading">
      Make Admin
    </button>
    <button 
      class="btn btn-danger" 
      (click)="onDeleteUser()"
      [disabled]="loading">
      Delete User
    </button>
  </div>

  <div class="error-message" *ngIf="error">
    {{ error }}
  </div>
</div>

<ng-template #loadingTemplate>
  <div class="loading-spinner">
    <div class="spinner"></div>
    <p>Loading user...</p>
  </div>
</ng-template>
```

## 🏪 Service Development

### Service Pattern
```typescript
// user.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';
import { Observable, BehaviorSubject, map, tap } from 'rxjs';
import { User } from '../models/user.model';
import { ApiResponse } from '../models/api-response.model';

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private readonly apiUrl = '/api/users';
  private usersSubject = new BehaviorSubject<User[]>([]);
  public users$ = this.usersSubject.asObservable();

  constructor(private http: HttpClient) {}

  getUsers(params?: { page?: number; limit?: number; search?: string }): Observable<User[]> {
    let httpParams = new HttpParams();
    
    if (params?.page) httpParams = httpParams.set('page', params.page.toString());
    if (params?.limit) httpParams = httpParams.set('limit', params.limit.toString());
    if (params?.search) httpParams = httpParams.set('search', params.search);

    return this.http.get<ApiResponse<User[]>>(this.apiUrl, { params: httpParams })
      .pipe(
        map(response => response.data),
        tap(users => this.usersSubject.next(users))
      );
  }

  getUser(id: string): Observable<User> {
    return this.http.get<ApiResponse<User>>(`${this.apiUrl}/${id}`)
      .pipe(map(response => response.data));
  }

  createUser(user: Omit<User, 'id'>): Observable<User> {
    return this.http.post<ApiResponse<User>>(this.apiUrl, user)
      .pipe(
        map(response => response.data),
        tap(newUser => {
          const currentUsers = this.usersSubject.value;
          this.usersSubject.next([...currentUsers, newUser]);
        })
      );
  }

  updateUser(id: string, updates: Partial<User>): Observable<User> {
    return this.http.put<ApiResponse<User>>(`${this.apiUrl}/${id}`, updates)
      .pipe(
        map(response => response.data),
        tap(updatedUser => {
          const currentUsers = this.usersSubject.value;
          const updatedUsers = currentUsers.map(user => 
            user.id === id ? updatedUser : user
          );
          this.usersSubject.next(updatedUsers);
        })
      );
  }

  deleteUser(id: string): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`)
      .pipe(
        tap(() => {
          const currentUsers = this.usersSubject.value;
          const filteredUsers = currentUsers.filter(user => user.id !== id);
          this.usersSubject.next(filteredUsers);
        })
      );
  }
}
```

## 🛣️ Routing & Navigation

### Routing Module
```typescript
// user-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { UserListComponent } from './components/user-list/user-list.component';
import { UserDetailComponent } from './components/user-detail/user-detail.component';
import { UserEditComponent } from './components/user-edit/user-edit.component';
import { AuthGuard } from '../core/guards/auth.guard';
import { UserResolver } from './resolvers/user.resolver';

const routes: Routes = [
  {
    path: '',
    component: UserListComponent,
    canActivate: [AuthGuard]
  },
  {
    path: ':id',
    component: UserDetailComponent,
    canActivate: [AuthGuard],
    resolve: { user: UserResolver }
  },
  {
    path: ':id/edit',
    component: UserEditComponent,
    canActivate: [AuthGuard],
    resolve: { user: UserResolver }
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class UserRoutingModule { }
```

### Route Resolver
```typescript
// user.resolver.ts
import { Injectable } from '@angular/core';
import { Resolve, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs';
import { User } from '../models/user.model';
import { UserService } from '../services/user.service';

@Injectable({
  providedIn: 'root'
})
export class UserResolver implements Resolve<User> {
  constructor(private userService: UserService) {}

  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<User> {
    const userId = route.paramMap.get('id');
    if (!userId) {
      throw new Error('User ID is required');
    }
    return this.userService.getUser(userId);
  }
}
```

## 🎨 Styling Guidelines

### SCSS Organization
```scss
// user-profile.component.scss
.user-profile {
  padding: 1.5rem;
  border: 1px solid var(--border-color);
  border-radius: 0.5rem;
  background: var(--background-color);

  &__header {
    margin-bottom: 1rem;
    
    h2 {
      font-size: 1.5rem;
      font-weight: 600;
      margin: 0 0 0.5rem 0;
      color: var(--text-primary);
    }
  }

  &__email {
    color: var(--text-secondary);
    font-size: 0.875rem;
  }

  &__details {
    display: grid;
    gap: 0.75rem;
    margin-bottom: 1.5rem;
  }

  &__actions {
    display: flex;
    gap: 0.75rem;
  }
}

.detail-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  
  label {
    font-weight: 500;
    color: var(--text-secondary);
  }
}

.loading-spinner {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 200px;
  
  .spinner {
    width: 2rem;
    height: 2rem;
    border: 2px solid var(--border-color);
    border-top: 2px solid var(--primary-color);
    border-radius: 50%;
    animation: spin 1s linear infinite;
  }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error-message {
  color: var(--error-color);
  background: var(--error-background);
  padding: 0.75rem;
  border-radius: 0.25rem;
  margin-top: 1rem;
}
```

## ⚡ Performance Optimization

### OnPush Change Detection
```typescript
import { ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-user-list',
  templateUrl: './user-list.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserListComponent {
  users: User[] = [];

  constructor(private cdr: ChangeDetectorRef) {}

  loadUsers(): void {
    this.userService.getUsers().subscribe(users => {
      this.users = users;
      this.cdr.markForCheck(); // Trigger change detection
    });
  }
}
```

### TrackBy Functions
```typescript
// user-list.component.ts
export class UserListComponent {
  trackByUserId(index: number, user: User): string {
    return user.id;
  }
}
```

```html
<!-- user-list.component.html -->
<div *ngFor="let user of users; trackBy: trackByUserId" class="user-item">
  {{ user.name }}
</div>
```

### Lazy Loading
```typescript
// app-routing.module.ts
const routes: Routes = [
  {
    path: 'users',
    loadChildren: () => import('./features/user/user.module').then(m => m.UserModule)
  },
  {
    path: 'admin',
    loadChildren: () => import('./features/admin/admin.module').then(m => m.AdminModule),
    canLoad: [AdminGuard]
  }
];
```

## 🧪 Testing Patterns

### Component Testing
```typescript
// user-profile.component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { UserProfileComponent } from './user-profile.component';
import { UserService } from '../services/user.service';
import { of, throwError } from 'rxjs';

describe('UserProfileComponent', () => {
  let component: UserProfileComponent;
  let fixture: ComponentFixture<UserProfileComponent>;
  let userService: jasmine.SpyObj<UserService>;

  const mockUser: User = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com',
    role: 'user'
  };

  beforeEach(async () => {
    const userServiceSpy = jasmine.createSpyObj('UserService', ['getUser', 'updateUser']);

    await TestBed.configureTestingModule({
      declarations: [UserProfileComponent],
      providers: [
        { provide: UserService, useValue: userServiceSpy }
      ]
    }).compileComponents();

    fixture = TestBed.createComponent(UserProfileComponent);
    component = fixture.componentInstance;
    userService = TestBed.inject(UserService) as jasmine.SpyObj<UserService>;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should load user on init', () => {
    userService.getUser.and.returnValue(of(mockUser));
    component.userId = '1';
    
    component.ngOnInit();
    
    expect(userService.getUser).toHaveBeenCalledWith('1');
    expect(component.user).toEqual(mockUser);
  });

  it('should handle error when loading user', () => {
    const error = new Error('Failed to load user');
    userService.getUser.and.returnValue(throwError(() => error));
    component.userId = '1';
    
    component.ngOnInit();
    
    expect(component.error).toBe('Failed to load user');
  });
});
```

### Service Testing
```typescript
// user.service.spec.ts
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { UserService } from './user.service';

describe('UserService', () => {
  let service: UserService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [UserService]
    });
    service = TestBed.inject(UserService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  afterEach(() => {
    httpMock.verify();
  });

  it('should fetch users', () => {
    const mockUsers: User[] = [
      { id: '1', name: 'John Doe', email: 'john@example.com' }
    ];

    service.getUsers().subscribe(users => {
      expect(users).toEqual(mockUsers);
    });

    const req = httpMock.expectOne('/api/users');
    expect(req.request.method).toBe('GET');
    req.flush({ data: mockUsers });
  });
});
```

## 🔧 Common Patterns

### Interceptor Pattern
```typescript
// auth.interceptor.ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';
import { AuthService } from '../services/auth.service';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const token = this.authService.getToken();
    
    if (token) {
      const authReq = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${token}`)
      });
      return next.handle(authReq);
    }
    
    return next.handle(req);
  }
}
```

### Guard Pattern
```typescript
// auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, Router, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs';
import { AuthService } from '../services/auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    } else {
      this.router.navigate(['/login'], { 
        queryParams: { returnUrl: state.url } 
      });
      return false;
    }
  }
}
```

## 🚨 Common Pitfalls

1. **Memory leaks**: Always unsubscribe from observables using takeUntil pattern
2. **Change detection issues**: Use OnPush strategy and markForCheck() appropriately
3. **Circular dependencies**: Avoid importing services in each other
4. **Large bundle sizes**: Use lazy loading and tree shaking
5. **Performance issues**: Use trackBy functions for *ngFor loops

## 📚 Recommended Libraries

- **State Management**: NgRx, Akita
- **UI Components**: Angular Material, PrimeNG, Ng-Bootstrap
- **Forms**: Reactive Forms (built-in)
- **Testing**: Jasmine, Karma, Protractor
- **Styling**: SCSS, Angular Material, TailwindCSS
- **Charts**: Chart.js, D3.js

## 🎯 AI Agent Best Practices

When assisting with Angular development:

1. **Always use TypeScript** for better type safety
2. **Follow Angular style guide** and naming conventions
3. **Use reactive programming** with RxJS observables
4. **Implement proper error handling** with catchError operators
5. **Use OnPush change detection** for better performance
6. **Write comprehensive tests** for components and services
7. **Follow dependency injection** patterns
8. **Use guards and interceptors** for security and cross-cutting concerns
9. **Implement proper routing** with lazy loading
10. **Follow accessibility guidelines** with Angular CDK

## 🔗 Additional Resources

- [Angular Documentation](https://angular.io/docs)
- [Angular Style Guide](https://angular.io/guide/styleguide)
- [RxJS Documentation](https://rxjs.dev/)
- [Angular Material](https://material.angular.io/)
- [NgRx Documentation](https://ngrx.io/)

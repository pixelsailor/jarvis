# Svelte Development Guide

## 🎯 AI Agent Instructions for Svelte Development

When working with Svelte projects, follow these guidelines to ensure optimal AI assistance and code quality.

## 📁 Project Structure

```
src/
├── lib/
│   ├── components/
│   │   ├── ui/           # Reusable UI components
│   │   ├── forms/        # Form-specific components
│   │   └── layout/       # Layout components
│   ├── stores/           # Svelte stores
│   ├── utils/            # Utility functions
│   ├── types/            # TypeScript type definitions
│   └── constants/        # Application constants
├── routes/               # SvelteKit routes
├── app.html              # Main HTML template
└── app.css               # Global styles
```

## 🧩 Component Development

### Component Naming
- Use PascalCase for component files: `UserProfile.svelte`
- Use kebab-case for component usage: `<user-profile>`
- Prefix with `Ui` for reusable components: `UiButton.svelte`

### Component Structure Template
```svelte
<script lang="ts">
  import type { ComponentType } from './types';
  
  // Props with default values
  export let title: string = '';
  export let variant: 'primary' | 'secondary' = 'primary';
  export let disabled: boolean = false;
  
  // Reactive statements
  $: computedValue = title.toUpperCase();
  
  // Event handlers
  function handleClick(event: MouseEvent) {
    // Handle click logic
  }
</script>

<!-- Component markup -->
<div class="component-wrapper" class:disabled>
  <h2>{title}</h2>
  <button 
    on:click={handleClick}
    disabled={disabled}
    class="btn btn-{variant}"
  >
    {computedValue}
  </button>
</div>

<style>
  .component-wrapper {
    /* Component styles */
  }
  
  .btn {
    /* Button base styles */
  }
  
  .btn-primary {
    /* Primary variant */
  }
  
  .disabled {
    /* Disabled state */
  }
</style>
```

## 🏪 Store Management

### Store Patterns
```typescript
// stores/user.ts
import { writable, derived } from 'svelte/store';

export const user = writable<User | null>(null);
export const isLoggedIn = derived(user, $user => $user !== null);

// Custom store with methods
function createUserStore() {
  const { subscribe, set, update } = writable<User | null>(null);
  
  return {
    subscribe,
    login: (userData: User) => set(userData),
    logout: () => set(null),
    updateProfile: (updates: Partial<User>) => 
      update(user => user ? { ...user, ...updates } : null)
  };
}

export const userStore = createUserStore();
```

## 🎨 Styling Guidelines

### CSS Organization
- Use CSS custom properties for theming
- Follow BEM methodology for complex components
- Use `:global()` sparingly and document why

```svelte
<style>
  :root {
    --primary-color: #3b82f6;
    --secondary-color: #64748b;
  }
  
  .user-card {
    /* BEM block */
  }
  
  .user-card__header {
    /* BEM element */
  }
  
  .user-card--featured {
    /* BEM modifier */
  }
  
  :global(.external-library-class) {
    /* Only when necessary */
  }
</style>
```

## 🚀 SvelteKit Integration

### Route Structure
```
routes/
├── +layout.svelte        # Root layout
├── +layout.server.ts     # Server-side layout logic
├── +page.svelte          # Home page
├── +page.server.ts       # Server-side page logic
├── about/
│   └── +page.svelte      # About page
└── api/
    └── users/
        └── +server.ts    # API endpoint
```

### Server-Side Rendering
```typescript
// +page.server.ts
import type { PageServerLoad } from './$types';

export const load: PageServerLoad = async ({ params, fetch }) => {
  const response = await fetch('/api/users');
  const users = await response.json();
  
  return {
    users
  };
};
```

## ⚡ Performance Optimization

### Key Principles
1. **Minimize reactivity**: Only make data reactive when necessary
2. **Use `keyed each` blocks**: For dynamic lists that change frequently
3. **Lazy load components**: Use dynamic imports for large components
4. **Optimize stores**: Use derived stores instead of computed values in templates

```svelte
<!-- Good: Keyed each block -->
{#each items as item (item.id)}
  <ItemComponent {item} />
{/each}

<!-- Good: Lazy loading -->
<script>
  import { onMount } from 'svelte';
  
  let HeavyComponent;
  
  onMount(async () => {
    const module = await import('./HeavyComponent.svelte');
    HeavyComponent = module.default;
  });
</script>

{#if HeavyComponent}
  <svelte:component this={HeavyComponent} />
{/if}
```

## 🧪 Testing Patterns

### Component Testing
```typescript
// UserProfile.test.ts
import { render, fireEvent } from '@testing-library/svelte';
import UserProfile from './UserProfile.svelte';

test('renders user name', () => {
  const { getByText } = render(UserProfile, {
    props: { name: 'John Doe' }
  });
  
  expect(getByText('John Doe')).toBeInTheDocument();
});

test('handles click events', async () => {
  const { getByRole } = render(UserProfile);
  const button = getByRole('button');
  
  await fireEvent.click(button);
  // Assert expected behavior
});
```

## 🔧 Common Patterns

### Form Handling
```svelte
<script>
  import { createEventDispatcher } from 'svelte';
  
  const dispatch = createEventDispatcher();
  
  let formData = {
    name: '',
    email: ''
  };
  
  function handleSubmit() {
    dispatch('submit', formData);
  }
</script>

<form on:submit|preventDefault={handleSubmit}>
  <input bind:value={formData.name} placeholder="Name" />
  <input bind:value={formData.email} type="email" placeholder="Email" />
  <button type="submit">Submit</button>
</form>
```

### Modal Pattern
```svelte
<!-- Modal.svelte -->
<script>
  import { createEventDispatcher } from 'svelte';
  
  export let isOpen = false;
  const dispatch = createEventDispatcher();
  
  function close() {
    isOpen = false;
    dispatch('close');
  }
</script>

{#if isOpen}
  <div class="modal-backdrop" on:click={close}>
    <div class="modal-content" on:click|stopPropagation>
      <slot />
      <button on:click={close}>Close</button>
    </div>
  </div>
{/if}
```

## 🚨 Common Pitfalls

1. **Avoid unnecessary reactivity**: Don't make everything reactive
2. **Don't mutate props directly**: Use events or stores for communication
3. **Be careful with `bind:`**: Can cause performance issues with large objects
4. **Don't forget cleanup**: Use `onDestroy` for subscriptions and timers

## 📚 Recommended Libraries

- **UI Components**: Svelte Material UI, Carbon Components Svelte
- **State Management**: Built-in stores, Zustand
- **Routing**: SvelteKit (built-in)
- **Testing**: Vitest, Testing Library
- **Styling**: TailwindCSS, UnoCSS

## 🎯 AI Agent Best Practices

When assisting with Svelte development:

1. **Always use TypeScript** for better type safety
2. **Follow the component structure template** above
3. **Prefer SvelteKit** over vanilla Svelte for new projects
4. **Use stores for global state**, props for component communication
5. **Optimize for performance** by minimizing reactivity
6. **Test components thoroughly** with proper test patterns
7. **Follow accessibility guidelines** (ARIA attributes, semantic HTML)

## 🔗 Additional Resources

- [Svelte Documentation](https://svelte.dev/docs)
- [SvelteKit Documentation](https://kit.svelte.dev/docs)
- [Svelte Society](https://sveltesociety.dev/)
- [Svelte Examples](https://svelte.dev/examples)

# TailwindCSS Development Guide

## 🎯 AI Agent Instructions for TailwindCSS

When working with TailwindCSS projects, follow these guidelines to ensure optimal AI assistance and consistent styling.

## 📁 Project Structure

```
src/
├── styles/
│   ├── globals.css          # Global styles and Tailwind imports
│   ├── components.css       # Component-specific styles
│   └── utilities.css        # Custom utility classes
├── components/
│   ├── ui/                  # Reusable UI components
│   └── layout/              # Layout components
└── pages/                   # Page components
```

## 🎨 TailwindCSS Configuration

### tailwind.config.js Template
```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
        },
        secondary: {
          50: '#f8fafc',
          100: '#f1f5f9',
          200: '#e2e8f0',
          300: '#cbd5e1',
          400: '#94a3b8',
          500: '#64748b',
          600: '#475569',
          700: '#334155',
          800: '#1e293b',
          900: '#0f172a',
        }
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace'],
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem',
        '128': '32rem',
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.3s ease-out',
        'bounce-slow': 'bounce 2s infinite',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        slideUp: {
          '0%': { transform: 'translateY(10px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
      },
      screens: {
        'xs': '475px',
        '3xl': '1600px',
      }
    },
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio'),
  ],
}
```

### Global Styles Template
```css
/* src/styles/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    @apply scroll-smooth;
  }
  
  body {
    @apply bg-white text-gray-900 antialiased;
  }
  
  h1, h2, h3, h4, h5, h6 {
    @apply font-semibold text-gray-900;
  }
  
  h1 {
    @apply text-4xl md:text-5xl lg:text-6xl;
  }
  
  h2 {
    @apply text-3xl md:text-4xl lg:text-5xl;
  }
  
  h3 {
    @apply text-2xl md:text-3xl lg:text-4xl;
  }
  
  h4 {
    @apply text-xl md:text-2xl lg:text-3xl;
  }
  
  h5 {
    @apply text-lg md:text-xl lg:text-2xl;
  }
  
  h6 {
    @apply text-base md:text-lg lg:text-xl;
  }
  
  p {
    @apply text-gray-600 leading-relaxed;
  }
  
  a {
    @apply text-primary-600 hover:text-primary-700 transition-colors duration-200;
  }
  
  button {
    @apply focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2;
  }
  
  input, textarea, select {
    @apply focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-primary-500;
  }
}

@layer components {
  .btn {
    @apply inline-flex items-center justify-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2;
  }
  
  .btn-primary {
    @apply btn bg-primary-600 text-white hover:bg-primary-700 focus:ring-primary-500;
  }
  
  .btn-secondary {
    @apply btn bg-secondary-600 text-white hover:bg-secondary-700 focus:ring-secondary-500;
  }
  
  .btn-outline {
    @apply btn border-gray-300 text-gray-700 bg-white hover:bg-gray-50 focus:ring-primary-500;
  }
  
  .btn-ghost {
    @apply btn text-gray-700 hover:bg-gray-100 focus:ring-gray-500;
  }
  
  .btn-sm {
    @apply px-3 py-1.5 text-xs;
  }
  
  .btn-lg {
    @apply px-6 py-3 text-base;
  }
  
  .card {
    @apply bg-white rounded-lg shadow-sm border border-gray-200 overflow-hidden;
  }
  
  .card-header {
    @apply px-6 py-4 border-b border-gray-200;
  }
  
  .card-body {
    @apply px-6 py-4;
  }
  
  .card-footer {
    @apply px-6 py-4 border-t border-gray-200 bg-gray-50;
  }
  
  .form-input {
    @apply block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm placeholder-gray-400 focus:outline-none focus:ring-primary-500 focus:border-primary-500 sm:text-sm;
  }
  
  .form-label {
    @apply block text-sm font-medium text-gray-700 mb-1;
  }
  
  .form-error {
    @apply mt-1 text-sm text-red-600;
  }
  
  .form-help {
    @apply mt-1 text-sm text-gray-500;
  }
  
  .container-custom {
    @apply max-w-7xl mx-auto px-4 sm:px-6 lg:px-8;
  }
  
  .section-padding {
    @apply py-12 md:py-16 lg:py-20;
  }
  
  .text-gradient {
    @apply bg-gradient-to-r from-primary-600 to-secondary-600 bg-clip-text text-transparent;
  }
  
  .glass {
    @apply bg-white/80 backdrop-blur-sm border border-white/20;
  }
  
  .shadow-glow {
    @apply shadow-lg shadow-primary-500/25;
  }
}

@layer utilities {
  .text-balance {
    text-wrap: balance;
  }
  
  .scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
  }
  
  .scrollbar-hide::-webkit-scrollbar {
    display: none;
  }
  
  .animate-fade-in-up {
    animation: fadeInUp 0.6s ease-out;
  }
  
  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
}
```

## 🧩 Component Patterns

### Button Component Template
```tsx
// components/ui/Button.tsx
import React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none ring-offset-background',
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

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
}

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

export { Button, buttonVariants };
```

### Card Component Template
```tsx
// components/ui/Card.tsx
import React from 'react';
import { cn } from '@/lib/utils';

const Card = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      'rounded-lg border bg-card text-card-foreground shadow-sm',
      className
    )}
    {...props}
  />
));
Card.displayName = 'Card';

const CardHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('flex flex-col space-y-1.5 p-6', className)}
    {...props}
  />
));
CardHeader.displayName = 'CardHeader';

const CardTitle = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLHeadingElement>
>(({ className, ...props }, ref) => (
  <h3
    ref={ref}
    className={cn(
      'text-2xl font-semibold leading-none tracking-tight',
      className
    )}
    {...props}
  />
));
CardTitle.displayName = 'CardTitle';

const CardDescription = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLParagraphElement>
>(({ className, ...props }, ref) => (
  <p
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
));
CardDescription.displayName = 'CardDescription';

const CardContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div ref={ref} className={cn('p-6 pt-0', className)} {...props} />
));
CardContent.displayName = 'CardContent';

const CardFooter = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('flex items-center p-6 pt-0', className)}
    {...props}
  />
));
CardFooter.displayName = 'CardFooter';

export { Card, CardHeader, CardFooter, CardTitle, CardDescription, CardContent };
```

### Form Component Template
```tsx
// components/ui/Form.tsx
import React from 'react';
import { cn } from '@/lib/utils';

interface FormFieldProps {
  label: string;
  error?: string;
  help?: string;
  required?: boolean;
  children: React.ReactNode;
  className?: string;
}

export const FormField: React.FC<FormFieldProps> = ({
  label,
  error,
  help,
  required,
  children,
  className
}) => (
  <div className={cn('space-y-1', className)}>
    <label className="block text-sm font-medium text-gray-700">
      {label}
      {required && <span className="text-red-500 ml-1">*</span>}
    </label>
    {children}
    {error && (
      <p className="text-sm text-red-600" role="alert">
        {error}
      </p>
    )}
    {help && !error && (
      <p className="text-sm text-gray-500">{help}</p>
    )}
  </div>
);

interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  error?: boolean;
}

export const Input = React.forwardRef<HTMLInputElement, InputProps>(
  ({ className, error, type, ...props }, ref) => (
    <input
      type={type}
      className={cn(
        'flex h-10 w-full rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50',
        error && 'border-red-500 focus-visible:ring-red-500',
        className
      )}
      ref={ref}
      {...props}
    />
  )
);
Input.displayName = 'Input';

interface TextareaProps extends React.TextareaHTMLAttributes<HTMLTextAreaElement> {
  error?: boolean;
}

export const Textarea = React.forwardRef<HTMLTextAreaElement, TextareaProps>(
  ({ className, error, ...props }, ref) => (
    <textarea
      className={cn(
        'flex min-h-[80px] w-full rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50',
        error && 'border-red-500 focus-visible:ring-red-500',
        className
      )}
      ref={ref}
      {...props}
    />
  )
);
Textarea.displayName = 'Textarea';
```

## 🎨 Layout Patterns

### Grid Layout Template
```tsx
// components/layout/Grid.tsx
import React from 'react';
import { cn } from '@/lib/utils';

interface GridProps {
  children: React.ReactNode;
  cols?: 1 | 2 | 3 | 4 | 5 | 6 | 12;
  gap?: 'sm' | 'md' | 'lg' | 'xl';
  className?: string;
}

const gapClasses = {
  sm: 'gap-2',
  md: 'gap-4',
  lg: 'gap-6',
  xl: 'gap-8',
};

const colClasses = {
  1: 'grid-cols-1',
  2: 'grid-cols-1 md:grid-cols-2',
  3: 'grid-cols-1 md:grid-cols-2 lg:grid-cols-3',
  4: 'grid-cols-1 md:grid-cols-2 lg:grid-cols-4',
  5: 'grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5',
  6: 'grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-6',
  12: 'grid-cols-12',
};

export const Grid: React.FC<GridProps> = ({
  children,
  cols = 3,
  gap = 'md',
  className
}) => (
  <div
    className={cn(
      'grid',
      colClasses[cols],
      gapClasses[gap],
      className
    )}
  >
    {children}
  </div>
);

interface GridItemProps {
  children: React.ReactNode;
  span?: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12;
  className?: string;
}

export const GridItem: React.FC<GridItemProps> = ({
  children,
  span = 1,
  className
}) => (
  <div
    className={cn(
      'col-span-1',
      span > 1 && `col-span-${span}`,
      className
    )}
  >
    {children}
  </div>
);
```

### Container Template
```tsx
// components/layout/Container.tsx
import React from 'react';
import { cn } from '@/lib/utils';

interface ContainerProps {
  children: React.ReactNode;
  size?: 'sm' | 'md' | 'lg' | 'xl' | 'full';
  padding?: 'none' | 'sm' | 'md' | 'lg';
  className?: string;
}

const sizeClasses = {
  sm: 'max-w-2xl',
  md: 'max-w-4xl',
  lg: 'max-w-6xl',
  xl: 'max-w-7xl',
  full: 'max-w-full',
};

const paddingClasses = {
  none: '',
  sm: 'px-4',
  md: 'px-6',
  lg: 'px-8',
};

export const Container: React.FC<ContainerProps> = ({
  children,
  size = 'lg',
  padding = 'md',
  className
}) => (
  <div
    className={cn(
      'mx-auto w-full',
      sizeClasses[size],
      paddingClasses[padding],
      className
    )}
  >
    {children}
  </div>
);
```

## 🎭 Animation Patterns

### Animation Utilities
```css
/* src/styles/animations.css */
@layer utilities {
  .animate-fade-in {
    animation: fadeIn 0.5s ease-in-out;
  }
  
  .animate-slide-up {
    animation: slideUp 0.3s ease-out;
  }
  
  .animate-slide-down {
    animation: slideDown 0.3s ease-out;
  }
  
  .animate-scale-in {
    animation: scaleIn 0.2s ease-out;
  }
  
  .animate-bounce-in {
    animation: bounceIn 0.6s ease-out;
  }
  
  .animate-pulse-slow {
    animation: pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }
  
  @keyframes slideUp {
    from {
      transform: translateY(20px);
      opacity: 0;
    }
    to {
      transform: translateY(0);
      opacity: 1;
    }
  }
  
  @keyframes slideDown {
    from {
      transform: translateY(-20px);
      opacity: 0;
    }
    to {
      transform: translateY(0);
      opacity: 1;
    }
  }
  
  @keyframes scaleIn {
    from {
      transform: scale(0.9);
      opacity: 0;
    }
    to {
      transform: scale(1);
      opacity: 1;
    }
  }
  
  @keyframes bounceIn {
    0% {
      transform: scale(0.3);
      opacity: 0;
    }
    50% {
      transform: scale(1.05);
    }
    70% {
      transform: scale(0.9);
    }
    100% {
      transform: scale(1);
      opacity: 1;
    }
  }
}
```

### Animation Components
```tsx
// components/ui/Animated.tsx
import React, { useEffect, useRef, useState } from 'react';
import { cn } from '@/lib/utils';

interface AnimatedProps {
  children: React.ReactNode;
  animation?: 'fadeIn' | 'slideUp' | 'slideDown' | 'scaleIn' | 'bounceIn';
  delay?: number;
  duration?: number;
  trigger?: 'onMount' | 'onScroll' | 'onHover';
  className?: string;
}

export const Animated: React.FC<AnimatedProps> = ({
  children,
  animation = 'fadeIn',
  delay = 0,
  duration = 500,
  trigger = 'onMount',
  className
}) => {
  const [isVisible, setIsVisible] = useState(false);
  const elementRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (trigger === 'onMount') {
      const timer = setTimeout(() => setIsVisible(true), delay);
      return () => clearTimeout(timer);
    }
  }, [delay, trigger]);

  useEffect(() => {
    if (trigger === 'onScroll' && elementRef.current) {
      const observer = new IntersectionObserver(
        ([entry]) => {
          if (entry.isIntersecting) {
            setTimeout(() => setIsVisible(true), delay);
          }
        },
        { threshold: 0.1 }
      );

      observer.observe(elementRef.current);
      return () => observer.disconnect();
    }
  }, [delay, trigger]);

  const animationClasses = {
    fadeIn: 'animate-fade-in',
    slideUp: 'animate-slide-up',
    slideDown: 'animate-slide-down',
    scaleIn: 'animate-scale-in',
    bounceIn: 'animate-bounce-in',
  };

  return (
    <div
      ref={elementRef}
      className={cn(
        'transition-all duration-500',
        isVisible && animationClasses[animation],
        !isVisible && 'opacity-0',
        className
      )}
      style={{ animationDuration: `${duration}ms` }}
    >
      {children}
    </div>
  );
};
```

## 🎨 Responsive Design Patterns

### Responsive Utilities
```tsx
// components/ui/Responsive.tsx
import React from 'react';
import { cn } from '@/lib/utils';

interface ResponsiveProps {
  children: React.ReactNode;
  hide?: {
    sm?: boolean;
    md?: boolean;
    lg?: boolean;
    xl?: boolean;
  };
  show?: {
    sm?: boolean;
    md?: boolean;
    lg?: boolean;
    xl?: boolean;
  };
  className?: string;
}

export const Responsive: React.FC<ResponsiveProps> = ({
  children,
  hide,
  show,
  className
}) => {
  const hideClasses = hide ? Object.entries(hide)
    .filter(([, shouldHide]) => shouldHide)
    .map(([breakpoint]) => `hidden ${breakpoint}:block`)
    .join(' ') : '';

  const showClasses = show ? Object.entries(show)
    .filter(([, shouldShow]) => shouldShow)
    .map(([breakpoint]) => `block ${breakpoint}:hidden`)
    .join(' ') : '';

  return (
    <div className={cn(hideClasses, showClasses, className)}>
      {children}
    </div>
  );
};

// Usage examples:
// <Responsive hide={{ sm: true, lg: false }}>Mobile only</Responsive>
// <Responsive show={{ md: true }}>Desktop only</Responsive>
```

## 🎯 TailwindCSS Best Practices

### Class Organization
1. **Group related classes**: Layout, spacing, colors, typography
2. **Use consistent ordering**: Position → Display → Flexbox/Grid → Spacing → Sizing → Typography → Colors → Effects
3. **Extract common patterns**: Create component classes for repeated patterns
4. **Use responsive prefixes**: Mobile-first approach with responsive breakpoints
5. **Leverage state variants**: hover:, focus:, active:, disabled:, etc.

### Performance Optimization
1. **Use JIT mode**: Enable Just-In-Time compilation for better performance
2. **Purge unused styles**: Configure content paths to remove unused CSS
3. **Use arbitrary values sparingly**: Prefer predefined values when possible
4. **Optimize bundle size**: Use @apply for repeated patterns
5. **Leverage CSS custom properties**: For dynamic values

### Accessibility Guidelines
1. **Use semantic colors**: Ensure sufficient color contrast
2. **Include focus states**: Always add focus-visible: styles
3. **Use proper spacing**: Follow accessibility guidelines for touch targets
4. **Support reduced motion**: Use motion-reduce: prefix for animations
5. **Ensure keyboard navigation**: Use focus: and active: states

### Component Patterns
1. **Create reusable components**: Extract common UI patterns
2. **Use compound components**: For complex UI elements
3. **Implement proper variants**: Use consistent variant patterns
4. **Support composition**: Allow className overrides
5. **Follow design system**: Maintain consistent spacing and colors

## 🔧 Utility Functions

### Class Name Utilities
```typescript
// lib/utils.ts
import { type ClassValue, clsx } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

// Conditional class utilities
export function conditionalClasses(
  baseClasses: string,
  conditionalClasses: Record<string, boolean>
): string {
  const classes = [baseClasses];
  
  Object.entries(conditionalClasses).forEach(([className, condition]) => {
    if (condition) {
      classes.push(className);
    }
  });
  
  return classes.join(' ');
}

// Responsive class utilities
export function responsiveClasses(
  baseClasses: string,
  responsiveClasses: Record<string, string>
): string {
  const classes = [baseClasses];
  
  Object.entries(responsiveClasses).forEach(([breakpoint, className]) => {
    classes.push(`${breakpoint}:${className}`);
  });
  
  return classes.join(' ');
}
```

## 🎯 AI Agent Best Practices

When assisting with TailwindCSS development:

1. **Follow mobile-first approach** with responsive design
2. **Use semantic class names** and consistent patterns
3. **Leverage component composition** for reusable UI elements
4. **Implement proper accessibility** with focus states and ARIA attributes
5. **Optimize for performance** with proper purging and JIT compilation
6. **Use design tokens** consistently across components
7. **Follow the established patterns** in the project's design system
8. **Ensure proper contrast ratios** for accessibility compliance
9. **Use appropriate animations** that respect user preferences
10. **Maintain consistent spacing** using the design system scale

## 🔗 Additional Resources

- [TailwindCSS Documentation](https://tailwindcss.com/docs)
- [TailwindUI Components](https://tailwindui.com/)
- [Headless UI](https://headlessui.com/)
- [TailwindCSS Playground](https://play.tailwindcss.com/)
- [TailwindCSS Cheat Sheet](https://tailwindcomponents.com/cheatsheet/)

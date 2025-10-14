# Bits-UI Development Guide

## 🎯 AI Agent Instructions for Bits-UI

When working with Bits-UI projects, follow these guidelines to ensure optimal AI assistance and consistent component development.

## 📁 Project Structure

```
src/
├── components/
│   ├── ui/                  # Bits-UI components
│   │   ├── accordion/       # Accordion component
│   │   ├── alert-dialog/    # Alert dialog component
│   │   ├── avatar/          # Avatar component
│   │   ├── button/          # Button component
│   │   ├── checkbox/        # Checkbox component
│   │   ├── collapsible/     # Collapsible component
│   │   ├── dialog/          # Dialog component
│   │   ├── dropdown-menu/   # Dropdown menu component
│   │   ├── form/            # Form components
│   │   ├── input/           # Input component
│   │   ├── label/           # Label component
│   │   ├── popover/         # Popover component
│   │   ├── progress/        # Progress component
│   │   ├── radio-group/     # Radio group component
│   │   ├── select/          # Select component
│   │   ├── separator/       # Separator component
│   │   ├── sheet/           # Sheet component
│   │   ├── slider/          # Slider component
│   │   ├── switch/          # Switch component
│   │   ├── tabs/            # Tabs component
│   │   ├── textarea/        # Textarea component
│   │   ├── toast/           # Toast component
│   │   └── tooltip/         # Tooltip component
│   └── composite/           # Composite components using Bits-UI
├── lib/
│   ├── utils.ts             # Utility functions
│   └── bits-ui.ts           # Bits-UI configuration
└── styles/
    └── components.css       # Component-specific styles
```

## 🧩 Component Development Patterns

### Base Component Template
```typescript
// components/ui/button/button.tsx
import { createButton } from '@bits-ui/button';
import { type VariantProps, cva } from 'class-variance-authority';
import { cn } from '@/lib/utils';

const buttonVariants = cva(
  'inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground hover:bg-primary/90',
        destructive: 'bg-destructive text-destructive-foreground hover:bg-destructive/90',
        outline: 'border border-input bg-background hover:bg-accent hover:text-accent-foreground',
        secondary: 'bg-secondary text-secondary-foreground hover:bg-secondary/80',
        ghost: 'hover:bg-accent hover:text-accent-foreground',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        default: 'h-10 px-4 py-2',
        sm: 'h-9 rounded-md px-3',
        lg: 'h-11 rounded-md px-8',
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
    const { buttonProps } = createButton({
      asChild,
      ...props,
    });

    return (
      <button
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...buttonProps}
      />
    );
  }
);

Button.displayName = 'Button';

export { Button, buttonVariants };
```

### Dialog Component Template
```typescript
// components/ui/dialog/dialog.tsx
import * as React from 'react';
import { createDialog } from '@bits-ui/dialog';
import { cn } from '@/lib/utils';

interface DialogProps {
  children: React.ReactNode;
  open?: boolean;
  onOpenChange?: (open: boolean) => void;
  modal?: boolean;
}

const Dialog = React.forwardRef<HTMLDivElement, DialogProps>(
  ({ children, open, onOpenChange, modal = true, ...props }, ref) => {
    const { rootProps, triggerProps, contentProps, titleProps, descriptionProps } = createDialog({
      open,
      onOpenChange,
      modal,
    });

    return (
      <div ref={ref} {...rootProps} {...props}>
        {React.Children.map(children, (child) => {
          if (React.isValidElement(child)) {
            return React.cloneElement(child, {
              triggerProps,
              contentProps,
              titleProps,
              descriptionProps,
            });
          }
          return child;
        })}
      </div>
    );
  }
);

Dialog.displayName = 'Dialog';

const DialogTrigger = React.forwardRef<
  HTMLButtonElement,
  React.ButtonHTMLAttributes<HTMLButtonElement> & { asChild?: boolean }
>(({ className, asChild = false, ...props }, ref) => {
  const { triggerProps } = React.useContext(DialogContext);
  
  if (asChild) {
    return React.cloneElement(props.children as React.ReactElement, {
      ...triggerProps,
      ref,
    });
  }

  return (
    <button
      ref={ref}
      className={cn(
        'inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
        className
      )}
      {...triggerProps}
      {...props}
    />
  );
});

DialogTrigger.displayName = 'DialogTrigger';

const DialogContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, children, ...props }, ref) => {
  const { contentProps } = React.useContext(DialogContext);

  return (
    <div
      ref={ref}
      className={cn(
        'fixed left-[50%] top-[50%] z-50 grid w-full max-w-lg translate-x-[-50%] translate-y-[-50%] gap-4 border bg-background p-6 shadow-lg duration-200 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[state=closed]:slide-out-to-left-1/2 data-[state=closed]:slide-out-to-top-[48%] data-[state=open]:slide-in-from-left-1/2 data-[state=open]:slide-in-from-top-[48%] sm:rounded-lg',
        className
      )}
      {...contentProps}
      {...props}
    >
      {children}
    </div>
  );
});

DialogContent.displayName = 'DialogContent';

const DialogHeader = ({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn('flex flex-col space-y-1.5 text-center sm:text-left', className)}
    {...props}
  />
);

DialogHeader.displayName = 'DialogHeader';

const DialogFooter = ({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn('flex flex-col-reverse sm:flex-row sm:justify-end sm:space-x-2', className)}
    {...props}
  />
);

DialogFooter.displayName = 'DialogFooter';

const DialogTitle = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLHeadingElement>
>(({ className, ...props }, ref) => {
  const { titleProps } = React.useContext(DialogContext);

  return (
    <h2
      ref={ref}
      className={cn('text-lg font-semibold leading-none tracking-tight', className)}
      {...titleProps}
      {...props}
    />
  );
});

DialogTitle.displayName = 'DialogTitle';

const DialogDescription = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLParagraphElement>
>(({ className, ...props }, ref) => {
  const { descriptionProps } = React.useContext(DialogContext);

  return (
    <p
      ref={ref}
      className={cn('text-sm text-muted-foreground', className)}
      {...descriptionProps}
      {...props}
    />
  );
});

DialogDescription.displayName = 'DialogDescription';

export {
  Dialog,
  DialogTrigger,
  DialogContent,
  DialogHeader,
  DialogFooter,
  DialogTitle,
  DialogDescription,
};
```

### Form Component Template
```typescript
// components/ui/form/form.tsx
import * as React from 'react';
import { createForm } from '@bits-ui/form';
import { cn } from '@/lib/utils';

interface FormProps {
  children: React.ReactNode;
  onSubmit?: (data: any) => void;
  validation?: any;
  className?: string;
}

const Form = React.forwardRef<HTMLFormElement, FormProps>(
  ({ children, onSubmit, validation, className, ...props }, ref) => {
    const { formProps, fieldProps, errors } = createForm({
      onSubmit,
      validation,
    });

    return (
      <form
        ref={ref}
        className={cn('space-y-6', className)}
        {...formProps}
        {...props}
      >
        <FormContext.Provider value={{ fieldProps, errors }}>
          {children}
        </FormContext.Provider>
      </form>
    );
  }
);

Form.displayName = 'Form';

const FormContext = React.createContext<{
  fieldProps: any;
  errors: any;
}>({
  fieldProps: {},
  errors: {},
});

const FormField = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & {
    name: string;
    render: (props: any) => React.ReactNode;
  }
>(({ className, name, render, ...props }, ref) => {
  const { fieldProps, errors } = React.useContext(FormContext);
  const field = fieldProps[name];
  const error = errors[name];

  return (
    <div ref={ref} className={cn('space-y-2', className)} {...props}>
      {render({ field, error })}
    </div>
  );
});

FormField.displayName = 'FormField';

const FormItem = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => (
    <div ref={ref} className={cn('space-y-2', className)} {...props} />
  )
);

FormItem.displayName = 'FormItem';

const FormLabel = React.forwardRef<
  HTMLLabelElement,
  React.LabelHTMLAttributes<HTMLLabelElement>
>(({ className, ...props }, ref) => (
  <label
    ref={ref}
    className={cn('text-sm font-medium leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70', className)}
    {...props}
  />
));

FormLabel.displayName = 'FormLabel';

const FormControl = React.forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ ...props }, ref) => <div ref={ref} {...props} />
);

FormControl.displayName = 'FormControl';

const FormDescription = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLParagraphElement>
>(({ className, ...props }, ref) => (
  <p
    ref={ref}
    className={cn('text-sm text-muted-foreground', className)}
    {...props}
  />
));

FormDescription.displayName = 'FormDescription';

const FormMessage = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLParagraphElement>
>(({ className, children, ...props }, ref) => {
  return (
    <p
      ref={ref}
      className={cn('text-sm font-medium text-destructive', className)}
      {...props}
    >
      {children}
    </p>
  );
});

FormMessage.displayName = 'FormMessage';

export {
  Form,
  FormField,
  FormItem,
  FormLabel,
  FormControl,
  FormDescription,
  FormMessage,
};
```

## 🎨 Advanced Component Patterns

### Compound Component Pattern
```typescript
// components/ui/accordion/accordion.tsx
import * as React from 'react';
import { createAccordion } from '@bits-ui/accordion';
import { cn } from '@/lib/utils';

interface AccordionProps {
  children: React.ReactNode;
  type?: 'single' | 'multiple';
  collapsible?: boolean;
  defaultValue?: string | string[];
  value?: string | string[];
  onValueChange?: (value: string | string[]) => void;
  className?: string;
}

const Accordion = React.forwardRef<HTMLDivElement, AccordionProps>(
  ({ children, type = 'single', collapsible = true, defaultValue, value, onValueChange, className, ...props }, ref) => {
    const { rootProps, itemProps, triggerProps, contentProps } = createAccordion({
      type,
      collapsible,
      defaultValue,
      value,
      onValueChange,
    });

    return (
      <div
        ref={ref}
        className={cn('space-y-2', className)}
        {...rootProps}
        {...props}
      >
        {React.Children.map(children, (child) => {
          if (React.isValidElement(child)) {
            return React.cloneElement(child, {
              itemProps,
              triggerProps,
              contentProps,
            });
          }
          return child;
        })}
      </div>
    );
  }
);

Accordion.displayName = 'Accordion';

const AccordionItem = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & { value: string }
>(({ className, value, ...props }, ref) => {
  const { itemProps } = React.useContext(AccordionContext);

  return (
    <div
      ref={ref}
      className={cn('border rounded-md', className)}
      {...itemProps}
      {...props}
    />
  );
});

AccordionItem.displayName = 'AccordionItem';

const AccordionTrigger = React.forwardRef<
  HTMLButtonElement,
  React.ButtonHTMLAttributes<HTMLButtonElement> & { asChild?: boolean }
>(({ className, children, asChild = false, ...props }, ref) => {
  const { triggerProps } = React.useContext(AccordionContext);

  if (asChild) {
    return React.cloneElement(children as React.ReactElement, {
      ...triggerProps,
      ref,
    });
  }

  return (
    <button
      ref={ref}
      className={cn(
        'flex flex-1 items-center justify-between py-4 px-4 font-medium transition-all hover:bg-muted [&[data-state=open]>svg]:rotate-180',
        className
      )}
      {...triggerProps}
      {...props}
    >
      {children}
      <svg
        className="h-4 w-4 shrink-0 transition-transform duration-200"
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <path d="m6 9 6 6 6-6" />
      </svg>
    </button>
  );
});

AccordionTrigger.displayName = 'AccordionTrigger';

const AccordionContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, children, ...props }, ref) => {
  const { contentProps } = React.useContext(AccordionContext);

  return (
    <div
      ref={ref}
      className={cn(
        'overflow-hidden text-sm transition-all data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down',
        className
      )}
      {...contentProps}
      {...props}
    >
      <div className="pb-4 pt-0">{children}</div>
    </div>
  );
});

AccordionContent.displayName = 'AccordionContent';

const AccordionContext = React.createContext<{
  itemProps: any;
  triggerProps: any;
  contentProps: any;
}>({
  itemProps: {},
  triggerProps: {},
  contentProps: {},
});

export {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
};
```

### Select Component with Search
```typescript
// components/ui/select/select.tsx
import * as React from 'react';
import { createSelect } from '@bits-ui/select';
import { cn } from '@/lib/utils';

interface SelectProps {
  children: React.ReactNode;
  value?: string;
  defaultValue?: string;
  onValueChange?: (value: string) => void;
  disabled?: boolean;
  placeholder?: string;
  searchable?: boolean;
  className?: string;
}

const Select = React.forwardRef<HTMLDivElement, SelectProps>(
  ({ children, value, defaultValue, onValueChange, disabled, placeholder, searchable = false, className, ...props }, ref) => {
    const { rootProps, triggerProps, contentProps, itemProps, labelProps, separatorProps } = createSelect({
      value,
      defaultValue,
      onValueChange,
      disabled,
      searchable,
    });

    return (
      <div
        ref={ref}
        className={cn('relative', className)}
        {...rootProps}
        {...props}
      >
        {React.Children.map(children, (child) => {
          if (React.isValidElement(child)) {
            return React.cloneElement(child, {
              triggerProps,
              contentProps,
              itemProps,
              labelProps,
              separatorProps,
            });
          }
          return child;
        })}
      </div>
    );
  }
);

Select.displayName = 'Select';

const SelectTrigger = React.forwardRef<
  HTMLButtonElement,
  React.ButtonHTMLAttributes<HTMLButtonElement> & { asChild?: boolean }
>(({ className, children, asChild = false, ...props }, ref) => {
  const { triggerProps } = React.useContext(SelectContext);

  if (asChild) {
    return React.cloneElement(children as React.ReactElement, {
      ...triggerProps,
      ref,
    });
  }

  return (
    <button
      ref={ref}
      className={cn(
        'flex h-10 w-full items-center justify-between rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background placeholder:text-muted-foreground focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 [&>span]:line-clamp-1',
        className
      )}
      {...triggerProps}
      {...props}
    >
      {children}
      <svg
        className="h-4 w-4 opacity-50"
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <path d="m6 9 6 6 6-6" />
      </svg>
    </button>
  );
});

SelectTrigger.displayName = 'SelectTrigger';

const SelectValue = React.forwardRef<
  HTMLSpanElement,
  React.HTMLAttributes<HTMLSpanElement> & { placeholder?: string }
>(({ className, placeholder, ...props }, ref) => {
  const { value, onValueChange } = React.useContext(SelectContext);

  return (
    <span
      ref={ref}
      className={cn('block truncate', className)}
      {...props}
    >
      {value || placeholder}
    </span>
  );
});

SelectValue.displayName = 'SelectValue';

const SelectContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, children, position = 'popper', ...props }, ref) => {
  const { contentProps } = React.useContext(SelectContext);

  return (
    <div
      ref={ref}
      className={cn(
        'relative z-50 max-h-96 min-w-[8rem] overflow-hidden rounded-md border bg-popover text-popover-foreground shadow-md data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2',
        position === 'popper' &&
          'data-[side=bottom]:translate-y-1 data-[side=left]:-translate-x-1 data-[side=right]:translate-x-1 data-[side=top]:-translate-y-1',
        className
      )}
      {...contentProps}
      {...props}
    >
      <div className="p-1">{children}</div>
    </div>
  );
});

SelectContent.displayName = 'SelectContent';

const SelectItem = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & { value: string }
>(({ className, children, value, ...props }, ref) => {
  const { itemProps } = React.useContext(SelectContext);

  return (
    <div
      ref={ref}
      className={cn(
        'relative flex w-full cursor-default select-none items-center rounded-sm py-1.5 pl-8 pr-2 text-sm outline-none focus:bg-accent focus:text-accent-foreground data-[disabled]:pointer-events-none data-[disabled]:opacity-50',
        className
      )}
      {...itemProps}
      {...props}
    >
      <span className="absolute left-2 flex h-3.5 w-3.5 items-center justify-center">
        <svg
          className="h-4 w-4"
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          strokeWidth="2"
          strokeLinecap="round"
          strokeLinejoin="round"
        >
          <path d="M20 6 9 17l-5-5" />
        </svg>
      </span>
      {children}
    </div>
  );
});

SelectItem.displayName = 'SelectItem';

const SelectContext = React.createContext<{
  value?: string;
  onValueChange?: (value: string) => void;
  triggerProps: any;
  contentProps: any;
  itemProps: any;
  labelProps: any;
  separatorProps: any;
}>({
  triggerProps: {},
  contentProps: {},
  itemProps: {},
  labelProps: {},
  separatorProps: {},
});

export {
  Select,
  SelectTrigger,
  SelectValue,
  SelectContent,
  SelectItem,
};
```

## 🎨 Styling and Theming

### Component Styling Patterns
```css
/* styles/components.css */
@layer components {
  /* Bits-UI specific styles */
  .bits-ui-button {
    @apply inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50;
  }
  
  .bits-ui-dialog-overlay {
    @apply fixed inset-0 z-50 bg-background/80 backdrop-blur-sm data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0;
  }
  
  .bits-ui-dialog-content {
    @apply fixed left-[50%] top-[50%] z-50 grid w-full max-w-lg translate-x-[-50%] translate-y-[-50%] gap-4 border bg-background p-6 shadow-lg duration-200 data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[state=closed]:slide-out-to-left-1/2 data-[state=closed]:slide-out-to-top-[48%] data-[state=open]:slide-in-from-left-1/2 data-[state=open]:slide-in-from-top-[48%] sm:rounded-lg;
  }
  
  .bits-ui-select-trigger {
    @apply flex h-10 w-full items-center justify-between rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background placeholder:text-muted-foreground focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 [&>span]:line-clamp-1;
  }
  
  .bits-ui-select-content {
    @apply relative z-50 max-h-96 min-w-[8rem] overflow-hidden rounded-md border bg-popover text-popover-foreground shadow-md data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0 data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95 data-[side=bottom]:slide-in-from-top-2 data-[side=left]:slide-in-from-right-2 data-[side=right]:slide-in-from-left-2 data-[side=top]:slide-in-from-bottom-2;
  }
  
  .bits-ui-accordion-trigger {
    @apply flex flex-1 items-center justify-between py-4 px-4 font-medium transition-all hover:bg-muted [&[data-state=open]>svg]:rotate-180;
  }
  
  .bits-ui-accordion-content {
    @apply overflow-hidden text-sm transition-all data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down;
  }
}
```

### Theme Configuration
```typescript
// lib/bits-ui.ts
import { createTheme } from '@bits-ui/theme';

export const theme = createTheme({
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
    },
  },
  spacing: {
    xs: '0.5rem',
    sm: '0.75rem',
    md: '1rem',
    lg: '1.5rem',
    xl: '2rem',
  },
  borderRadius: {
    sm: '0.25rem',
    md: '0.375rem',
    lg: '0.5rem',
    xl: '0.75rem',
  },
  shadows: {
    sm: '0 1px 2px 0 rgb(0 0 0 / 0.05)',
    md: '0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)',
    lg: '0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)',
  },
});

export const { css, cx } = theme;
```

## 🧪 Testing Patterns

### Component Testing
```typescript
// components/ui/button/button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './button';

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

  it('applies size classes correctly', () => {
    render(<Button size="lg">Large Button</Button>);
    const button = screen.getByRole('button');
    expect(button).toHaveClass('h-11');
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
});
```

### Integration Testing
```typescript
// components/ui/dialog/dialog.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Dialog, DialogTrigger, DialogContent, DialogTitle, DialogDescription } from './dialog';

describe('Dialog', () => {
  it('opens and closes dialog', () => {
    render(
      <Dialog>
        <DialogTrigger>Open Dialog</DialogTrigger>
        <DialogContent>
          <DialogTitle>Test Dialog</DialogTitle>
          <DialogDescription>This is a test dialog</DialogDescription>
        </DialogContent>
      </Dialog>
    );

    // Dialog should be closed initially
    expect(screen.queryByText('Test Dialog')).not.toBeInTheDocument();

    // Open dialog
    fireEvent.click(screen.getByText('Open Dialog'));
    expect(screen.getByText('Test Dialog')).toBeInTheDocument();
    expect(screen.getByText('This is a test dialog')).toBeInTheDocument();

    // Close dialog with Escape key
    fireEvent.keyDown(document, { key: 'Escape' });
    expect(screen.queryByText('Test Dialog')).not.toBeInTheDocument();
  });

  it('handles focus management', () => {
    render(
      <Dialog>
        <DialogTrigger>Open Dialog</DialogTrigger>
        <DialogContent>
          <DialogTitle>Test Dialog</DialogTitle>
          <button>Focus me</button>
        </DialogContent>
      </Dialog>
    );

    fireEvent.click(screen.getByText('Open Dialog'));
    
    // Focus should be trapped within dialog
    const focusableElement = screen.getByText('Focus me');
    expect(document.activeElement).toBe(focusableElement);
  });
});
```

## 🎯 Bits-UI Best Practices

### Component Design
1. **Use compound components** for complex UI elements
2. **Implement proper accessibility** with ARIA attributes
3. **Follow consistent naming conventions** across components
4. **Use TypeScript** for better type safety
5. **Implement proper focus management** for interactive elements
6. **Use semantic HTML** elements when possible
7. **Follow the established patterns** in the Bits-UI library

### Performance Optimization
1. **Use React.memo** for expensive components
2. **Implement proper key props** for list items
3. **Use lazy loading** for heavy components
4. **Optimize re-renders** with proper dependency arrays
5. **Use CSS-in-JS efficiently** to avoid runtime overhead
6. **Implement proper cleanup** for event listeners and subscriptions

### Accessibility Guidelines
1. **Use proper ARIA attributes** for screen readers
2. **Implement keyboard navigation** for all interactive elements
3. **Ensure proper color contrast** for text and backgrounds
4. **Use semantic HTML** elements for better screen reader support
5. **Implement focus management** for modal and popover components
6. **Provide alternative text** for images and icons
7. **Use proper heading hierarchy** for content structure

## 🎯 AI Agent Best Practices

When assisting with Bits-UI development:

1. **Follow the compound component pattern** for complex UI elements
2. **Use the create* functions** from Bits-UI for proper behavior
3. **Implement proper accessibility** with ARIA attributes and keyboard navigation
4. **Use consistent styling patterns** with TailwindCSS classes
5. **Follow TypeScript best practices** for type safety
6. **Implement proper error handling** and validation
7. **Use the established component patterns** from the library
8. **Ensure proper focus management** for interactive components
9. **Follow the design system** established in the project
10. **Write comprehensive tests** for all components

## 🔗 Additional Resources

- [Bits-UI Documentation](https://bits-ui.com/)
- [Bits-UI GitHub](https://github.com/bitovi/bits-ui)
- [Component Examples](https://bits-ui.com/examples)
- [Accessibility Guide](https://bits-ui.com/accessibility)
- [Migration Guide](https://bits-ui.com/migration)

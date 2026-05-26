---
name: component-library
description: Build design systems and component libraries with Storybook, documentation, accessibility, theming, and versioning. Create reusable UI components that scale.
tags: [components, design-system, storybook, ui, reusable, documentation]
version: 1.0.0
author: Enhanced from design system best practices)
---

# Component Library

## Overview

Build reusable UI components that scale across your application.

**Why component libraries matter:**
- **Consistency:** Same look and feel everywhere
- **Efficiency:** Build once, use everywhere
- **Maintainability:** Update in one place
- **Collaboration:** Designers and developers aligned
- **Quality:** Tested, accessible, documented

**Key principles:**
- **Atomic design:** Build from atoms to organisms
- **Composition:** Combine small components into larger ones
- **Accessibility:** WCAG compliant by default
- **Documentation:** Clear examples and usage
- **Theming:** Customizable appearance

## When to Use

**Use for:**
- Multi-page applications
- Design systems
- Component reuse across projects
- Team collaboration
- Consistent UI/UX

**Critical for:**
- Large applications (10+ pages)
- Multiple products sharing design
- Teams with designers and developers
- Open source UI libraries
- White-label products

## The 6-Layer Component Library Framework

```
┌─────────────────────────────────────────────────────────┐
│        COMPONENT LIBRARY FRAMEWORK                       │
└─────────────────────────────────────────────────────────┘

    ┌──────────────┐
    │   LAYER 1    │  Atomic Design
    │    ATOMIC    │  - Atoms
    │              │  - Molecules
    └──────┬───────┘  - Organisms
           │
           ▼
    ┌──────────────┐
    │   LAYER 2    │  Component API
    │     API      │  - Props
    │              │  - Events
    └──────┬───────┘  - Slots
           │
           ▼
    ┌──────────────┐
    │   LAYER 3    │  Styling
    │   STYLING    │  - CSS-in-JS
    │              │  - CSS Modules
    └──────┬───────┘  - Theming
           │
           ▼
    ┌──────────────┐
    │   LAYER 4    │  Documentation
    │     DOCS     │  - Storybook
    │              │  - Examples
    └──────┬───────┘  - Guidelines
           │
           ▼
    ┌──────────────┐
    │   LAYER 5    │  Testing
    │   TESTING    │  - Unit tests
    │              │  - Visual tests
    └──────┬───────┘  - Accessibility
           │
           ▼
    ┌──────────────┐
    │   LAYER 6    │  Distribution
    │ DISTRIBUTION │  - npm package
    │              │  - Versioning
    └──────────────┘  - Changelog
```

## Layer 1: Atomic Design

### Goal: Build components from small to large

**Atomic Design Hierarchy:**

```markdown
## Atoms (Basic building blocks)

**Examples:**
- Button
- Input
- Label
- Icon
- Typography (Heading, Text)
- Color tokens
- Spacing tokens

**Button.tsx:**
```tsx
import React from 'react';
import './Button.css';

export interface ButtonProps {
  /** Button text */
  children: React.ReactNode;
  /** Button variant */
  variant?: 'primary' | 'secondary' | 'danger';
  /** Button size */
  size?: 'small' | 'medium' | 'large';
  /** Disabled state */
  disabled?: boolean;
  /** Click handler */
  onClick?: () => void;
}

export const Button: React.FC<ButtonProps> = ({
  children,
  variant = 'primary',
  size = 'medium',
  disabled = false,
  onClick,
}) => {
  return (
    <button
      className={`btn btn--${variant} btn--${size}`}
      disabled={disabled}
      onClick={onClick}
      type="button"
    >
      {children}
    </button>
  );
};
```

**Button.css:**
```css
.btn {
  /* Base styles */
  font-family: var(--font-family);
  font-weight: 600;
  border: none;
  border-radius: var(--border-radius);
  cursor: pointer;
  transition: all 0.2s;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* Variants */
.btn--primary {
  background: var(--color-primary);
  color: white;
}

.btn--primary:hover:not(:disabled) {
  background: var(--color-primary-dark);
}

.btn--secondary {
  background: var(--color-secondary);
  color: var(--color-text);
}

.btn--danger {
  background: var(--color-danger);
  color: white;
}

/* Sizes */
.btn--small {
  padding: 0.5rem 1rem;
  font-size: 0.875rem;
}

.btn--medium {
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
}

.btn--large {
  padding: 1rem 2rem;
  font-size: 1.125rem;
}
```

## Molecules (Combinations of atoms)

**Examples:**
- Form field (Label + Input + Error)
- Search bar (Input + Button)
- Card header (Icon + Title + Actions)
- Alert (Icon + Message + Close button)

**FormField.tsx:**
```tsx
import React from 'react';
import { Input, InputProps } from './Input';
import { Label } from './Label';
import './FormField.css';

export interface FormFieldProps extends InputProps {
  /** Field label */
  label: string;
  /** Error message */
  error?: string;
  /** Help text */
  helpText?: string;
  /** Required field */
  required?: boolean;
}

export const FormField: React.FC<FormFieldProps> = ({
  label,
  error,
  helpText,
  required,
  id,
  ...inputProps
}) => {
  const fieldId = id || `field-${label.toLowerCase().replace(/\s/g, '-')}`;
  
  return (
    <div className="form-field">
      <Label htmlFor={fieldId} required={required}>
        {label}
      </Label>
      
      <Input
        id={fieldId}
        aria-invalid={!!error}
        aria-describedby={error ? `${fieldId}-error` : undefined}
        {...inputProps}
      />
      
      {error && (
        <span id={`${fieldId}-error`} className="form-field__error" role="alert">
          {error}
        </span>
      )}
      
      {helpText && !error && (
        <span className="form-field__help">{helpText}</span>
      )}
    </div>
  );
};
```

## Organisms (Complex components)

**Examples:**
- Navigation bar
- Data table
- Modal dialog
- Form
- Card with header, body, footer

**Modal.tsx:**
```tsx
import React, { useEffect, useRef } from 'react';
import { Button } from './Button';
import { Icon } from './Icon';
import './Modal.css';

export interface ModalProps {
  /** Modal title */
  title: string;
  /** Modal content */
  children: React.ReactNode;
  /** Open state */
  isOpen: boolean;
  /** Close handler */
  onClose: () => void;
  /** Footer actions */
  actions?: React.ReactNode;
}

export const Modal: React.FC<ModalProps> = ({
  title,
  children,
  isOpen,
  onClose,
  actions,
}) => {
  const modalRef = useRef<HTMLDivElement>(null);
  
  useEffect(() => {
    if (isOpen) {
      // Focus first focusable element
      const firstFocusable = modalRef.current?.querySelector<HTMLElement>(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      );
      firstFocusable?.focus();
      
      // Trap focus
      const handleKeyDown = (e: KeyboardEvent) => {
        if (e.key === 'Escape') {
          onClose();
        }
      };
      
      document.addEventListener('keydown', handleKeyDown);
      return () => document.removeEventListener('keydown', handleKeyDown);
    }
  }, [isOpen, onClose]);
  
  if (!isOpen) return null;
  
  return (
    <div className="modal-overlay" onClick={onClose}>
      <div
        ref={modalRef}
        className="modal"
        role="dialog"
        aria-modal="true"
        aria-labelledby="modal-title"
        onClick={(e) => e.stopPropagation()}
      >
        <div className="modal__header">
          <h2 id="modal-title" className="modal__title">
            {title}
          </h2>
          <Button
            variant="secondary"
            size="small"
            onClick={onClose}
            aria-label="Close modal"
          >
            <Icon name="close" />
          </Button>
        </div>
        
        <div className="modal__body">{children}</div>
        
        {actions && <div className="modal__footer">{actions}</div>}
      </div>
    </div>
  );
};
```
```

---

## Layer 2: Component API

### Goal: Design intuitive component interfaces

**Props Design:**

```tsx
// ✅ Good: Clear, typed props
interface ButtonProps {
  /** Button text */
  children: React.ReactNode;
  /** Button style variant */
  variant?: 'primary' | 'secondary' | 'danger';
  /** Button size */
  size?: 'small' | 'medium' | 'large';
  /** Disabled state */
  disabled?: boolean;
  /** Loading state */
  loading?: boolean;
  /** Icon to display before text */
  icon?: React.ReactNode;
  /** Click handler */
  onClick?: () => void;
}

// ❌ Bad: Unclear, untyped props
interface ButtonProps {
  text: any;
  type: string;
  big?: boolean;
  handler: Function;
}
```

**Composition Patterns:**

```tsx
// Compound components
<Select>
  <Select.Option value="1">Option 1</Select.Option>
  <Select.Option value="2">Option 2</Select.Option>
</Select>

// Render props
<DataTable
  data={users}
  renderRow={(user) => (
    <tr>
      <td>{user.name}</td>
      <td>{user.email}</td>
    </tr>
  )}
/>

// Slots (children as object)
<Card>
  <Card.Header>Title</Card.Header>
  <Card.Body>Content</Card.Body>
  <Card.Footer>Actions</Card.Footer>
</Card>
```

**Controlled vs Uncontrolled:**

```tsx
// Controlled (parent manages state)
const [value, setValue] = useState('');

<Input
  value={value}
  onChange={(e) => setValue(e.target.value)}
/>

// Uncontrolled (component manages state)
<Input defaultValue="initial" />

// Support both
interface InputProps {
  value?: string;           // Controlled
  defaultValue?: string;    // Uncontrolled
  onChange?: (value: string) => void;
}
```

---

## Layer 3: Styling

### Goal: Flexible, maintainable styling

**CSS Modules:**

```tsx
// Button.module.css
.button {
  padding: 0.75rem 1.5rem;
  border-radius: 0.25rem;
}

.button--primary {
  background: var(--color-primary);
  color: white;
}

// Button.tsx
import styles from './Button.module.css';

export const Button = ({ variant = 'primary', children }) => (
  <button className={`${styles.button} ${styles[`button--${variant}`]}`}>
    {children}
  </button>
);
```

**CSS-in-JS (styled-components):**

```tsx
import styled from 'styled-components';

const StyledButton = styled.button<{ variant: string; size: string }>`
  padding: ${props => props.size === 'small' ? '0.5rem 1rem' : '0.75rem 1.5rem'};
  background: ${props => props.theme.colors[props.variant]};
  color: white;
  border: none;
  border-radius: ${props => props.theme.borderRadius};
  cursor: pointer;
  
  &:hover {
    background: ${props => props.theme.colors[`${props.variant}Dark`]};
  }
  
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
`;

export const Button = ({ variant = 'primary', size = 'medium', children, ...props }) => (
  <StyledButton variant={variant} size={size} {...props}>
    {children}
  </StyledButton>
);
```

**Theming:**

```tsx
// theme.ts
export const theme = {
  colors: {
    primary: '#0066cc',
    primaryDark: '#0052a3',
    secondary: '#6c757d',
    danger: '#dc3545',
    success: '#28a745',
    warning: '#ffc107',
    text: '#212529',
    textLight: '#6c757d',
    background: '#ffffff',
    border: '#dee2e6',
  },
  spacing: {
    xs: '0.25rem',
    sm: '0.5rem',
    md: '1rem',
    lg: '1.5rem',
    xl: '2rem',
  },
  borderRadius: '0.25rem',
  fontFamily: '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif',
  fontSize: {
    sm: '0.875rem',
    md: '1rem',
    lg: '1.125rem',
    xl: '1.25rem',
  },
};

// App.tsx
import { ThemeProvider } from 'styled-components';
import { theme } from './theme';

function App() {
  return (
    <ThemeProvider theme={theme}>
      <YourApp />
    </ThemeProvider>
  );
}
```

**CSS Variables:**

```css
/* tokens.css */
:root {
  /* Colors */
  --color-primary: #0066cc;
  --color-primary-dark: #0052a3;
  --color-secondary: #6c757d;
  --color-danger: #dc3545;
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  
  /* Typography */
  --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  --font-size-sm: 0.875rem;
  --font-size-md: 1rem;
  --font-size-lg: 1.125rem;
  
  /* Border */
  --border-radius: 0.25rem;
  --border-width: 1px;
  --border-color: #dee2e6;
}

/* Dark theme */
[data-theme="dark"] {
  --color-primary: #4da6ff;
  --color-text: #ffffff;
  --color-background: #1a1a1a;
  --border-color: #333333;
}
```

---

## Layer 4: Documentation

### Goal: Clear, comprehensive documentation

**Storybook Setup:**

```bash
# Install Storybook
npx storybook@latest init

# Run Storybook
npm run storybook
```

**Button.stories.tsx:**

```tsx
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from './Button';

const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'danger'],
      description: 'Button style variant',
    },
    size: {
      control: 'select',
      options: ['small', 'medium', 'large'],
      description: 'Button size',
    },
    disabled: {
      control: 'boolean',
      description: 'Disabled state',
    },
  },
};

export default meta;
type Story = StoryObj<typeof Button>;

// Default story
export const Primary: Story = {
  args: {
    children: 'Button',
    variant: 'primary',
  },
};

// Variants
export const Secondary: Story = {
  args: {
    children: 'Button',
    variant: 'secondary',
  },
};

export const Danger: Story = {
  args: {
    children: 'Delete',
    variant: 'danger',
  },
};

// Sizes
export const Small: Story = {
  args: {
    children: 'Small Button',
    size: 'small',
  },
};

export const Large: Story = {
  args: {
    children: 'Large Button',
    size: 'large',
  },
};

// States
export const Disabled: Story = {
  args: {
    children: 'Disabled',
    disabled: true,
  },
};

export const WithIcon: Story = {
  args: {
    children: 'Download',
    icon: '⬇️',
  },
};

// All variants showcase
export const AllVariants: Story = {
  render: () => (
    <div style={{ display: 'flex', gap: '1rem', flexWrap: 'wrap' }}>
      <Button variant="primary">Primary</Button>
      <Button variant="secondary">Secondary</Button>
      <Button variant="danger">Danger</Button>
    </div>
  ),
};
```

**Component Documentation:**

```markdown
# Button

A button component for user actions.

## Usage

```tsx
import { Button } from '@your-library/components';

function App() {
  return (
    <Button variant="primary" onClick={() => alert('Clicked!')}>
      Click me
    </Button>
  );
}
```

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `children` | `ReactNode` | - | Button text or content |
| `variant` | `'primary' \| 'secondary' \| 'danger'` | `'primary'` | Button style variant |
| `size` | `'small' \| 'medium' \| 'large'` | `'medium'` | Button size |
| `disabled` | `boolean` | `false` | Disabled state |
| `loading` | `boolean` | `false` | Loading state |
| `onClick` | `() => void` | - | Click handler |

## Examples

### Primary Button
```tsx
<Button variant="primary">Save</Button>
```

### Danger Button
```tsx
<Button variant="danger">Delete</Button>
```

### Disabled Button
```tsx
<Button disabled>Disabled</Button>
```

## Accessibility

- Uses semantic `<button>` element
- Supports keyboard navigation (Enter, Space)
- Includes `disabled` attribute for disabled state
- Supports `aria-label` for icon-only buttons

## Best Practices

- Use `primary` for main actions
- Use `secondary` for secondary actions
- Use `danger` for destructive actions
- Always provide meaningful button text
- Use `aria-label` for icon-only buttons
```

---

## Layer 5: Testing

### Goal: Ensure components work correctly

**Unit Tests (Jest + React Testing Library):**

```tsx
// Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders button text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });
  
  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
  
  it('does not call onClick when disabled', () => {
    const handleClick = jest.fn();
    render(<Button disabled onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).not.toHaveBeenCalled();
  });
  
  it('applies variant class', () => {
    const { container } = render(<Button variant="danger">Delete</Button>);
    expect(container.firstChild).toHaveClass('btn--danger');
  });
  
  it('applies size class', () => {
    const { container } = render(<Button size="large">Large</Button>);
    expect(container.firstChild).toHaveClass('btn--large');
  });
});
```

**Visual Regression Tests (Chromatic):**

```bash
# Install Chromatic
npm install --save-dev chromatic

# Run visual tests
npx chromatic --project-token=<your-token>
```

**Accessibility Tests:**

```tsx
// Button.a11y.test.tsx
import { render } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import { Button } from './Button';

expect.extend(toHaveNoViolations);

describe('Button accessibility', () => {
  it('has no accessibility violations', async () => {
    const { container } = render(<Button>Click me</Button>);
    const results = await axe(container);
    expect(results).toHaveNoViolations();
  });
  
  it('is keyboard accessible', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    const button = screen.getByText('Click me');
    button.focus();
    expect(button).toHaveFocus();
    
    fireEvent.keyDown(button, { key: 'Enter' });
    expect(handleClick).toHaveBeenCalled();
  });
});
```

---

## Layer 6: Distribution

### Goal: Package and distribute components

**Package Structure:**

```
my-component-library/
├── src/
│   ├── components/
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.css
│   │   │   ├── Button.test.tsx
│   │   │   ├── Button.stories.tsx
│   │   │   └── index.ts
│   │   ├── Input/
│   │   └── index.ts
│   ├── theme/
│   │   ├── tokens.css
│   │   └── theme.ts
│   └── index.ts
├── package.json
├── tsconfig.json
├── .storybook/
└── README.md
```

**package.json:**

```json
{
  "name": "@your-org/component-library",
  "version": "1.0.0",
  "description": "Reusable UI components",
  "main": "dist/index.js",
  "module": "dist/index.esm.js",
  "types": "dist/index.d.ts",
  "files": [
    "dist"
  ],
  "scripts": {
    "build": "rollup -c",
    "test": "jest",
    "storybook": "storybook dev -p 6006",
    "build-storybook": "storybook build"
  },
  "peerDependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  },
  "devDependencies": {
    "@rollup/plugin-typescript": "^11.0.0",
    "@storybook/react": "^7.0.0",
    "@testing-library/react": "^14.0.0",
    "jest": "^29.0.0",
    "rollup": "^3.0.0",
    "typescript": "^5.0.0"
  }
}
```

**Versioning (Semantic Versioning):**

```markdown
## Version Format: MAJOR.MINOR.PATCH

**MAJOR (1.0.0 → 2.0.0):**
- Breaking changes
- Removed components
- Changed component APIs
- Renamed props

**MINOR (1.0.0 → 1.1.0):**
- New components
- New features
- New props (backward compatible)

**PATCH (1.0.0 → 1.0.1):**
- Bug fixes
- Documentation updates
- Performance improvements

## Changelog

### [1.2.0] - 2026-05-26
#### Added
- New `Modal` component
- `loading` prop to `Button`

#### Changed
- Improved `Input` accessibility

#### Fixed
- `Button` focus indicator in Safari

### [1.1.0] - 2026-05-01
#### Added
- New `Card` component
- Dark theme support

### [1.0.0] - 2026-04-01
#### Added
- Initial release
- `Button`, `Input`, `FormField` components
```

**Publishing:**

```bash
# Build
npm run build

# Test
npm test

# Publish to npm
npm publish --access public

# Or publish to private registry
npm publish --registry https://your-registry.com
```

---

## Component Library Checklist

**Setup:**
- [ ] Project structure created
- [ ] TypeScript configured
- [ ] Storybook installed
- [ ] Testing framework setup
- [ ] Build tool configured (Rollup, Webpack)

**Components:**
- [ ] Atomic design hierarchy followed
- [ ] Props well-typed and documented
- [ ] Accessible (WCAG AA)
- [ ] Responsive
- [ ] Themeable

**Documentation:**
- [ ] Storybook stories for all components
- [ ] Usage examples
- [ ] Props documented
- [ ] Accessibility notes
- [ ] Best practices

**Testing:**
- [ ] Unit tests (>80% coverage)
- [ ] Accessibility tests
- [ ] Visual regression tests
- [ ] Cross-browser testing

**Distribution:**
- [ ] Package.json configured
- [ ] Build output optimized
- [ ] Versioning strategy defined
- [ ] Changelog maintained
- [ ] Published to npm

---

## Common Component Library Mistakes

### Mistake 1: Too Many Props
**Problem:** Components with 20+ props

**Solution:** Composition over configuration

### Mistake 2: Inconsistent Naming
**Problem:** `onClick`, `onPress`, `handleClick`

**Solution:** Consistent naming conventions

### Mistake 3: No Accessibility
**Problem:** Components not keyboard/screen reader accessible

**Solution:** Build accessibility in from start

### Mistake 4: Tight Coupling
**Problem:** Components depend on specific state management

**Solution:** Keep components framework-agnostic

### Mistake 5: No Documentation
**Problem:** Developers don't know how to use components

**Solution:** Storybook + comprehensive docs

### Mistake 6: Breaking Changes
**Problem:** Frequent breaking changes

**Solution:** Semantic versioning, deprecation warnings

---

## Integration with Other Skills

**Use before building component library:**
- `accessibility-audit` - Accessible components
- `responsive-design` - Responsive components
- `api-design` - Component API design

**Use during development:**
- `test-driven-development` - Test components
- `requesting-code-review` - Review components

**Use after building:**
- `user-research` - Test with users
- `performance-optimization` - Optimize components

---

**Remember:** Component libraries are about consistency and reusability. Start small (Button, Input, Card), then grow. Document everything. Test thoroughly. Version carefully. Listen to users.

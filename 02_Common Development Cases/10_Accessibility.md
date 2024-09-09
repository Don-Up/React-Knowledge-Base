# Accessibility

## Implementing ARIA Roles
### 1. Accessible Rich Internet Applications

**Implementing ARIA Roles for Accessibility:**

Enhance accessibility by adding ARIA roles to improve screen reader support.

**Example Code:**

```tsx
import React from 'react';

const AccessibleButton: React.FC = () => {
  return (
    <button 
      aria-label="Close"  // Descriptive label for screen readers
      role="button"       // Explicitly defines the role
      onClick={() => alert('Button clicked')}
    >
      X
    </button>
  );
};

export default AccessibleButton;
```

**Comments in Code:**

- **`aria-label`**: Provides a descriptive label for screen/skriːn/ readers.
- **`role="button"`**: Explicitly defines the element's role as a button.

### 2. Enhancing Screen Reader Support

**Enhancing Screen Reader Support with ARIA Roles:**

Improve screen reader support by using ARIA roles to provide context and descriptions.

**Example Code:**

```tsx
const AccessibleForm: React.FC = () => {
  return (
    <form aria-labelledby="form-title"> {/* Describes the form with a label */}
      <h2 id="form-title">Sign Up</h2> {/* ARIA label for the form */}
      <label htmlFor="username">Username</label>
      <input
        id="username"
        type="text"
        aria-required="true"  // Indicates the field is required
      />
      <button type="submit">Submit</button>
    </form>
  );
};
```

**Comments in Code:**

- **`aria-labelledby`**: Associates the form with a descriptive title.
- **`aria-required`**: Marks the input field as required for form completion.



## Ensuring Keyboard Navigation
### 1. Handling Focus Management

**Ensuring Keyboard Navigation with Focus Management:**

Implement focus management to ensure accessible keyboard navigation.

**Example Code:**

```tsx
const FocusableComponent: React.FC = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  // Manage focus on component mount
  useEffect(() => {
    inputRef.current?.focus(); // Automatically focus on input when component mounts
  }, []);

  return (
    <div>
      <button>Click Me</button>
      <input
        ref={inputRef} // Ref to manage focus
        type="text"
        aria-label="Focusable input" // Describes input for screen readers
      />
    </div>
  );
};
```

**Comments in Code:**

- **`useRef`**: Creates a reference to the input element for focus management.
- **`useEffect`**: Focuses on the input field when the component mounts.
- **`aria-label`**: Provides a description for screen/skriːn/ readers.

### 2. Testing with Keyboard Navigation

**Testing Keyboard Navigation:**

Ensure components are accessible via keyboard by simulating user interactions.

**Example Code:**

```tsx
const KeyboardAccessibleComponent: React.FC = () => (
  <div>
    <button>First Button</button>
    <button>Second Button</button>
  </div>
);
```

**Test Code with React Testing Library:**

```js
test('navigates buttons using keyboard', () => {
  render(<KeyboardAccessibleComponent />);

  // Focus first button using Tab key
  const firstButton = screen.getByText('First Button');
  const secondButton = screen.getByText('Second Button');

  // Simulate Tab key to focus the first button
  fireEvent.focus(firstButton);
  expect(firstButton).toHaveFocus();

  // Simulate Tab key to move focus to the second button
  fireEvent.keyDown(firstButton, { key: 'Tab', code: 'Tab' });
  expect(secondButton).toHaveFocus();
});
```

**Comments in Code:**

- **`fireEvent.focus`**: Simulates focusing on elements.
- **`fireEvent.keyDown`**: Simulates keyboard events like Tab for navigation.
- **`expect(...).toHaveFocus()`**: Verifies the focus state of elements.



## Testing for Accessibility
### 1. Using Tools like Axe

**Testing for Accessibility with Axe:**

Use the Axe library to check for accessibility issues in React components.

**Example Code:**

```tsx
import { render } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import MyComponent from './MyComponent';

// Extend Jest with toHaveNoViolations
expect.extend(toHaveNoViolations);

test('should have no accessibility violations', async () => {
  const { container } = render(<MyComponent />);
  
  // Run accessibility checks
  const results = await axe(container);
  
  // Assert no violations
  expect(results).toHaveNoViolations();
});
```

**Comments in Code:**

- **`jest-axe`**: Library for accessibility testing.
- **`render(<MyComponent />)`**: Renders the component to be tested.
- **`axe(container)`**: Analyzes the rendered component for accessibility issues.
- **`toHaveNoViolations()`**: Asserts that there are no accessibility violations.

### 2. Conducting Manual Tests

**Conducting Manual Accessibility Tests:**

Manually test React components using browser tools and keyboard navigation.

**Example Code:**

```tsx
// Sample component with accessibility features
const MyComponent: React.FC = () => (
  <div>
    <button aria-label="Submit">Submit</button>
    <input type="text" placeholder="Enter text" />
  </div>
);

// Manual Testing Steps:
// 1. **Keyboard Navigation**: Test tab navigation and focus order.
// 2. **Screen Readers**: Verify ARIA labels are read correctly by screen readers.
// 3. **Color Contrast**: Check text and background color contrast.
```

**Comments in Code:**

- **`aria-label`**: Used for screen/skriːn/ reader support.
- **Manual Testing Steps**: Instructions for testing keyboard navigation, screen/skriːn/ readers, and color contrast.
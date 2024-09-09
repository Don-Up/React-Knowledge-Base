# Documentation

## Documenting Components and Hooks

Use comments and tools like JSDoc to document React components and hooks for better maintainability.

**Example Code:**

```tsx
/**
 * Custom hook for managing input value.
 * @param initialValue - Initial value of the input.
 * @returns A tuple with the input value and a setter function.
 */
function useInput(initialValue: string): [string, React.Dispatch<React.SetStateAction<string>>] {
  const [value, setValue] = useState(initialValue);
  return [value, setValue];
}

/**
 * Example component using the useInput hook.
 * @returns JSX.Element
 */
const ExampleComponent: React.FC = () => {
  const [inputValue, setInputValue] = useInput('');

  return (
    <div>
      <input
        type="text"
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        placeholder="Type something"
      />
    </div>
  );
};
```

**Comments in Code:**

- **JSDoc**: Describes the purpose and usage of `useInput` hook and `ExampleComponent`.



## Using Storybook for UI Components

Storybook helps visualize/ˈvɪʒuəlaɪz/ and document React UI components in isolation. It enables interactive testing and showcases different states.

**Example Code:**

1. **Component (`Button.tsx`):**

```tsx
/**
 * Button component for user actions.
 * @param label - Button text.
 * @param onClick - Click handler function.
 * @returns JSX.Element
 */
const Button: React.FC<{ label: string; onClick: () => void }> = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);
```

1. **Story (`Button.stories.tsx`):**

```tsx
import { Meta, Story } from '@storybook/react';

export default {
  title: 'Components/Button',
  component: Button,
} as Meta;

/**
 * Default button story.
 * @returns JSX.Element
 */
export const Default: Story<{ label: string; onClick: () => void }> = (args) => (
  <Button {...args} />
);

Default.args = {
  label: 'Click Me',
  onClick: () => alert('Button clicked!'),
};
```

**Comments in Code:**

- **Component**: Describes `Button` with its props.
- **Storybook**: Defines a `Default` story to showcase `Button` with a label and click handler.



## Creating API Documentation for Props

**Creating API Documentation for Props:**

Use TypeScript and JSDoc to document props for React components, aiding developers in understanding usage and requirements.

**Example Code:**

1. **Component (`Card.tsx`):**

```tsx
/**
 * Card component to display content in a card layout.
 * @param title - The title of the card.
 * @param content - The content to be displayed in the card.
 * @param footer - Optional footer text.
 * @returns JSX.Element
 */
const Card: React.FC<{ title: string; content: string; footer?: string }> = ({ title, content, footer }) => (
  <div className="card">
    <h2>{title}</h2>
    <p>{content}</p>
    {footer && <footer>{footer}</footer>}
  </div>
);
```

**Comments in Code:**

- **`title`**: The card's title.
- **`content`**: Main content of the card.
- **`footer`**: Optional footer text.
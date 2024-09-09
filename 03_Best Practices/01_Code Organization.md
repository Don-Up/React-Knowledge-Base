# Code Organization

## Structuring Files and Folders

Organize React TypeScript projects by separating components, hooks, and utilities into distinct folders. This improves maintainability and scalability.

**Demo Structure:**

```
/src
  /components
    /Button
      Button.tsx
      Button.test.tsx
      Button.styles.ts
    /Header
      Header.tsx
      Header.test.tsx
      Header.styles.ts
  /hooks
    useFetch.ts
  /utils
    api.ts
  /pages
    Home.tsx
    About.tsx
  App.tsx
  index.tsx
```

**Usage:**

1. **Components**: Place reusable UI components in their own folders.
2. **Hooks**: Store custom hooks in a dedicated folder.
3. **Utils**: Organize utility functions separately.

This structure promotes clarity and ease of navigation in your project.



## Organizing Components and Hooks

**React TS - Code Organization: Organizing Components and Hooks**

Structure components and hooks by creating dedicated folders. Each component and its related files (e.g., styles, tests) should reside in its folder, while hooks are organized separately for clarity and reusability.

**Demo Structure:**

```
/src
  /components
    /Button
      Button.tsx
      Button.test.tsx
      Button.styles.ts
    /Header
      Header.tsx
      Header.test.tsx
      Header.styles.ts
  /hooks
    useFetch.ts
    useToggle.ts
  App.tsx
  index.tsx
```

**Usage:**

1. **Components**: Store UI components and related files (styles, tests) in dedicated folders.
2. **Hooks**: Organize custom hooks in a separate folder.

This setup enhances modularity and ease of maintenance.



## Managing Dependencies and Imports

Organize dependencies and imports by grouping them logically and avoiding deep relative paths. Use index files to simplify imports and maintain clarity.

**Demo Code:**

**/src/index.ts**
```typescript
export * from './components';
export * from './hooks';
export * from './utils';
```

**/src/components/Button/Button.tsx**
```tsx
import React from 'react';
import './Button.styles.ts';

const Button: React.FC = () => {
  return <button>Click Me</button>;
};

export default Button;
```

**/src/App.tsx**
```tsx
import React from 'react';
import Button from './components/Button/Button'; // Import from index file
import { useFetch } from './hooks/useFetch';

const App: React.FC = () => {
  const data = useFetch();

  return (
    <div>
      <Button />
      <div>{data}</div>
    </div>
  );
};

export default App;
```

**Usage:**

1. **Group Imports**: Use index files to re-export modules for easier imports.
2. **Avoid Deep Paths**: Simplify import statements to avoid long relative paths.

This practice improves code readability and maintainability.
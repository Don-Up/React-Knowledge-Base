# Error Handling

## Implementing Error Boundaries
### 1. Catching JavaScript Errors

**React TS - Error Handling: Implementing Error Boundaries**

Error boundaries in React are used to catch JavaScript errors anywhere in their child component tree and log those errors or display fallback UI. 

Implementing them in TypeScript involves creating a class component with `componentDidCatch` and `getDerivedStateFromError` methods.

**Demo Code:**

```tsx
import React, { Component, ErrorInfo, ReactNode } from 'react';

// Define Props and State types
interface ErrorBoundaryProps {
  children: ReactNode;
}

interface ErrorBoundaryState {
  hasError: boolean;
}

// Error Boundary Component
class ErrorBoundary extends Component<ErrorBoundaryProps, ErrorBoundaryState> {
  constructor(props: ErrorBoundaryProps) {
    super(props);
    this.state = { hasError: false };
  }

  // Catch errors in child components
  static getDerivedStateFromError(): Partial<ErrorBoundaryState> {
    return { hasError: true };
  }

  // Log error information
  componentDidCatch(error: Error, errorInfo: ErrorInfo): void {
    console.error("Error caught by ErrorBoundary:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>; // Fallback UI
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

**Usage:**

Wrap your components with `ErrorBoundary` to handle errors:

```tsx
<ErrorBoundary>
  <YourComponent />
</ErrorBoundary>
```

This setup will display "Something went wrong." if an error occurs in `YourComponent` or its children.

### 2. Displaying Fallback UI

Error boundaries can display a fallback UI when an error occurs. Implement this by defining a fallback component within the `render` method of your error boundary.

**Demo Code:**

```tsx
import React, { Component, ErrorInfo, ReactNode } from 'react';

// Define Props and State types
interface ErrorBoundaryProps {
  children: ReactNode;
}

interface ErrorBoundaryState {
  hasError: boolean;
}

// Error Boundary Component
class ErrorBoundary extends Component<ErrorBoundaryProps, ErrorBoundaryState> {
  constructor(props: ErrorBoundaryProps) {
    super(props);
    this.state = { hasError: false };
  }

  // Update state to show fallback UI on error
  static getDerivedStateFromError(): Partial<ErrorBoundaryState> {
    return { hasError: true };
  }

  // Log error information
  componentDidCatch(error: Error, errorInfo: ErrorInfo): void {
    console.error("Error caught by ErrorBoundary:", error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // Fallback UI
      return (
        <div>
          <h1>Something went wrong.</h1>
          <button onClick={() => window.location.reload()}>Reload</button>
        </div>
      );
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

**Usage:**

Wrap your components with `ErrorBoundary`:

```tsx
<ErrorBoundary>
  <YourComponent />
</ErrorBoundary>
```

The fallback UI ("Something went wrong.") will be shown if an error occurs in `YourComponent` or its children, with an option to reload the page.



## Handling Errors in Async Code
### 1. Try-Catch with Promises

In React with TypeScript, handle errors in async code using `try-catch` blocks within `async` functions. This helps catch and handle errors from asynchronous operations like data fetching.

**Demo Code:**

```tsx
import React, { useState } from 'react';

const FetchDataComponent: React.FC = () => {
  const [data, setData] = useState<string | null>(null);
  const [error, setError] = useState<string | null>(null);

  const fetchData = async () => {
    try {
      const response = await fetch('https://api.example.com/data');
      if (!response.ok) throw new Error('Network response was not ok');
      const result = await response.json();
      setData(result.data);
    } catch (error) {
      setError((error as Error).message);
    }
  };

  return (
    <div>
      <button onClick={fetchData}>Fetch Data</button>
      {data && <div>Data: {data}</div>}
      {error && <div>Error: {error}</div>}
    </div>
  );
};

export default FetchDataComponent;
```

**Usage:**

Use `fetchData` in your component to handle data fetching and error handling. The `catch` block catches errors from the `fetch` operation and sets an error message.

### 2. Error Handling in useEffect

Handle errors in asynchronous operations within `useEffect` by using a `try-catch` block inside an asynchronous function. This ensures errors are caught and managed properly.

**Demo Code:**

```tsx
import React, { useState, useEffect } from 'react';

const FetchDataComponent: React.FC = () => {
  const [data, setData] = useState<string | null>(null);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) throw new Error('Network response was not ok');
        const result = await response.json();
        setData(result.data);
      } catch (error) {
        setError((error as Error).message);
      }
    };

    fetchData();
  }, []); // Empty dependency array to run only once

  return (
    <div>
      {data && <div>Data: {data}</div>}
      {error && <div>Error: {error}</div>}
    </div>
  );
};

export default FetchDataComponent;
```

**Usage:**

In `useEffect`, `fetchData` handles asynchronous data fetching. Errors are caught by `try-catch` and displayed as needed.



## Debugging React Applications
### 1. Using DevTools for Debugging

Use React DevTools for debugging by inspecting component hierarchies, props, and state. This helps identify issues in rendering and state management.

**Demo Code:**

No code is required for this; instead, use React DevTools in your browser:

1. **Install React DevTools**: Available as a browser extension for Chrome and Firefox.
2. **Open DevTools**: Access through the browser’s developer tools.
3. **Inspect Components**: View component trees, props, and state.

**Usage:**

1. **Components Panel**: Inspect the component tree and state.
2. **Profiler**: Analyze performance and rendering behavior.
3. **Console**: Check logs and error messages.

React DevTools helps debug and optimize React applications efficiently.

### 2. Analyzing Component Behavior

Analyze component behavior using React DevTools by inspecting props, state, and re-renders. This helps understand why a component behaves unexpectedly.

**Demo Code:**

No code needed; use React DevTools:

1. **Inspect Props/State**: Select a component to view its current props and state.
2. **Track Re-renders**: Check which components re-render and why.
3. **Check Component Hierarchy**: View parent-child relationships and props flow.

**Usage:**

1. **Open DevTools**: Use the React tab.
2. **Select Component**: Click on a component in the tree.
3. **Analyze Data**: Observe props, state, and render behavior.

React DevTools is essential for debugging and analyzing component behavior.
# Performance Optimization

## Avoiding Unnecessary Re-renders
### 1. React.memo for Functional Components

**Avoiding Unnecessary Re-renders with React.memo:**

Optimize performance by preventing re-renders of functional components with unchanged props.

**Example Code:**

```tsx
import React, { memo } from 'react';

// Functional component wrapped with React.memo to avoid unnecessary re-renders
const ExpensiveComponent: React.FC<{ value: number }> = memo(({ value }) => {
  console.log('Rendering:', value);
  return <div>Value: {value}</div>;
});

const ParentComponent: React.FC = () => {
  const [count, setCount] = React.useState(0);

  return (
    <div>
      <ExpensiveComponent value={count} />
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};

export default ParentComponent;
```

**Comments in Code:**
- **`ExpensiveComponent`**: Wrapped with `React.memo` to prevent re-rendering if `value` prop doesn't change.
- **`ParentComponent`**: Renders `ExpensiveComponent` and updates `count` state, demonstrating how `React.memo` helps in performance optimization.

### 2. shouldComponentUpdate for Class Components

**Avoiding Unnecessary Re-renders with shouldComponentUpdate:**

Optimize performance by preventing unnecessary re-renders in class components using `shouldComponentUpdate`.

**Example Code:**

```tsx
import React, { Component } from 'react';

// Class component with shouldComponentUpdate for performance optimization
class ExpensiveComponent extends Component<{ value: number }> {
  // Only re-render if `value` prop changes
  shouldComponentUpdate(nextProps: { value: number }) {
    return nextProps.value !== this.props.value;
  }

  render() {
    console.log('Rendering:', this.props.value);
    return <div>Value: {this.props.value}</div>;
  }
}

class ParentComponent extends Component {
  state = { count: 0 };

  render() {
    return (
      <div>
        <ExpensiveComponent value={this.state.count} />
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Increment
        </button>
      </div>
    );
  }
}

export default ParentComponent;
```

**Comments in Code:**
- **`shouldComponentUpdate`**: Method in `ExpensiveComponent` to check if `value` prop has changed before re-rendering.
- **`ParentComponent`**: Demonstrates the use of `shouldComponentUpdate` to optimize rendering of `ExpensiveComponent`.



## Memoization Techniques
### 1. useMemo for Expensive Calculations

**Memoization with `useMemo` for Expensive Calculations:**

Optimize performance by memoizing results of expensive calculations with `useMemo`.

**Example Code:**

```tsx
import React, { useState, useMemo } from 'react';

// Component demonstrating useMemo
const ExpensiveComponent: React.FC = () => {
  const [count, setCount] = useState(0);
  const [input, setInput] = useState('');

  // Expensive calculation
  const expensiveCalculation = (num: number) => {
    console.log('Expensive calculation running...');
    return num * 2;
  };

  // Memoized result of expensive calculation
  const memoizedResult = useMemo(() => expensiveCalculation(count), [count]);

  return (
    <div>
      <p>Result: {memoizedResult}</p>
      <input type="text" value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
    </div>
  );
};

export default ExpensiveComponent;
```

**Comments in Code:**
- **`useMemo`**: Memoizes the result of `expensiveCalculation`, re-computing only when `count` changes.
- **`expensiveCalculation`**: Simulates an expensive computation to demonstrate `useMemo`'s benefit.
- **`setInput`**: Input field change does not trigger re-computation of the memoized value.

### 2. useCallback for Stable Functions

**Memoization with `useCallback` for Stable Functions:**

Ensure functions are not re-created on every render by using `useCallback`.

**Example Code:**

```tsx
// Component demonstrating useCallback
const MemoizedComponent: React.FC = () => {
  const [count, setCount] = useState(0);

  // Stable function using useCallback
  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []); // Empty dependency array means this function will not change

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment Count</button>
    </div>
  );
};
```

**Comments in Code:**

- **`useCallback`**: Memoizes the `increment` function, avoiding unnecessary re-creations.
- **Dependency Array**: An empty array means `increment` function remains stable across renders.
- **`setCount`**: Updates count when button is clicked, using the stable function reference.



## Code Splitting and Lazy Loading
### 1. React.lazy and Suspense

**Code Splitting and Lazy Loading with React.lazy and Suspense:**

Optimize performance by splitting code into chunks and loading components lazily.

**Example Code:**

```tsx
import React, { Suspense, lazy } from 'react';

// Lazy load the component
const LazyComponent = lazy(() => import('./LazyComponent'));

// Example component with lazy loading
const App: React.FC = () => {
  return (
    <div>
      <h1>Code Splitting with React.lazy</h1>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent /> {/* Render lazy-loaded component */}
      </Suspense>
    </div>
  );
};

export default App;
```

**Comments in Code:**

- **`lazy`**: Dynamically imports the component, splitting code.
- **`Suspense`**: Handles the loading state while the component is being fetched.
- **`fallback`**: Displays a loading indicator during the component load.

### 2. Dynamic Imports

**Code Splitting and Lazy Loading with Dynamic Imports:**

Load components only when needed to improve performance using dynamic imports.

**Example Code:**

```tsx
import React, { Suspense, useState } from 'react';

// Dynamically import the component
const DynamicComponent = React.lazy(() => import('./DynamicComponent'));

const App: React.FC = () => {
  const [show, setShow] = useState(false);

  return (
    <div>
      <button onClick={() => setShow(true)}>Load Component</button>
      {show && (
        <Suspense fallback={<div>Loading...</div>}>
          <DynamicComponent /> {/* Render dynamically imported component */}
        </Suspense>
      )}
    </div>
  );
};
```

**Comments in Code:**

- **`React.lazy`**: Performs dynamic import, enabling code splitting.
- **`Suspense`**: Manages loading state while the component is loaded.
- **`fallback`**: Displays a loading message while fetching the component.



## Performance Profiling
### 1. Using React Profiler

**Performance Profiling with React Profiler:**

Use React Profiler to measure performance of React components and identify bottlenecks.

**Example Code:**

```tsx
import React, { Profiler } from 'react';

// Profiler component to measure performance
const App: React.FC = () => {
  const onRenderCallback = (
    id: string, // The Profiler ID
    phase: "mount" | "update", // The phase of the lifecycle
    actualDuration: number, // Actual render time
    baseDuration: number, // Base render time without optimizations
    startTime: number, // When rendering started
    commitTime: number // When rendering committed
  ) => {
    console.log(`Component ${id} rendered during ${phase} phase.`);
    console.log(`Actual duration: ${actualDuration}ms`);
  };

  return (
    <Profiler id="App" onRender={onRenderCallback}>
      <div>
        {/* Other components */}
        <p>Profiling React Performance</p>
      </div>
    </Profiler>
  );
};
```

**Comments in Code:**

- **`Profiler`**: Wraps components to measure rendering performance.
- **`onRenderCallback`**: Callback function provides performance metrics.
- **Performance Metrics**: Includes `actualDuration`, `baseDuration`, `startTime`, and `commitTime` for detailed analysis.

### 2. Identifying Bottlenecks

**Identifying Performance Bottlenecks with React Profiler:**

Use React Profiler to identify performance issues by analyzing component render times and frequencies.

**Example Code:**

```tsx
import React, { Profiler } from 'react';

// Example component with performance profiling
const ExampleComponent: React.FC = () => {
  const onRenderCallback = (
    id: string, // Profiler ID
    phase: "mount" | "update", // Lifecycle phase
    actualDuration: number, // Render duration
    baseDuration: number, // Base render time
    startTime: number, // Start time of render
    commitTime: number // Commit time of render
  ) => {
    console.log(`Component ${id} ${phase} phase took ${actualDuration}ms`);
    console.log(`Base duration: ${baseDuration}ms`);
  };

  return (
    <Profiler id="ExampleComponent" onRender={onRenderCallback}>
      <div>
        {/* Your component's content */}
        <p>Profiling for performance bottlenecks</p>
      </div>
    </Profiler>
  );
};
```

**Comments in Code:**

- **`Profiler`**: Wraps the component to capture performance data.
- **`onRenderCallback`**: Logs performance metrics to identify bottlenecks.
- **Metrics**: `actualDuration`, `baseDuration`, `startTime`, and `commitTime` help analyze performance issues.

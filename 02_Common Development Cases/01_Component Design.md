# Component Design

## Creating Reusable Components
### 1. Custom Hooks

Custom hooks in React allow you to extract and reuse stateful logic across multiple components, enhancing code reusability.

**Demo TypeScript Code:**

1. **Creating a Custom Hook:**
   ```tsx
   // useCounter.ts
   
   // Custom hook for counter logic
   const useCounter = (initialCount: number = 0) => {
     const [count, setCount] = useState<number>(initialCount);
   
     const increment = () => setCount(count + 1);
     const decrement = () => setCount(count - 1);
   
     return { count, increment, decrement };
   };
   ```

2. **Using the Custom Hook:**
   ```tsx
   // CounterComponent.tsx
   const CounterComponent: React.FC = () => {
     const { count, increment, decrement } = useCounter(10);
   
     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={increment}>Increment</button>
         <button onClick={decrement}>Decrement</button>
       </div>
     );
   };
   ```
   
3. **Best Practices:**
   - **Encapsulate Logic**: Extract and reuse stateful logic with custom hooks.
   - **Keep Hooks Simple**: Ensure hooks are focused on a single piece of functionality.

**Comments in Code:**

- **Custom Hook**: Encapsulates and returns reusable logic.
- **Usage**: Custom hook logic is used in components.

Custom hooks enhance component reusability by sharing stateful logic efficiently.

### 2. Higher-Order Components (HOCs)

Higher-Order Components (HOCs) in React are functions that take a component and return a new component with additional props or logic, enabling reuse and composition.

**Demo TypeScript Code:**

1. **Creating an HOC:**
   ```tsx
   // withLogger.tsx
   import React from 'react';
   
   // HOC that logs props
   const withLogger = <P extends object>(WrappedComponent: React.ComponentType<P>) => {
     return (props: P) => {
       console.log('Props:', props);
       return <WrappedComponent {...props} />;
     };
   };
   
   export default withLogger;
   ```

2. **Using the HOC:**
   ```tsx
   // Hello.tsx
   import React from 'react';
   
   interface HelloProps {
     name: string;
   }
   
   const Hello: React.FC<HelloProps> = ({ name }) => {
     return <p>Hello, {name}!</p>;
   };
   
   export default Hello;
   ```

   ```tsx
   // App.tsx
   import React from 'react';
   import Hello from './Hello';
   import withLogger from './withLogger';
   
   // Enhanced component with logging
   const HelloWithLogger = withLogger(Hello);
   
   const App: React.FC = () => {
     return (
       <div>
         <HelloWithLogger name="World" />
       </div>
     );
   };
   
   export default App;
   ```

3. **Best Practices:**
   - **Reuse Logic**: Enhance components with shared functionality.
   - **Preserve Props**: Pass all props through to the wrapped component.

**Comments in Code:**
- **HOC**: Adds functionality (logging) to the wrapped component.
- **Usage**: Wraps component to enhance its behavior.

HOCs allow for reusing component logic and adding additional functionality without modifying the original component.



## Component Composition
### 1. Using Children Props

Component composition in React allows components to be nested and combined, often using the `children` prop to pass and render nested elements.

**Demo TypeScript Code:**

1. **Creating a Component with Children:**
   ```tsx
   // Card.tsx
   
   // Card component that uses children prop
   interface CardProps {
     title: string;
     children: ReactNode; // Accepts any valid React node
   }
   
   const Card: React.FC<CardProps> = ({ title, children }) => {
     return (
       <div className="card">
         <h2>{title}</h2>
         <div className="card-content">
           {children} {/* Render nested content */}
         </div>
       </div>
     );
   };
   ```

2. **Using the Component with Children:**
   ```tsx
   // App.tsx
   const App: React.FC = () => {
     return (
       <div>
         <Card title="My Card">
           <p>This is some content inside the card.</p>
           <button>Click Me</button>
         </Card>
       </div>
     );
   };
   ```
   
3. **Best Practices:**
   - **Flexible Composition**: Allow components to accept and render nested content.
   - **Reusable Layout**: Create reusable layout components with customizable content.

**Comments in Code:**
- **Children Prop**: Allows passing and rendering nested elements within `Card`.
- **Usage**: Enables flexible and reusable component composition.

Using the `children` prop facilitates component composition, letting you build flexible and reusable layouts.

### 2. Composition vs Inheritance

Component composition in React uses nesting to combine components, while inheritance is less common in React. Composition promotes flexibility and reuse by passing components as children, unlike inheritance which relies on class hierarchies.

**Demo TypeScript Code:**

1. **Component Composition:**
   ```tsx
   // Button.tsx
   interface ButtonProps {
     children: ReactNode; // Content inside button
     onClick: () => void;
   }
   
   const Button: React.FC<ButtonProps> = ({ children, onClick }) => {
     return <button onClick={onClick}>{children}</button>;
   };
   ```
   
   ```tsx
   // App.tsx
   const App: React.FC = () => {
     return (
       <div>
         <Button onClick={() => alert('Clicked!')}>
           Click Me
         </Button>
       </div>
     );
   };
   ```
   
2. **Component Inheritance (Not Recommended in React):**
   React prefers composition over inheritance. However, you might use inheritance for extending functionality in some cases, but it is not idiomatic in React.

**Comments in Code:**
- **Composition**: `Button` component receives content through `children` and renders it.
- **Flexibility**: Allows creating complex UIs with nested components.

Composition enables flexible component structures, whereas inheritance is not typical for React, focusing instead on creating reusable, composable components.



## Managing Component State
### 1. Local State

Managing local state in React involves using the `useState` hook within a component to track and update state values.

**Demo TypeScript Code:**

```tsx
// Counter.tsx
const Counter: React.FC = () => {
  // Initialize state with useState
  const [count, setCount] = useState<number>(0);

  // Handler to update state
  const increment = () => setCount(count + 1);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
};
```

**Comments in Code:**
- **useState Hook**: Initializes local state with a default value (`0`).
- **State Management**: `setCount` updates the state and triggers re-render.

Local state in React components allows managing and updating data specific to that component, enhancing interactivity and responsiveness.

### 2. Derived State

Derived state in React involves computing state values based on other state variables, avoiding redundant storage and ensuring consistency.

**Demo TypeScript Code:**

```tsx
// TemperatureConverter.tsx
const TemperatureConverter: React.FC = () => {
  // State for Celsius temperature
  const [celsius, setCelsius] = useState<number>(0);

  // Derived state: Fahrenheit temperature based on Celsius
  const fahrenheit = celsius * 9 / 5 + 32;

  return (
    <div>
      <label>
        Celsius:
        <input
          type="number"
          value={celsius}
          onChange={(e) => setCelsius(Number(e.target.value))}
        />
      </label>
      <p>Fahrenheit: {fahrenheit.toFixed(2)}</p>
    </div>
  );
};
```

**Comments in Code:**
- **Derived State**: `fahrenheit` is calculated from `celsius`, ensuring updated value based on `celsius`.
- **State Calculation**: Avoids storing redundant state by computing derived values directly.

Derived state reduces redundancy by computing values from existing state, ensuring consistency and reducing the complexity of state management.



## Handling Component Lifecycle
### 1. useEffect for Side Effects

`useEffect` in React manages side effects like data fetching or subscriptions, running after render or state updates.

**Demo TypeScript Code:**

```tsx
// DataFetcher.tsx
const DataFetcher: React.FC = () => {
  const [data, setData] = useState<string | null>(null);
  const [loading, setLoading] = useState<boolean>(true);

  useEffect(() => {
    // Fetch data on component mount
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        setData(result.message);
      } catch (error) {
        console.error('Error fetching data:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []); // Empty dependency array means this runs once after initial render

  if (loading) return <p>Loading...</p>;

  return <div>Data: {data}</div>;
};
```

**Comments in Code:**
- **`useEffect` Hook**: Runs `fetchData` after the component mounts.
- **Side Effect Management**: Handles asynchronous data fetching and updates state.

`useEffect` helps manage side effects by performing operations like data fetching after rendering, ensuring components react to changes properly.

### 2. useLayoutEffect for Layout Changes

`useLayoutEffect` runs synchronously after all DOM mutations, ensuring layout updates before browser painting.

**Demo TypeScript Code:**

```tsx
// LayoutEffectExample.tsx
const LayoutEffectExample: React.FC = () => {
  const [size, setSize] = useState<number>(0);
  const ref = useRef<HTMLDivElement>(null);

  useLayoutEffect(() => {
    // Measure the element's size after DOM update
    if (ref.current) {
      setSize(ref.current.getBoundingClientRect().width);
    }
  }, []); // Empty dependency array ensures this runs after initial render

  return (
    <div>
      <div ref={ref} style={{ width: '100%', height: '100px', backgroundColor: 'lightblue' }}>
        Resize me!
      </div>
      <p>Width: {size}px</p>
    </div>
  );
};
```

**Comments in Code:**
- **`useLayoutEffect` Hook**: Measures and updates layout after DOM changes but before painting.
- **Layout Adjustment**: Ensures accurate size calculations by accessing the DOM directly.

`useLayoutEffect` helps with measuring and reacting to layout changes before the browser paints, ensuring precise layout adjustments and avoiding visual inconsistencies.
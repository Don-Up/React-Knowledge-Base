# State Management

## Local State Management
### Using useState

`useState` manages local state in functional components, allowing state variables and their setters.

**Demo TypeScript Code:**

```tsx
// Counter.tsx
import React, { useState } from 'react';

const Counter: React.FC = () => {
  // Declare a state variable and its setter function
  const [count, setCount] = useState<number>(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
    </div>
  );
};

export default Counter;
```

**Comments in Code:**
- **`useState` Hook**: Manages `count` state with initial value `0`.
- **State Management**: `setCount` updates the state on button clicks.

`useState` provides a way to manage and update local component state in functional components, reacting to user interactions and ensuring state persistence between renders.



## Context API
### 1. Creating Context

`Context API` provides a way to pass state through the component tree without props drilling.

**Demo TypeScript Code:**

**ThemeContext**: A context object created with `createContext` to hold the current theme and a function to change it. It's initialized with `undefined`, indicating that it's not provided by default.

**ThemeProvider**: A component that uses `useState` to manage the current theme (`'light'` by default). It provides the theme and `setTheme` function to its children via `ThemeContext.Provider`.

**useTheme**: A custom hook that accesses the context value. It throws an error if used outside of a `ThemeProvider`, ensuring that the context is available.

```tsx
// ThemeContext.tsx
import React, { createContext, useContext, useState, ReactNode } from 'react';

// Create a context with default values
const ThemeContext = createContext<{ theme: string; setTheme: (theme: string) => void } | undefined>(undefined);

const ThemeProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [theme, setTheme] = useState<string>('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
};

export { ThemeProvider, useTheme };
```

**Comments in Code:**

- **`createContext`**: Initializes `ThemeContext` with default values.
- **`ThemeProvider` Component**: Provides state and updater function to the context.
- **`useTheme` Hook**: Custom hook to access context values.

`Context API` helps manage global state or settings, avoiding prop drilling by providing context to nested components.

### 2. Providing and Consuming Context

`Context API` allows sharing state across components without passing props manually.

**Demo TypeScript Code:**

```tsx
// App.tsx

// Component that consumes context
const ThemeConsumer: React.FC = () => {
  const { theme, setTheme } = useTheme();

  return (
    <div>
      <p>Current Theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
};

// Main App component
const App: React.FC = () => {
  return (
    <ThemeProvider>
      <ThemeConsumer />
    </ThemeProvider>
  );
};
```

**Comments in Code:**
- **`ThemeProvider`**: Wraps the `ThemeConsumer` to provide context.
- **`useTheme`**: Custom hook to access context values.
- **`ThemeConsumer`**: Consumes and updates the theme from context.

`Context API` simplifies state management by providing and consuming context, allowing components to access shared state directly.



## State Management Libraries
### 1. Redux (Actions, Reducers, Store)

`Redux` manages global state with actions, reducers, and a store, ensuring predictable state changes.

**Demo TypeScript Code:**

```tsx
// actions.ts
export const increment = () => ({ type: 'INCREMENT' } as const);
export const decrement = () => ({ type: 'DECREMENT' } as const);

type Action = ReturnType<typeof increment> | ReturnType<typeof decrement>;

// reducer.ts
const initialState = { count: 0 };

const counterReducer = (state = initialState, action: Action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
};

// store.ts
const store = createStore(counterReducer);

// App.tsx
const Counter: React.FC = () => {
  const dispatch = useDispatch();
  const count = useSelector((state: { count: number }) => state.count);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
    </div>
  );
};

const App: React.FC = () => (
  <Provider store={store}>
    <Counter />
  </Provider>
);
```

**Comments in Code:**
- **`actions.ts`**: Defines action creators.
- **`reducer.ts`**: Manages state updates based on actions.
- **`store.ts`**: Creates Redux store with the reducer.
- **`App.tsx`**: Connects components to the store using `Provider`, `useDispatch`, and `useSelector`.

`Redux` streamlines global state management by clearly defining actions, reducers, and a centralized store.

### 2. MobX (Observables, Actions, Stores)

`MobX` provides reactive state management with observables, actions, and stores, ensuring automatic updates to UI components.

**Demo TypeScript Code:**

```tsx
// store.ts
import { makeAutoObservable } from 'mobx';

// Store definition
class CounterStore {
  count = 0;

  constructor() {
    makeAutoObservable(this);
  }

  increment() {
    this.count += 1;
  }

  decrement() {
    this.count -= 1;
  }
}

const counterStore = new CounterStore();
export default counterStore;

// App.tsx
import React from 'react';
import { observer } from 'mobx-react-lite';
import counterStore from './store';

// Observer component
const Counter: React.FC = observer(() => {
  return (
    <div>
      <p>Count: {counterStore.count}</p>
      <button onClick={() => counterStore.increment()}>Increment</button>
      <button onClick={() => counterStore.decrement()}>Decrement</button>
    </div>
  );
});

const App: React.FC = () => <Counter />;
```

**Comments in Code:**
- **`store.ts`**: Defines `CounterStore` with observable state and actions using `makeAutoObservable`.
- **`App.tsx`**: `Counter` component observes the store and automatically updates on state changes.

`MobX` enables seamless state management with reactive observables, allowing automatic updates and simplified state handling.



## Handling Complex State Logic
### 1. useReducer for Complex State

`useReducer` manages complex state logic by using a reducer function and dispatching actions to update the state.

**Demo TypeScript Code:**

```tsx
// reducer.ts
type State = { count: number; text: string };
type Action =
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'setText'; payload: string };

const initialState: State = { count: 0, text: '' };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + 1 };
    case 'decrement':
      return { ...state, count: state.count - 1 };
    case 'setText':
      return { ...state, text: action.payload };
    default:
      return state;
  }
}

export { initialState, reducer, Action };

// App.tsx
const App: React.FC = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <input
        type="text"
        value={state.text}
        onChange={(e) =>
          dispatch({ type: 'setText', payload: e.target.value })
        }
      />
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
};
```

**Comments in Code:**
- **`reducer.ts`**: Defines state, actions, and the reducer function to handle state updates.
- **`App.tsx`**: Uses `useReducer` to manage complex state logic for both count and text inputs.

`useReducer` simplifies handling complex state logic and action management, making state updates more predictable and maintainable.

### 2. Custom Reducers

Custom reducers in React manage complex state logic by defining a reducer function that updates state based on actions.

**Demo TypeScript Code:**

```tsx
// customReducer.ts
type State = { count: number; text: string };
type Action =
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'setText'; payload: string }
  | { type: 'reset' };

const initialState: State = { count: 0, text: '' };

function customReducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + 1 };
    case 'decrement':
      return { ...state, count: state.count - 1 };
    case 'setText':
      return { ...state, text: action.payload };
    case 'reset':
      return initialState;
    default:
      return state;
  }
}

export { initialState, customReducer, Action };

// App.tsx
const App: React.FC = () => {
  const [state, dispatch] = useReducer(customReducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <input
        type="text"
        value={state.text}
        onChange={(e) =>
          dispatch({ type: 'setText', payload: e.target.value })
        }
      />
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
};
```

**Comments in Code:**
- **`customReducer.ts`**: Defines `customReducer` to handle various state actions including a reset action.
- **`App.tsx`**: Uses `useReducer` with `customReducer` to manage state with additional complex logic.

Custom reducers provide flexibility for managing complex state logic and action types, keeping state updates consistent and organized.



## State Persistence
### 1. Persisting State in Local Storage

Persisting state in local storage ensures state survives page reloads. Use `useEffect` to synchronize local storage with state.

**Demo TypeScript Code:**

```tsx
// App.tsx

// Hook to load and save state to localStorage
const useLocalStorage = (key: string, initialValue: string) => {
  const [value, setValue] = useState<string>(() => {
    const stored = localStorage.getItem(key);
    return stored ? stored : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, value);
  }, [key, value]);

  return [value, setValue] as const;
};

const App: React.FC = () => {
  // Use custom hook for state persistence
  const [text, setText] = useLocalStorage('textKey', '');

  return (
    <div>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something..."
      />
      <p>Stored Text: {text}</p>
    </div>
  );
};
```

**Comments in Code:**
- **`useLocalStorage` Hook**: Initializes state from local storage and updates local storage on state change.
- **`App.tsx`**: Demonstrates using `useLocalStorage` to persist text input between page reloads.

This approach ensures that user data is maintained across page reloads, enhancing user experience.

### 2. Syncing State with Backend

Syncing state with a backend ensures data consistency across sessions. Use `useEffect` for fetching and updating backend data.

**Demo TypeScript Code:**

```tsx
// App.tsx

// URL of the backend API
const API_URL = 'https://api.example.com/data';

const App: React.FC = () => {
  const [data, setData] = useState<string>('');

  // Fetch data from the backend on mount
  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get(API_URL);
        setData(response.data);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };
    fetchData();
  }, []);

  // Save data to the backend on change
  const handleChange = async (event: React.ChangeEvent<HTMLInputElement>) => {
    const newData = event.target.value;
    setData(newData);
    try {
      await axios.post(API_URL, { data: newData });
    } catch (error) {
      console.error('Error saving data:', error);
    }
  };

  return (
    <div>
      <input
        type="text"
        value={data}
        onChange={handleChange}
        placeholder="Enter data..."
      />
      <p>Data: {data}</p>
    </div>
  );
};
```

**Comments in Code:**
- **`useEffect`**: Fetches initial data from the backend when the component mounts.
- **`handleChange`**: Updates local state and syncs changes with the backend.

This ensures data is consistent with the backend and updated across sessions.
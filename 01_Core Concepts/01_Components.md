# Components

## Functional Components

Functional components are *stateless* React components defined as functions, providing a simpler alternative to class components.

**Demo TypeScript Code:**

1. **Functional Component Example:**
   ```tsx
   // Greeting.tsx
   
   // Props interface
   interface GreetingProps {
     name: string;
   }
   
   // Functional component with props
   const Greeting: React.FC<GreetingProps> = ({ name }) => {
     return <h1>Hello, {name}!</h1>;
   };
   ```

2. **Using the Component:**
   ```tsx
   // App.tsx
   const App: React.FC = () => {
     return (
       <div>
         <Greeting name="Alice" />
         <Greeting name="Bob" />
       </div>
     );
   };
   ```
   
3. **Best Practices:**
   - **Use Props**: Define expected props with TypeScript interfaces.
   - **Keep Simple**: Use functional components for simpler logic.
   - **Avoid State**: For components without state, functional components are preferred.

**Comments in Code:**

- **Props Interface**: Define the shape of component props.
- **Functional Component**: Simplify component structure with function-based components.

Functional components provide a concise, readable way to define UI elements in React.



## Class Components

Class components in React are ES6 classes with lifecycle methods and state management, offering more features compared to functional components.

**Demo TypeScript Code:**

1. **Class Component Example:**
   ```tsx
   // Greeting.tsx
   import React, { Component } from 'react';
   
   // Props and state interfaces
   interface GreetingProps {
     name: string;
   }
   
   interface GreetingState {
     message: string;
   }
   
   // Class component with state
   class Greeting extends Component<GreetingProps, GreetingState> {
     constructor(props: GreetingProps) {
       super(props);
       this.state = {
         message: 'Welcome',
       };
     }
   
     render() {
       return (
         <div>
           <h1>{this.state.message}, {this.props.name}!</h1>
         </div>
       );
     }
   }
   
   export default Greeting;
   ```

2. **Using the Component:**
   ```tsx
   // App.tsx
   import React from 'react';
   import Greeting from './Greeting';
   
   const App: React.FC = () => {
     return (
       <div>
         <Greeting name="Alice" />
         <Greeting name="Bob" />
       </div>
     );
   };
   
   export default App;
   ```

3. **Best Practices:**
   - **Use Lifecycle Methods**: Implement lifecycle hooks as needed.
   - **Manage State**: Use `this.state` for internal component state.
   - **Props and State**: Define interfaces for both props and state.

**Comments in Code:**
- **State Initialization**: Set initial state in the constructor.
- **Lifecycle Methods**: Add methods for component lifecycle events.

Class components are useful for more complex components requiring state management and lifecycle methods.



## Component Lifecycle
### Mounting, Updating, Unmounting

React component lifecycle includes mounting (initialization), updating (prop/state changes), and unmounting (cleanup).

**Demo TypeScript Code:**

1. **Lifecycle Methods Example:**
   ```tsx
   // LifecycleDemo.tsx
   import React, { Component } from 'react';
   
   class LifecycleDemo extends Component {
     // Mounting: Called once when component is inserted into the DOM
     componentDidMount() {
       console.log('Component mounted');
     }
   
     // Updating: Called when component is updated
     componentDidUpdate(prevProps: Readonly<any>, prevState: Readonly<any>) {
       console.log('Component updated');
     }
   
     // Unmounting: Called once when component is removed from the DOM
     componentWillUnmount() {
       console.log('Component unmounted');
     }
   
     render() {
       return <div>Lifecycle Demo</div>;
     }
   }
   
   export default LifecycleDemo;
   ```

2. **Using the Component:**
   ```tsx
   // App.tsx
   import React, { useState } from 'react';
   import LifecycleDemo from './LifecycleDemo';
   
   const App: React.FC = () => {
     const [show, setShow] = useState(true);
   
     return (
       <div>
         <button onClick={() => setShow(!show)}>Toggle Component</button>
         {show && <LifecycleDemo />}
       </div>
     );
   };
   
   export default App;
   ```

3. **Best Practices:**
   - **Mounting**: Initialize state or fetch data.
   - **Updating**: Respond to prop/state changes.
   - **Unmounting**: Clean up resources like timers or subscriptions.

**Comments in Code:**

- **componentDidMount**: Runs after initial render.
- **componentDidUpdate**: Runs after prop or state changes.
- **componentWillUnmount**: Cleans up before removal.

Understanding lifecycle methods helps manage component behavior at different stages.



## Props and State
### Passing Props

Props are used to pass data from parent to child components in React. They are immutable within the child component.

**Demo TypeScript Code:**

1. **Passing Props Example:**
   ```tsx
   // Greeting.tsx
   
   // Props interface
   interface GreetingProps {
     name: string;
   }
   
   // Functional component receiving props
   const Greeting: React.FC<GreetingProps> = ({ name }) => {
     return <h1>Hello, {name}!</h1>;
   };
   ```

2. **Using the Component:**
   ```tsx
   // App.tsx
   const App: React.FC = () => {
     return (
       <div>
         <Greeting name="Alice" />
         <Greeting name="Bob" />
       </div>
     );
   };
   ```
   
3. **Best Practices:**
   - **Define Props Interface**: Clearly define the structure of props.
   - **Use Props in Child Components**: Access and render props in child components.

**Comments in Code:**
- **Props Interface**: Define expected props using TypeScript interfaces.
- **Passing Props**: Provide data to components through props.

Props allow components to be reusable and flexible by passing data dynamically.

### Managing State

State in React is used to manage data that changes over time, affecting the component's behavior and rendering.

**Demo TypeScript Code:**

1. **Managing State Example:**
   ```tsx
   // Counter.tsx
   const Counter: React.FC = () => {
     // State initialization
     const [count, setCount] = useState<number>(0);
   
     // Event handler to update state
     const increment = () => setCount(count + 1);
   
     return (
       <div>
         <p>Count: {count}</p>
         <button onClick={increment}>Increment</button>
       </div>
     );
   };
   ```
   
2. **Using the Component:**
   ```tsx
   // App.tsx
   const App: React.FC = () => {
     return (
       <div>
         <Counter />
       </div>
     );
   };
   ```
   
3. **Best Practices:**
   - **Initialize State**: Use `useState` to set initial state.
   - **Update State**: Use state updater functions like `setCount`.

**Comments in Code:**
- **useState Hook**: Initialize and manage state with `useState`.
- **State Updates**: Modify state using updater functions.

State management allows React components to maintain and update data dynamically.

### Controlled vs Uncontrolled Components

Controlled components manage form data through React state, while uncontrolled components handle form data through the DOM.

**Demo TypeScript Code:**

1. **Controlled Component Example:**
   ```tsx
   // ControlledForm.tsx
   import React, { useState } from 'react';
   
   const ControlledForm: React.FC = () => {
     const [value, setValue] = useState<string>('');
   
     // Handle input change
     const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
       setValue(e.target.value);
     };
   
     return (
       <form>
         <label>
           Name:
           <input type="text" value={value} onChange={handleChange} />
         </label>
       </form>
     );
   };
   
   export default ControlledForm;
   ```

2. **Uncontrolled Component Example:**
   ```tsx
   // UncontrolledForm.tsx
   import React, { useRef } from 'react';
   
   const UncontrolledForm: React.FC = () => {
     const inputRef = useRef<HTMLInputElement>(null);
   
     // Handle form submission
     const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
       e.preventDefault();
       alert('A name was submitted: ' + (inputRef.current?.value || ''));
     };
   
     return (
       <form onSubmit={handleSubmit}>
         <label>
           Name:
           <input type="text" ref={inputRef} />
         </label>
         <button type="submit">Submit</button>
       </form>
     );
   };
   
   export default UncontrolledForm;
   ```

3. **Best Practices:**
   - **Controlled Components**: Manage form state via React.
   - **Uncontrolled Components**: Use refs to access form values.

**Comments in Code:**
- **Controlled Component**: Value is controlled by React state.
- **Uncontrolled Component**: Value is accessed directly via the DOM.

Controlled components offer better integration with React state, while uncontrolled components use DOM references.
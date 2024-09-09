# Testing

## Unit Testing Components
### 1. Using Jest and React Testing Library

Unit test React components with Jest and React Testing Library to ensure functionality and UI accuracy.

**Example Setup:**

1. **Install Dependencies:**
   
   ```bash
   npm install --save-dev jest @testing-library/react @testing-library/jest-dom
   ```
   
2. **Sample Component:**
   ```tsx
   // components/Button.tsx
   import React from 'react';
   
   interface ButtonProps {
     label: string;
     onClick: () => void;
   }
   
   const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
     <button onClick={onClick}>{label}</button>
   );
   
   export default Button;
   ```

3. **Unit Test:**
   ```tsx
   // __tests__/Button.test.tsx
   import { render, screen, fireEvent } from '@testing-library/react';
   import '@testing-library/jest-dom';
   import Button from '../components/Button';
   
   test('renders button with label and handles click', () => {
     const handleClick = jest.fn();
     render(<Button label="Click me" onClick={handleClick} />);
   
     // Verify button text
     expect(screen.getByText('Click me')).toBeInTheDocument();
   
     // Simulate button click
     fireEvent.click(screen.getByText('Click me'));
     expect(handleClick).toHaveBeenCalledTimes(1);
   });
   ```

**Code Explanation:**
- **Component:** `Button.tsx` displays a button with a label and click handler.
- **Test:** `Button.test.tsx` checks if the button renders correctly and simulates a click event to verify the handler is called.

**Run Tests:**
```bash
npx jest
```

Unit testing with Jest and React Testing Library ensures components work as expected.

### 2. Mocking Component Props and State

Mock props and state to test components in isolation. Use Jest for mocking and React Testing Library for rendering.

**Example Setup:**

1. **Install Dependencies:**
   ```bash
   npm install --save-dev jest @testing-library/react @testing-library/jest-dom
   ```

2. **Sample Component:**
   ```tsx
   // components/UserProfile.tsx
   import React from 'react';
   
   interface UserProfileProps {
     user: { name: string; age: number };
   }
   
   const UserProfile: React.FC<UserProfileProps> = ({ user }) => (
     <div>
       <h1>{user.name}</h1>
       <p>Age: {user.age}</p>
     </div>
   );
   
   export default UserProfile;
   ```

3. **Unit Test with Mock Props:**
   ```tsx
   // __tests__/UserProfile.test.tsx
   import { render, screen } from '@testing-library/react';
   import '@testing-library/jest-dom';
   import UserProfile from '../components/UserProfile';
   
   test('renders user profile with mock data', () => {
     // Mock user data
     const mockUser = { name: 'John Doe', age: 30 };
   
     // Render component with mock props
     render(<UserProfile user={mockUser} />);
   
     // Verify rendered output
     expect(screen.getByText('John Doe')).toBeInTheDocument();
     expect(screen.getByText('Age: 30')).toBeInTheDocument();
   });
   ```

**Code Explanation:**
- **Component:** `UserProfile.tsx` displays user name and age.
- **Test:** `UserProfile.test.tsx` uses mock data to test if component renders user information correctly.

**Run Tests:**
```bash
npx jest
```

Mocking props allows isolated testing of components with predefined data.



## Integration Testing
### 1. Testing Component Interactions

Test interactions between components to ensure they work together as expected. Use Jest and React Testing Library for integration tests.

**Example Setup:**

1. **Install Dependencies:**
   ```bash
   npm install --save-dev jest @testing-library/react @testing-library/jest-dom
   ```

2. **Sample Components:**

   ```tsx
   // components/Counter.tsx
   import React, { useState } from 'react';

   const Counter: React.FC = () => {
     const [count, setCount] = useState(0);
     return (
       <div>
         <button onClick={() => setCount(count + 1)}>Increment</button>
         <p>Count: {count}</p>
       </div>
     );
   };

   export default Counter;
   ```

   ```tsx
   // components/App.tsx
   import React from 'react';
   import Counter from './Counter';

   const App: React.FC = () => (
     <div>
       <h1>App Component</h1>
       <Counter />
     </div>
   );

   export default App;
   ```

3. **Integration Test:**
   ```tsx
   // __tests__/App.test.tsx
   import { render, screen, fireEvent } from '@testing-library/react';
   import '@testing-library/jest-dom';
   import App from '../components/App';
   
   test('increments count on button click', () => {
     // Render App component
     render(<App />);
   
     // Find and click button
     const button = screen.getByText('Increment');
     fireEvent.click(button);
   
     // Check if count is incremented
     expect(screen.getByText('Count: 1')).toBeInTheDocument();
   });
   ```

**Code Explanation:**
- **Components:** `Counter.tsx` manages a count state, `App.tsx` includes `Counter`.
- **Test:** `App.test.tsx` checks if clicking the button in `Counter` updates the displayed count.

**Run Tests:**
```bash
npx jest
```

Integration tests verify that components interact correctly and state changes as expected.

### 2. Testing Hooks and Context

Test React hooks and context to ensure they work correctly within components. Use Jest and React Testing Library.

**Example Setup:**

1. **Install Dependencies:**
   ```bash
   npm install --save-dev jest @testing-library/react @testing-library/jest-dom
   ```

2. **Sample Hook and Context:**

   ```tsx
   // context/ThemeContext.tsx
   import React, { createContext, useState, ReactNode, useContext } from 'react';

   interface ThemeContextType {
     theme: string;
     setTheme: (theme: string) => void;
   }

   const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

   export const ThemeProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
     const [theme, setTheme] = useState('light');
     return (
       <ThemeContext.Provider value={{ theme, setTheme }}>
         {children}
       </ThemeContext.Provider>
     );
   };

   export const useTheme = () => {
     const context = useContext(ThemeContext);
     if (context === undefined) {
       throw new Error('useTheme must be used within a ThemeProvider');
     }
     return context;
   };
   ```

   ```tsx
   // components/ThemeSwitcher.tsx
   import React from 'react';
   import { useTheme } from '../context/ThemeContext';

   const ThemeSwitcher: React.FC = () => {
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

   export default ThemeSwitcher;
   ```

3. **Integration Test:**

   ```tsx
   // __tests__/ThemeSwitcher.test.tsx
   import { render, screen, fireEvent } from '@testing-library/react';
   import '@testing-library/jest-dom';
   import { ThemeProvider } from '../context/ThemeContext';
   import ThemeSwitcher from '../components/ThemeSwitcher';
   
   test('toggles theme on button click', () => {
     // Render ThemeSwitcher within ThemeProvider
     render(
       <ThemeProvider>
         <ThemeSwitcher />
       </ThemeProvider>
     );
   
     // Check initial theme
     expect(screen.getByText('Current Theme: light')).toBeInTheDocument();
   
     // Click button to toggle theme
     fireEvent.click(screen.getByText('Toggle Theme'));
   
     // Check updated theme
     expect(screen.getByText('Current Theme: dark')).toBeInTheDocument();
   });
   ```

**Code Explanation:**
- **Context/Hook:** `ThemeContext.tsx` provides a `useTheme` hook for theme management.
- **Component:** `ThemeSwitcher.tsx` uses `useTheme` to toggle themes.
- **Test:** `ThemeSwitcher.test.tsx` verifies theme toggling functionality.

**Run Tests:**
```bash
npx jest
```

Integration tests ensure hooks and context work as expected when used in components.



## Snapshot Testing
### Creating and Updating Snapshots

Snapshot testing ensures UI components render consistently. Jest and React Testing Library are used to create and update snapshots.

**Example Setup:**

1. **Install Dependencies:**
   ```bash
   npm install --save-dev jest @testing-library/react @testing-library/jest-dom
   ```

2. **Sample Component:**

   ```tsx
   // components/Button.tsx
   import React from 'react';

   interface ButtonProps {
     label: string;
   }

   const Button: React.FC<ButtonProps> = ({ label }) => {
     return <button>{label}</button>;
   };

   export default Button;
   ```

3. **Snapshot Test:**

   ```tsx
   // __tests__/Button.test.tsx
   import { render } from '@testing-library/react';
   import Button from '../components/Button';
   import { create } from 'react-test-renderer';
   
   test('Button component renders correctly', () => {
     // Create a snapshot
     const tree = create(<Button label="Click Me" />).toJSON();
     expect(tree).toMatchSnapshot();
   });
   ```

**Code Explanation:**
- **Component:** `Button.tsx` is a simple button component.
- **Test:** `Button.test.tsx` creates a snapshot of the button component and checks it against the saved snapshot.

**Run Tests:**
```bash
npx jest
```

**Updating Snapshots:**
```bash
npx jest --updateSnapshot
```

Snapshot testing helps ensure UI consistency by capturing and comparing component output.



## Testing Asynchronous Operations
### Handling Async Logic in Tests

Testing asynchronous operations involves handling async functions or data fetches within React components. Use `async`/`await` in your tests to wait for state updates or promises.

**Example Setup:**

1. **Install Dependencies:**
   ```bash
   npm install --save-dev jest @testing-library/react @testing-library/jest-dom
   ```

2. **Sample Component:**

   ```tsx
   // components/FetchData.tsx
   import React, { useEffect, useState } from 'react';

   const FetchData: React.FC = () => {
     const [data, setData] = useState<string | null>(null);

     useEffect(() => {
       const fetchData = async () => {
         const response = await fetch('/api/data');
         const result = await response.json();
         setData(result.message);
       };
       fetchData();
     }, []);

     return <div>{data ? data : 'Loading...'}</div>;
   };

   export default FetchData;
   ```

3. **Asynchronous Test:**

   ```tsx
   // __tests__/FetchData.test.tsx
   import { render, screen, waitFor } from '@testing-library/react';
   import FetchData from '../components/FetchData';
   
   test('FetchData component fetches and displays data', async () => {
     // Mocking fetch
     global.fetch = jest.fn(() =>
       Promise.resolve({
         json: () => Promise.resolve({ message: 'Hello, World!' }),
       })
     ) as jest.Mock;
   
     render(<FetchData />);
   
     // Wait for async operation to complete
     await waitFor(() => expect(screen.getByText('Hello, World!')).toBeInTheDocument());
   });
   ```

**Code Explanation:**
- **Component:** `FetchData.tsx` fetches data from an API and displays it.
- **Test:** `FetchData.test.tsx` mocks the `fetch` function, renders the component, and uses `waitFor` to handle the async data update.

**Run Tests:**
```bash
npx jest
```

This ensures that async operations are properly tested by mocking API calls and waiting for state updates.



## End-to-End Testing
### Using Cypress or Puppeteer

End-to-end testing with Cypress involves simulating real user interactions and verifying the application's behavior. It provides a robust way to test the full functionality of a React application.

**Example Setup:**

1. **Install Cypress:**
   ```bash
   npm install --save-dev cypress
   ```

2. **Sample Test:**

   ```tsx
   // cypress/integration/sample.spec.ts
   describe('My React App', () => {
     beforeEach(() => {
       // Visit the application
       cy.visit('http://localhost:3000');
     });
   
     it('should load and display content', () => {
       // Check if the title is visible
       cy.contains('Welcome to My App').should('be.visible');
     });
   
     it('should navigate to about page', () => {
       // Click on a link
       cy.contains('About').click();
       // Verify the new URL
       cy.url().should('include', '/about');
       // Check if the about page content is visible
       cy.contains('About Us').should('be.visible');
     });
   
     it('should handle form submission', () => {
       // Type into a form field
       cy.get('input[name="username"]').type('testuser');
       // Submit the form
       cy.get('form').submit();
       // Verify the result
       cy.contains('Submission Successful').should('be.visible');
     });
   });
   ```

**Code Explanation:**
- **Visit:** `cy.visit` loads the application.
- **Assertions:** `cy.contains` and `cy.url` verify text and URL.
- **Interactions:** `cy.get` and `cy.type` simulate user actions.

**Run Tests:**
```bash
npx cypress open
```

This setup ensures end-to-end functionality by mimicking user interactions and validating the application's behavior.
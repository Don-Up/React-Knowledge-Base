# Routing

## Setting Up Routing
### 1. Using React Router 6

**React Routing with React Router 6:**

Set up routing in React applications using React Router 6. Define routes and navigate between them.

**Example Code:**

```tsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';

// Components for routes
const Home: React.FC = () => <h2>Home Page</h2>;
const About: React.FC = () => <h2>About Page</h2>;

const App: React.FC = () => (
  <Router>
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
    </nav>
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
    </Routes>
  </Router>
);

export default App;
```

**Comments in Code:**
- **Router**: Wraps the application to enable routing.
- **Routes**: Defines route paths and their corresponding components.
- **Link**: Provides navigation between routes.

### 2. Defining Routes and Paths

**Defining Routes and Paths with React Router 6:**

Set up and configure routes with specific paths and components using React Router 6.

**Example Code:**

```tsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

// Component definitions
const Home: React.FC = () => <h2>Home Page</h2>;
const About: React.FC = () => <h2>About Page</h2>;
const NotFound: React.FC = () => <h2>404 Not Found</h2>;

const App: React.FC = () => (
  <Router>
    <Routes>
      {/* Define route for Home */}
      <Route path="/" element={<Home />} />
      {/* Define route for About */}
      <Route path="/about" element={<About />} />
      {/* Catch-all route for 404 errors */}
      <Route path="*" element={<NotFound />} />
    </Routes>
  </Router>
);

export default App;
```

**Comments in Code:**
- **`<Routes>`**: Contains all route definitions.
- **`<Route path="..." element={<Component />} />`**: Maps a URL path to a component.
- **`path="*"`**: Catches all undefined routes for 404 handling.



## Nested Routes
### 1. Configuring Nested Routes

**Configuring Nested Routes with React Router 6:**

Set up nested routes to render child components within parent routes.

**Example Code:**

```tsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Outlet } from 'react-router-dom';

// Parent component with nested routes
const Dashboard: React.FC = () => (
  <div>
    <h2>Dashboard</h2>
    <Outlet /> {/* Render nested routes here */}
  </div>
);

// Child components
const Overview: React.FC = () => <h3>Overview</h3>;
const Settings: React.FC = () => <h3>Settings</h3>;

const App: React.FC = () => (
  <Router>
    <Routes>
      {/* Parent route */}
      <Route path="dashboard" element={<Dashboard />}>
        {/* Nested routes */}
        <Route path="overview" element={<Overview />} />
        <Route path="settings" element={<Settings />} />
      </Route>
      {/* Fallback route */}
      <Route path="*" element={<h2>404 Not Found</h2>} />
    </Routes>
  </Router>
);

export default App;
```

**Comments in Code:**
- **`<Outlet />`**: Placeholder for nested routes inside the parent component.
- **`<Route path="dashboard" element={<Dashboard />}>`**: Defines parent route.
- **`<Route path="overview" element={<Overview />} />`**: Defines nested routes.

### 2. Rendering Nested Components

**Rendering Nested Components with React Router 6:**

Use nested routes to display child components within a parent component.

**Example Code:**

```tsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Outlet } from 'react-router-dom';

// Parent component with an outlet for nested routes
const Dashboard: React.FC = () => (
  <div>
    <h2>Dashboard</h2>
    <Outlet /> {/* Renders nested components */}
  </div>
);

// Nested components
const Overview: React.FC = () => <div>Overview Content</div>;
const Settings: React.FC = () => <div>Settings Content</div>;

const App: React.FC = () => (
  <Router>
    <Routes>
      {/* Parent route with nested routes */}
      <Route path="dashboard" element={<Dashboard />}>
        <Route path="overview" element={<Overview />} /> {/* Nested route */}
        <Route path="settings" element={<Settings />} /> {/* Nested route */}
      </Route>
      {/* Fallback route */}
      <Route path="*" element={<div>404 Not Found</div>} />
    </Routes>
  </Router>
);

export default App;
```

**Comments in Code:**
- **`<Outlet />`**: Renders nested routes within the `Dashboard` component.
- **`<Route path="dashboard" element={<Dashboard />}>`**: Defines parent route.
- **`<Route path="overview" element={<Overview />} />`**: Defines a nested route for the `Overview` component.
- **`<Route path="settings" element={<Settings />} />`**: Defines a nested route for the `Settings` component.



## Route Guards and Redirection
### 1. Implementing Route Protection

**Implementing Route Protection with React Router 6:**

Use route guards to protect routes and redirect unauthorized users.

**Example Code:**

```tsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Outlet, Navigate } from 'react-router-dom';

// Simulated authentication state
const isAuthenticated = false;

// Protected route component
const ProtectedRoute: React.FC<{ element: React.ReactElement }> = ({ element }) => {
  return isAuthenticated ? element : <Navigate to="/login" />;
};

// Components
const Home: React.FC = () => <div>Home Page</div>;
const Dashboard: React.FC = () => <div>Dashboard (Protected)</div>;
const Login: React.FC = () => <div>Login Page</div>;

const App: React.FC = () => (
  <Router>
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/login" element={<Login />} />
      {/* Protected route */}
      <Route path="/dashboard" element={<ProtectedRoute element={<Dashboard />} />} />
      {/* Fallback route */}
      <Route path="*" element={<div>404 Not Found</div>} />
    </Routes>
  </Router>
);

export default App;
```

**Comments in Code:**
- **`ProtectedRoute`**: Component that checks authentication and redirects if not authenticated.
- **`<Navigate to="/login" />`**: Redirects to login if the user is not authenticated.
- **`element={<ProtectedRoute element={<Dashboard />} />}`**: Protects the `Dashboard` route.

### 2. Redirecting Based on Conditions

**Redirecting Based on Conditions with React Router 6:**

Redirect users based on dynamic conditions such as user roles or data.

**Example Code:**

```tsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';

// Simulated user role
const userRole = 'guest';

// Components
const Home: React.FC = () => <div>Home Page</div>;
const Admin: React.FC = () => <div>Admin Dashboard</div>;
const Login: React.FC = () => <div>Login Page</div>;

// Conditional redirect component
const ConditionalRedirect: React.FC<{ element: React.ReactElement; role: string }> = ({ element, role }) => {
  return role === 'admin' ? element : <Navigate to="/login" />;
};

const App: React.FC = () => (
  <Router>
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/login" element={<Login />} />
      {/* Redirects based on user role */}
      <Route path="/admin" element={<ConditionalRedirect element={<Admin />} role={userRole} />} />
      {/* Fallback route */}
      <Route path="*" element={<div>404 Not Found</div>} />
    </Routes>
  </Router>
);

export default App;
```

**Comments in Code:**
- **`ConditionalRedirect`**: Component that redirects based on user role.
- **`role === 'admin'`**: Checks if the user role is 'admin'.
- **`<Navigate to="/login" />`**: Redirects to login if the user is not an admin.
- **`element={<ConditionalRedirect element={<Admin />} role={userRole} />}`**: Protects the `Admin` route based on role.
# Styling

## Applying Styles to Components
### 1. Inline Styles

Inline styles in React are defined directly within the component using JavaScript objects. They offer a quick way to apply styles without external CSS files.

**Example:**

```tsx
import React from 'react';

const MyComponent: React.FC = () => {
  // Define inline styles
  const style = {
    container: {
      padding: '20px',
      backgroundColor: '#f0f0f0',
    },
    heading: {
      color: 'blue',
      fontSize: '24px',
    },
  };

  return (
    <div style={style.container}>
      <h1 style={style.heading}>Hello, World!</h1>
      <p>This is an example of inline styles in React.</p>
    </div>
  );
};

export default MyComponent;
```

**Code Explanation:**
- **Inline Styles:** Defined in a `style` object.
- **Apply Styles:** Use `style={style.container}` for the `<div>` and `style={style.heading}` for the `<h1>`.

**Benefits:**
- Quick styling for small components.
- Styles are scoped to the component.

**Drawbacks:**
- Limited to JavaScript expressions.
- May become unwieldy for large styles.

### 2. CSS Modules

CSS Modules allow scoped CSS for each component, avoiding global conflicts. Styles are imported as objects and applied directly to elements.

**Example:**

```tsx
// styles.module.css
.container {
  padding: 20px;
  background-color: #f0f0f0;
}

.heading {
  color: blue;
  font-size: 24px;
}

// MyComponent.tsx
import React from 'react';
import styles from './styles.module.css';

const MyComponent: React.FC = () => {
  return (
    <div className={styles.container}>
      <h1 className={styles.heading}>Hello, World!</h1>
      <p>This is an example of CSS Modules in React.</p>
    </div>
  );
};

export default MyComponent;
```

**Code Explanation:**
- **CSS File:** `styles.module.css` defines scoped styles.
- **Import Styles:** `import styles from './styles.module.css'`.
- **Apply Styles:** Use `className={styles.container}` and `className={styles.heading}`.

**Benefits:**
- Scoped styles prevent conflicts.
- Easy to manage styles locally.



## Styling Libraries
### 1. Styled Components

Styled Components use tagged template literals to style components, allowing CSS to be written within JavaScript. Styles are scoped to components.

**Example:**

```tsx
// styles.ts
import styled from 'styled-components';

export const Container = styled.div`
  padding: 20px;
  background-color: #f0f0f0;
`;

export const Heading = styled.h1`
  color: blue;
  font-size: 24px;
`;

// MyComponent.tsx
import React from 'react';
import { Container, Heading } from './styles';

const MyComponent: React.FC = () => {
  return (
    <Container>
      <Heading>Hello, World!</Heading>
      <p>This is an example of Styled Components in React.</p>
    </Container>
  );
};

export default MyComponent;
```

**Code Explanation:**
- **Styled Components:** `Container` and `Heading` are styled using `styled.div` and `styled.h1`.
- **Usage:** Apply styled components directly in JSX.

**Benefits:**
- CSS-in-JS approach.
- Scoped styles with dynamic capabilities.

### 2. Emotion for CSS-in-JS

Emotion is a CSS-in-JS library that allows writing CSS styles with JavaScript. It provides two main ways to style: `css` and `styled` components.

**Example:**

```tsx
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';
import styled from '@emotion/styled';

const containerStyle = css`
  padding: 20px;
  background-color: #f0f0f0;
`;

const Heading = styled.h1`
  color: blue;
  font-size: 24px;
`;

// MyComponent.tsx
import React from 'react';
import { css } from '@emotion/react';
import styled from '@emotion/styled';

const MyComponent: React.FC = () => {
  return (
    <div css={containerStyle}>
      <Heading>Hello, World!</Heading>
      <p>This is an example of Emotion in React.</p>
    </div>
  );
};

export default MyComponent;
```

**Code Explanation:**

- **`css` Function:** Defines styles as a plain JavaScript object.
- **`styled` Function:** Creates styled components with a more traditional CSS syntax.
- **Usage:** Apply styles directly using the `css` prop or styled components.

**Benefits:**

- Flexible styling with dynamic capabilities.
- Supports both CSS and styled components approaches.



## Managing Responsive Design
### 1. Media Queries

Use media queries to handle responsive design by applying different styles based on screen size.

**Example:**

```tsx
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

const responsiveStyle = css`
  padding: 20px;
  background-color: #f0f0f0;

  @media (max-width: 600px) {
    padding: 10px;
    background-color: #e0e0e0;
  }

  @media (min-width: 601px) and (max-width: 1024px) {
    padding: 15px;
    background-color: #d0d0d0;
  }
`;

// MyResponsiveComponent.tsx
import React from 'react';
import { css } from '@emotion/react';

const MyResponsiveComponent: React.FC = () => {
  return (
    <div css={responsiveStyle}>
      <h1>Responsive Design Example</h1>
      <p>This component adjusts its styles based on screen size.</p>
    </div>
  );
};

export default MyResponsiveComponent;
```

**Code Explanation:**

- **`@media` Queries:** Define different styles for various screen sizes.
- **Usage:** Apply styles conditionally based on screen width to achieve responsive design.

**Benefits:**

- Creates a flexible UI that adapts to different devices and screen sizes.

### 2. Flexbox and Grid Layout

Use Flexbox and Grid Layout to create responsive designs that adapt to different screen sizes.

**Example:**

```tsx
/** @jsxImportSource @emotion/react */
import { css } from '@emotion/react';

// Flexbox example
const flexContainer = css`
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
`;

// Grid example
const gridContainer = css`
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
`;

// MyResponsiveComponent.tsx
import React from 'react';
import { css } from '@emotion/react';

const MyResponsiveComponent: React.FC = () => {
  return (
    <div css={flexContainer}>
      <div css={gridContainer}>
        <div>Item 1</div>
        <div>Item 2</div>
        <div>Item 3</div>
      </div>
    </div>
  );
};

export default MyResponsiveComponent;
```

**Code Explanation:**

- **Flexbox (`flexContainer`):** Uses `flex-wrap` and `justify-content` to manage item alignment and wrapping.
- **Grid (`gridContainer`):** Defines a responsive grid layout using `grid-template-columns` and `auto-fill`.

**Benefits:**

- Flexbox: Ideal for aligning items in a row or column.
- Grid: Perfect for creating complex, multi-dimensional layouts.



## Theming and Customization
### 1. Creating Themes

Implement themes to apply consistent styles across your app using context and CSS variables.

**Example:**

```
tsxCopy code/** @jsxImportSource @emotion/react */
import { css, Global } from '@emotion/react';
import React, { createContext, useContext, useState } from 'react';

// Define theme
const themes = {
  light: {
    background: '#ffffff',
    color: '#000000',
  },
  dark: {
    background: '#333333',
    color: '#ffffff',
  },
};

// Create Theme Context
const ThemeContext = createContext({
  theme: themes.light,
  setTheme: (theme: keyof typeof themes) => {},
});

// ThemeProvider Component
const ThemeProvider: React.FC = ({ children }) => {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');

  return (
    <ThemeContext.Provider value={{ theme: themes[theme], setTheme }}>
      <Global
        styles={css`
          body {
            background-color: ${themes[theme].background};
            color: ${themes[theme].color};
          }
        `}
      />
      {children}
    </ThemeContext.Provider>
  );
};

// Use Theme
const ThemedComponent: React.FC = () => {
  const { setTheme } = useContext(ThemeContext);

  return (
    <div>
      <button onClick={() => setTheme('light')}>Light Theme</button>
      <button onClick={() => setTheme('dark')}>Dark Theme</button>
    </div>
  );
};

export default function App() {
  return (
    <ThemeProvider>
      <ThemedComponent />
    </ThemeProvider>
  );
}
```

**Code Explanation:**

- **Themes:** Define light and dark themes with different background and text colors.
- **Theme Context:** Manage and provide the current theme throughout the app.
- **ThemeProvider Component:** Uses `Global` from `@emotion/react` to apply theme styles globally.
- **ThemedComponent:** Provides buttons to switch between themes.

**Benefits:**

- Centralized theme management.
- Easy customization and theme switching.

### 2. Implementing Theme Switchers

Create a theme switcher to toggle between light and dark themes using React's context API.

**Example:**

```
tsxCopy code/** @jsxImportSource @emotion/react */
import { css, Global } from '@emotion/react';
import React, { createContext, useContext, useState } from 'react';

// Define themes
const themes = {
  light: {
    background: '#ffffff',
    color: '#000000',
  },
  dark: {
    background: '#333333',
    color: '#ffffff',
  },
};

// Create Theme Context
const ThemeContext = createContext({
  theme: themes.light,
  toggleTheme: () => {},
});

// ThemeProvider Component
const ThemeProvider: React.FC = ({ children }) => {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');

  const toggleTheme = () => {
    setTheme((prev) => (prev === 'light' ? 'dark' : 'light'));
  };

  return (
    <ThemeContext.Provider value={{ theme: themes[theme], toggleTheme }}>
      <Global
        styles={css`
          body {
            background-color: ${themes[theme].background};
            color: ${themes[theme].color};
          }
        `}
      />
      {children}
    </ThemeContext.Provider>
  );
};

// ThemeSwitcher Component
const ThemeSwitcher: React.FC = () => {
  const { toggleTheme } = useContext(ThemeContext);

  return (
    <button onClick={toggleTheme}>Toggle Theme</button>
  );
};

export default function App() {
  return (
    <ThemeProvider>
      <ThemeSwitcher />
    </ThemeProvider>
  );
}
```

**Code Explanation:**

- **Themes:** Define light and dark themes with different styles.
- **Theme Context:** Provides current theme and a function to toggle between themes.
- **ThemeProvider Component:** Applies theme styles globally with `Global`.
- **ThemeSwitcher Component:** Button to toggle between light and dark themes.

**Benefits:**

- Easy theme switching with context.
- Centralized theme management and global style updates.

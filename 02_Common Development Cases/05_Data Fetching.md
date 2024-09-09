# Data Fetching

## Fetching Data with useEffect
### Managing Loading and Error States

**Fetching Data with `useEffect` - Managing Loading and Error States:**

Fetch data inside `useEffect` and handle loading and error states for robust data fetching.

**Example Code:**

```tsx
import React, { useState, useEffect } from 'react';

const DataFetcher: React.FC = () => {
  const [data, setData] = useState<any>(null); // Data state
  const [loading, setLoading] = useState<boolean>(true); // Loading state
  const [error, setError] = useState<string | null>(null); // Error state

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) throw new Error('Network response was not ok');
        const result = await response.json();
        setData(result); // Update data state
      } catch (error) {
        setError(error.message); // Set error state
      } finally {
        setLoading(false); // Set loading to false
      }
    };

    fetchData();
  }, []); // Empty dependency array means this effect runs once

  if (loading) return <div>Loading...</div>; // Show loading message
  if (error) return <div>Error: {error}</div>; // Show error message

  return <div>Data: {JSON.stringify(data)}</div>; // Render fetched data
};

export default DataFetcher;
```

**Comments in Code:**
- **`useState`**: Manages `data`, `loading`, and `error` states.
- **`useEffect`**: Fetches data on component mount.
- **`setLoading(false)`**: Updates loading state after data fetching.
- **`setError(error.message)`**: Handles errors by setting the error state.
- **Conditional Rendering**: Shows loading or error messages based on state.



## Using Libraries for Data Fetching
### 1. Axios for HTTP Requests

**Using Axios for HTTP Requests in React:**

Axios simplifies HTTP requests in React. Set up Axios and use it in `useEffect` for data fetching.

**Example Code:**

```tsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const DataFetcher: React.FC = () => {
  const [data, setData] = useState<any>(null); // Data state
  const [loading, setLoading] = useState<boolean>(true); // Loading state
  const [error, setError] = useState<string | null>(null); // Error state

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await axios.get('https://api.example.com/data');
        setData(response.data); // Update data state
      } catch (error) {
        setError(error.message); // Set error state
      } finally {
        setLoading(false); // Set loading to false
      }
    };

    fetchData();
  }, []); // Empty dependency array means this effect runs once

  if (loading) return <div>Loading...</div>; // Show loading message
  if (error) return <div>Error: {error}</div>; // Show error message

  return <div>Data: {JSON.stringify(data)}</div>; // Render fetched data
};

export default DataFetcher;
```

**Comments in Code:**
- **`axios.get`**: Performs the HTTP GET request.
- **`setData(response.data)`**: Updates the state with fetched data.
- **`setError(error.message)`**: Handles errors by setting the error state.
- **Conditional Rendering**: Shows loading or error messages based on state.

### 2. React Query for Data Management

**Using React Query for Data Management in React:**

React Query simplifies data fetching and state management. Use `useQuery` to fetch data and handle loading and error states.

**Example Code:**

```tsx
import React from 'react';
import { useQuery } from 'react-query';
import axios from 'axios';

// Fetch function
const fetchData = async () => {
  const response = await axios.get('https://api.example.com/data');
  return response.data;
};

const DataFetcher: React.FC = () => {
  // useQuery hook for data fetching
  const { data, error, isLoading } = useQuery('fetchData', fetchData);

  if (isLoading) return <div>Loading...</div>; // Show loading message
  if (error) return <div>Error: {error.message}</div>; // Show error message

  return <div>Data: {JSON.stringify(data)}</div>; // Render fetched data
};

export default DataFetcher;
```

**Comments in Code:**
- **`useQuery`**: Manages fetching, caching, and updating data.
- **`fetchData`**: Function to fetch data using Axios.
- **Conditional Rendering**: Displays loading or error messages based on query state.



## Handling Pagination and Filtering
### 1. Implementing Infinite Scroll

**Implementing Infinite Scroll in React:**

Infinite Scroll loads more data as the user scrolls down. Use an `IntersectionObserver` to trigger data fetching when nearing the bottom.

**Example Code:**

```tsx
import React, { useState, useEffect, useCallback } from 'react';

const fetchData = async (page: number) => {
  // Fetch paginated data
  const response = await fetch(`https://api.example.com/items?page=${page}`);
  return response.json();
};

const InfiniteScroll: React.FC = () => {
  const [items, setItems] = useState<any[]>([]);
  const [page, setPage] = useState(1);
  const [loading, setLoading] = useState(false);

  // Fetch data and update state
  const loadMoreItems = useCallback(async () => {
    setLoading(true);
    const newItems = await fetchData(page);
    setItems(prev => [...prev, ...newItems]);
    setPage(prev => prev + 1);
    setLoading(false);
  }, [page]);

  // Handle scroll event using IntersectionObserver
  useEffect(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting && !loading) {
          loadMoreItems();
        }
      },
      { threshold: 1.0 }
    );

    const sentinel = document.getElementById('sentinel');
    if (sentinel) observer.observe(sentinel);

    return () => {
      if (sentinel) observer.unobserve(sentinel);
    };
  }, [loadMoreItems, loading]);

  return (
    <div>
      <ul>
        {items.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      {/* Sentinel element for intersection observer */}
      <div id="sentinel" style={{ height: '20px' }}></div>
      {loading && <div>Loading more...</div>}
    </div>
  );
};

export default InfiniteScroll;
```

**Comments in Code:**
- **`fetchData`**: Function to fetch paginated data.
- **`IntersectionObserver`**: Triggers data loading when the sentinel element is visible.
- **Sentinel Element**: Detects when to load more items.

### 2. Filtering Data from API

**Filtering Data from API in React:**

Filter data fetched from an API by applying conditions based on user input.

**Example Code:**

```tsx
import React, { useState, useEffect } from 'react';

const fetchData = async (query: string) => {
  // Fetch data with filtering query
  const response = await fetch(`https://api.example.com/items?filter=${query}`);
  return response.json();
};

const DataFilter: React.FC = () => {
  const [items, setItems] = useState<any[]>([]);
  const [query, setQuery] = useState('');
  const [loading, setLoading] = useState(false);

  // Fetch and filter data based on query
  useEffect(() => {
    const fetchFilteredData = async () => {
      setLoading(true);
      const data = await fetchData(query);
      setItems(data);
      setLoading(false);
    };

    fetchFilteredData();
  }, [query]); // Re-fetch data when query changes

  return (
    <div>
      <input
        type="text"
        placeholder="Filter..."
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />
      {loading ? (
        <div>Loading...</div>
      ) : (
        <ul>
          {items.map(item => (
            <li key={item.id}>{item.name}</li>
          ))}
        </ul>
      )}
    </div>
  );
};

export default DataFilter;
```

**Comments in Code:**
- **`fetchData`**: Fetches filtered data based on the query.
- **`useEffect`**: Re-fetches data whenever the `query` state changes.
- **Input Field**: Updates the filter query.
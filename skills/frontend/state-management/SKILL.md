---
name: state-management
description: State management patterns for React applications - Redux, Zustand, React Query, Context API, and local state. Choose the right tool for your data flow needs.
tags: [state, redux, zustand, react-query, context, hooks, data-flow]
version: 1.0.0
author: Enhanced from state management best practices)
---

# State Management

## Overview

Manage application state effectively across components.

**Why state management matters:**
- **Data flow:** Predictable data updates
- **Sharing:** Share state across components
- **Performance:** Avoid unnecessary re-renders
- **Debugging:** Track state changes
- **Testing:** Easier to test

**Key principles:**
- **Single source of truth:** One place for each piece of state
- **Immutability:** Never mutate state directly
- **Predictability:** State changes are predictable
- **Separation:** UI state vs server state
- **Minimal:** Only lift state when needed

## When to Use

**Use for:**
- Multi-component state sharing
- Complex data flows
- Server data caching
- Global application state
- Form state management

**Critical for:**
- Large applications (20+ components)
- Real-time data updates
- Complex user interactions
- Multi-step workflows
- Data-heavy applications

## The State Management Decision Tree

```
┌─────────────────────────────────────────────────────────┐
│           STATE MANAGEMENT DECISION TREE                 │
└─────────────────────────────────────────────────────────┘

    Is it server data?
           │
    ┌──────┴──────┐
    │             │
   YES           NO
    │             │
    ▼             ▼
React Query   Is it shared?
TanStack           │
Query         ┌────┴────┐
              │         │
             YES       NO
              │         │
              ▼         ▼
        How many    useState
        components?    (local)
              │
        ┌─────┴─────┐
        │           │
       2-3        5+
        │           │
        ▼           ▼
    Context     Zustand
      API       or Redux
```

## Layer 1: Local State (useState)

### Goal: Component-level state

**When to use:**
- State used in single component
- Simple state (string, number, boolean)
- No sharing needed

**Examples:**

```tsx
import { useState } from 'react';

// Simple toggle
function Toggle() {
  const [isOn, setIsOn] = useState(false);
  
  return (
    <button onClick={() => setIsOn(!isOn)}>
      {isOn ? 'ON' : 'OFF'}
    </button>
  );
}

// Form input
function SearchBar() {
  const [query, setQuery] = useState('');
  
  return (
    <input
      value={query}
      onChange={(e) => setQuery(e.target.value)}
      placeholder="Search..."
    />
  );
}

// Counter
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
    </div>
  );
}

// Complex state (object)
function UserForm() {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0,
  });
  
  const updateField = (field: string, value: any) => {
    setUser(prev => ({ ...prev, [field]: value }));
  };
  
  return (
    <form>
      <input
        value={user.name}
        onChange={(e) => updateField('name', e.target.value)}
      />
      <input
        value={user.email}
        onChange={(e) => updateField('email', e.target.value)}
      />
      <input
        type="number"
        value={user.age}
        onChange={(e) => updateField('age', parseInt(e.target.value))}
      />
    </form>
  );
}
```

**useReducer (complex local state):**

```tsx
import { useReducer } from 'react';

// State type
interface State {
  count: number;
  step: number;
}

// Action types
type Action =
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'setStep'; payload: number }
  | { type: 'reset' };

// Reducer
function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + state.step };
    case 'decrement':
      return { ...state, count: state.count - state.step };
    case 'setStep':
      return { ...state, step: action.payload };
    case 'reset':
      return { count: 0, step: 1 };
    default:
      return state;
  }
}

// Component
function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0, step: 1 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <p>Step: {state.step}</p>
      
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      
      <input
        type="number"
        value={state.step}
        onChange={(e) => dispatch({ type: 'setStep', payload: parseInt(e.target.value) })}
      />
      
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

---

## Layer 2: Context API (Shared State)

### Goal: Share state across component tree

**When to use:**
- State shared by 2-5 components
- Theme, locale, auth user
- Avoid prop drilling
- Not frequently updated

**Basic Context:**

```tsx
import { createContext, useContext, useState, ReactNode } from 'react';

// 1. Create context
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

// 2. Create provider
interface ThemeProviderProps {
  children: ReactNode;
}

export function ThemeProvider({ children }: ThemeProviderProps) {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Create hook
export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

// 4. Use in components
function App() {
  return (
    <ThemeProvider>
      <Header />
      <Main />
    </ThemeProvider>
  );
}

function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header className={theme}>
      <button onClick={toggleTheme}>
        Toggle Theme
      </button>
    </header>
  );
}
```

**Auth Context:**

```tsx
import { createContext, useContext, useState, ReactNode } from 'react';

interface User {
  id: string;
  name: string;
  email: string;
}

interface AuthContextType {
  user: User | null;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  
  const login = async (email: string, password: string) => {
    // Call API
    const response = await fetch('/api/login', {
      method: 'POST',
      body: JSON.stringify({ email, password }),
    });
    
    const userData = await response.json();
    setUser(userData);
  };
  
  const logout = () => {
    setUser(null);
  };
  
  return (
    <AuthContext.Provider value={{
      user,
      login,
      logout,
      isAuthenticated: !!user,
    }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// Usage
function LoginButton() {
  const { login, isAuthenticated, user } = useAuth();
  
  if (isAuthenticated) {
    return <p>Welcome, {user?.name}!</p>;
  }
  
  return (
    <button onClick={() => login('user@example.com', 'password')}>
      Login
    </button>
  );
}
```

**Performance Optimization (split contexts):**

```tsx
// ❌ Bad: Single context with all state
const AppContext = createContext({
  user: null,
  theme: 'light',
  locale: 'en',
  notifications: [],
  // ... many more
});

// ✅ Good: Split into separate contexts
const UserContext = createContext(null);
const ThemeContext = createContext('light');
const LocaleContext = createContext('en');
const NotificationsContext = createContext([]);

// Only components using theme re-render when theme changes
```

---

## Layer 3: Zustand (Global State)

### Goal: Simple global state management

**When to use:**
- State shared by 5+ components
- Global app state (cart, settings)
- Simpler than Redux
- Minimal boilerplate

**Installation:**

```bash
npm install zustand
```

**Basic Store:**

```tsx
import { create } from 'zustand';

interface CounterState {
  count: number;
  increment: () => void;
  decrement: () => void;
  reset: () => void;
}

export const useCounterStore = create<CounterState>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));

// Usage
function Counter() {
  const { count, increment, decrement, reset } = useCounterStore();
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

// Select specific state (performance)
function CountDisplay() {
  const count = useCounterStore((state) => state.count);
  return <p>Count: {count}</p>;
}
```

**Shopping Cart Store:**

```tsx
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface CartItem {
  id: string;
  name: string;
  price: number;
  quantity: number;
}

interface CartState {
  items: CartItem[];
  addItem: (item: Omit<CartItem, 'quantity'>) => void;
  removeItem: (id: string) => void;
  updateQuantity: (id: string, quantity: number) => void;
  clearCart: () => void;
  total: number;
}

export const useCartStore = create<CartState>()(
  persist(
    (set, get) => ({
      items: [],
      
      addItem: (item) => set((state) => {
        const existing = state.items.find(i => i.id === item.id);
        
        if (existing) {
          return {
            items: state.items.map(i =>
              i.id === item.id
                ? { ...i, quantity: i.quantity + 1 }
                : i
            ),
          };
        }
        
        return {
          items: [...state.items, { ...item, quantity: 1 }],
        };
      }),
      
      removeItem: (id) => set((state) => ({
        items: state.items.filter(i => i.id !== id),
      })),
      
      updateQuantity: (id, quantity) => set((state) => ({
        items: state.items.map(i =>
          i.id === id ? { ...i, quantity } : i
        ),
      })),
      
      clearCart: () => set({ items: [] }),
      
      get total() {
        return get().items.reduce(
          (sum, item) => sum + item.price * item.quantity,
          0
        );
      },
    }),
    {
      name: 'cart-storage', // localStorage key
    }
  )
);

// Usage
function Cart() {
  const { items, removeItem, total } = useCartStore();
  
  return (
    <div>
      <h2>Cart</h2>
      {items.map(item => (
        <div key={item.id}>
          <p>{item.name} x {item.quantity}</p>
          <p>${item.price * item.quantity}</p>
          <button onClick={() => removeItem(item.id)}>Remove</button>
        </div>
      ))}
      <p>Total: ${total}</p>
    </div>
  );
}
```

**Async Actions:**

```tsx
interface UserState {
  users: User[];
  loading: boolean;
  error: string | null;
  fetchUsers: () => Promise<void>;
}

export const useUserStore = create<UserState>((set) => ({
  users: [],
  loading: false,
  error: null,
  
  fetchUsers: async () => {
    set({ loading: true, error: null });
    
    try {
      const response = await fetch('/api/users');
      const users = await response.json();
      set({ users, loading: false });
    } catch (error) {
      set({ error: error.message, loading: false });
    }
  },
}));
```

---

## Layer 4: Redux Toolkit (Complex State)

### Goal: Predictable state container for complex apps

**When to use:**
- Very large applications (50+ components)
- Complex state logic
- Time-travel debugging needed
- Team familiar with Redux

**Installation:**

```bash
npm install @reduxjs/toolkit react-redux
```

**Store Setup:**

```tsx
// store.ts
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';
import userReducer from './userSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
    user: userReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

// App.tsx
import { Provider } from 'react-redux';
import { store } from './store';

function App() {
  return (
    <Provider store={store}>
      <YourApp />
    </Provider>
  );
}
```

**Slice (Redux Toolkit):**

```tsx
// counterSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface CounterState {
  value: number;
  step: number;
}

const initialState: CounterState = {
  value: 0,
  step: 1,
};

const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += state.step;
    },
    decrement: (state) => {
      state.value -= state.step;
    },
    setStep: (state, action: PayloadAction<number>) => {
      state.step = action.payload;
    },
    reset: (state) => {
      state.value = 0;
      state.step = 1;
    },
  },
});

export const { increment, decrement, setStep, reset } = counterSlice.actions;
export default counterSlice.reducer;

// Component
import { useSelector, useDispatch } from 'react-redux';
import { RootState } from './store';
import { increment, decrement, reset } from './counterSlice';

function Counter() {
  const { value, step } = useSelector((state: RootState) => state.counter);
  const dispatch = useDispatch();
  
  return (
    <div>
      <p>Count: {value}</p>
      <p>Step: {step}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(reset())}>Reset</button>
    </div>
  );
}
```

**Async Thunks:**

```tsx
// userSlice.ts
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

interface User {
  id: string;
  name: string;
  email: string;
}

interface UserState {
  users: User[];
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  users: [],
  loading: false,
  error: null,
};

// Async thunk
export const fetchUsers = createAsyncThunk(
  'user/fetchUsers',
  async () => {
    const response = await fetch('/api/users');
    return response.json();
  }
);

const userSlice = createSlice({
  name: 'user',
  initialState,
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.users = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message || 'Failed to fetch users';
      });
  },
});

export default userSlice.reducer;

// Component
function UserList() {
  const { users, loading, error } = useSelector((state: RootState) => state.user);
  const dispatch = useDispatch();
  
  useEffect(() => {
    dispatch(fetchUsers());
  }, [dispatch]);
  
  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---

## Layer 5: React Query (Server State)

### Goal: Fetch, cache, and sync server data

**When to use:**
- Fetching data from APIs
- Caching server responses
- Background refetching
- Optimistic updates
- Pagination, infinite scroll

**Installation:**

```bash
npm install @tanstack/react-query
```

**Setup:**

```tsx
// App.tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <YourApp />
    </QueryClientProvider>
  );
}
```

**Basic Query:**

```tsx
import { useQuery } from '@tanstack/react-query';

interface User {
  id: string;
  name: string;
  email: string;
}

function UserList() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['users'],
    queryFn: async () => {
      const response = await fetch('/api/users');
      if (!response.ok) throw new Error('Failed to fetch');
      return response.json() as Promise<User[]>;
    },
  });
  
  if (isLoading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;
  
  return (
    <ul>
      {data?.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

**Mutations:**

```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query';

function CreateUser() {
  const queryClient = useQueryClient();
  
  const mutation = useMutation({
    mutationFn: async (newUser: { name: string; email: string }) => {
      const response = await fetch('/api/users', {
        method: 'POST',
        body: JSON.stringify(newUser),
      });
      return response.json();
    },
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });
  
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    mutation.mutate({ name: 'John', email: 'john@example.com' });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <button type="submit" disabled={mutation.isPending}>
        {mutation.isPending ? 'Creating...' : 'Create User'}
      </button>
      {mutation.isError && <p>Error: {mutation.error.message}</p>}
      {mutation.isSuccess && <p>User created!</p>}
    </form>
  );
}
```

**Optimistic Updates:**

```tsx
const mutation = useMutation({
  mutationFn: updateUser,
  onMutate: async (newUser) => {
    // Cancel outgoing refetches
    await queryClient.cancelQueries({ queryKey: ['users', newUser.id] });
    
    // Snapshot previous value
    const previousUser = queryClient.getQueryData(['users', newUser.id]);
    
    // Optimistically update
    queryClient.setQueryData(['users', newUser.id], newUser);
    
    // Return context with snapshot
    return { previousUser };
  },
  onError: (err, newUser, context) => {
    // Rollback on error
    queryClient.setQueryData(
      ['users', newUser.id],
      context?.previousUser
    );
  },
  onSettled: (newUser) => {
    // Refetch after error or success
    queryClient.invalidateQueries({ queryKey: ['users', newUser?.id] });
  },
});
```

---

## State Management Comparison

| Tool | Use Case | Complexity | Boilerplate | Performance |
|------|----------|------------|-------------|-------------|
| **useState** | Local state | Low | Minimal | Excellent |
| **useReducer** | Complex local | Low | Low | Excellent |
| **Context API** | 2-5 components | Low | Low | Good |
| **Zustand** | 5+ components | Low | Minimal | Excellent |
| **Redux Toolkit** | Very large apps | Medium | Medium | Excellent |
| **React Query** | Server data | Low | Low | Excellent |

---

## State Management Checklist

**Local State:**
- [ ] Use useState for simple state
- [ ] Use useReducer for complex state
- [ ] Keep state close to where it's used

**Shared State:**
- [ ] Use Context for 2-5 components
- [ ] Use Zustand for 5+ components
- [ ] Split contexts for performance

**Server State:**
- [ ] Use React Query for API data
- [ ] Cache responses
- [ ] Handle loading and error states

**Performance:**
- [ ] Select only needed state
- [ ] Memoize expensive computations
- [ ] Avoid unnecessary re-renders

---

## Common State Management Mistakes

### Mistake 1: Over-using Global State
**Problem:** Everything in Redux/Zustand

**Solution:** Keep state local when possible

### Mistake 2: Prop Drilling
**Problem:** Passing props through 5+ levels

**Solution:** Use Context or global state

### Mistake 3: Mixing Server and UI State
**Problem:** Storing API data in Redux

**Solution:** Use React Query for server data

### Mistake 4: Not Splitting Contexts
**Problem:** Single context causes unnecessary re-renders

**Solution:** Split into separate contexts

### Mistake 5: Mutating State
**Problem:** `state.count++`

**Solution:** Always return new state

---

## Integration with Other Skills

**Use before state management:**
- `component-library` - Component structure
- `architecture-blueprint` - Data flow design

**Use during development:**
- `performance-optimization` - Optimize re-renders
- `test-driven-development` - Test state logic

**Use after implementation:**
- `requesting-code-review` - Review state logic
- `systematic-debugging` - Debug state issues

---

**Remember:** Choose the right tool for the job. Start simple (useState), lift state when needed (Context), use global state sparingly (Zustand/Redux), and always use React Query for server data. Don't over-engineer.

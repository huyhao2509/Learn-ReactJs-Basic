# 📘 Tóm Tắt ReactJS – Tài Liệu Học Nhanh

> Tổng hợp từ 12 sessions của khóa học ReactJS Basic

---

## 📑 Mục Lục

1. [React là gì & Cài đặt môi trường](#1-react-là-gì--cài-đặt-môi-trường)
2. [JSX](#2-jsx)
3. [Components](#3-components)
4. [Props](#4-props)
5. [State & Handling Events](#5-state--handling-events)
6. [Rendering – Conditional & Lists](#6-rendering--conditional--lists)
7. [Forms](#7-forms)
8. [Hooks](#8-hooks)
9. [Simple CRUD với API](#9-simple-crud-với-api)
10. [React Router](#10-react-router)
11. [Global State Management](#11-global-state-management)

---

## 1. React là gì & Cài đặt môi trường

### React là gì?
- Thư viện JavaScript do Facebook phát triển để xây dựng giao diện người dùng (UI).
- Có thể làm **Web App**, **Mobile App** (React Native), **Desktop App**.
- Phổ biến, cộng đồng lớn, được dùng bởi Facebook, Instagram, Netflix, Airbnb...

### Cài đặt dự án

```bash
# Cách 1: Create React App (CRA) với TypeScript
npx create-react-app my-app --template typescript

# Cách 2: Vite (khuyến nghị – nhanh hơn)
npm create vite@latest
# Chọn: React → TypeScript + SWC

cd my-app
yarn        # cài dependencies
yarn dev    # chạy project
```

### Lệnh npm / yarn cơ bản

| npm | yarn | Chức năng |
|-----|------|-----------|
| `npm install` | `yarn` | Cài tất cả dependencies |
| `npm install <pkg>` | `yarn add <pkg>` | Cài package |
| `npm install <pkg> -D` | `yarn add <pkg> -D` | Cài devDependency |
| `npm uninstall <pkg>` | `yarn remove <pkg>` | Gỡ package |

### VS Code Extensions nên cài
- ES7+ React/Redux/React-Native snippets
- ESLint, Prettier
- TypeScript React code snippets

---

## 2. JSX

JSX là cú pháp mở rộng của JavaScript, cho phép viết HTML-like bên trong JS.

```jsx
// Thay vì React.createElement(...)
const element = <h1 className="greeting">Hello, world!</h1>;

// Chèn biến/biểu thức với {}
const name = 'React';
const el = <h1>Hello, {name}!</h1>;
const el2 = <h1>1 + 1 = {1 + 1}</h1>;
```

### ⚠️ Lưu ý quan trọng trong JSX

| HTML | JSX |
|------|-----|
| `class` | `className` |
| `background-color` | `backgroundColor` |
| `<img>` | `<img />` (phải đóng thẻ) |
| `<!-- comment -->` | `{/* comment */}` |

```jsx
// Luôn có một root element (hoặc dùng Fragment)
return (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);

// Hoặc dùng Fragment để tránh thêm thẻ div
return (
  <>
    <h1>Title</h1>
    <p>Content</p>
  </>
);

// Inline style dùng object với camelCase
<div style={{ backgroundColor: 'black', color: 'pink' }}>...</div>
```

---

## 3. Components

Component là một phần UI độc lập, có thể tái sử dụng.

### Tạo Function Component

```tsx
// Tên component PHẢI viết HOA chữ đầu (PascalCase)
function Button() {
  return <button type="button">Click me</button>;
}

// Arrow function
const Button = () => {
  return <button>Click me</button>;
};

export default Button;
```

### Sử dụng & lồng Component

```tsx
import Button from './components/Button';

function App() {
  return (
    <section>
      <h1>Hello</h1>
      <Button />
      <Button />
    </section>
  );
}
```

### Cấu trúc thư mục chuẩn

```
src/
├─ components/     ← các component tái sử dụng
│  ├─ Button.tsx
│  └─ Header.tsx
├─ pages/          ← các trang
│  ├─ HomePage.tsx
│  └─ AboutPage.tsx
├─ layouts/        ← layout chung
│  └─ MainLayout.tsx
├─ context/        ← Context API
└─ App.tsx
```

---

## 4. Props

Props là tham số truyền từ component **CHA → CON** (một chiều).

```tsx
// Định nghĩa type cho props (TypeScript)
type ButtonProps = {
  label: string;
  color?: string;  // ? = optional
};

function Button({ label, color = 'blue' }: ButtonProps) {
  return <button style={{ color }}>{label}</button>;
}

// Sử dụng
<Button label="Thêm giỏ hàng" color="red" />
<Button label="Gọi tư vấn" />  // color = 'blue' (default)
```

### Truyền dữ liệu từ CON → CHA (callback)

```tsx
const Child = ({ onMessageChange }: { onMessageChange: (msg: string) => void }) => {
  return (
    <button onClick={() => onMessageChange('Hello từ con!')}>
      Gửi lên cha
    </button>
  );
};

const Parent = () => {
  const handleMessage = (message: string) => {
    console.log(message);
  };
  return <Child onMessageChange={handleMessage} />;
};
```

### Props Children

```tsx
function Card({ children }: { children: React.ReactNode }) {
  return <div className="card">{children}</div>;
}

// Sử dụng
<Card>
  <h2>Tiêu đề</h2>
  <p>Nội dung bất kỳ</p>
</Card>
```

---

## 5. State & Handling Events

### useState

```tsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);  // [giá trị, hàm thay đổi]

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Tăng</button>
      <button onClick={() => setCount(prev => prev - 1)}>Giảm</button>
    </div>
  );
}
```

### ⚠️ Quy tắc State
- **Không** được mutate state trực tiếp: `state.name = 'abc'` ❌
- **Phải** dùng setter: `setState({ ...state, name: 'abc' })` ✅
- State thay đổi → Component **re-render**

### Handling Events

```tsx
function Form() {
  const [value, setValue] = useState('');

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value);
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();  // ngăn reload trang
    console.log(value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input value={value} onChange={handleChange} />
      <button type="submit">Gửi</button>
    </form>
  );
}
```

---

## 6. Rendering – Conditional & Lists

### Conditional Rendering

```tsx
const isLoggedIn = true;

// Cách 1: Toán tử 3 ngôi (? :)
{isLoggedIn ? <UserPanel /> : <LoginButton />}

// Cách 2: AND operator (&&) - chỉ hiện khi true
{isLoggedIn && <UserPanel />}

// Cách 3: if/else return JSX
function Panel({ isAdmin }: { isAdmin: boolean }) {
  if (isAdmin) return <AdminView />;
  return <UserView />;
}

// Cách 4: return null để không render gì
{shouldHide && null}
```

### List Rendering với map()

```tsx
const products = [
  { id: 1, name: 'Sản phẩm A', price: 100000 },
  { id: 2, name: 'Sản phẩm B', price: 200000 },
];

function ProductList() {
  return (
    <ul>
      {products.map(product => (
        // Key PHẢI là unique và ổn định – dùng id, không dùng index nếu có thể
        <li key={product.id}>
          {product.name} - {product.price}
        </li>
      ))}
    </ul>
  );
}
```

### Filter + Map

```tsx
// Chỉ hiển thị sản phẩm có discount
const discounted = products
  .filter(p => p.discount > 0)
  .map(p => <ProductCard key={p.id} product={p} />);
```

---

## 7. Forms

### Controlled Component (khuyến nghị)

```tsx
function LoginForm() {
  const [form, setForm] = useState({ email: '', password: '' });

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    console.log(form);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" value={form.email} onChange={handleChange} />
      <input name="password" type="password" value={form.password} onChange={handleChange} />
      <button type="submit">Đăng nhập</button>
    </form>
  );
}
```

---

## 8. Hooks

> Hooks chỉ dùng trong **Function Component** và phải gọi ở **top level** (không trong if/loop).

### useState – Lưu trữ state

```tsx
const [count, setCount] = useState<number>(0);
const [user, setUser] = useState<User | null>(null);
```

### useEffect – Side Effects

```tsx
import { useEffect } from 'react';

// 1. Chạy MỖI LẦN render
useEffect(() => {
  document.title = 'Page Title';
});

// 2. Chạy CHỈ LẦN ĐẦU (component mount)
useEffect(() => {
  fetchData();
}, []);  // mảng rỗng

// 3. Chạy khi dependency thay đổi
useEffect(() => {
  fetchUserById(userId);
}, [userId]);  // chạy lại khi userId thay đổi

// 4. Cleanup (component unmount)
useEffect(() => {
  const timer = setInterval(() => setCount(c => c + 1), 1000);
  return () => clearInterval(timer);  // cleanup
}, []);

// 5. Fetch API đúng cách
useEffect(() => {
  const controller = new AbortController();
  fetch('/api/data', { signal: controller.signal })
    .then(res => res.json())
    .then(data => setData(data));
  return () => controller.abort();
}, []);
```

### useRef – Tham chiếu DOM / giá trị không re-render

```tsx
import { useRef } from 'react';

function InputFocus() {
  const inputRef = useRef<HTMLInputElement>(null);

  const focusInput = () => {
    inputRef.current?.focus();
  };

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus</button>
    </>
  );
}
```

### useContext – Chia sẻ state không cần prop drilling

```tsx
// 1. Tạo context (context/ThemeContext.tsx)
import { createContext, useContext } from 'react';

type Theme = 'light' | 'dark';
const ThemeContext = createContext<Theme>('light');

export const ThemeProvider = ({ children }: { children: React.ReactNode }) => {
  return (
    <ThemeContext.Provider value="dark">
      {children}
    </ThemeContext.Provider>
  );
};

// 2. Dùng ở component bất kỳ trong cây
function Button() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Click</button>;
}

// 3. Bọc ở App
<ThemeProvider>
  <App />
</ThemeProvider>
```

### useMemo – Cache kết quả tính toán nặng

```tsx
import { useMemo } from 'react';

const expensiveResult = useMemo(() => {
  return heavyCalculation(data);  // chỉ tính lại khi `data` thay đổi
}, [data]);
```

### useCallback – Cache hàm tránh re-render con

```tsx
import { useCallback, memo } from 'react';

const Parent = () => {
  const [count, setCount] = useState(0);

  // addTodo không tạo reference mới khi count thay đổi
  const addTodo = useCallback(() => {
    setTodos(t => [...t, 'New']);
  }, []);  // dependency

  return <Child onAdd={addTodo} />;
};

// Component con cần memo() để hưởng lợi từ useCallback
const Child = memo(({ onAdd }: { onAdd: () => void }) => {
  return <button onClick={onAdd}>Add</button>;
});
```

### useReducer – Quản lý state phức tạp

```tsx
import { useReducer } from 'react';

type State = { count: number };
type Action = { type: 'increment' | 'decrement' | 'reset' };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    case 'reset':     return { count: 0 };
    default:          return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </>
  );
}
```

### React.memo – Tránh re-render component con không cần thiết

```tsx
import { memo } from 'react';

// Component chỉ re-render khi props thay đổi
const ExpensiveComponent = memo(({ data }: { data: string }) => {
  return <div>{data}</div>;
});
```

### Custom Hook – Tái sử dụng logic

```tsx
// hooks/useFetch.ts
import { useState, useEffect } from 'react';

function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => { setData(data); setLoading(false); })
      .catch(err => { setError(err); setLoading(false); });
  }, [url]);

  return { data, loading, error };
}

// Sử dụng
const { data, loading } = useFetch<Product[]>('/api/products');
```

---

## 9. Simple CRUD với API

```tsx
const API_URL = 'http://localhost:3000/products';

// GET – Lấy danh sách
const fetchProducts = async () => {
  const res = await fetch(API_URL);
  return res.json();
};

// POST – Thêm mới
const createProduct = async (product: Omit<Product, 'id'>) => {
  const res = await fetch(API_URL, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(product),
  });
  return res.json();
};

// PUT – Cập nhật toàn bộ
const updateProduct = async (id: number, product: Product) => {
  const res = await fetch(`${API_URL}/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(product),
  });
  return res.json();
};

// DELETE – Xóa
const deleteProduct = async (id: number) => {
  await fetch(`${API_URL}/${id}`, { method: 'DELETE' });
};
```

---

## 10. React Router

### Cài đặt

```bash
yarn add react-router-dom
```

### Cấu hình Routes

```tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<HomePage />} />
          <Route path="products" element={<ProductPage />} />
          <Route path="products/:id" element={<ProductDetailPage />} />
          <Route path="*" element={<NotFoundPage />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

### Layout với Outlet

```tsx
import { Outlet, Link } from 'react-router-dom';

const Layout = () => (
  <>
    <nav>
      <Link to="/">Home</Link>
      <Link to="/products">Products</Link>
    </nav>
    <main>
      <Outlet />  {/* Nội dung trang con hiển thị ở đây */}
    </main>
    <footer>Footer</footer>
  </>
);
```

### Hooks của React Router

```tsx
import { useParams, useNavigate, useSearchParams } from 'react-router-dom';

// Lấy URL parameter: /products/:id
const { id } = useParams();

// Chuyển trang bằng code
const navigate = useNavigate();
navigate('/login');
navigate(-1);  // quay lại trang trước

// Query string: /products?page=2
const [searchParams] = useSearchParams();
const page = searchParams.get('page');  // '2'
```

---

## 11. Global State Management

### Khi nào dùng Global State?
- Nhiều component cần cùng một dữ liệu.
- Tránh **prop drilling** (truyền props qua nhiều tầng không cần thiết).

### Zustand (đơn giản, nhẹ)

```bash
yarn add zustand
```

```tsx
import { create } from 'zustand';

// Định nghĩa store
type CartStore = {
  items: Product[];
  addItem: (product: Product) => void;
  removeItem: (id: number) => void;
};

const useCartStore = create<CartStore>((set) => ({
  items: [],
  addItem: (product) =>
    set((state) => ({ items: [...state.items, product] })),
  removeItem: (id) =>
    set((state) => ({ items: state.items.filter(p => p.id !== id) })),
}));

// Dùng ở bất kỳ component nào
function Cart() {
  const { items, removeItem } = useCartStore();
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>
          {item.name}
          <button onClick={() => removeItem(item.id)}>Xóa</button>
        </li>
      ))}
    </ul>
  );
}
```

### Redux Toolkit (dự án lớn)

```bash
yarn add @reduxjs/toolkit react-redux
```

```tsx
// store/counterSlice.ts
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
  },
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;

// store/index.ts
import { configureStore } from '@reduxjs/toolkit';
const store = configureStore({ reducer: { counter: counterSlice.reducer } });

// App.tsx – bọc Provider
import { Provider } from 'react-redux';
<Provider store={store}><App /></Provider>

// Component – dùng useSelector & useDispatch
import { useSelector, useDispatch } from 'react-redux';
const count = useSelector((state: RootState) => state.counter.value);
const dispatch = useDispatch();
dispatch(increment());
```

---

## 🔑 Tổng Hợp Các Khái Niệm Quan Trọng

| Khái niệm | Mô tả ngắn |
|-----------|------------|
| **JSX** | Viết HTML trong JS |
| **Component** | Khối UI độc lập, tái sử dụng |
| **Props** | Dữ liệu từ cha → con (read-only) |
| **State** | Dữ liệu nội bộ, thay đổi → re-render |
| **useState** | Hook quản lý state đơn giản |
| **useEffect** | Xử lý side effects (API, DOM, timer) |
| **useRef** | Tham chiếu DOM, không gây re-render |
| **useContext** | Chia sẻ state không cần prop drilling |
| **useMemo** | Cache kết quả tính toán nặng |
| **useCallback** | Cache function tránh re-render con |
| **useReducer** | Quản lý state phức tạp như Redux |
| **React.memo** | Tránh re-render component khi props không đổi |
| **React Router** | Điều hướng nhiều trang |
| **Zustand** | Global state đơn giản |
| **Redux Toolkit** | Global state cho app lớn |

---

## 📚 Tài Liệu Tham Khảo

- [React Official Docs](https://react.dev)
- [TypeScript for React](https://www.typescriptlang.org/docs/handbook/react.html)
- [React Router Docs](https://reactrouter.com)
- [Zustand Docs](https://github.com/pmndrs/zustand)
- [Redux Toolkit Docs](https://redux-toolkit.js.org)
- [W3Schools React](https://www.w3schools.com/react/)

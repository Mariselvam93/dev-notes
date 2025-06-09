
## 📘 **What is React with TypeScript?**

* **React** is a JavaScript library for building UIs.
* **TypeScript** is a typed superset of JavaScript that adds **static typing** and **type safety**.
* Together, React + TypeScript gives you powerful developer tooling: **autocompletion, early bug detection, and better maintainability**.

---

## 🛠️ **1. Project Setup**

You can use **Vite** or **Create React App (CRA)**.

### ✅ Using Vite (Recommended):

```bash
npm create vite@latest my-app --template react-ts
cd my-app
npm install
npm run dev
```

---

## 🧱 **2. Project Structure**

```
my-app/
│
├── src/
│   ├── components/
│   ├── App.tsx
│   ├── main.tsx
│   └── types/
├── public/
├── index.html
├── package.json
└── tsconfig.json
```

---

## ⚙️ **3. TypeScript Basics in React**

### ✅ Functional Component with Props:

```tsx
type UserProps = {
  name: string;
  age: number;
};

const User: React.FC<UserProps> = ({ name, age }) => (
  <div>{name} is {age} years old.</div>
);
```

### ✅ useState with types:

```tsx
const [count, setCount] = useState<number>(0);
```

### ✅ useEffect:

```tsx
useEffect(() => {
  console.log('Component mounted');
}, []);
```

---

## 🧩 **4. Props, State, and Events**

### ✅ Passing props:

```tsx
<User name="John" age={30} />
```

### ✅ Handling events:

```tsx
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  console.log("Clicked", e);
};
```

---

## 🗂️ **5. Organizing Types**

Create reusable types:

```tsx
// src/types/User.ts
export type User = {
  id: number;
  name: string;
  email: string;
};
```

Use them:

```tsx
import { User } from '../types/User';
```

---

## 🔄 **6. Fetching API with TypeScript**

```tsx
const [users, setUsers] = useState<User[]>([]);

useEffect(() => {
  fetch("https://jsonplaceholder.typicode.com/users")
    .then((res) => res.json())
    .then((data: User[]) => setUsers(data));
}, []);
```

---

## 🧪 **7. React Router (Optional for Routing)**

```bash
npm install react-router-dom
```

```tsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/about" element={<About />} />
  </Routes>
</BrowserRouter>
```

---

## 🎨 **8. Styling Options**

* **CSS Modules**
* **Tailwind CSS**
* **Styled-components**

Example:

```tsx
import styles from './Button.module.css';
```

---

## ✅ **9. Form Handling with React Hook Form**

```bash
npm install react-hook-form
```

```tsx
import { useForm } from "react-hook-form";

type FormData = { name: string };

const Form = () => {
  const { register, handleSubmit } = useForm<FormData>();
  const onSubmit = (data: FormData) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("name")} />
      <button type="submit">Submit</button>
    </form>
  );
};
```

---

## 🧪 **10. Testing (Optional)**

Install Testing Library:

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

Write tests in `*.test.tsx` files.

---

## 🚀 **11. Build and Deploy**

```bash
npm run build
```

Use platforms like:

* **Vercel** (Best for React)
* **Netlify**
* **GitHub Pages**

---

## 📚 Tools and Libraries to Explore

| Tool                  | Purpose                 |
| --------------------- | ----------------------- |
| **Vite**              | Fast bundler for dev    |
| **React Router**      | Routing                 |
| **Zustand / Redux**   | State management        |
| **Axios**             | API calls               |
| **React Query**       | Data fetching & caching |
| **Jest / RTL**        | Testing                 |
| **Tailwind CSS**      | Styling                 |
| **ESLint / Prettier** | Linting & formatting    |
| **React Hook Form**   | Form handling           |

---

## 🧠 Learning Tips

* Focus on **React first**, then add **TypeScript types** gradually.
* Use tools like **TypeScript Playground** to practice types.
* Refer official docs:

  * [React Docs](https://react.dev)
  * [TypeScript-React Cheatsheet](https://react-typescript-cheatsheet.netlify.app)

---


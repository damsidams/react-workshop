# React Workshop - Build a To-Do List App

Support and ⭐ Star me !!

## Introduction

Welcome to this **2-hour React workshop**, where we will build a simple **To-Do List App** while learning the core concepts of React. This step-by-step guide will help you understand how React works and give you hands-on experience in building a basic interactive web application.

## Step 1: Introduction to React

### What is React?

React is a JavaScript library developed by Facebook for building interactive user interfaces. It enables developers to create reusable UI components and manage application state efficiently.

### Why Use React?

- Component-based architecture (reusable UI parts)
- Virtual DOM for efficient updates
- One-way data flow for predictable state management
- Large community and ecosystem

### Key Concepts

- **Components**: Reusable building blocks of UI
- **tsx**: A syntax extension that allows writing HTML within JavaScript
- **Props**: Pass data between components
- **State**: Manage dynamic data in components

### Task

- Research React and its advantages
- Install Node.js and npm if not installed

### Tips

- Visit [React Docs](https://react.dev) for official documentation
- Practice by reading tsx examples

## Step 2: Setting Up a React Project

### Creating a React App with Vite

Vite is a fast build tool that allows for an optimized development experience.

### Task

1. Open a terminal and run:

```bash
npm create vite@latest my-todo-app -- --template react-ts
```

2. Navigate into the project and install dependencies:

```bash
cd my-todo-app
npm install
```

3. Start the development server:

```bash
npm run dev
```

### Project Structure Overview
- **src/** → Contains your React components and app logic
- **index.html** → The main HTML file
- **package.json** → Manages project dependencies

#### Tips
- If you face issues, delete `node_modules` and run `npm install` again.
- Open `App.tsx` and modify some text to see live updates.

---

## Step 3: Creating Our First Component
### Understanding Components
A React app is built using components. Each component is a function that returns tsx.

#### Task
1. Inside `src/`, create a new file: `TodoList.tsx`.
2. Define a simple component:
   ```tsx
   function TodoList() {
       return <h1>My To-Do List</h1>;
   }
   export default TodoList;
   ```
3. Import it into `App.tsx` and use it:
   ```tsx
   import TodoList from "./TodoList";
   function App() {
       return <TodoList />;
   }
   export default App;
   ```

#### Tips
- Component names should start with an uppercase letter.
- A component must return a single root element.
- tsx looks like HTML but is actually JavaScript.

---

## Step 4: Managing State with useState
### What is State?
State is how React components store dynamic data that affects the UI.

### Introducing useState Hook
React provides the `useState` hook to store and update values in components.

#### Task
1. Open `TodoList.tsx` and add state:
   ```tsx
   import { useState } from "react";

   function TodoList() {
       const [tasks, setTasks] = useState([]);
       return <div>{tasks.length === 0 ? "No tasks yet" : tasks.join(", ")}</div>;
   }
   export default TodoList;
   ```

2. Initialize state with an empty array.
3. Display the tasks dynamically.

#### Tips
- `useState` returns an array: `[state, setStateFunction]`.
- Always update state using the provided function (`setTasks`).

---

## Step 5: Adding and Rendering Tasks
### Handling User Input
React uses event handlers to capture user input.

#### Task
1. Add an input field and a button:
   ```tsx
   function TodoList() {
       const [tasks, setTasks] = useState<String[]>([]);
       const [task, setTask] = useState("");
       
       const addTask = () => {
           if (task.trim()) {
               setTasks([...tasks, task]);
               setTask("");
           }
       };

       return (
           <div>
               <input value={task} onChange={(e) => setTask(e.target.value)} />
               <button onClick={addTask}>Add Task</button>
               <ul>
                   {tasks.map((t, index) => (
                       <li key={index}>{t}</li>
                   ))}
               </ul>
           </div>
       );
   }
   ```

#### Tips
- Always use a unique `key` prop when rendering lists.
- `onChange` updates state on user input.
- Prevent empty tasks from being added.

---

## Step 6: Deleting Tasks
### Task
1. Add a delete button to each task:
   ```tsx
   function TodoList() {
       const [tasks, setTasks] = useState([]);
       const [task, setTask] = useState("");
       
       const deleteTask = (index: number) => {
           setTasks(tasks.filter((_, i) => i !== index));
       };
       
       return (
           <ul>
               {tasks.map((t, index) => (
                   <li key={index}>{t} <button onClick={() => deleteTask(index)}>Delete</button></li>
               ))}
           </ul>
       );
   }
   ```

#### Tips
- Use `.filter()` to remove a specific item.
- Keep UI updates efficient.

---

## Step 7: Persisting Data with localStorage
### Task
1. Save tasks when adding or deleting them.
2. Load tasks on app start using `useEffect`.

#### Tips
- Use `localStorage.setItem("tasks", JSON.stringify(tasks))`.
- Retrieve with `JSON.parse(localStorage.getItem("tasks")) || []`.

---

## Bonus Step: Firebase Authentication
### Setting Up Firebase

#### Task
1. Install Firebase:
```bash
npm install firebase
```

2. Create a Firebase project and add configuration:
```tsx
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
};

export const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);
```

### Adding Authentication Components

#### Task
1. Create `src/components/Auth.tsx`:
```tsx
import { useState } from "react";
import { auth } from "../firebase";
import { createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "firebase/auth";

function Auth() {
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");

    const register = async () => {
        try {
            await createUserWithEmailAndPassword(auth, email, password);
            setEmail("");
            setPassword("");
        } catch (error) {
            console.error(error);
        }
    };

    const login = async () => {
        try {
            await signInWithEmailAndPassword(auth, email, password);
            setEmail("");
            setPassword("");
        } catch (error) {
            console.error(error);
        }
    };

    return (
        <div>
            <input
                type="email"
                placeholder="Email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
            />
            <input
                type="password"
                placeholder="Password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
            />
            <button onClick={register}>Register</button>
            <button onClick={login}>Login</button>
            <button onClick={() => signOut(auth)}>Logout</button>
        </div>
    );
}

export default Auth;
```

### Connecting Todo List with Firebase

#### Task
1. Update `TodoList.tsx` to use Firestore:
```tsx
import { useState, useEffect } from "react";
import { auth, db } from "./firebase";
import { collection, addDoc, query, where, getDocs, deleteDoc, doc } from "firebase/firestore";

function TodoList() {
    const [tasks, setTasks] = useState([]);
    const [task, setTask] = useState("");

    useEffect(() => {
        if (auth.currentUser) {
            loadTasks();
        }
    }, [auth.currentUser]);

    const loadTasks = async () => {
        const q = query(
            collection(db, "tasks"),
            where("userId", "==", auth.currentUser.uid)
        );
        const querySnapshot = await getDocs(q);
        setTasks(querySnapshot.docs.map(doc => ({
            id: doc.id,
            ...doc.data()
        })));
    };

    const addTask = async () => {
        if (task.trim() && auth.currentUser) {
            await addDoc(collection(db, "tasks"), {
                text: task,
                userId: auth.currentUser.uid
            });
            setTask("");
            loadTasks();
        }
    };

    const deleteTask = async (taskId) => {
        await deleteDoc(doc(db, "tasks", taskId));
        loadTasks();
    };

    if (!auth.currentUser) {
        return <div>Please login to manage tasks</div>;
    }

    return (
        <div>
            <input value={task} onChange={(e) => setTask(e.target.value)} />
            <button onClick={addTask}>Add Task</button>
            <ul>
                {tasks.map((t) => (
                    <li key={t.id}>
                        {t.text}
                        <button onClick={() => deleteTask(t.id)}>Delete</button>
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default TodoList;
```

#### Tips
- Enable Email/Password authentication in Firebase Console
- Add security rules to Firestore for user data protection
- Handle loading states and errors appropriately

---


## Conclusion & Q&A
Congratulations! You have built a **React To-Do List App** while learning fundamental React concepts. Review the following:
- tsx and Components
- State with `useState`
- Rendering lists dynamically
- Event handling
- Data persistence with `localStorage`

Happy coding! 🚀

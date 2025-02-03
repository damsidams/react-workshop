# React Workshop - Build a To-Do List App

## Introduction

Welcome to this **2-hour React workshop**, where we will build a simple **To-Do List App** while learning the core concepts of React. This step-by-step guide will help you understand how React works and give you hands-on experience in building a basic interactive web application.

## Step 1: Introduction to React (10 min)

### What is React?

React is a JavaScript library developed by Facebook for building interactive user interfaces. It enables developers to create reusable UI components and manage application state efficiently.

### Why Use React?

- Component-based architecture (reusable UI parts)
- Virtual DOM for efficient updates
- One-way data flow for predictable state management
- Large community and ecosystem

### Key Concepts

- **Components**: Reusable building blocks of UI
- **JSX**: A syntax extension that allows writing HTML within JavaScript
- **Props**: Pass data between components
- **State**: Manage dynamic data in components

### Task

- Research React and its advantages
- Install Node.js and npm if not installed

### Tips

- Visit [React Docs](https://react.dev) for official documentation
- Practice by reading JSX examples

## Step 2: Setting Up a React Project (20 min)

### Creating a React App with Vite

Vite is a fast build tool that allows for an optimized development experience.

### Task

1. Open a terminal and run:

```bash
npm create vite@latest my-todo-app --template react
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
- Open `App.jsx` and modify some text to see live updates.

---

## Step 3: Creating Our First Component (20 min)
### Understanding Components
A React app is built using components. Each component is a function that returns JSX.

#### Task
1. Inside `src/`, create a new file: `TodoList.jsx`.
2. Define a simple component:
   ```jsx
   function TodoList() {
       return <h1>My To-Do List</h1>;
   }
   export default TodoList;
   ```
3. Import it into `App.jsx` and use it:
   ```jsx
   import TodoList from "./TodoList";
   function App() {
       return <TodoList />;
   }
   export default App;
   ```

#### Tips
- Component names should start with an uppercase letter.
- A component must return a single root element.
- JSX looks like HTML but is actually JavaScript.

---

## Step 4: Managing State with useState (20 min)
### What is State?
State is how React components store dynamic data that affects the UI.

### Introducing useState Hook
React provides the `useState` hook to store and update values in components.

#### Task
1. Open `TodoList.jsx` and add state:
   ```jsx
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

## Step 5: Adding and Rendering Tasks (20 min)
### Handling User Input
React uses event handlers to capture user input.

#### Task
1. Add an input field and a button:
   ```jsx
   function TodoList() {
       const [tasks, setTasks] = useState([]);
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

## Step 6: Deleting Tasks (15 min)
### Task
1. Add a delete button to each task:
   ```jsx
   function TodoList() {
       const [tasks, setTasks] = useState([]);
       const [task, setTask] = useState("");
       
       const deleteTask = (index) => {
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

## Step 7: Persisting Data with localStorage (15 min)
### Task
1. Save tasks when adding or deleting them.
2. Load tasks on app start using `useEffect`.

#### Tips
- Use `localStorage.setItem("tasks", JSON.stringify(tasks))`.
- Retrieve with `JSON.parse(localStorage.getItem("tasks")) || []`.

---

## Conclusion & Q&A (10 min)
Congratulations! You have built a **React To-Do List App** while learning fundamental React concepts. Review the following:
- JSX and Components
- State with `useState`
- Rendering lists dynamically
- Event handling
- Data persistence with `localStorage`

Happy coding! 🚀
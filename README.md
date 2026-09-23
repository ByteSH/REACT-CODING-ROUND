# 🚀 React Mastery Journey

Welcome to my React learning repository! I created this project to document my journey of learning React.js, from basic concepts to advanced state management and routing. 

Feel free to explore the code snippets and concepts below.

---

## 📑 Table of Contents
1. [Props Drilling of Objects](#1-props-drilling-of-objects)
2. [Child Component (children prop)](#2-child-component)
3. [useState Hooks](#3-usestate-hooks)
4. [useRef Hooks](#4-useref-hooks)
5. [useEffect Hooks](#5-useeffect-hooks)
6. [Passing Functions as Props](#6-how-to-pass-functions-in-react-components)
7. [Lifting State Up](#7-lifting-state-up)
8. [Updating Objects in State](#8-updating-objects-in-state)
9. [useReducer Hook](#9-usereducer-hook)
10. [useContext](#10-usecontext)
11. [useMemo()](#11-usememo)
12. [React Router Installation & Setup](#12-react-router-installation-and-test)
13. [Lazy Loading](#13-lazy-loading)
14. [CRUD API Integration (Fake API)](#14-get-post-api-integration-with-fake-api)
15. [Redux Toolkit (RTK)](#15-rtk)

---

## 1. Props Drilling of Objects

Passing an object from a parent component down to a child component.

**`App.jsx`**
```jsx
import { useState } from "react";
import User from "./User";

export default function App() {
  let userDetails = {
    name: "Hamza"
  }
  return (
    <>
      <User obj={userDetails} />
    </>
  )
}
```

**`User.jsx`**
```jsx
export default function User({ obj }) {
  return (
    <div>
      <h3>Name : {obj.name} </h3>
    </div>
  );
}
```

---

## 2. Child Component
Using the `children` prop to pass components dynamically inside a wrapper.

**`App.jsx`**
```jsx
export default function App() {
  return (
    <>
      <h1>This is Hamza</h1>
      <Wrapper>
        <h3>This is byte</h3>
      </Wrapper>
    </>
  )
}
```

**`Wrapper.jsx`**
```jsx
export default function Wrapper({ children }) {
  return <div>{children}</div>;
}
```

---

## 3. useState Hooks
Handling arrays and checkboxes with `useState`.

**`App.jsx`**
```jsx
import { useState } from "react";

export default function App() {
  const [skills, setSkills] = useState([]);
  
  const handleEvent = (event) => {
    const { value, checked } = event.target;
    if (checked) {
      setSkills((prevSkills) => [...prevSkills, value]);
    } else {
      setSkills((prevSkills) => prevSkills.filter((item) => item !== value));
    }
  };
  
  return (
    <>
      <input type="checkbox" value="JAVA" id="java" onChange={handleEvent} />
      <label htmlFor="java">JAVA</label>
      <br />
      <input type="checkbox" value="React" id="react" onChange={handleEvent} />
      <label htmlFor="react">React</label>
      <br />
      <h3>Selected: {skills.join(", ")}</h3>
    </>
  );
}
```
> 🖼️ **Output:** *(Add your screenshot here showing checkboxes and selected text)*

---

## 4. useRef Hooks
Directly accessing DOM elements without re-rendering.

**`App.jsx`**
```jsx
import { useRef } from 'react';

export default function App() {
  const inputRef = useRef(null);
  
  function handleClick() {
    inputRef.current.focus();
    inputRef.current.style.backgroundColor = "yellow";
  }
  
  return (
    <>
      <input ref={inputRef} type="text" placeholder="Click button to focus me" />
      <button onClick={handleClick}>Focus Input</button>
    </>
  );
}
```
> 🖼️ **Output:** *(Add your screenshot here showing the yellow focused input field)*

---

## 5. useEffect Hooks
Fetching data from an API when the component mounts.

**`App.jsx`**
```jsx
import axios from "axios"
import { useEffect, useState } from "react"

function App() {
  const [users, setUsers] = useState([])
  
  useEffect(() => {
    axios.get("https://dummyjson.com/users")
      .then((response) => {
        setUsers(response.data.users)
      })
      .catch((error) => {
        console.log("Not able to fetch data showing error : ", error);
      })
  }, []) // Empty dependency array added for best practice!

  return (
    <div>
      <h1>Hii Hamza</h1>
      <ul>
        {users.map((value, index) => (
          <li key={index}>{value.firstName}</li>
        ))}
      </ul>
    </div>
  )
}
export default App
```

---

## 6. How to pass functions in React Components?
Passing a handler function from Parent to Child.

**`App.jsx`**
```jsx
import './App.css'
import User from './User';

export default function App() {
  const handleClick = (name) => {
    console.log(`Hello ${name}`);
  }
  return (
    <>
      <User handleClick={handleClick} name="Hamza" />
    </>
  )
}
```

**`User.jsx`**
```jsx
export default function User({ handleClick, name }) {
  return (
    <div>
      <button onClick={() => handleClick(name)}>Click here</button>
    </div>
  );
}
```

---

## 7. Lifting State Up
Moving state to a common parent so sibling components can share data.

**`App.jsx`**
```jsx
import { useState } from "react";
import AddUser from "./AddUser";
import DisplayUser from "./DisplayUser";

function App() {
  const [user, setUser] = useState("");
  return (
    <>
      <AddUser setUser={setUser} />
      <DisplayUser user={user} />
    </>
  )
}
export default App
```

**`AddUser.jsx`**
```jsx
function AddUser({ setUser }) {
  return (
    <div>
      <h3>Enter your username: </h3>
      <input 
        type="text" 
        placeholder="Enter your name." 
        onChange={(event) => setUser(event.target.value)} 
      />
    </div>
  );
}
export default AddUser;
```

**`DisplayUser.jsx`**
```jsx
function DisplayUser({ user }) {
  return (
    <div>
      <h3>The User Name is : {user}</h3>
    </div>
  );
}
export default DisplayUser;
```
> 🖼️ **Output:** *(Add your screenshot here showing the input updating the text below)*

---

## 8. Updating Objects in State
Using the spread operator (`...`) to preserve existing object properties.

**`App.jsx`**
```jsx
import { useState } from "react"

export default function App() {
  const [data, setData] = useState({
    name: "Hamza",
    address: {
      city: "Mumbai"
    }
  });
  
  function handleName(event) {
    setData({ ...data, name: event })
  }
       
  function handleCity(event) {
    setData({ ...data, address: { ...data.address, city: event } })
  }
  
  return (
    <>
      <input type="text" name="name" id="name" onChange={(event) => handleName(event.target.value)} />
      <input type="text" name="city" id="city" onChange={(event) => handleCity(event.target.value)} />
      
      <h1>NAME : {data.name}</h1>
      <h1>CITY : {data.address.city}</h1>
    </>
  )
}
```
> 🖼️ **Output:** *(Add your screenshot here showing dynamic state updates for object keys)*

---

## 9. useReducer Hook
Managing complex state logic using actions and reducers.

**`App.jsx`**
```jsx
import { useReducer } from "react";

const initialState = {
  name: "",
  password: ""
};

const reducer = (state, action) => {
  return { ...state, [action.field]: action.value };
};

export default function App() {
  const [formState, dispatch] = useReducer(reducer, initialState);
  console.log(formState);
  
  return (
    <div>
      <input
        type="text"
        placeholder="Enter Name"
        onChange={(e) => dispatch({ field: "name", value: e.target.value })}
      />
      <br /><br />
      <input
        type="text"
        placeholder="Enter Password"
        onChange={(e) => dispatch({ field: "password", value: e.target.value })}
      />
      <br /><br />
      <button>Add User</button>
    </div>
  );
}
```

---

## 10. useContext
Avoiding prop drilling by providing data globally.

**`SubjectContext.js`**
```jsx
import { createContext } from "react";
export const SubjectContext = createContext(null);
```

**`App.jsx`**
```jsx
import { useState } from "react";
import { SubjectContext } from "./SubjectContext";
import SubjectDisplay from "./SubjectDisplay";

export default function App() {
  const [subject, setSubject] = useState("Mathematics");
  return (
    <div>
      <SubjectContext.Provider value={subject}>
        <input type="text" value={subject} onChange={(event)=> setSubject(event.target.value)} />
        <SubjectDisplay />
      </SubjectContext.Provider>
    </div>
  );
}
```

**`SubjectDisplay.jsx`**
```jsx
import { useContext } from "react"
import { SubjectContext } from "./SubjectContext"

export default function SubjectDisplay() {
    const subject = useContext(SubjectContext)
    return (
        <>
            <h3>Subject : {subject}</h3>
        </>
    )
}
```

---

## 11. useMemo()
Caching the result of an expensive calculation to improve performance.

**`App.jsx`**
```jsx
import { useMemo, useState } from "react";

export default function App() {
  const [count, setCount] = useState(0);
  const [num, setNum] = useState(10);
  
  const squared = useMemo(() => {
    console.log("Calculating...");
    return num * num;
  }, [num]);
  
  return (
    <>
      <p>Square: {squared}</p>
      <button onClick={() => setCount(count + 1)}>
        Re-render ({count})
      </button>
    </>
  );
}
```

---

## 12. React Router Installation and Test
Setting up multiple pages in a React App.

**Terminal Setup**
```bash
npm create vite
npm install
npm install react-router-dom
```

**`main.jsx`**
```jsx
import { createRoot } from 'react-dom/client'
import App from './App.jsx'
import { BrowserRouter } from 'react-router-dom';

createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
)
```

**`App.jsx`**
```jsx
import { Routes, Route, Link } from 'react-router-dom';
import Home from './Home';
import About from './About';

export default function App() {
  return (
    <div className="App">
      <nav>
        <Link to='/'>Home</Link><br />
        <Link to='/about'>About</Link>
      </nav>
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='/about' element={<About />} />
      </Routes>
    </div>
  );
}
```

**`Home.jsx` & `About.jsx`**
```jsx
// Home.jsx
export default function Home() {
  return <div><h1>Welcome to the Home Page</h1></div>;
}

// About.jsx
export default function About() {
  return <div><h1>About Us</h1></div>;
}
```

---

## 13. Lazy Loading
Loading components asynchronously to optimize bundle size.

**`App.jsx`**
```jsx
import { lazy, Suspense, useState } from "react"

const User = lazy(() => import('./UserList'))

export default function App() {
  const [load, setLoad] = useState(false);
  return (
    <div>
      <h1>This is Main Component</h1>
      <button onClick={() => setLoad(true)}>Click here</button>
      {
        load ? <Suspense fallback={<h1>Loading Data...</h1>}><User /></Suspense> : null
      }
    </div>
  )
}
```

**`UserList.jsx`**
```jsx
export default function UserList() {
  return (
    <div>
      <h1>This is User Data</h1>
    </div>
  );
}
```

---

## 14. GET, POST API Integration with Fake API
Full CRUD (Create, Read, Update, Delete) operations using Axios.

**`App.jsx`**
```jsx
import axios from "axios";
import { useEffect, useState } from "react";

const API_URL = "http://localhost:3000/users";

function App() {
  const [users, setUsers] = useState([]);
  const [name, setName] = useState("");
  const [age, setAge] = useState("");
  const [email, setEmail] = useState("");
  
  const [editId, setEditId] = useState(null);
  const [editName, setEditName] = useState("");
  const [editAge, setEditAge] = useState("");
  const [editEmail, setEditEmail] = useState("");

  useEffect(() => {
    fetchData();
  }, []);

  // GET
  const fetchData = async () => {
    try {
      const response = await axios.get(API_URL);
      setUsers(response.data);
    } catch (error) {
      console.log("GET error:", error);
    }
  };

  // POST
  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await axios.post(API_URL, { name, age, email });
      setName("");
      setAge("");
      setEmail("");
      fetchData();
    } catch (error) {
      console.log("POST error:", error);
    }
  };

  // Edit button click → form me data load karo
  const handleEditClick = (user) => {
    setEditId(user.id);
    setEditName(user.name);
    setEditAge(user.age);
    setEditEmail(user.email);
  };

  // PUT
  const handleUpdate = async (e) => {
    e.preventDefault();
    try {
      await axios.put(`${API_URL}/${editId}`, {
        name: editName,
        age: editAge,
        email: editEmail,
      });
      setEditId(null);
      fetchData();
    } catch (error) {
      console.log("PUT error:", error);
    }
  };

  // DELETE
  const handleDelete = async (id) => {
    try {
      await axios.delete(`${API_URL}/${id}`);
      fetchData();
    } catch (error) {
      console.log("DELETE error:", error);
    }
  };

  return (
    <>
      {/* POST FORM */}
      <h2>Add User</h2>
      <form onSubmit={handleSubmit}>
        <input type="text" placeholder="Name" value={name} onChange={(e) => setName(e.target.value)} />
        <input type="text" placeholder="Age" value={age} onChange={(e) => setAge(e.target.value)} />
        <input type="email" placeholder="Email" value={email} onChange={(e) => setEmail(e.target.value)} />
        <button type="submit">Add</button>
      </form>

      {/* PUT FORM — sirf tab dikhega jab Edit button click hoga */}
      {editId && (
        <>
          <h2>Edit User</h2>
          <form onSubmit={handleUpdate}>
            <input type="text" value={editName} onChange={(e) => setEditName(e.target.value)} />
            <input type="text" value={editAge} onChange={(e) => setEditAge(e.target.value)} />
            <input type="email" value={editEmail} onChange={(e) => setEditEmail(e.target.value)} />
            <button type="submit">Update</button>
            <button type="button" onClick={() => setEditId(null)}>Cancel</button>
          </form>
        </>
      )}

      {/* GET LIST + DELETE + EDIT TRIGGER */}
      <h2>Users List</h2>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name} - {user.age} - {user.email}
            <button onClick={() => handleEditClick(user)}>Edit</button>
            <button onClick={() => handleDelete(user.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </>
  );
}
export default App;
```

---

## 15. RTK (Redux Toolkit)
Managing global state the modern way.

**Terminal Setup**
```bash
npm install @reduxjs/toolkit react-redux
```

**`store.js`**
```javascript
import { configureStore } from "@reduxjs/toolkit"
import counterReducer from "./counterSlice"

export const store = configureStore({
  reducer: {
    counter: counterReducer
  }
})
```

**`counterSlice.js`**
```javascript
import { createSlice } from "@reduxjs/toolkit"

const counterSlice = createSlice({
  name: "counter",
  initialState: {
    value: 0
  },
  reducers: {
    increment(state) {
      state.value = state.value + 1
    }
  }
})
export const { increment } = counterSlice.actions
export default counterSlice.reducer
```

**`main.jsx`**
```jsx
import { createRoot } from 'react-dom/client'
import App from './App.jsx'
import { Provider } from 'react-redux'
import { store } from './store'

createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>,
)
```

**`App.jsx`**
```jsx
import { useSelector, useDispatch } from "react-redux"
import { increment } from "./counterSlice"

function App() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()
  
  return (
    <div style={{ padding: "20px" }}>
      <h2>Count: {count}</h2>
      <button onClick={() => dispatch(increment())}>
        Increase
      </button>
    </div>
  )
}
export default App
```

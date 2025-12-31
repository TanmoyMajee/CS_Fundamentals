## Middleware 
Middleware is essentially a function that sits between the incoming request and the final route handler in a web application. It can perform tasks like authentication, logging, and validation before the request reaches the main logic. In Express.js, middleware functions are chained, and each one can either pass control to the next middleware or end the request-response cycle."


Request comes in → Middleware (e.g., authentication) runs first.
If successful, it passes control to the next middleware or the main route handler.
If there's an error, it can stop the request or handle the error.
This keeps things modular and manageable.

```js
async function getDataWithFetch() {
  try {
const res = fetch('/your-api-endpoint', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${yourToken}`
  }
});
if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const data = await response.json();    // manually persing in fetch
    console.log(data);
  } catch (error) {
    console.error('Fetch error:', error);
  }
}

const res = await axios.get('/your-api-endpoint', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
})
  console.log(response.data); // Automatically parsed as JSON 

const authHeader = req.headers.authorization;
const token = authHeader && authHeader.split(' ')[1];

```
## fetch vs Axios
fetch is a built-in JavaScript API that’s simple and straightforward, but it requires more manual handling. axios, on the other hand, is a third-party library that provides a more convenient syntax, automatic JSON parsing, and features like interceptors."

## JSON vs js Object
“JSON is a string format used to exchange data between client and server.

JavaScript object is a data structure within JavaScript that can hold key-value pairs, where keys are strings and values can be any data type.

The main difference is that JSON is a string representation of data, strictly formatted with double quotes around keys and string values, while a JavaScript object is an in-memory data structure. We use JSON because it allows data to be easily shared between different systems, such as between a client and a server, in a consistent and standardized way."

```js
{
  "name": "Tanmoy",   /// JSON
  "age": 22
}
const user = { // js objetc
  name: 'Tanmoy',
  age: 22
};
for (let key in user) {   //it over object  , arr same
  console.log(key, user[key]);
}
 
````

## Event Definition (script)
Events are actions that happen in the browser, like clicking, typing, or submitting a form. JavaScript listens to these actions and runs functions in response

When we click the child element, the event first goes top-down in the capturing phase, then reaches the target, and then bubbles back bottom-up to ancestor elements. We can stop this using stopPropagation().”

Bubbling (default): event flows child → parent → grandparent (btm up)
Capturing: event flows grandparent → parent → child (top down ) 

```jsx
<!DOCTYPE html>
<html>
<head>
  <title>Event Bubbling & Capturing</title>
  <style>
    .grandparent { padding: 40px; background: lightcoral; }
    .parent { padding: 30px; background: lightgreen; }
    .child { padding: 20px; background: lightskyblue; }
  </style>
</head>
<body>

  <div class="grandparent">
    Grandparent
    <div class="parent">
      Parent
      <div class="child">
        Child
      </div>
    </div>
  </div>

  <script>
    const grand = document.querySelector('.grandparent');
    const parent = document.querySelector('.parent');
    const child = document.querySelector('.child');

    // Bubbling (default)
    grand.addEventListener('click', () => console.log('Grandparent Bubbling'));
    parent.addEventListener('click', () => console.log('Parent Bubbling'));
    child.addEventListener('click', () => console.log('Child Bubbling'));

    // Capturing
    grand.addEventListener('click', () => console.log('Grandparent Capturing'), true);
    parent.addEventListener('click', () => console.log('Parent Capturing'), true);
    child.addEventListener('click', () => console.log('Child Capturing'), true);

    //child.addEventListener("click", (event) => {
    //event.stopPropagation(); // ✋ stops bubbling here
    //console.log("Child clicked");
  });
  </script>

</body>
</html>

```
##Window , Document

“window is the global object in the browser. It represents the browser tab or window and contains everything like the DOM, history, localStorage, timers, and global variables and functions.”


“document represents the web page loaded inside the window. It is the root of the DOM and is used to access and manipulate HTML elements.

window.alert("Hello");


## ✅ SPA vs MPA

SPA loads one HTML page and updates content dynamically without reloading.
Example: Gmail, Facebook.

MPA reloads full pages on navigation.
Example: traditional websites.

SPA gives better performance and UX.


## ✅ What are components?

Components are reusable UI pieces.

Types:
functional components
class components

## State vs Props

State – local data, changeable (object that holds a dynamic value )
Props – read-only data passed from parent to child


## What is Mongoose?

"Mongoose is an ODM, which stands for Object Data Modeling library for MongoDB and Node.js. It helps us interact with the MongoDB database using JavaScript in a more structured way.

With Mongoose, we define schemas for our data models. This means we can enforce a structure on the documents we store in MongoDB. It also provides a lot of built-in helper methods to create, read, update, and delete data easily. Plus, we can connect to the database using a connection URL, and Mongoose handles the rest."

It provides:
schema validation
query methods
middleware support


## ✅ Access Token

Short lifetime (like 5–15 minutes)
Sent with every API request
Used to access protected routes
Stored in browser storage or cookies
👉 If stolen, attacker has limited time to misuse.

❌ Problem
When it expires, user will be logged out again and again.

##  ✅ Refresh Token

Longer lifetime (days or weeks)
NOT sent on every API request
Stored securely (usually HTTP-only cookie)
Used only to generate a new access token
👉 If access token expires, refresh token creates another one.

## 🎤 What is a Hook?

"A hook is a special function in React that lets us use features like state and lifecycle methods inside functional components. Before hooks, these features were only available in class components. Hooks make functional components powerful and easier to write."

## 🎤 useState — Script

"useState is a React hook used to create and manage state in a functional component.
It returns two things: the current value and a function to update that value.
Whenever we update the state, the component re-renders and shows the latest value."

👉 Very short example you can mention verbally:

For example, I can use useState to store a counter value and update it when a button is clicked.

##🎤 useEffect — Script

"useEffect is a React hook used to perform side effects in a component.
Side effects include operations like API calls, timers, subscriptions, or updating the DOM.
It runs after the component renders, and we can control when it runs using the dependency array."

👉 One-line behavioral summary:

If the dependency changes, useEffect runs again. If the array is empty, it runs only once on mount.

## useMemo
useMemo is a React hook that helps us store the result of an expensive calculation so we don’t have to recompute it on every render. Basically, it only recalculates the value if the dependencies change.

So in simple terms, if I have a function that runs a loop from 1 to a million and then returns a doubled value, that’s an expensive calculation. useMemo will remember the result for a specific input. If the input changes, it recalculates. If not, it just returns the stored value instantly
````js

import React, { useMemo, useState } from 'react';

function ExpensiveComponent({ number }) {
  const doubledValue = useMemo(() => {
    console.log("Recalculating for", number);
    // Imagine a loop or heavy calculation here
    return number * 2;
  }, [number]);

  return <div>Double: {doubledValue}</div>;
}
  const memoizedValue = useMemo(() => {
    return expensiveCalculation(number);
  }, [number]); // runs only when number changes

````

```js
Script answer

React.memo is an optimization technique in React that we use to memoize a component. That means if we have a parent component and a child component, and we wrap the child component with React.memo, the child component will only re-render if its props actually change. So even if the parent re-renders multiple times, the child stays the same unless its props are updated

🧾 Small example
const Child = React.memo(function Child({ value }) {
  console.log("Child rendered");
  return <h3>{value}</h3>;
});

function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <Child value="Hello" />
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </>
  );
}

useCallback is a React hook that we use to memoize a function. In other words, it returns the same function instance as long as its dependencies don’t change. This is especially useful when you’re passing functions down to child component

So what we do is wrap the function in useCallback in the parent component. This makes sure the function reference stays the same unless its dependencies change. That way, the child component doesn’t re-render unnecessarily and React.memo can do its job properly

🧾 Small example
function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []); // function will not be recreated

  return (
    <>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </>
  );
}

const Child = React.memo(({ onClick }) => {
  console.log("Child rendered");
  return <button onClick={onClick}>Child Button</button>;
});

🧠 Explanation to say

Here, I used useCallback so that the handleClick function is not recreated on every render.
Because its reference stays same, React.memo also works properly
and the Child does not re-render unnecessarily.

🔍 Difference between React.memo and useCallback (script)
🎤 Short answer you can speak

React.memo memoizes a component

useCallback memoizes a function

both reduce unnecessary re-renders

they are often used together
````

## Use Ref

useRef is a React hook that serves two main purposes.

First, it lets us create a variable that stays the same across multiple renders without causing re-renders. For example, if we need a variable to persist between renders but we don’t want changing it to trigger a re-render, we can use useRef. This is handy for things like building a stopwatch where we keep track of time without causing extra renders.

Second, we use useRef to directly interact with DOM elements. For example, if we want to focus an input or change the style of an element when a button is clicked, we can do that by creating a ref, attaching it to the element, and then manipulating it directly."


## What is React Router, and how do you set up routing?
React Router is a library in React used for navigation between different pages or components without reloading the whole page. It helps us build single-page applications with multiple routes.

To set it up, we wrap our app with BrowserRouter, then define routes using the Routes and Route components. Each route maps a URL path to a React component."

## How do you handle forms in React?
"In React, forms are usually handled using state. We create input fields and connect them to state variables, and whenever the user types, we update the state using onChange. These are called controlled components because the form values are controlled by React state. On form submit, we use an event handler to read the state and process the data."
```js
`const [name, setName] = useState("");

<form onSubmit={handleSubmit}>
  <input
    type="text"
    value={name}
    onChange={(e) => setName(e.target.value)}
  />
  <button type="submit">Submit</button>
</form>
````


## What are controlled components versus uncontrolled components?

## What is the difference between React and other frameworks like Angular or Vue?
"Sure! Angular is a full-fledged framework that comes with a lot of built-in features like dependency injection, a powerful CLI, and a structured way to build large enterprise apps. It’s more opinionated and comes with everything included.

On the other hand, I chose React because it’s more lightweight and flexible. React is just a library for building the UI, and it lets you choose other tools as you need them. It’s really popular, has a big community, and it’s easy to create reusable components. So basically, React gave me more flexibility and control over how I build my app."

## React optimization

lazy loading
pagination
avoid unnecessary re-renders

##  MERN stack, how MVC fits?
MVC separates application into Model, View, and Controller so that UI, business logic, and data handling stay independent.
Model → Mongoose models (User, Post, Order, etc.)

View → React frontend

Controller → Express route controllers handling APIs

## This bind ,aply , Call Back , Promise , 







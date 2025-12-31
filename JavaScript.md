| Feature   | var                              | let               | const            |
| --------- | -------------------------------- | ----------------- | ---------------- |
| Scope     | Function                         | Block             | Block            |


>"Sure! So basically, we usually prefer let and const over var. The reason is that let and const are block-scoped,
meaning they're confined to the block they're defined in. On the other hand, var is function-scoped, which can lead to unintended access outside of the block.
In other programming languages, variables are usually block-scoped by default, so relying on var can be a bit confusing in JavaScript. That's why we stick to let and const. And with const,
we can set a variable that can't be reassigned, making it really useful for fixed values.


***Hoisting***
>"Hoisting in JavaScript means all declarations are moved to the top of their scope before execution.
But only declarations are hoisted, not the actual values."

>var Example "If I use var, it gets hoisted and initialized as undefined."
console.log(a); // undefined
var a = 10;

>let / const Example
"let and const are also hoisted, but they stay in the Temporal Dead Zone, so accessing them early gives a ReferenceError."
console.log(x); // ReferenceError
let x = 5;

>Function Declaration
"Function declarations are fully hoisted, so I can call them before they appear."
sayHi(); // works
function sayHi() { console.log("Hello"); }

**JavaScript: Synchronous or Asynchronous?**
>"JavaScript is single-threaded and synchronous by default, but it can behave asynchronously because of the event loop
>"When an async task (like setTimeout or fetch) runs, the browser handles it in Web APIs.
Once it finishes, the callback goes to a queue.
The event loop waits for the call stack to be empty, then pushes the queued callback or promise into the stack to execute."
```javascript
console.log("1");

setTimeout(() => {
  console.log("2"); // goes to Web API → callback queue → event loop → call stack
}, 0);

console.log("3");
````
>opt  1 , 3 2

**Event Loop**
>"The Event Loop is the mechanism in JavaScript that allows it to handle asynchronous operations even though it is single-threaded."
>Call Stack → runs your normal code
Web APIs → handle timers, API calls, DOM events
Callback Queue / Microtask Queue → stores finished async tasks
Event Loop → pushes tasks from queue to call stack when stack is empty

**Call Back**
>"A callback is a function that is passed as an argument to another function and gets executed later, usually after some operation is completed."
"Callbacks are used to handle asynchronous tasks like API calls, timers, and events."  ( outdated  , we use async await inted of this )

**Promise**
>"A Promise is an object in JavaScript that represents the result of an asynchronous operation.
It can be in three states: pending, fulfilled, or rejected."
"A Promise gives us a cleaner way to handle async operations without nested callbacks."

>pending – operation is running
fulfilled – operation succeeded
rejected – operation failed

```javascript
const myPromise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Data received");
  } else {
    reject("Error occurred");
  }
});

myPromise
  .then(result => console.log(result))   // success
  .catch(error => console.log(error))    // failure
  .finally(() => console.log("Done"));
````

**Async/Await**
>"Async/await is a cleaner way to handle Promises, especially during API calls.
It makes asynchronous code look synchronous, which makes it easier to read and understand."

>When I use async before a function, it returns a Promise.
Inside it, await pauses the execution until the API Promise resolves.
So the code looks like it is running line-by-line, just like synchronous code, but it’s still non-blocking."

```javascript
async function fetchUser() {
  try {
    // This line waits until the API gives a response
    const response = await fetch("https://api.example.com/user");

    // Converts response to JSON, also returns a Promise
    const data = await response.json();

    console.log("User Data:", data);
  } catch (error) {
    console.log("API Error:", error);
  }
}

fetchUser();
````
>"Here, fetch() returns a Promise.
When I use await, JavaScript pauses at that line until the Promise resolves.
But it does NOT block the entire thread — only this function waits.
This makes asynchronous API calls look like synchronous code, which is easier to understand and debug."


**slice() Method**

>slice() is used to extract a portion of an array.
It does not modify the original array.
Syntax: array.slice(start, end)
Example:
```javascript
let nums = [10, 20, 30, 40, 50];
let result = nums.slice(1, 4); ( string indx , ending indx )
console.log(result);      // [20, 30, 40]
console.log(nums);        // Original array unchanged
````
**splice() Method**

>splice() is used to add, remove, or replace elements.
It modifies the original array.
Syntax: array.splice(start, deleteCount, items...)

```javascript
let fruits = ["Apple", "Banana", "Mango", "Orange"];
fruits.splice(1, 2, "Grapes"); // ( string indx , length , replaced val )
console.log(fruits);  
// ["Apple", "Grapes", "Orange"]
```

# 📘 JavaScript Closures – Complete Notes (with Real-World Example)

---

## 🔹 What is a Closure?

A **closure** is formed when an inner function remembers and can access variables from its outer function **even after the outer function has finished executing**.


## 🔹 What Pattern Is This? (Factory + Module Pattern)

The code example uses two patterns together:

### ✅ **Factory Function**
A function that *creates and returns* an object.

### ✅ **Module Pattern**
A way to:
- Expose **public methods**
- Hide **private variables**  
(using closures)

This pattern is widely used in real-world JavaScript apps (Node.js, backend services, authentication modules).

---


### ✔️ YES — closures + module pattern give **encapsulation**.


# 💻 Real-World Closure Example (User Profile – Private Password)

```javascript
// Factory + Module Pattern using Closure
function createUser(name, email, password) {
  let userPassword = password;  // ❗ private variable (encapsulated)

  return {
    getName: function () {
      return name;  // public method
    },

    getEmail: function () {
      return email;  // public method
    },

    checkPassword: function (inputPassword) {
      // Only accessible due to closure
      return inputPassword === userPassword;
    },

    updatePassword: function (newPass) {
      userPassword = newPass; // secure update
      console.log("Password updated");
    }
  };
}

// -------------------------------------------
// USAGE
// -------------------------------------------

const user = createUser("Tanmoy", "tanmoy@example.com", "secret123");

// Public methods
console.log(user.getName());    // Tanmoy
console.log(user.getEmail());   // tanmoy@example.com

// Direct access to password is blocked ❌
console.log(user.userPassword); // undefined

// Secure password check
console.log(user.checkPassword("wrong"));      // false
console.log(user.checkPassword("secret123"));  // true

// Update password securely
user.updatePassword("newPass456");
console.log(user.checkPassword("newPass456")); // true
````

# 📘 forEach vs map vs filter — Short Interview Notes

## 🔹 forEach()
**Definition:**  
Used to *iterate* over an array and perform actions.  
Does **not** return a new array.

**Use case:** Logging, API calls, updating external variables.

```javascript
const nums = [1, 2, 3];
nums.forEach(n => console.log(n));
// 1 2 3
```
## 🔹 map()

**Definition:**  
Creates a new array by transforming each element.

Use case: Modify data → return new array.
```javascript
const nums = [1, 2, 3];
const doubled = nums.map(n => n * 2);
console.log(doubled); // [2, 4, 6]
```

##🔹 filter()

**Definition:**  
Returns a new array with elements that pass a condition.

Use case: Select or remove items.
```javascript
const nums = [1, 2, 3, 4, 5];
const evens = nums.filter(n => n % 2 === 0);
console.log(evens); // [2, 4]
```


```js
for Loop

"Use the for loop for maximum control and early exit when you need to break out of the loop as soon as a condition is met."

2. forEach

"Use forEach when you want to perform side effects on each element but cannot break early, and you don’t need a return value."

3. map

"Use map to transform each element of an array and return a new array with the transformed values."

4. filter

"Use filter to create a new array of elements that match a specific condition, returning all matching items."

5. find

"Use find to get the first element that matches a condition, stopping as soon as it finds a match."

6. some

"Use some to check if at least one element in the array meets the condition, returning a boolean value."

7. Reduce
"reduce() is used to convert an array into a single value like a sum, object, or another array using an accumulator

array.reduce((accumulator, currentValue) => {
  // combine and return
}, initialValue);

Sample Array of Objects
const employees = [
  { empId: 101, name: "Alice" },
  { empId: 123, name: "Bob" },
  { empId: 150, name: "Charlie" }
];

1. for Loop
// Using for loop to find employee with empId = 123
let foundEmployee = null;
for (let i = 0; i < employees.length; i++) {
  if (employees[i].empId === 123) {
    foundEmployee = employees[i];
    break; // Can exit early
  }
}
// foundEmployee will be { empId: 123, name: "Bob" }


Features:

Early exit: Can stop the loop as soon as the condition is met.

Flexible: Can perform complex logic.

2. forEach
// Using forEach to find employee with empId = 123
let foundEmployee = null;
employees.forEach(emp => {
  if (emp.empId === 123) {
    foundEmployee = emp;
    // Cannot break early, so it will continue to iterate over all elements
  }
});
// foundEmployee will be { empId: 123, name: "Bob" }


Features:

No early exit: Will iterate over every element.

No return value: Used mainly for side effects.

3. map
// Using map to create a new array of employee names
const namesArray = employees.map(emp => emp.name);
// namesArray will be ["Alice", "Bob", "Charlie"]


Features:

Returns a new array: Transforms each element.

No early exit: Always processes every element.

4. filter
// Using filter to get employees with empId = 123
const filteredEmployees = employees.filter(emp => emp.empId === 123);
// filteredEmployees will be [{ empId: 123, name: "Bob" }]


Features:

Returns a new array: All matching elements.

No early exit: Checks every element.

5. find
// Using find to get the first employee with empId = 123
const found = employees.find(emp => emp.empId === 123);
// found will be { empId: 123, name: "Bob" }


Features:

Early exit: Stops as soon as a match is found.

Returns the first matching element or undefined.

6. some
// Using some to check if any employee has empId = 123
const has123 = employees.some(emp => emp.empId === 123);
// has123 will be true


Features:

Early exit: Stops as soon as a match is found.

Returns a boolean: true if a match is found, otherwise false.

Example 1 — Sum of array elements
const numbers = [1, 2, 3, 4];

const sum = numbers.reduce((acc, curr) => acc + curr, 0);  // arr.reduce((acc,c)=> {return acc = acc+cur},0)

// sum = 10

Example 2 — Frequency of each element
const letters = ["a", "b", "a", "c", "b", "a"];

const frequency = letters.reduce((acc, curr) => {
  acc[curr] = (acc[curr] || 0) + 1;
  return acc;
}, {});

// frequency = { a: 3, b: 2, c: 1 }
```

```js
async function readFileExample() {
  try {
    const data = await fs.readFile('example.txt', 'utf8');  //UTF-8 is a character encoding that converts text into bytes and supports all Unicode characters, commonly used to correctly read and write text files.
    console.log('File content:', data);
  } catch (err) {
    console.error('Error reading file:', err);
  }
}

readFileExample();


```


# Local Storage vs Session Storage vs Cookies

## 🔹 One-line explanations

- **Local Storage:** Stores data in the browser **without expiration**, persists even after closing the browser.
- **Session Storage:** Stores data in the browser **for a single tab session** and is cleared when the tab is closed.
- **Cookies:** Small pieces of data that are **automatically sent to the server with each request** and can have an expiration time.

---

## 📊 Comparison Table

| Feature | Local Storage | Session Storage | Cookies |
|--------|----------------|-----------------|---------|
| Basic Idea | Persistent browser storage | Tab-based temporary storage | Small data sent to server |
| Expiry | No automatic expiry | Cleared on tab close | Can be set manually |
| Size Limit | ~5–10 MB | ~5 MB | ~4 KB |
| Accessible from JavaScript | Yes | Yes | Yes (except HttpOnly) |
| Automatically Sent to Server | No | No | Yes |
| Storage Location | Browser | Browser | Browser + HTTP headers |
| Security Risk | XSS | XSS | XSS + CSRF |
| Typical Use | dark/lght mode , cached data | otp ver , | Authentication/session tokens |

---

## What is REST?

REST stands for Representational State Transfer. It is an architectural style for designing networked applications. RESTful APIs allow communication between client and server using standard HTTP methods like GET, POST, PUT, DELETE, and PATCH. Data is typically transferred in JSON or XML format.

## Key points to mention:

-REST is stateless Independent , meaning each request from the client to the server must contain all the information the server needs to fulfill that request.

-RESTful services are resource-based, and each resource is identified by a URL.

-The use of standard HTTP methods makes REST simple and widely adopted.

## HTTP

HTTP is indeed the protocol that facilitates data exchange between the client and the server.

It’s stateless, meaning each request is independent and doesn’t rely on any previous requests.

It uses standard methods like GET, POST, PUT, and DELETE to perform different operations.


## What Are HTTP Status Codes?

HTTP status codes are standard response codes given by web servers to indicate the outcome of a client's request. They are grouped into ranges to indicate the general type of response.

- **200s**: Success – The request was received, understood, and accepted.
- **400s**: Client Errors – The request had bad syntax or cannot be fulfilled by the server.
- **500s**: Server Errors – The server failed to fulfill an apparently valid request.

## HTTP vs HTTPS

HTTP (Hypertext Transfer Protocol) is the standard protocol for transferring data between a web browser and a server.

HTTPS (Hypertext Transfer Protocol Secure) is the secure version of HTTP. It uses encryption (SSL/TLS) to protect the data being transferred.

## Key Differences:

Security: HTTPS encrypts the data, making it secure from eavesdropping and tampering, while HTTP does not.

Port: HTTP typically uses port 80, whereas HTTPS uses port 443.

----

SSL stands for Secure Sockets Layer.
TLS stands for Transport Layer Security.

## Common HTTP Status Codes

| Status Code | Description                             |
|--------------|----------------------------------|
| 200          | OK – The request has succeeded.     |
| 201          | Created – The request has been fulfilled and resulted in a new resource being created. |
| 204          | No Content – The server successfully processed the request and is not returning any content. |
| 400          | Bad Request – The server could not understand the request due to invalid syntax. |
| 401          | Unauthorized – The request requires user authentication. |
| 403          | Forbidden – The server understood the request, but it refuses to authorize it. |
| 404          | Not Found – The server cannot find the requested resource. |
| 500          | Internal Server Error – A generic error message when the server encounters an unexpected condition. |
| 502          | Bad Gateway – The server, while acting as a gateway or proxy, received an invalid response from the upstream server. |
| 503          | Service Unavailable – The server is not ready to handle the request, often due to being overloaded or down for maintenance. |

---
CommonJS uses require and module.exports
✔️ common in older Node.js apps

ES Modules (ESM) use import and export
✔️ standard in modern JavaScript & React

## HTML and HTML5
✔️ Semantic tags — <header>, <footer>, <section>, <article>
✔️ Multimedia support — <video>, <audio>, <canvas>
✔️ Web storage — localStorage, sessionStorage
✔️ APIs — Geolocation, Offline storage
✔️ Form enhancements — new input types like email, date, tel
✔️ Better performance & mobile support






| Feature   | var                              | let               | const            |
| --------- | -------------------------------- | ----------------- | ---------------- |
| Scope     | Function                         | Block             | Block            |
| Hoisting  | Yes (initialized as `undefined`) | Yes (TDZ)         | Yes (TDZ)        |
| Redeclare | ✔ Yes                            | ❌ No              | ❌ No             |
| Reassign  | ✔ Yes                            | ✔ Yes             | ❌ No             |
| Best Use  | Avoid                            | Mutable variables | Fixed references |

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
```
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

```
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

````
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
```
let nums = [10, 20, 30, 40, 50];
let result = nums.slice(1, 4);
console.log(result);      // [20, 30, 40]
console.log(nums);        // Original array unchanged
````
**splice() Method**

>splice() is used to add, remove, or replace elements.
It modifies the original array.
Syntax: array.splice(start, deleteCount, items...)
```
let fruits = ["Apple", "Banana", "Mango", "Orange"];
fruits.splice(1, 2, "Grapes");
console.log(fruits);  
// ["Apple", "Grapes", "Orange"]
```

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

## This bind ,aply , Call Back , Promise , 

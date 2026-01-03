
```jsx
Link

    Purpose: Used for standard navigation between pages without a page refresh.

    Use Case: Logos, "Read More" buttons, Footer links, Cards.

    Key Feature: Does not know if it is currently active.

JavaScript

import { Link } from 'react-router-dom';

// Simple navigation
<Link to="/profile">Go to Profile</Link>

2. NavLink

    Purpose: A special version of Link that knows when it is "active" (when the URL matches the to prop).

    Use Case: Navigation Bars, Sidebar menus, Tabs.

    Key Feature: Exposes an isActive boolean for styling.

JavaScript

import { NavLink } from 'react-router-dom';

// Adds 'active-style' class only when on the About page
<NavLink 
  to="/about" 
  className={({ isActive }) => isActive ? "font-bold text-red-500" : ""}
>
  About
</NavLink>

One-line difference for Interview:

    "I use Link for general navigation and NavLink specifically for menus where I need to highlight the current page to the user."
````

```jsx
Ah, understood. So for a standard dropdown list, you'd use the <select> tag with <option>:
HTML

<label for="standard-select">Choose an option:</label>
<select id="standard-select" name="standard-select">
  <option value="option1">Option 1</option>
  <option value="option2">Option 2</option>
  <option value="option3">Option 3</option>
</select>

And for the input with datalist, as mentioned earlier:
HTML

<label for="datalist-input">Type or select:</label>
<input list="data-options" id="datalist-input" name="datalist-input" />
<datalist id="data-options">
  <option value="Suggestion 1"></option>
  <option value="Suggestion 2"></option>
  <option value="Suggestion 3"></option>
</datalist>
````







✅ Common “Design on Paper” Questions
1️⃣ Design a simple login page

Include:

email / username

password

forgot password

login button

signup link

Follow-ups they may ask:

how will you center it?

mobile responsive layout?

accessibility labels?

2️⃣ Design a registration form

Fields like:

Name

Email

Mobile

Password

Confirm password

Gender

Date of birth

Submit & Reset

Follow-ups:

how to validate?

client vs server validation

password strength indicator?

3️⃣ Design a shopping website product card

Draw:

product image

title

price

rating stars

add to cart button

wishlist icon

Follow-ups:

grid vs flexbox?

hover effects?

responsiveness?

4️⃣ Design a dashboard layout

Sections:

sidebar

top navbar

main content

cards with stats

chart placeholder

They may ask:

CSS Grid vs Flexbox choice

Which component reusable?

5️⃣ Design a portfolio homepage

Include:

navbar

hero section with image + text

about section

projects grid

contact form

footer

6️⃣ Design a blog page

Draw:

navbar

list of blog cards

title + image + short description

pagination buttons

7️⃣ Design a chat UI

Include:

sidebar with users

chat messages area

input + send button

Follow-ups:

message alignment (self vs others)

scroll behavior (overflow)

sticky header/footer

8️⃣ Design a responsive navbar

Must contain:

logo

menu items

hamburger icon for mobile

Follow-ups:

flex vs grid?

breakpoints?

sticky nav?

9️⃣ Design a landing page hero section

Elements:

heading

sub-heading

call to action

illustration image

They may ask:

center vertically?

background image vs gradient?

above-the-fold content?

🔟 Design an e-commerce checkout page

Draw sections:

delivery address

order summary

payment options

price details

🎯 Very TCS-Digital Specific Paper Questions

They may also ask:

draw layout of Spotify / Netflix / Zomato home page

wireframe Railway ticket booking UI

design Admin panel user table

sketch Kanban board (like Trello)

wireframe online exam screen layout

design OTP verification screen

design 404 error page


1) “Design a responsive navbar”

They may say:

logo on the left

menu items on right

hamburger menu in mobile

highlight active item

Follow-ups:

flexbox vs grid?

sticky navbar?

aria-labels for accessibility?

🔹 2) “Create a sidebar layout”

They might ask:

collapsible sidebar

icons + text

content area on right

sticky/scrollable behavior

Sometimes:

👉 “Sidebar collapses on small screen”

🔹 3) “Dashboard layout”

Very common in TCS Digital.

They will want:

sidebar

top navbar

cards with metrics

content panel

maybe chart placeholder

🔹 4) “Navbar + Sidebar together”

Admin-panel type layout.

🔹 5) “Sticky header while scrolling”

They test position: sticky / fixed understanding.

🔹 6) “Build navbar without Bootstrap”

They check raw CSS skills, not dependency usage.

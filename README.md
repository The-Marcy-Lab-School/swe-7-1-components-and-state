# Multilingual Lab

**Table Of Contents**
- [Assignment Overview](#assignment-overview)
  - [Set Up](#set-up)
- [Tech Checklist (21 points + 2 bonus points)](#tech-checklist-21-points--2-bonus-points)
- [Tips: Proper Coding Style In React](#tips-proper-coding-style-in-react)
  - [Thinking About State and Component Structure](#thinking-about-state-and-component-structure)
  - [Rendering a List from an Array](#rendering-a-list-from-an-array)
- [Challenges](#challenges)
  - [Optional: File Organization Challenge](#optional-file-organization-challenge)
  - [Optional: Styling Challenge](#optional-styling-challenge)

## Assignment Overview

In this assignment, you will build an interactive greeting app that lets users translate a greeting into different languages, adjust the font size, toggle a dark/light theme, and track a history of their language selections.

You will divide your UI into multiple components, each managing or receiving state, and will practice lifting state up to parent components and passing handlers down as props.

In this project, you will practice:
- Creating a React project using Vite
- Creating functional components and rendering HTML using JSX
- Managing state with multiple `useState` instances
- Lifting state up and passing handlers as props
- Handling events in React and updating state accordingly
- Rendering lists from state arrays

### Set Up

For guidance on setting up and submitting this assignment, refer to the Marcy Lab School Docs How-To guide for [Working with Short Response and Coding Assignments](https://marcylabschool.gitbook.io/marcy-lab-school-docs/how-tos/working-with-assignments#how-to-work-on-assignments).

After cloning your repository, make sure to create a `draft` branch:

```
git checkout -b draft
```

Then, use [Vite](https://vitejs.dev/guide/) to create your starter code. You can run these commands to get started:

```sh
npm create vite
# Press "Enter" to select the default project name `vite-project`
# Select React
# Select JavaScript

cd app
npm i
npm run dev
```

Open up the `App.jsx` file and remove the starter app. Then get started building!

## Tech Checklist (21 points + 2 bonus points)

Starting in this unit, we will be using a different form of grading. Rather than running automated tests, we will be testing your application as a user would. We'll run your application and see what features your app can do. Your score will be determined based on the number of completed requirements listed below.

There are 21 tasks to complete and 2 bonuses.

Your goal is to meet at least 75% of these requirements to complete the assignment. But don't stop there — shoot for 100%!

**Greeting & Language Buttons** (4 points)
- [ ] Your app renders a greeting (e.g., "Good Morning") in English by default
- [ ] Beneath the greeting there are five buttons, each for a different language (e.g., Spanish, Haitian Creole, Portuguese, French, Japanese)
- [ ] Clicking a language button translates the greeting to the appropriate language (hint: use an object to map each language to its translation!)
- [ ] When a language button is active (selected), it is visually distinguished from the others (e.g., a different background color or border)

**Size Buttons** (3 points)
- [ ] Above the greeting there are two buttons to increase and decrease the font size
- [ ] Clicking the buttons changes the greeting's font size accordingly
- [ ] The font size cannot go below a minimum value (e.g., 12px) or above a maximum value (e.g., 72px)

**Theme Toggle** (3 points)
- [ ] There is a button to toggle between a light theme (white background, black text) and a dark theme (black background, white text)
- [ ] When toggled, the background color and text color of the whole app change appropriately
- [ ] The button label updates to reflect the current theme (e.g., "Switch to Dark Mode" / "Switch to Light Mode")

**Click History** (4 points)
- [ ] Below the language buttons there is a "History" section
- [ ] Every time a language button is clicked, the name of that language is added to the top of a history list
- [ ] The history list displays the last 5 language selections (older entries are dropped when the list exceeds 5)
- [ ] There is a "Clear History" button that empties the history list

**React Fundamentals** (4 points)
- [ ] Component names use PascalCase (`MyComponent` instead of `myComponent`)
- [ ] Props are extracted in child components using destructuring
- [ ] `useState` is used to manage state in at least 4 separate state variables
- [ ] State that is shared between sibling components is lifted up to the nearest common parent

**Miscellaneous** (3 points)
- [ ] Used Vite to create the project
- [ ] The size buttons, greeting, language buttons, theme toggle, and click history are each their own component for a total of 5 components (plus the root `App`)
- [ ] At no point did you use any vanilla DOM JS methods (e.g., `document.querySelector` or `document.createElement`)

**Bonus** (2 points)
- [ ] Bonus: You have a `components` directory. Each component has its own file and is exported (1 export per file). The filename matches the component name (`GreetingDisplay.jsx` exports a `GreetingDisplay` component).
- [ ] Bonus: Your project has extra CSS styling!

## Tips: Proper Coding Style In React

Below is an example of a Counter application written using React and JSX. It has `CounterButtons` that either increase or decrease a counter by 1 and a `CounterDisplay` to show the current count value.

Note the following:
* Each component is a function that returns JSX. Function names are in UpperCamelCase.
* Components that return multiple elements are surrounded by `()` and must have a top-level element, even if it is just `<> </>`
* Components can take in "props" — data passed down from parent components.
* Data is injected into JSX using `{}`
* Event handlers are set directly on the element (for example, using `onClick={handler}`)

```jsx
// useState is imported at the top
import { useState } from 'react';

// component names use PascalCase
const CounterDisplay = ({ count }) => {
  return <p>{count}</p>
}

const CounterButtons = ({ increment, decrement }) => {
  // props are destructured ^          ^

  // multiple returned components are wrapped with () and <> </>
  return (
    <>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </>
  )
}

// App is exported (default or named is fine)
export const App = () => {
  // state is "lifted up" and passed down with props
  const [count, setCount] = useState(0);

  // helper functions can be passed down instead of the setter function itself
  const increment = () => { setCount(count + 1) }
  const decrement = () => { setCount(count - 1) }

  return (
    <>
      <CounterDisplay count={count} />
      <CounterButtons increment={increment} decrement={decrement} />
    </>
  )
}
```

### Thinking About State and Component Structure

Before you code, think about which component "owns" each piece of state:
- Which components need to read a value? Which components need to change it?
- If two sibling components both need the same state, **lift it up** to their common parent.
- Pass the setter (or a handler that wraps it) down as a prop to the child that triggers the change.

For example, `App` should own the `language` state because both `GreetingDisplay` (reads it to show the right greeting) and `LanguageButtons` (reads it to highlight the active button) need access to it.

### Rendering a List from an Array

When your state is an array, use `.map()` to render each item. Remember to add a unique `key` to each item. The key can be the index of the item in the array.

```jsx
const ThingsList = ({ things }) => {
  return (
    <div>
      <h3>List of Things</h3>
      <ul>
        {things.map((thing, index) => (
          <li key={index}>{thing}</li>
        ))}
      </ul>
    </div>
  )
}
```

## Challenges

### Optional: File Organization Challenge

Your app should utilize multiple components to organize its content. But maybe you put all of your components in the `App.jsx` file? This is honestly fine for a project of this size, but as projects grow you'll need a more organized solution.

Here is what we recommend:
* Create a `components` folder inside `src`
* For each component, create a file named `MyComponentName.jsx` (note the capitalized filename — it should match the component being exported)
* Each component file should export a single component as the default export
* In `App.jsx`, import your components. You shouldn't need to change the `App` component itself.

### Optional: Styling Challenge

Ready to add some pizzazz? Add CSS styling to your app!

Check out this article from FreeCodeCamp on [styling React apps with CSS](https://www.freecodecamp.org/news/style-react-apps-with-css/).

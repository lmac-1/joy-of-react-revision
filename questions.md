# Module 1

## Hello React
1. When we import React dependencies for the web it looks like this:
    
    ```
    import React from 'react'; // core React library from react
    import { createRoot } from 'react-dom/client'; // function from react-dom
    ```
    
    Why are there 2 separate packages?
    
2. What 3 arguments does `React.createElement` take?
3. What does DOM stand for?

<details>
<summary><strong>Answers</strong></summary>

1. React is platform agnostic. There are different packages for different platforms. Each platform has its own “primitives” (eg web has div, p button, React Native has Text, View and Pressable)
2. (1) the type of element we want to create, (2) the properties we want this element to have, (3) the element’s contents (what the element should have as children)
`const element = React.createElement(
  'p',
  { id: 'hello' },
  'Hello World!'
);`
3. Document Object Model
</details>

## Understanding JSX
1. Why do we have to compile our JSX code into JavaScript? What does it get converted to?
2. How can you add whitespace in JSX?

<details>
<summary><strong>Answers</strong></summary>

1. JSX cannot be run in the browser. It’s usually compiled as part of a build step using a tool like Babel. The JSX we write gets converted into `React.createElement`
2. `{" "}`
</details>

# Module 2

## Component Instance
1. What is a component instance?
2. What two steps happen when mounting a component?
3. What happens when we unmount a component? What happens to its state?

<details>
<summary><strong>Answers</strong></summary>
1. a long lived object that holds contextual information about this particular instance of the component
2. (1) We evaluate the returned JSX and pass it onto React, so that React can create the corresponding DOM elements
(2) We create a component instance, a long-lived object that holds contextual information about this particular instance of the component
3. We destroy the component instance and **any stored state is lost forever**

</details>

## State Management Tools
1. Why was Redux created? What problem was it solving?
2. What does Redux do?
3. What are some negatives about using Redux? 
4. Who created Redux?

# Module 3

## General
1. What do hooks help us do? (clue: business logic)

<details>
<summary><strong>Answers</strong></summary>
Help us to package up and re-use business logic in our application
</details>

## `useId`
1. Can you give an example of where you should use the `useId` hook?
2. What happens to `useId` on re-renders? 
3. Is the value of `useId` the same across server and client renders?

<details>
<summary><strong>Answers</strong></summary>
1. A form component that has different inputs. For example, a `LoginForm` component with a username and password field. You can create an id with `useId` and then use that to generate ids for the two inputs: 
    
    ```jsx
    // Pluck this instance's unique ID from React
    const id = React.useId();
    
    // Create element IDs using this unique ID
    const usernameId = `${id}-username`;
    const passwordId = `${id}-password`;
    ```
    
2. The value stays the same. The value persists.
3. Yes. `useId` produces the same value across server and client renders.
</details>

## Rules of Hooks
1. What are the 2 “Rules of Hooks” that we should learn?
2. (follow on question) What’s the exception to the 1st rule?
3. Can you call hooks conditionally? Why or why not? (explaining simply)
4. How can we correct this code?

    ```jsx
    function TextInput({ id, label, type }) {
      let appliedId = id;
    
      if (typeof appliedId === 'undefined') {
        appliedId = React.useId();
      }
    
      return (
        <div className="text-input">
          <label htmlFor={appliedId}>
            {label}
          </label>
          <input
            id={appliedId}
            type={type}
          />
        </div>
      );
    }
    ```

<details>
<summary><strong>Answers</strong></summary>
  
1. (1) Hooks have to be called within the scope of a React application. We can’t call them outside of our React components. (2) We have to call our hooks at the top level of a component

2. Hooks can be called within custom hooks

3. No. Hooks cannot be called conditionally. React uses the order of the function calls to figure out which state to provide to each hook. If we render a component 100 times, it should call the exact same hooks in the exact same order. When we render a hook conditionally, we make it possible for the hooks to change from one render to another. 

4. You should make sure you are calling `useId` first, and then checking for an id. 

      ```jsx
      function TextInput({ id, label, type }) {
        // change the order of when you call useId, it should be first
      	let generatedId = React.useId();
        let appliedId = id || generatedId;
      
        return (
          <div className="text-input">
            <label htmlFor={appliedId}>
              {label}
            </label>
            <input
              id={appliedId}
              type={type}
            />
          </div>
        );
      }
      ```
</details>

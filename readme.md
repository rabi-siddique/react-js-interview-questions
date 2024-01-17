# What does `create-react-app` do?
It is a command line tool that helps create a boilerplate react project with the pre-configured development environment. 

This includes the build setup using module bundlers like Webpack and a development server for testing the application locally. 

It also sets up a testing environment using Jest and provides the necessary configurations for writing and executing tests for your React components.

This helps developers to get started quickly in building their applications instead of spending time on configuring setups for the build. 

# What's the purpose of binding in class components?
The binding step is necessary because class methods don’t automatically bind this to the class instance. Let’s demonstrate it with the help of the following ES6 class component:

```js
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
```
The component renders just fine, but when you click the button, you see undefined in your developer console log. This is one of the main sources of bugs developers encounter in React. If you want to access this.state in your class method, it cannot be retrieved because this is undefined. To make this accessible in your class methods, you have to bind the class methods to this. In the following class component the class method is properly bound in the class constructor:

```js
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.onClickMe = this.onClickMe.bind(this);
  }

  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
```

Some things to avoid would be to stop binding in the render method. Because in that case, binding happens every time the render() method runs, meaning every time the component updates, which will hurt your application’s performance eventually. Binding the class method in the constructor need only be done once, when the component is instantiated.

Some developers will define the business logic of their class methods in the constructor. Avoid this approach as well, as it will clutter your constructor over time. The constructor is only there to instantiate your class with all its properties, so the business logic of class methods should be defined outside the constructor.

Class methods can be auto-bound using JavaScript ES6 arrow functions:
```js
class ExplainBindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
```
Use this method if the repetitive binding in the constructor annoys you.

# What's the difference between Controlled and UnControlled Components?
Controlled components get their data from a parent, using props. The state is typically handled by the parent component, and any changes to the state are passed down to the child component via props. On the other hand, Uncontrolled components do their own thing. They take care of their own data/state without relying on a parent to manage it.

Since the parent manages their state, controlled components are more predictable. Data flow is controlled and consistent, making it simpler to debug and trace issues. As changes in the component's state are initiated and directed by the parent. A downside with Controlled components would be that it involve more code to handle the flow of data between parent and child components. 

# What are Functional Stateless Components?
These components are stateless, meaning they don't manage local state within the component. They solely rely on the input provided through props to determine their behavior and render content. Functional Stateless Components are functions that take input and return an output. The inputs are the props, and the output is a component instance in plain JSX. They have no local state (stateless). You cannot access or update the state.

# When does constructor method and render method runs in the context of class methods?
The constructor runs only once in the lifetime of a component, whereas the render() class method runs once at the beginning and every time the component updates. 

# What's the difference between Named & Default Exports?
In JavaScript ES6, you can import and export functionalities from modules. These can be functions, classes, components, constants, essentially anything you can assign to a variable. The act of exporting one or multiple variables is called a named export:
```js
// file1
const firstname = 'Rabi';
const lastname = 'Siddique';

export { firstname, lastname };
```

And import them in another file with a relative path to the first file.
```js
// file2.js
import { firstname, lastname } from './file1.js';
```

On the other hand, default statement can be used to export and import a single functionality.

```js
// file1.js
const person = {
  firstname: 'Rabi',
  lastname: 'Siddique',
};

export default person;
```

You have to leave out the curly braces to import the default export. Also the import name can differ from the exported default name.
```js
import rabi from './file1.js';
```
# What is One-Way Data Flow in React and How it useful?
React follows one way data flow. That means I can pass data from parent component to the child components. But, I cannot pass data from child components to parent components. 

If I have an issue in the parent component. I know that it's because of the parent component and not the child component. Because, the child components cannot mess with it's parent component. Basically, it makes debugging in React straight-forward.

# What is TreeShaking?
Tree shaking is the optimization process that selectively includes only the essential, or live code in a final bundle. This technique ensures that the generated bundle contains only the code that is actively utilized in your project.

For example, I make the import:
```js
import ReactDOM from "react-dom";

```
In this initial import, the entire react-dom library is included. However, with tree shaking, you can optimize further by importing only the specific component you require:
```js
import { createRoot } from "react-dom";

```
This is better because when the final bundle is created, only the implemenation of createRoot is includeded. And not the entire ReactDOM.

# Why Hooks should be called at the top level of your React function?
In React, Hooks should be called at the top of your functional component. We should not place them inside conditionals, loops or nested functions. But why? This is because React keeps track of the order by which these hooks are called. Using this order, React is able to maintain state values between re-renders. If you mix up the order of these steps, React gets confused, and it can't keep the data in order. 

For example, consider this component:
```js
function Form() {
  // 1. Use the name state variable
  const [name, setName] = useState('Mary');

  // 2. Use an effect for persisting the form
  useEffect(function persistForm() {
    localStorage.setItem('formData', name);
  });

  // 3. Use the surname state variable
  const [surname, setSurname] = useState('Poppins');

  // 4. Use an effect for updating the title
  useEffect(function updateTitle() {
    document.title = name + ' ' + surname;
  });

  // ...
}

```

How does React know which state corresponds to which useState call? The answer is that React relies on the order in which Hooks are called. Above example works because the order of the Hook calls is the same on every render:
```js
// ------------
// First render
// ------------
useState('Mary')           // 1. Initialize the name state variable with 'Mary'
useEffect(persistForm)     // 2. Add an effect for persisting the form
useState('Poppins')        // 3. Initialize the surname state variable with 'Poppins'
useEffect(updateTitle)     // 4. Add an effect for updating the title

// -------------
// Second render
// -------------
useState('Mary')           // 1. Read the name state variable (argument is ignored)
useEffect(persistForm)     // 2. Replace the effect for persisting the form
useState('Poppins')        // 3. Read the surname state variable (argument is ignored)
useEffect(updateTitle)     // 4. Replace the effect for updating the title

// ...

```

As long as the order of the Hook calls is the same between renders, React can associate some local state with each of them. But what happens if we put a Hook call inside a condition?
```js
 // 🔴 We're breaking the first rule by using a Hook in a condition
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }

```

The name !== '' condition is true on the first render, so we run this Hook. However, on the next render the user might clear the form, making the condition false. Now that we skip this Hook during rendering, the order of the Hook calls becomes different:
```js
useState('Mary')           // 1. Read the name state variable (argument is ignored)
// useEffect(persistForm)  // 🔴 This Hook was skipped!
useState('Poppins')        // 🔴 2 (but was 3). Fail to read the surname state variable
useEffect(updateTitle)     // 🔴 3 (but was 4). Fail to replace the effect
```

React wouldn’t know what to return for the second useState Hook call. React expected that the second Hook call in this component corresponds to the persistForm effect, just like during the previous render, but it doesn’t anymore. From that point, every next Hook call after the one we skipped would also shift by one, leading to bugs.

# Why Local Variables are not suitable for states?
There are two reasons why local reasons are not perfect to be used as states. 
1. Betweeen re-renders the values of local variables are not preserved. It is set to its original value. This is because when React renders a component, it renders it from scratch. 
2. Local variables dont trigger renders. Due to which we can't reflect the values of local variables on the screen in case they change.

# How does React handle multiple consecutive calls to the set function of the state in the same render cycle?
Within a render, the value of state remains the same. Also, when setting the state using `set` function, it changes it for the next render only. For example, consider this snippet:

```js
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

In this snippet, upon clicking the button, you might think the state is being updated 3 times, so in the next render, the value might be 3. Which is not the case. React updates states only for the next render. 
Which means no matter how many times you call `set` function, it will update state based on the current value for the very next render. Which in this will be 1. 

# What are Refs in React?
Refs are useful when we want a component to remember information without triggering renders on value changes. You can use refs by importing the `useRef` hook and pass it an initial value: 

```js
import { useRef } from 'react';
const ref = useRef(0);

// Returns a plain object:

/*

{ 
  current: 0 // The value you passed to useRef
}

*/
```

We can access the current value of the ref using `ref.current` property. This value is intentionally mutable, meaning you can both read and write to it.

# How Refs are different from State?
- `useRef(initialValue)` returns `{ current: initialValue }`. While `useState(initialValue)` returns the current value of a state variable and a state setter function `( [value, setValue])`.

- Refs doesn’t trigger re-render when you change it, unlike states. 

- Refs are mutable while state is immutable. 

- Refs are synchronous. State acts like a snapshot for every render and doesn’t update synchronously. But when you mutate the current value of a ref, it changes immediately:
```js
ref.current = 5;
console.log(ref.current); // 5
```

# When should we use Refs?
Refs are handy when you want to step outside of React and perhaps want to access to some external browser APIs. Some situations we can use refs:

- Storing timeout IDs.
- Manipulating and accessing DOM elements directly. 

Although, React automaitcally updates the DOM as a result we don't need to do it ourselves. However, there are times when we need access to a DOM element, for example to focus an input field, or getting the size of an element or scroll an element. React does not have any inbuilt method to do it. For this purpose, we can use of refs. Here is an example:

```js
import { useRef } from 'react';
const myRef = useRef(null);
<div ref={myRef}>
```
On the initial render, `myRef` contains `null` until when React creates the DOM and points `myRef.current` to the `div` containing it. We can now access this DOM node from our event handlers and use the built-in browser APIs defined on it. 

# What are Forward Refs in React?
You cannot attach refs to our own defined components. By default, React returns `null` in this case. And it is intentional behavior. You will in fact get this warning:

```bash
Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?
```
This is because by default, React does not allow component to access DOM nodes of another component, even if its a child component. Manually manipulating another component’s DOM nodes makes your code even more fragile.

To make sure, a component provides access to its DOM nodes, we have to make use of `forwardRef`. A component can specify that it `forwards` its ref to one of its children. For example:

```js
const MyInput = forwardRef((props, ref) => {
  return <input {...props} ref={ref} />;
});
```

# How many steps are involved in any React Screen Update?
Any screen update in a React app happens in three steps:
1. Trigger
2. Render
3. Commit

# How to Trigger a render in React?
A render is trigger in React only two times. These are:
1. Initial render of the component. It’s done by calling `createRoot` with the target DOM node, and then calling its render method with your component. Here is the code:

```js
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);

```
If we comment out this portion of the code, we will see a blank screen. 

2. A component's state is updated. When a component has been rendered initially we can trigger renders by updating state of the component using `set` function. 

# What is Rendering in React?
Rendering in React involves invoking functions. On the initial render its invoking the function of the root component. Subsequent renders invole invoking the function of the component whose state has been updated, along with any nested components.

During this rendering process, React assesses the properties of these components to identify changes. The goal is to determine the most efficient way to apply these changes to the actual DOM. However, this information is not immediately utilized; it is held until the commit phase.

# What is the Commit Phase in React?
After rendering our components, React will modify the DOM now. 

1. For the initial render, React will use the appendChild() DOM API to put all the DOM nodes it has created on screen.
2. For re-renders, React will apply the minimal necessary operations (calculated while rendering!) to make the DOM match the latest rendering output.

React only changes the DOM nodes if there’s a difference between renders. For example, Consider this component:
```js
export default function Clock({ time }) {
  return (
    <>
      <h1>{time}</h1>
      <input />
    </>
  );
}
```
It re-renders with different props passed from its parent every second. Notice how you can add some text into the `<input>`, updating its value, but the text doesn’t disappear when the component re-renders. This works because during this last step, React only updates the content of `<h1>` with the new time. It sees that the `<input>` appears in the JSX in the same place as last time, so React doesn’t touch the `<input>` or its value.

# What is Painting in React?
After rendering is done and React updated the DOM, the browser will repaint the screen. This process is known as `browser rendering`.


# What are Effects in React?
Effects let you run some code after rendering so that you can synchronize your component with some system outside of React Effects run at the end of a commit after the screen updates. An example of an effect is connecting to Chat server after the app loads, in case of a chat app. Which can done using the `useEffect` hock:

```js
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    // Code here will run after *every* render
  });
  return <div />;
}
```
Every time your component renders, React will update the screen and then run the code inside useEffect. **In other words, useEffect “delays” a piece of code from running until that render is reflected on the screen.**

Summary of declaration of `useEffect`:
```js
useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount *and also* if either a or b have changed since the last render
}, [a, b]);
```

# What are cleanup functions in useEffect?
Some Effects need to specify how to stop, undo, or clean up whatever they were doing. For example, `connect` needs `disconnect`, `subscribe` needs `unsubscribe`, and `fetch` needs either `cancel` or `ignore`. You do this by returning a cleanup function.

You’re writing a ChatRoom component that needs to connect to the chat server when it appears. You are given a `createConnection()` API that returns an object with `connect()` and `disconnect()` methods. Something like:

```js
function createConnection() {
  // A real implementation would actually connect to the server
  return {
    connect() {
      console.log('✅ Connecting...');
    },
    disconnect() {
      console.log('❌ Disconnected.');
    }
  };
}

useEffect(() => {
  const connection = createConnection();
  connection.connect();
}, []);
```

Imagine the `ChatRoom` component is a part of a larger app with many different screens. The user starts their journey on the `ChatRoom` page. The component mounts and calls `connection.connect()`. Then imagine the user navigates to another screen—for example, to the `Settings` page. The `ChatRoom` component unmounts. Finally, the user clicks `Back` and `ChatRoom` mounts again. This would set up a second connection—but the first connection was never destroyed! As the user navigates across the app, the connections would keep piling up.

To fix the issue, return a cleanup function from your Effect:

```js
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, []);
```
React will call your cleanup function each time before the Effect runs again, and one final time when the component unmounts (gets removed). Let’s see what happens when the cleanup function is implemented:


# Why does useEffect prints `✅ Connecting...` twice in code example below?
Code Example:
```js
function createConnection() {
  // A real implementation would actually connect to the server
  return {
    connect() {
      console.log('✅ Connecting...');
    },
    disconnect() {
      console.log('❌ Disconnected.');
    }
  };
}

useEffect(() => {
  const connection = createConnection();
  connection.connect();
}, []);
```

This Effect only runs on **mount**, so you might expect `✅ Connecting...` to be printed once in the console. However, if you check the console, `✅ Connecting...` gets printed twice. Why does it happen?

Seeing the `✅ Connecting...` log twice helps you notice the real issue: your code doesn’t close the connection when the component unmounts. Bugs like this are easy to miss without extensive manual testing. To help you spot them quickly, in development React remounts every component once immediately after its initial mount.

To fix the issue, return a cleanup function from your Effect:

```js
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, []);
```

Let’s see what happens when the cleanup function is implemented. Now you get three console logs in development:

1. `✅ Connecting...`
2. `❌ Disconnected.`
3. `✅ Connecting...`

This is the correct behavior in development. By remounting your component, React verifies that navigating away and back would not break your code. Disconnecting and then connecting again is exactly what should happen! When you implement the cleanup well, there should be no user-visible difference between running the Effect once vs running it, cleaning it up, and running it again. There’s an extra connect/disconnect call pair because React is probing your code for bugs in development. 

In production, you would only see `✅ Connecting...` printed once. Remounting components only happens in development to help you find Effects that need cleanup. You can turn off Strict Mode to opt out of the development behavior. 

# How to stop Event Propagation in React?
By default, events bubbles up or propagates in React. It starts with where the event happened, and then goes up the tree.

```js
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>
      <button onClick={() => alert('Uploading!')}>
        Upload Image
      </button>
    </div>
  );
}
```

In the example above, no matter which button we click, two callbacks will be called. One for the button we clicked and one for the `div`. To stop this, we use the `event` object received by event handlers. By convention, it’s usually called `e`, which stands for `event`. You can use this object to read information about the event. That event object also lets you stop the propagation. If you want to prevent an event from reaching parent components, you need to call `e.stopPropagation()` like this: Button component does:

```js
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={(e) => {
      e.stopPropagation();
      alert('You clicked on the toolbar!');
    }}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>
      <button onClick={(e) => {
        e.stopPropagation();
        alert('Uploading!')}}>
        Upload Image
      </button>
    </div>
  );
}
```

# How to prevent Page Reloads on Form Submission in React?
Some browser events have default behavior associated with them. For example, a `<form>` submit event, which happens when a button inside of it is clicked, will reload the whole page by default. You can call `e.preventDefault()` on the event object to stop this from happening:

```js
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>
    </form>
  );
}
```

# Functional vs Class Based Components

# What is JSX

# Why use key with Map

# React component cycle

# Dynamic ClassName

# Getting rid of the same network calls using map 
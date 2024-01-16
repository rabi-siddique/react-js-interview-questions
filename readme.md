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


# Functional vs Class Based Components

# What is JSX

# Normal variable vs useState variable

# Why use key with Map

# React component cycle

# Dynamic ClassName

# Getting rid of the same network calls using map 
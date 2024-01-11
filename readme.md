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
Class method binding can happen somewhere else too. For instance, it can happen in the render() class method:
```js
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe.bind(this)}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
```

Avoid this practice, however, because it binds the class method every time the render() method runs, meaning every time the component updates, which will hurt your application’s performance eventually. Binding the class method in the constructor need only be done once, when the component is instantiated.

Some developers will define the business logic of their class methods in the constructor:
```js
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.onClickMe = () => {
      console.log(this);
    }
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
Avoid this approach as well, as it will clutter your constructor over time. The constructor is only there to instantiate your class with all its properties, so the business logic of class methods should be defined outside the constructor.

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
Use this method if the repetitive binding in the constructor annoys you. The official React documentation sticks to the class method bindings in the constructor.

# Controlled vs UnControlled Components
In React, components can be categorized as either controlled or uncontrolled based on how they manage their data and state values.

## Controlled Components:
A controlled component in React relies on receiving its data and state values through props, which are managed by a parent component. This ensures a centralized and predictable flow of data, allowing the parent component to exert control over the state.

## Uncontrolled Components:
On the other hand, an uncontrolled component in React is one that internally handles its state. This means the component itself manages its own state without relying on external props for data updates.

## Understanding Uncontrolled Components:
For instance, an HTML input tag comes with a value attribute. The value attribute usually contains the value shown in the input field. Form elements such as <input>, <textarea>, and <select> hold their own state in plain HTML. They modify the value internally once someone changes it from the outside. In React, that’s called an uncontrolled component, because it handles its own state. We want to make sure those elements are controlled components instead.

# When does constructor method and render method runs in the context of class methods
The constructor runs only once in the lifetime of a component, whereas the render() class method runs once at the beginning and every time the component updates. 

# What are functional stateless components?
Functional Stateless Components are functions that take input and return an output. The inputs are the props, and the output is a component instance in plain JSX. They have no local state (stateless). You cannot access or update the state.


# Functional vs Class Based Components

# What is JSX

# Normal variable vs useState variable

# Ways of exporting

# Why use key with Map

# React component cycle

# Dynamic ClassName

# Getting rid of the same network calls using map 
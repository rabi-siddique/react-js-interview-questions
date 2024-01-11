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

# Functional vs Class Based Components

# What is JSX

# Normal variable vs useState variable

# Ways of exporting

# Why use key with Map

# React component cycle

# Dynamic ClassName

# Getting rid of the same network calls using map 
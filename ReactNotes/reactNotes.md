# React

React is a UI library developed at Facebook to facilitate the creation of interactive, stateful & reusable UI components. It is used at Facebook in production, and Instagram is written entirely in React.

React uses virtual DOM based mechanism to fill in data (views) in HTML DOM. The virtual DOM works fast owning to the fact that it only changes individual DOM elements instead of reloading complete DOM every time.

By doing most of the processing inside the virtual DOM rather than directly in the browser's DOM, React can act quickly and only add, update, and remove components which have changed since the last render cycle occurred.

### How Virtual DOM works or what are it's advantages ?
since the virtual DOM is a JavaScript representation of the actual DOM, React can keep track of the difference between the current virtual DOM(computed after some data changes), with the previous virtual DOM (computed befores some data changes). 

React then isolates the changes between the old and new virtual DOM and then only updates the real DOM with the necessary changes. 

In more layman’s terms, because manipulating the actual DOM is slow, React is able to minimize manipulations to the actual DOM by keeping track of a virtual DOM and only updating the real DOM when necessary and with only the necessary changes.

Typically UI’s have lots of state which makes managing state difficult. By re-rendering the virtual DOM every time any state change occurs, React makes it easier to think about what state your application is in. 

The process looks something like this:
**Some user event which changes the state of your app → Re-render virtual DOM -> Diff previous virtual DOM with new virtual DOM -> Only update real DOM with necessary changes.**

## Components
React is all about components. Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

### 3 ways to create Components in React

#### 1. Stateless Components (functions):

```javascript
const FirstComponent = props => (
    <div>{props.content}</div>
);
```

Stateless components are getting their philosophy from **_functional programming._** Which implies that: A function returns all time the same thing exactly on what is given to it.

They are side-effect free and will create the same behaviour always **(Idempotent)**.

They also do not require internal state management.

Another example of Stateless component:

```javascript
// When using JSX inside a module you must import React
import React from 'react';
import PropTypes from 'prop-types';

const FirstComponent = props => (
    <div>
        Hello, {props.name}! I am a FirstComponent.
    </div>
);

//arrow components also may have props validation
FirstComponent.propTypes = {
    name: PropTypes.string.isRequired,
}

// To use FirstComponent in another file it must be exposed through an export call:
export default FirstComponent;
```

#### 2. React.createClass()

```javascript
const SecondComponent = React.createClass({
    render: function () {
        return (
            <div>{this.props.content}</div>
        );
    }
});
```

#### 3. ES2015 Classes

```javascript
class ThirdComponent extends React.Component {
    render() {
        return (
            <div>{this.props.content}</div>
        );
    }
}
```

> **"Functional components cannot have "state" within them. So if your component needs to have a state, then go for class based components"**

### Props
Props are a way to pass information into a React component, they can have any type including functions - sometimes referred to as callbacks.

#### propTypes and defaultProps
It's important to define all props, their types, and where applicable, their default value

```javascript
// defined at the bottom of MyComponent
MyComponent.propTypes = {
    someObject: React.PropTypes.object,
    userID: React.PropTypes.number.isRequired,
    title: React.PropTypes.string
};

MyComponent.defaultProps = {
    someObject: {},
    title: 'My Default Title'
}
```
In this example the prop someObject is optional, but the prop userID is required. If you fail to provide userID to MyComponent, at runtime the React engine will show a console warning you that the required prop was not provided. Beware though, this warning is only shown in the development version of the React library, the production version will not log any warnings.

### State in React
State in React components is essential to manage and communicate data in your application. It is represented as a JavaScript object and has a component level scope, it can be thought of as the private data of your component.

In the example below we are defining some initial state in the constructor function of our component and make use of it in the render function:
```javascript
class ExampleComponent extends React.Component {
    constructor(props){
        super(props);
        // Set-up our initial state
        this.state = {
            greeting: 'Hiya Buddy!'
        };
    }
    render() {
        // We can access the greeting property through this.state
        return(
            <div>{this.state.greeting}</div>
        );
    }
}
```

#### setState()
React monitors every component state for changes. But for React to do so efficiently, we have to change the state field through another React API - **this.setState()**
This function will perform a **_shallow merge_** between the new state that you provide and the previous state, and will trigger a re-render of your component and all decedents.

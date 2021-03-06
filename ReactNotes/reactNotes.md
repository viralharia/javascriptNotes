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

### Types of Components in React
#### 1. Stateless Component
It does not contain state. It may use props and return JSX.
```javascript
import React, { Component } from 'react';
import { render } from 'react-dom';
class FirstComponent extends Component {
    render() {
        return (
            <div>
                Hello, {this.props.name}! I am a FirstComponent.
            </div>
        );
    }
}
    
render(
    <FirstComponent name={ 'User' } />,
    document.getElementById('content')
);
```    
For stateless components, people find it preferabble to use **stateless functional Component** which are based on ES6 arrow functions.
```javascript
const FirstComponent = props => (
    <div>
        Hello, {props.name}! I am a FirstComponent.
    </div>
);
```
#### 2. Stateful Component
In contrast to the 'stateless' components shown above, 'stateful' components have a state object that can be updated with the **setState** method. The state must be initialized in the constructor before it can be set:
```javascript
import React, { Component } from 'react';
class SecondComponent extends Component {
    constructor(props) {
        super(props);
        this.state = {
            toggle: true
        };
        // This is to bind context when passing onClick as a callback
        this.onClick = this.onClick.bind(this);
    }
    
    onClick() {
        this.setState((prevState, props) => ({
            toggle: !prevState.toggle
        }));
    }
    
    render() {
        return (
            <div onClick={this.onClick}>
                Hello, {this.props.name}! I am a SecondComponent.
                <br />
                Toggle is: {this.state.toggle}
            </div>
        );
    }
}
```
#### 3. Higher Order Component
Higher order components (HOC) allow to share component functionality.
Higher order components are used when you want to share logic across several components regardless of how different they render.
```javascript
import React, { Component } from 'react';
const PrintHello = ComposedComponent => class extends Component {
    onClick() {
        console.log('hello');
    }
    
    /* The higher order component takes another component as a parameter
    and then renders it with additional props */
    render() {
        return <ComposedComponent {...this.props } onClick={this.onClick} />
    }
}

const FirstComponent = props => (
    <div onClick={ props.onClick }>
        Hello, {props.name}! I am a FirstComponent.
    </div>
);

const ExtendedComponent = PrintHello(FirstComponent);
```

### Props
Props are a way to pass information into a React component, they can have any type including functions - sometimes referred to as callbacks.

#### propTypes and defaultProps
It's important to define all props, their types, and where applicable, their default value

**ES6**
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
**ES7**
```javascript
class MyClass extends React.Component {
    static propTypes = {
        randomObject: React.PropTypes.object,
        callback: React.PropTypes.func.isRequired,
        ...
    };
    
    static defaultProps = {
        randomObject: {},
        ...
    };
}
```
In this example the prop someObject is optional, but the prop userID is required. If you fail to provide userID to MyComponent, at runtime the React engine will show a console warning you that the required prop was not provided. Beware though, this warning is only shown in the development version of the React library, the production version will not log any warnings.

Some primitive propTypes and commonly useable propTypes are -
* optionalArray: React.PropTypes.array,
* optionalBool: React.PropTypes.bool,
* optionalFunc: React.PropTypes.func,
* optionalNumber: React.PropTypes.number,
* optionalObject: React.PropTypes.object,
* optionalString: React.PropTypes.string,
* optionalSymbol: React.PropTypes.symbol

**More complex props validation**
In the same way, PropTypes allows you to specify more complex validation
**Validating an object**
```javascript
...
randomObject: React.PropTypes.shape({
    id: React.PropTypes.number.isRequired,
    text: React.PropTypes.string,
}).isRequired,
...
```
**Validating on array of objects**
```javascript
...
arrayOfObjects: React.PropTypes.arrayOf(React.PropTypes.shape({
    id: React.PropTypes.number.isRequired,
    text: React.PropTypes.string,
})).isRequired,
...
```

#### Passing down props using spread operator
Instead of
`var component = <Component foo={this.props.foo} bar={this.props.bar} />;`
Where each property needs to be passed to the same name property in the child component, you could use the spread operator `...` 
The component will now look like this.
`var component = <Component {...props} />;`
The properties of the object that you pass in are copied onto the component's props.
supported for arrays in ES6 to pass down all your values. 

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

**Parameters of setState() api:**
1. updater: It can be an object with a number of key-value pairs that should be merged into the state or a function that returns such an object.
    * Using with Object as 1st param:
        ```javascript
        this.setState({
            greeting: 'Hello World!'
        });
        ```
    * Using with function as 1st param: This is often used when you want to check or make use of previous state before updating the state.
        ```javascript
        this.setState(function(previousState, currentProps) {
            return {
                counter: previousState.counter + 1
            };
        });
        ```
        The functional approach can also be used to move state setting logic outside of components. This allows for isolation and re-use of state logic.
        ```javascript
        // Outside of component class, potentially in another file/module
        function incrementCounter(previousState, currentProps) {
            return {
                counter: previousState.counter + 1
            };
        }
        
        // Within component
        this.setState(incrementCounter);
        ```
2. callback (optional): a function which will be executed after setState() has been executed successfully.
Due to the fact that calls to setState() are not guaranteed by React to be atomic, this can sometimes be useful if you want to perform some action after you are positive that setState() has been executed successfully.
    `this.setState({ name: 'John Doe' }, console.log('Hi there'));`

## React AJAX call
### 1. Using 'Superagent' library
A request can be initiated by invoking the appropriate method on the **request** object, then calling **_.end()_** to send the request.
##### Get request example:
```javascript
import React from 'react'
import request from 'superagent'
class App extends React.Component {
    constructor () {
        super()
        this.state = {}
    }
    componentDidMount () {
        request
        .get('/search')
        .query({ query: 'Manny' })
        .query({ range: '1..5' })
        .query({ order: 'desc' })
        .set('API-Key', 'foobar')
        .set('Accept', 'application/json')
        .end((err, resp) => {
        if (!err) {
            this.setState({someData: resp.text})
        }
        })
    },
    render() {
        return (
            <div>{this.state.someData || 'waiting for response...'}</div>
        )
    }
}
React.render(<App />, document.getElementById('root'))
```
* **_set()_** - Setting header fields is simple, invoke **_.set()_** with a field name and value.
* **_query_** - The **_.query()_** method accepts objects, which when used with the GET method will form a query-string. 
The following will produce the path -  /search?query=Manny&range=1..5&order=desc.

##### POST request example:
```javascript
request.post('/user')
    .set('Content-Type', 'application/json')
    .send('{"name":"tj","pet":"tobi"}')
    .end(callback)
```

### 2. Using 'fetch' library
**_fetch_**, is build into most browsers. Use a **_fetch polyfill_** in production to support older browsers.
```javascript
import React from 'react';
class Users extends React.Component {
    constructor() {
        super();
        this.state = { users: [] };
    }
    componentDidMount() {
        fetch('/api/users')
        .then(response => response.json())
        .then(json => this.setState({ users: json.data }));
    }
    render() {
        return (
            <div>
                <h1>Users</h1>
                {
                    this.state.users.length == 0
                    ? 'Loading users...'
                    : this.state.users.map(user => (
                    <figure key={user.id}>
                        <img src={user.avatar} />
                        <figcaption>
                            {user.name}
                        </figcaption>
                    </figure>
                    ))
                }
            </div>
        );
    }
}
ReactDOM.render(<Users />, document.getElementById('root'));
```

### 3. Using plain vanilla javascript. Without any library
```javascript
import React from 'react'
class App extends React.Component {
    constructor () {
        super()
        this.state = {someData: null}
    }
    componentDidMount () {
        var request = new XMLHttpRequest();
        request.open('GET', '/my/url', true);
        request.onload = () => {
            if (request.status >= 200 && request.status < 400) {
                // Success!
                this.setState({someData: request.responseText})
            } else {
                // We reached our target server, but it returned an error
                // Possibly handle the error by changing your state.
            }
        };
        request.onerror = () => {
            // There was a connection error of some sort.
            // Possibly handle the error by changing your state.
        };
        request.send();
    },
    render() {
        return (
            <div>{this.state.someData || 'waiting for response...'}</div>
        )
    }
}
React.render(<App />, document.getElementById('root'))
```
### Some Tips and Tricks
#### Using single quotes instead of back-ticks
Strings created with back-ticks (`` `...` ``) are different from strings created with single quotes ('...').

We create a string using back-ticks when we need to include dynamic expressions inside that string (without resorting to string concatenation).
```javascript
`This is a string template literal that can include expressions`
'This is just a string, you cannot include expressions here'
```
Let’s say you want a string that always reports the current time: 
“Time is ...”
```javascript
// Current time string
const time = new Date().toLocaleTimeString();
// When using regular strings (single or double quotes), 
// you need to use string concatenation:
'Time is ' + time
// When using back-ticks,
// you can inject the time in the string using ${}
`Time is ${time}`
```
Also, when using string literals (with back-ticks), you can create a string that spans multiple lines:
```javascript
const template = `I
CAN
SPAN
Multiple Lines`;
```
You can’t do that with regular strings.




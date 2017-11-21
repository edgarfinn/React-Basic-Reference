# React-Basic-Reference

This repo is intended to provide a quick reference to the key elements and functionalities of React.

Accessing the DOM
---
Once your React App is compiled through Webpack and Babel, it is exported in a concentrated JS file (usually called ```bundle.js```) which needs to be able to access the DOM in order to render the App to the browser.

- index.html file:

```html
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="/style/style.css">
</head>
<body>
  <div class="container"></div>
  <!-- provides a target that React can use to channel the app through -->
</body>
<script src="/bundle.js"></script>
<!-- contains the compiled react app,
and can access the DOM via elements in the body -->
</html>

```

## Setting up your app modules
This section assumes webpack and babel are configured. For information on webpack and babel configuration for react, read:
- [Webpack documentation](https://webpack.js.org/)
- [Babel Documentation](https://babeljs.io/docs/setup/#installation)
- or [follow this tutorial](https://scotch.io/tutorials/setup-a-react-environment-using-webpack-and-babel)

```
My React Project
  src/
    index.js
  index.html
```

**~/index.html**:
``` html
<body>
  <div class="container">
    <!-- Any react compontns will be rendered to here -->
  </div>
</body>
<script src="/bundle.js"></script>
```
**~/src/index.js**:

```jsx

import React, {Component} from 'react';
import ReactDOM from 'react-dom';

class Hello extends React.Component {
  render() {
    return <div>Hello World</div>;
  }
}

ReactDOM.render(<Hello />,document.querySelector('.container');

```

#### Keep It Modular
In order to keep your app easily testable, its important to keep everything functional modular.
Any module should be creating just one element / component.

Keep one JS file per module, and file these under a ```src/``` folder in the root directory (or better still, file it them under ```src/components/``` in the root directory of your app, and create sub-folders within components for each individual module and its respective purpose / functionality.

You should then have one central JS file which pulls all these components together (commonly named ```index.js``` or ```app.js```)
```
My React Project
  src/
    components/
      content/
        content.js
      footer/
        footer.js
      hero/
        hero.js
    index.js
  index.html
```
#### Importing and Exporting for modular architecture

#### Importing:
Importing exposes modules from elsewhere in your app (OR from node) into a file.

```import React from 'react';``` (ES6)

 is much the same as

 ```var React = require('react')``` (ES5)

When importing your own modules, you have to provide the absolute file path, like so:

```js
import Hero from './components/hero/hero';
```
#### Exporting:
Exporting mounts a module to the centre of your app, making it possible to be imported elsewhere:

```export default myCode;``` (ES6)

much the same as

```module.exports = myCode; ```(ES5)


## [Components](https://reactjs.org/docs/components-and-props.html)
- In React, components are snippets of code that produce HTML.
- There are two main types of components:
  - Functional Components

  and
  - Class components

### Functional components:
Functional components are quite simply react components which are created from javascript functions. These are the most simple form of component, as they generally will behave the same regardless of user interaction, and as such, have no consideration for the app's "[state](#state)";

**~/src/components/hero/hero.js**:
```jsx
import React from 'react';

const HeroHeader = (props) => {
  return (
    <h1> Welcome to my App! </h1>
    );
  };

  export default HeroHeader;
  ```

This component can then be imported into our index module like so:

**~/src/index.js**:

```jsx
import React, {Component} from 'react';
import ReactDOM from 'react-dom';
import HeroHeader from './components/hero/hero';

class App extends Component {
  constructor(props) {
    super(props)    
  }
  render() {
    /*serve HeroHeader to the DOM whenever App is re-rendered */
    return <HeroHeader />
  }
}
/* Render App to the DOM */
ReactDOM.render(<App />, document.querySelector('.container'));

```

### Class Components:
Class components are sub-classes of the React.Component class, written using the [ES6 class keyword](https://github.com/edgarfinn/ES6-OOP#defining-classes-with-es6).

```jsx
/* expose the Component class in the scope of the module  */
import React, { Component } from 'react';

/* define a class component using an ES6 class declaration */
class HeroHeader extends Component {
  render () {
    return (
      <h1> Welcome to my App, {this.props.name}! </h1>
      );
    }  
  };

export default HeroHeader;
```

If you require a component to have any consideration for the app's state you will need the component to be a class component. Unless you absolutely certain added functionalities, such as updating the DOM, or access to the app's state, then its generally best to default to using functional components.

### State
State is a plain javascript object which is used to **record** and **react** to user events. Each class-based component has its own state.

Whenever a component's state is changed, the component immediately re-renders, and forces any of its child components to re-render as well.

Whenever making use of state in a component, the state object must first be initialised inside the class' [constructor() method](https://github.com/edgarfinn/ES6-OOP#constructor).

```jsx
class Hero extends Component {

  constructor(props) {
    super(props);
    // initialise state
    this.state = { greeting: 'Welcome to my App!' };
  }

  render() {
    return (
      <div>
        <h1>{this.state.greeting}</h1>
      </div>
    );
  }
};

export default Hero;
```

Once there is key: value pair defined in a component's state (such as ```greeting``` above), this value can be accessed from elsewhere within the component using ```this.state.keyName```.

You can see that the code previously defined by the functional component has now been morphed into the ```render()``` method of the class component.

### Setting State
Once state is initialised in the constructor function, you can use event handlers to update state to a user's input data, using an inline ES6 arrow function for an event handler.

```jsx
import React, {Component} from 'react';

class SearchBar extends Component {

  constructor(props) {
    super(props);
    // initialise state
    this.state = { term: '' };
  }

  // write a 'render' method for sending an input field to the DOM
  render() {
    return (
      <div>
        <input
          placeholder = { 'search' }
          // write an event handler for input going into the searchbar
          // use setState() to set the state's 'term' value to the user's input
          onChange = { (event) => this.setState({term: event.target.value})}

          // when the value of a component is provided by state, the component becomes a 'controlled component'
          value = { this.state.term }
        />
      </div>
    );
  }
};

export default SearchBar;
```

Every time a user enters data into the above input field, the event handler is triggered, which calls the ```setState()``` method.

### Controlled Components
A controlled component has its ```value``` set by state. As demonstrated above, you can see that the input component's value is set to the ```state.term``` variable, which is initialised as ```''```.

#### Sequence of events here:

1) User enetrs text into ```<input />```.

2) The text entry triggers the onChange event handler.

3) The onChange event handler updates the component's state using ```setState()``` to the value of the event's target (event.target.value).

4) setState is called, causing the component to immediately re-render (as well as any child components, remember)

5) The element is re-rendered, this time with ```state.term```'s value pre-set to whatever the user's input. So the input component's value is now initialised to the value of ```this.state.term```, which is equal to the text entered in 1).

#### Referencing javascript variables in JSX
Whenever referencing javascript variables in JSX, the reference needs to be wrapped in curly braces like so:
```jsx
<h1>The value of input is: {this.state.term}</h1>
```

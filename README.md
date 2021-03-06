# React-Basic-Reference

This repo is intended to provide a quick reference to the key elements and functionalities of React.

## Setting up your app modules
This document assumes webpack and babel are configured. For information on webpack and babel configuration for react, read:
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
    <!-- Any react components will be rendered to here -->
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

ReactDOM.render(<Hello />, document.getElementById('.container'));
```

Accessing the DOM
---
Once your React App is compiled through Webpack and Babel, it is exported into a concentrated JS file (usually called ```bundle.js```) which needs to be able to access the DOM in order to render the App to the browser.

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
<!-- contains the compiled react app, and can access the DOM via elements in the body -->
</html>

```

#### Keep It Modular
In order to keep your app easily testable, it helps to keep everything modular and functional.
Any module should be creating just one element / component.

Keep one JS file per module, and file these under a ```src/``` folder in the root directory (or better still, file it them under ```src/components/``` in the root directory of your app, and create sub-folders within components for each individual module and its respective purpose / functionality.

You should then have one central JS file in the root of the ```src/``` folder, which pulls all these components together (commonly named ```index.js``` or ```app.js```)
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
You may also find it helps to give the parent-most element of any component a class name which reflects the name of the component, so that you can keep one CSS file to each component:

**~/src/components/content/hero.js**:

```JSX
<section className="hero">
  stuff
</section>
```

#### Importing and Exporting for modular architecture

#### Importing:
Importing a module from elsewhere exposes it to the scope of aanother module or file. You may need to import node modules such as React or ReactDOM, or your own custom modules, like 'header' or 'nav-bar'.

```import React from 'react';``` (ES6)

 is much the same as

 ```var React = require('react')``` (ES5)

When importing your own custom modules, you have to provide the absolute file path, like so:

```js
import Hero from './components/hero/hero';
```
#### Exporting:
Exporting modules exposes their code to a global space in your app, where can be imported by other modules. But it must be imported in order to be used elsewhere, and cannot be imported to another module until it is first exported:

(ES6):
```jsx
const myCode = () => {
  return (
    ...
    )
}

// expose myCode to the rest of the app
export default myCode;

```

much the same as its ES5 equivalent:

```module.exports = myCode; ```


## [Components](https://reactjs.org/docs/components-and-props.html)
- In React, components are snippets of code that produce HTML.
- There are two main types of components:
  - Functional Components

  and
  - Class components

### Functional components:
Functional components are quite simply react components which are created from javascript functions. These are the most simple form of component, as they generally will behave the same regardless of user interaction, and as such, have no consideration for the app's [state](#state);

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
Class components are sub-classes of the React.Component class, written using the [ES6 class keyword](https://github.com/edgarfinn/ES6-OOP#defining-classes-with-es6). As sub-classes of the React.Component class, all class modules inherit React.Component's properties, but can also override certain properties or methods (such as the constructor method).

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

If you require a component to have any consideration for the app's state you will need the component to be a class component. Unless you really need added functionalities, such as updating the app's state or the DOM, then its generally best to default to using functional components.

### Invoking Components

Once a component has been defined, it is rendered to the DOM through JSX invocations. In JSX, invoking a function or class component is done using chevrons like so:
```jsx
// Define component
const Greeting = () => {
  return <h1>Hello, world!</h1>
}

ReactDOM.render(
  <Greeting />,
  document.getElementById('root')
);

```
By invoking an instance of a component in another component's ```render()``` method, and passing it up the chain of invocations to the ReactDOM.render() method, you are telling ReactDOM to render all components invoked in that chain:

```jsx
// Define a (child) Greeting component
const Greeting = () => {
  return <h1>Hello, world!</h1>
}

class App extends React.Component {
  constructor(props) {
    super(props)    
  }
  render() {
    return (
      // invoke the Greeting component
      <Greeting />
    )
  }
}
// Render the <App /> component, and all of its (child) component invocations
ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

In React, parent-child relationships are relative to the DOM tree, and therefore the order of invocations in the ReactDOM.render() method. So whichever component is being passed directly into ReactDOM.Render() in your index module, is essentially the parent-most component.

Above, the ```Greeting``` is a child component of ```App```

### State
State is a plain javascript object which is used to **record** and **react** to user events. Each class-based component has its own component-level state. (Not to be confused with application-level state).

Whenever a component's state is changed, the component immediately re-renders, and causes any of its child components to re-render as well.

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

Once there is a key->value pair defined in a component's state (such as ```greeting``` above), this value can be accessed from elsewhere within the component using ```this.state.keyName```.

You can see that the code previously defined by the functional component has now been morphed into the ```render()``` method of the class component.

### Setting State
Once state is initialised, you can use the ```setState()``` method to update state properties to the value of a user's input data, using an inline ES6 arrow function for an event handler.

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
          // write an event handler for text entered into the searchbar
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
A controlled component has its ```value``` set by state. As demonstrated above, you can see that the ```<input />``` component's value is set to the ```state.term``` variable, which is initialised as ```''```.

#### Sequence of events here:

1) User enetrs text into ```<input />```.

2) The text entry triggers the onChange event handler.

3) The onChange event handler updates the component's state using ```setState()``` to the value of the event's target ```(event.target.value)```.

4) ```setState()``` is called, causing the component to immediately re-render (as well as any child components, remember)

5) The element is re-rendered, this time with ```state.term```'s value pre-set to the user's text input. So the ```<input />``` component's value is now initialised to the value of ```this.state.term```, which is equal to the text entered in 1).

#### Referencing javascript variables in JSX
Whenever referencing javascript variables in JSX, the reference needs to be wrapped in curly braces like so:
```jsx
<h1>The value of input is: {this.state.term}</h1>
```

### Downwards data flow
Redux architecture revolves around a strict unidirectional data flow.

Downwards data flow is therefore a popular principal, in which only the parent-most component in an application is responsible for fetching data, which can then be passed in a single direction downwards, to its child components.

Reminder: Parent-child relationships in react are relative to the DOM tree, and therefore the order of invocations in the ReactDOM.render() method. So whichever component is being passed into the ReactDOM.render() method is essentially the parent-most component.

Any child components invoked by that component, which in turn invoke more components then become 'parents' of those components they invoke.

### Passing data through props
Data can be passed from a parent (invoking) component to a child component by defining values within the component invocation
```<ChildComponent myProperty="value" />```
and accessed by the child component through the ```props``` argument: ```const ChildComponent = (props)=> {<p>{props.myProperty}</p>}```

```jsx

class Parent extends React.Component {
  constructor(props) {
    super(props)
  }

  render() {
    return (
      // Invoke an instance of the Child component, passing a 'greeting' property to props
      <Child greeting="Hello World!" />
    )
  }
}

const Child = (props)=> {
  // Access the greeting property through props.
  // remember JS referenced within JSX needs to be wrapped in curlies.
  return <h1>{props.greeting}</h1>
}

ReactDOM.render(
  <Parent />,
  document.getElementById('root')
);
```
Here's a slightly more verbose, modular example

**~/index.js**:
```js
import React, {Component} from 'react';
import ReactDOM from 'react-dom';
import YTSearch from 'youtube-api-search';
import VideoList from './components/video_list';

const API_KEY = 'abcdef12356789';

class App extends Component {
  constructor(props) {
    super(props);

    this.state = { videos: [] };
    YTSearch({ key: API_KEY, term: 'surf boards'}, (videos) => { this.setState({ videos: videos })}
    );
  }

  render() {
    return (
      <div>

        <SearchBar />

        // invoke an instance of VideoList and pass the state's videos object to it via props
        <VideoList videos={this.state.videos} />

      </div>
    )
  }
}

ReactDOM.render(<App />, document.querySelector('.container'));
```
The VideoList Component can then be accessed

**~/src/components/video_list.js**:
```jsx
import React from 'react';

const VideoList = (props) => {
  return (
    <ul>
    {props.videos.length}
    </ul>
    );
  };

  export default VideoList;
  ```

  In functional components, the props object needs to be passed in as an argument in order to be accessible to that function:
  ```js
  const Component = (props) => {<p>{props.stuff}</p>}
  ```
In class components props is automatically inherited as a pre-defined property of the Component class, and accessed using ```this.props```.

So whenever re-factoring a component from a function to a class, remember to update any reference to props from ```props.stuff``` to ```this.props.stuff```.

### Rendering arrays of components

You can create arrays of JSX components and pass them into React to render. React will recognise it as a list of components and traverse the array rendering each element.

In order to identify specific elements in an array being rendered, react requires each JSX element to have a set ```key``` property in its props, with a value which is unique to that element.

This only needs to be set within the component invocation. ```<List item key={this.props.uniqueDataTag} />```. You dont have to set it in the definition of that component.

You can see this mini-app in action by copying the contents into the javascript field of [this React CodePen](https://codepen.io/gaearon/pen/rrpgNB?editors=0010)

```JSX
class App extends React.Component {

  constructor(props) {
    super(props)
  // pre-define state with a list of data objects
  // This is the core data which is serving each item in the list
    this.state = {
      ApiDataList: [
        {dataId:1,information:"item one", infoLabel:"a"},
        {dataId:2,information:"item two", infoLabel:"b"},
        {dataId:3,information:"item three", infoLabel:"c"}
      ]
    };
  }

  render() {
    return (
      // invoke list and pass a props.dataList argument
      <List dataList={this.state.ApiDataList}/>
    )
  }
}

// List takes props from App
const List = (props) => {
// DataItems returns an array of JSX ListItem elements
// Each with its own unique 'key', which is seemingly invisible to the DOM
// And each comes with its own information taken different objects from iteration through the data list
  const DataItems = props.dataList.map((item) => {
    return (
      <ListItem
        key={item.dataId}
        dataMessage={item.information}
        label={item.infoLabel}
      />
    )
  })

  return (
    <ul>
      {DataItems}
    </ul>
  );
};
// ListItem takes props from List
const ListItem = (props) => {
  const message = props.dataMessage;
  const label = props.label;
  return (
    <li>
      <h1>{label}</h1>
      <h2>{message}</h2>
    </li>
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

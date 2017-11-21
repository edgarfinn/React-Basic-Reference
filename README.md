# React-Basic-Reference

This repo is intended to provide a quick reference to the key elements and functionalities of React.

Accessing the DOM
---
Once your React App is compiled through Webpack and Babel, it is exported in a concentrated JS file (usually called ```bundle.js``` which needs to be able to access the DOM in order to render the App to the browser.

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
Functional components are quite simply react components which are created from javascript functions. These are the most simple form of component, as they generally will behave the same regardless of user interaction, and as such, have no consideration for the app's "[state](State)";

**~/src/components/hero/hero.js**:
```jsx
import React from 'react';

const HeroHeader = (props) => {
  return (
    <h1> Welcome to my App, {this.props.name}! </h1>
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
/*Render App to the DOM*/
ReactDOM.render(<App />, document.querySelector('.container'));

```

### Class Components:
Class components are sub-classes of the

### State

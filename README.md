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

## [Components](https://reactjs.org/docs/components-and-props.html)
- In React, components are snippets of code that produce HTML.
- There are two main types of components:
  - Functional Components

  and
  - Class components

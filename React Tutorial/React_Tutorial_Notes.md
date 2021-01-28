# [React Tutorial: An Overview and Walkthrough](https://www.taniarascia.com/getting-started-with-react/)

## Prerequisites

* Basic familiarity with **HTML & CSS**.
* Basic knowledge of **JavaScript** and programming.
* Basic understanding of the **DOM**.
* Familiarity with **ES6** syntax and features.**Node.js** and **npm** installed globally.

## Goals

* Learn about essential React concepts and related terms, such as Babel, Webpack, JSX, components, props, state, and lifecycle.
* Build a very simple React app that demonstrates the above concepts.

## What is React?

* React is a **JavaScript** library - one of the most popular ones, with over 100,000 stars on GitHub.
* React is not a framework (unlike Angular, which is more opinionated).
* React is an open-source project created by **Facebook**.
* React is used to **build user interfaces (UI) on the front end**.
* React is the **view** layer of an MVC application (Model View Controller)
* One of the most important aspects of React is the fact that you can create **components**, which are like custom, reusable HTML elements, to quickly and efficiently build user interfaces. React also streamlines how data is stored and handled, using **state** and **props**.

## Setup and Installation

### Static HTML File

Let's start by making a basic `index.html` file. We're going to load in three CDNs in the `head` - **React, React DOM, and Babel.** We're also going to make a div with an `id` called `root`, and finally we'll create a `script` tag where your custom code will live.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />

    <title>Hello React!</title>

    <script src="https://unpkg.com/react@^16/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@16.13.0/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
  </head>

  <body>
    <div id="root"></div>

    <script type="text/babel">
      // React code will go here
    </script>
  </body>
</html>
```

- [React](https://reactjs.org/docs/react-api.html) - the React top level API
- [React DOM](https://reactjs.org/docs/react-dom.html) - adds DOM-specific methods
- [Babel](https://babeljs.io/) - a JavaScript compiler that lets us use ES6+ in old browsers

The entry point for our app will be the `root` div element, which is named by convention. You'll also notice the `text/babel` script type, which is mandatory for using Babel.

We're going to use ES6 classes to create a React component called `App`.

```react
class App extends React.Component {
    // ...
}
```

Now we'll add the `render()` method, the only required method in a class component, which is used to render DOM nodes.

```react
class App extends React.Component {
    render() {
        return (
            // ...
        );
    }
}
```

Inside the `return`, we're going to put what looks like a simple HTML element. Note that we're not returning a string here, so don't use quotes around the element. This is called `JSX`, and we'll learn more about it soon.

```react
class App extends React.Component {
    render() {
        return <h1>Hello world</h1>
    }
}
```

Finally, we're going to use the React DOM render() method to render the App class we created into the root div in our HTML.

```react
ReactDOM.render(<App />, document.getElementById('root'))
```

Here is the full code for our `index.html`.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />

    <title>Hello React!</title>

    <script src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
  </head>

  <body>
    <div id="root"></div>

    <script type="text/babel">
      class App extends React.Component {
        render() {
          return <h1>Hello world!</h1>
        }
      }

      ReactDOM.render(<App />, document.getElementById('root'))
    </script>
  </body>
</html>
```

Now if you view your `index.html` in the browser, you'll see the `h1` tag we created rendered to the DOM.

### Create React App

Facebook has created[ Create React App](https://github.com/facebook/create-react-app), an environment that comes pre-configured with everything you need to build a React app. It will create a live development server, use **Webpack** to automatically compile React, JSX, and ES6, auto-prefix CSS files, and use **ESLint** to test and warn about mistakes in the code.

To set up `create-react-app`, run the following code in your terminal, one directory up from where you want the project to live.

```sh
npx create-react-app react-tutorial

cd react-tutorial && npm start
```

Once you run this command, a new window will popup at `localhost:3000` with your new React app.

> Create React App is very good for getting started for beginners as well as large-scale enterprise applications, but it's not perfect for every workflow. You can also create your own Webpack setup for React.

If you look into the project structure, you'll see a `/public` and `/src` directory, along with the regular `node_modules`, `.gitignore`, `README.md`, and `package.json`.

```shell
my-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── serviceWorker.js
    └── setupTests.js
```

In `/public`, our important file is `index.html`, which is very similar to the static `index.html` file we made earlier - just a `root` div. 

The `/src` directory will contain all our React code. Go ahead and delete all the files out of the `/src` directory, and we'll create our own boilerplate file without any bloat. We'll just keep `index.css` and `index.js`.

For `index.css`, I just copy-and-pasted the contents of [Primitive CSS](https://taniarascia.github.io/primitive/css/main.css) into the file. If you want, you can use Bootstrap or whatever CSS framework you want, or nothing at all. I just find it easier to work with.

Now in `index.js`, we're importing React, ReactDOM, and the CSS file.

```react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```

Let's create our `App` component again. 

```react
class App extends React.Component {
  render() {
    return (
      <div className="App">
        <h1>Hello, React!</h1>
      </div>
    )
  }
}
```

Finally, we'll render the `App` to the root as before.

```react
ReactDOM.render(<App />, document.getElementById('root'))
```

Here's our full `index.js`. This time, we're loading the `Component` as a property of React, so we no longer need to extend `React.Component`.

```react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

class App extends React.Component {
  render() {
    return (
      <div className="App">
        <h1>Hello, React!</h1>
      </div>
    )
  }
}

ReactDOM.render(<App />, document.getElementById('root'))
```

## React Developer Tools

There is an extension called React Developer Tools that will make your life much easier when working with React. Download [React DevTools for Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi), or whatever browser you prefer to work on.

## JSX: JavaScript + XML

As you've seen, we've been using what looks like HTML in our React code, but it's not quite HTML. This is **JSX**, which stands for JavaScript XML.

With JSX, we can write what looks like HTML, and also we can create and use our own XML-like tags. Here's what JSX looks like assigned to a variable.

```jsx
const heading = <h1 className="site"></h1>
```

Using JSX is not mandatory for writing React. Under the hood, it's running `createElement`, which takes the tag, object containing the properties, and children of the component and renders the same information.

```react
const heading = React.createElement('h1', {className: 'site-heading'}, 'Hello, React!')
```

JSX is actually closer to JavaScript, not HTML, so there are a few key differences to note when writing it.

- `className` is used instead of `class` for adding CSS classes, as `class` is a reserved keyword in JavaScript.
- Properties and methods in JSX are **camelCase** - `onclick` will become `onClick`.
- **Self-closing** tags *must* end in a slash - e.g. `<img />`

JavaScript expressions can also be embedded inside JSX using curly braces, including variables, functions, and properties.

```jsx
const name = 'Tania'
const heading = <h1>Hello, {name}</h1>
```

## Components

So far, we've created one component - the `App` component. Almost everything in React consists of components, which can be **class components** or **simple components**.

Most React apps have many small components, and everything loads into the main `App` component. Components also often get their own file.

Remove the `App` class from `index.js`. 

```react
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'))
```

We'll create a new file called `App.js` and put the component in there. 

```react
import React, {Component} from 'react'

class App extends React.Component {
  render() {
    return (
      <div className="App">
        <h1>Hello, React!</h1>
      </div>
    )
  }
}

export default App;
```

We export the component as App and load it in `index.js`. It's not mandatory to separate components into files, but an application will start to get unwieldy and out-of-hand if you don't.

### Class Components

Let's create another component. We're going to create a table. Make `Table.js`, and fill it with data.

```react
import React, {Component} from 'react'

class Table extends Component {
  render() {
    return (
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Job</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Charlie</td>
            <td>Janitor</td>
          </tr>
          <tr>
            <td>Mac</td>
            <td>Bouncer</td>
          </tr>
          <tr>
            <td>Dee</td>
            <td>Aspiring actress</td>
          </tr>
          <tr>
            <td>Dennis</td>
            <td>Bartender</td>
          </tr>
        </tbody>
      </table>
    )
  }
}

export default Table
```

This component we created is a **custom class component**. We capitalize custom components to differentiate them from regular HTML elements. Back in `App.js`, we can load in the Table, first by importing it in.

```react
import Table from './Table'
```

Then by loading it into the `render()` of `App`, where before we had "Hello, React!". I also changed the class of the outer container.

```react
import React, {Component} from 'react'
import Table from './Table'

class App extends React.Component {
  render() {
    return (
      <div className="container">
        <Table />
      </div>
    )
  }
}

export default App
```

If you check back on your live environment, you'll see the `Table` loaded in.

### Simple Components

The other type of component in React is the **simple component**, which is a function. This component doesn't use the `class` keyword. Let's take our `Table` and make two simple components for it - a table header, and a table body.

We're going to use ES6 arrow functions to create these simple components. First, the table header.

```react
const TableHeader = () => {
    return (
        <thead>
            <tr>
                <th>Name</th>
                <th>Job</th>
            </tr>
        </thead>
    )
}
```

Then the body.

```react
const TableBody = () => {
    return (
      <tbody>
        <tr>
          <td>Charlie</td>
          <td>Janitor</td>
        </tr>
        <tr>
          <td>Mac</td>
          <td>Bouncer</td>
        </tr>
        <tr>
          <td>Dee</td>
          <td>Aspiring actress</td>
        </tr>
        <tr>
          <td>Dennis</td>
          <td>Bartender</td>
        </tr>
      </tbody>
    )
}
```

Now our `Table` file will look like this. Note that the `TableHeader` and `TableBody` components are all in the same file, and being used by the `Table` class component.

```react
const TableHeader = () => { ... }
const TableBody = () => { ... }

class Table extends Component {
  render() {
    return (
      <table>
        <TableHeader />
        <TableBody />
      </table>
    )
  }
}
```

As you can see, components can be nested in other components, and simple and class components can be mixed.

> A class component must include `render()`, and the `return` can only return one parent element.

As a wrap up, let's compare a simple component with a class component.

```react
const SimpleComponent = () {
    return <div>Example</div>
}
```

```react
class ClassComponent extends Component {
    render() {
        return <div>Example</div>
    }
}
```

Note that if the `return` is contained to one line, it does not need parentheses.

## Props

Right now, we have a cool `Table` component, but the data is being hard-coded. One of the big deals about React is how it handles data, and it does so with properties, referred to as **props**, and with state. Now, we'll focus on handling data with props.

First, let's remove all the data from our `TableBody` component.

```react
const TableBody = () => {
  return <tbody />
}
```

Then let's move all that data to an array of objects, as if we were bringing in a JSON-based API. We'll have to create this array inside our `render()`.

```react
class App extends Component {
  render() {
    const characters = [
      {
        name: 'Charlie',
        job: 'Janitor',
      },
      {
        name: 'Mac',
        job: 'Bouncer',
      },
      {
        name: 'Dee',
        job: 'Aspring actress',
      },
      {
        name: 'Dennis',
        job: 'Bartender',
      },
    ]

    return (
      <div className="container">
        <Table />
      </div>
    )
  }
}
```

Now, we're going to pass the data through to the child component (`Table`) with properties, kind of how you might pass data through using `data-` attributes. We can call the property whatever we want, as long as it's not a reserved keyword, so I'll go with `characterData`. The data I'm passing through is the `characters` variable, and I'll put curly braces around it as it's a JavaScript expression.

```react
return (
  <div className="container">
    <Table characterData={characters} />
  </div>
)
```

Now that data is being passed through to `Table`, we have to work on accessing it from the other side.

This data is not in the actual DOM yet, though. In `Table`, we can access all props through `this.props`. We're only passing one props through, characterData, so we'll use `this.props.characterData` to retrieve that data.

I'm going to use the ES6 property shorthand to create a variable that contains `this.props.characterData`.

```react
const {characterData} = this.props
```

Since our `Table` component actually consists of two smaller simple components, I'm going to pass it through to the `TableBody`, once again through props.

```react
class Table extends Component {
  render() {
    const {characterData} = this.props

    return (
      <table>
        <TableHeader />
        <TableBody characterData={characterData}/>
      </table>
    )
  }
}
```

We're going to pass the props through as a parameter, and [map through the array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) to return a table row for each object in the array. This map will be contained in the `rows` variable, which we'll return as an expression.

```react
const TableBody = (props) => {
  const rows = props.characterData.map((row, index) => {
    return (
      <tr key={index}>
        <td>{row.name}</td>
        <td>{row.job}</td>
      </tr>
    )
  })

  return <tbody>{rows}</tbody>
}
```

If you view the front end of the app, all the data is loading in now.

You'll notice I've added a key index to each table row. You should always use [keys](https://reactjs.org/docs/lists-and-keys.html#keys) when making lists in React, as they help identify each list item. We'll also see how this is necessary in a moment when we want to manipulate list items.

Props are an effective way to pass existing data to a React component, however the component cannot change the props - they're read-only. In the next section, we'll learn how to use state to have further control over handling data in React.

## State


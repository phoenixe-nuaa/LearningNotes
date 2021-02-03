[toc]

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

Inside the `return`, we're going to put what looks like a simple HTML element.

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

The `/src` directory will contain all our *React code*. Go ahead and delete all the files out of the `/src` directory. We'll just keep `index.css` and `index.js`.

For `index.css`, I just copy-and-pasted the contents of [Primitive CSS](https://taniarascia.github.io/primitive/css/main.css) into the file. You can use Bootstrap or whatever CSS framework you want.

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

JavaScript expressions can also be embedded inside JSX using ***curly braces***, including variables, functions, and properties.

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

The other type of component in React is the **simple component**, which is a *function*. This component doesn't use the `class` keyword. Let's take our `Table` and make two simple components for it - a table header, and a table body.

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

As you can see, components can be **nested** in other components, and simple and class components can be **mixed**.

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

Then let's move all that data to an array of objects, as if we were bringing in a *JSON-based API*. We'll have to create this array inside our `render()`.

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

Now, we're going to pass the data through to the child component `Table` with properties, kind of how you might pass data through using `data-` attributes. We can call the property `characterData`. The data I'm passing through is the `characters` variable, and I'll put curly braces around it as it's a JavaScript expression.

```react
return (
  <div className="container">
    <Table characterData={characters} />
  </div>
)
```

Now that data is being passed through to `Table`, we have to work on accessing it from the other side. In `Table`, we can access all props through `this.props`. We're only passing one props through, characterData, so we'll use `this.props.characterData` to retrieve that data.

I'm going to use the ES6 property shorthand to create a variable that contains `this.props.characterData`.

```react
const {characterData} = this.props
```

I'm going to pass it through to the `TableBody`, once again through props.

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

If you view the front end of the app, all the data is loading in now. You'll notice I've added a key index to each table row. You should always use [**keys**](https://reactjs.org/docs/lists-and-keys.html#keys) when making lists in React, as they help identify each list item. We'll also see how this is necessary in a moment when we want to manipulate list items.

Props are an effective way to pass existing data to a React component, however the component cannot change the props - they're *read-only*. In the next section, we'll learn how to use state to have further control over handling data in React.

## State

Right now, we're storing our character data in an array in a variable, and passing it through as props. With props, we have a *one way* data flow, but with state we can update private data from a component.

You can think of state as any data that should be saved and modified without necessarily being added to a database - for example, adding and removing items from a shopping cart before confirming your purchase.

To start, we're going to create a `state` object.

```react
class App extends Component {
  state = {}
}
```

The object will contain properties for everything you want to store in the state. For us, it's `characters`.

```react
class App extends Component {
  state = {
    characters: [],
  }
}
```

Move the entire array of objects we created earlier into `state.characters`.

```react
class App extends Component {
  state = {
    characters: [
      {
        name: 'Charlie',
        // the rest of the data
      },
    ],
  }
}
```

Our data is officially contained in the state. Since we want to be able to remove a character from the table, we're going to create a `removeCharacter` method on the parent `App` class.

To retrieve the state, we'll get `this.state.characters` using the same ES6 method as before. To update the state, we'll use `this.setState()`, a built-in method for manipulating state. We'll [filter the array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) based on an `index` that we pass through, and return the new array.

> You must use `this.setState()` to modify an array. Simply applying a new value to `this.state.property` will not work.

```react
removeCharacter = (index) => {
  const {characters} = this.state

  this.setState({
    characters: characters.filter((character, i) => {
      return i !== index
    }),
  })
}
```

`filter` does not mutate but rather ***creates a new array***, and is a preferred method for modifying arrays in JavaScript. This particular method is testing an index vs. all the indices in the array, and returning all but the one that is passed through.

Now we have to pass that function through to the component, and render a button next to each character that can invoke the function. We'll pass the `removeCharacter` function through as a prop to `Table`.

```react
render() {
  const { characters } = this.state

  return (
    <div className="container">
      <Table characterData={characters} removeCharacter={this.removeCharacter} />
    </div>
  )
}
```

Since we're passing it down to `TableBody` from `Table`, we're going to have to pass it through again as a prop, just like we did with the character data.

In addition, since it turns out that the only components having their own states in our project are `App` and `Form`, it would be best practice to transform `Table` into a simple component from the class component it currently is.

```react
const Table = (props) => {
  const {characterData, removeCharacter} = props

  return (
    <table>
      <TableHeader />
      <TableBody characterData={characterData} removeCharacter={removeCharacter} />
    </table>
  )
}
```

Here's where that index we defined in the `removeCharacter()` method comes in. In the `TableBody` component, we'll pass the key/index through as a parameter, so the filter function knows which item to remove. We'll create a button with an `onClick` and pass it through.

```react
<tr key={index}>
  <td>{row.name}</td>
  <td>{row.job}</td>
  <td>
    <button onClick={() => props.removeCharacter(index)}>Delete</button>
  </td>
</tr>
```

> The `onClick` function must pass through a function that returns the `removeCharacter()` method, otherwise it will try to run automatically.

## Submitting Form Data

Now we have data stored in state, and we can remove any item from the state. However, what if we wanted to be able to add new data to state? In a real world application, you'd more likely start with empty state and add to it, such as with a to-do list or a shopping cart.

Before anything else, let's remove all the hard-coded data from `state.characters`, as we'll be updating that through the form now.

```react
class App extends Component {
  state = {
    characters: [],
  }
}
```

Now let's go ahead and create a `Form` component in a new file called `Form.js`.

We're going to set the initial state of the `Form` to be an object with some empty properties, and assign that initial state to `this.state`.

```react
import React, {Component} from 'react'

class Form extends Component {
  initialState = {
    name: '',
    job: '',
  }

  state = this.initialState
}
```

*Our goal for this form will be to update the state of `Form` every time a field is changed in the form, and when we submit, all that data will pass to the `App` state, which will then update the `Table`.*

First, we'll make the function that will run every time a change is made to an input. The `event` will be passed through, and we'll set the state of `Form` to have the `name` (key) and `value` of the inputs.

```react
handleChange = (event) => {
  const {name, value} = event.target

  this.setState({
    [name]: value,
  })
}
```

Let's get this working before we move on to submitting the form. 

In the render, let's get our two properties from state, and assign them as the values that correspond to the proper form keys. We'll run the `handleChange()` method as the `onChange` of the input, and finally we'll export the `Form` component.

```react
render() {
  const { name, job } = this.state;

  return (
    <form>
      <label htmlFor="name">Name</label>
      <input
        type="text"
        name="name"
        id="name"
        value={name}
        onChange={this.handleChange} />
      <label htmlFor="job">Job</label>
      <input
        type="text"
        name="job"
        id="job"
        value={job}
        onChange={this.handleChange} />
    </form>
  );
}

export default Form;
```

In `App.js`, we can render the form below the table.

```react
import Form from './Form'
```

```react
return (
  <div className="container">
    <Table characterData={characters} removeCharacter={this.removeCharacter} />
    <Form />
  </div>
)
```

Now if we go to the front end of our app, we'll see a form that doesn't have a submit yet.

Cool. Last step is to allow us to actually submit that data and update the parent state. We'll create a function called `handleSubmit()` on `App` that will update the state by taking the existing `this.state.characters` and adding the new `character` parameter, using the [ES6 spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).

```react
handleSubmit = (character) => {
  this.setState({characters: [...this.state.characters, character]})
}
```

Let's make sure we pass that through as a parameter on `Form`.

```react
<Form handleSubmit={this.handleSubmit} />
```

Now in `Form`, we'll create a method called `submitForm()` that will call that function, and pass the `Form` state through as the `character` parameter we defined earlier. It will also reset the state to the initial state, to clear the form after submit.

```react
submitForm = () => {
  this.props.handleSubmit(this.state)
  this.setState(this.initialState)
}
```

Finally, we'll add a submit button to submit the form. We're using an `onClick` instead of an `onSubmit` since we're not using the standard submit functionality. The click will call the `submitForm` we just made.

```react
<input type="button" value="Submit" onClick={this.submitForm} />
```

And that's it! The app is complete. We can create, add, and remove users from our table. Since the `Table` and `TableBody` were already pulling from the state, it will display properly.

## Pulling in API Data

One very common usage of React is pulling in data from an API. If you're not familiar with what an API is or how to connect to one, I would recommend reading [How to Connect to an API with JavaScript](https://www.taniarascia.com/how-to-connect-to-an-api-with-javascript/), which will walk you through what APIs are and how to use them with vanilla JavaScript.

As a little test, we can create a new `Api.js` file, and create a new `App` in there. A public API we can test with is the [Wikipedia API](https://en.wikipedia.org/w/api.php), and I have a [URL endpoint right here](https://en.wikipedia.org/w/api.php?action=opensearch&search=Seona+Dancing&format=json&origin=*) for a random* search. You can go to that link to see the API - and make sure you have [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc) installed on your browser.

We're going to use [JavaScript's built-in Fetch](https://www.taniarascia.com/how-to-use-the-javascript-fetch-api-to-get-json-data/) to gather the data from that URL endpoint and display it. You can switch between the app we created and this test file by just changing the URL in `index.js` - `import App from './Api';`.

I'm not going to explain this code line-by-line, as we've already learned about creating a component, rendering, and mapping through a state array. The new aspect to this code is `componentDidMount()`, a React lifecycle method. **Lifecycle** is the order in which methods are called in React. **Mounting** refers to an item being inserted into the DOM.

When we pull in API data, we want to use `componentDidMount`, because we want to make sure the component has rendered to the DOM before we bring in the data. In the below snippet, you'll see how we bring in data from the Wikipedia API, and display it on the page

```react
import React, {Component} from 'react'

class App extends Component {
  state = {
    data: [],
  }

  // Code is invoked after the component is mounted/inserted into the DOM tree.
  componentDidMount() {
    const url =
      'https://en.wikipedia.org/w/api.php?action=opensearch&search=Seona+Dancing&format=json&origin=*'

    fetch(url)
      .then((result) => result.json())
      .then((result) => {
        this.setState({
          data: result,
        })
      })
  }

  render() {
    const {data} = this.state

    const result = data.map((entry, index) => {
      return <li key={index}>{entry}</li>
    })

    return <ul>{result}</ul>
  }
}

export default App
```

Once you save and run this file in the local server, you'll see the Wikipedia API data displayed in the DOM.

There are other lifecycle methods, but going over them will be beyond the scope of this article. You can [read more about React components here](https://reactjs.org/docs/react-component.html).

## Building and Deploying a React App

Everything we've done so far has been in a development environment. We've been compiling, hot-reloading, and updating on the fly. For production, we're going to want to have static files loading in - none of the source code. We can do this by making a build and deploying it.

Now, if you just want to compile all the React code and place it in the root of a directory somewhere, all you need to do is run the following line:

```shell
npm run build
```

This will create a `build` folder which will contain your app. Put the contents of that folder anywhere, and you're done!

We can also take it a step further, and have npm deploy for us. We're going to build to GitHub pages, so you'll already have to [be familiar with Git](https://www.taniarascia.com/getting-started-with-git/) and getting your code up on GitHub.

Make sure you've exited out of your local React environment, so the code isn't currently running. First, we're going to add a `homepage` field to `package.json`, that has the URL we want our app to live on.

```js
"homepage": "https://taniarascia.github.io/react-tutorial",
```

We'll also add these two lines to the `scripts` property.

```js
"scripts": {
  // ...
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
}
```

In your project, you'll add `gh-pages` to the devDependencies.

```shell
npm install --save-dev gh-pages
```

We'll create the `build`, which will have all the compiled, static files.

```shell
npm run build
```

Finally, we'll deploy to `gh-pages`.

```shell
npm run deploy
```

And we're done! The app is now available live at https://taniarascia.github.io/react-tutorial.

\- End of tutorial -
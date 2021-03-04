# [Modern React with Redux](https://www.udemy.com/course/react-redux/learn/lecture/12531046#content)

## Section 1 Let's dive in!

### Some critical questions

- What was that '`App`' function?
  - The App function *is* a React Component (produces JSX and handles user events)
  - JSX *is* Set of instructions to tell React what content we want to show on the screen
    - JSX Elements are aimed to:
      - Tell React to create a normal HTML element
      - Tell React to show another component
- How did some content get displayed on the screen?
  - `React.render(<App />, document.getElementById("root"));`
  - `index.js`call the App function `App Component`, get back JSX, and turn it into HTML.
  - Then take that HTML and put into the DOM inside the div in`index.html`.
- What's the difference between React and ReactDOM?
  - React: knows how to work with components; called as a 'reconciler'.
  - ReactDOM: knows how to take instructions on what we want to show and turn into HTML; called as a 'renderer'.
- What was all the 'useState' stuff?
  - Function for working with React's '`state`' system
  - `State` is used to keep track of data changes over time
  - used to make React update the HTML on the screen

### Project Directory

- src
  - Folder where we put all the source code we write
- public
  - Folder that stores static files, like images
- node_modules
  - Folder that contains all of our project dependencies
- package.json
  - Records our project dependencies and configures our project
- package-lock.json
  - Records the exact version of packages that we install
- README.md
  - instructions on how to use this project

### Start and stop the React app

To start,

> Run `npm start` in the project directory.
>

To stop,

> Press `Control + C` at the terminal.
>

### JavaScript module systems

```react
// Import the React and ReactDOM libraries
import React from 'react';
import ReactDOM from 'react-dom';

// Create a react component
const App = () => {
    return <div>Hi There!</div>;
};

// Take the react component and show it on the screen
ReactDOM.render(
    <App />,
    document.querySelector('#root')
);
```

## Section 2 Building content with JSX

### What is JSX?

with [babeljs.io](https://babeljs.io/),

```jsx
const App = () => {
	return <div>Hi There!</div>;
};
```

turns into:

```react
const App = () => {
    return React.createElement(
        "div",
        null,
        "Hi there!"
    );
};
```

### Converting HTML into JSX

#### JSX vs HTML

- Adding custom styling to an element uses different syntax

  - HTML

    ```html
    <div style="background-color:red; color: white;"></div>
    ```
    
  - JSX
  
    ```jsx
    <div style={{ backgroundColor:'red', color:'white'}}></div>
    ```
  
- Adding a class to an element uses different syntax

  - `class` to `className`.

- JSX can reference JS variables, or expressions

  - using `{}`.

  - ```jsx
    function getButtonText() {
    	return 'Click on me!';
    }
    
    const App = () => {
        ...
        return (
            ...
            // to invoke a function
            {getButtonText()}
        )
    };
    ```

#### Values JSX can't show

we are not allowed to take a JavaScript object and reference it inside JSX, especially where we normally shows text. Such as,

```jsx
const App = () => {
    const buttonText = { text: 'Click me'};
    const style = { backgroundColor: 'blue', color: 'white' };
    ...
    return (
        ...
        // {buttonText} will throw the error as 'Ojects are not valid as a React child'
        // similarily for style, we are referencing a js variable -- style
        <button style={style}>
        	{buttonText.text}
        </button>
    )
};
```

## Section 3 Communicating with props

### Three tenets of components

- Component Nesting
- Component Reusability
- Component Configuration

### Creating a Reusable, Configurable Component

- Identify the JSX that appears to be duplicated
- What is the purpose of that block of JSX? Think of a descriptive name for what it does
- Create a new file to house this new component - it should have the same name as the component
- Create a new component in the new file, paste the JSX into it
- Make the new component configurable by using React's 'props' system

### Props

- System for passing data from a ***parent*** component to a ***child*** component
- Goal is to customize or configure a child component

### Application

Let's try to make a application of showing some approval cards. We use the naïve component approach to do so. We are using `faker` to generate random images. We are passing and receiving props between components for customization. 

In `index.js`,

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import faker from 'faker';
import CommentDetail from './CommentDetail';
import ApprovalCard from './ApprovalCard';

const App = () => {
    return (
        <div className="ui container comments">
            <ApprovalCard>
                <div>
                    <h4>Warning!</h4>
                    Are you sure you want to do this?
                </div>
            </ApprovalCard>
            <ApprovalCard>
                {/* wrapped in as a children of props */}
                <CommentDetail 
                    {/* pass through props */}
                	author="Sam" 
                	timeAgo="Today at 4:00 pm"
                	avatar={faker.image.avatar()}
                />
            </ApprovalCard>
            {/* ... */}
        </div>
    );
};

ReactDOM.render(<App />, document.getElementById('root'));
```

In `CommentDetail.js`,

```jsx
import React from 'react';

const CommentDetail = props => {
    return (
        <div className="comment">
            <a href="/" className="avatar">
                <img alt="avatar" src={props.avatar} />
            </a>
            <div className="content">
                <a href="/" className="author">
                    {props.author}s
                </a>
                <div className="metadata">
                    <span className="date">{props.timeAgo}</span>
                </div>
            </div>
        </div>
    );
};
```

In `ApprovalCard.js`,

```jsx
import React from 'react';

const ApprovalCard = props => {
    return (
        <div className="ui card">
            <div className="content">
                {props.children}
            </div>
            <div className="extra content">
                <div className="ui two buttons">
                    <div className="ui basic green button">Approve</div>
                    <div className="ui basic red button">Reject</div>
                </div>
            </div>
        </div>
    );
};

export default ApprovalCard;
```

## Section 4 Structing Apps with Class-based Components

### Class-based Components

| Class Components                                             | Hooks System (Function Components)                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Can produce JSX to show content to the user                  | Can produce JSX to show content to the user                  |
| Can use the Lifecycle Method system to run code at specific points in time | Can use **Hooks** to run code at specific points in time     |
| Can use the `state` system to update content on the screen   | Can use **Hooks** to access state system and update content on screen |

### Rules of Class Components

- Must be a JavaScript Class
- Must extend (subclass) `React.Component`
- Must define a 'render' method that returns some amount of JSX

## Section 5 State in React Components

### Rules of State

- Only usable with class components
- You will confuse props with state :(
- 'State' is a JS object that contains data relevant to a component
- Updating 'state' on a component causes the component to (almost) instantly re-render
- State must be initialized when a component is created
- State can **only** be updated using the function '`setState`'

### Application

Let's try to build a Application which could tell which season a user is in.

#### Challenges

- Need to get the user's physical location
- Need to determine the current month
- Need to change text and styling based on location + month

#### [Geolocation API](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/Using_geolocation)

Noted that `getCurrentPostition` takes some amount of time, we use **Class Components** and `state` to solve this.

In `SeasonDisplay.js`,

```jsx
import React from 'react';

const SeasonDisplay = () => {
    return <div>Season Display</div>;
};

export default SeasonDisplay;
```

In `index.js`,

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

// changing to a class component to handle the async opertion
class App extends React.Component {
    // overriding a constructor in React.Component
    constructor(props) {
        // reference to the parent function
        super(props);
		
        // THIS IS THE ONLY TIME we do direct assignment to this.state
        this.state = { lat: null, errorMessage: '' };
    
        // getCurrentPostition taking SOME AMOUNT OF TIME
        window.navigator.geolocation.getCurrentPosition(
            // callback
            position => {
                // we called setState!! 
                // not getting invoked until we finally get the location
                this.setState({ lat: positions.coords.latitude });
            },
            // handling error
            err => {
                this.setState({ errorMessage: err.message });
            }
        );
    } 
    
    componentDidMount() {
        console.log('My component was rendered to the screen');
    }
    
    componentDidUpdate() {
        console.log('My component was just updated - it re-rendered');
    }
    
    // React says we have to define render!!
    render() {
        // conditional rendering
        if (this.state.errorMessage && !this.state.lat) {
            return <div>Error: {this.state.errorMessage}</div>
        }
        
        if (!this.state.errorMessage && this.state.lat) {
            return <div>Latitude: {this.state.lat}</div>
        }
    	
        return <div>Loading...</div>
    }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

#### Timeline

- JS file loaded by browser
- Instance of App component is created
- App components 'constructor' function gets called
- State object is created and assigned to the '`this.state`' property
- We call geolocation service
- React calls the components render method
- App returns JSX, gets rendered to page as HTML
- ...
- We get result of geolocation
- We update our state object with a call to '`this.setState`'
- React sees that we updated the state of a component
- React calls our '`render`' method a second time
- Render method returns some (updated) JSX
- React takes that JSX and updates content on the screen

## Section 6 Understanding Lifecycle Methods

### Component Lifecycle

Component lifecycle methods are functions we can define inside class based components. They will be called automatically by react at certain point of time.

| Methods (called chronologically)                        | Scenarios used                                               |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| ***constructor*** (optional)                            | good place to do one-time setup                              |
| ***render***                                            | avoid doing anything besides returning JSX                   |
| *content visible on screen*                             |                                                              |
| ***componentDidMount***                                 | good place to do data-loading                                |
| *sit and wait for updates...*                           |                                                              |
| ***componentDidUpdate***                                | good place to do more data-loading when state/props change (re-fetching) |
| *sit and wait until this component is not longer shown* |                                                              |
| ***componentWillUnmount***                              | good place to do cleanup (especially for non-react stuff)    |

#### Refactoring data loading to lifecycle methods

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

class App extends React.Component {
    // alternate way of state initialization without constructor
    // babel will handle it all
    state = { lat: null, errorMessgae: ''};
    
    // data-loading
    componentDidMount() {
        window.navigator.geolocation.getCurrentPosition(
            position => this.setState({ lat: positions.coords.latitude }),
            err => this.setState({ errorMessage: err.message })
        );
    }
    // ...
    }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

### Working back to the Application

We will pass state as props, determine the season base on the month and latitude, show the icons, refactor it by extracting options to config objects, and add some styling as well.

To load the icons, install the CSS library locally:

```shell
npm install semantic-ui-css
```

Then, import the library into your root index.js component:

```jsx
import "semantic-ui-css/semantic.min.css";
```

In `index.js`,

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import SeasonDisplay from './SeasonDisplay';

class App extends React.Component {
    // alternate way of state initialization as an instance property
    // babel will handle it all
    state = { lat: null, errorMessgae: ''};
    
    // data-loading
    componentDidMount() {
        window.navigator.geolocation.getCurrentPosition(
            position => this.setState({ lat: positions.coords.latitude }),
            err => this.setState({ errorMessage: err.message })
        );
    }
    
	renderContent() {
        if (this.state.errorMessage && !this.state.lat) {
            return <div>Error: {this.state.errorMessage}</div>
        }
        
        if (!this.state.errorMessage && this.state.lat) {
            return <SeasonDisplay lat={this.state.lat} />
        }
    	
        return <Spinner message="Please accept location request"/>;
    }
	
    render() {
        return (
            <div className="border red">
                {this.renderContent()}
            </div>
        )
    }
}

ReactDOM.render(<App />, document.getElementById('root'));
```

In `SeasonDisplay.js`,

```jsx
import './SeasonDisplay.css';
import React from 'react';

const SeasonDisplay = props => {
    const season = getSeason(props.lat, new Date().getMonth());
    const { text, iconName } = seasonConfig[season];
    
    return  (
        <div className={`season-display &{season}`}>
            <i className={`icon-left massive ${iconName} icon`} />
            <h1>{text}</h1>
            <i className={`icon-right massive ${iconName} icon`} />
        </div>
    );
};

const seasonConfig = {
    summer: {
        text: 'Lets hit the beach!',
        iconName: 'sun'
    },
    winter: {
        text: 'Burr, it is chilly',
        iconName: 'snowflake'
    }
};

const getSeason = (lat, month) => {
    if (month > 2 && month < 9) {
        return lat > 0 ? 'summer' : 'winter';
    }
    else {
        return lat > 0 ? 'winter'  :'summer';
    }  
};

export default SeasonDisplay;
```

To add some styling, in `SeasonDisplay.css`,

```css
.icon-left {
    position: absolute;
    top: 10px;
    left: 10px;
}

.icon-right {
    position: absolute;
    bottom: 10px;
    right: 10px;
}

.season-display.winter i {
    color: blue;
}

.season-display.summer i {
    color: red;
}

.season-display {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
```

Add a loading spinner in `spinner.js`, 

```jsx
import React from 'react';

const Spinner = props => {
    return (
        <div className="ui active dimmer">
            <div className="ui big text loader">{props.message}</div>
        </div>
    );
};

// specifying default props
Spinner.defaultProps = {
	message: 'Loading...'	    
};

export default Spinner;
```

## Section 7 Handling User Input with Forms and Events

### App Overview

Users can type in any term and click enter. Then we will call an outside API to search for corresponding images. We will fetch the results and show them on the screen as a list.

### App Challenges

- Need to get a search term from the user
- Need to use that search term to make a request to an outside API and fetch data
- Need to take the fetched images and show them on the screen in a list

### Component Design

- SearchBar
- ImageList

### Property Name

- User clicks on something --> **onClick**
- User changes text in an input --> **onChange**
- User submits a form --> **onSubmit**

In `index.js`,

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './Components/App';

ReactDOM.render(<App />. document.getElementById('root'));
```

We will place all our components in the `components` folder.

In `App.js`,

```jsx
import React from 'react';
import SearchBar from './SearchBar';

const App = () => {
    return ( 
        <div className="ui container" style={{ marginTop: '10px' }}>
            <SearchBar />
        </div>;
    )
}

export default App;
```

Firstly, build our `SearchBar` component. In `SearchBar.js` ,

```jsx
import React from 'react';

class SearchBar extends React.Component {
    // Name Conversion: on + element + event
    onInputChange(event) {
        console.log(event.target.value);
    }
    
    render() {
        return (
            <div className="ui segment">
                <form className="ui form">
                    <div className="field">
                        <label>Image Search</label>
                        {/* callback, putting () will automatically call onInputChange function whenever out component is rendered. equvilant to onChange={(event) => console.log(event.target.value)} */}
                        <input type="text" onChange={this.onInputChange} />
                    </div>
                </form>
            </div>
         );
    }
}

export default SearchBar;
```

### Controlled vs Uncontrolled Elements

With uncontrolled elements, only the DOM knows the value of the input at anytime. *We prefer the react component driving and storing the data instead of DOM.*

|                   | React World                    | DOM World                                                 |
| ----------------- | ------------------------------ | --------------------------------------------------------- |
| Uncontrolled Form | `???`                          | `<input value="hi there" />`                              |
| Controlled Form   | `state -> { term: 'hi there'}` | `<input value={go look at state to get current value} />` |

Refactoring it using controlled elements:

```jsx
import React from 'react';

class SearchBar extends React.Component {
    state = { term: '' };	

	// handling form submittal
	// fix the context of the value of this in the callback: 
	// 1. by assigning it as a arrow function to the onFormSubmit property
	onFormSubmit = (event, props) => {
        event.preventDefault();
        
        this.props.onSubmit(this.state.term);
    };
    
    render() {
        return (
            <div className="ui segment">
                {/* 2. wrapping the callback here inside of an arrow function 
                <form onSubmit={(event) => this.onFormSubmit(event)} ...> */}
                <form onSubmit={this.onFormSubmit} className="ui form">
                    <div className="field">
                        <label>Image Search</label>
                        <input
                            type="text"
                            value={this.state.term}
                            {/* creating event handlers */}
                            onChange={e => this.setState({ term: e.target.value })} />
                        />
                    </div>
                </form>
            </div>
         );
    }
}

export default SearchBar;
```

### Handling Form Submittal

In `App.js`, we will communicate child to parent by invoking callbacks in children:

```jsx
import React from 'react';
import SearchBar from './SearchBar';

class App extends React.Component {
    onSearchSubmit(term) {
        console.log(term);
    }
    
    render() {
        return ( 
        <div className="ui container" style={{ marginTop: '10px' }}>
            <SearchBar onSubmit={this.onSearchSubmit} />
        </div>
    	);
    }
}

export default App;
```

## Section 8 Making API Requests with React

### Unsplash API

Link: `unsplash.com/developers`.

### Flows

React App -> AJAX Client -- (send me data about pics for cars) --> Unsplash API

​										    				<-- (Here you go!) --

### Import axios (vs fetch)

a library for managing network requests and fetch some amount of data.

### Handling requests with Async Await

```jsx
import React from 'react';
import axios from 'axios';
import SearchBar from './SearchBar';

class App extends React.Component {
    state = { images: [] };

    onSearchSubmit = async (term) => {
    	const response = await axios.get('https://api.unsplash.com/search/photos', {
            params: { query: term },
            headers: {
                Authorization: 'Client-ID xxx-Your-Access-Key-xxx'
            }
        });
        this.setState{{ images: response.data.results }};
    }
    
    render() {
        return ( 
        <div className="ui container" style={{ marginTop: '10px' }}>
            <SearchBar onSubmit={this.onSearchSubmit} />
            Found: {this.state.images.length} images
        </div>
    	);
    }
}

export default App;
```

An alternative method:

```jsx
// ... imports

class App extends React.Component {
    onSearchSubmit(term) {
        // async request
        axios.get('https://api.unsplash.com/search/photos', {
            params: { query: term },
            headers: {
                Authorization: 'Client-ID xxx-Your-Access-Key-xxx'
            }
        // axios requests will return a Promise
        	    // callback
        }).then((response) => {
            console.log(response.data.results);
        }); 
    }
    
    // render() {
    // 	...
	// }
}

// ... export
```

### Creating custom clients

Refactoring the `App.js` file by building a new file called `Unsplash.js` inside the api folder,

```jsx
import axios from 'axios';

export default axios.create({
    baseURL: 'https://api.unsplash.com',
    headers: {
        Authorization: 'Client-ID xxx-Your-Access-Key-xxx'
    } 
});
```

In `App.js`,

```jsx
import React from 'react';
import axios from '../api/unsplash';
import SearchBar from './SearchBar';

class App extends React.Component {
    state = { images: [] };

    onSearchSubmit = async (term) => {
    	const response = await unsplash.get('/search/photos', {
            params: { query: term }
        });
        this.setState{{ images: response.data.results }};
    }
    
    render() {
        return ( 
        <div className="ui container" style={{ marginTop: '10px' }}>
            <SearchBar onSubmit={this.onSearchSubmit} />
            Found: {this.state.images.length} images
        </div>
    	);
    }
}

export default App;
```

## Section 9 Building Lists of Records

let's move on to `ImageList` Component. In `ImageList.js`,

```jsx
import React from 'react';

const ImageList = (props) => {
    // rendering lists of components
    const images = props.image.map(({ description, id, urls }) => {
       // key helps react to render the list more precisely
       return <img alt={description} key={id} src={urls.regular} /> 
    });
    
    return <div className="image-list">{images}</div>;
};

export default ImageList;
```

In `App.js`,

```jsx
import React from 'react';
import axios from '../api/unsplash';
import SearchBar from './SearchBar';
import ImageList from './ImageList';

class App extends React.Component {
    state = { images: [] };

    onSearchSubmit = async (term) => {
    	const response = await unsplash.get('/search/photos', {
            params: { query: term }
        });
        this.setState{{ images: response.data.results }};
    }
    
    render() {
        return ( 
        <div className="ui container" style={{ marginTop: '10px' }}>
            <SearchBar onSubmit={this.onSearchSubmit} />
            <ImageList images={this.state.images} />
        </div>
    	);
    }
}

export default App;
```

### Review of Map Statements

```javascript
const numbers = [0, 1, 2, 3, 4];

let newNumbers = [];

for (let i = 0; i < numbers.length; i++) {
    newNumbers.push(numbers[i] * 10);
}

numbers;    // [0, 1, 2, 3, 4]
newNumbers; // [0, 10, 20, 30, 40]

numbers.map((num) => {
    return num * 10;
})

numbers; // [0, 10, 20, 30, 40]
```

## Section 10 Using Ref's for DOM Access

Let's work on CSS.

### Grid CSS

Create a new file called `ImageList.css` inside the `Components` folder.

```css
.image-list {
    display: grid;
    /* grid-template-columns: creates a set number of columns */
    /* auto-fill: automatically decide how many columns to insert */
    /* minmax: each one of the column at minimum 250px, at most 1fr(a maximum allocation of space, at this context, they are equally sized)*/
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    grid-gap: 0 10px;
    /* we want the pics scrunch together vertically */
    grid-auto-rows: 10px;
}

.image-list img {
    width: 250px;
    /* specifies a grid item's end position within the grid row by contributing 2 spans */
    grid-row-end: span 2;
}
```

### Creating an ImageCard Component

We need to specify a `grid-row-end` value depending on the height of every individual image. To solve this, let's create a new component `ImageCard.js` responsible for rendering one singe image at a time:

```jsx
import React from 'react';

class ImageCard extends React.Component {
    render() {
        // destructure out just the properties we care about
        const { description, urls } = this.props.image;
        
        return (
            <div>
                <img alt={description} src={urls.regular} />
            </div>
        );
    }
}

export default ImageCard;
```

In `ImageList.js`,

```jsx
import './ImageList.css';
import React from 'react';
import ImageCard from './ImageCard';

const ImageList = (props) => {
    // rendering lists of components
    const images = props.image.map((image) => {
       // key helps react to render the list more precisely
       return <ImageCard key={image.id} src={image} /> 
    });
    
    return <div className="image-list">{images}</div>;
};

export default ImageList;
```

### Flow Diagram

- Let the ImageCard render itself and its image
- Reach into the DOM and figure out the height of the image (**How???**)
- Set the image height on state to get the component to rerender
- When re-rendering, assign a 'grid-row-end' to make sure the image take up the appropriate space

### Accessing the DOM with Refs

React *Refs* system

- Gives access to a single DOM element
- We create refs in the constructor, assign them to instance variables, then pass to a particular JSX element as props

In `ImageCard.js`,

```jsx
import React from 'react';

class ImageCard extends React.Component {
    constructor(props) {
        super(props);
        
        this.state = { spans: 0 };
        
        this.imageRef = React.createRef();
    }
    
    componentDidMount() {
        // after we have successfully loads the image, we can access the height
        									  // callbacks on image load
        this.imageRef.current.addEventListener('load', this.setSpans);
    }
    
    // callback has to be bind otherwise the value of 'this' is undefined
    setSpans = () => {
        const height = this.imageRef.clientHeight;
        
        const spans = Math.ceil(height / 10) + 1;
        
        this.setState({ spans });
    }
    
    render() {
        const { description, urls } = this.props.image;
        
        return (
            // dynamic spans
            <div stytle={{ gridRowEnd: `span ${this.state.spans}` }}>
                <img ref={this.imageRef} alt={description} src={urls.regular} />
            </div>
        );
    }
}

export default ImageCard;
```

## Section 11 Let's Test Your React Mastery

### App Overview

| App                                           | List of video items |
| --------------------------------------------- | ------------------- |
| SearchBar                                     | show video          |
| VideoDisplayer (make requests to YouTube API) | show video          |
| TitleAndDescription                           | show video          |

### Component Design

- App
  - SearchBar
  - VideoDetail
  - VideoList
    - VideoItem

#### Flow

![](C:\Users\Chen.JL\OneDrive - Northeastern University\LearningNotes\Modern_React_With_Redux\flow.png)


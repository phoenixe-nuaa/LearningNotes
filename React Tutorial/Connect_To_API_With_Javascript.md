# How to Connect to an API with JavaScript

We're going to make a very simple web app with plain JavaScript that will retrieve information from an API and display it on the page.

## Prerequisites

- Basic knowledge of [HTML and CSS](https://internetingishard.com/html-and-css/).
- Basic knowledge of [JavaScript syntax and datatypes](https://www.sitepoint.com/beginners-guide-javascript-variables-and-datatypes/).
- Basic knowledge of working with [JSON and JavaScript objects](https://www.taniarascia.com/how-to-use-json-data-with-php-or-javascript/).

Everything else we'll cover along the way.

## Goals

We are going to write from scratch [this simple web app](https://taniarascia.github.io/sandbox/ghibli/) that connects to a [Studio Ghibli API](https://ghibliapi.herokuapp.com/), retrieves the data with JavaScript, and displays it on the front end of a website. This is *not* meant to be an extensive resource on APIs or REST - just the simplest possible example to get up and running that you can build from in the future. We'll learn:

- What a Web API is.
- Learn how to use the HTTP request `GET` with JavaScript
- How create and display HTML elements with JavaScript.

## Overview

**API** stands for *Application Program Interface*, which can be defined as a set of methods of communication between various software components. In other words, an API allows software to communicate with another software.

We'll be focusing specifically on Web APIs, which allow a web server to interact with third-party software. In this case, the web server is using **HTTP requests** to communicate to a publicly available URL **endpoint** containing JSON data.

You may be familiar with the concept of a **CRUD** app, which stands for *Create, Read, Update, Delete*. A web API uses HTTP requests that correspond to the CRUD verbs.

| Action | HTTP Method   | Description                  |
| :----- | :------------ | :--------------------------- |
| Create | `POST`        | Creates a new resource       |
| Read   | `GET`         | Retrieves a resource         |
| Update | `PUT`/`PATCH` | Updates an existing resource |
| Delete | `DELETE`      | Deletes a resource           |

> If you've heard **REST** and RESTful APIs, that is simply referring to a set of standards that conform to a specific architectural style.

## Setting Up

We're going to start by creating an **index.html** file in a new directory. The project will only consist of **index.html**, **style.css**, and **scripts.js** at the end. This HTML skeleton just links to a CSS and JavaScript file, loads in a font, and contains a div with a `root` id. This file is complete and will not change. We'll be using JavaScript to add everything from here out.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Ghibli App</title>

    <link href="https://fonts.googleapis.com/css?family=Dosis:400,700" rel="stylesheet" />
    <link href="style.css" rel="stylesheet" />
  </head>

  <body>
    <div id="root"></div>
    <script src="scripts.js"></script>
  </body>
</html>
```

We will create a **style.css** that will be used to create a grid. For brevity's sake, I only included the most pertinent **structural** CSS below, but you can copy the [full CSS code here](https://raw.githubusercontent.com/taniarascia/sandbox/master/ghibli/style.css).

```css
#root {
  max-width: 1200px;
  margin: 0 auto;
}

.container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  margin: 1rem;
  border: 1px solid gray;
}

@media screen and (min-width: 600px) {
  .card {
    flex: 1 1 calc(50% - 2rem);
  }
}

@media screen and (min-width: 900px) {
  .card {
    flex: 1 1 calc(33% - 2rem);
  }
}
```

Now we have HTML and CSS set up, so we can make **scripts.js** and we'll continue from there.

## Connecting to the API

Let's take a look at the [**Studio Ghibli API documentation**](https://ghibliapi.herokuapp.com/). This API was created to help developers learn how to interact with resources using HTTP requests. Since an API can be accessed by many different methods - JavaScript, PHP, Ruby, Python and so on - the documentation for most APIs doesn't tend to give specific instructions for how to connect.

We can see from this documentation that it tells us we can make requests with `curl` or regular REST calls.

## Obtaining the API endpoint

To get started, let's scroll to the [films section](https://ghibliapi.herokuapp.com/#tag/Films). On the right you'll see `GET /films`. It will show us the URL of our API endpoint, https://ghibliapi.herokuapp.com/films. Clicking on that link will display an array of objects in JSON.

> If you do not have an extension on your browser for viewing JSON files, add one now, such as [JSON View](https://chrome.google.com/webstore/category/extensions?hl=en). This will make reading JSON much, much easier. Remember, if you've never worked with JSON, [read this prerequisite article](https://www.taniarascia.com/how-to-use-json-data-with-php-or-javascript/).

## Retrieving the data with an HTTP request

Before we try to put anything on the front end of the website, let's open a connection the API. We'll do so using `XMLHttpRequest` objects, which is a way to open files and make an HTTP request.

- We'll create a `request` variable and assign a new `XMLHttpRequest` object to it. 
- Then we'll open a new connection with the `open()` method - in the arguments we'll specify the type of request as `GET` as well as the URL of the API endpoint. 
- The request completes and we can access the data inside the `onload` function. 
- When we're done, we'll send the request.

> **Note:** At the time of writing this article, `XMLHttpRequest` was the default method of making an API request. Fetch API has much better browser support now, so you can also do this article using Fetch. Read [How to use the JavaScript Fetch API](https://www.taniarascia.com/how-to-use-the-javascript-fetch-api-to-get-json-data) to learn how.

```js
// Create a request variable and assign a new XMLHttpRequest object to it.
var request = new XMLHttpRequest()

// Open a new connection, using the GET request on the URL endpoint
request.open('GET', 'https://ghibliapi.herokuapp.com/films', true)

request.onload = function () {
  // Begin accessing JSON data here
}

// Send request
request.send()
```

## Working with the JSON response

Now we've received a response from our HTTP request, and we can work with it. However, the response is in JSON, and we need to convert that JSON in to JavaScript objects in order to work with it.

We're going to use `JSON.parse()` to parse the response, and create a `data` variable that contains all the JSON as an array of JavaScript objects. Using `forEach()`, we'll console log out the title of each film to ensure it's working properly.

```js
// Begin accessing JSON data here
var data = JSON.parse(this.response)

data.forEach((movie) => {
  // Log each movie's title
  console.log(movie.title)
})
```

Using *Inspect* on **index.html** and viewing the console, you should see the titles of 20 Ghibli films.

The only thing we're missing here is some way to deal with errors. What if the wrong URL is used, or the URL broke and nothing was being displayed? When an HTTP request is made, the response returns with [HTTP status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status). `404` is the most well-known response, meaning **Not Found**, and `200` **OK** is a successful request.

Let's just wrap our code in an `if` statement, succeeding on any response in the 200-300 range, and log out an error if the request fails. You can mess up the URL to test the error.

```js
// Begin accessing JSON data here
var data = JSON.parse(this.response)

if (request.status >= 200 && request.status < 400) {
  data.forEach((movie) => {
    console.log(movie.title)
  })
} else {
  console.log('error')
}
```

We've successfully used a `GET` HTTP request to retrieve (or consume) the API endpoint, which consisted of data in JSON format. However, we're still stuck in the console - we want to display this data on the front end of the website, which we'll do by modifying the DOM.

## Displaying the Data

In order to display information on the front end of a site, we'll be working with the DOM, which is actually an API itself that allows JavaScript to communicate with HTML. If you have no experience at all with the DOM, I wrote [Understanding and Modifying the DOM in JavaScript](https://www.digitalocean.com/community/tutorials/introduction-to-the-dom) for DigitalOcean that will clarify what the DOM is and how the DOM differs from HTML source code.

By the end, our page will consist of a logo image followed by a container with multiple card elements - one for each film. Each card will have a heading and a paragraph, that contains the title and description of each film.

If you remember, our **index.html** just has a root div - `<div id="root">` right now. We'll access it with `getElementById()`. We can briefly remove all the previous code we've written for now, which we'll add back in just a moment.

```js
const app = document.getElementById('root')
```

If you're not 100% positive what `getElementById()` does, take the above code and `console.log(app)`. That should help clarify what is actually happening there.

```html
<div id="root"></div>
```

The first thing in our website is the logo, which is an `img` element. We'll create the image element with `createElement()`.

```js
const logo = document.createElement('img')
```

An empty `img` is no good, so we'll set the `src` attribute to `logo.png`. (Found [here](https://github.com/taniarascia/sandbox/blob/master/ghibli/logo.png))

```js
logo.src = 'logo.png'
```

We'll create another element, a `div` this time, and set the `class` attribute to `container`.

```js
const container = document.createElement('div')
container.setAttribute('class', 'container')
```

Now we have a logo and a container, and we just need to place them in the website. We'll use the `appendChild()` method to append the logo image and container div to the app root.

Now we have a logo and a container, and we just need to place them in the website. We'll use the `appendChild()` method to append the logo image and container div to the app root.

```js
app.appendChild(logo)
app.appendChild(container)
```

After saving, on the front end of the website, you'll see the following.

```html
<div id="root">
  <img src="logo.png" />
  <div class="container"></div>
</div>
```

This will only be visible in the *Inspect* Elements tab, not in the HTML source code, as explained in the DOM article I linked.

Now we're going to paste all our code from earlier back in. The last step will be to take what we consoled out previously and make them into card elements.

Paste everything back in, but we'll just be looking at what's inside the `forEach()`.

```js
data.forEach((movie) => {
  console.log(movie.title)
  console.log(movie.description)
})
```

Instead of `console.log`, we'll use `textContent` to set the text of an HTML element to the data from the API. I'm using `substring()` on the `p` element to limit the description and keep each card equal length.

```js
data.forEach((movie) => {
  // Create a div with a card class
  const card = document.createElement('div')
  card.setAttribute('class', 'card')

  // Create an h1 and set the text content to the film's title
  const h1 = document.createElement('h1')
  h1.textContent = movie.title

  // Create a p and set the text content to the film's description
  const p = document.createElement('p')
  movie.description = movie.description.substring(0, 300) // Limit to 300 chars
  p.textContent = `${movie.description}...` // End with an ellipses

  // Append the cards to the container element
  container.appendChild(card)

  // Each card will contain an h1 and a p
  card.appendChild(h1)
  card.appendChild(p)
})
```

I'll also replace the console'd error with an error on the front end, using the best HTML element, [`marquee`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/marquee)! (I'm only doing this as a joke for fun and demonstrative purposes, do not actually use `marquee` in any sort of real application, or take me seriously here.)

```js
const errorMessage = document.createElement('marquee')
errorMessage.textContent = `Gah, it's not working!`
app.appendChild(errorMessage)
```

## References

### Request API Data with JavaScript

#### Using data from JSON with JavaScript

We're going to create a JavaScript variable called `data` and apply the JSON string.

```js
var data =
  '[ { "name": "Aragorn", "race": "Human" }, { "name": "Gimli", "race": "Dwarf" } ]'
```

Now we'll use JavaScript built in `JSON.parse()` function to decode the string.

```javascript
data = JSON.parse(data);
```

From here we can access the data like a regular JavaScript object.

```javascript
console.log(data[1].name)
```

And we can loop through each iteration with a `for` loop.

```js
for (var i = 0; i < data.length; i++) {
  console.log(data[i].name + ' is a ' + data[i].race + '.')
}
```

That was easy! Now, we'll probably need to access JSON from a URL. There's an extra step involved, where we have to make a request to the file. Let's just take the above JSON string and put it in **data.json**.

```js
;[
  {
    name: 'Aragorn',
    race: 'Human',
  },
  {
    name: 'Gimli',
    race: 'Dwarf',
  },
]
```

Now we'll make an `XMLHttpRequest()`.

```js
var request = new XMLHttpRequest()
```

We'll open the file (**data.json**) via GET (URL) request.

```js
request.open('GET', 'data.json', true)
```

From here, we'll parse and work with all our JSON data within the `onload` function.

```js
request.onload = function () {
  // begin accessing JSON data here
}
```

Then finally, submit the request.

```js
request.send()
```

Here's the final code.

```js
var request = new XMLHttpRequest()

request.open('GET', 'data.json', true)

request.onload = function () {
  // begin accessing JSON data here
  var data = JSON.parse(this.response)

  for (var i = 0; i < data.length; i++) {
    console.log(data[i].name + ' is a ' + data[i].race + '.')
  }
}

request.send()
```

#### Using Fetch

Now you can also use the Fetch API to do the same thing. Read [How to use the JavaScript Fetch API to Get JSON Data](https://www.taniarascia.com/how-to-use-the-javascript-fetch-api-to-get-json-data/) for an easier method to get this data.

```js
// Replace ./data.json with your JSON feed
fetch('./data.json')
  .then((response) => {
    return response.json()
  })
  .then((data) => {
    // Work with JSON data here
    console.log(data)
  })
  .catch((err) => {
    // Do something for an error here
  })
```

#### Using jQuery

As you can see, it's not too difficult to retrieve a JSON feed with plain JavaScript. However, it's even easier with jQuery, using the `getJSON()` function. If you don't know how jQuery works, you'll need to load the [jQuery JavaScript library](https://jquery.com/) before any of this custom code.

```js
$(document).ready(function () {
  $.getJSON('data.json', function (data) {
    // begin accessing JSON data here
    console.log(data[0].name)
  })
})
```

You might also see jQuery access JSON via an AJAX request, which is a little more verbose.

```js
$(document).ready(function () {
  var data
  $.ajax({
    dataType: 'json',
    url: 'data.json',
    data: data,
    success: function (data) {
      // begin accessing JSON data here
      console.log(data[0].name)
    },
  })
})
```

Both will have the same output.

### [DOM](https://www.digitalocean.com/community/tutorials/introduction-to-the-dom)

#### Introduction

The **Document Object Model**, usually referred to as the **DOM**, is an essential part of making websites interactive. It is an interface that allows a programming language to manipulate the content, structure, and style of a website. JavaScript is the client-side scripting language that connects to the DOM in an internet browser.

Almost any time a website performs an action, such as rotating between a slideshow of images, displaying an error when a user attempts to submit an incomplete form, or toggling a navigation menu, it is the result of JavaScript accessing and manipulating the DOM.

We will learn what the DOM is, how to work with the `document` object, and the difference between HTML source code and the DOM.

#### Prerequisites

In order to effectively understand the DOM and how it relates to working with the web, it is necessary to have an existing knowledge of [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) and [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS). It is also beneficial to have familiarity with fundamental [JavaScript syntax and code structure](https://www.digitalocean.com/community/tutorials/understanding-syntax-and-code-structure-in-javascript).

#### What is the DOM?

At the most basic level, a website consists of an HTML document. The browser that you use to view the website is a program that interprets HTML and CSS and renders the style, content, and structure into the page that you see.

In addition to parsing the style and structure of the HTML and CSS, the browser creates a representation of the document known as the Document Object Model. This **model** allows JavaScript to access the text content and elements of the website **document** as **objects**.

JavaScript is an interactive language, and it is easier to understand new concepts by doing. Let’s create a very basic website. 

Create an `index.html` file and save it in a new project directory.

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <title>Learning the DOM</title>
  </head>

  <body>
    <h1>Document Object Model</h1>
  </body>

</html>
```

This code is the familiar HTML source of a new website skeleton. It contains the absolute most essential aspects of a website document — a doctype, and an `html` tag with the `head` and `body` nested inside.

Within Chrome, open up `index.html`. You’ll see a plain website with our heading saying “Document Object Model”. Right click anywhere on the page and select “Inspect”. This will open up Developer Tools.

Under the *Elements* tab, you’ll see the DOM.

In this case, when expanded, it looks exactly the same as the HTML source code we just wrote — a doctype, and the few other HTML tags that we added. Hovering over each element will highlight the respective element in the rendered website. Little arrows to the left of the HTML elements allow you to toggle the view of nested elements.

#### The Document Object

The `document` object is a built-in object that has many **properties** and **methods** that we can use to access and modify websites. In order to understand how to work with the DOM, you must understand how objects work in JavaScript. Review [Understanding Objects in JavaScript](https://www.digitalocean.com/community/tutorials/understanding-objects-in-javascript) if you don’t feel comfortable with the concept of objects.

In Developer Tools on **index.html**, move to the *Console* tab. Type `document` into the console and press `ENTER`. You will see that what is output is the same as what you see in the *Elements* tab.

```bash
> document;
```

```html
#document
<!DOCTYPE html>
<html lang="en">

  <head>
    <title>Learning the DOM</title>
  </head>

  <body>
    <h1>Document Object Model</h1>
  </body>

</html>
```

Typing `document` and otherwise working directly in the console is not something that you’ll generally ever do outside of debugging, but it helps solidify exactly what the `document` object is and how to modify it, as we will discover below.

#### What is the Difference Between the DOM and HTML Source Code?

Currently, with this example, it seems that HTML source code and the DOM are the exact same thing. There are two instances in which the browser-generated DOM will be different than HTML source code:

- The DOM is modified by ***client-side*** JavaScript
- The browser ***automatically*** fixes errors in the source code

Let’s demonstrate how the DOM can be modified by client-side JavaScript. Type the following into the console:

```bash
> document.body;
```

The console will respond with this output:

```html
<body>
  <h1>Document Object Model</h1>
</body>
```

`document` is an object, `body` is a property of that object that we have accessed with dot notation. Submitting `document.body` to the console outputs the `body` element and everything inside of it.

In the console, we can change some of the live properties of the `body` object on this website. We’ll edit the `style` attribute, changing the background color to `fuchsia`. Type this into the console:

```bash
> document.body.style.backgroundColor = 'fuchsia';
```

After typing and submitting the above code, you’ll see the live update to the site, as the background color changes.

Switching to the *Elements* tab, or typing `document.body` into the console again, you will see that the DOM has changed.

```html
<body style="background-color: fuchsia;">
  <h1>Document Object Model</h1>
</body>
```

The JavaScript code we typed, assigning `fuchsia` to the background color of the `body`, is now a part of the DOM.

However, right click on the page and select “View Page Source”. You will notice that the source of the website does *not* contain the new style attribute we added via JavaScript. The source of a website will not change and will never be affected by client-side JavaScript. If you refresh the page, the new code we added in the console will disappear.

The other instance in which the DOM might have a different output than HTML source code is when there are errors in the source code. One common example of this is the `table` tag — a `tbody` tag is required inside a `table`, but developers often fail to include it in their HTML. The browser will automatically correct the error and add the `tbody`, modifying the DOM. The DOM will also fix tags that have not been closed.

#### Conclusion

In this tutorial, we defined the DOM, accessed the `document` object, used JavaScript and the console to update a property of the `document` object, and went over the difference between HTML source code and the DOM.

For more in-depth information on the DOM, review the [Document Object Model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) page on the Mozilla Developer Network.
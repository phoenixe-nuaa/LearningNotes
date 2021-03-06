[toc]

# [Javascript Review](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

## Overview

JavaScript is a multi-paradigm, dynamic language with types and operators, standard built-in objects, and methods. JavaScript supports ***object-oriented programming*** with object **prototypes**, instead of classes (see more about [prototypical inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) and ES2015 [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)). JavaScript also supports ***functional programming*** — because they are objects, functions may be stored in variables and passed around like any other object.

## Types

JavaScript's types are:

* [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
* [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
* [`Boolean`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
* [`Symbol`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) (new in ES2015)
* [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
  - [`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)
  - [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
  - [`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
  - [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
* [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null)
* [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)

And there are some built-in `Error` types as well.

## Numbers

> Numbers in JavaScript are double-precision 64-bit format IEEE 754 values".

according to the spec — There's no such thing as an **integer** in JavaScript (except [`BigInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)).

```javascript
console.log(3 / 2);	            // 1.5, not 1
console.log(Math.floor(3 / 2)); // 1
```

So an ***apparent integer*** is in fact ***implicitly a float***.

The standard [arithmetic operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#Arithmetic_operators) are supported, including addition, subtraction, modulus (or remainder) arithmetic, and so forth. There's also a built-in object called [`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) that provides advanced mathematical functions and constants:

```javascript
Math.sin(3.5);
var cirsumference = 2 * Math.PI * r;
```

You can convert a string to an integer using the built-in [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) function. This takes the base for the conversion as an optional second argument:

```javascript
parseInt('123', 10); // 123
parseInt('010', 10); // 10
```

If you want to convert a binary number to an integer, just change the base:

```javascript
parseInt('11', 2); // 3
```

You can also use the unary `+` operator to convert values to numbers:

```javascript
+ '42';   // 42
+ '010';  // 10
+ '0x10'; // 16
```

A special value called [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN) (short for "Not a Number") is returned if the string is non-numeric:

```javascript
parseInt('hello', 10); // NaN
```

`NaN` is **toxic**: if you provide it as an operand to any mathematical operation, the result will also be `NaN`:

```javascript
NaN + 5; // NaN
```

You can test for `NaN` using the built-in [`isNaN()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isNaN) function:

```javascript
isNaN(NaN); // true
```

JavaScript also has the special values [`Infinity`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity) and `-Infinity`:

```javascript
1 / 0;  // Infinity
-1 / 0; // -Infinity
```

You can test for `Infinity`, `-Infinity` and `NaN` values using the built-in [`isFinite()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/isFinite) function:

```javascript
isFinite(1 / 0);     // false
isFinite(-Infinity); // false
isFinite(NaN);       // false
```

> Comparison: The [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) and [`parseFloat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) functions parse a string until they reach a character that isn't valid for the specified number format, then return the number parsed up to that point. However the "+" operator converts the string to `NaN` if there is an invalid character contained within it.

```javascript
console.log(parseInt('10.2abc'));   // 10
console.log(parseFloat('10.2abc')); // 10.2
console.log(+'10.2abc');            // NaN
```

## Strings

> Strings in JavaScript are sequences of [Unicode characters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Values,_variables,_and_literals#Unicode).

More accurately, they are sequences of UTF-16 code units; each code unit is represented by a 16-bit number. Each Unicode character is represented by either 1 or 2 code units.

To find the length of a string (in code units), access its `length` property:

```javascript
'hello'.length; // 5
```

Strings have [methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Methods) as well that allow you to manipulate the string and access information about the string:

```javascript
'hello'.charAt(0);                       // "h"
'hello, world'.replace('world', 'mars'); // "hello, mars"
'hello'.toUpperCase();                   // "HELLO"
```

## Other Types

### null & undefined

JavaScript distinguishes between [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null), which is a value that indicates a deliberate ***non-value*** (and is only accessible through the `null` keyword), and [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined), which is a value of type `undefined` that indicates an ***uninitialized*** variable — that is, ***a value hasn't even been assigned yet***. 

In JavaScript it is possible to declare a variable without assigning a value to it. If you do this, the variable's type is `undefined`. `undefined` is actually a **constant**.

### Boolean

JavaScript has a boolean type, with possible values `true` and `false`. Any value can be converted to a boolean according to the following rules:

1. `false`, `0`, empty strings (`""`), `NaN`, `null`, and `undefined` all become `false.`
2. All other values become `true.`

You can perform this conversion explicitly using the `Boolean()` function:

```javascript
Boolean('');  // false
Boolean(234); // true
```

However, this is rarely necessary, as JavaScript will silently perform this conversion when it expects a boolean, such as in an `if` statement.

Boolean operations such as `&&` (logical *and*), `||` (logical *or*), and `!` (logical *not*) are supported.

## Variables

New variables in JavaScript are declared using one of three keywords: `let`, `const`, or `var`.

**`let`** allows you to declare **block-level** variables. The declared variable is available from the *block* it is enclosed in.

```javascript
let a;
let name = 'Simon';
```

The following is an example of scope with a variable declared with **`let`**:

```javascript
// myLetVariable is *not* visible out here

for (let myLetVariable = 0; myLetVariable < 5; myLetVariable++) {
  // myLetVariable is only visible in here
}

// myLetVariable is *not* visible out here
```

**`const`** allows you to declare variables whose values are never intended to change. The variable is available from the *block* it is declared in.

```javascript
const Pi = 3.14; // variable Pi is set
Pi = 1;          // will throw an error because you cannot change a constant variable.
```

**`var`** is the most common declarative keyword. A variable declared with the **`var`** keyword is available from the ***function*** it is declared in.

```javascript
var a;
var name = 'Simon';
```

An example of scope with a variable declared with **`var`:**

```javascript
// myVarVariable *is* visible out here

for (var myVarVariable = 0; myVarVariable < 5; myVarVariable++) {
  // myVarVariable is visible to the whole function
}

// myVarVariable *is* visible out here
```

If you declare a variable without assigning any value to it, its type is `undefined`.

> **Attention**: In JavaScript, blocks do not have scope; only functions have a scope. So if a variable is defined using `var` in a compound statement (for example inside an `if` control structure), it will be visible to the entire function. However, starting with ECMAScript 2015, `let` and `const` declarations allow you to create block-scoped variables.

## Operators

JavaScript's numeric operators are `+`, `-`, `*`, `/` and `%` which is the remainder operator ([which is the same as modulo](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Remainder_()).) Values are assigned using `=`, and there are also compound assignment statements such as `+=` and `-=`.

```javascript
x += 5; // x = x + 5;
```

You can use `++` and `--` to increment and decrement respectively. These can be used as a prefix or postfix operators.

The [`+` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Addition) also does string concatenation:

```javascript
'hello' + ' world'; // "hello world"
```

If you add a string to a number (or other value) everything is converted into a string first:

```javascript
'3' + 4 + 5; // "345"
3 + 4 + '5'; // "75"
```

*Adding an empty string to something is a useful way of converting it to a string itself.*

[Comparisons](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators) in JavaScript can be made using `<`, `>`, `<=` and `>=`. These work for both *strings* and *numbers*. The double-equals operator performs **type** **coercion** if you give it different types:

```javascript
123 == '123'; // true
1 == true;    // true
```

To avoid type coercion, use the triple-equals operator:

```javascript
123 === '123'; // false
1 === true;    // false
```

There are also `!=` and `!==` operators.

JavaScript also has [bitwise operations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators).

## Control structures

Conditional statements are supported by `if` and `else`; you can chain them together if you like:

```javascript
var name = 'kittens';
if (name == 'puppies') {
  name += ' woof';
} 
else if (name == 'kittens') {
  name += ' meow';
}
else {
  name += '!';
}
name == 'kittens meow';
```

JavaScript has `while` loops and `do-while` loops. The first is good for basic looping; the second for loops where you wish to ensure that the body of the loop is executed **at least once**:

```javascript
while (true) {
  // an infinite loop!
}

var input;
do {
  input = get_input();
} 
while (inputIsNotValid(input));
```

JavaScript's [`for` loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) lets you provide the control information for your loop on a single line.

```javascript
for (var i = 0; i < 5; i++) {
  // Will execute 5 times
}
```

JavaScript also contains two other prominent for loops: [`for`...`of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

```javascript
for (let value of array) {
  // do something with value
}
```

and [`for`...`in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in):

```javascript
for (let property in object) {
  // do something with object property
}
```

The `&&` and `||` operators use **short-circuit** logic, which means whether they will execute their second operand is dependent on the first. This is useful for **checking for null objects before accessing their attributes**:

```javascript
var name = o && o.getName();
```

Or for caching values (when falsy values are invalid):

```javascript
var name = cachedName || (cachedName = getName());
```

JavaScript has a ternary operator for conditional expressions:

```javascript
var allowed = (age > 18) ? 'yes' : 'no';
```

The `switch` statement can be used for multiple branches based on a number or string:

```javascript
switch (action) {
  case 'draw':
    drawIt();
    break;
  case 'eat':
    eatIt();
    break;
  default:
    doNothing();
}
```

If you don't add a `break` statement, execution will "fall through" to the next level.

The default clause is *optional*. comparisons take place between the two using the `===` operator.

## Objects

JavaScript objects can be thought of as simple collections of **name-value pairs**.

The "name" part is a JavaScript string, while the value can be any JavaScript value — including more objects.

There are two basic ways to create an empty object:

```javascript
var obj = new Object();
```

And:

```javascript
var obj = {};
```

These are semantically equivalent; the second is called object literal syntax and is more convenient. *This syntax is also the core of JSON format and should be preferred at all times.*

Object literal syntax can be used to initialize an object in its entirety:

```javascript
var obj = {
    name: 'Carrot',
    _for: 'Max', // 'for' is a reserved word, use '_for' instead
    details: {
        color: 'orange',
        size: 12
    }
};
```

Attribute access can be chained together:

```javascript
obj.details.color;      // orange
obj['details']['size']; // 12
```

The following example creates an object prototype (`Person`) and an instance of that prototype (`you`).

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

// Define an object
var you = new Person('You', 24);
// We are creating a new person named 'You' aged 24.
```

**Once created**, an object's properties can again be accessed in one of two ways:

```javascript
// dot notation
obj.name = 'Simon';
var name = obj.name;
```

And ...

```javascript
// bracket notation
obj['name'] = 'Simon';
var name = obj['name'];

// can use a variable to define a key
var user = prompt('What is your key?')
obj[user] = prompt('What is its value?')
```

These are also semantically equivalent. The second method has the advantage that the name of the property is provided as a string, which means it can be calculated at run-time. However, using this method prevents some JavaScript engine and minifier optimizations being applied. It can also be used to set and get properties with names that are [reserved words](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords):

```javascript
obj.for = 'Simon';    // Syntax error, because 'for' is a reserved word
obj['for'] = 'Simon'; // works fine
```

For more on objects and prototypes see [Object.prototype](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype). For an explanation of object prototypes and the object prototype chains see [Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain).

## Arrays

Arrays in JavaScript are actually a special type of **object**. They work very much like regular objects (numerical properties can naturally be accessed only using `[]` syntax) but they have one magic property called '`length`'.

One way of creating arrays is as follows:

```javascript
var a = new Array();
a[0] = 'dog';
a[1] = 'cat';
a[2] = 'hen';
a.length; // 3
```

A more convenient notation is to use an array literal:

```javascript
var a = ['dog', 'cat', 'hen'];
a.length; // 3
```

Note that `array.length` isn't necessarily the number of items in the array:

```javascript
var a = ['dog', 'cat', 'hen'];
a[100] = 'fox';
a.length; // 101
```

**Remember** — *the length of the array is one more than the highest index*.

If you query a non-existent array index, you'll get a value of `undefined` in return:

```javascript
typeof a[90]; // undefined
```

If you take the above about `[]` and `length` into account, you can iterate over an array using the following `for` loop:

```javascript
for (var i = 0; i < a.length; i++) {
    // Do something with a[i]
}
```

ES2015 introduced the more concise [`for`...`of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) loop for iterable objects such as arrays:

```javascript
for (const currentValue of a) {
  // Do something with currentValue
}
```

You could also iterate over an array using a [`for`...`in`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in) loop, however this does not iterate over the array elements, but the *array indices*. Furthermore, if someone added new properties to `Array.prototype`, they would also be iterated over by such a loop. Therefore this loop type is *not recommended* for arrays.

Another way of iterating over an array that was added with ECMAScript 5 is `forEach()`:

```javascript
['dog', 'cat', 'hen'].forEach(function(currentValue, index, array)) {
    // Do something with currentValue or array[index]
};
```

If you want to append an item to an array do it like this:

```javascript
a.push(item);
```

Arrays come with a number of methods. See also the [full documentation for array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array).

| Method name                                          | Description                                                  |
| :--------------------------------------------------- | :----------------------------------------------------------- |
| `a.toString()`                                       | Returns a string with the `toString()` of each element separated by commas. |
| `a.toLocaleString()`                                 | Returns a string with the `toLocaleString()` of each element separated by commas. |
| `a.concat(item1[, item2[, ...[, itemN]]])`           | Returns a new array with the items added on to it.           |
| `a.join(sep)`                                        | Converts the array to a string — with values delimited by the `sep` param |
| `a.pop()`                                            | Removes and returns the last item.                           |
| `a.push(item1, ..., itemN)`                          | Appends items to the end of the array.                       |
| `a.shift()`                                          | Removes and returns the first item.                          |
| `a.unshift(item1[, item2[, ...[, itemN]]])`          | Prepends items to the start of the array.                    |
| `a.slice(start[, end])`                              | Returns a sub-array.                                         |
| `a.sort([cmpfn])`                                    | Takes an optional comparison function.                       |
| `a.splice(start, delcount[, item1[, ...[, itemN]]])` | Lets you modify an array by deleting a section and replacing it with more items. |
| `a.reverse()`                                        | Reverses the array.                                          |

## Functions

Along with objects, functions are the core component in understanding JavaScript. The most basic function couldn't be much simpler:

```javascript
function add(x, y) {
    var total = x + y;
    return total;
}
```

A JavaScript function can take 0 or more named parameters. The function body can contain as many statements as you like and can declare its own variables which are local to that function. The `return` statement can be used to return a value at any time, terminating the function. If no return statement is used (or an empty return with no value), JavaScript returns `undefined`.

The named parameters turn out to be more like guidelines than anything else. 

You can call a function without passing the parameters it expects, in which case they will be set to `undefined`.

```javascript
add(); // NaN
// You can't perform addition on undefined
```

You can also pass in more arguments than the function is expecting:

```javascript
add(2, 3, 4); // 5
// added the first two; 4 was ignored
```

Functions have access to an additional variable inside their body called [`arguments`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments), which is an array-like object holding all of the values passed to the function. Let's re-write the add function to take as many values as we want:

```javascript
function add() {
    var sum = 0;
    for (var i = 0, j = arguments.length; i < j; i++) {
        sum += arguments[i];
    }
    return sum;
}
add(2, 3, 4, 5); // 14
```

That's really not any more useful than writing `2 + 3 + 4 + 5` though. Let's create an averaging function:

```javascript
function avg() {
    var sum = 0;
    for (var i = 0, j = arguments.length; i < j; i++) {
        sum += arguments[i];
    }
    return sum / arguments.length;
}

avg(2, 3, 4, 5); // 3.5
```

To reduce this code a bit more we can look at substituting the use of the arguments array through [Rest parameter syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters). The **rest parameter operator** is used in function parameter lists with the format: **...variable** and it will include within that variable the entire list of uncaptured arguments that the function was called with. We will also replace the **for** loop with a **for...of** loop to return the values within our variable.

```javascript
function avg(...args) {
    var sum = 0;
    for (let value of args) {
        sum += value;
    }
    return sum / args.length;
}

avg(2, 3, 4, 5); // 3.5
```

> In the above code, the variable **args** holds all the values that were passed into the function.
>
> It is important to note that wherever the rest parameter operator is placed in a function declaration it will store all arguments *after* its declaration, but not before. *i.e. function* *avg(***firstValue,** *...args)* will store the first value passed into the function in the **firstValue** variable and the remaining arguments in **args**. That's another useful language feature but it does lead us to a new problem. The `avg()` function takes a comma-separated list of arguments — but what if you want to find the average of an array? You could just rewrite the function as follows:

```javascript
function avgArray(arr) {
    var sum = 0;
    for (var i = 0, j = arr.length; i < j; i++) {
        sum += arr[i];
    }
    return sum / arr.length;
}

avgArray([2, 3, 4, 5]); // 3.5
```

But it would be nice to be able to reuse the function that we've already created. Luckily, JavaScript lets you call a function with an arbitrary array of arguments, using the [`apply()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) method of any function object.

```javascript
avg.apply(null, [2, 3, 4, 5]); // 3.5
```

The second argument to `apply()` is the array to use as arguments; the first will be discussed later on. This emphasizes the fact that functions are objects too.

> You can achieve the same result using the [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) in the function call.
>
> For instance: `avg(...numbers)`

JavaScript lets you create **anonymous** functions.

```javascript
var avg = function() {
    var sum = 0;
    for (var i = 0, j = arguments.length; i < j; i++) {
        sum += arguments[i];
    }
    return sum / arguments.length;
};
```

This is semantically equivalent to the `function avg()` form. It's extremely powerful, as it lets you put a full function definition anywhere that you would normally put an expression. This enables all sorts of clever tricks. Here's a way of "hiding" some local variables:

```javascript
var a = 1;
var b = 2;

(function() {
  var b = 3;
  a += b;
})();

a; // 4
b; // 2
```

JavaScript allows you to call functions recursively. This is particularly useful for dealing with tree structures, such as those found in the browser DOM.

```javascript
function countChars(elm) {
  if (elm.nodeType == 3) { // TEXT_NODE
    return elm.nodeValue.length;
  }
  var count = 0;
  for (var i = 0, child; child = elm.childNodes[i]; i++) {
    count += countChars(child);
  }
  return count;
}
```

This highlights a potential problem with anonymous functions: how do you call them recursively if they don't have a name? JavaScript lets you name function expressions for this. You can use named [IIFEs (Immediately Invoked Function Expressions)](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) as shown below:

```javascript
var charsInBody = (function counter(elm)) {
    if (elm.nodeType == 3) {
    	return elm.nodeValue.length;
	}
	var count = 0;
	for (var i = 0, child; child = elm.childNodes[i]; i++) {
        count += counter(child);
    }
	return count;
})(document.body);
```

The name provided to a function expression as above is only available to the function's own scope. This allows more optimizations to be done by the engine and results in more readable code. The name also shows up in the debugger and some stack traces, which can save you time when debugging.

Note that JavaScript functions are themselves objects — like everything else in JavaScript — and you can add or change properties on them just like we've seen earlier in the Objects section.

## Custom objects

> For a more detailed discussion of object-oriented programming in JavaScript, see [Introduction to Object-Oriented JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript).

In classic Object Oriented Programming, objects are collections of data and methods that operate on that data. JavaScript is a **prototype-based** language that contains no class statement, as you'd find in C++ or Java. Instead, JavaScript uses functions as classes. 

Let's consider a person object with first and last name fields. There are two ways in which the name might be displayed: as "first last" or as "last, first". Using the functions and objects that we've discussed previously, we could display the data like this:

```javascript
function makePerson(first, last) {
    return {
        first: first,
        last: last
    };
}
function personFullName(person) {
    return person.first + ' ' + person.last;
}
function personFullNameReserved(person) {
    return person.last + ', ' + person.first;
}

var s = makePerson('Simon', 'Willison');
personFullName(s);         // 'Simon Willison'
personFullNameReserved(s); // 'Willison, Simon'
```

This works, but it's pretty ugly. You end up with dozens of functions in your global namespace. What we really need is a way to attach a function to an object. Since functions are objects, this is easy:

```javascript
function makePerson(first, last) {
  return {
    first: first,
    last: last,
    fullName: function() {
      return this.first + ' ' + this.last;
    },
    fullNameReversed: function() {
      return this.last + ', ' + this.first;
    }
  };
}

var s = makePerson('Simon', 'Willison');
s.fullName();         // "Simon Willison"
s.fullNameReversed(); // "Willison, Simon"
```

Note on the `this` keyword. Used inside a function, `this` refers to the current object. What that actually means is specified by the way in which you called that function. If you called it using [dot notation or bracket notation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Accessing_properties) on an object, that object becomes `this`. If dot notation wasn't used for the call, `this` refers to the global object.

Note that `this` is a frequent cause of mistakes. For example:

```javascript
var s = makePerson('Simon', 'Willison');
var fullName = s.fullName;
fullName(); // undefined undefined
```

When we call `fullName()` alone, without using `s.fullName()`, `this` is bound to the global object. Since there are no global variables called `first` or `last` we get `undefined` for each one.

We can take advantage of the `this` keyword to improve our `makePerson` function:

```javascript
function Person(first, last) {
    this.first = first;
    this.last = last;
    this.fullName = function() {
        return this.first + ' ' + this.last;
    };
    this.fullNameReserved = function() {
        return this.last + ', ' + this.last;
    };
}
var s = new Person('Simon', 'Willison');
```

We have introduced another keyword: `new`. `new` is strongly related to `this`. It creates a brand new empty object, and then calls the function specified, with `this` set to that new object. 

Notice though that the function specified with `this` does not return a value but merely modifies the `this` object. It's `new` that returns the `this` object to the calling site. Functions that are designed to be called by `new` are called constructor functions. Common practice is to capitalize these functions as a reminder to call them with `new`.

The improved function still has the same pitfall with calling `fullName()` alone. Our person objects are getting better, but there are still some ugly edges to them. Every time we create a person object we are creating two brand new function objects within it — wouldn't it be better if this code was shared?

```javascript
function personFullName() {
  return this.first + ' ' + this.last;
}
function personFullNameReversed() {
  return this.last + ', ' + this.first;
}
function Person(first, last) {
  this.first = first;
  this.last = last;
  this.fullName = personFullName;
  this.fullNameReversed = personFullNameReversed;
}
```

That's better: we are creating the method functions only once, and assigning references to them inside the constructor. Can we do any better than that? The answer is yes:

```javascript
function Person(first, last) {
    this.first = first;
    this.last = last;
}
Person.prototype.fullName = function() {
    return this.first + ' ' + this.last;
};
Person.prototype.fullNameReversed = function() {
    return this.last + ', ' + this.first;
};
```

`Person.prototype` is an object shared by all instances of `Person`. It forms part of a lookup chain (that has a special name, "prototype chain"): any time you attempt to access a property of `Person` that isn't set, JavaScript will check `Person.prototype` to see if that property exists there instead. As a result, anything assigned to `Person.prototype` becomes available to all instances of that constructor via the `this` object.

This is an incredibly powerful tool. JavaScript lets you modify something's prototype at any time in your program, which means you can add extra methods to existing objects at runtime:

```javascript
var s = new Person('Simon', 'Willison');
s.firstNameCaps(); // TypeError on line 1: s.firstNameCaps is not a function

Person.prototype.firstNameCaps = function() {
    return this.first.toUpperCase();
};
s.firstNameCaps(); // "SIMON"
```

Interestingly, you can also add things to the prototype of built-in JavaScript objects. Let's add a method to `String` that returns that string in reverse:

```javascript
var s = 'Simon';
s.reversed(); // TypeError on line 1: s.reversed is not a function

String.prototype.reserved = function() {
    var r = '';
    for (var i = this.length - 1; i >= 0; i--) {
        r += this[i];
    }
    return r;
};

s.reversed(); // nomiS
```

Our new method even works on string literals!

```javascript
'This can now be reversed'.reversed(); // desrever eb won nac sihT
```

As mentioned before, the prototype forms part of a chain. The root of that chain is `Object.prototype`, whose methods include `toString()` — it is this method that is called when you try to represent an object as a string. This is useful for debugging our `Person` objects:

```javascript
var s = new Person('Simon', 'Willison');
s.toString(); // [object Object]

Person.prototype.toString = function() {
    return '<Person: ' + this.fullName() + '>';
};

s.toString(); // "<Person: Simon Willison>"
```

Remember how `avg.apply()` had a null first argument? We can revisit that now. The first argument to `apply()` is the object that should be treated as '`this`'. For example, here's a trivial implementation of `new`:

```javascript
function trivialNew(constructor, ...args) {
  var o = {}; // Create an object
  constructor.apply(o, args);
  return o;
}
```

This isn't an exact replica of `new` as it doesn't set up the prototype chain (it would be difficult to illustrate). This is not something you use very often, but it's useful to know about. In this snippet, `...args` (including the ellipsis) is called the "[rest arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)" — as contains the rest of the arguments.

Calling

```javascript
var bill = trivialNew(Person, 'William', 'Orange');
```

is therefore almost equivalent to

```javascript
var bill = new Person('William', 'Orange');
```

`apply()` has a sister function named [`call`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call), which again lets you set `this` but takes an expanded argument list as opposed to an array.

```javascript
function lastNameCaps() {
    return this.last.toUpperCase();
}
var s = new Person('Simon', 'Willison');
lastNameCaps.call(s);
// Is the same as:
s.lastNameCaps = lastNameCaps;
s.lastNameCaps(); // WILLISON
```

## Inner functions

JavaScript function declarations are allowed inside other functions. We've seen this once before, with an earlier `makePerson()` function. An important detail of nested functions in JavaScript is that they can ***access variables in their parent function's scope***:

```javascript
function parentFunc() {
    var a = 1;
    
    function nestedFunc() {
        var b = 4; // parentFunc can't use this
        return a + b;
    }
    return nestedFunc(); // 5
}
```

This provides a great deal of utility in writing more maintainable code. If a called function relies on one or two other functions that are not useful to any other part of your code, you can nest those utility functions inside it. This keeps the number of functions that are in the global scope down, which is always a good thing.

This is also a great counter to the lure of global variables. When writing complex code it is often tempting to use global variables to share values between multiple functions — which leads to code that is hard to maintain. Nested functions can share variables in their parent, so you can use that mechanism to couple functions together when it makes sense without polluting your global namespace — "local globals" if you like. This technique should be used with caution, but it's a useful ability to have.

## Closures

This leads us to one of the most powerful abstractions that JavaScript has to offer — but also the most potentially confusing. What does this do?

```javascript
function makeAdder(a) {
    return function(b) {
        return a + b;
    };
}
var add5 = makeAdder(5);
var add20 = makeAdder(20);
add5(6); // ?
add20(7); // ?
```

The name of the `makeAdder()` function should give it away: it creates new 'adder' functions, each of which, when called with one argument, adds it to the argument that it was created with.

What's happening here is pretty much the same as was happening with the inner functions earlier on: a function defined inside another function has access to the outer function's variables. The only difference here is that the outer function has returned, and hence common sense would seem to dictate that its local variables no longer exist. But they *do* still exist — otherwise, the adder functions would be unable to work. What's more, there are two different "copies" of `makeAdder()`'s local variables — one in which `a` is 5 and the other one where `a` is 20. So the result of that function calls is as follows:

```javascript
add5(6); // returns 11
add20(7); // returns 27
```

Here's what's actually happening. 

Whenever JavaScript executes a function, a **'scope' object** is created to hold the local variables created within that function. It is initialized with any variables passed in as function parameters. This is similar to the global object that all global variables and functions live in, but with a couple of important differences: 

- firstly, a brand new scope object is created every time a function starts executing, 
- and secondly, unlike the global object (which is accessible as `this` and in browsers as `window`) these scope objects cannot be directly accessed from your JavaScript code. There is no mechanism for iterating over the properties of the current scope object, for example.

So when `makeAdder()` is called, a scope object is created with one property: `a`, which is the argument passed to the `makeAdder()` function. `makeAdder()` then returns a newly created function. Normally JavaScript's garbage collector would clean up the scope object created for `makeAdder()` at this point, but the returned function maintains a reference back to that scope object. As a result, the scope object will not be garbage-collected until there are no more references to the function object that `makeAdder()` returned.

Scope objects form a chain called the ***scope chain***, similar to the prototype chain used by JavaScript's object system.

A **closure** is the combination of a function and the scope object in which it was created. Closures let you save state — as such, they can often be used in place of objects. You can find [several excellent introductions to closures](http://stackoverflow.com/questions/111102/how-do-javascript-closures-work).

\- End -
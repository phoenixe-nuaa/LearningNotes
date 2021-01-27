# [Javascript Review](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)

## Overview

JavaScript is a multi-paradigm, dynamic language with types and operators, standard built-in objects, and methods. Its syntax is based on the Java and C languages — many structures from those languages apply to JavaScript as well. JavaScript supports ***object-oriented programming*** with object **prototypes**, instead of classes (see more about [prototypical inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) and ES2015 [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)). JavaScript also supports **functional programming** — because they are objects, functions may be stored in variables and passed around like any other object.

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

### Numbers

> Numbers in JavaScript are double-precision 64-bit format IEEE 754 values".

according to the spec — There's no such thing as an **integer** in JavaScript (except [`BigInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)), so you have to be a little careful.

```javascript
console.log(3 / 2);				// 1.5, not 1
console.log(Math.floor(3 / 2)); // 1
```

So an ***apparent integer*** is in fact ***implicitly a float***.

In practice, integer values are treated as 32-bit ints. This can be important for bit-wise operations.

The standard [arithmetic operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#Arithmetic_operators) are supported, including addition, subtraction, modulus (or remainder) arithmetic, and so forth. There's also a built-in object that we did not mention earlier called [`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) that provides advanced mathematical functions and constants:

```javascript
Math.sin(3.5);
var cirsumference = 2 * Math.PI * r;
```

You can convert a string to an integer using the built-in [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) function. This takes the base for the conversion as an optional second argument, which you should always provide:

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
+ '42'; // 42
+ '010'; // 10
+ '0x10'; // 16
```

A special value called [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN) (short for "Not a Number") is returned if the string is non-numeric:

```javascript
parseInt('hello', 10); // NaN
```

`NaN` is toxic: if you provide it as an operand to any mathematical operation, the result will also be `NaN`:

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
isFinite(1 / 0);  	 // false
isFinite(-Infinity); // false
isFinite(NaN);       // false
```

> The [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) and [`parseFloat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) functions parse a string until they reach a character that isn't valid for the specified number format, then return the number parsed up to that point. However the "+" operator converts the string to `NaN` if there is an invalid character contained within it.

```javascript
console.log(parseInt('10.2abc'));   // 10
console.log(parseFloat('10.2abc')); // 10.2
console.log(+'10.2abc');            // NaN
```

### Strings

> Strings in JavaScript are sequences of [Unicode characters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Values,_variables,_and_literals#Unicode).

More accurately, they are sequences of UTF-16 code units; each code unit is represented by a 16-bit number. Each Unicode character is represented by either 1 or 2 code units.

To find the length of a string (in code units), access its `length` property:

```javascript
'hello'.length; // 5
```

Strings have [methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Methods) as well that allow you to manipulate the string and access information about the string:

```javascript
'hello'.charAt(0); // "h"
'hello, world'.replace('world', 'mars'); // "hello, mars"
'hello'.toUpperCase(); // "HELLO"
```

### Other Types

#### null & undefined

JavaScript distinguishes between [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null), which is a value that ***indicates a deliberate non-value*** (and is only accessible through the `null` keyword), and [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined), which is a value of type `undefined` that indicates an uninitialized variable — that is, ***a value hasn't even been assigned yet***. 

In JavaScript it is possible to declare a variable without assigning a value to it. If you do this, the variable's type is `undefined`. `undefined` is actually a constant.

#### Boolean

JavaScript has a boolean type, with possible values `true` and `false`. Any value can be converted to a boolean according to the following rules:

1. `false`, `0`, empty strings (`""`), `NaN`, `null`, and `undefined` all become `false.`
2. All other values become `true.`

You can perform this conversion explicitly using the `Boolean()` function:

```javascript
Boolean('');  // false
Boolean(234); // true
```

However, this is rarely necessary, as JavaScript will silently perform this conversion when it expects a boolean, such as in an `if` statement (see below).

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
Pi = 1; // will throw an error because you cannot change a constant variable.
```

**`var`** is the most common declarative keyword. A variable declared with the **`var`** keyword is available from the *function* it is declared in.

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

JavaScript's numeric operators are `+`, `-`, `*`, `/` and `%` which is the remainder operator ([which is the same as modulo](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Remainder_()).) Values are assigned using `=`, and there are also compound assignment statements such as `+=` and `-=`. These extend out to `x = x (operator) y`.

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

[Comparisons](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators) in JavaScript can be made using `<`, `>`, `<=` and `>=`. These work for both *strings* and *numbers*. Equality is a little less straightforward. The double-equals operator performs **type** **coercion** if you give it different types:

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

JavaScript has `while` loops and `do-while` loops. The first is good for basic looping; the second for loops where you wish to ensure that the body of the loop is executed at least once:

```javascript
while (true) {
  // an infinite loop!
}

var input;
do {
  input = get_input();
} while (inputIsNotValid(input));
```

JavaScript's [`for` loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) is the same as that in C and Java: it lets you provide the control information for your loop on a single line.

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

The `&&` and `||` operators use short-circuit logic, which means *whether they will execute their second operand is dependent on the first*. This is useful for **checking for null objects before accessing their attributes**:

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

The default clause is *optional*. You can have expressions in both the switch part and the cases if you like; comparisons take place between the two using the `===` operator.
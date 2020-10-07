---
title: JavaScript Basics
tags: [Programming Language, JavaScript]
---

Read a little history of JavaScript [here](https://en.wikipedia.org/wiki/JavaScript#History).

## Miscellaneous

- Display content
    - In the console: `console.log('Hello, world!');`
    - Show alert message: `alert('Hello, world!');`
- Comments
    - Single-line comment: `//`
    - Multi-line comment: `/* */`
- Semicolons
    - One statement ends and another begins: `;`
- Variables
    - Naming conventions: `camelCase`

## Data Types

- Numbers
    - Arithmetic operators: `+`, `-`, `*`, `/`, `%`
    - Comparison operators: `<`, `>`, `<=`, `>=`, `==`, `!=`
- Strings
    - String literals: `" "`, `' '`
    - String concatenation: `+`
    - String index: `'James'[0];`
    - Escaping strings: `\\`, `\"`, `\'`, `\n`, `\t`
    - Comparing strings: character-by-character for the ASCII values (`charCodeAt(0)`)
- Booleans
    - `true`
    - `false`
- Null, Undefined, and NaN
    - Value of nothing: `null`
    - Absence of value: `undefined`
    - Not-A-Number: `NaN`
- Equality
    - Implicit type coercion: `'1' == 1`, `0 == false`, `' ' == false` (Returns: `true`)
    - Strict equality: `===`, `!==`

## Conditions

- If...else statements

    ```js
  if (/* this expression is true */) {
      // run this code
  } else {
      // run this code
  }
    ```

- Else ff statements

    ```js
  if (/* this expression is true */) {
      // run this code
  } else if (/* exra conditional statement */) {
      // run this code
  } else {
      // run this code
  }
    ```

- Logical operators
    - Logical AND: `&&`
    - Logical OR: `||`
    - Logical NOT: `!`
- Short-circuiting
    - `false && _` returns `false`
    - `true || _` returns `true`
- Truthy and falsy
    - Only six falsy values in all JavaScript
        - The Boolean value `false`
        - The `null` type
        - The `undefined` type
        - The number `0`
        - The empty string `''`
        - The odd value `NaN`
    - It's truthy if it's not in the list of falsy values
- Ternary operator
    - `conditional ? (if condition is true) : (if condition is false)`
- Switch statement

    ```js
  switch (option) {
      case 1:
          console.log('You selected option 1.');
          break;
      case 2:
          console.log('You selected option 2.');
          break;
      case 3:
          console.log('You selected option 3.');
          break;
      default:
          console.log('You selected option ' + option + '.');
  }
    ```

## Loops

- While loop

    ```js
  var start = 0; // when to start
  while (start < 10) { // when to stop
      console.log(start);
      start = start + 2; // how to get to the next item
  }
    ```

- For loop

    ```js
  for (start; stop; step) {
      // do this thing
  }
    ```

- Increment and decrement
    - `x++ or ++x // same as x = x + 1`
    - `x-- or --x // same as x = x - 1`
    - `x += 3 // same as x = x + 3`
    - `x -= 6 // same as x = x - 6`
    - `x *= 2 // same as x = x * 2`
    - `x /= 5 // same as x = x / 5`

## Scope and Variable Declaration

- Global scope
    - When a particular variable is visible (can be used) anywhere in the code.
- Function scope
    - When a particular variable is visible (can be used) within a particular function only.
- Block scope
    - When a particular variable is visible (can be used) within a pair of { . . . } only.

```js
/*
 * Global scope.
 * This variable declared outside of any function is called Global variable.
 * Hence, you can use this anywhere in the code
 */
var opinion = 'This nanodegree is amazing';

// Function scope
function showMessage() {
    // Local variable, visible within the function `showMessage`
    var message = 'I am an Udacian!';

    // Block scope
    {
          let greet = 'How are you doing?';
        /*
         * We have used the keyword `let` to declare a variable `greet` because variables declared with the `var` keyword can not have Block Scope.
         */
    } // block scope ends

    console.log( message ); // OK
    console.log( greet ); // ERROR.
    // Variable greet can NOT be used outside the block

    console.log( opinion ); // OK    to use the gobal variable anywhere in the code

} // function scope ends
```

- Variable declaration
    - `let`
        - It is a new way to declare a variable in any scope. The value of this variable can be changed or reassigned anytime within its scope.
    - `const`
        - It is also a way to declare constants in any scope. Once you are assigned a value to a const variable, the value of this variable CANNOT be changed or reassigned throughout the code.
    - `var`
        - This is the old way of declaring variables in only two scope — Global, or Local. Variables declared with the var keyword can not have Block Scope. The value of this variable can be changed or reassigned anytime within its scope.

## Functions

```js
// x and y are parameters in this function declaration
function add(x, y) {
    // function body
    // Here, `sum` variable has a scope within the function.
    // Such variables defined within a function are called Local variables
    // You can try giving it another name
    var sum = x + y;
    return sum; // return statement
}

// 1 and 2 are passed into the function as arguments,
// and the result returned by the function is stored in a new variable `sum`
// Here, `sum` is another variable, different from the one used inside the function
var sum = add(1, 2);
```

- Function body

    ```js
  function add(x, y) {
      // function body!
  }
    ```

- Return statements

    ```js
  return sum;
    ```

- Invoke or call a function

    ```js
  add(1, 2);
    ```

### Hoistinng

- Examples

    ```js
  sayHi('Julia');
  function sayHi(name) {
      console.log(greeting + ' ' + name);
      var greeting = 'Hello';
  }
    ```

    > undefined Julia

    ```js
  sayHi('Julia');
  function sayHi(name) {
      console.log(greeting + ' ' + name);
      var greeting = 'Hello';
  }
    ```

    > undefined Julia

    ```js
  function sayHi(name) {
      var greeting = 'Hello';
      console.log(greeting + ' ' + name);
  }
  sayHi('Julia');
    ```

    > Hello Julia

- JavaScript hoists function declarations and variable declarations to the top of the current scope.
- Variable assignments are not hoisted.
- Declare functions and variables at the top of your scripts, so the syntax and behavior are consistent with each other.

### Function Expressions

**Function Expression**: When a function is assigned to a variable. The function can be named, or anonymous. Use the variable name to call a function defined in a function expression.

```js
// anonymous function expression
var doSomething = function(y) {
    return y + 1;
};
```

```js
// named function expression
var doSomething = function addOne(y) {
    return y + 1;
};
```

```js
// for either of the definitions above, call the function like this:
doSomething(5);
```

> All *function declarations* are hoisted and loaded before the script is actually run. *Function expressions* are not hoisted, since they involve variable assignment, and only variable declarations are hoisted. The function expression will not be loaded until the interpreter reaches it in the script.

**Callback**: A function that is passed into another function.

```js
// function expression catSays
var catSays = function(max) {
    var catMessage = '';
    for (var i = 0; i < max; i++) {
      catMessage += 'meow ';
    }
    return catMessage;
};

// function declaration helloCat accepting a callback
function helloCat(callbackFunc) {
    return 'Hello ' + callbackFunc(3);
}

// pass in catSays as a callback function
helloCat(catSays);
```

**Inline function expressions**: Writing function expressions that pass a function into another function inline.

```js
// function declaration that takes in two arguments: a function for displaying
// a message, along with a name of a movie
function movies(messageFunction, name) {
    messageFunction(name);
}

// call the movies function, pass in the function and name of movie
movies(function displayFavorite(movieName) {
    console.log('My favorite movie is ' + movieName);
}, 'Finding Nemo');
```

## Arrays

```js
// creates a `donuts` array with three strings
var donuts = ['glazed', 'powdered', 'jelly'];
```

```js
// creates a `mixedData` array with mixed data types
var mixedData = ['abcd', 1, true, undefined, null, 'all the things'];
```

```js
// creates a `arraysInArrays` array with three arrays
var arraysInArrays = [[1, 2, 3], ['Julia', 'James'], [true, false, true, false]];
```

### Indexing

```js
var donuts = ['glazed', 'powdered', 'sprinkled'];
console.log(donuts[0]); // 'glazed' is the first element in the `donuts` array
```

```js
donuts[1] = 'glazed cruller'; // changes the second element in the `donuts` array to 'glazed cruller'
```

### Properties and Methods

- Array.length

    ```js
  var donuts = ['glazed', 'powdered', 'sprinkled'];
  console.log(donuts.length);
    ```

    > 3

- Push

    ```js
  var donuts = ['glazed', 'chocolate frosted', 'Boston creme', 'glazed cruller', 'cinnamon sugar', 'sprinkled'];
  donuts.push('powdered'); // pushes 'powdered' onto the end of the `donuts` array, and returns the length of the array after an element has been added
    ```

    > 7

- Pop

    ```js
  var donuts = ['glazed', 'chocolate frosted', 'Boston creme', 'glazed cruller', 'cinnamon sugar', 'sprinkled', 'powdered'];
  donuts.pop(); // pops 'powdered' off the end of the `donuts` array, and returns the element that has been removed
    ```

    > powdered

- Splice
    - `arrayName.splice(start, deleteCount, item1, ..., itemX);`
        - `start` Mandatory argument. Specifies the starting index position to add/remove items. You can use a negative value to specify the position from the end of the array e.g., -1 specifies the last element.
        - `deleteCount` Optional argument. Specifies the count of elements to be removed. If set to 0, no items will be removed.
        - `item1, ..., itemX` are the items to be added at index position `start`
        - Returns the item(s) that were removed.

### Array Loops

- `for` or `while` loop

    ```js
  var donuts = ['jelly donut', 'chocolate donut', 'glazed donut'];
  // the variable `i` is used to step through each element in the array
  for (var i = 0; i < donuts.length; i++) {
      donuts[i] += ' hole';
      donuts[i] = donuts[i].toUpperCase();
  }
    ```

- `forEach()`

    ```js
  var donuts = ['jelly donut', 'chocolate donut', 'glazed donut'];

  donuts.forEach(function(donut) {
      donut += ' hole';
      donut = donut.toUpperCase();
      console.log(donut);
  });
    ```

    The function passed to the `forEach()` method can take up to three parameters: element, index, and array. It always returns `undefined`.

    ```js
  words = ['cat', 'in', 'hat'];
  words.forEach(function(word, num, all) {
      console.log('Word ' + num + ' in ' + all.toString() + ' is ' + word);
  });
    ```

    > Word 0 in cat,in,hat is cat
    >
    > Word 1 in cat,in,hat is in
    >
    > Word 2 in cat,in,hat is hat

- `map()`

    The `map()` method takes an array, performs some operation on each element of the array, and returns a new array.

    ```js
  var donuts = ['jelly donut', 'chocolate donut', 'glazed donut'];

  var improvedDonuts = donuts.map(function(donut) {
      donut += ' hole';
      donut = donut.toUpperCase();
      return donut;
  });
    ```

## Objects

```js
var sister = {
    name: 'Sarah',
    age: 23,
    parents: ['alice', 'andy'],
    siblings: ['julia'],
    favoriteColor: 'purple',
    pets: true,
    paintPicture: function() { return 'Sarah   paints!'; }
};

// two equivalent ways to use the key to return its value
sister['parents'] // returns ['alice', 'andy']
sister.parents // also returns ['alice', 'andy']

sister.paintPicture();
```

## References

[Intro to JavaScript](https://www.udacity.com/course/intro-to-javascript--ud803)

# Javascript and introduction to web development style-guide

## Why?
There are style-guides already available for JavaScript and indeed, at least 3 tools for linting and verifying styles.  Why do we need another?

This styleguide is different in that it is focused on teaching & learning JS, starting with front-end JavaScript and CSS and eventually teaching jQuery, Node, Express and friends. When we teach we generally increase complexity over time. This guide aims to set guidelines to ensure that syntax across lessons is the same, while slowly introducing additional syntactical complexity.

**tldr; This style-guide is intended to make learning easier for students new to JavaScript by increasing syntactical complexity slowly, over time.**



## CSS

#### Use two-spaces for indentation

```css
/* avoid tabs */
.ridiculously-huge {
	font-size: 100px;
}

/* recommended */
.ridiculously-huge {
  font-size: 100px;
}
```

## Introductory JS

<a name="use-triple-equals"></a>
#### Choose `===` and `!==` over `==` and `!=`. <sup>[[link](#use-triple-equals)]</sup>

#### Use braces around any multi-line blocks.

```js
// avoid
if (test)
  return false;

// recommended - single line
if (test) return false;

// recommended - multi-line
if (test) {
  return false;
}
```

#### Use literal array syntax for array creation

```js
// avoid
var stuff = new Array();

// recommended
var items = [];
```

#### Use array.push to add elements rather than index access

```js
// avoid
someStuff[someStuff.length] = 'new todo';

// recommended
someStuff.push('new todo');
```

#### When returning multiple values, use object destructuring, not arrays

* It's safer if the return values are ever extended.
* Avoids mistakes in handling return value order.

```js
// avoid
function processInput(input) {
  // ...
  return [strangeness, spin, charm];
}

// recommended
function processInput(input) {
  // ...
  return { strangeness: strangeness,
           spin: spin,
           charm: charm };
}
```

#### Limit lines to around 100 characters and use multi-line String concatenation with +

Strings that cause the line to exceed ~100 characters should be concatenated across multiple lines.

```js
// avoid
var html = '<ol><li>Of shoes</li><li>and ships</li><li>and sealing-wax</li><li>Of cabbages</li><li>and kings</li></ol>'; // Lewis Carroll

// less bad
var html = '<ol> \
              <li>Of shoes</li> \
              <li>and ships</li> \
              <li>and sealing-wax</li> \
              <li>Of cabbages</li> \
              <li>and kings</li> \
              </ol>'; // Lewis Carroll


// recommended
var html = (
    '<ol>' +
      '<li>Of shoes</li>' +
      '<li>and ships</li>' +
      '<li>and sealing-wax</li>' +
      '<li>Of cabbages</li>' +
      '<li>and kings</li>' +
    '</ol>' // Lewis Carroll
);
```


### Whitespace

#### Use soft tabs set to 2 spaces.

#### Use one-space before a leading brace.
```js
// avoid
function yay(){
  console.log('yay');
}

// recommended
function yay() {
  console.log('yay');
}
```

#### Place 1 space before the opening parenthesis in control statements (if, while etc.).

```js
// avoid
if(x === y) {
  return x;
}

// recommended
if (x === y) {
  return x;
}
```

#### Use spaces around operators

```js
// avoid
a=b+c-(d/2);

// recommended
a = b + c - (d / 2);
```

#### Place no space between the argument list and the function name in function calls and declarations.

```js
// avoid
function addNumbers( a, b ) { ... }

// recommended
function addNumbers(a,b) { ... }
```

function calls:

```js
// avoid
var addedNumbers = addNumbers( 1, x );

// recommended
var addedNumbers = addNumbers(1, x);
```

##### When using anonymous functions a space is OK

```js
// OK
var saute = function () { ... };

// also OK
var saute = function() { ... };
```

#### Use one space after a comment marker and indent multiple lines

```js
// avoid
//my comment with no space

// avoid
/* This is a comment that doesn't
use indentation when continuing a sentence. */

// recommended
// my comment with a space

/* This is a comment that does
     use indentation when continuing a sentence. */
```


#### Use leading-dots and indentation when making long method chains.

```js
// avoid
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount();

// recommended
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

```

> Example lifted from airbnb style-guide.







### Functions

#### Prefer named functions over anonymous functions

> This rule is particularly important in the first 2-3 weeks of the course.  However, we should eventually introduce and use anonymous functions.

```js
// avoid
$.get(url).success(function(result) {
  console.log(result);
});

// recommended
$.get(url).success(handleIndexResult(result));

// sometimes OK
$.get(url).success(function handleIndexResult(result) {
  console.log(result);
});

```

#### Use function declarations instead of function expressions. jscs: requireFunctionDeclarations

* Function declarations are fully hoisted.

```js
// bad
var boilPasta = function () {
};

// recommended
function boilPasta() {
}
```

#### Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead.


#### Never name a parameter `arguments`.

* It overrides the arguments object the function receives.

```js
// avoid
function nope(name, options, arguments) {
  // ...stuff...
}

// recommended
function yup(name, options, args) {
  // ...stuff...
}
```

> lifted from airbnb

#### Classes or objects that will be `new`ed should use Dromedary Case

```js
// avoid
function cat() {
  this.sound = 'meow';
}

var kitten = new cat;

// recommended
function Cat() {
}

var kitten = new Cat;
```

## jQuery

### Big over-arching example:
<sup>Thanks Justin</sup>

```js
// Using the core $.ajax() method
$.ajax({

    // The URL for the request
    url: "post.php",

    // The data to send (will be converted to a query string)
    data: {
        id: 123
    },

    // Whether this is a POST or GET request
    type: "GET",

    // The type of data we expect back
    dataType : "json",

    // Code to run if the request succeeds;
    // the response is passed to the function
    success: onSuccess,

    // Code to run if the request fails; the raw request and
    // status codes are passed to the function
    error: onError,

    // Code to run regardless of success or failure
    complete: onComplete
});

function onSuccess( json ) {
    $("<h1>").text(json.title).appendTo("body");
    $("<div class=\"content\">").html(json.html).appendTo("body");
}

function onError(xhr, status, errorThrown) {
    alert("Sorry, there was a problem!");
    console.log("Error: " + errorThrown);
    console.log("Status: " + status);
    console.dir(xhr);
}

function onComplete( xhr, status ) {
    alert("The request is complete!");
}
```

#### Use long form `$.ajax` over `$.get` and `$.post`

* This makes the syntax identical for the more difficult PUT and DELETE operations.
* Eventually students will discover the short-cut methods on their own; we should allow them to use them at that point.

```js
// avoid
$.get("http://pokemon.com/api/pokemon").success(renderPokemonList(json));

// recommended
$.ajax({
    url: "http://pokemon.com/api/pokemon",
    type: "GET",
    dataType : "json",
    success: renderPokemonList,
    error: handlePokemonIndexError,
});

function renderPokemonList(json) { ... }
```

#### Use named, out-of-line functions for jQuery callbacks; introduce anonymous functions later.

* In-line function definitions are hard to read and hard for students to find syntax errors around.
* See above rule on anonymous functions as well.

```js
// avoid
$.ajax({
    url: "http://pizza.com/api/toppings",
    type: "GET",

  	 // named in-line function (avoid)
    success: function populateToppingsList(data) {
    	// possibly many lines of code
    	// possibly many lines of code
    },

    // anonymous in-line function (avoid)
    error: function(xhr, status, errorThrown) {
      // possibly many lines of code
      // possibly many lines of code
    },
});


// recommended
function populateToppingsList(data) {
  // many lines of code
}

function handleToppingsError(xhr, status, errorThrown) {
  // many lines of code
}

$.ajax({
    url: "http://pizza.com/api/toppings",
    type: "GET",
    success: populateToppingsList,
    error: handleToppingsError,
});
```


## File Organization

### Front-end basics file organization

#### Separate assets into directories `scripts`, `styles`

<pre>
├── README.md
├── index.html
├── scripts
│   └── app.js
├── styles
│   └── styles.css
└── images
    └── spinner.gif
</pre>

#### Name the initial client-side JS file `app.js`

### Node file organization

Main example:
<pre>
.
├── server.js
├── config
│   └── routes.js
├── controllers
│   └── quotesController.js
├── models
│   ├── index.js
│   └── quote.js
├── package.json
├── public
│   └── styles
│       └── styles.css
│   └── scripts
│       └── app.js
└── views
    ├── partials
    │   ├── footer.hbs
    │   ├── head.hbs
    │   └── header.hbs
    └── quotes
        ├── index.hbs
        └── new.hbs
</pre>

#### Start with all server-side code in `server.js`


##### Later separate code into `server.js`, `config/*`, `controllers/*`, `models/*`

* This follows GA-global curriculum except we use `server.js` instead of `app.js` (which we reserve for the client).

<pre>
.
├── server.js
├── config
│   └── routes.js
└── models
    ├── index.js
    └── quote.js
</pre>

##### Use index.js in each directory
* promote use of the module pattern


## ES6

### `var`, `let`, `const`


Prefer `const` for all variables that don't change and for variables that do not change which object they reference.

> Note: the following code is completely valid.
```js
const letters = [];
letters.push('A');
```

If `const` is not appropriate, prefer `let`. This is for values that change, including reference types that will refer to different objects.

> An example of `let` for a reference type:
```js
let current = head;
///
current = current.next;
```

Avoid `var` (unless for some reason you need hoisting?).

### Loops

Prefer `let` over `var` or `const` for `for` loops.

```js
// old es5 style
for (var item of arr){
	console.log(item.price);
}

// using const in this way is confusing - avoid
for (const item of arr){
	console.log(item.price);
}

// best
for (let item of arr){
	console.log(item.price);
}
```

### Arrow Functions

Prefer arrow functions over anonymous functions.

```js
// es5
function (a,b) { return a + b; }

// preferred
(a,b) => { return a + b; }
```

Always use braces around the function body.

Always use parentheses around the parameters, even if it's a single parameter.

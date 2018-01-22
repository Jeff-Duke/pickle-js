## Intro:
---

When I first started programming, I found code examples using variable and function names like `Foo, Bar, Baz, Bash` to be extremely confusing.

For example, from one of Kyle Simpson's talks about scope:
```javascript
var foo = “bar”;

function bar() {
  var foo = “baz”;
}

function baz(foo) {
  foo = “bam”;
  bam = “yay”;
}
```

For a lot of folks who might have been programming for a while, this is straight-forward and easy to understand. To a beginner, this can be extremely confusing. It's a pattern so often used that, when I was first learning JavaScript, I thought `foo`, `bar`, `bam`, `baz`, `bash` were somehow special keywoards like `return` and `function`.

When looking at code examples, the reader should be able to determine which parts of the example are essential, and which are non-essential like variable names, function names, arguments etc. In this post we'll use sandwiches, and the folks who make them, as examples for talking about JavaScript's `closure`. The ridiculously long variable and function names should make it easy to distinguish what parts of an example are part of the JavaScript language and syntax; and what parts are up to you as the developer to name.

## Before we get started
---

We'll assume that you've already been writing a bit of JavaScript, that you understand [functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions) and [function declarations.] Being familiar with (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)[scope](https://en.wikipedia.org/wiki/Scope_(computer_science)) and Lexical Scope is necessary in order to understand the power of closures, so if you're not familiar with these concepts, take a little time to read up on them and come back.


## What _is_ a closure?
---

This is a question that often gets asked in technical interviews. Closures are not unique to JavaScript, but they are one of the things that makes JavaScript so powerful. "Closure" is a computer science term that defines how a function can maintain a record of the environment in which it was called. This means that a function can keep track of the arguments and variables it was initially called with, even when it's called outside of that scope. Put a slightly different way, the [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) define closure as "the combination of a function and the lexical environment within which that function was declared."  If that doesn't quite make sense, don't worry, we'll have it all figured out by the end of this post!

### Where might you already be using Closures

All Javascript functions are closures. Functions have access to the variables defined within the function, along with variables defined in any enclosing scope(s), like the global scope. Other common places where closures occur are `setTimeout`, event/click handlers, and objects.

### Why would you want to take advantage of closures?

As we stated above, you will use closure when defining click handlers, writing callbacks, or when you're using `setTimeout` or `setInterval`.

A `setTimeout` function might look something like this:

```javascript
var deliEmployeeName = 'Alex';
var customerName = 'Bailey';

setTimeout(function greetTheCustomer() {
  console.log(
    'Hello ' + customerName + ', my name is ' + deliEmployeeName + '. What can I get you today?',
  );
}, 100);
// Logs "Hello Bailey, my name is Alex. What can I get you today?" to the console after 100ms
```

Where we have declared our `greetTheCustomer` function, we have created a new scope. Without closures, the function would not have access to `customerName` or `deliEmployeeName` variables and we would have to explicitly pass those to `greetTheCustomer`. In this example, we've declared our variables on the [global scope](https://developer.mozilla.org/en-US/docs/Glossary/Global_scope), but the concept is the same if this were all defined within a function, or as a method on an object.

Closures are very useful when defining private variables inside an object:

```javascript
function Sandwich(bread, topping) {
  this.bread = bread;
  this.topping = topping;

  var typicalNumberOfPickles = 2;
}

var hamOnWheat = new Sandwich('wheat', 'ham');

hamOnWheat.bread
//'wheat'

hamOnWheat.topping
//'ham'

hamOnWheat.typicalNumberOfPickles
//undefined
```

In this example, we've made the bread and the topping publicly available by defining them as properties on the Sandwich object. The `typicalNumberOfPickles` variable is not available outside of the Sandwich object, in order to get access to it outside the scope of the `Sandwich` object, you can write a method using that variable, and that method will act as a closure.

```javascript
function Sandwich(bread, topping) {
  this.bread = bread;
  this.topping = topping;

  var typicalNumberOfPickles = 2;

  this.howManyPicklesAreOnMySandwich = function() {
    console.log("there are " + typicalNumberOfPickles + " pickles on your " + this.topping + " sandwich" );
  }
}

var hamOnWheat = new Sandwich('wheat', 'ham');

hamOnWheat.howManyPicklesAreOnMySandwich()
//"there are 2 pickles on your ham sandwich"
```
Here, we've exposed a public method `howManyPicklesAreOnMySandwich` to give the customer a crucial bit of information. Even though we do not have direct access to the `typicalNumberOfPickles` variable in the global scope, thanks to the closure, we can get that information through our new method.

A pattern often used in place of an object with a single method, is to take advantage of closure by creating a function that returns an inner function. That inner function will have access to any variables defined in the outer function, without ever exposing those variables to a larger scope.

```javascript
function askForMorePickles(topping) {
  var typicalNumberOfPickles = 2;

  return function addAPickleToThisSandwich() {
    var previousPickles = typicalNumberOfPickles;
    typicalNumberOfPickles += 1;

    console.log("There were " + previousPickles + " pickles on your " + topping + " sandwich. Now there are " + typicalNumberOfPickles + " pickles. That is much better!");
  }
}

var morePicklesPlease = askForMorePickles('tempeh');

morePicklesPlease()
// "There were 2 pickles on your tempeh sandwich. Now there are 3 pickles. That is much better!"

morePicklesPlease()
// "There were 3 pickles on your tempeh sandwich. Now there are 4 pickles. That is much better!"
```
Here our `morePicklesPlease` function maintains access to the original `typicalNumberOfPickles` variable and will continue to update it. It's important to note that each time you store the returned function for later use, it forms its own closure. The original variable is not shared between each function, each function has its own copy of that original environment.

In the example below, we'll make 2 different sandwiches from the same `askForMorePickles` function, each sandwich will have its own number of pickles.

```javascript
function askForMorePickles(topping) {
  var typicalNumberOfPickles = 2;

  return function addAPickleToThisSandwich() {
    var previousPickles = typicalNumberOfPickles;
    typicalNumberOfPickles += 1;

    console.log("There were " + previousPickles + " pickles on your " + topping + " sandwich. Now there are " + typicalNumberOfPickles + " pickles. That is much better!");
  }
}

var morePicklesForTempeh = askForMorePickles('tempeh');
var morePicklesForTurkey = askForMorePickles('turkey');

morePicklesForTempeh()
// "There were 2 pickles on your tempeh sandwich. Now there are 3 pickles. That is much better!"

morePicklesForTurkey()
// "There were 2 pickles on your turkey sandwich. Now there are 3 pickles. That is much better!"
```
This allows the consumer to tailor their sandwich experience to the perfect number of pickles!

## Drawbacks to using closures
---

Because each version of a closure keeps a copy of that original environment, that environment isn't removed from the [callstack](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack) until all of the closures using it have been [garbage collected](https://developer.mozilla.org/en-US/docs/Glossary/Garbage_collection). So, for example, if you had 100 different functions referencing that original variable, and 99 of them get garbage collected, that variable still exists until the 100th function gets collected. This uses up local memory and eventually can lead to [memory leaks](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management).

## Wrapping up, or, how you might talk about closure in an interview
---

Closures are a fundamental part of many programming languages, not just JavaScript. A closure refers to a function maintaining access to the free variables defined where the function was called. Some use cases for closures are event handlers, timeouts, intervals, callbacks and keeping variables private within functions. One drawback to using closures is that they can lead to overconsumption of memory, and possibly memory leaks if not handled properly.

Hopefully this has helped de-mystify closures for you a bit. I would highly recommend the "You Don't Know Javascript: Scope & Closure" book if you'd like to learn more.

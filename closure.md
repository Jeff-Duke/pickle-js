### Intro:

Welcome to what will (hopefully) become a series of blog posts on some of the fundamental concepts of JavaScript.  When I was first learning, I found code examples using things like `Foo, Bar, Baz, Bash` to be extremely confusing.  My goal is to try to express these concepts in a relateable manner.  I want the reader to be able to visualize what's happening, and be able to determine which parts of the example are essential, and which are non-essential like variable names, function names, arguments etc.  The thought of Pickle.js was inspired by my experience at the Turing School of Software & Design.  The staff were very conscious of using relatable examples to help us learn new technical concepts.  We used things like monsters or mythical creatures to talk about Objects.  Pizzas to talk about arrays or data structures.  And, one of my favorites, sandwiches.  Today we'll use sandwiches, and the folks who make them, as a method for talking about `closure` in JavaScript.

TODO: Add a code snippet with some confusing `foo bar baz bash`

### Before we get started

We'll assume that you've already been writing a bit of JavaScript, that you understand functions and function declaration, and that you understand variable scope, specifically the concept of Lexical Scope.  If you're not familiar with these concepts, take a little time to read up on them and come back.

TODO: Add links to relevant articles/wikipedia pages.

### What is closure

This is a question that often gets asked in technical interviews.  Closure is not unique to JavaScript, but it is one of the things that makes JavaScript so powerful.  Closure is a computer science term that defines how a function can maintain a record of the environment in which it was called.  This means that a function can maintain access to the variables it has access to, even when it's called otuside of that scope.  Put a slightly different way, the [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) define closure as "the combination of a function and the lexical environment within which that function was declared."  If that doesn't quite make sense, don't worry!

### Where might you already be using Closure

All Javascript functions, inherently are closures.  Functions have access to the variables defined within the function, along with variables defined in any enclosing scopes, like the global scope.  Other common places where closures occur are `setTimeout`, event/click handlers, and objects.

TODO: Add a setTimeout Example
TODO: Add a basic global scope example

### Why would you want to take advantage of closure

As we stated above, you will use closure when defining click handlers, writing callbacks, or using `setTimeout` or `setInterval`.

Closures are very useful when defining private variables inside an object:

TODO: Private variable in object example

A pattern often used in place of an object with a single method, is to take advantage of closure by creating a function that returns an inner function.  That inner function will have access to any variables defined in the outer function, without ever exposing those variables to a larger scope.

TODO: Function returning a function example



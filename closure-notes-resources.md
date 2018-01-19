<!-- Notes from FEM course -->

Lexical Scope, Nesting and Hoisting

* Hoisting is made up
  * metaphor for the compiler
*

_Closure is one of the most important concepts in computing science_

The Module Pattern is one of the most important

* Scope: Where to look for things
  * What are we looking for?
    * any variable
* Scope is the set of rules by which we _sort the marbles_
* Javascript is a compiled language
* Who's doing the looking??
* Scope is a compile time process
* We often hear about closure and what we're really talking about is a function closure
* Javscript organizes its scopes by functions (mostly)
*

Closure comes to us from Lambda Calculus. Ok.

How do we observe what closure is?

Closure is when a functino "remembers" its lexical scope, even when the function is executed outside that lexical scope.

So what does that mean?

Talk about a set timeout
Click handlers as well
Preserves access as long as that function exists

Be careful:

Closure can nest. Closure preserves access to those original variables for as long as those functions exist. So, those variables will be garbage collected until all functions that reference them go away.

You can accidentally prevent things from being garbage collected. Make your functions at the outermost level.

https://www.youtube.com/watch?v=CQqwU2Ixu-U&app=desktop
https://www.youtube.com/watch?v=rBBwrBRoOOY

Every time we call the outer function, a new inner function is created, so a new sandwich with its own amount of pickles.
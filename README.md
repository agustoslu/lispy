# Lispy
It is meant as a little weekend project and it's just me working through the chapters of the book "Build Your Own Lisp" by Daniel Holden to get better at C and to understand the workings that are under the hood. Below you will find a little guidance, so that you can start getting familiar with the strange looking syntax of the language. On the other hand it should be noted that it barely scratches the surface. To learn more about this reimplementation and the language Lisp itself, you can check out the following links: 

* Build Your Own Lisp [Book](https://www.amazon.com/Build-Your-Lisp-Daniel-Holden/dp/1501006622/) and [Website](http://www.buildyourownlisp.com/)
* Wikipedia page of [Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language)) 
* ["What Made Lisp Different"](http://www.paulgraham.com/diff.html) by Paul Graham and his [books](http://www.paulgraham.com/books.html)
* ["Is Lisp Still Unique? Or at Least Different?"](http://norvig.com/Lisp-retro.html) by Peter Norvig


## Installation
The software is supported by all platforms. Besides the [MPC library](https://github.com/orangeduck/mpc) which is written by the book's author Daniel Holden, the only other dependency is the library called "editline." On Mac the library comes with Command Line Tools. On Linux you can install editline with `sudo apt-get install libedit-dev`. A C compiler is also needed.


## Basics
Lispy is a interpreted, [homoiconic](https://en.wikipedia.org/wiki/Homoiconicity), functional programming language which is written in C and based on Lisp. <br>

It consists of 7 lvals - is short for "lisp values". <br>


#### Numbers

Currently it only supports integers. Decimals will be added.

```
lispy>  / 42 6
7

lispy> / 42 7.0
<stdin>:1:7: error: expected one of '0123456789', '-', one or more of one of '0123456789', one or more of one of 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_+-*/\=<>!&', '"', ';', '(', '{', newline or end of input at '.'
```

#### Strings

Any combination of zero or more characters contained inside apostrophes.

#### Errors

Errors are automatically generated when the user attempts to evaluate invalid code, but can also be generated using the "error" function followed by a string.

```
lispy> + 4 "2"
Error: Function '+' passed incorrect type for argument 1. Got String, Expected Number.

lispy> error "custom"
Error: custom
```

#### Functions

[Polish Notation](https://en.wikipedia.org/wiki/Polish_notation) or Prefix Syntax makes life easier and allows our functions and operators to take any number of arguments.

```
lispy> + 1 2 3 4 5 6 7 8 9
45
```
We listed functions as one of our lispy values and what that means is that they are just like any other data type such as integers, strings etc. can be stored in variables and can be passed as arguments.  And here is where our Lisp's one of the most powerful features comes into play: expressions as lists. 

```
lispy> def {fun} (\ {args body} {def (head args) (\ (tail args) body)})
()
lispy> fun {pack f & xs} {f xs}
()
;;; combining our new custom function with a built-in function
lispy> pack head 4 2 1
{4}
```

You can use the built-in ones, on the other hand it’s also possible to create custom ones. All you need to do is making use of the lambda function and prefixing  “ \ “  while defining your function. 

```
lispy> (\ {x y} {* 2 (+ x y)}) 4 2
12
lispy> def {add-mult} (\ {x y} {* 2 (+ x y)})
()
lispy> add-mult 4 2
12
```

Some clever ways to define your functions and even to define functions that can define functions you can refer [here](https://buildyourownlisp.com/chapter12_functions#interesting_functions).

#### S-Expressions
By placing the code inside parentheses, we let the interpreter know that the code given has to be evaluated. In the interface every piece of code is an S-Expression by default. It is not the case for external files and due to that we need to make use of parentheses. Make sure you properly place these, since nesting is important, when it comes to evaluating your code and the interpreter will start to evaluate from the most internal one.

#### Q-Expressions
We make use of Q-Expressions to bypass the default evaluation rules and not evaluate the expression (S-Expressions or Symbols). By wrapping an expression inside curly braces, we pass the input along to the function exactly as typed. It is similar to the quote operator of Common Lisp.

#### Symbols
To connect everything together, it makes sense to introduce another term which we call symbols. The parser recognizes any combination of numbers and characters as symbols and before an lvalue is assigned, each symbol is in an "unbound" state,  after an lvalue is assigned it acts as a variable and will be ready for recall later on. 
We use Q-Expressions to avoid treating our list expressions as code. The same can be said also for symbols. We place them inside Q-Expressions, so that they won’t be treated as variables by our interpreter. 

#### Environment
Creating a global environment and loading every built-in function and its symbols in it is the first thing that our interpreter does when executed. As mentioned earlier you can associate any lvalue to a symbol and save them for future use like a variable, but as a little reminder it should also be mentioned that the environment is deleted every time you interrupt the execution of the interpreter.

#### Evaluating External Files
With our built-in function “load”, it’s possible to evaluate external files.

```
lispy>  load "hello_world.lspy"
"Hello World!"
;;; loading standard library
lispy> load "std_lib.lspy"
()
```
## Credits
This project owes its existence to Daniel Holden's brilliant work on the book ["Build Your Own Lisp"](http://www.buildyourownlisp.com/) and the [MPC library](https://github.com/orangeduck/mpc), which is used for parsing. I am truly thankful for all his efforts that made the project possible.

## TODO:
-[ ]Expand standard library
-[ ]Load the library automatically


---
layout: post
title:  "Clojure Macros Tutorial - part 1: functions vs. macros"
description:  "clojure macros clojurescript tutorial klipse"
date:   2016-05-01 18:32:53 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "DD5F1774-FD45-4678-8322-54F96EA49A62"
author: "@viebel"

---

`Clojure` as any other `LISP` language is [homoiconic](https://en.wikipedia.org/wiki/Homoiconicity){:target="_blank"}: the code written in the language is encoded as data structures that the language has tools to manipulate.

The most powerful tool to manipulate code is the macro. 


We all know how to call macros in our `clojure` code. But have you ever asked yourself:

>What is exactly the difference between a macro and a function?

or: 


>What would it take to replace macro calls with function calls in a `clojure` program?

In this article, we will try to give an answer by stating two assertions and demonstrating them with live code in [KLIPSE][app-url].

![Mountain](/assets/mountain.jpg)

### Macros vs. functions

One common explanation of the difference between functions and macros is: 

- a function transforms values into other values.
- a macro transforms code into other code.

We want to give a more precise answer, analysing two aspects of functions and macros:

1. when the arguments are evaluated?
2. is the return value evaluated?


Regarding to those two aspects, here are the differences between macros and functions:

<br/>


|   | arguments evaluation  | return value evaluated?  |
|---|:---:|:---:|
|functions   |  before function code execution | not evaluated |  
| macros  |  only when macro code evaluates them explicitly | evaluated  |   
|---|-----|-----|

<br/>

### The proof for functions

About functions, we stated that:

1. function arguments (the input) are evaluated before the function code execution 
2. function return value (the output) is not evaluated

Let's check it by writing a function with different side effects in the input and the output. We'll see what side effect is executed.

We will modify the background color of `KLIPSE` as side effects (with a simple function `set-body-bg-color`): red for the input and green for the output.

As you can see below, the background is red: it means the input is evaluated and the output is not evaluated; the output is returned, but it's not evaluated.

Q.E.D.∎

<iframe frameborder="0" width="100%" height="250px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.tuto)%0A%0A(defn%20set-body-bg-color%20%5Bc%5D%0A%20%20(set!%20(..%20js%2Fdocument%20-body%20-style%20-backgroundColor)%20c))%0A%0A(defn%20color-me%20%5Bx%5D%0A%20%20%20'(set-body-bg-color%20%22green%22))%0A%0A(color-me%20(set-body-bg-color%20%22red%22))%0A%0A&eval_only=1">
</iframe>


### The proof for macros

About macros, we stated that:

1. macros arguments (the input) are not evaluated before the macro code evaluates them explicitly
2. macros return value (the output) is evaluated

Let's check it by writing a macros with different side effects in the input and the output. The macro will not evaluate the input and we'll see what side effect is executed.

As before, we will modify the background color of `KLIPSE` as side effects: red for the input and green for the output.

We are adding a tweak to the `color-me` function such that it cannot be run more than once.

As you can see below, the background is green: it means that the input is not evaluated and the output is evaluated; the output is returned, and evaluated.
If the input had been evaluated, the `locked?` variable had been set to true and the color wouldn't be green, but red.

Q.E.D.∎

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.tuto%24macros)%0A%0A(defn%20set-body-bg-color%20%5Bc%5D%0A%20%20(set!%20(..%20js%2Fdocument%20-body%20-style%20-backgroundColor)%20c))%0A%0A(def%20locked%3F%20false)%0A(defmacro%20color-me-once%20%5Bx%5D%0A%20%20(when-not%20locked%3F%0A%20%20%20%20(set!%20locked%3F%20true)%0A%20%20%20%20'(set-body-bg-color%20%22green%22)))%0A%0A(my.tuto%2Fcolor-me-once%20(set-body-bg-color%20%22red%22))%0A%0A&eval_only=1">
</iframe>



If you wonder why we have to append `$macros` to the namespace and to reference the fully-qualified macro with self-hosted `clojurescript`, read [Messing with Macros at the REPL]({% post_url 2016-03-17-messing-with-macros %}){:target="_blank"}.

### Two assertions about functions and macros

Now we will try to see how functions and macros are interchangeable.

Let's say we have a function `foo-f` and a macro `foo-m` with exactly the same code. Then, the following two assertions hold:

#### Assertion #1: the macro expansion is equivalent to the function execution with arguments quoted

~~~clojure 
(= (macroexpand-1 '(foo-m arg1 arg2 ...))
   (foo-f 'arg1 'arg2 '...))
~~~


#### Assertion #2: the macro execution is equivalent to the evaluation of the function execution with arguments quoted


~~~clojure
(= (foo-m arg1 arg2 ...)
   (eval (foo-f 'arg1 'arg2 '...)))
~~~

### First assertion - macro expansion

The first assertion states that the macro expansion is equivalent to the function execution with arguments quoted.

~~~clojure 
(= (macroexpand-1 '(foo-m arg1 arg2 ...))
   (foo-f 'arg1 'arg2 '...))
~~~


Let's check it with [KLIPSE][app-url], using the simple `when` macro as defined in [core.clj](https://github.com/clojure/clojure/blob/clojure-1.7.0/src/clj/clojure/core.clj#L477):


<iframe frameborder="0" width="100%" height="400px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.tuto%24macros)%0A%0A(defmacro%20disp%20%5B%26%20forms%5D%20(cons%20%60str%20(for%20%5Bform%20forms%5D%20%60(str%20(pr-str%20'~form)%20%22%20%3D%3E%20%22%20(pr-str%20~form)%20%22%5Cn%22))))%0A%0A(defmacro%20when%0A%20%20%5Btest%20%26%20body%5D%0A%20%20(list%20'if%20test%20(cons%20'do%20body)))%0A%0A(defn%20when-function%0A%20%20%5Btest%20%26%20body%5D%0A%20%20(list%20'if%20test%20(cons%20'do%20body)))%0A%0A(my.tuto%2Fdisp%0A%20%20(macroexpand-1%20'(my.tuto%2Fwhen%20true%20%5B1%202%203%5D%20%5B4%205%206%5D))%0A%20%20(when-function%20'true%20'%5B1%202%203%5D%20%5B4%205%206%5D))%0A&eval_only=1">
</iframe>



### Second assertion - function evaluation

The second assertion states that the macro execution is equivalent to the evaluation of the function execution with arguments quoted.

~~~clojure
(= (foo-m arg1 arg2 ...)
   (eval (foo-f 'arg1 'arg2 '...)))
~~~

In order to check the second assertion, we have to define `eval` in `clojurescript`: this is easily done with self-host function `eval-str`:

(You can read more about self-host `clojurescript` in [Self-Hosted Clojurescript in action - Part 1]({% post_url 2016-04-05-self-host-part-1 %}){:target="_blank"}.)

<iframe frameborder="0" width="100%" height="550px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.tuto%24macros%0A%20%20(%3Arequire%20%5Bcljs.js%20%3Aas%20cljs%5D))%0A%0A(defmacro%20disp%20%5B%26%20forms%5D%20(cons%20%60str%20(for%20%5Bform%20forms%5D%20%60(str%20(pr-str%20'~form)%20%22%20%3D%3E%20%22%20(pr-str%20~form)%20%22%5Cn%22))))%0A%0A(defn%20eval%20%5Bexp%5D%0A%20%20(-%3E%20(cljs%2Feval-str%20(cljs%2Fempty-state)%20(str%20exp)%20%22bla%22%20%7B%3Aeval%20cljs%2Fjs-eval%20%3Acontext%20%3Aexpr%7D%20identity)%0A%20%20%20%20%20%20%3Avalue))%0A%0A(defmacro%20when%0A%20%20%5Btest%20%26%20body%5D%0A%20%20(list%20'if%20test%20(cons%20'do%20body)))%0A%0A(defn%20when-function%0A%20%20%5Btest%20%26%20body%5D%0A%20%20(list%20'if%20test%20(cons%20'do%20body)))%0A%0A(my.tuto%2Fdisp%0A%20%20(my.tuto%2Fwhen%20true%20%5B1%202%203%5D)%0A%20%20(my.tuto%2Fwhen%20false%20%5B1%202%203%5D)%0A%20%20(eval%20(when-function%20'true%20'%5B1%202%203%5D))%0A%20%20(eval%20(when-function%20'false%20'%5B1%202%203%5D)))%0A&eval_only=1">
</iframe>

After having illustrated the differences between macros and functions, we will show step-by-step how to write the `disp` macro that we use so much in our blog (including in this article). Hopefully, it will illustrate and clarify all the basic building blocks of macros in `clojure`, namely:

- regular quote
- syntax quote
- fully-qualified symbols
- unquote
- unique symbol generation
- quote splicing

[Follow us on twitter](https://twitter.com/intent/follow?original_referer=http%3A%2F%2Fblog.klipse.tech%2Fclojure%2F2016%2F05%2F01%2Fmacro-tutorial-1.html&ref_src=twsrc%5Etfw&region=follow_link&screen_name=viebel&tw_p=followbutton){:target="_blank"} to get notified, when the next article in the macro tutorial series is published.


Clojure rocks!

[app-url]: http://app.klipse.tech


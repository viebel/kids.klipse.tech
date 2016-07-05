---
layout: post
title:  "Clojure Macros Tutorial - part 2: how not to write macros"
description:  "clojure macros clojurescript tutorial klipse naive dummies macros for dummies"
date:   2016-05-05 01:14:22 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "DC4BEB2C-768D-4AF2-BE3B-70A5A1267DFA"
author: "@viebel"

---

After having understood the [fundamental difference between functions and macros]({% post_url 2016-05-01-macro-tutorial-1 %}){:target="_blank"}, we are now ready to write our first macro.

In this article, we want to write macros without the help of idiomatic tools that `clojure` provides for writing macros (syntax quote, unquote, `gensym` etc...). The purpose of this article it to let you understand why those powerful tools are mandatory, by experiencing how it feels to write a macro without those tools.

We will train ourselves with a simple macro that we use a lot for our blog posts: the `disp` macro.

Kind of [eating our own dog food](https://en.wikipedia.org/wiki/Eating_your_own_dog_food){:target="_blank"}...

![Dog](/assets/dog.jpg)

### Definition of the `disp` macro

The `disp` macro receives expressions and returns a string with the expressions and their respective evaluations.

For instance:

~~~clojure
(disp (map inc [1 2 3]) (str "hello" " " "world") (true? 1)) 
; (map inc [1 2 3]) => (2 3 4)
; (str "hello" " " "world") => "hello world"
; (true? 1) => false
~~~

In this article, for the sake of simplicity, we will limit ourselves to the case of a single expression.

Let's start coding!

There are three parts in the `disp` macro:

1. expression quoting (keeping is at it is)
2. expression evaluation (evaluating it)
3. concatenation of the quoting and the evaluation

### How to quote inside a macro
First, we need to understand how to quote an expression inside a macro.
Let's try to write a macro that receives an expression and return it, quoted.

<iframe frameborder="0" width="100%" height="250px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.repl%24macros)%0A%0A(defmacro%20my-quote%20%5Bform%5D%0A%20%20(quote%20form))%0A%0A%5B%0A%20%20(macroexpand-1%20'(my.repl%2Fmy-quote%20(map%20inc%20%5B1%202%203%5D)))%0A%20%20(my.repl%2Fmy-quote%20(map%20inc%20%5B1%202%203%5D))%0A%5D%0A&eval_only=1">
</iframe>

It doesn't work. The reason is that - as you can see with `macroexpand-1` - `(quote form)` is `form`.

(If you wonder why we have to append `$macros` to the namespace and to reference the fully-qualified macro with self-hosted `clojurescript`, read [Messing with Macros at the REPL]({% post_url 2016-03-17-messing-with-macros %}){:target="_blank"}.)

Let's try to state explicitly what is the requirement for the `my-quote` macro: We need to return an expression that when it is evaluated, it becomes `(map inc [1 2 3])`. The expression that fits this definition is `(quote (map inc [1 2 3]))`. In other words, it is a list whose first element is the symbol `quote` and its second element is `(map inc [1 2 3])`.

Now, try to update the klipse above with your correct implementation for the `my-quote` macro.

(Even on your mobile device, it works: just wait 3 seconds and your code is automatically evaluated.)

If you cannot make it after a couple of trials, you can read my solution below...
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
Give it another try, before surrending...
<br/>
<br/>
:)
<br/>
<br/>
;)
<br/>
<br/>
<br/>
<br/>
<br/>

Here is my solution:

~~~clojure
(defmacro my-quote [form]
  (list 'quote form))
~~~

Please copy/paste this code snippet in the klipse above and check by yourself.


#### How to concatenate evaluations inside a macro

Now, we have to figure out how to concatenate evaluations inside a macro. I hope that the following example will clarify what I mean by that.

~~~clojure
(concat-evaluations "hello" (+ 1 2))
;"hello 3"
~~~

Let's give it a try:

<iframe frameborder="0" width="100%" height="250px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.repl%24macros)%0A%0A(defmacro%20concat-evaluations%20%5Ba%20b%5D%0A%20%20(str%20a%20%22%20%22%20b))%0A%0A%5B%0A%20%20(my.repl%2Fconcat-evaluations%20%22hello%22%20%22world%22)%0A%20%20(my.repl%2Fconcat-evaluations%20%22hello%22%20(%2B%201%202))%0A%5D%0A%0A&eval_only=1">
</iframe>

It fails because inside the macro, the value of `b` is `(+ 1 2)`, so the `str` function appends `(+ 1 2)` to "hello".
Like for `my-quote`, the idea is to return an expression that when evaluated it becomes `(str "hello" " " 3)`.

Try to find a solution of your own. 

My solution is:

<iframe frameborder="0" width="100%" height="250px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.repl%24macros)%0A%0A(defmacro%20concat-evaluations%20%5Ba%20b%5D%0A%20%20(list%20'str%20a%20%22%20%22%20b))%0A%0A%5B%0A%20%20(macroexpand-1%20'(my.repl%2Fconcat-evaluations%20%22hello%22%20(%2B%201%202)))%0A%20%20(my.repl%2Fconcat-evaluations%20%22hello%22%20(%2B%201%202))%0A%5D%0A%0A&eval_only=1">
</iframe>


### Implementation of the `disp` macro
Now we are ready to implement the `disp` macro, by assembling our building blocks:

<iframe frameborder="0" width="100%" height="250px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.repl%24macros)%0A%0A(defmacro%20disp%20%5Bform%5D%0A%20%20(list%20'str%20(list%20'quote%20form)%20%22%20%3D%3E%20%22%20form))%0A%0A%5B%0A%20%20(macroexpand-1%20'(my.repl%2Fdisp%20(map%20inc%20%5B1%202%203%5D)))%0A%20%20(my.repl%2Fdisp%20(map%20inc%20%5B1%202%203%5D))%0A%5D%0A&eval_only=1">
</iframe>


Really simple right?

### Issues
There are a couple of issues with our naive implementation:

A. It's really complicated to write macros this way: almost everything has to be embedded into a expression that begins with `list`.

B. The code generated by the macro doesn't look like the code of the macro: in the generated code, `list` doesn't occur at all!

C. What happens if you  define local variables inside a macro? They might conflict with the variables defined in the scope of the macro caller. Here is an illustration of this issue:

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.repl%24macros)%0A%0A(defmacro%20concat-evaluations%20%5Ba%20b%5D%0A%20%20(list%20'let%20%5B'sep%20%22%3A%20%22%5D%0A%20%20%20%20%20%20%20%20(list%20'str%20a%20'sep%20b)))%0A%0A(def%20sep%20100)%0A%5B%0A%20%20(my.repl%2Fconcat-evaluations%20%22hello%22%20(map%20inc%20%5B100%202%203%5D))%3B%20so%20far%20so%20good%0A%20%20(macroexpand-1%20'(my.repl%2Fconcat-evaluations%20%22hello%22%20sep))%0A%20%20(my.repl%2Fconcat-evaluations%20%22hello%22%20(map%20inc%20%5Bsep%202%203%5D))%3B%20oops%20-%3E%20the%20'sep%20defined%20inside%20the%20macro%20has%20been%20incremented%0A%5D&eval_only=1">
</iframe>


D. Your macro will behave completely crazy when it is called inside a namespace where one of the functions that you use in the code of the macro is overriden.

Here is an example:

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.repl%24macros)%0A%0A(defmacro%20disp%20%5Bform%5D%0A%20%20(list%20'str%20(list%20'quote%20form)%20%22%20%3D%3E%20%22%20form))%0A%0A(ns%20my.ns)%0A(defn%20str%20%5B%26%20args%5D%20%22hacked!%22)%0A%0A%5B%0A%20%20(macroexpand-1%20'(my.repl%2Fdisp%20(map%20inc%20%5B1%202%203%5D)))%0A%20%20(my.repl%2Fdisp%20(map%20inc%20%5B1%202%203%5D))%0A%5D%0A&eval_only=1">
</iframe>


### Next steps

The 4 issues mentioned above are solved elegantly in `clojure` with the idiomatic tools I mentioned in the introduction.
Here is a [presentation of syntax quote]({% post_url 2016-05-05-macro-tutorial-3 %}){:target="_blank"}.

After reading this article, you should be able to re-write the `disp` macro, idiomatically.

Please share your best implementations in the comments below.

Clojure rocks!

[app-url]: http://app.klipse.tech


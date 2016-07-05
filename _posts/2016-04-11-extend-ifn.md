---
layout: post
title:  "IFn's magic beyond defprotocol secret"
description:  "IFn's magic beyond defprotocol secret"
date:   2016-04-11 10:19:23 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "F0604C6E-9CB4-4964-8C31-048F19B90FBF"
author: "@viebel"

---

In [this magic post]({% post_url 2016-04-07-ifn %}){:target="_blank"}, we showed how to make a type in `clojurescript` behave like a function by extending the `IFn` protocol.

In [this secret post]({% post_url 2016-04-09-clojurescript-protocols-secret %}){:target="_blank"}, we unveiled `defprotocol` secret in `clojurescript` by deep diving into its transpiled `javascript` code. 

Here, we'd like to connect the two: the **Magic** and the **Secret**.

### The Astonishment

`defprotocol` defines a set of named methods and their signatures.
Let's look at the `IFn` [protocol definition](https://github.com/clojure/clojurescript/blob/master/src/main/cljs/cljs/core.cljs#L435){:target="_blank"} in `clojurescript` and ask ourselves the following question:

>What set of named methods and signatures are defined by IFn?

~~~clojure
(defprotocol IFn
  "Protocol for adding the ability to invoke an object as a function.
  For example, a vector can also be used to look up a value:
  ([1 2 3 4] 1) => 2"
  (-invoke
    [this]
    [this a]
    [this a b]
    [this a b c]
    [this a b c d]
    [this a b c d e]
    [this a b c d e f]
    [this a b c d e f g]
    [this a b c d e f g h]
    [this a b c d e f g h i]
    [this a b c d e f g h i j]
    [this a b c d e f g h i j k]
    [this a b c d e f g h i j k l]
    [this a b c d e f g h i j k l m]
    [this a b c d e f g h i j k l m n]
    [this a b c d e f g h i j k l m n o]
    [this a b c d e f g h i j k l m n o p]
    [this a b c d e f g h i j k l m n o p q]
    [this a b c d e f g h i j k l m n o p q r]
    [this a b c d e f g h i j k l m n o p q r s]
    [this a b c d e f g h i j k l m n o p q r s t]
    [this a b c d e f g h i j k l m n o p q r s t rest]))
~~~


The answer seems to be trivial: 22 flavours of `-invoke`.

It becomes less trivial when you pay attention that a type that implements `-invoke` is callable as a function with no mention of `-invoke`.

For instance:

~~~clojure
(:a {:a "A"})
~~~

The purpose of this article is to show you what happens behind the scenes with `IFn` and `defprotocol`.


![Secret](/assets/magic.png)

### The Magic

In `clojure[script]`, there are two kinds of function calls:

1. regular function call e.g. `(fn x y z)`
2. with `apply` e.g. `(apply fn [x y z])`

Let's see how the `clojurescript` compiler translates for the function `=` the two kinds of function calls:

<iframe frameborder="0" width="100%" height="200px"
    src= 
    "http://app.klipse.tech/?cljs_in=(%3D%201%202)%0A(apply%20%3D%20%5B1%202%203%5D)&js_only=1">
</iframe>

So nice! All the function calls in `clojurescript` are routed to either `call` or `apply`.

Therefore, in order to make a type behaves like a function in `clojurescript`, we simply need to add implementation for `call` and `apply` to the type `Prototype`.

Let's see how to implement `call` and `apply` for `RegExp` type in `javascript`:

<p data-height="407" data-theme-id="23367" data-slug-hash="XdZPZm" data-default-tab="js" data-user="raphaelboukara" class="codepen">See the Pen <a href="http://codepen.io/raphaelboukara/pen/XdZPZm/">XdZPZm</a> by Raphael Boukara (<a href="http://codepen.io/raphaelboukara">@raphaelboukara</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Now, let's observe the transpiled `javascript` code for `extend-type` and `IFn`:


<iframe frameborder="0" width="100%" height="470px"
    src= 
    "http://app.klipse.tech/?cljs_in=(ns%20my.regexp)%0A%0A(extend-type%20js%2FRegExp%0A%20%20IFn%0A%20%20(-invoke%20%0A%20%20%20%20(%5Bmatch%20s%5D%20(re-find%20match%20s))))%0A%0A(%23%22clojure%22%20%22clojurescript%22)%0A&js_only=1">
</iframe>

The compiler - not like for [regular protocols]({% post_url 2016-04-09-clojurescript-protocols-secret %}){:target="_blank"} - created code for `call` and `apply` into `RegExp` prototype.

This is the magic that allows types to behave like functions in `clojurescript`.

(The compiler also created code for `cljs$core$IFn$_invoke$arity$1` and I don't know why. If someone knows the reason, please add a comment below.)

### The Secret

Now that you have seen the Magic, you probably wonder how it works.

It's time to uncover the Secret of `IFn`:

> There is a special treatment for `IFn` in the [code](https://github.com/clojure/clojurescript/blob/master/src/main/clojure/cljs/core.cljc#L1422){:target="_blank"} of the `extend-type` macro:

Here is the [piece of code](https://github.com/clojure/clojurescript/blob/master/src/main/clojure/cljs/core.cljc#L1422){:target="_blank"}  used by `extend-type` to behaves differently when the implemented protocol is `IFn`:of the `extend-type` macro:

~~~clojure
(if (= psym 'cljs.core/IFn)
  (add-ifn-methods type type-sym sig)
  (add-proto-methods* pprefix type type-sym sig))
~~~

Feel free to deep dive into the code of `extend-type`, `proto-assign-impls` and `add-ifn-methods` and to share your insights in the comments below.

Clojurescript rocks!

[app-url]: http://app.klipse.tech?blog=klipse


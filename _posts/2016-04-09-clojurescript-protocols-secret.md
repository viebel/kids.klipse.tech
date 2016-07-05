---
layout: post
title:  "Clojurescript defprotocol's secret"
date:   2016-04-09 23:57:46 +0200
categories: clojurescript
thumbnail: assets/klipse.png
description: "In this post, we explain how clojurescript implement defprotocol in javascript."
author: "@RaphaelBoukara"
---

In [this precedent]({% post_url 2016-03-30-clojure-oop-part1 %}) post we have presented the skeleton of the `javascript` transpiled code of `defrecord` + `deftype` when they work together. 

<br>
In this post we will review it by exploring the entire `javascript` transpiled code of `defprotocol` in clojurescript. 
We will reveal the secret of protocols in clojurescript.

<br>
![Secret](/assets/secret.jpg)

## Protocols

As defined in the [documentation](http://clojure.org/reference/protocols), 

> A protocol is a named set of named methods and their signatures, defined using defprotocol.

~~~ clojure
(defprotocol IFoo
  (fooA [this])
  (fooB [this]))
~~~

After creating your protocol, you can use `deftype`, `defrecord` or `reify` for defining its methods implementation.
In our case, we will use `deftype` because the major difference between `deftype` and `defrecord` is that `deftype` provides just the functionalities implemented by the user, contrary to `defrecord` that implements a lot of things that will not help us to understand the generated `javascript` code. You can read more about `defrecord` [here]({% post_url 2016-04-24-records-wacky-maps %}){:target="_blank"}.

Let’s see the classical object oriented example of the animals’ cry:

<iframe frameborder="0" width="100%" height="350px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Animal%0A%20%20(cry%20%5Bthis%5D))%0A%0A(deftype%20Bird%20%5B%5D%0A%20%20Animal%0A%20%20(cry%20%5Bthis%5D%20%22Tweet%20tweet!%22))%0A%0A(deftype%20Dog%20%5B%5D%0A%20%20Animal%0A%20%20(cry%20%5Bthis%5D%20%22Woof%20woof!%22))%0A%0A(def%20Pluto%20(Dog.))%0A(def%20Tweety%20(Bird.))%0A%0A(map%20cry%20%5BPluto%20Tweety%5D)&eval_only=1">
</iframe>

Now, step by step, we will try to understand how the clojurescript compiler generates the protocol part in `javascript`. Let's go with our basic protocol. Use [KLIPSE][app-url]{:target="_blank"} to see the generated `javascript` code:

<iframe frameborder="0" width="100%" height="350px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Animal%0A%20%20(cry%20%5Bthis%5D))&js_only=true">
</iframe>

YES! this code seems very hard and incomprehensible! But don't worry, it is simpler than it looks!

__What do we got here?__ Two properties are added to the current user namespace: 

1. One named `Animal`, return an empty function! We will explain in another post its utility. 
2. A second property, named `cry`, return a function named like `namespace$user$cry` that receives an object. This part is the core of your protocol definition. 

## We will first focus on the first part of the transpiled code:

~~~ javascript
cljs.user.cry = (function cljs$user$cry(this$) {
    if ((!((this$ == null))) 
        && (!((this$.cljs$user$Animal$cry$arity$1 == null)))) {
        return this$.cljs$user$Animal$cry$arity$1(this$);
    } else {
        ...
    }
});
~~~

The `cljs$user$cry` function checks that the received object (`this$` will have to be a `deftype` or a `defrecord`) isn't `null` and if it implements the `cljs$user$Animal$cry$arity$1` function. If these conditions are respected, launch the `cljs$user$Animal$cry$arity$1` function.

__Whence comes this `cljs$user$Animal$cry$arity$1` function?__ 

Let's compile a `deftype` form to answer:

<iframe frameborder="0" width="100%" height="350px"
    src= 
    "http://app.klipse.tech/?cljs_in=(deftype%20Bird%20%5B%5D%0A%20%20Animal%0A%20%20(cry%20%5Bthis%5D%20%22Tweet%20tweet!%22))&js_only=true">
</iframe>

In the `Bird` prototype, the `cljs$user$Animal$cry$arity$1` match the implementation of the `cry` function from the `Animal` protocol. The `arity$1` part puts forward the number of arguments received by the `cry` function. Like this, your program knows which `cry` function to call. [This](http://app.klipse.tech/?cljs_in=(defprotocol%20Fly%0A%20%20(fly%20%5Bthis%5D)%0A%20%20(fly%20%5Bthis%20x%5D)%0A%20%20(fly%20%5Bthis%20x%20y%5D))&js_only=true) example speaks for itself.

## Now let's focus on the second part:

~~~ javascript
...
var x__25043__auto__ = (((this$ == null)) ? null : this$);
var m__25044__auto__ = (cljs.user.cry[goog.typeOf(x__25043__auto__)]);
if (!((m__25044__auto__ == null))) {
    return m__25044__auto__.call(null, this$);
} else {
    var m__25044__auto____$1 = (cljs.user.cry["_"]);
    if (!((m__25044__auto____$1 == null))) {
        return m__25044__auto____$1.call(null, this$);
    } else {
        throw cljs.core.missing_protocol.call(null, "Animal.cry", this$);
    }
}
...
~~~

In this step there is 2 possible cases, the first when `this$` is `null` and the second when you didn't implement the cry function when you defined your `deftype`.

### first case:

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Animal%0A%20%20(cry%20%5Bthis%5D))%0A%0A(cry%20%22hello!%22)&eval_only=1">
</iframe>

__What is going on here?__ `x__25043__auto__` is `null`, so `m__25044__auto__` is `null`, the code is looking for `cljs.user.cry["_"]` which is, of course, `null` so the browser throw an exception!

__But in which case is possible that `cljs.user.cry["_"]` won't be `null`?__ [tom@tomjack.co](https://clojurians.slack.com/team/tomjack) gave me the answer on the famous [slack clojurians](https://clojurians.slack.com/messages/clojurescript/)! 

> "... it is for (extend-type default ..." @tomjack

You will find the answer using the `extend-type` macro on type `default`! Let's see this macro in action:

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Animal%0A%20%20(cry%20%5Bthis%5D))%0A%0A(extend-type%20default%0A%20%20Animal%0A%20%20(cry%20%5Bthis%5D%20%22%3F%3F%3F%3F%3F%22))%0A%0A(cry%20%22hello!%22)&eval_only=1">
</iframe>

In the current namespace, extend all clojure data type for it implements a default `cry` method of the `Animal` protocol. Let's see the transpiled code of the `extend-type` macro:

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(extend-type%20default%0A%20%20Animal%0A%20%20(cry%20%5Bthis%5D%20%22%3F%3F%3F%3F%3F%22))&js_only=1">
</iframe>

WOW! It seems that we found our `cljs.user.cry["_"]`!! Let's ask the question again:

__What is going on here?__ `x__25043__auto__` is `null`, so `m__25044__auto__` is `null`, the code is looking for `cljs.user.cry["_"]` that means the code wants to know if, in the current namespace, a default implementation for `cry` function is available, if there is no, throws an error, else call this default `cry` function.


### second case:

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Animal%0A%20%20(cry%20%5Bthis%5D))%0A%0A(deftype%20Bird%20%5B%5D%0A%20%20Animal)%0A%0A(cry%20(Bird.))&eval_only=1">
</iframe>

__What is going on here?__ `x__25043__auto__` is an `object`, so `m__25044__auto__` is `null` and it is the same logic as the first case.

## what do we have left?

We didn't talk about this part:

~~~ javascript
...
var x__25043__auto__ = (((this$ == null)) ? null : this$);
var m__25044__auto__ = (cljs.user.cry[goog.typeOf(x__25043__auto__)]);
if (!((m__25044__auto__ == null))) {
    return m__25044__auto__.call(null, this$);
} else {
...
~~~

In which case `m__25044__auto__` is not `null`? The response is very simple. According to the code, `m__25044__auto__` is not `null` if, in the `cry` object, we can find a property which its name is the type of `this$`. Let's present an example to meet this case:

<iframe frameborder="0" width="100%" height="300px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Animal%0A%20%20(cry%20%5Bthis%5D))%0A%0A(extend-type%20string%0A%20%20Animal%0A%20%20(cry%20%5Bthis%5D%20%22A%20string%20can%27t%20be%20an%20animal!!!%22))%0A%0A(cry%20%22hello%22)&js_only=1">
</iframe>

Because you use `extend-type` not with `default` but with `string` type (or any another type), the compiler will add, to the protocol `Animal`, a property named `string` equals `true`. Additionally, the compiler will also add, to the `cry` object, a property named 'string' contains the implementation of the `cry` function from the `Animal` protocol for `string` objects as you can see in the transpiled `javascript` code:

~~~ javascript
...
(cljs.user.Animal["string"] = true);

(cljs.user.cry["string"] = (function (this$){
    return "A string can't be an animal!!!";
}));
cljs.user.cry.call(null,"hello");
~~~

So you obtain that `goog.typeOf(x__25043__auto__)` returns `"string"` which is a property of the `cry` object. 

Great! Now, You belong to those who know the secret! ;)

[app-url]: http://app.klipse.tech/



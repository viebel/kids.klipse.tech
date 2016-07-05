---
layout: post
title:  "Clojurescript: abstraction and data structure clarification - Part 1"
date:   2016-04-01 07:57:46 +0200
categories: clojurescript
thumbnail: assets/klipse.png
description: "In this post, we review methods: defprotocol, deftype, extend-type, defrecord, reify, extend-protocol and specify."
author: "@RaphaelBoukara"
---

## 1- Protocols

As define in the [documentation](http://clojure.org/reference/protocols), 

> A protocol is a named set of named methods and their signatures, defined using defprotocol.

~~~ clojure
(defprotocol IFoo
  "IFoo doc"
  (fooA [this] "fooA doc")
  (fooB [this] [this a] "fooB doc"))
~~~

You can add some documentation to the protocol and to each methods it contains. You don't need to provide methods' implemantation. After created your protocol, you can use `deftype`, `defrecord` or `reify` for defining its methods implementation.

In our case, we will use `deftype` because the major difference between `deftype` and `defrecord` is that `deftype` provides just the functionalities implemented by the user, contrary to `defmethod` that implements a lot of things that will not here to help us to understand the generated javascript code.

Let's see the below example:

<iframe frameborder="0" width="100%" height="350px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Fly%0A%20%20(fly%20%5Bthis%5D))%0A%0A(deftype%20Bird%20%5B%5D%0A%20%20Fly%0A%20%20(fly%20%5Bthis%5D%20%22Bird%20uses%20natural%20wings.%22))%0A%0A(deftype%20Plane%20%5B%5D%0A%20%20Fly%0A%20%20(fly%20%5Bthis%5D%20%22Plane%20uses%20metal%20wings.%22))%0A%0A(def%20a380%20(Plane.))%0A(def%20eagle%20(Bird.))%0A%0A(map%20fly%20%5Ba380%20eagle%5D)&eval_only=1">
</iframe>

Now try to understand how the clojurescript compiler generates a basic protocol in javascript. Use [KLIPSE][app-url]{:target="_blank"} to see the generated javascript code:

<iframe frameborder="0" width="100%" height="500px"
    src= 
    "http://app.klipse.tech/?cljs_in=(defprotocol%20Fly%0A%20%20(fly%20%5Bthis%5D))%0A%0A(deftype%20Bird%20%5B%5D%0A%20%20Fly%0A%20%20(fly%20%5Bthis%5D%20%22Bird%20uses%20natural%20wings.%22))%0A%0A(deftype%20Plane%20%5B%5D%0A%20%20Fly%0A%20%20(fly%20%5Bthis%5D%20%22Plane%20uses%20metal%20wings.%22))%0A%0A(def%20a380%20(Plane.))%0A(def%20eagle%20(Bird.))%0A%0A(map%20fly%20%5Ba380%20eagle%5D)&js_only=1">
</iframe>


OK! If you are like me, you give me that the generated code is difficult to understand! So, let's simplify it:

~~~ javascript
/**
 * Protocol Fly receive a javascript object
 * if the object implement fly => call fly
 * else throw exception
 */
fly = function(this$) {
    if(this$._Fly$fly != null) {
        return this$._Fly$fly();
    } else {
        throw "fly not supported!";
    }
};

/**
 * Type Bird that implement fly function
 */
Bird = function() {};
Bird.prototype._Fly$fly = function() {
    return "Bird uses natural wings.";
};

/**
 * Type Plane that implement fly function
 */
Plane = function() {};
Plane.prototype._Fly$fly = function() {
    return "Plane uses metal wings.";
};

/**
 * Type Empty that not implement fly function
 */
Empty = function() {};

/**
 * Call Fly.fly for each type you build
 */
a380 = new Plane();
eagle = new Bird();
none = new Empty();
console.log(fly.call(null, a380)); 
console.log(fly.call(null, eagle));
console.log(fly.call(null, none));
~~~

I hope now it's more clear, when you define a protocol, you define a set of functions. Each function receive a javascript object, checks if this object implements a specific function, if that is the case this function launch the specific function. That's all!

[app-url]: http://app.klipse.tech/

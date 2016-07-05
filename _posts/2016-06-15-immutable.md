---
layout: post
title:  "immutable.js live tutorial"
description:  "spec tutorial clojure klipse live coding examples clojurescript cljs"
date:   2016-03-30 18:17:52 +0200
categories: javascript
thumbnail: assets/klipse.png
guid: "4BEFF6B0-8E1C-482F-98B0-BCA39A0F84B1"
author: "@viebel"
minified_plugin: true

---

`Immutable.js` is a library that provides immutable collections for JavaScript, inspired by clojure[script] immutable data structures. It has been [developed by Facebook](https://facebook.github.io/immutable-js/).

> Immutable data cannot be changed once created, leading to much simpler application development, no defensive copying, and enabling advanced memoization and change detection techniques with simple logic. Persistent data presents a mutative API which does not update the data in-place, but instead always yields new updated data.


The purpose of this article is to demonstrate the power of interactive code snippets in the browser. For that purpose, we are going to play a bit with `immutable.js`, like kids in a playground sandbox.

![Sandbox](/assets/playground.jpg)

# Playing with Immutable.js 

First, let's load `immutable.js` in the browser with the help of the [klipse plugin]({% post_url 2016-06-06-klipse-plugin-tuto %}):

<pre>
<div class="language-klipse-eval-js" data-external-libs="immutable">
     Immutable
</div>
</pre>

Now, we can play with the `immutable.js` library.

>All the code snippets of this article are live and interactive. Simply modify the code, wait for 2 seconds and see the results...


Let’s start by creating an immutable map. A map is basically an object consisting of key-value pairs.
Let's create, a `person` with `name` and `phone` attributes:

~~~klipse-eval-js
person = Immutable.Map({ 
        name: 'John', 
        phone: '12345678'
});
~~~

Now, let's modify the `phone`:

~~~klipse-eval-js
changePhone = function( person, newPhone ) {
        return person.set( 'phone', newPhone );
};

person2 = changePhone( person, '87654321' );
~~~

The `changePhone` function returns a new immutable map: When `changePhone` is executed, `person2` is created as a return value.


The phone numbers of each person map can be accessed via the `get` method. The properties of the maps are hidden behind the `get` and `set` interface, therefore they cannot be directly accessed or modified.

~~~klipse-eval-js
[person.get('phone'), person2.get( 'phone' )]
~~~

# Equality vs. Identity

Now let's see what happens if we change `phone` of `person2` back to the `phone` of `person`:

~~~klipse-eval-js
person3 = changePhone( person2, '12345678' );

person3 == person
~~~

`person3` and `person` are not the same.

But from an value perspective, they are equal:

~~~klipse-eval-js
person3.equals( person )

~~~

The immutable abstraction is intelligent enough to detect when an attribute is changed to the same value as before. In the following case, the new person is identical to the old one: both `==` and `===` comparisons return true.

~~~klipse-eval-js
changePhone( person, '12345678' ) == person
~~~

~~~klipse-eval-js
changePhone( person, '12345678' ) === person
~~~


Read [more about immutable.js](https://facebook.github.io/immutable-js/)...

Tell us what you think about the interactive code snippets of this article in the comments below.

---
[app-url]: http://app.klipse.tech?blog=klipse


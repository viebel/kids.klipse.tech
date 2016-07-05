---
layout: post
title:  "immutable.js is much faster than native javascript"
description:  "persistent data structures immutable immutable.js javascript live tutorial"
date:   2016-06-23 10:17:52 +0200
categories: javascript
thumbnail: assets/klipse.png
guid: "8F9DF3EE-C133-4A2D-A701-3F539018513E"
author: "@viebel"
minified_plugin: true

---

`Immutable.js` is a library that provides immutable collections for JavaScript, inspired by clojure[script] immutable data structures. It has been [developed by Facebook](https://facebook.github.io/immutable-js/).

As they explain on their website:

> Immutable data cannot be changed once created, leading to much simpler application development, no defensive copying, and enabling advanced memoization and change detection techniques with simple logic.

> Persistent data presents a mutative API which does not update the data in-place, but instead always yields new updated data.


In this article, we are going to go over a basic use case where `immutable.js` is much much faster than `javascript`: pushing an element to an array without modifying the original array.

In `javascript` the only way of doing that is by first copying the array then pushing the element to it. With `immutable.js`, `push` returns a new list with the element appended to it; and this is super fast.


![Fast](/assets/velo.jpg)

# Immutable lists vs. javascript arrays

First, let's load `immutable.js` in the browser: 

<pre>
<div class="language-klipse-eval-js" data-external-libs="immutable">
     Immutable
</div>
</pre>

Here is a `benchmark` function that calculates execution time of code:

~~~klipse-eval-js
function benchmark(iterations, f) {
var start = new Date();
for (var i = 0; i < iterations; i++) {
f();
}
var end = new Date();
return "Elapsed time: " + (end - start) + " msec";
}
benchmark(1000000, function() {});
~~~

By the way, you see that `javascript` function call mechanism is super fast.

All the code snippets of this page are **live** and **interactive** powered by the [klipse plugin](https://github.com/viebel/klipse):

1. **Live**: The code is executed in your browser
2. **Interactive**: You can modify the code and it is evaluated as you type

Now, let's compare the performance of:

1. creating a `javascript` array vs. creating an immutable list

2. copying and pushing an element to a `javascript` array vs. pushing an element to an `immutable.js` list



## Collection creation


Creating a `javascipt` array of size `100000` takes almost no time:

~~~klipse-eval-js
benchmark(1, function() {
var jsArr = [];
jsArr[100000] = 42;
});
~~~

While creating an `immutable.js` list of size `100000` takes a bit of time: 

~~~klipse-eval-js
benchmark(1, function() {
      immutableList = Immutable.Range(0,100000).toList();
})
~~~


## Javascript array slice is slow

Manipulation a `javascript` array with `slice` and `push` is quit expensive:

~~~klipse-eval-js
var jsArr = [];
jsArr[100000] = 42;
benchmark(100,  function() {
    jsArr.slice(0).push(43);
})
~~~

A few remarks:

1. `slice(0)` is equivalent to copying the array

2. `push` modifies the array it is called on

~~~klipse-eval-js
myArr = [];
myArr.push(42);
myArr.push(43);
myArr
~~~

# Immutable push is super fast

Pushing an element to an `immutable` list is super fast:

~~~klipse-eval-js
immutableList = Immutable.Range(0, 100000).toList();

benchmark(100, function() {
          immutableList.push(43);
})
~~~

If you see that the elapsed time is `0 msec`, it's not a bug: it is indeed very fast. Increase by `10x` the number of iterations and see what happens.

> On my computer `push` of `immutable.js` is about `100x` faster than `push` of native `javascript`.

Note that when pushing an element to an `immutable.js` list, the list is not modified. A new list is returned with the element appended to it: this is how immutable collections work. 

~~~klipse-eval-js
myList = Immutable.Range(0, 10).toList(); 
myList.push(42);
myList.size
~~~

# Go deeper

Here are a couple of interesting articles that describe in details what are persistent data structures and why they are so fast:

- [Understanding Clojure's Persistent Vectors](http://hypirion.com/musings/understanding-persistent-vector-pt-1)
- [Persistent data structure - Wikipedia](https://en.wikipedia.org/wiki/Persistent_data_structure)
- [immutable.js documentation](https://facebook.github.io/immutable-js/)
 


---
[app-url]: http://app.klipse.tech?blog=klipse


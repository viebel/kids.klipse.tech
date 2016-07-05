---
layout: post
title:  "Demistify clojurescript namespaces"
date:   2016-03-27 17:32:46 +0200
categories: clojure
thumbnail: assets/klipse.png
description: "Demystify clojurescript namespaces"
author: "@RaphaelBoukara"
---

If you are like us, you use clojurescript namespaces for a while, but without really understanding how it works under the hoods.

Let's clarify it by looking at the transpiled javascript code, with [KLIPSE][app-url]{:target="_blank"}:

~~~klipse-js
(ns my-project.my-ns)

(def x 1)
(def y 2)
~~~

As you know, javascript doesn't support namespaces; clojurescript implements namespaces using the [google closure library](https://developers.google.com/closure/library/docs/gettingstarted#hello-closure) where namespaces are js objects.

The clojurescript compiler translates the variables of the clojurescript namespace in keys of the namespace object.

If you look at the transpiled code above, you see that `goog.provide` initializes the `my_project.my_ns` object and the `x` and `y` variables are nested into it. 

Now, you probably wonder what's the meaning of the `goog.require('cljs.core')` piece of code; we will discuss `goog.require` in another blog post. 

You can learn more about google closure library [here](https://developers.google.com/closure/library/docs/gettingstarted#hello-closure).

[app-url]: http://app.klipse.tech

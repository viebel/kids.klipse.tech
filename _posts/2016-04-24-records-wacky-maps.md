---
layout: post
title:  "In Clojure, records are wacky maps"
description:  "records, defrecord, clojure, clojurescript,  Michał Marczyk"
date:   2016-04-25 17:17:14 +0200
categories: clojurescript
thumbnail: assets/klipse.png
guid: "4F96220D-2B51-41AE-A092-BD7192257900"
author: "@viebel"

---

In `clojure`, records, types and protocols are parts of the fundamental building blocks of the language.

We have unveiled `defprotocol`'s secret in [a previous post]({% post_url 2016-04-09-clojurescript-protocols-secret  %}){:target="_blank"}. Now it's time to explore `defrecord`.

This article has been - inspired by this great talk by Michał Marczyk: [defrecord/deftype in Clojure and ClojureScript](https://youtu.be/vZtkqDIicqI){:target="_blank"}.

Michał knows this topic very deeply as he contributed to the implementation of types, records and
protocols in `clojurescript` in 2012.

He's also the #4 contributor on `clojurescript`.

![Michal Marczyk](/assets/michalmarczyk.jpg)


We were very honoured when Michał encouraged us to write a blog post on the topic using [KLIPSE][app-url]{:target="_blank"}.

### Introduction

What is a record?

> In computer science, a record (also called struct or compound data) is a basic data structure. A record is a collection of fields, possibly of different data types, typically in fixed number and sequence. [Wikipedia](https://en.wikipedia.org/wiki/Record_(computer_science)){:target="_blank"} 

In `clojure`, `defrecord`, `deftype` and `defprotocol` were introduced in version 1.2

In `clojurescript` the persistent data structures (maps, vectors ...) are based on types, records and protocols.

### Records: creation and identity

There are 3 equivalent ways to create a record: a constructor and two factory functions: `(A.)`, `(->A)` and `(map->A)`.

~~~klipse

(defrecord A [x y])
(def a (A. 1 2))
(def aa (map->A {:x 1 :y 2}))
(def aaa (->A 1 2))
[a aa aaa]
~~~

You can retrieve the basis keys of the record with `getBasis` that returns a vector of basis keys as symbols.

~~~klipse
(.getBasis A)
~~~

Records of the same type and same values are equal

~~~klipse
(= a aa aaa)
~~~

And have the same hash code.

~~~klipse
(map hash [a aa aaa])
~~~

Records of different types are never equal (even if they have the same values).

~~~klipse
(defrecord B [x y])
(def b (B. 1 2))
(= a b)
~~~


In `clojurescript`, the `hash` function doesn't take the record type into account.

~~~klipse
(map hash [a b])
~~~

### Maps

At first glance, records behave like maps: keyword access, `get`, `count`, `keys` and iteration work as expected.

~~~klipse
[(:x a) (get a :z "n/a")]
~~~

~~~klipse
(keys a)
~~~

~~~klipse
  (count a)
~~~

~~~klipse
    (map (fn [[k v]] [k (inc v)]) a)
~~~

But records are not real maps. Michał called them **Wacky maps**.

### Wacky maps

Here are the differences between records and maps:

- Unlike a regular map, a record is not callable as a function.
- When you `assoc` a record you get another record.
- When you `dissoc` basis keys in a record you get a map instead of a record.
- In `clojure` boxing happens on records when you use type hints; but not in `clojurescript`. (Boxing can be avoided with type hints, if you use field access syntax and you're in a primitive-friendly context e.g. an arithmetic expression.)

`assoc` works as expected:

~~~klipse
(assoc a :z "zzz")
~~~

But `dissoc` is surprising:

~~~klipse
(dissoc a :x)
~~~


### Behind the scenes : defrecord's transpiled javascript code

If you are curious to see how the magic occurs in `clojurescript`, you will find it very interesting to observe and meditate around the transpiled `javascript` code of `defrecord`:



~~~klipse-js
(defrecord A [x y])
~~~

If you want to discover stuff you didn't know about `deftype`, go to [The power and danger of deftype]({% post_url 2016-04-26-deftype-explained %})...

Clojure & Clojurescript rock!

[app-url-static]: http://app.klipse.tech?blog=klipse&js_only=1
[app-url]: http://app.klipse.tech?blog=klipse&static-fns=true&js_only=1


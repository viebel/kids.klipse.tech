---
layout: post
title:  "Destructuring in Clojure - Part 1"
description:  "Destructuring in Clojure - Part 1"
date:   2016-03-30 19:21:46 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "E1D50DFC-9C61-4EC8-AA69-C6203C38CCBC"
author: "@viebel"

---

## Introduction

Basically, destructuring allows you to create local bindings in a very succint (and elegant) syntax.

In this article, we will demonstrate the basics of destructuring in clojure, using [KLIPSE][app-url]{:target="_blank"}.

## Destructuring a vector

The simplest example is destructuring the first `n` values of a vector:

~~~klipse
(def point [5 7])

(let [[x y] point]
    {:x x
         :y y})
~~~


A more advanced example is splitting a vector into a `head` and a `tail`:

~~~klipse
(def indexes [1 2 3])

(let [[x & more] indexes]
  {:x x :more more})
~~~

It's also worth noticing that you can bind the entire vector to a local using the `:as` directive.


~~~klipse
(def indexes [1 2 3])

(let [[x & more :as full-list] indexes]
  {:x x :more more :full-list full-list})
~~~

## Destructuring a map

Simple destructuring on a map is as easy as choosing a local name and providing the key.

~~~klipse
(def point {:x 5 :y 7})

(let [{the-x :x the-y :y} point]
         {:x the-x :y the-y})
~~~

As the example shows, the values of `:x` and `:y` are bound to locals with the names `the-x` and `the-y`.

Usually, you want to create locals with the same name as the keys of the map.

In this case, the syntax becomes even simpler, using the `:keys` directive:

~~~klipse
(def point {:x 5 :y 7})

(let [{:keys [x y]} point]
  (+ x y))
~~~

As with vectors, you can bind the entire map to a local using the `:as` directive.

Here is how to combine `:keys` and `:as`.

~~~klipse
(def point {:x 5 :y 7})

(let [{:keys [x y]} point]
  (+ x y))
~~~

And here is how to combine destructuring of vectors and maps (thank you Paul Salaberria):

~~~klipse
(let [[[one :as vec-one]
[{:keys [k] :as themap} :as vec-two] :as x]
[[1] [{:k "val"}]]]
{:x x
:one one
:vec-one vec-one
:k k
:themap themap
:vec-two vec-two})
~~~

In the [next article]({% post_url 2016-03-31-destructuring-part-2 %}){:target="_blank"}, we will show more advanced usages og destructuring in clojure.

[app-url]: http://app.klipse.tech


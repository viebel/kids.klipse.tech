---
layout: post
title:  "Clojure Append Cyclic"
date:   2016-03-28 12:31:46 +0200
categories: clojure
thumbnail: assets/klipse.png
description: "Clojure Append Cyclic"
author: "@viebel"
---

## Introduction
In this article, we present an interesting function named `append-cyclic`.

Let's say, you want to log events but you don't want to limit the memory consumption. Let's say you want to store the 1000 last events of your system.

In clojure, it's really easy to achieve that:

1. you initialize a vector of size 1000 (e.g 1000 times `nil`)
2. on each `append`, you remove the first (the oldest) element


## Naive Implementation

Let's see it in action with [KLIPSE][app-url]{:target="_blank"}:

~~~klipse
(defn append-cyclic[lst a]
(concat (rest lst) [a]))


(-> (repeat 3 nil)
(append-cyclic  9)
(append-cyclic  10)
(append-cyclic  11)
(append-cyclic  12))
~~~


On each call to `append-cyclic`, the first element of the vector is removed and the new element is appended at the end of the vector.

## Efficient implementation

Now, let's improve our code from a performace perspective, by using a FIFO queue - `PersistentQueue` - instead of lists, as it was suggested by [Jozef Wagner](https://disqus.com/by/jozefwagner/){:target="blank"}.

~~~klipse
(defn queue
[size]
(into (PersistentQueue.) (repeat size nil)))

(defn append-cyclic-queue
[queue x]
(pop (conj queue x)))


(-> (queue 3)
(append-cyclic-queue  9)
(append-cyclic-queue  10)
(append-cyclic-queue  11)
(append-cyclic-queue  12))
~~~


## Performance comparison
Here is a perfomance comparison of the two approaches using [KLIPSE][app-url]{:target="_blank"}. (Read more about [Performance comparison with KLIPSE]({% post_url 2016-03-29-profiling %})).

~~~klipse

(defn run [q iterations func]
(loop [n 0 
       q q]
           (if (< n iterations)
                 (recur (inc n) (func q n))
                       q)))


(with-out-str
(time (run (queue 100) 1000 append-cyclic-queue)))
~~~

~~~klipse
(with-out-str
(time (run (queue 100) 1000 append-cyclic)))
~~~


         
You see that the queue based approach is much must faster.


## Clojure vs. Clojurescript
Almost, all the code presented here is portable between `clojure` and `clojurescript`, except the code for the queue creation.

If you want to use the queue based implementation in `clojure`, you have to create the queue with a slighlty diffent syntax:

~~~ clojure
(clojure.lang.PersistentQueue/EMPTY)
~~~

[app-url]: http://app.klipse.tech

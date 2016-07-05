---
layout: post
title:  "Measuring clojurescript performance with KLIPSE"
description:  "Measuring clojurescript performance with KLIPSE"
date:   2016-03-29 10:12:36 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "F043D871-9E0C-4CD6-B80E-067D97B0E85A"
author: "@viebel"
---

Let's say that you want to profile a piece of code in clojurescript. 
There is a cool macro for that in clojurescript: [time](https://clojuredocs.org/clojure.core/time){:target="_blank}.

>`time` evaluates an expression and prints the time it took and returns the value of the expression.


The interesting question is:

> Where does `time` print the time it took?

Usually, it prints in the browser console. This is because, you have probably called `(enable-console-print!)` somewhere in your code.

But as a [KLIPSE][app-url-js]{:target="_blank"} user, you want the time to be displayed in the evaluation rectangle instead of inside the console.

It's simple to achieve that using [with-out-str](https://clojuredocs.org/clojure.core/with-out-str){:target="_blank}.

Let's see it in action by comparing the running time of two naive implementations for prime number generation.


Here is a very naive implementation:

~~~klipse
(defn is-prime? [n]
  (empty? (filter #(= 0 (mod n  %)) (range 2 n))))

(defn nth-prime [n]
  (last (take n (filter #(is-prime? %) (iterate inc 2)))))


(with-out-str (time (nth-prime 50)))

~~~

And now a less naive implementation:

~~~klipse
(defn is-prime-opt? [n]
  (or (= 2 n)
     (not-any? #(= 0 (mod n %)) (range 3 (inc (Math/sqrt n)) 2))))

(defn nth-prime-opt [n]
  (last (take n (filter #(is-prime-opt? %) (cons 2 (iterate (partial + 2) 3))))))


(with-out-str (time (nth-prime-opt 50)))

~~~


[app-url-js]: http://app.klipse.tech?js_only=1


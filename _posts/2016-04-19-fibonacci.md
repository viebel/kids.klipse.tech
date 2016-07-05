---
layout: post
title:  "3 ways to generate Lazy Fibonacci sequences in Clojure"
description:  "How to write Lazy Fibonacci in Clojure. share code clojure with KLIPSE. macros with klipse. lazy sequences fibonacci."
date:   2016-04-20 01:28:14 +0200
categories: clojurescript
thumbnail: assets/klipse.png
guid: "F34D7A10-69DB-493D-909D-374E5D4580A6"
author: "@viebel"

---


Generating the Fibonacci sequence is a good exercise to illustrate the concept of lazy sequences in `Clojure`.
In this article, we will present 3 ways to create a lazy Fibonacci sequence:

- lazy-seq
- lazy-cat
- iterate

We will also demonstrate two interesting features of [KLIPSE][app-url]{:target="_blank"}:

- how to share code with KLIPSE
- macros usage in KLIPSE


### Math and History

In mathematics, the Fibonacci numbers or Fibonacci sequence are the numbers in the following integer sequence:

> 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 ...


or (often, in modern usage):

> 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 ...

The first two numbers in the Fibonacci sequence are either 1 and 1, or 0 and 1, and each subsequent number is the sum of the previous two.

The Fibonacci sequence is named after Italian mathematician Leonardo of Pisa, known as Fibonacci. His 1202 book Liber Abaci introduced the sequence to Western European mathematics, although the sequence had been described earlier as Virahanka numbers in Indian mathematics.


![Secret](/assets/fibonacci.jpg)

### Fibonacci with `lazy-seq`



Here is the doc for `lazy-seq`:

~~~
(lazy-seq & body)
Takes a body of expressions that returns an ISeq or nil, and yields
a Seqable object that will invoke the body only the first time seq
is called, and will cache the result and return it on all subsequent
seq calls. See also - realized?
~~~

Let's do Fibonacci with `lazy-seq`:

~~~klipse
(def fib-seq-seq
  ((fn fib [a b] 
       (lazy-seq (cons a (fib b (+ a b)))))
     0 1))

(take 30 fib-seq-seq)
~~~

Now, imagine you are very proud of your code and you'd like to share it by email, on twitter or on Clojurians Slack. How would you do that? 

- The common option is to copy/paste it => no indentation and colors
- A more elegant way is to create a github gist => indentation, colors but the code is **static**
- Another way is to share your klipse => indentation, colors and the code is **dynamic** and **editable**

In order to share your klipse, you press `Ctrl-S` (`S` is for share or save) and the url of a klipse with your code (something like http://app.klipse.tech?cljs_in=....) will be displayed in an alert box and in the browser console.

Go ahead! Give it a try in the Klipse box above that contains the `lazy-seq` implementation of the Fibonacci sequence.

### Fibonacci with `lazy-cat`

Here is the doc for `lazy-cat`:

~~~
(lazy-cat & colls)

Expands to code which yields a lazy sequence of the concatenation
of the supplied colls.  Each coll expr is not evaluated until it is
needed. 

(lazy-cat xs ys zs) === (concat (lazy-seq xs) (lazy-seq ys) (lazy-seq zs))
~~~

Now, we'll do Fibonacci with `lazy-cat`:

~~~klipse
(def fib-seq-cat
  (lazy-cat [0 1] (map + (rest fib-seq-cat) fib-seq-cat)))

(take 30 fib-seq-cat)
~~~

If you prefer this version, don't hesitate share it! `Ctrl-S` is your friend...

### Fibonacci with `iterate`

Here is the doc for `iterate`:

~~~
(iterate f x)
Returns a lazy sequence of x, (f x), (f (f x)) etc...
f must be free of side-effects
~~~

Now, Fibonacci with `iterate`:


~~~klipse
(def fib-seq-iterate
  (map first (iterate 
                 (fn [[a b]] [b (+ a b)]) [0 1])))

(take 30 fib-seq-iterate)
~~~


Do you have another interesting Fibonacci sequence implementation? Feel free to share your klipse in the comments below...

Clojure rocks!

[app-url-static]: http://app.klipse.tech?blog=klipse&static-fns=true&js_only=1
[app-url]: http://app.klipse.tech?blog=klipse&static-fns=true&js_only=1


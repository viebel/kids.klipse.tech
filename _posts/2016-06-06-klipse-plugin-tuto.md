---
layout: post
title:  "How to klipsify a clojure[script] blog post #cljklipse @viebel"
description:  "klipsify klipse plugin blog article post awesome live code coding examples gist clojure clojurescript"
date:   2016-06-07 03:27:22 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "A46AC86E-0840-4A97-A976-93F421CE8FA4"
author: "@viebel"
---

# The pain

Look at a typical blog post in `clojure[script]`. The post usually presents a couple of code snippets. As I see it, there are two pains with code snippets:

1. they contain the input and the output but not the actual evaluation of the input
2. it's impossible for the reader to modify the output

# The forgotten dream

A long time ago, all the developers had a common dream. The dream was about interactivity, liveness, evaluation...

But we put this dream aside - because the browser understands only `javascript`.

And after a while, we even forgot that we ever had this dream.


Still, there are some people that didn't forget this dream, like Alan Kay:

>Question: Well, look at Wikipedia — it's a tremendous collaboration.

>Alan Kay: It is, but go to the article on [Logo](https://en.wikipedia.org/wiki/Logo_(programming_language)), can you write and execute Logo programs? Are there examples? No. The Wikipedia people didn't even imagine that, in spite of the fact that they're on a computer.

Here is the [full interview of Alan Kay](http://www.drdobbs.com/architecture-and-design/interview-with-alan-kay/240003442?pgno=2){:target="_blank"}. (Thanks [@fasihsignal](https://twitter.com/fasihsignal) for bringing this quote to our awareness.)

![dream](/assets/dream.jpg)

# The klipse plugin

The klipse plugin is a small step toward this dream: it is a `javascript` tag that transforms static `clojure` code snippets of an html page to live and interactive snippets.

[Klipse](https://github.com/viebel/klipse) is written in `clojurescript`, it uses [replumb](https://github.com/Lambda-X/replumb) for code evaluation (self-hosted cljs) and [CodeMirror](http://codemirror.net/) for text editing. 

For instance, let's have on this page a static code snippet with `(map inc [1 2 3])`:

~~~clojure
(map inc [1 2 3])
;(2 3 4)
~~~

(This blog is written with `jekyll`: the `kramdown` plugin helps a lot in beautifying the code snippets.)

And now, we are going to **klipsify** this code snippet:

~~~klipse
(map inc [1 2 3])
~~~

Feel free to edit the code above: it's interactive => it evaluates again as you type.

All I had to do in order to **klipsify** my code snippet, was to set the `language-klipse` class (configurable) to the appropriate html element.

See it by yourself: here is the source of this page:

~~~html
<p>And now, we are going to <strong>klipsify</strong> this code snippet:</p>


<pre><code class="language-klipse">(map inc [1 2 3])
</code></pre>
~~~

Ah! I forgot to mention that I had to remove `;(2 3 4)` from the code snippet.

Before dealing about integration of the klipse plugin on a web page, let's enjoy another klipse snippet for generating a lazy Fibonacci sequence:

~~~klipse
(def fib-seq-seq
  ((fn fib [a b] 
       (lazy-seq (cons a (fib b (+ a b)))))
     0 1))
(take 13 fib-seq-seq)
~~~

# Integration

All you need to do in order to integrate the klipse plugin to your blog (or any other web page), is to add this `javascript` tag to your web page:

~~~html
<link rel="stylesheet" type="text/css" href="http://app.klipse.tech/css/codemirror.css">

<script>
    window.klipse_settings = {
        selector: '.language-klipse'// css selector for the html elements you want to klipsify
    };
</script>
<script src="http://app.klipse.tech/plugin/js/klipse_plugin.js"></script>
~~~

By the way, this is exactly what we did on the page that you are currently reading.

Feel free to contact us for further explanations: 

- [@viebel](https://github.com/viebel) (clojurians + twitter), [viebel@gmail.com](mailto:viebel@gmail.com?Subject=Klipse){:target="_blank"}
- @raphael (clojurians), [@RaphaelBoukara](https://github.com/raphaelboukara) (twitter) , 
[raphael.boukara@gmail.com](mailto:raphael.boukara@gmail.com?Subject=Klipse){:target="_blank"}

# Other languages

The klipse plugin is designed as a platform that could support any language that has a client-side evaluator, by writing modules to the klipse plugin. Currently, there are modules available for the following languages: 


- javascript: [A new way of blogging about javascript](http://blog.klipse.tech/javascript/2016/06/20/blog-javascript.html)
- ruby: [A new way of blogging about ruby](http://blog.klipse.tech/ruby/2016/06/20/blog-ruby.html)

# Limitations and issues

There are a couple of limitations and issues with the klipse plugin:

- The `javascript` is quite big (around 1.2MB zipped, 9.4MB unzipped). Currently self-host `clojurescript` doesn't support advanced compilation. We did our best not to impact the page loading time.

- You can require `clojure` libraries in your code snippet but the code might take a while to execute, as the code of the library is JIT loaded from github. Here is an example with `clojure.set`:


~~~klipse
(ns my.set
  (:require [clojure.set :refer [map-invert]]))

(map-invert {:klipse "snippet" :dreaming "interactivity"})
~~~

- When you require a `clojure` library, klipse will try to load the library from several repos. As a consequence, if you open the browser dev tools, you'll see a lot of `404`s.  

- Most of the `clojure` code is valid `clojurescript` code and will be evaluated properly by self-host `clojurescript`. But there are a lot of things that are not (yet) implemented in `clojurescript`: refs, agents, Ratio, BigDecimal, BigInteger, Character literals, runtime enforcement of arity when calling a fn... (see the full list here: [Differences between Clojure and Clojurescript](https://github.com/clojure/clojurescript/wiki/Differences-from-Clojure)).

- Requiring custom `clojure[script]` libraries in a klipse snippet is not yet available.

Share your thoughts (and your dreams) in the comments below.

Clojurescript rocks!

[app-url]: http://app.klipse.tech?blog=klipse


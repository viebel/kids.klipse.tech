---
layout: post
title:  "A new way of blogging about javascript"
description:  "blogging klipse javascript repl live examples code snippets"
date:   2016-06-20 08:11:42 +0200
categories: javascript
thumbnail: assets/klipse.png
guid: "2ADBAB51-2DEF-4394-88F7-271C2C8DFF9E"
author: "@viebel"
minified_plugin: true
---

This blog post is about to show a new way of blogging about `javascript`.

Look at a typical blog post about `javascript`: The post usually presents a couple of code snippets. As I see it, there are two pains with code snippets:

1. they contain the input and the output but not the actual evaluation of the input
2. it's impossible for the reader to modify the output

We have great tools to evaluate `javascript` code snippets: `jsfiddle`, `codepen`, `plnkr` and many more...

But all of this tools deal are very cumbersome to embed into a blog post about `javascript`.

Try to embed a `jsfiddle` that evaluates `2 + 3` and you will understand that cumbersome is actually an understatement...

# Alan Kay's vision

For Alan Kay, evaluation an interactivity should be available everywhere in the web:

>Question: Well, look at Wikipedia — it's a tremendous collaboration.

>Alan Kay: It is, but go to the article on [Logo](https://en.wikipedia.org/wiki/Logo_(programming_language)), can you write and execute Logo programs? Are there examples? No. The Wikipedia people didn't even imagine that, in spite of the fact that they're on a computer.

Here is the [full interview of Alan Kay](http://www.drdobbs.com/architecture-and-design/interview-with-alan-kay/240003442?pgno=2){:target="_blank"}. (Thanks [@fasihsignal](https://twitter.com/fasihsignal) for bringing this quote to our awareness.)

![dream](/assets/dream.jpg)

# The klipse plugin

The klipse plugin is a small step toward Alan Kay's vision: it is a `javascript` tag that transforms static `javascript` code snippets of an html page to live and interactive snippets:

1. **Live**: The code is executed in your browser
2. **Interactive**: You can modify the code and it is evaluated as you type

[Klipse](https://github.com/viebel/klipse) is written in `clojurescript`, and uses [CodeMirror](http://codemirror.net/) for text editing. 


# Klipsify a javascript code snippet

Let's have on this page a static code snippet with `[1,2,3].map(function(x){ return x + 1;})`:

~~~javascript
[1,2,3].map(function(x){ return x + 1;})
//2,3,4
~~~

(This blog is written with `jekyll`: the `kramdown` plugin helps a lot in beautifying the code snippets.)

And now, we are going to **klipsify** this code snippet:

~~~klipse-eval-js
[1,2,3].map(function(x){ return x + 1;})
~~~

Feel free to edit the code above: it's interactive => it evaluates as you type.

All I had to do in order to **klipsify** my code snippet, was to set the `language-klipse-eval-js` class (configurable) to the appropriate html element.

See it by yourself: here is the source of this page:

~~~html
<p>And now, we are going to <strong>klipsify</strong> this code snippet:</p>

<pre><code class="language-klipse-eval-js">[1,2,3].map(function(x){ return x + 1;})
</code></pre>
~~~


# Live demo

Before dealing about integration of the klipse plugin on a web page, let's enjoy another klipse snippet for an `Hello World` in `EcmaScript 6`:

~~~klipse-eval-js
var hi = name => "hello " + name;
hi("klipse")
~~~

Go ahead! modify the klipse snippet above, and it will evaluate as you type...

# Loading external libraries


You can specify the external libraries that your klipse snippet needs by setting the `data-external-libs` attribute of the `DOM` element.

For instance, let's use `reduce` from [underscore.js](http://underscorejs.org/):

<pre>
<div class="language-klipse-eval-js" data-external-libs="http://underscorejs.org/underscore-min.js">

_.reduce([1, 2, 3], function(memo, num){ return memo + num; }, 0);
</div>
</pre>

Again, enjoy the interactivity and modify the code...

Look at the page source to see the details of `data-external-libs`...

# Evaluating a gist

We can also evaluate code from a `gist`.

For instance, let's klipsify [this gist](https://gist.github.com/viebel/62d62220da0507860102c8ca6ad6db86) that returns the cartesian product of `[1, 2], [3, 4], ['a', 'b']`:

<pre>
<div class="language-klipse-eval-js" data-gist-id="viebel/db1f5c9dac9bf5198ceb0b95827dedf1"></div>
</pre>

Again, enjoy the interactivity and modify the code...

If you look at the page source, you'll see that I don't need to load `underscore.js` again because it was already loaded by a previous klipse snippet.

# Klipse plugin integration

All you need to do in order to integrate the klipse plugin to your blog (or any other web page), is to add this `javascript` tag to your web page:

~~~html
<link rel="stylesheet" type="text/css" href="http://app.klipse.tech/css/codemirror.css">

<script>
    window.klipse_settings = {
        selector_eval_js: '.language-klipse-eval-js', // css selector for the html elements you want to klipsify
    };
</script>
<script src="http://app.klipse.tech/plugin_prod/js/klipse_plugin.min.js"></script>
~~~

By the way, this is exactly what we did on the page that you are currently reading.

# Other languages

The klipse plugin is designed as a platform that could support any language that has a client-side evaluator, by writing modules to the klipse plugin. Currently, there are modules available for the following languages: 

- ruby: [A new way of blogging about ruby](http://blog.klipse.tech/ruby/2016/06/20/blog-ruby.html)
- clojure[script]: [How to klipsify a clojure[script] blog post](http://blog.klipse.tech/clojure/2016/06/07/klipse-plugin-tuto.html)
- PHP: [A new way of blogging about PHP](http://blog.klipse.tech/php/2016/06/26/blog-php.html)

# Blog posts examples

Here are a couple of blog posts with  `javascript` klipse snippets:

- [immutable.js live tutorial](http://blog.klipse.tech/javascript/2016/03/30/immutable.html)
- [A secret about javascript eval](http://blog.klipse.tech/javascript/2016/06/20/js-eval-secrets.html)


---
[app-url]: http://app.klipse.tech?blog=klipse


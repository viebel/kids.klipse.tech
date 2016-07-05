---
layout: post
title:  "Multi language live snippets with the klipse plugin by @viebel"
description:  "Multi language live and interactive code snippets with the klipse plugin: javascript, ruby, PHP, clojure."
date:   2016-06-28 02:07:23 +0200
categories: all
thumbnail: assets/klipse.png
guid: "2B82D2A8-DB18-44F0-BAD6-A4738F970302"
author: "@viebel"
ruby: true
php: true
---

# What is Klipse?

The klipse plugin is a javascript tag that transforms static code snippets of an html page to **live** and **interactive** snippets:

1. **Live**: The code is executed in your browser
2. **Interactive**: You can modify the code and it is evaluated as you type

The code evaluation is done in the browser: no server is involved at all!

Read more [about the klipse plugin](https://github.com/viebel/klipse).

In this short article, we show live code snippets in different languages.


Modify the code in the snippets and enjoy the interactivity!

![jaguar](/assets/jaguar.jpg)

# Javascript

~~~klipse-eval-js
[1, 2, 3].map((x) => x + 1)
~~~

# Ruby

~~~klipse-eval-ruby
[1, 2, 3].map{|x| x + 1}
~~~

# PHP

~~~klipse-eval-php
$name = "klipse";
echo "hello". " " . $name;
~~~

# Clojure[script]

~~~klipse
(map inc [1 2 3])
~~~






---
layout: post
title:  "Dead code elimination in clojurescript"
date:   2016-03-24 00:25:46 +0200
categories: clojure
thumbnail: assets/klipse.png
description: "A simple example of dead code elimination in clojurescript, using KLIPSE"
author: "@viebel"

---

Do you see any substantial difference between the two pieces of code below?

**Code #1:**

~~~clojure
(if 2 3)
~~~

**Code #2:**

~~~clojure
(def x (if 2 3))
~~~

They seem to be quite the same, so you'd expect their js transpiled code to be the same. 

Let's check it with [KLIPSE][app-url-with-input]: 


~~~klipse-js
(if 2 3)
(def x (if 2 3))
~~~

Do you see what's going on here?

**Code #1** is completely dead code no matter what code will come after it. Therefore it has been completly eliminated by the cljs transpiler!

Feel free to continue to play with dead code with [KLIPSE][app-url-with-input].

[app-url]: http://app.klipse.tech
[app-url-with-input]: http://app.klipse.tech?js_only=1&cljs_in=(if%202%203)%0A(def%20x%20(if%202%203)) 

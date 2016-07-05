---
layout: post
title:  "javascript eval secrets"
description:  "javscript eval live example ecmascript 5 6"
date:   2016-06-20 08:11:42 +0200
categories: javascript
thumbnail: assets/klipse.png
guid: "4BEFF6B0-8E1C-482F-98B0-BCA39A0F84B1"
author: "@viebel"
minified_plugin: true

---

Every `javscript` developer know that `eval` is evil.

But `eval` is really powerful and from a theoretical perspective `eval` is very interesting.

For instance, did you ever ask yourself in what context does `eval` run?

In this article, we are going to show a couple of examples that relates to this question.

![Power](/assets/power.jpg)

# Outside Scope or Inside Scope?

Usually, `eval` runs inside the scope of the caller function:

~~~klipse-eval-js
var context = 'outside';
(function(){
    var context = 'inside';
    return eval('context');
})();
~~~

All the examples of this page are live and interactive examples:

1. The code is executed in your browser
2. You can modify the code and it is evaluated as you type

But sometimes, `eval` runs outside the scope of the caller function:

~~~klipse-eval-js
var context = 'outside';
(function(){
    var context = 'inside';
    return eval.call(null, 'context');
})();
~~~

Also in this case - outside:

~~~klipse-eval-js
var context = 'outside';
(function(){
    var context = 'inside';
    return (1, eval)('context');
})();
~~~

But in this case - it's inside:

~~~klipse-eval-js
var context = 'outside';
(function(){
    var context = 'inside';
    return (eval)('context');
})();
~~~

And another one that gets outside:

~~~klipse-eval-js
var context = 'outside';
(function(){
    var context = 'inside';
    var my_eval = eval;
    return my_eval('context');
})();
~~~


It depends whether the `eval` call is direct or indirect.

Furher details and explanations in this [article](http://perfectionkills.com/global-eval-what-are-the-options/) that explains the difference between `eval` direct and indirect call in `EcmaScript 5` and other cool stuff.

---
[app-url]: http://app.klipse.tech?blog=klipse


---
layout: post
title:  "Clojure Macros Tutorial - part 3: Syntax Quote (aka Backtick) in Clojure"
description:  "macros tutorial splicing Syntax Quote, Backquote, Backtick in Clojure"
date:   2016-05-05 03:24:53 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "C1450F62-F4D4-4909-BB44-1C091B53ABBC"
author: "@viebel"

---

`Clojure` provides a powerful tool to allow the developer to write macros effectively. This tool is called "Syntax quote".

This tool provide an elegant solution to the issues mentioned in [how not to write macros in Clojure]({% post_url 2016-05-04-macro-tutorial-2 %}){:target="_blank"}.

Syntax quote has 4 powerful features:

1. fully-qualified symbols
2. unquote with `~`
3. unquote splicing with `~@`
4. generated symbols with `#`

Here is the [official documentation for syntax quote](http://clojure.org/reference/reader#__a_id_syntax_quote_a_syntax_quote_note_the_backquote_character_unquote_and_unquote_splicing){:target="_blank"}.

But this documentation is too cryptic.

In this article, we present this powerful tool in a much digestible way...

![Dummies](/assets/quoting_dummies.jpg)

### Regular Quote

Before dealing with syntax quoting, let's remember how the regular quote works.

There are two equivalent ways to quote a form either with `quote` or with `'`.
The latter is much more convenient so we will use it.

It works recursively with any kind of forms and types: strings, maps, lists, vectors...

Let's have a look at some examples:

~~~klipse
'(a :a 1)
~~~

~~~klipse
'(a (b (c d (e f (g h) i) j)))
~~~

~~~klipse
'{:a (1 2 3) b (c d "x")}
~~~



### Syntax Quote - symbol resolution

Syntax quote is done with a backtick `` ` ``. It quotes a form and resolves symbols in the current context yielding a fully-qualified symbol. If the symbol doesn't exist in the current namespace, it is resolved to the current namespace.

When we use the regular quote, there is no namespace resolution:

~~~klipse
'(map)
~~~

While with syntax quote, the symbol is resolved:

~~~klipse
`(map)
~~~

If a symbol exists in the current namespace, it is resolved there:

~~~klipse
(ns my.quote)
(def a 123)
`(a)
~~~

If a symbol cannot be resolved, it is also resolved in the current namespace:

~~~klipse
`(b)
~~~


### Syntax Quote - unquote

With syntax quote, it's possible to unquote part of the form that is quoted with `~`. It allows you to evaluate part of the expression.

Without evaluation:

~~~klipse
`(16 17 (inc 17))
~~~

With evaluation:

~~~klipse
`(16 17 ~(inc 17))
~~~

Another one:

~~~klipse
`(16 17 ~(map inc [16 17]))
~~~


### Syntax Quote - unquote splicing

But what if you want to unquote a list and insert its elements (not the list) inside the quoted form?

No problem, `~@` is your friend (his official name is unquote splicing). And `~@` is really a good friend as he knows to handle any kind of collection.

Without splicing:

~~~klipse
 `(16 17 ~(map inc [16 17]))
~~~

With splicing:

~~~klipse
`(16 17 ~@(map inc [16 17]))
~~~

Other examples:

~~~klipse
`(1 2 ~@[1 [2 3]])
~~~

~~~klipse
`(1 2 ~@#{1 2 3})
~~~

~~~klipse
`(1 2 ~@{:a 1 :b 2 :c 3})
~~~

### Syntax Quote - symbol generation


Inside syntax quote, you can generate unique symbol names by appending `#` to the symbol.


~~~klipse
`(A#)
~~~

The cool thing is that all the references to that symbol within a syntax-quoted expression resolve to the same generated symbol.

~~~klipse
`(a b a# b#)
~~~

~~~klipse
`(a b a# b# a# b#)
~~~

~~~klipse
`{:a a# :b b# :c b#}
~~~

There are other advanced features available inside syntax quote like `~'`, `~~`and `'~@`.

We might write an article on it in the (near) future...


Clojure rocks!

[app-url-static]: http://app.klipse.tech?blog=klipse&js_only=1
[app-url]: http://app.klipse.tech?blog=klipse&static-fns=true&js_only=1


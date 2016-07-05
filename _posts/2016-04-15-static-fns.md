---
layout: post
title:  "Deep into clojurescript function call mechanisms: static vs. dynamic dispatch"
description:  "Deep into clojurescript function call mechanisms: static vs. dynamic dispatch"
date:   2016-04-14 01:19:23 +0200
categories: clojurescript
thumbnail: assets/klipse.png
guid: "C7711B82-686B-44A7-A626-D10AC56C087E"
author: "@viebel"

---


Function call is really the most fundamental piece of functional programming. This is the reason why we are writing so many articles on this topic: 

- [Extend IFn protocol like if you were part of clojurescprit core team]({% post_url 2016-04-07-ifn %})
- [Clojurescript defprotocol's secret]({% post_url 2016-04-09-clojurescript-protocols-secret  %})
- [IFn's magic beyond defprotocol secret]({% post_url 2016-04-11-extend-ifn %})

In this article, we present the differences between static and dynamic dispath in `clojurescript`.

### The Power of `call` in Javascript

As we explained it [in this article]({% post_url 2016-04-11-extend-ifn %}), javascript's `call` mechanism is very powerful, as it allows any object to behave like a function.

### Performance issues with `call`

The problem is that `call` causes severe performance issues as reported in [CLJS JIRA](http://dev.clojure.org/jira/browse/CLJS-910){:target="_blank"}  and in [WebKit Bugzilla](https://bugs.webkit.org/show_bug.cgi?id=139847){:target="_blank"}.

At its beginning, [KLIPSE][app-url]'s  performances were very poor on Safari and our blog was completely broken on iOS devices. We couldn't use advanced compilation as it is not supported in self hosted `clojurescript`.


### The solution: static-fns 

[Mike Fikes](https://twitter.com/mfikes){:target="_blank"} informed us that `:static-fns` [solves performance issues for self-host on Safari](http://dev.clojure.org/jira/browse/CLJS-1381){:target="_blank"}.

Here is the description of `:static-fns` as it appear in [clojurescript wiki page](https://github.com/clojure/clojurescript/wiki/Compiler-Options#static-fns){:target="_blank"}:

>:static-fns employs static dispatch to specific function arities in emitted JavaScript, as opposed to making use of the call construct. 

> Defaults to false except under advanced optimizations. 

> Useful to have set to false at REPL development to facilitate function redefinition, and useful to set to true for release for performance.

> This setting does not apply to the standard library, which is always compiled with :static-fns implicitly set to true.

Since Apr 8 2016, KLIPSE is compiled with `:static-fns true` and our Safari readers are happy.

![Secret](/assets/cheetah-speed.jpg)

If you are curious about what could break the static dispatch mechanism, read [this article by Mike Fikes](http://blog.fikesfarm.com/posts/2016-04-14-static-free-clojurescript-repl.html){:target="_blank"} 

### Examples

The good news for us - the KLIPSE fans - is that the `static-fns` option is available in self host. It's available in KLIPSE via the `static-fns` url parameter. Here is the [full list of KLIPSE url parameters]({% post_url 2016-03-27-klipse-manual %}){:target="_blank"}.

Now, let's explore the effects of `static-fns` by playing with live code. 
You might find it more convenient to open [KLIPSE with static-fns=false](http://app.klipse.tech/?cljs_in=(defn%20foo%20%0A%20%20(%5B%5D%20%22foo%22)%0A%20%20(%5Bx%5D%20x)%0A%20%20(%5Bx%20y%5D%20y))%0A%0A(foo)%0A(foo%201)%0A(foo%201%202)%0A(foo%201%202%203)%0A%20%20&js_only=1&blog=klipse&static-fns=false){:target="_blank"} or [KLIPSE with static-fns=true](http://app.klipse.tech/?cljs_in=(defn%20foo%20%0A%20%20(%5B%5D%20%22foo%22)%0A%20%20(%5Bx%5D%20x)%0A%20%20(%5Bx%20y%5D%20y))%0A%0A(foo)%0A(foo%201)%0A(foo%201%202)%0A(foo%201%202%203)%0A%20%20&js_only=1&blog=klipse&static-fns=true){:target="_blank"} in another window.

Here is a kind of hello world program compiled without static dispatch.

First let's look how a multi-arity function is transpiled:

~~~klipse-js
(defn foo 
  ([] "foo")
    ([x] x)
      ([x y] y))
~~~

And now, for the function calls:

~~~klipse-js
(foo)
(foo 1)
(foo 1 2)
(foo 1 2 3)
~~~
  

Without static dispatch, the compiler emits the same code for all the calls to `foo` no matter what is the arity: `cljs.user.foo.call(null,...)`. The proper function is calculated at run time. Actually, this is exactly what `cljs.user.foo` does: it dispatches to the proper arity function!


Now, let's see what happens with static dispatch:


<pre>
<div class="language-klipse-js" data-static-fns="true">
(foo)
(foo 1)
(foo 1 2)
(foo 1 2 3)
</div>
</pre>

With static dispatch, the compiler emits code that calls the proper arity for `foo`: 

- 0-arity => `cljs.user.foo.cljs$core$IFn$_invoke$arity$0`
- 1-arity => `cljs.user.foo.cljs$core$IFn$_invoke$arity$1`
- 2-arity => `cljs.user.foo.cljs$core$IFn$_invoke$arity$2`

And for 3-arity, it falls back to `cljs.user.foo((1),(2),(3))` as `foo` doesn't implement 3-arity.

### Behind the scenes: static dispatch 

Static dispatch is done by the `clojurescript` compiler inside the function [cljs.compiler.emit](https://github.com/clojure/clojurescript/blob/master/src/main/clojure/cljs/compiler.cljc#L919-L1013){:target="_blank"}.

Here is the part that does the static dispatch for the case where the arity is completly known at compile time: the compiler checks whether there is an implementation for the arity invoked in the expression:

~~~clojure
;; direct dispatch to specific arity case
 :else
 (let [arities (map count mps)]
   (if (some #{arity} arities)
     [(update-in f [:info]
        (fn [info]
          (-> info
            (assoc :name (symbol (str (munge info) ".cljs$core$IFn$_invoke$arity$" arity)))
            ;; bypass local fn-self-name munging, we're emitting direct
            ;; shadowing already applied
            (update-in [:info]
              #(-> % (dissoc :shadow) (dissoc :fn-self-name)))))) nil]
     [f nil]))))
~~~

### What else?

I really encourage you to play with `static-fns=true` in [KLIPSE][app-url-static]{:target="_blank"} and to try to find other cases that might break the compiler static dispatch.

In another post, we will explain how static dispatch mechanism works for `IFn` protocol extension.

Meanwhile, please share your thoughts in the comments below...


Clojurescript rocks!

[app-url-static]: http://app.klipse.tech?blog=klipse&static-fns=true&js_only=1
[app-url]: http://app.klipse.tech?blog=klipse&static-fns=true&js_only=1


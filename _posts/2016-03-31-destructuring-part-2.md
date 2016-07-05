---
layout: post
title:  "Destructuring in Clojure - Part 2"
description:  "Destructuring in Clojure - Part 2"
date:   2016-03-31 14:19:46 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "152A4315-3237-4C87-9569-74CC15878CE2"
author: "@viebel"

---

In a [precedent post]({% post_url 2016-03-30-destructuring %}){:target="_blank"}, we have presented the basics of destructuring in Clojure.

Here, we present a trully amazing way of defining default options for a function, leveraging Clojure destructuring.

## The need

Let's say you want to write a function `foo` that is very cusomizable, something like 27 optional parameters where each parameter has a default value.

How do you write such a function in Clojure?

Think about it for a few minutes before going on... 

<br/>  
<br/>  
<br/>  
<br/>  
<br/>  
<br/>  

Think about it for a few more minutes before going on... 

<br/>  
<br/>  
<br/>  

## The solution

As it often happens, `Clojure` provides a simple and elegant solution for this complex semantic problem.

There is a cool `:or` directive available for destructuring.

We will illustrate it with a simple `hello-world` function that receives two options: `language` and `upper-case?`.

The cool thing is that with one line of code, you create local bindings with default values:

~~~ clojure
{:keys [language upper-case?] 
  :or {language :en
       upper-case? false}
~~~
**Pay attention to the way the keys are defined inside the `:or` directive: the keys are defined as symbols, there are no `:`!**


Enough words!

Let's see it in action, with [KLIPSE][app-url]{:target="_blank"} (feel free to play with the code, inside the article):

~~~klipse
(ns my.args
  (:require [clojure.string :as string]))

(defn hello-world [& {:keys [language upper-case?] 
                      :or {language :en
                                                 upper-case? false}}]

                                                   (let [greeting (case language
                                                                      :fr "bonjour monde"
                                                                                         :en "hello world")]
                                                                                             (if upper-case?
                                                                                                   (string/upper-case greeting)
                                                                                                         greeting)))

  (hello-world)
~~~


~~~klipse
    (hello-world :language :fr)
~~~

~~~klipse
      (hello-world :upper-case? true)
~~~


Clojure rocks!

[app-url]: http://app.klipse.tech


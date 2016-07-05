---
layout: post
title:  "KLIPSE: Clojure libs are now available!"
date:   2016-03-29 18:57:46 +0200
categories: clojure
thumbnail: assets/klipse.png
description: "Clojure libs are now available inside KLIPSE"
author: "@RaphaelBoukara"
---

Let's talk about a new pretty feature in [KLIPSE][app-url]{:target="_blank"}!

All your favorite clojure libraries you are using everyday in your development are now available inside KLIPSE.

~~~klipse
(ns my.ns
  (:require 
      [clojure.string :as string]))

(string/blank? "HELLO!!")
~~~

And `:refer` also works:

~~~klipse
(ns my.ns
  (:require 
      [clojure.string :refer [join]]))

(join "," ["hello" "world"])
~~~

[app-url]: http://app.klipse.tech

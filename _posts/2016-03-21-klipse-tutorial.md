---
layout: post
title:  "KLIPSE: how we built it?"
date:   2016-03-21 07:57:46 +0200
categories: clojure
thumbnail: assets/klipse.png
description: "In this post, we propose a basic tutorial to explain step by step how KLIPSE is build."
author: "@RaphaelBoukara"
---

In a [precedent post][klipse-presentation-url] you can read the explanation about what is [KLIPSE][klipse-presentation-url] and what was the motivation to build it.
In this post, you will find a basic tutorial to explain step by step how KLIPSE is build.

Final render of the tutorial: 

![KLIPSE Screenshot Tutorial](/assets/klipse_tutorial_final_render.png)

Of course, the design of the app not the subject of the tutorial.

### Tutorial Summary

1. [init project](#init-project)
2. [figwheel](#figwheel)
3. [compiler functions](#compiler-functions)
4. [om.next](#omnext)


1- Init Project
----------

Create a new project directory, switch into it, add a `project.clj` configuration file to it.

~~~
mkdir cljs-compiler
cd cljs-compiler
touch project.clj
~~~

~~~ clojure
(defproject cljs-compiler "0.1.0-SNAPSHOT"
  :description "Eval clojurescript to javascript!"
  :dependencies [[org.clojure/clojure "1.7.0"]
                 [org.clojure/clojurescript "1.7.170"]]
  :plugins [[lein-cljsbuild "1.1.2" :exclusions [[org.clojure/clojure]]]]
  :cljsbuild {:builds
              [{:id "dev"
                :source-paths ["src"]
                :compiler {
                  :main cljs-compiler.core
                  :asset-path "js"
                  :output-to "resources/public/js/main.js"
                  :output-dir "resources/public/js"}}]})
~~~

Create a `resources/public/index.html` file with the following contents:

~~~
mkdir -p resources/public
touch resources/public/index.html
~~~

~~~ html
<!DOCTYPE html>
<html>
    <head lang="en">
        <meta charset="UTF-8">
        <title>Eval clojurescript to javascript!</title>
    </head>
    <body>
        <script src="js/main.js"></script>
    </body>
</html>
~~~

Create a file `src/cljs_compiler/core.cljs` with the following contents:

~~~
mkdir -p src/cljs_compiler
touch src/cljs_compiler/core.cljs
~~~

~~~ clojure
(ns cljs_compiler.core)

(enable-console-print!)

(println "Hello world!")
~~~

~~~
lein cljsbuild once dev
~~~

Now open the `index.html` file in a chrome browser, open the console, great! you can see our "Hello world!" log.


2- figwheel
----------

It is a good idea to use [figwheel][figwheel-url] to make your devlopment more funny. Add figwheel plugin to your `project.clj` and `:figwheel true` configuration to your dev compilation configuration:

~~~ clojure
(defproject cljs-compiler "0.1.0-SNAPSHOT"
  :description "Eval clojurescript to javascript!"
  :dependencies [[org.clojure/clojure "1.7.0"]
                 [org.clojure/clojurescript "1.7.170"]]
  :plugins [[lein-figwheel "0.5.0-6"]
            [lein-cljsbuild "1.1.2" :exclusions [[org.clojure/clojure]]]]
  :cljsbuild {:builds
              [{:id "dev"
                :source-paths ["src"]
                :figwheel true
                :compiler {
                  :main cljs-compiler.core
                  :asset-path "js"
                  :output-to "resources/public/js/main.js"
                  :output-dir "resources/public/js"}}]})
~~~

Launch figwheel from the terminal:

~~~
lein figwheel
~~~

or

~~~
rlwrap lein figwheel
~~~

Open `http://localhost:3449/` in chrome, open console, there is your "Hello world!" log. Now, update `core.cljs`:

~~~ clojure
(ns cljs_compiler.core)

(enable-console-print!)

(defn hello [] (println "Hello world!"))
~~~

And in the terminal, in the figwheel session:

~~~ clojure
(in-ns 'cljs_compiler.core)
(hello)

;; repl => nil
;; chrome console => "Hello world!"
~~~


3- compiler functions
----------

Add to dependencies the `cljs.js` namespace.
Learn more about `cljs.js` [here][cljs.js-url].

~~~ clojure
(ns cljs_compiler.core
  (:require
    [cljs.js :as cljs]))
~~~

You'll need a callback function as util to handle compilation errors.

~~~ clojure
(defn callback [{:keys [value error]}]
  (let [status (if error :error :ok)
        res (if error
              (.. error -cause -message)
              value)]
    [status res]))
~~~

The function `_compilation` will receive a clojurescript source as a string.
Return [:error "reason"] in case of error and [:ok "js-code"] in case of success.

~~~ clojure
(defn _compilation [s]
  (cljs/compile-str 
    (cljs/empty-state) 
    s
    callback))
~~~

The function `_eval` will receive a clojurescript source as a string and evaluate it.
Return [:error "reason"] in case of error and [:ok "js-code"] in case of success.

~~~ clojure
(defn _eval [s]
  (cljs/eval-str 
    (cljs/empty-state) 
    s 
    'test 
    {:eval cljs/js-eval} 
    callback))
~~~

The function `_evaluation-js` return the jsonify `_eval` result.

~~~ clojure
(defn _evaluation-js [s]
  (let [[status res] (_eval s)]
    [status (.stringify js/JSON res nil 4)]))
~~~

The function `_evaluation-clj` return the stringify `_eval` result.

~~~ clojure
(defn _evaluation-clj [s]
  (let [[status res] (_eval s)]
    [status (str res)]))
~~~

In this step you can test the compilation functions using the repl.

~~~ clojure
(in-ns 'cljs_compiler.core)
(_compilation "(+ 1 2"))

;; => [:ok "(2 + 2);\n"]
~~~


4- om.next
----------

[Om.next][om.next-url] is a great client framework based on [React][react.js-url]. It wasn't necessary to use it for this small app but it was a cute opportunity to discover it.

Add om.next dependencies to `project.clj` 

~~~ clojure
{
  ;; ...
  :dependencies [[org.clojure/clojure "1.7.0"]
                 [org.clojure/clojurescript "1.7.170"]
                 [org.omcljs/om "1.0.0-alpha22"]]
  ;; ...
}
~~~

and to `core.cljs`

~~~ clojure
(ns cljs_compiler.core
  (:require
    [cljs.js :as cljs]
    [goog.dom :as gdom]
    [om.next :as om :refer-macros [defui]]
    [om.dom :as dom]))
~~~

You can use an atom to store your local data

~~~ clojure
(defonce app-state (atom
  {:input ""
   :compilation "" 
   :evaluation-js "" 
   :evaluation-clj ""}))
~~~

a read function to read your local data

~~~ clojure
(defn read [{:keys [state]} key params]
  {:value (get @state key "")})
~~~

a mutation multimethod to mutate your local data

~~~ clojure
(defmulti mutate om/dispatch)

(defmethod mutate 'input/save [{:keys [state]} _ {:keys [value]}]
  {:action (fn [] 
            (swap! state assoc :input value))})

(defmethod mutate 'cljs/compile [{:keys [state]} _ {:keys [value]}]
  {:action (fn [] 
            (swap! state update :compilation 
              (partial _compilation value)))})

(defmethod mutate 'js/eval [{:keys [state]} _ {:keys [value]}]
  {:action (fn [] 
            (swap! state update :evaluation-js 
              (partial _evaluation-js value)))})

(defmethod mutate 'clj/eval [{:keys [state]} _ {:keys [value]}]
  {:action (fn [] 
            (swap! state update :evaluation-clj 
              (partial _evaluation-clj value)))})
~~~

a parser to say om.next what is your read/mutate functions

~~~ clojure
(def parser (om/parser {:read read 
                        :mutate mutate}))
~~~

a reconciler to merge the result of your mutations to your local data.

~~~ clojure
(def reconciler 
  (om/reconciler 
    {:state app-state 
     :parser parser}))
~~~

an om transact function, adding for convenience

~~~ clojure
(defn process-input [compiler s]
  (om/transact! compiler 
       [(list 'input/save     {:value s})
        (list 'cljs/compile   {:value s})
        (list 'js/eval        {:value s})
        (list 'clj/eval       {:value s})]))
~~~

and of course test all that using the repl:

~~~ clojure
(in-ns 'cljs_compiler.core)
(process-input reconciler "(+ 2 2)")
(parser {:state app-state} '[:input 
                             :compilation 
                             :evaluation-js 
                             :evaluation-clj])

;; => {:input "(+ 2 2)", 
;;     :compilation [:ok "(2 + 2);\n"], 
;;     :evaluation-js [:ok "4"], 
;;     :evaluation-clj [:ok "4"]}
~~~


OK! now you need UI components:
Build an om component that contains 4 textarea, one for input and one for each compile/eval results.

~~~ clojure
(defui CompilerUI
  
  static om/IQuery
  (query [this] 
    '[:compilation :evaluation-js :evaluation-clj])
  
  Object
  
  (render [this]
    (as->
      (om/props this) $
      (dom/div nil
        (input-ui this)
        (compile-cljs-ui $)
        (evaluate-clj-ui $)
        (evaluate-js-ui $)))))
~~~

and the 4 textareas

~~~clojure
(defn input-ui [reconciler]
  (dom/section nil
    (dom/textarea #js {:autoFocus true
                       :onChange #(process-input 
                                    reconciler
                                    (.. % -target -value))})))

(defn compile-cljs-ui [{:keys [compilation]}]
  (let [[status result] compilation]
    (dom/section nil
                 (dom/textarea #js {:value result
                                    :readOnly true}))))

(defn evaluate-clj-ui [{:keys [evaluation-clj]}]
  (let [[status result] evaluation-clj]
    (dom/section nil
                 (dom/textarea #js {:value result
                                    :readOnly true}))))

(defn evaluate-js-ui [{:keys [evaluation-js]}]
  (let [[status result] evaluation-js]
    (dom/section nil
                 (dom/textarea #js {:value result
                                    :readOnly true}))))
~~~

Go to [http://localhost:3449/][figwheel-url], you have an awesome clojurescript compiler !!!
Now let's go! fork it!

[figwheel-url]: http://localhost:3449/
[klipse-presentation-url]: http://blog.klipse.tech/clojure/2016/03/17/klipse.html
[cljs.js-url]: https://github.com/clojure/clojurescript/blob/master/src/main/cljs/cljs/js.cljs
[om.next-url]: https://github.com/omcljs/om/wiki/Quick-Start-(om.next)
[react.js-url]: https://facebook.github.io/react/

---
layout: post
title:  "Chapter 2: Nested Expressions"
description:  "programming for kids tutorial examples clojure nested expressions"
date:   2016-06-18 21:17:23 +0200
categories: clojure
programming_course: true
hidden: true
thumbnail: assets/klipse.png
guid: "C6CCCB84-BD23-4A61-B5E2-3A209A9EE2AF"
author: "@viebel"
---


In the [previous chapter]({% post_url 2016-01-01-programming-kids-1%}), you have learned to compose **simple** expressions in computer programming language.

In this chapter, you are going to learn how to compose **nested** expressions.

![Russian Dolls](/assets/russian_dolls.jpg)

# Nested Expressions

Nested expressions are like russian dolls: one expression contains an expression that contains an expression that contains an expression that contains...

You are probably asking yourself how could we have an expression that contains an expression?

Well, as an example, Let's compose a **nested** expression that adds `3` and `4` and multiply the result by `5`.

For that, we are going to use again the **3 steps of an expression** that we introduced in [previous chapter]({% post_url 2016-01-01-programming-kids-1%}):

1. First, you need to tell the computer that you want him to execute something. For that you use the parenthesis: `()`. The computer will execute for you the content of the parenthesis.

2. Then, you need to tell him what `operation` you want him to execute: in our case, the operation is the multiplication.

3. Finally, you need to tell him what are the details of the `operation`: the `operands`. In our case, the `operands` are: `(+ 3 4)` and `5`.

Combining all of that, we get:

~~~klipse
(* (+ 3 4) 5)
~~~

Congratulations! This is your first **nested expression**.

Now, try to use more `operands`: for instance, you could type `(* (+ 3 4) 5 6 8)`.

# Exercises

If you are having difficulties with one exercise, read again the details of the **3 steps of an expression**.

A. add 10 to 12 and multiply the result by 3

~~~klipse
()
~~~

B. add 7 to 9 and multiply the result by 5

~~~klipse
()
~~~
C. multiply 7 and 9 and add 6 to the result

~~~klipse
()
~~~

D. multiply 7 and 9 and add 6 and 9 to the result

~~~klipse
()
~~~

E. add 7 to 9 and multiply the result by 5 and 3

~~~klipse
()
~~~

F. add 7 to 9 and multiply the result by `(+ 3 8 9)`

~~~klipse
()
~~~


Here are the solutions:

A. 66

B. 80

C. 69

D. 78

E. 240

F. 320


Send us a screenshot with your programs to [viebel@gmail.com](mailto:viebel@gmail.com?Subject=Chapter%202).

Now, you are ready for [chapter 3]({% post_url 2016-06-19-programming-kids-3 %}).

---
[app-url]: http://app.klipse.tech?blog=klipse

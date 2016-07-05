---
layout: post
title:  "Chapter 3: Giving Names to Expressions"
description:  "programming for kids tutorial examples clojure naming expressions"
date:   2016-06-18 21:17:23 +0200
categories: clojure
programming_course: true
hidden: true
thumbnail: assets/klipse.png
guid: "AB0B0C04-1C84-4597-AB73-1B382D2578C4"
author: "@viebel"
---


In the [previous chapter]({% post_url 2016-06-18-programming-kids-2 %}), you have learned to compose **nested** expressions in computer programming language.

In this chapter, you are going to learn how to name expressions.

But before that, you need to understand why it is important to have the ability to name expressions. One reason is that without naming expressions, computer programming can rapidly become a mess.

When you name expressions, computer programming is more organized.

![Organized](/assets/organized.jpg)

# The problem: Without naming expressions, it's sometimes a mess

Look at this expression

~~~klipse
(+ (* (+ 3 4) 7) (* (+ 3 4) 9) (* (+ 3 4) 5))
~~~

Do you see that the expression `(+ 3 4)` is repeated three times?

This repetition makes the expression very hard to understand.


# The solution

The solution to this mess is give a name to the expression `(+ 3 4)` and to use its name instead of the expression itself.

In order to name name expressions, we use the `def` operation. Like this

~~~klipse
(def a (+ 3 4))
~~~

(For the moment, don't pay attention to `#'cljs.user/a`.)

And using `a` instead of `(+ 3 4)` our expression becomes simpler:


~~~klipse
(+ (* a 7) (* a 9) (* a 5))
~~~

# How to use `def`

`def` is the `operation` that tells the computer to give a `name` to an `expression`. (The word `def` is a shorthand for `define`).

In order to use `def`, you need to compose an expression in the same way that you did with `+` and `*` in the previous chapters: with the **3 steps of an expression** that we introduced in [chapter 1]({% post_url 2016-01-01-programming-kids-1%}):

1. First, you need to tell the computer that you want him to execute something. For that you use the parenthesis: `()`. 

2. Then, you need to tell him what `operation` you want him to execute: in our case, the operation is `def`.

3. Finally, you need to tell him what are the details of the `operands` for `def` operation: the first operand is the `name` you want to give to the expression and the second operand is the `expression` you want to name.

Combining all of that, we get:

~~~klipse
(def a (+ 3 4))
~~~

Can you see the 3 parts of the expression?

1. `()`
2. `def`
3. first operand: `a`, second operand `(+ 3 4)`

Congratulations! This is the first time you gave a name to an **expression**.

Now, try to use different names:  for instance, you could replace `a` by `b`, `c` or your firstname.

You can also replace `(+ 3 4)` by another expression: `(* 6 7)` or `(+ (* 7 2) (* 2 9))`.

# Using names

Once you give a name to an expression, you can use it everywhere.

Here, we define `my-favourite-number` to be `18`:

~~~klipse
(def my-favourite-number 18)
~~~

And now, we add `10` to `my-favourite-number`:

~~~klipse
(+ my-favourite-number 10)
~~~

# Back to the complicated expression

Now, we can re-write the complicated expression we started with: `(+ (* (+ 3 4) 7) (* (+ 3 4) 9) (* (+ 3 4) 5))`:

~~~klipse
(def my-number (+ 3 4))
(+ (* my-number 7) (* my-number 9) (* my-number 5))
~~~

Do you agree that it looks much simpler?

Of course, it gives exactly the same result as the original expression:

~~~klipse
(+ (* (+ 3 4) 7) (* (+ 3 4) 9) (* (+ 3 4) 5))
~~~


# Exercises

If you are having difficulties with one exercise, read again the details of the **3 steps of an expression**.

A. Calculate `(4 + 7 + 8)*3 + (4 + 7 + 8)*7 + (4 + 7 + 8)*9`

~~~klipse
()
~~~

You should get `361`.

B. Calculate `(2*3 + 4)*3 + (2*3 + 4)*7 + (2*3 + 4)*9`

~~~klipse
()
~~~

You should get `190`.


C. Calculate `2*3 + 4*5`. Then multiply the result by `4 + 5`.

~~~klipse
()
~~~

You should get: `234`.

C. Calculate `2*3 + 4*5`. Then multiply the result by `4 + 5`. Then multiply the result by `19`.

~~~klipse
()
~~~


You should get: `4446`.

Send us a screenshot with your programs to [viebel@gmail.com](mailto:viebel@gmail.com?Subject=Chapter%203).


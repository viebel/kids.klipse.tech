---
layout: post
title:  "Chapter 1: What is computer programming?"
description:  "programming for kids tutorial examples clojure"
date:   2016-05-03 18:17:52 +0200
categories: clojure
programming_course: true
hidden: true
thumbnail: assets/klipse.png
guid: "F7EB3559-E509-4AED-AC6F-D902CB3AD184"
author: "@viebel"
---


Computer Programming is to let the computer know what we want it to execute.

Computer understands only a special language. When you want to communicate with your friends, you use human languages like english, french or spanish. When you want to communicate with a computer you use computer languages like ruby, python or clojure. All of them are examples of computer programming languages.


![Calculator](/assets/calc.jpg)


Enough introduction, let's start to do some real programming.

#  The 3 steps of an expression

Let's imagine you don't remember your table of multiplications and you want the computer to calculate `7*8` for you. How are you going to do that?

For that purpose, you will have to master the  **3 steps of an expression**:

1. First, you need to tell the computer that you want him to execute something. For that you use the parenthesis: `()`. The computer will execute for you the content of the parenthesis.

2. Then, you need to tell him what `operation` you want him to execute: in our case, the operation is the multiplication. The symbol for multiplication is: `*`.

3. Finally, you need to tell him what are the details of the `operation`. We call them the `operands`. In our case, the `operands` are `7` and `8`. The operands must be separated by one or more white spaces one from the other and one from the `operation`.

Combining all of that, we get:

~~~klipse
(* 7 8)
~~~


Now, modify the `operands` above and try to replace `7` and `8` by `4` and `5`.

What happened?

Did you get `20`?


Now, try to add more `operands`: for instance you could type `(* 2 3 4 6 8 2 3)`.

Try to add more white spaces between the operands, or between an operand and a parenthesis.

What results do you get?

When you talk to a friend you use sentences. When you talk to a computer, you use `expressions`.

# Exercises

If you are having difficulties with one exercise, read again the details of the **3 steps of an expression**.

A. Write a program that calculates `7*8`

~~~klipse
()
~~~

Did you get `56`?

B. Write a program that calculates `2*3*4*5`

~~~klipse
()
~~~

Did you get `120`?

C. Write a program that calculates `2+3+4+5`

~~~klipse
()
~~~

Did you get `14`?

Here are the solutions in video.


Send us a screenshot with your programs to [viebel@gmail.com](mailto:viebel@gmail.com?Subject=Chapter%201).

Now, you are ready for [chapter 2]({% post_url 2016-06-18-programming-kids-2 %}).

---
[app-url]: http://app.klipse.tech?blog=klipse

---
layout: post
title:  "第四章：对几个表达式求值"
description:  "用 Clojure 教小孩子编程：用列表对几个表达式求值"
date:   2017-06-21 21:17:23 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "35E2D040-CA44-4C9E-8C96-180298922658"
author: "@viebel"
language: cn
---


在 [前一章]({% post_url cn/2016-06-19-programming-kids-3 %}) 里，你学会了给表达式起名字。

目前为止，你学会了如何让计算机求一个表达式的值。如果你在盒子（box）里写了几个表达式，计算机将只会显示最后一个表达式的结果。

例如，在这个盒子里

~~~klipse
(+ 3 4)
(* 7 8)
~~~

计算机只显示计算 `(* 7 8)` 的结果。


在这一章里，你将学会如何让计算机显示几个表达式的结果。用计算机术语来说，这也称为一个表达式的 `列表` 。


![Organized](/assets/images/shopping-list.jpg)

# 新的朋友：列表

为了让计算机求几个表达式的值，你需要使用称为 `列表` 的操作，操作数是你打算求值的表达式。


(You see, even when you tell the computer to evaluate several expressions, you write an expression. It is a bit confusing. Right?) 
（你瞧，尽管你让计算机求几个表达式的值，你还是写了一个表达式。这有点令人困惑，对吗？）

为此，我们需要使用表达式的三个步骤：

1、括号
2、操作的名字： `list`
3、操作数： 要计算的表达式


举个例子，让计算机显示 `(+ 3 4)` 和 `(* 7 8)` 的结果：


~~~klipse
(list
  (+ 3 4)
    (* 7 8))
~~~


现在，尝试显示 `(+ 3 4)` 、 `(* 7 8)` 和 `(+ 9 8)` 的求值结果。



# 列表的列表

现在，你是如何让计算机显示几个列表的呢？

如此地简单：

~~~klipse
(list
  (list
      (+ 3 4)
          (* 7 8))

    (list (+ 6 5)
            (* 8 3)))

~~~


当然，在列表里，你可以混合列表和数字。

像这样：

~~~klipse
(list
  (list
      (+ 3 4)
          (* 7 8))
  
    1 4 9)
~~~



# 练习


A. 显示 `2*3` 、 `2*4` 和 `2*5` 。

~~~klipse
()
~~~


B. 显示数字 1、 2、 3、 4 和 9

~~~klipse
()
~~~

C. 显示你兄弟姐妹（或者朋友们）的年龄


~~~klipse
()
~~~


D. 显示你和你兄弟姐妹（或朋友们）的差值


~~~klipse
()
~~~

E. 显示两个列表：

1、你兄弟姐妹的年龄
2、1 到 10 之间的奇数


~~~klipse
()
~~~



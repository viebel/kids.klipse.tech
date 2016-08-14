---
layout: post
title:  "Chapitre 4: Évaluer plusieurs expressions"
description: "Tutoriel de programmation pour les enfants à propos de l'évaluation d'un ensemble d'expressions via des listes"
date:   2016-08-14 15:28:10 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "35E2D040-CA44-4C9E-8C96-180298922658"
author: "@viebel"
translator: "@ggeoffrey"
---

<!-- --- -->
<!-- layout: post -->
<!-- title:  "Chapter 4: Evaluating Several Expressions" -->
<!-- description:  "programming for kids tutorial examples clojure evaluating several expressions with list" -->
<!-- date:   2016-06-21 21:17:23 +0200 -->
<!-- categories: clojure -->
<!-- thumbnail: assets/klipse.png -->
<!-- guid: "35E2D040-CA44-4C9E-8C96-180298922658" -->
<!-- author: "@viebel" -->
<!-- --- -->


<!-- In the [previous chapter]({% post_url 2016-06-19-programming-kids-3 %}), you have learned to give names to expressions. -->

Dans le [chapitre précédent]({% post_url 2016-06-19-programming-kids-3 %}), tu as appris à donner un nom à des expressions.

<!-- Until now, you have learned how to tell the computer to evaluate one expression. If you write several expression inside a box, the computer will display only the result of the last expression. -->
Jusque maintenant, tu as appris comment dire à l'ordinateur d'évaluer une expression. Si tu écris plusieurs expressions dans une boîte de code, l'ordinateur n'affichera que le résultat de la dernière expression.

<!-- For instance, in this box: -->
Regardes cette boîte :

~~~klipse
(+ 3 4)
(* 7 8)
~~~

<!-- The computer displays only the result of the evaluation of `(* 7 8)`. -->
L'ordinateur n'affiches que le résultat de l'évaluation de l'expression `(* 7 8)`.

<!-- In this chapter, you are going to learn how to tell the computer to display the result of several expressions. In computer language, it is called a `list` of expression. -->
Dans ce chapitre, tu vas apprendre à dire à l'ordinateur d'afficher le résultat de plusieurs expressions. En langage informatique, on appelle ça une **liste** d'expressions, et on l'écrit `list`.

![Organized](/assets/images/shopping-list.jpg)

<!-- # A new friend: list -->

# Un nouvel amis : list

<!-- In order to tell the computer to evaluate several expressions, you need to use the operation named `list` and as the operands the expressions that you want to evaluate. -->
Pour dire à l'ordinateur d'évaluer plusieurs expressions, tu dois utiliser l'opération appelée `list` et écrire les expressions que tu veux évaluer comme opérandes.

<!-- (You see, even when you tell the computer to evaluate several expressions, you write an expression. It is a bit confusing. Right?) -->
(Tu vois, même si tu dis à l'ordinateur d'évaluer plusieurs expressions, tu dois écrire une expression. C'est un petit peu étrange, non ?)

<!-- For that, we need to use the 3 steps of an expression: -->
Pour cela, on utilise les 3 étapes d'une expression :

<!-- 1. parentheses -->
<!-- 2. the name of the operation: `list` -->
<!-- 3. the operands: the expressions that we want to evaluate -->

1. parenthèses
2. le nom de l'opération : `list`
3. les opérandes : les expressions que l'on veut évaluer

<!-- For instance, let's tell the computer to display the evaluation of `(+ 3 4)` and `(* 7 8)`: -->
Par exemple, demandons à l'ordinateur d'afficher l'évaluation de `(+ 3 4)` et de `(* 7 8)` :

~~~klipse
(list
  (+ 3 4)
  (* 7 8))
~~~


<!-- Now, try to display the evaluation of `(+ 3 4)` and `(* 7 8)` and `(+ 9 8)`. -->
Essaies d'afficher l'évaluation de `(+ 3 4)` et de `(* 7 8)` et de `(+ 9 8)`.


<!-- # list of lists -->

# Listes de listes

<!-- Now, how would you tell the computer to display several lists? -->
Comment demanderais-tu à l'ordinateur d'afficher plusieurs listes ?

<!-- Simply like this: -->
Tout simplement comme ça :

~~~klipse
(list
  (list
      (+ 3 4)
      (* 7 8))
  (list
      (+ 6 5)
      (* 8 3)))
~~~


<!-- And of course, inside a list, you can mix lists and numbers. -->
Et bien sûr, tu peux mélanger des listes et des nombres à l'interieur d'une liste.

<!-- Like this: -->

~~~klipse
(list
  (list
      (+ 3 4)
      (* 7 8))
  1 4 9)
~~~

<!-- # Exercises -->
# Exercices

<!-- A. Display `2*3`, `2*4` and `2*5`. -->
A. Affiches `2*3`, `2*4` et `2*5`.

~~~klipse
()
~~~


<!-- B. Display the numbers 1, 2, 3, 4 and 9 -->
B. Affiches les nombres 1, 2, 3, 4 et 9.

~~~klipse
()
~~~

<!-- C. Display the ages of your siblings (or friends) -->
C. Affiches l'age de tes frères et sœurs (ou de tes amis).

~~~klipse
()
~~~


<!-- D. Display the age differences between you and your siblings (or friends) -->
D. Affiches la différence d'age entre toi et des frères et sœurs (ou tes amis)

~~~klipse
()
~~~

<!-- E. Display two lists: -->
E. Affiches deux listes :

<!-- 1. the ages of your siblings -->
<!-- 2. the odd numbers between 1 and 10 -->
1. l'age de tes frères et sœurs (ou de tes amis)
2. les nombres impaires entre 1 et 10

~~~klipse
()
~~~

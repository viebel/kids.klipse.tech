---
layout: post
title:  "Chapitre 3: Donner un nom aux expressions"
description: "Tutoriel de programmation pour les enfants à propos du nomage des expressions en clojure"
date:   2016-08-13 23:06:23 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "AB0B0C04-1C84-4597-AB73-1B382D2578C4"
author: "@viebel"
translator: "@ggeoffrey"
---


<!-- --- -->
<!-- layout: post -->
<!-- title:  "Chapter 3: Giving Names to Expressions" -->
<!-- description:  "programming for kids tutorial examples clojure naming expressions" -->
<!-- date:   2016-06-18 21:17:23 +0200 -->
<!-- categories: clojure -->
<!-- thumbnail: assets/klipse.png -->
<!-- guid: "AB0B0C04-1C84-4597-AB73-1B382D2578C4" -->
<!-- author: "@viebel" -->
<!-- --- -->


<!-- In the [previous chapter]({% post_url 2016-06-18-programming-kids-2 %}), you have learned to compose **nested** expressions in computer programming language. -->
Dans le [chapitre précédent]({% post_url 2016-06-18-programming-kids-2 %}), tu as appris à composer des expressions **imbriquées** dans un langage de programmation.

<!-- In this chapter, you are going to learn how to name expressions. -->
Dans ce chapitre tu vas apprendre à donner un nom à des expressions.

<!-- But before that, you need to understand why it is important to have the ability to name expressions. One reason is that without naming expressions, computer programming can rapidly become a mess. -->
Mais avant ça tu dois comprendre pourquoi nommer des expressions est utile. Sans donner de nom aux expressions, programmer devient très vite compliqué et désorganisé.

<!-- When you name expressions, computer programming is more organized. -->
Quand tu donnes un nom à une expression, la programmation est plus organisée.

![Organized](/assets/images/organized.jpg)

<!-- # The problem: Without naming expressions, it's sometimes a mess -->

# Le problème : sans noms, c'est souvent le bazar

<!-- Look at this expression -->

Regarde cette expression

~~~klipse
(+ (* (+ 3 4) 7) (* (+ 3 4) 9) (* (+ 3 4) 5))
~~~

<!-- Do you see that the expression `(+ 3 4)` is repeated three times? -->
Est-ce que tu vois que l'expression `(+ 3 4)` est écrite trois fois ?

<!-- This repetition makes the expression very hard to understand. -->
Cette répétition rend l'expression très difficile à comprendre.

<!-- # The solution -->

# La solution

<!-- The solution to this mess is give a name to the expression `(+ 3 4)` and to use its name instead of the expression itself. -->
La solution à ce bazar est de donner un nom à l'expression `(+ 3 4)` et d'utiliser ce nom à la place de l'expression elle-même.

<!-- In order to name name expressions, we use the `def` operation. Like this -->
Pour nommer une expression, on utilise l'opération `def`. Comme ça

~~~klipse
(def a (+ 3 4))
~~~

<!-- (For the moment, don't pay attention to `#'cljs.user/a`.) -->
(Pour le moment ne te préoccupes pas de `#'cljs.user/a`.)

<!-- And using `a` instead of `(+ 3 4)` our expression becomes simpler: -->
En utilisant `a` à la place de `(+ 3 4)`, l'expression devient plus simple :

~~~klipse
(+ (* a 7) (* a 9) (* a 5))
~~~

<!-- # How to use `def` -->

# Comment utiliser `def`

<!-- `def` is the `operation` that tells the computer to give a `name` to an `expression`. (The word `def` is a shorthand for `define`). -->
`def` est une `opération` qui dit à l'ordinateur de donner un `nom` à une `expression`. (Le mot `def` est un surnom pour `définir`).

<!-- In order to use `def`, you need to compose an expression in the same way that you did with `+` and `*` in the previous chapters: with the **3 steps of an expression** that we introduced in [chapter 1]({% post_url 2016-01-01-programming-kids-1%}): -->
Pour utiliser `def`, tu dois composer une expression de la même façon que tu l'as fait avec `+` et `*` dans les chapitres précédents : avec les **3 étapes d'une expression** que nous avons vu dans le [chapitre 1]({% post_url 2016-01-01-programming-kids-1%}) :

<!-- 1. First, you need to tell the computer that you want it to execute something. For that you use the parenthesis: `()`. -->
<!-- 2. Then, you need to tell the computer what `operation` you want it to execute: in our case, the operation is `def`. -->
<!-- 3. Finally, you need to tell the computer what are the details of the `operands` for `def` operation: the first operand is the `name` you want to give to the expression and the second operand is the `expression` you want to name. -->

1. D'abord, tu dois dire à l'ordinateur que tu veux qu'il execute quelque chose. Pour cela tu utilises les parenthèses : `()`.
2. Ensuite, tu dois dire à l'ordinateur quelle `opération` il doit executer : dans notre cas, l'opération est `def`.
3. Enfin, tu dois dire à l'ordinateur quels sont les détails (les `opérandes`) de l'opération `def`. La première opérande est le `nom` que tu veux donner à l'expression. La seconde opérande est l'expression que tu veux nommer.

<!-- Combining all of that, we get: -->
En suivant ces rêgles, on obtient :

~~~klipse
(def a (+ 3 4))
~~~

<!-- Can you see the 3 parts of the expression? -->
Est-ce que tu vois les 3 parties de l'expression ?

<!-- 1. `()` -->
<!-- 2. `def` -->
<!-- 3. first operand: `a`, second operand `(+ 3 4)` -->

1. `()`
2. `def`
3. la première opérande `a` et la seconde opérande `(+ 3 4)`

<!-- Congratulations! This is the first time you gave a name to an **expression**. -->
Félicitation ! C'est la première fois que tu donnes un nom à une expression.

<!-- Now, try to use different names:  for instance, you could replace `a` by `b`, `c` or your firstname. -->
Essaies maintenant avec des noms différents : tu peux remplacer `a` par `b`, `c` ou ton prénom par exemple.

<!-- You can also replace `(+ 3 4)` by another expression: `(* 6 7)` or `(+ (* 7 2) (* 2 9))`. -->
Tu peux également remplacer `(+ 3 4)` par une autre expression : `(* 6 7)` ou `(+ (* 7 2) (* 2 9))`.

<!-- # Using names -->

# Utiliser des noms

<!-- Once you give a name to an expression, you can use it everywhere. -->
Une fois que tu as donné un nom à une expression, tu peux l'utiliser partout.


<!-- Here, we define `my-favourite-number` to be `18`: -->
Ici on définit que `mon-nombre-favorit` est `18` :

~~~klipse
(def mon-nombre-favorit 18)
~~~

<!-- And now, we add `10` to `my-favourite-number`: -->
Et maintenant, on ajoute `10` à `mon-nombre-favorit` :

~~~klipse
(+ mon-nombre-favorit 10)
~~~

<!-- # Back to the complicated expression -->

# Retour à l'expression compliquée

<!-- Now, we can re-write the complicated expression we started with: `(+ (* (+ 3 4) 7) (* (+ 3 4) 9) (* (+ 3 4) 5))`: -->
Nous pouvons maintenant réécrire l'expression compliquée avec laquelle nous avons commencer : `(+ (* (+ 3 4) 7) (* (+ 3 4) 9) (* (+ 3 4) 5))` :

~~~klipse
(def mon-nombre (+ 3 4))
(+ (* mon-nombre 7) (* mon-nombre 9) (* mon-nombre 5))
~~~

<!-- Do you agree that it looks much simpler? -->
Ne trouves-tu pas que c'est plus simple ?

<!-- Of course, it gives exactly the same result as the original expression: -->
Bien sûr le résultat est exctement le même que l'expression originale :

~~~klipse
(+ (* (+ 3 4) 7) (* (+ 3 4) 9) (* (+ 3 4) 5))
~~~


# Exercises

<!-- If you are having difficulties with one exercise, read again the details of the **3 steps of an expression**. -->
Si tu as des difficultés avec un exercice, relis les détails des **3 étapes d'une expression**.

<!-- A. Calculate `(4 + 7 + 8)*3 + (4 + 7 + 8)*7 + (4 + 7 + 8)*9` -->
A. Calcules `(4 + 7 + 8)*3 + (4 + 7 + 8)*7 + (4 + 7 + 8)*9`

~~~klipse
()
~~~

<!-- You should get `361`. -->
Tu devrais obtenir `361`.

<!-- B. Calculate `(2*3 + 4)*3 + (2*3 + 4)*7 + (2*3 + 4)*9` -->
B. Calcules `(2*3 + 4)*3 + (2*3 + 4)*7 + (2*3 + 4)*9`

~~~klipse
()
~~~

<!-- You should get `190`. -->
Tu devrais obtenir `190`.

<!-- C. Calculate `2*3 + 4*5`. Then multiply the result by `4 + 5`. -->
C. Calcules `2*3 + 4*5`. Puis multiplies le résultat par `4 + 5`.

~~~klipse
()
~~~

<!-- You should get: `234`. -->
Tu devrais obtenir `234`.


<!-- C. Calculate `2*3 + 4*5`. Then multiply the result by `4 + 5`. Then multiply the result by `19`. -->
C. Calcules `2*3 + 4*5`. Puis multiplies le résultat par `4 + 5`. Puis multiplies le résultat par `19`.

~~~klipse
()
~~~


<!-- You should get: `4446`. -->
Tu devrais obtenir `4446`.

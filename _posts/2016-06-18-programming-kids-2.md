---
layout: post
title: "Chapitre 2: Des expressions dans des expressions dans des expressions"
description: "Tutoriel de programmation pour les enfants à propos des expressions imbriquées en clojure"
date: 2016-08-13 21:17:23 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "C6CCCB84-BD23-4A61-B5E2-3A209A9EE2AF"
author: "@viebel"
---

<!-- --- -->
<!-- layout: post -->
<!-- title:  "Chapter 2: Expressions inside Expressions inside Expressions" -->
<!-- description:  "programming for kids tutorial examples clojure nested expressions" -->
<!-- date:   2016-06-18 21:17:23 +0200 -->
<!-- categories: clojure -->
<!-- thumbnail: assets/klipse.png -->
<!-- guid: "C6CCCB84-BD23-4A61-B5E2-3A209A9EE2AF" -->
<!-- author: "@viebel" -->
<!-- --- -->



<!-- In the [previous chapter]({% post_url 2016-01-01-programming-kids-1%}), you have learned to compose **simple** expressions in computer programming language. -->
Dans le [chapitre précédent]({% post_url 2016-01-01-programming-kids-1 %}), tu as appris à écrire des expressions **simples** dans un langage de programmation.

<!-- In this chapter, you are going to learn how to compose **nested** expressions: expressions inside expressions inside expressions... -->
Dans ce chapitre, tu vas apprendre à écrire des expressions **imbriquées** : des expressions dans des expressions dans des expressions…

![Russian Dolls](/assets/images/russian_dolls.jpg)

<!-- # Nested Expressions -->
# Expressions imbriquées

<!-- Nested expressions are like russian dolls: one expression contains an expression that contains an expression that contains an expression that contains... -->
Les expressions imbriquées ressemblent à des poupées russes : une expression contient une expression qui contient une expression qui contient une expression qui contient…

<!-- You are probably asking yourself how could we have an expression that contains an expression? -->
Tu dois te demander à quoi peut bien servir une expression contenu dans une expression ?

<!-- Well, as an example, Let's compose a **nested** expression that adds `3` and `4` and multiply the result by `5`. -->
Voici un exemple. Composons une expression **imbriquées** qui ajoute `3` et `4` et qui multiplie ce résultat par `5`.

<!-- For that, we are going to use again the **3 steps of an expression** that we introduced in [previous chapter]({% post_url 2016-01-01-programming-kids-1%}): -->
Pour ça nous allons réutiliser les **3 étapes d'une expression** que nous avons appris dans le [chapitre précédent]({% post_url 2016-01-01-programming-kids-1%}):

<!-- 1. First, you need to tell the computer that you want it to execute something. For that you use the parenthesis: `()`. The computer will execute for you the content of the parenthesis. -->
<!-- 2. Then, you need to tell the computer what `operation` you want it to execute: in our case, the operation is the multiplication. -->
<!-- 3. Finally, you need to tell the computer what are the details of the `operation`: the `operands`. In our case, the `operands` are: `(+ 3 4)` and `5`. -->

1. D'abord, tu dois dire à l'ordinateur qu'il doit executer quelque chose. Pour cela tu utilises les parenthèses `()`. L'ordinateur va executer pour toi ce qui se trouve entre les parenthèses.

2. Ensuite, tu dois dire à l'ordinateur quelle `opération` il doit executer : dans notre cas il s'agit de la multiplication.

3. Enfin, tu dois dire à l'ordinateur quels sont les détails de l'`opération` : les `opérandes`. Dans notre cas, les `opérandes` sont `(+ 3 4)` et 5.

<!-- Combining all of that, we get: -->
Si on suit ces trois rêgles, on obtient :

~~~klipse
(* (+ 3 4) 5)
~~~

<!-- Congratulations! This is your first **nested expression**. -->
Félicitation ! C'est ta première **expression imbriquées**.

<!-- Now, try to use more `operands`: for instance, you could type `(* (+ 3 4) 5 6 8)`. -->
Maintenant, essaie d'utiliser plus d'`opérandes` : tu peux écrire `(* (+ 3 4) 5 6 8)` par exemple.

<!-- # Exercises -->
# Exercices

<!-- If you are having difficulties with one exercise, read again the details of the **3 steps of an expression**. -->
Si tu as des difficultés avec un exercice, relis les détails des **3 étapes d'une expression**.

<!-- A. add 10 to 12 and multiply the result by 3 -->
A. ajoute 10 à 12 et multiplie le résultat par 3

~~~klipse
()
~~~

<!-- B. add 7 to 9 and multiply the result by 5 -->
B. ajoute 7 à 9 et multiplie le résultat par 5

~~~klipse
()
~~~

<!-- C. multiply 7 and 9 and add 6 to the result -->
C. multiplie 7 par 9 et ajoute 6 au résultat

~~~klipse
()
~~~

<!-- D. multiply 7 and 9 and add 6 and 9 to the result -->
D. multiplie 7 par 9 et ajoute 6 et 9 au résultat

~~~klipse
()
~~~

<!-- E. add 7 to 9 and multiply the result by 5 and 3 -->
E. ajoute 7 à 9 et multiplie le résultat par 5 et 3

~~~klipse
()
~~~

<!-- F. add 7 to 9 and multiply the result by `(+ 3 8 9)` -->
F. ajoute 7 à 9 et multiplie le résultat par `(+ 3 8 9)`

~~~klipse
()
~~~


<!-- Here are the solutions: -->
Voici les solutions :

A. 66

B. 80

C. 69

D. 78

E. 240

F. 320

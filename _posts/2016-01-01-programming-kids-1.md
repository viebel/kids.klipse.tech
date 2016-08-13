---
layout: post
title:  "Chapitre 1: Qu'est ce que la programmation informatique ?"
description:  "exemple de programmation en clojure pour les enfants"
date:   2016-08-13 18:17:52 +0200
categories: clojure
thumbnail: assets/klipse.png
guid: "F7EB3559-E509-4AED-AC6F-D902CB3AD184"
author: "@viebel"
---

La programmation informatique sert à dire à l'ordinateur ce qu'il doit faire.

Les ordinateurs ne comprennent qu'un langage particulier. Lorsque tu veux parler avec tes amis, tu utilises un langage humain comme le français, l'anglais ou l'espagnol. Lorsque tu veux parler avec un ordinateur, tu dois utiliser un langage informatique comme ruby, python ou clojure. Ce sont des langages de programmation.


![Calculator](/assets/images/calc.jpg)


<!-- Enough introduction, let's start to do some real programming. -->
Assez d'introduction, commençons à programmer.

<!-- #  The 3 steps of an expression -->

# Les 3 étapes d'une expression

<!-- Let's imagine you don't remember your table of multiplications and you want the computer to calculate `7` multiplied by `8` for you. How are you going to do that? -->
Imagine que tu ne te souviennes plus de tes tables de multiplications et que tu souhaites que l'ordinateur calcule pour toi `7` multiplié par `8`. Comment peux-tu lui demander de le faire?

<!-- For that purpose, you will have to master the  **3 steps of an expression**: -->
Pour cela tu dois comprendres les **3 étapes d'une expression**:

<!-- 1. First, you need to tell the computer that you want it to execute something. For that you use the parenthesis: `()`. The computer will execute for you the content of the parenthesis. -->
<!-- 2. Then, you need to tell the computer what `operation` you want it to execute: in our case, the operation is the multiplication. The symbol for multiplication is: `*`. -->

<!-- 3. Finally, you need to tell the computer what are the details of the `operation`. We call them the `operands`. In our case, the `operands` are `7` and `8`. The operands must be separated by one or more white spaces one from the other and one from the `operation`. -->


1. D'abord, tu dois dire à l'ordinateur que tu veux qu'il execute quelque chose. Pour cela tu dois utiliser les parenthèses : `()`. L'ordinateur va éxecuter pour toi ce qui se trouve entre les parenthèses.

2. Ensuite, tu dois dire à l'ordinateur quelle `opération` il doit executer : dans notre cas l'opération est la multiplication. Le symbole de la multiplication est : `*`.

3. Enfin, tu dois dire à l'ordinateur quel sont les détails de l'`opération`. On les appelle `opérandes`. Dans notre cas, les `opérandes` sont `7` et `8`. Les opérandes doivent êtres séparées par au moins une espace de l'opération, et par au moins une espace les uns des autres.

<!-- Combining all of that, we get: -->
Si on suit ces trois rêgles, on obtient :

~~~klipse
(* 7 8)
~~~


<!-- Now, modify the `operands` above and try to replace `7` and `8` by `4` and `5`. -->
Dans cet exemple, essaies de remplacer les opérandes `7` et `8` par `4` et `5`.

<!-- What happened? -->
Que c'est t'il passé ?

<!-- Did you get `20`? -->
As-tu obtenu `20` ?


<!-- Now, try to add more `operands`: for instance you could type `(* 2 3 4 6 8 2 3)`. -->
Maintenant essaies d'ajouter plus d'`opérandes` : tu peux essayer d'écrire `(* 2 3 4 6 8 2 3)` par exemple.

<!-- Try to add more white spaces between the operands, or between an operand and a parenthesis. -->
Essaies aussi d'ajouter plus d'espaces entre les opérandes, ou entre les opérandes et les parenthèses.

<!-- What results do you get? -->
Quel résultat obtiens-tu ?

<!-- When you talk to a friend you use sentences. When you talk to a computer, you use `expressions`. -->
Quand tu parles à un ami tu utilises des phrases. Quand tu parles à un ordinateur, tu utilises des `expressions`.

# Exercices

<!-- If you are having difficulties with one exercise, read again the details of the **3 steps of an expression**. -->
Si tu as des difficultés avec un exercice, relis les détails des **3 étapes d'une expression**.

<!-- A. Write a program that calculates `7*8` -->
A. Écris un programme qui calcule `7*8`

~~~klipse
()
~~~

<!-- Did you get `56`? -->
As-tu obtenu `56` ?

<!-- B. Write a program that calculates `2*3*4*5` -->
B. Écris un programme qui calcule `2*3*4*5`

~~~klipse
()
~~~

<!-- Did you get `120`? -->
As-tu obtenu `120` ?

<!-- C. Write a program that calculates `2+3+4+5` -->
C. Écrit un programme qui calcule `2+3+4+5`

~~~klipse
()
~~~

<!-- Did you get `14`? -->
As-tu obtenu `14` ?

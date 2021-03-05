[Retour au sommaire](./README.md)


## Failles XSS et CSRF

__Note__: j'ai choisi de traiter de ces deux failles au sein d'une même partie car
une XSS s'exploitera très souvent comme on aurait exploité une faille CSRF.

En effet, une faille XSS offre toutes les possibilité d'une CSRF à la difference que si la première est présente, il n'existe absolument aucune mesure permetant d'éviter la seconde.



### Qu'est ce qu'une faille CSRF

Les failles XSS et CSRF sont des vulnerabilité _client side_. Celà signifie qu'à la difference d'une faille qui affecterait le code serveur, une interaction 
de la part d'un utilisateur (qu'on appellera _victime_) est necessaire afin de les mener à bien.

Illustrons le fonctionnement de la failles CSRF par une attaque type.

#### Attaque CSRF type

Soient deux utilisateurs du site **target.com**: appellons les **Hacker** et **Victime**.

**Hacker** possède un compte utilisateur basique, tandis que **Victime** est administrateur. 


**Hacker** souhaite transformer son compte en compte administrateur (techniquement, on parle de **privilege escalation**)

**Victime** s'il le voulait, pourrait techniquement donner à **Hacker** le status d'administrateur par quelques simples clicks dans son interface admin.

Du point de vue des echanges entre le navigateur et le serveur, ça ressemblerait à ça:

![](./assets/CSRF1.png)





### Comment éviter les failles XSS 

#### Comment n'absolument pas éviter les failles XSS


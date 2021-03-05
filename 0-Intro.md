[Retour au sommaire](./README.md)

## Pourquoi les failles de sécurité arrivent ?

On peut essentiellement distinguer trois situations donnant lieu à des vulnerabilités:

* Informations sensible devinable
	* __**Exemple 1**:__ Mauvaise Politique de mot de passe (exemple: certaines application métier, lorsqu'un compte est créée, lui
affectera par défaut le prenom de l'utilisateur en guise de mot de passe.
	* __**Exemple 2**:__ : Page contenant des informations sensible accecible par une url contenant un id sequentiel: http://site/secret?id=123

* Les lois de la physique: par exemple, les rayons cosmiques. Oui, pour de vrai, et juste pour capter votre attention, je n'en dirai pas plus sur ce
sujet pour le moment.

* Une ambiguité d'intepretation d'une donnée (le plus souvent un input utilisateur)


Attardons nous sur ce dernier point.

Pour l'illustrer, imaginons le script PHP suivant:

`pseudo.php`
```php
<?php
//...
$user_infos = sql_query("SELECT pseudo FROM User WHERE id=$_GET['id']");
?>

Le pseudo de l'utilisateur d'id: <strong><?=$_GET['id']?></strong> est <strong><?=$user_infos['pseudo']?></strong>
```

L'utilisation normale, prévue par le développeur est la suivante: un requete du type
`http://site/pseudo.php?id=123`

afficherra
> Le pseudo de l'utilisateur d'id **123** est **BabarDu92**

Le paramètre `GET` `id` attendu est un entier.

Toutefois, rien n'empèche en l'état de lui donner la valeur:
`-1 UNION SELECT password FROM User WHERE pseudo='admin'`

ainsi la requete SQL sera:
```SQL
SELECT pseudo FROM User WHERE id=-1 UNION SELECT password FROM User WHERE pseudo='admin'
```

Qui s'il n'y a pas d'utilisateur d'id `-1` sera fonctionnellement strictement equivalente à 
```SQL
SELECT password FROM User WHERE pseudo='admin'
```

Il s'agit d'une injection SQL (Abrégé SQLi)


Le bout de code 
```html
<strong><?=$_GET['id']?></strong> 
```
est egalement vulnerable, on donnant au parametre `id` la valeur: `<script>alert('coucou')</script>` on pourra executer du code javascript. Il s'agit d'une faille de type Cross Site Scripting (XSS).


Une difference fondamentale entre ces deux types de failles est que pour la seconde, il sera necessaire de faire interagir un utilisateur.



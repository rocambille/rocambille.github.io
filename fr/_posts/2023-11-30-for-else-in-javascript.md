---
ref: 2023-11-30-for-else-in-javascript
lang: fr
title: "For...else en JavaScript"
tags: javascript pattern discuss
authors:
  - romain-guillemot
---

Un pattern courant lors de l'utilisation d'une boucle `for` est de la combiner avec une instruction `else`.<!--more-->
Ceci est possible dans de nombreux moteurs de templates, tels que [Twig](https://twig.symfony.com/doc/3.x/tags/for.html#the-else-clause), comme dans l'exemple ci-dessous :

{% raw %}

```twig
<ul>
  {% for user in users %}
    <li>{{ user.username|e }}</li>
  {% else %}
    <li><em>no user found</em></li>
  {% endfor %}
</ul>
```

{% endraw %}

Pourtant, dans les langages de programmation comme JavaScript, la construction `for...else` n'existe pas :

```js
for (const user of users) {
  console.log(user.username);
} else { // ❌ ça ne marche pas
  console.log("aucun utilisateur trouvé");
}
```

Le mot-clé `else` ne fonctionne qu'en combinaison avec le mot-clé `if`. C'est une limite, mais aussi une solution :

```js
if (users.length > 0) {
  console.log("aucun utilisateur trouvé");
} else {
  for (const user of users) {
    console.log(user.username);
  }
}
```

"Inverser" la logique nous permet d'approcher du pattern `for...else`. Au lieu d'écrire d'abord la boucle puis le `else`, nous vérifions d'abord le cas "aucun utilisateur", pour finir dans la boucle après le `else`. Ici, le bloc `else` ne contient qu'une seule instruction : l'instruction `for`. Nous pouvons supprimer quelques accolades et raccourcir le code :

```js
if (users.length > 0) {
  console.log("aucun utilisateur trouvé");
} else for (const user of users) {
  console.log(user.username);
}
```

C'est ce que j'ai trouvé de plus proche d'un pattern `for...else` en JavaScript. Qu'en penses-vous ? Est-ce que vous voyez une autre approche ?

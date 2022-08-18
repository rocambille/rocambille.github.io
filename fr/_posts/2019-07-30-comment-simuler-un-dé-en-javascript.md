---
ref: 2019-07-30-how-to-roll-a-dice-in-javascript
lang: fr
title: Comment simuler un dé en JavaScript ?
tags: javascript beginners random
authors:
  - romain-guillemot
updated_at: "2022-05-11"
---

![la beauté des dés](/assets/2019-07-30-how-to-roll-a-dice-in-javascript-cover-min.jpeg)

Construisons le dé ultime étape par étape.<!--more-->

## Math.random() comme base

Un dé est un outil fournissant un nombre entier aléatoire à chaque fois que vous le lancez. Quelque chose comme ça :

```js
function rollDice() {
  return /* un nombre généré aléatoirement */;
}
```

Chaque langage de programmation a une fonction aléatoire intégrée. En JavaScript, c'est [`Math.random`](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Math/random) :

```js
function rollDice() {
  return Math.random();
}
```

C'est un bon début : renvoyer un nombre aléatoire. N'oubliez pas que `Math.random` n'est pas assez aléatoire pour des choses sérieuses comme la cryptographie ou les jeux de casino -- regardez [Crypto.getRandomValues](https://developer.mozilla.org/fr/docs/Web/API/Crypto/getRandomValues) si c'est votre domaine. `Math.random` est assez "équilibré" pour lancer un dé entre amis. Essayons:

```js
>> function rollDice() {
     return Math.random();
   }
>> rollDice();
<- 0,7367823644188911
```

Ce `0.7367823644188911` n'est pas vraiment ce que nous voulions... D'après la documentation, `Math.random` renvoie un nombre décimal compris entre 0 (inclus) et 1 (exclu). Pour un dé à 6 faces, nous avons besoin d'un nombre entier de 1 à 6. Naïvement, nous pouvons multiplier par 6 :

```js
>> function rollDice() {
     return Math.random() * 6;
   }
>> rollDice();
<- 4.3380209914241235
```

Nous aurons donc un nombre décimal aléatoire entre 0 (inclus) et 6 (exclu). Il y a du mieux. La prochaine étape serait d'obtenir des valeurs entières :

- Si 0 ≤ `Math.random() * 6` < 1, retourner 1
- Si 1 ≤ `Math.random() * 6` < 2, retourner 2
- Si 2 ≤ `Math.random() * 6` < 3, retourner 3
- Si 3 ≤ `Math.random() * 6` < 4, retourner 4
- Si 4 ≤ `Math.random() * 6` < 5, retourner 5
- Si 5 ≤ `Math.random() * 6` < 6, retourner 6

Cela peut marcher en utilisant [`Math.floor`](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Math/floor). Essayons à nouveau -- avec une boucle `for` pour afficher plusieurs lancers avec `console.log` :

```js
>> function rollDice() {
     return Math.floor(Math.random() * 6);
   }
>> for(let i = 0; i < 5; i++) console.log(rollDice());
   5
   1
   4
   2
   0 // WTF ?
```

Encore une fois, ce n'est pas exactement ce que nous voulions... Ce que nous faisons ici, c'est :

- Si 0 ≤ `Math.floor(Math.random() * 6)` < 1, retourner 0. Pas 1.
- Si 1 ≤ `Math.floor(Math.random() * 6)` < 2, retourner 1. Pas 2.
- Si 2 ≤ `Math.floor(Math.random() * 6)` < 3, retourner 2. Pas 3.
- Si 3 ≤ `Math.floor(Math.random() * 6)` < 4, retourner 3. Pas 4.
- Si 4 ≤ `Math.floor(Math.random() * 6)` < 5, retourner 4. Pas 5.
- Si 5 ≤ `Math.floor(Math.random() * 6)` < 6, retourner 5. Pas 6.

Pour obtenir le résultat voulu avec `Math.floor`, nous devons ajouter 1 avant de retourner :

```js
function rollDice() {
  return 1 + Math.floor(Math.random() * 6);
}
```

Nous avons maintenant une fonction pour simuler notre dé à 6 faces :)

> Oui, mais... Et si nous en voulons un à 4, 8, 12 ou 20 faces ?

Pas de soucis : vous pouvez changer le chiffre magique 6 dans le code pour un paramètre, et y passer la valeur maximale pour votre dé. Quelque chose comme ça :

```js
function rollDice(max) {
  return 1 + Math.floor(Math.random() * max);
}

const rollDice4 = () => rollDice(4);
const rollDice6 = () => rollDice(6);
const rollDice8 = () => rollDice(8);
const rollDice12 = () => rollDice(12);
const rollDice20 = () => rollDice(20);
```

## Le dé ultime

Jadis, j'ai été inspiré par un texte visionnaire : "[The Ultimate Display](https://www.wired.com/2009/09/augmented-reality-the-ultimate-display-by-ivan-sutherland-1965/)" (Ivan E. Sutherland, 1965). Entre autres, pour cette citation :

> Il n'y a aucune raison pour que les objets affichés par un ordinateur suivent les règles ordinaires de la réalité physique qui nous est familière.

Nous avons utilisé un paramètre pour remplacer le nombre de faces de nos dés. Pourquoi ne pas supprimer l'autre nombre magique ? Ce vilain 1 peut devenir un autre paramètre :

```js
function rollDice(min, max) {
  return min + Math.floor(Math.random() * (max - min + 1));
}

const rollDice4 = () => rollDice(1, 4);
const rollDice6 = () => rollDice(1, 6);
const rollDice8 = () => rollDice(1, 8);
const rollDice12 = () => rollDice(1, 12);
const rollDice20 = () => rollDice(1, 20);
const rollSomeUltimateDice = () => rollDice(42, 42);
```

Cette version finale permet de simuler un dé qui ne démarre pas à 1. De plus le `max` permet de simuler un dé équilibré au-delà des "règles ordinaires de la réalité physique". Imaginez un dé à 7 côtés. Vous pouvez imiter votre jeu de dés préféré en suivant ses règles ordinaires. Mais si vous pouvez en imaginer un, lancez un dé qui n'existerait jamais dans la réalité ;)

---
ref: 2024-10-20-a-css-razor
lang: fr
title: "Un rasoir CSS"
tags: css beginners
authors:
  - romain-guillemot
---

![](/assets/2024-10-20-a-css-razor.webp){: width="768"}{: height="453" }

Un "rasoir" en philosophie est un principe méthodologique qui permet de simplifier des choix complexes en éliminant les hypothèses ou options inutiles.

<!--more-->

Le plus connu est le [rasoir d'Occam](https://fr.m.wikipedia.org/wiki/Rasoir_d%27Ockham), qui recommande de ne pas multiplier les entités ou hypothèses au-delà du nécessaire : choisir l'explication la plus simple qui fonctionne.

En appliquant cette idée à CSS, l'idée serait de rationaliser nos choix de propriétés de style pour concevoir des pages de manière simple et efficace, en adoptant des techniques qui résolvent les problèmes de mise en page sans surcomplexité inutile.

Pour appliquer l'idée du rasoir philosophique à CSS, il s'agit de choisir les solutions les plus simples et efficaces pour résoudre des problèmes de mise en page, sans surcharger le code avec des règles inutiles. Voici comment tu peux structurer efficacement tes choix de propriétés CSS, en adoptant une approche progressive pour maintenir la simplicité tout en gérant les exigences de mise en page complexes :

## Prioriser le flux normal

Le **flux normal** est la manière naturelle dont les éléments HTML s'organisent sur la page sans intervention particulière. C'est la base la plus simple et devrait être ton point de départ pour construire une mise en page.

- **Blocs** : Les éléments de type bloc (comme les `<section>`, `<p>`, `<h1>`, etc.) s'empilent verticalement, occupant toute la largeur disponible.
- **Inline** : Les éléments en ligne (comme `<strong>`, `<a>`) se placent les uns à côté des autres, en suivant le flux horizontal du texte.

Commence toujours par voir si la mise en page de base peut être accomplie simplement en jouant avec ces comportements naturels, par exemple :

- Ajuster les dimensions avec `max-width` ou `max-height` pour les conteneurs ou images uniquement et *après* la mise en place du contenu.
- Utiliser des propriétés de typographie (`font-size`, `line-height`, etc.) pour organiser le contenu.

## Passer à Flexbox ou Grid lorsque nécessaire

Lorsque le flux normal ne suffit pas, **Flexbox** et **CSS Grid** sont des outils puissants pour gérer des mises en page plus complexes. Utilise-les de manière réfléchie, en évitant de complexifier inutilement la structure :

- **Flexbox** est idéal pour des mises en page **unidimensionnelles** (une ligne ou une colonne) :
  - Par exemple, pour centrer un élément à la fois horizontalement et verticalement dans un conteneur, `display: flex` et `justify-content: center; align-items: center;` suffisent.
  - Flexbox excelle dans les layouts simples où la relation entre les éléments est linéaire (ex. navigation, cartes alignées, etc.).

- **CSS Grid** est mieux adapté pour des mises en page **bidimensionnelles** (organiser des éléments en lignes et colonnes) :
  - Utilise Grid pour des layouts plus complexes, comme des galeries d'images ou des tableaux de données.
  - Grid est plus puissant que Flexbox pour des agencements où tu as besoin de contrôler à la fois les rangées et les colonnes simultanément.
  - 

L'idée est d’introduire Flexbox ou Grid uniquement quand tu atteins les limites du flux normal, en évitant de les appliquer partout sans besoin réel.

Pour plus de détails, regarde ces excellents guides de Josh Comeau :

* [An Interactive Guide to Flexbox](https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/).
* [An Interactive Guide to CSS Grid](https://www.joshwcomeau.com/css/interactive-guide-to-grid/).

## Gérer les espacements avec `padding` et `margin`

Pour organiser les espaces entre les éléments, il est essentiel de comprendre les différences entre `padding` et `margin` et d'appliquer ces propriétés de manière méthodique :

- **Padding** : Gère l’espace **à l’intérieur** de l'élément, entre son contenu et sa bordure. Utilise `padding` pour ajouter de l'espace entre le contenu interne et le bord du conteneur, comme dans un bouton ou une carte.
  
- **Margin** : Gère l’espace **à l’extérieur** de l'élément, entre la bordure de l'élément et les autres éléments autour. Utilise `margin` pour espacer des éléments les uns des autres dans le flux.

En général, `padding` est pour l’espace **interne**, et `margin` pour l’espace **externe**. Il est souvent plus clair d'utiliser `margin` pour contrôler les espacements entre des éléments indépendants, et de réserver `padding` pour ajuster l'espace à l’intérieur des éléments conteneurs.

La preuve en images dans cet article de Nanthan Curtis : [Space in Design Systems](https://medium.com/eightshapes-llc/space-in-design-systems-188bcbae0d62).

## Utiliser les valeurs de `position` pour la superposition

Le positionnement en CSS permet de créer des mises en page plus dynamiques, mais il faut éviter de les utiliser de manière excessive. Voici comment choisir entre les différentes valeurs de `position` :

- **`position: static`** (par défaut) : Les éléments sont positionnés en fonction du flux normal.
  
- **`position: relative`** : L'élément reste dans le flux normal, mais peut être décalé par rapport à sa position d'origine. Utilise-le quand tu veux déplacer légèrement un élément sans modifier le flux des autres éléments.
  
- **`position: absolute`** : L'élément est retiré du flux normal et positionné par rapport à son premier conteneur parent positionné (ayant une `position: relative`, `absolute` ou `fixed`). Il est utile pour superposer des éléments ou positionner précisément un élément dans un conteneur sans influencer les autres éléments.
  
- **`position: fixed`** : Similaire à `absolute`, mais l'élément est positionné par rapport à la fenêtre du navigateur et reste fixe lors du défilement de la page (ex. barres de navigation fixes, pop-ups).

- **`position: sticky`** : Un compromis entre `relative` et `fixed`, il permet à un élément de rester dans le flux jusqu'à ce qu'une certaine condition soit atteinte (ex. lorsque l'élément atteint un certain point dans le défilement, il devient fixe). C'est utile pour des éléments comme des barres de navigation qui doivent rester visibles après un certain défilement.

Utilise le positionnement de manière judicieuse pour des cas spécifiques où le flux normal et Flexbox/Grid ne peuvent pas répondre aux exigences.

Un exemple concret : le [sticky footer solved by flexbox](https://philipwalton.github.io/solved-by-flexbox/demos/sticky-footer/).

## Choisir les tailles appropriées pour une mise en page fluide et responsive

Pour garantir que la mise en page reste fluide et responsive, utilise des unités flexibles comme :

- **`%`** : Les tailles en pourcentage sont relatives à la taille du conteneur parent et permettent aux éléments de s'adapter à différentes tailles d'écran.
- **`em` et `rem`** : Ces unités sont relatives à la taille de la police parent (ou de l'élément racine pour `rem`). Elles sont idéales pour créer des tailles adaptatives, surtout pour les espacements (`margin`, `padding`) et les dimensions autres que `100%` (`width`, `height`).
- **`vw` et `vh`** : Ces unités sont relatives à la taille de la fenêtre du navigateur (1 `vw` = 1% de la largeur de la fenêtre, 1 `vh` = 1% de la hauteur). Utilise-les pour des layouts adaptables à la taille de l’écran entier.

Évite de fixer les dimensions avec des unités fixes comme `px`, sauf pour des éléments où c’est absolument nécessaire, afin d'assurer que le design reste fluide sur différents appareils.

Un excellent cas d'usage : la [typographie fluide](https://www.smashingmagazine.com/2022/01/modern-fluid-typography-css-clamp/).

## Mon rasoir CSS en bref

1. **Commencer avec le flux normal** : Crée une base de mise en page avec des éléments bloc et inline, en jouant simplement avec `display`, `width`, `height`.
2. **Utiliser Flexbox ou Grid avec modération** : Si le flux normal est insuffisant, passe à Flexbox pour des dispositions unidimensionnelles ou à Grid pour des dispositions bidimensionnelles plus complexes.
3. **Gérer les espacements intelligemment** : Utilise `margin` pour espacer les éléments entre eux et `padding` pour gérer l’espace à l’intérieur des conteneurs.
4. **Positionner les éléments au besoin** : Utilise `relative` pour des ajustements mineurs, `absolute` ou `fixed` pour des éléments hors du flux normal, et garde `static` pour tout le reste.
5. **Prioriser les unités fluides** : Utilise des unités relatives comme `%`, `em`, `rem`, `vw` et `vh` pour garantir un layout fluide et adaptable.

En suivant cette approche méthodologique et en simplifiant au maximum, tu pourras concevoir des pages efficaces sans tomber dans les pièges de la surcomplexité, tout en assurant la maintenabilité du code.

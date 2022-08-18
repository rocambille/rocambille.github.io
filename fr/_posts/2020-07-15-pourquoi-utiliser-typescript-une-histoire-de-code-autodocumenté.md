---
ref: 2020-07-15-why-you-should-use-typescript-a-tale-of-self-documented-code
lang: fr
title: "Pourquoi utiliser TypeScript : une histoire de code autodocumenté"
tags: typescript javascript react
authors:
  - romain-guillemot
updated_at: "2022-05-14"
---

![de l'importance de l'autodocumentation](/assets/2020-07-15-why-you-should-use-typescript-a-tale-of-self-documented-code-cover-min.jpeg)

Faites de votre code le meilleur commentaire possible.<!--more-->

## Le tableau mystère

Commençons par une simple ligne de JavaScript :

```ts
const a = [];
```

Eh bien... c'est une déclaration de tableau : rien de bien impressionnant. À quoi va servir ce tableau ?

Difficile à dire sans contexte, et le nom de la variable n'aide pas. Voici le contexte : je travaille sur un projet personnel utilisant React et React Router (v6). Voici le vrai code, avec un vrai nom de variable, dans un fichier nommé `routes.js` :

```ts
const routes = [
  { path: "/foo", element: <Foo /> },
  { path: "/bar", element: <Bar /> },
];

export default routes;
```

Et voilà : le tableau va lister les routes de l'application. Ensuite, vous pouvez l'utiliser :

```jsx
import { useRoutes } from "react-router";
import routes from "./routes";

export default function App() {
  return useRoutes(routes);
}
```

Bien sûr, je n'ai pas inventé ce code. J'ai vu un code similaire et je voulais en faire un [modèle de dépôt](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-template-repository) sur GitHub. Une sorte de framework React personnel. Dans ce contexte, le tableau doit être vide dans `routes.js`. En effet le modèle ne peut déclarer aucune route : je ne connais pas à l'avance l'application que je vais construire avec mon framework.

```ts
const routes = [];

export default routes;
```

Puisque c'est mon propre code, je me souviendrai comment il doit être rempli ... quelques temps du moins.

## La loi d'Eagleson

Vous l'avez peut-être déjç entendu :

> "Tout code de votre cru que vous n'avez pas regardé depuis six mois ou plus pourrait tout aussi bien avoir été écrit par quelqu'un d'autre."

Bien sûr, je serais reconnaissant envers moi-même 6 mois plus tard &mdash; ou la semaine prochaine &mdash; si j'ajoute un commentaire:

```ts
/* format de route : {path: '/foo', element: <Foo />} */
const routes = [];

export default routes;
```

Cela peut sembler utile à première vue, mais en y regardant de plus près, ce n'est pas le cas. Vraiment pas. Tout d'abord, je ne peux m'attendre à ce qu'une personne &mdash; pas même moi &mdash; comprenne ce commentaire. Il décrit quelque chose appelé un "format de route". Il n'explique pas quoi faire avec le tableau `routes`. En fait, cela n'est utile que si vous savez _déjà_ quoi faire. Assez bon pour un rappel, mais pas ce que vous attendriez d'une documentation.

Et il y a un autre problème. Est-ce que vous lisez toujours les commentaires dans le code ? Moi, non. Des heures de lecture de code ont entraîné mes yeux et mon cerveau à les ignorer autant que possible. J'ai l'habitude de voir les commentaires comme un décor entre 2 lignes de code.

Voyons le verre à moitié plein. Nous avons maintenant une liste de nos besoins pour rédiger une documentation utile :

- Elle doit être exprimée sous une forme explicite et non ambiguë.
- Elle doit s'assurer qu'elle sera lue.

L'expression "explicite et non ambiguë" ne ressemble pas à une définition de l'écriture, mais du codage. Et vous ne pouvez pas être sûr qu'un humain lira ce que vous avez écrit. Mais si vous le demandez à un programme, il "lira" toujours votre code. Alors pourquoi ne pas coder la documentation ? Une version codée de ce commentaire :

```ts
/* format de route : {path: '/foo', element: <Foo />} */
```

## Quand le meilleur commentaire est le code lui-même

C'est là que [TypeScript](https://www.typescriptlang.org/) peut nous aider. En résumé, vous pouvez utiliser TypeScript pour décrire le type de valeur attendu pour une variable :

```ts
const anyValue = "42"; // JS standard : ne se plaindra jamais
const anyString: string = "42"; // ok : '42' est une chaîne de cractères
const anyNumber: number = "42"; // non : '42' n'est pas un nombre
```

Voilà pour les types primitifs. Si vous avez besoin de vous assurer qu'un objet possède des propriétés d'un type spécifique, vous pouvez définir [une interface ou un alias de type](https://www.typescriptlang.org/docs/handbook/2/objects.html) :

```ts
interface MyInterface {
  anyString: string;
  anyNumber: number;
}

const myObject: MyInterface = {
  anyString: "42",
  anyNumber: "42", // encore une fois, mauvais type !!!
};
```

Et c'est exactement ce dont nous avons besoin. Au lieu d'un commentaire à l'utilité incertaine, nous pouvons "typer" le tableau. Cela décrira son futur contenu:

```ts
import { ReactNode } from "react";

interface MyRoute {
  path: string;
  element: ReactNode;
}

const routes: MyRoute[] = [];

export default routes;
```

TypeScript ne peut pas mal comprendre ce code, et il n'oubliera jamais de le lire. Vous pouvez essayer avec un objet invalide :

```ts
import { ReactNode } from "react";

interface MyRoute {
  path: string;
  element: ReactNode;
}

const routes: MyRoute[] = [{ whenToRender: "/foo", whatToRender: <Foo /> }];

export default routes;
```

Cela affichera quelque chose comme ça :

```ts
Type '{ whenToRender: string; whatToRender: any; }' is not assignable to type 'MyRoute'
Object literal may only specify known properties, and 'whenToRender' does not exist in type 'MyRoute'
```

Ainsi, TypeScript peut vous aider à autodocumenter votre code ;)

---
ref: 2025-05-05-how-starter-solves-the-express-vite-ssr-puzzle
lang: fr
title: "Comment StartER résout le casse-tête Express + Vite avec SSR"
tags: javascript express react intermediate starter
authors:
  - romain-guillemot
---

![ajouter une pièce au puzzle](/assets/2025-05-05-how-starter-solves-the-express-vite-ssr-puzzle.webp){: width="1280"}{: height="855" }

Avez-vous déjà essayé de combiner Express et Vite pour le rendu côté serveur dans une application React ?<!--more-->

Si oui, vous avez probablement rencontré le défi de gérer deux serveurs distincts et de les faire fonctionner ensemble de manière fluide.

Dans cet article, je vous explique comment [StartER](https://github.com/rocambille/start-express-react) résout ce problème grâce à une approche « serveur unique » qui non seulement simplifie le développement, mais offre également de puissantes fonctionnalités de rendu côté serveur (SSR).

## Le problème : Deux serveurs, une seule application

Lors du développement d'une application web moderne avec React et Express, vous disposez généralement de :

- Un serveur de développement Vite gérant le remplacement de modules à chaud (HMR) et les fonctionnalités spécifiques à React
- Un serveur back-end Express gérant les routes des API et la logique côté serveur

Gérer ces serveurs comme des serveurs distincts crée une complexité inutile :
- Des configurations de proxy sont nécessaires
- La communication entre les serveurs devient complexe
- Le développement local nécessite l'exécution de plusieurs processus
- Le déploiement devient plus complexe

## La solution de StartER : Un seul serveur pour tout gérer

StartER adopte une approche différente en **attachant un serveur de développement Vite au serveur d'applications Express**, créant ainsi un serveur unique et unifié. Cette architecture offre plusieurs avantages :

- Un workflow de développement simplifié
- Une structure de base de code plus claire
- La suppression des configurations de proxy complexes
- Un déploiement simplifié

Mais quel est l'avantage le plus important ? **Il permet le rendu côté serveur dès sa sortie.**

## Comment StartER implémente le SSR avec Express et Vite

La clé de l'implémentation du SSR dans StartER réside dans l'intégration du mode middleware de Vite avec Express, tout en exploitant les capacités de rendu statique de React Router.

### La structure principale

```
.
├── index.html
├── server.ts             # Serveur d'application principal
└── src
    ├── entry-client.tsx  # Monter l'application sur un élément DOM
    ├── entry-server.tsx  # Restituer l'application à l'aide de l'API SSR de Vite
    └── react
        └── routes.tsx    # Point d'entrée pour le code React (client/serveur indépendant)
```

Cette structure sépare les préoccupations tout en maintenant une architecture applicative cohérente.

### Configuration du serveur de développement

La magie commence dans `server.ts` où nous configurons Vite en mode middleware :

```typescript
import express from "express";
import { createServer as createViteServer } from "vite";

const app = express();
const isProduction = process.env.NODE_ENV === "production";

if (!isProduction) {
  // Créer un serveur Vite en mode middleware
  const vite = await createViteServer({
    server: { middlewareMode: true },
    appType: "custom",
  });

  // Utiliser l'instance connect de Vite comme middleware
  app.use(vite.middlewares);

  app.use(/(.*)/, async (req, res, next) => {
    // Gérer le rendu SSR
    // (Nous verrons cela plus loin)
  });
}
```

En utilisant `middlewareMode: true` et `appType: "custom"`, nous indiquons à Vite de laisser notre serveur Express prendre le contrôle tout en conservant toutes ses fonctionnalités de développement.

### Implémentation du rendu côté serveur

La véritable magie du SSR réside dans le gestionnaire de route fourre-tout et le fichier `entry-server.tsx`. StartER utilise les gestionnaires statiques de React Router et `renderToPipeableStream` pour un rendu efficace avec la prise en charge de Suspense :

```typescript
app.use(/(.*)/, async (req, res, next) => {
  const url = req.originalUrl;
  const indexHtml = fs.readFileSync("index.html", "utf-8");

  // Transformation du code HTML avec les plugins Vite
  const template = await vite.transformIndexHtml(url, indexHtml);

  // Chargement du module d'entrée du serveur
  const { render } = await vite.ssrLoadModule("/src/entry-server");

  // Rendu du code HTML de l'application
  await render(template, req, res);
});
```

Et dans `entry-server.tsx` :

```typescript
import { Transform } from "node:stream";
import { renderToPipeableStream } from "react-dom/server";
import { createStaticHandler, createStaticRouter, StaticRouterProvider } from "react-router";

import routes from "./react/routes";

const { query, dataRoutes } = createStaticHandler(routes);

export const render = async (template, req, res) => {
  // Obtenir le contexte de routage
  const context = await query(
    new Request(`${req.protocol}://${req.get("host")}${req.originalUrl}`)
  );
  
  // Créer un routeur statique pour le SSR
  const router = createStaticRouter(dataRoutes, context);
  
  // Diffuser le contenu rendu
  const { pipe } = renderToPipeableStream(
    <StaticRouterProvider router={router} context={context} />
  );

  // Répondre avec du HTML diffusé
  res.status(200).set("Content-Type", "text/html; charset=utf-8");
  
  const [htmlStart, htmlEnd] = template.split("<!--ssr-outlet-->");
  res.write(htmlStart);
  
  const transformStream = new Transform({
    transform(chunk, encoding, callback) {
      res.write(chunk, encoding);
      callback();
    },
  });
  
  pipe(transformStream);
  transformStream.on("finish", () => {
    res.end(htmlEnd);
  });
};
```

Cette approche de streaming présente plusieurs avantages :
- Prise en charge de `<Suspense>` de React sur le serveur
- Temps de réponse au premier octet (TTFB) plus rapide
- Rendu progressif du contenu
- Meilleure expérience utilisateur, notamment sur les connexions lentes

## Déploiement en production

En production, StartER utilise un processus de build en deux parties :

```json
{
  "scripts": {
    "build:client": "vite build --outDir dist/client",
    "build:server": "vite build --outDir dist/server --ssr src/entry-server"
  }
}
```

Le code serveur charge ensuite le bundle SSR pré-compilé en mode production au lieu d'utiliser le module de développement `ssrLoadModule` de Vite.

## Pourquoi choisir StartER pour votre prochain projet ?

StartER est bien plus qu'une simple implémentation SSR : c'est un framework d'application web complet, conçu à des fins éducatives, sans compromis sur la puissance ni la flexibilité.

### Principaux avantages :

- **Orientation pédagogique** : Idéal pour les débutants et ceux qui se perfectionnent
- **Développement fluide** : Une approche serveur unique simplifie les choses
- **Pile moderne** : React + Express + Vite + SSR
- **Prêt pour la production** : Processus de build optimisé pour le déploiement
- **Bonnes pratiques** : Combine les meilleurs packages de l'écosystème JavaScript

Que vous appreniez le développement web ou que vous développiez un prototype d'application, StartER vous offre une base solide pour vous concentrer sur l'essentiel : le développement des fonctionnalités de votre application.

## Démarrer avec StartER

Prêt à essayer cette approche « serveur unique » pour votre prochain projet React + Express ?

Découvrez [StartER sur GitHub](https://github.com/rocambille/start-express-react) et attribuez-lui une étoile si vous le trouvez utile !

Les détails complets de l'implémentation de la fonctionnalité SSR sont disponibles dans :
- [`server.ts`](https://github.com/rocambille/start-express-react/blob/main/server.ts)
- [`entry-server.tsx`](https://github.com/rocambille/start-express-react/blob/main/src/entry-server.tsx)
- [`entry-client.tsx`](https://github.com/rocambille/start-express-react/blob/main/src/entry-client.tsx)

À quels défis de développement web êtes-vous confrontés et que StartER pourrait vous aider à résoudre ? N'hésitez [ouvrir une discussion](https://github.com/rocambille/start-express-react/discussions) pour me le faire savoir !

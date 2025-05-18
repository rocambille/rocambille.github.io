---
ref: 2025-05-19-starter-directory-structure-freedom-with-a-framework
lang: fr
title: "La structure des répertoires dans StartER : la liberté avec un framework"
tags: javascript express react intermediate starter
authors:
  - romain-guillemot
---

![freedom](/assets/2025-05-19-starter-directory-structure-freedom-with-a-framework.webp){: width="1280"}{: height="853" }

Dans le monde des frameworks web, il existe souvent un compromis entre structure et flexibilité. Trop de structure peut paraître contraignant, tandis qu'une trop grande flexibilité peut submerger les développeurs.<!--more-->

Avec [StartER](https://github.com/rocambille/starter), nous pensons qu'il est important de trouver le juste milieu : fournir des valeurs par défaut judicieuses tout en vous laissant une liberté totale d'adaptation selon vos besoins. Aujourd'hui, je souhaite vous présenter l'un des aspects clés de StartER : sa structure de répertoires.

## La philosophie : vous avez le contrôle

La philosophie de base de StartER est simple :

> StartER n'impose aucune contrainte quant à l'emplacement d'un élément, à condition que vous adaptiez le code à votre organisation : **Vous avez le contrôle !**

Alors que de nombreux frameworks imposent des conventions rigides, StartER offre un point de départ solide que vous êtes libre de réorganiser selon vos préférences ou les exigences de votre projet.

## Structure de répertoire par défaut

Voici à quoi ressemble la structure par défaut d'une application StartER :

```
.
├── .env
├── .env.sample
├── compose.yaml
├── compose.prod.yaml
├── Dockerfile
├── index.html
├── server.ts
└── src
    ├── database
    │   └── schema.sql
    ├── express
    │   ├── routes.ts
    │   └── modules
    │       └── ...
    ├── react
    │   ├── routes.tsx
    │   ├── components
    │   │   └── ...
    │   └── pages
    │       └── ...
    └── types
        └── index.d.ts
```

Décomposons cette structure pour mieux comprendre comment StartER organise votre application.

## Le répertoire racine

Le répertoire racine contient plusieurs fichiers de configuration nécessaires au fonctionnement de votre application :

### Variables d'environnement

StartER utilise des fichiers `.env` pour séparer vos informations sensibles de votre code :

- `.env.sample` : un modèle qui définit les variables d'environnement attendues
- `.env` : votre fichier d'environnement réel avec les valeurs utilisées par votre application (créé en copiant `.env.sample`)

Par défaut, les variables requises incluent :
- `APP_SECRET` : pour les signatures d'authentification
- `MYSQL_ROOT_PASSWORD` : pour l'accès à la base de données
- `MYSQL_DATABASE` : le nom de la base de données de votre application

Cette séparation de la configuration et du code est une bonne pratique qui simplifie considérablement le déploiement dans différents environnements.

### Configuration Docker

StartER exploite Docker pour la conteneurisation, vous offrant un environnement de développement et de production cohérent :

- `Dockerfile` : définit le conteneur de votre application
- `compose.yaml` : configure un environnement de développement avec trois conteneurs :
  - `server` : votre application
  - `database` : serveur MySQL
  - `adminer` : interface de gestion de base de données
- `compose.prod.yaml` : configuration spécifique à la production

Cette approche conteneurisée vous permet d'exécuter votre application avec un simple `docker compose up --build`, éliminant ainsi le syndrome du « ça marche sur ma machine ».

### Points d'entrée de l'application

- `index.html` : référence le point d'entrée client
- `server.ts` : connecte votre application Express à un serveur Vite

## Le répertoire source

Le répertoire `src` contient la majeure partie du code de votre application, divisé en sections logiques :

### Database

Le répertoire `database` contient votre schéma et vos connexions client. StartER simplifie l'intégration de la base de données tout en vous offrant la flexibilité de structurer vos données selon vos besoins.

### Express

Le backend de votre application se trouve dans le répertoire `express`, avec :
- `routes.ts` : définition des points de terminaison de votre API
- `modules` : organisation de la logique métier

### React

Le frontend se trouve dans le répertoire `react`, avec :
- `routes.tsx` : définition des routes de vos pages
- `components` : éléments d'interface utilisateur réutilisables
- `pages` : composants de page complète

### Types

Le répertoire `types` contient les définitions TypeScript partagées dans toute votre application, garantissant ainsi la sécurité des types dans votre projet full-stack.

## Pourquoi cette structure est efficace

La structure de répertoires de StartER est efficace car elle :

1. **Sépare les préoccupations** : les codes back-end, front-end et base de données sont clairement délimités
2. **Favorise l'organisation** : regroupement logique des fichiers connexes
3. **Reste flexible** : s'adapte à la croissance de votre projet
4. **Rationalise le développement** : chaque chose a sa place, mais vous pouvez la modifier

## Idéal pour l'éducation

En tant que framework conçu à des fins éducatives, la structure claire de StartER aide les débutants à comprendre l'architecture des applications web sans se perdre dans la complexité. Chaque section a un objectif clair, facilitant la compréhension du rôle des différents composants d'une application full-stack.

Si vous recherchez un framework qui vous offre des bases solides tout en respectant votre liberté de développeur, StartER pourrait être la solution idéale. Que ce soit pour :

- Des projets éducatifs
- Du prototypage d'applications web modernes et full-stack
- L'apprentissage de React et Express dans un environnement intégré
- La création de projets évolutifs

Découvrez [StartER sur GitHub](https://github.com/rocambille/start-express-react) et attribuez-lui une étoile si vous le trouvez utile !

Et si vous avez manqué mon précédent article expliquant comment StartER résout le casse-tête Express/Vite/SSR, [lisez-le ici](https://rocambille.github.io/fr/2025/05/05/how-starter-solves-the-express-vite-ssr-puzzle/).

Quels aspects des structures de répertoires des frameworks web trouvez-vous les plus utiles ? N'hésitez [ouvrir une discussion](https://github.com/rocambille/start-express-react/discussions) pour me le faire savoir !

---
ref: 2025-05-19-starter-directory-structure-freedom-with-a-framework
lang: en
title: "StartER Directory Structure: Freedom with a Framework"
tags: javascript express react intermediate starter
authors:
  - romain-guillemot
---

![freedom](/assets/2025-05-19-starter-directory-structure-freedom-with-a-framework.webp){: width="1280"}{: height="853" }

In the world of web frameworks, there's often a trade-off between structure and flexibility. Too much structure can feel restrictive, while too much flexibility can leave developers overwhelmed by choices.<!--more-->

With [StartER](https://github.com/rocambille/starter), we believe in finding the sweet spot: providing sensible defaults while giving you complete freedom to adapt as needed. Today, I'd like to walk you through one of the key aspects of StartER: its directory structure.

## The Philosophy: Control is Yours

StartER's core philosophy is simple:

> StartER imposes no constraints on the location of an element, as long as you adapt the code to your organization: **you have control!**

While many frameworks force you into rigid conventions, StartER offers a solid starting point that you're free to reorganize according to your preferences or project requirements.

## Default Directory Structure

Here's what the default structure of a StartER application looks like:

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

Let's break down the key components of this structure to better understand how StartER organizes your application.

## The Root Directory

The root directory contains several configuration files that make your application work:

### Environment Variables

StartER uses `.env` files to keep your sensitive information separate from your code:

- `.env.sample` - A template that defines the current environment variables
- `.env` - Your actual environment file with real values (created by copying `.env.sample`)

Required variables include:
- `APP_SECRET` - For authentication signatures
- `MYSQL_ROOT_PASSWORD` - For database access
- `MYSQL_DATABASE` - Your application's database name

This separation of configuration from code is a best practice that makes deployment across different environments much easier.

### Docker Configuration

StartER leverages Docker for containerization, giving you a consistent development and production environment:

- `Dockerfile` - Defines your application container
- `compose.yaml` - Sets up a development environment with three containers:
  - `server` - Your application
  - `database` - MySQL server
  - `adminer` - Database management interface
- `compose.prod.yaml` - Production-specific configuration

This containerized approach means you can run your application with a simple `docker compose up --build`, eliminating the "it works on my machine" syndrome.

### Application Entry Points

- `index.html` - References client entry point and includes Pico CSS for styling
- `server.ts` - Connects your Express application to a Vite server

## The Source Directory

The `src` directory is where most of your application code lives, divided into logical sections:

### Database

The `database` directory contains your schema and client connections. StartER makes database integration straightforward while giving you the flexibility to structure your data as needed.

### Express

The backend of your application lives in the `express` directory, with:
- `routes.ts` - Defining your API endpoints
- `modules` directory - For organizing business logic

### React

The frontend is contained in the `react` directory, with:
- `routes.tsx` - Defining your page routes
- `components` - Reusable UI elements
- `pages` - Full page components

### Types

The `types` directory contains TypeScript definitions shared throughout your application, ensuring type safety across your full-stack project.

## Why This Structure Works

StartER's directory structure works because it:

1. **Separates concerns** - Backend, frontend, and database code have clear boundaries
2. **Promotes organization** - Logical grouping of related files
3. **Remains flexible** - Can be adapted as your project grows
4. **Streamlines development** - Everything has a place, but you can change that place

## Perfect for Education

As a framework designed with educational purposes in mind, StartER's clear structure helps newcomers understand web application architecture without getting lost in complexity. Each section has a clear purpose, making it easier to grasp the role of different components in a full-stack application.

If you're looking for a framework that gives you a solid foundation while respecting your freedom as a developer, StartER might be just what you need. It's perfect for:

- Educational projects
- Prototyping modern, full-stack web applications
- Learning React and Express in an integrated environment
- Building projects that can grow with your skills

Check out [StartER on GitHub](https://github.com/rocambille/start-express-react) and give it a star if you find it useful!

And if you missed my previous article about how StartER solves the Express/Vite/SSR puzzle, [read it here](https://rocambille.github.io/en/2025/05/05/how-starter-solves-the-express-vite-ssr-puzzle/).

What aspects of web framework directory structures do you find most helpful? [Open a discussion](https://github.com/rocambille/start-express-react/discussions) to let me know!

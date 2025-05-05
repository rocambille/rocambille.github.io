---
ref: 2025-05-05-how-starter-solves-the-express-vite-ssr-puzzle
lang: en
title: "How StartER Solves the Express + Vite SSR Puzzle"
tags: javascript express react intermediate starter
authors:
  - romain-guillemot
---

![one more piece](/assets/2025-05-05-how-starter-solves-the-express-vite-ssr-puzzle.webp){: width="1280"}{: height="855" }

Have you ever tried to combine Express and Vite for server-side rendering in a React application?<!--more-->

If so, you've probably encountered the challenge of managing two separate servers and making them work together smoothly.

In this post, I'll walk you through how [StartER](https://github.com/rocambille/start-express-react) solves this problem with a "one server" approach that not only simplifies development but also enables powerful Server-Side Rendering (SSR) capabilities.

## The Problem: Two Servers, One Application

When building a modern web application with React and Express, you typically have:

- A Vite development server handling hot module replacement (HMR) and React-specific features
- An Express backend server managing API routes and server-side logic

Managing these as separate servers creates unnecessary complexity:
- You need proxy configurations
- Communication between servers becomes cumbersome
- Local development requires running multiple processes
- Deployment gets more complicated

## StartER's Solution: One Server to Rule Them All

StartER takes a different approach by **attaching a Vite development server to the Express application server**, creating a single unified server. This architecture brings several benefits:

- Simplified development workflow
- Cleaner codebase structure
- Elimination of complex proxy setups
- Streamlined deployment

But the most powerful benefit? **It enables Server-Side Rendering out of the box.**

## How StartER Implements SSR with Express and Vite

The key to StartER's SSR implementation is how it integrates Vite's middleware mode with Express while leveraging React Router's static rendering capabilities.

### The Core Structure

```
.
├── index.html
├── server.ts             # Main application server
└── src
    ├── entry-client.tsx  # Mounts the application on a DOM element
    ├── entry-server.tsx  # Renders the application using Vite's SSR API
    └── react
        └── routes.tsx    # Entry point for React code (client/server agnostic)
```

This structure separates the concerns while maintaining a cohesive application architecture.

### Setting Up the Development Server

The magic starts in `server.ts` where we configure Vite in middleware mode:

```typescript
import express from "express";
import { createServer as createViteServer } from "vite";

const app = express();
const isProduction = process.env.NODE_ENV === "production";

if (!isProduction) {
  // Create Vite server in middleware mode
  const vite = await createViteServer({
    server: { middlewareMode: true },
    appType: "custom",
  });

  // Use vite's connect instance as middleware
  app.use(vite.middlewares);

  app.use(/(.*)/, async (req, res, next) => {
    // Handle SSR rendering
    // (We'll explore this next)
  });
}
```

By using `middlewareMode: true` and `appType: "custom"`, we tell Vite to let our Express server take control while still providing all its development features.

### Server-Side Rendering Implementation

The real SSR magic happens in the catch-all route handler and the `entry-server.tsx` file. StartER uses React Router's static handlers and `renderToPipeableStream` for efficient rendering with Suspense support:

```typescript
app.use(/(.*)/, async (req, res, next) => {
  const url = req.originalUrl;
  const indexHtml = fs.readFileSync("index.html", "utf-8");
  
  // Transform HTML with Vite plugins
  const template = await vite.transformIndexHtml(url, indexHtml);
  
  // Load the server entry module
  const { render } = await vite.ssrLoadModule("/src/entry-server");
  
  // Render the app HTML
  await render(template, req, res);
});
```

And in `entry-server.tsx`:

```typescript
import { Transform } from "node:stream";
import { renderToPipeableStream } from "react-dom/server";
import { createStaticHandler, createStaticRouter, StaticRouterProvider } from "react-router";

import routes from "./react/routes";

const { query, dataRoutes } = createStaticHandler(routes);

export const render = async (template, req, res) => {
  // Get routing context
  const context = await query(
    new Request(`${req.protocol}://${req.get("host")}${req.originalUrl}`)
  );
  
  // Create a static router for SSR
  const router = createStaticRouter(dataRoutes, context);
  
  // Stream the rendered content
  const { pipe } = renderToPipeableStream(
    <StaticRouterProvider router={router} context={context} />
  );

  // Respond with streamed HTML
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

This streaming approach has several benefits:
- Support for React's `<Suspense>` on the server
- Faster Time-To-First-Byte (TTFB)
- Progressive rendering of content
- Better user experience, especially on slower connections

## Production Deployment

For production, StartER uses a two-part build process:

```json
{
  "scripts": {
    "build:client": "vite build --outDir dist/client",
    "build:server": "vite build --outDir dist/server --ssr src/entry-server"
  }
}
```

The server code then loads the pre-built SSR bundle in production mode instead of using Vite's development-time `ssrLoadModule`.

## Why Choose StartER for Your Next Project?

StartER is more than just an SSR implementation – it's a complete web application framework designed for educational purposes that doesn't sacrifice power or flexibility.

### Key Benefits:

- **Educational Focus**: Perfect for beginners and those advancing their skills
- **Seamless Development**: One server approach eliminates complexity
- **Modern Stack**: React + Express + Vite + SSR
- **Production Ready**: Optimized build process for deployment
- **Best Practices**: Combines the best packages in the JS ecosystem

Whether you're learning web development or building a production application, StartER provides a solid foundation that lets you focus on what matters – building your application features.

## Get Started with StartER

Ready to try this "one server" approach for your next React + Express project? 

Check out [StartER on GitHub](https://github.com/rocambille/start-express-react) and give it a star if you find it useful!

The full implementation details for the SSR functionality can be found in:
- [`server.ts`](https://github.com/rocambille/start-express-react/blob/main/server.ts)
- [`entry-server.tsx`](https://github.com/rocambille/start-express-react/blob/main/src/entry-server.tsx)
- [`entry-client.tsx`](https://github.com/rocambille/start-express-react/blob/main/src/entry-client.tsx)

What web development challenges are you facing that StartER might help solve? [Open a discussion](https://github.com/rocambille/start-express-react/discussions) to let me know!

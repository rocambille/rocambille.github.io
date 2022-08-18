---
ref: 2020-07-15-why-you-should-use-typescript-a-tale-of-self-documented-code
lang: en
title: "Why you should use TypeScript: a tale of self-documented code"
tags: typescript javascript react
authors:
  - romain-guillemot
updated_at: "2022-05-14"
---

![documentation is beautiful](/assets/2020-07-15-why-you-should-use-typescript-a-tale-of-self-documented-code-cover-min.jpeg)

Make your code the best comment ever.<!--more-->

## The undocumented array

Let's start with a simple line of JavaScript:

```ts
const a = [];
```

Well... that's an array declaration: no big deal. What will be this array used for?

Hard to say without any context, plus the variable name isn't helping. Here is the context: I'm working on a side project using React and React Router (v6). Here is the real code, with a real variable name, in a file named `routes.js`:

```ts
const routes = [
  { path: "/foo", element: <Foo /> },
  { path: "/bar", element: <Bar /> },
];

export default routes;
```

Here you are: the array will list the routes of the application. Then you may use it:

```jsx
import { useRoutes } from "react-router";
import routes from "./routes";

export default function App() {
  return useRoutes(routes);
}
```

Of course, I didn't invent that pattern. I saw a similar code, and wanted to make it a [template repository](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-template-repository) on GitHub. Some kind of personal React framework. Thus the array should be empty in `routes.js`. Indeed the template shouldn't declare any route: I don't know the application I will build from my framework.

```ts
const routes = [];

export default routes;
```

I will remember how it should be filled since that's my own code... at least a while.

## Eagleson's law

You may have heard it:

> "Any code of your own that you haven't looked at for six or more months might as well have been written by someone else."

Sure I would be thankful to myself 6 months later &mdash; or next week &mdash; if I add some comment:

```ts
/* route format: {path: '/foo', element: <Foo />} */
const routes = [];

export default routes;
```

It may looks ok at first glance, but looking closer it's not. Really not. First, I can't expect anyone &mdash; including me &mdash; to understand this comment. It describes something as a route format. It doesn't explain what to do with the `routes` array. In fact it's only useful if you _already_ know what to do. Good enough for a reminder, but not what you would expect of a documentation.

And there is an other issue. Do you always read comments in code? I don't. Hours of code reading trained my eyes and my brain to ignore them as much as they can. I'm used to see comments as pollution between 2 lines of code.

Let's see the glass half full. Now we have a checklist to write useful documentation:

- you should express it in a explicit, non-ambiguous form,
- you should ensure it will be read.

"Explicit, non-ambiguous expression" doesn't look like a definition of writing, but of coding. You can't be sure any human will read what you wrote. But would you ask a program, you can always be sure it will "read" your code. So why not coding the documentation? A code version of this comment:

```ts
/* route format: {path: '/foo', element: <Foo />} */
```

## When the best comment is the code itself

That's where [TypeScript](https://www.typescriptlang.org/) can help us. In a nutshell, you can use TypeScript to describe the type of value expected for a variable:

```ts
const anyValue = "42"; // standard JS: will never complain
const anyString: string = "42"; // ok: '42' is a string
const anyNumber: number = "42"; // no: '42' is not a number
```

That's for primitives. Would you need to ensure an object has specific, typed properties you can define [an interface or a type alias](https://www.typescriptlang.org/docs/handbook/2/objects.html):

```ts
interface MyInterface {
  anyString: string;
  anyNumber: number;
}

const myObject: MyInterface = {
  anyString: '42',
  anyNumber: '42'; // again, wrong type !!!
};
```

And that's what we need. Instead of a not-sure-to-be-useful comment, we can "type" the array. This will describe its future content:

```ts
import { ReactNode } from "react";

interface MyRoute {
  path: string;
  element: ReactNode;
}

const routes: MyRoute[] = [];

export default routes;
```

TypeScript can't misunderstand this, and it will never forget to read it. You can try it with an invalid object:

```ts
import { ReactNode } from "react";

interface MyRoute {
  path: string;
  element: ReactNode;
}

const routes: MyRoute[] = [{ whenToRender: "/foo", whatToRender: <Foo /> }];

export default routes;
```

This will output something like that:

```ts
Type '{ whenToRender: string; whatToRender: any; }' is not assignable to type 'MyRoute'
Object literal may only specify known properties, and 'whenToRender' does not exist in type 'MyRoute'
```

That's how TypeScript can help you to self-document your code ;)

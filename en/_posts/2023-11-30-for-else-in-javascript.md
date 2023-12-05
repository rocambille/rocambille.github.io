---
ref: 2023-11-30-for-else-in-javascript
lang: en
title: "For...else in JavaScript"
tags: javascript pattern discuss
authors:
  - romain-guillemot
---

A common pattern when using a `for` loop is to combine it with an `else` statement. This is possible in many template engines, such as [Twig](https://twig.symfony.com/doc/3.x/tags/for.html#the-else-clause), as shown below:

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

Yet in programming languages like JavaScript, a `for...else` construct is not supported:

```js
for (const user of users) {
  console.log(user.username);
} else { // ❌ this is not working
  console.log("no user found");
}
```

The `else` keyword only works in combination with the `if` keyword. That's the limit, but also a solution:

```js
if (users.length > 0) {
  console.log("no user found");
} else {
  for (const user of users) {
    console.log(user.username);
  }
}
```

"Reversing" the logic let us approach the `for...else` pattern. Instead of writing the loop first then the `else`, we're checking the "no user" case first, to end up in the loop after the `else`. Here the `else` block only contains one statement: the `for` statement. We can remove some curly brackets and shorten the code:

```js
if (users.length > 0) {
  console.log("no user found");
} else for (const user of users) {
  console.log(user.username);
}
```

That's the closest I get from a `for...else` pattern in JavaScript. What do you think? Do you see a closer one?

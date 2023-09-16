---
ref:
lang: en
title: "An MVC architecture to learn Express... and vice versa"
tags: express mvc javascript
authors:
  - romain-guillemot
---

```js
const express = require("express");

const app = express();

app.get("/", (req, res) => {
  res.send("Hello World !!!");
});

const port = 5000;

app.listen(port, (err) => {
  if (err) {
    console.error("Something bad happened");
  } else {
    console.log(`Server is listening on ${port}`);
  }
});
```

```js
app.get("/", (req, res) => {
  res.send("Hello World !!!");
});
```

```js
const helloWorld = (req, res) => {
  res.send("Hello World !!!");
};

app.get("/", helloWorld);
```

```js
const getUnicorns = async (req, res) => {
  try {
    const [unicorns] = await database.query("select * from unicorns");

    res.json(unicorns);
  } catch (err) {
    console.error(err);

    res.sendStatus(500);
  }
};

app.get("/unicorns", getUnicorns);
```



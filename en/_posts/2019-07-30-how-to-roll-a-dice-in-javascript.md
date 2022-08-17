---
ref: 2019-07-30-how-to-roll-a-dice-in-javascript
lang: en
title: How to roll a dice in JavaScript?
tags: javascript beginners random
authors:
  - romain-guillemot
updated_at: "2022-05-11"
---

![dices are beautiful](https://cdn-images-1.medium.com/max/1024/0*yAFHaiCH9sI5cdLe)

Let's build the ultimate dice step by step.<!--more-->

## Math.random() as a basis

A dice is a tool providing a random integer each time you roll it. Something like that:

```js
function rollDice() {
  return /* some randomly generated number */;
}
```

Every programming language has a built-in random function. In JavaScript, it's [`Math.random`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random):

```js
function rollDice() {
  return Math.random();
}
```

That’s a good start: returning a random number. Remember `Math.random` is not "random enough" for serious things like cryptography, or casino games -- read about [Crypto.getRandomValues](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/getRandomValues) if that’s your business. `Math.random` is fair enough to roll a dice with friends. Let’s try it:

```js
>> function rollDice() {
     return Math.random();
   }
>> rollDice();
<- 0.7367823644188911
```

This `0.7367823644188911` is not really what we wanted... According to documentation, `Math.random` returns a decimal number between 0 (inclusive) and 1 (exclusive). For a 6-sided dice, we need an integer from 1 to 6. As a first guess, you may multiply by 6:

```js
>> function rollDice() {
     return Math.random() * 6;
   }
>> rollDice();
<- 4.3380209914241235
```

So we would have a random decimal number between 0 (inclusive) and 6 (exclusive). So far, so good. Next step would be to get integer values:

- If 0 ≤ `Math.random() * 6` < 1, return 1
- If 1 ≤ `Math.random() * 6` < 2, return 2
- If 2 ≤ `Math.random() * 6` < 3, return 3
- If 3 ≤ `Math.random() * 6` < 4, return 4
- If 4 ≤ `Math.random() * 6` < 5, return 5
- If 5 ≤ `Math.random() * 6` < 6, return 6

This can be done using [`Math.floor`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/floor). Let's try again -- with a for-loop to `console.log` multiple rolls:

```js
>> function rollDice() {
     return Math.floor(Math.random() * 6);
   }
>> for(let i = 0; i < 5; i++) console.log(rollDice());
   5
   1
   4
   2
   0 // WTF?
```

Once again, not exactly what we wanted... What we get here is:

- If 0 ≤ `Math.floor(Math.random() * 6)` < 1, return 0. Not 1.
- If 1 ≤ `Math.floor(Math.random() * 6)` < 2, return 1. Not 2.
- If 2 ≤ `Math.floor(Math.random() * 6)` < 3, return 2. Not 3.
- If 3 ≤ `Math.floor(Math.random() * 6)` < 4, return 3. Not 4.
- If 4 ≤ `Math.floor(Math.random() * 6)` < 5, return 4. Not 5.
- If 5 ≤ `Math.floor(Math.random() * 6)` < 6, return 5. Not 6.

To get the wanted result with `Math.floor`, we will have to add 1 before returning:

```js
function rollDice() {
  return 1 + Math.floor(Math.random() * 6);
}
```

Now we have a function to simulate our 6-sided dice :)

> Yes, but... What if we want a 4-, 8-, 12- or 20-sided one?

No big deal: you can change the magic number 6 in the code for a parameter, passing the maximum value for your dice. Something like this:

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

## The ultimate dice

I was once inspired by a vision: "[The Ultimate Display](https://www.wired.com/2009/09/augmented-reality-the-ultimate-display-by-ivan-sutherland-1965/)" by Ivan E. Sutherland, 1965. Among others, I like this quote:

> There is no reason why the objects displayed by a computer have to follow the ordinary rules of physical reality with which we are familiar.

We used a parameter to replace the number of sides of our dice. Why not removing the other magic number? This ugly 1 may become an other parameter:

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

This final version allows to simulate a dice which is not starting at 1. Moreover the `max` allows to simulate a uniform fair dice beyond "the ordinary rules of physical reality". Imagine a 7-sided one. You can mimic your favorite dice game following its ordinary rules. But if you can imagine one, roll a dice which would never exist in reality ;)

+++
author = "Alcadramin"
title = "TL;DR; 3 Different ways to create an array from 1..N in JavaScript"
description = "TL;DR; 3 Different ways to create an array from 1..N in JavaScript"
tags = ["JavaScript"]
series = ["TL;DR;"]
categories = ["TL;DR;"]
+++

We need this often, here are 3 snippets that give you the same results.

```js
Array.from({ length: N }, (_, i) => i + 1); // => [1, 2, 3, 4, .., N]
```

```js
[...Array(N).keys()].map((i) => i + 1); // => [1, 2, 3, 4, .., N]
[...Array(N).keys()]; // => [0, 1, 2, 3, 4, .., N]
```

```js
Array.from(Array(N), (_, i) => i + 1); // => [1, 2, 3, 4, .., N]
Array.from(Array(N).keys()); // => [0, 1, 2, 3, 4, .., N]
```

> Credit: https://stackoverflow.com/a/33352604

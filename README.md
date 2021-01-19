# 🧙‍♂️ JavaScript Tips & Tricks To Code Like A Wizard hero 🦸‍♂️

### 1. Generate numbers between a range

There are some situations when we create an array with numbers range. Let's say for birthday input where you need that range. Here's the easiest way to implement it.

```js
let start = 1900,
    end = 2000;
[...new Array(end + 1).keys()].slice(start);
// [ 1900, 1901, ..., 2000]

// also this, but more unstable results for big range
Array.from({ length: end - start + 1 }, (_, i) => start + i);
```

### 2. Use array of values as arguments for functions

We have cases when you need to collect your values in array(s) and then pass it as arguments for function. With ES6 you can just use spread operator (...) and extract your array from `[arg1, arg2] > (arg1, arg2)`

```js
const parts = {
  first: [0, 2],
  second: [1, 3],
};

["Hello", "World", "JS", "Tricks"].slice(...parts.second);
// ["World", "JS", "Tricks"]
```

_You can use it with any function_


### 3. Use values as arguments with Math methods
So, we're good in spreading values to put them into functions. When we need to find Math.max or Math.min of our numbers in array we do it like below.

```js
// Find the highest element's y position is 474px
const elementsHeight =  [...document.body.children].map(
  el => el.getBoundingClientRect().y
);
Math.max(...elementsHeight);
// 474

const numbers = [100, 100, -1000, 2000, -3000, 40000];
Math.min(...numbers);
// -3000
```

### 4. Merge/flatten your arrays in arrays
There's a nice method for Array called `Array.flat`, as an argument it needs depth you need to flat `(default: 1)`. But what if you don't know the depth, you need to flatten it all. We just put `Infinity` as the argument. Also there's a nice [flatMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap) method.

```js
const arrays = [[10], 50, [100, [2000, 3000, [40000]]]];
arrays.flat(Infinity);
// [ 10, 50, 100, 2000, 3000, 40000 ]
```

### 5. Preventing your code crash
It's not good to have unpredictable behavior in your code, but if you have it you need to handle it.

For example. Common mistake `TypeError`, if you're trying to get property of undefined/null and etc.

Note. Use it if your project doesn't support [optional chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining?fbclid=IwAR2_X1H9hAPhmChM5ACKNrYHMWb-vA0G7KOmakCmhURzjl5Biy4ta8_bQrM)

```js
const found = [{ name: "Alex" }].find(i => i.name === 'Jim');
console.log(found.name);
// TypeError: Cannot read property 'name' of undefined
```

You can avoid it like this
```js
const found = [{ name: "Alex" }].find(i => i.name === 'Jim') || {};
console.log(found.name);
// undefined
```

Of course it depends on situations, but for minor handle it's okay. You don't need to write huge code to handle it.

### 6. Nice way to pass arguments
Good example of using this feature is `styled-components`, in ES6 you can pass **Template literals** as argument for function without brackets. Nice trick if you're implementing function that format/convert your text.

```js
const makeList = (raw) =>
  raw
    .join()
    .trim()
    .split("\n")
    .map((s, i) => `${i + 1}. ${s}`)
    .join("\n");

makeList`
Hello, World
Hello, World
`;
// 1. Hello
// 2. World
```

### 7. Swap variables like a wizard
With Destructuring assignment syntax we can easily swap variables.

```js
let a = "hello";
let b = "world";

// Wrong
a = b
b = a
// { a: 'world', b: 'world' }

// Correct
[a, b] = [b, a];
// { a: 'world', b: 'hello' }
```
Solution for wrong way is to add third temporary variable :(

### 8. Sort by alphabetical order
I worked a lot in international companies and their apps had non-english data. When you do your **"awesome"** tricks to sort list of this kind of data it looks okay, sometimes because there're just a few strings for that moment. Maybe it looks okay cause you don't know that language's alphabet.
Use correct one to be sure that it's sorted by alphabetical order for that language.

For example. Deutsche alphabet
```js
// Wrong
["a", "z", "ä"].sort((a, b) => a - b);
// ['a', 'z', 'ä']

// Correct
["a", "z", "ä"].sort((a, b) => a.localeCompare(b));
// [ 'a', 'ä', 'z' ]
```

### 9. Mask it nicely
Final trick is about masking strings. When you need to mask any variable. Not password of course :) it's just example. We just get part of string `substr(-3)`, 3 characters from its end and fill length that left with any symbols (example `*`)

```js
const password = "hackme";
password.substr(-3).padStart(password.length, "*");
// ***kme
// Hmmm I guess your first attempts might be wrong
```

### 10. Generate a list with random numbers

We need A LOT of fake data, for different reasons. So here's a way to do that gently.

```jsx
Array.from({ length: 1000 }, Math.random)
// [ 0.6163093133259432, 0.8877401276499153, 0.4094354756035987, ...] - 1000 items
```

### 11. Generate a list with numbers

Yep, just one more trick to generate a list with numbers.

```jsx
Array.from({ length: 1000 }, (v, i) => i)
// [0, 1, 2, 3, 4, 5, 6....999]
```
<i>1-2 edited. Thanks to [Andrew Courtice](https://dev.to/andrewcourtice)</i>

### 12. Convert RGB → HEX

Convert your RGB to HEX without any libs!

```jsx
const rgb2hex = ([r, g, b]) =>
  `#${(1 << 24) + (r << 16) + (g << 8) + b}`.toString(16).substr(1);

rgb2hex([76, 11, 181]);
// #4c0bb5
```

### 13. Convert HEX → RGB

What's about convert it back?! Here's a nice way to implement that.

```jsx
const hex2rgb = hex =>
  [1, 3, 5].map((h) => parseInt(hex.substring(h, h + 2), 16));

hex2rgb("#4c0bb5");
// [76, 11, 181]
```

### 14. Odd or Even

Another yet odd/even checking.

```jsx
const value = 232;  

if (value & 1) console.log("odd");
else console.log("even");
// even
```

### 15. Check valid  URL

I guess most of you use this way to validate URL, but anyway. Let's share it

```jsx
const isValidURL = (url) => {
  try {
    new URL(url);
    return true;
  } catch (error) {
    return false;
  }
};

isValidURL("https://dev.to");
// true

isValidURL("https//invalidto");
// false
```

### 16. N something ago

Sometimes you need something to print a date to `6 minute(s) ago`, but  don't want to import monster-size libs. Here's a small snippet that does it, easily modify it as you wish and go ahead.

```jsx
const fromAgo = (date) => {
  const ms = Date.now() - date.getTime();
  const seconds = Math.round(ms / 1000);
  const minutes = Math.round(ms / 60000);
  const hours = Math.round(ms / 3600000);
  const days = Math.round(ms / 86400000);
  const months = Math.round(ms / 2592000000);
  const years = Math.round(ms / 31104000000);

  switch (true) {
    case seconds < 60:
      return `${seconds} second(s) ago"`;
    case minutes < 60:
      return `${minutes} minute(s) ago"`;
    case hours < 24:
      return `${hours} hour(s) ago"`;
    case days < 30:
      return `${days} day(s) ago`;
    case months < 12:
      return `${months} month(s) ago`;
    default:
      return `${years} year(s) ago`;
  }
};

const createdAt = new Date(2021, 0, 5);
fromAgo(createdAt); // 14 day(s) ago;
```

### 17. Generate path with params

We work a lot with routes/paths and we always need to manipulate them. When we need to generate a path w/ params to push browser there, **generatePath** helps us!

```jsx
const generatePath = (path, obj) =>
	path.replace(/(\:[a-z]+)/g, (v) => obj[v.substr(1)]);

const route = "/app/:page/:id";
generatePath(route, {
  page: "products",
  id: 85,
});
// /app/products/123
```

### 18. Get params from path

Yes! Now we need to get our params back. Also, you can pass **serializer** to parse your data gently.

```jsx
const getPathParams = (path, pathMap, serializer) => {
  path = path.split("/");
  pathMap = pathMap.split("/");
  return pathMap.reduce((acc, crr, i) => {
    if (crr[0] === ":") {
      const param = crr.substr(1);
      acc[param] = serializer && serializer[param]
        ? serializer[param](path[i])
        : path[i];
    }
    return acc;
  }, {});
};

getPathParams("/app/products/123", "/app/:page/:id");
// { page: 'products', id: '123' }

getPathParams("/items/2/id/8583212", "/items/:category/id/:id", {
  category: v => ['Car', 'Mobile', 'Home'][v],
  id: v => +v
});
// { category: 'Home', id: 8583212 }
```

### 19. Generate path with query string

Of course, we work with paths and we need to generate path with query too.

```jsx
const generatePathQuery = (path, obj) =>
  path +
  Object.entries(obj)
    .reduce((total, [k, v]) => (total += `${k}=${encodeURIComponent(v)}&`), "?")
    .slice(0, -1);

generatePathQuery("/user", { name: "Orkhan", age: 30 }); 
// "/user?name=Orkhan&age=30"
```

### 20. Get params from query string

Now it's a time to get params from query string.

```jsx
const getQueryParams = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce((total, crr) => {
    const [key, value] = crr.split("=");
    total[key] = value;
    return total;
  }, {});

getQueryParams("/user?name=Orkhan&age=30");
// { name: 'Orkhan', age: '30' }
```


### Conclusion

Try to understand how everything works and keep your code nice/clean ❤️


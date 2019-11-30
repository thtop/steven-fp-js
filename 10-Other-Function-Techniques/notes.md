## Other Function Techniques and Articles

### Recursion

> Recursion is a function that calls itself

> The basic idea if recursion is that you write a functioh that will solve a small part of the problem and then you call it over and over again until the larger problem is solved.

```js
// 5 * 4 * 3 * 2 * 1 = 120

const factorial = function(num) {
  // console.log(num);
  if (num === 1) {
    return 1;
  }

  return num * factorial(num - 1);
};

console.log(factorial(5)); // 120
```

### Recursin and the Stack

- 5 x factorial(4)
- 5 x 4 x factorial(3)
- 5 x 4 x 3 x factorial(2)
- 5 x 4 x 3 x 2 x factorial(1)
- 5 x 4 x 3 x 2 x 1
- 5 x 4 x 3 x 2
- 5 x 4 x 6
- 5 x 24
- 120

### Arrow Function

```js
// 5 * 4 * 3 * 2 * 1 = 120

const factorial = num => (num === 1 ? 1 : num * factorial(num - 1));

console.log(factorial(5)); // 120
```

### Additional Articles

I have found it helpful to learn functional concepts from multiple authors. I've included here some of the best articles on functional programming. I hope they will add to what you have already learned:

- [Functional Programming Principles in JavaScript](https://www.freecodecamp.org/news/functional-programming-principles-in-javascript-1b8fc6c3563f/)

- [Functional Light JavaScript](https://github.com/getify/Functional-Light-JS/blob/master/manuscript/ch11.md/#chapter-11-putting-it-all-together)

- [Master the JavaScript Interview: What is Function Composition?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-function-composition-20dfb109a1a0)

- [Curry and Function Composition](https://medium.com/javascript-scene/curry-and-function-composition-2c208d774983)

- [Functional Programming in JavaScript](https://codeburst.io/functional-programming-in-javascript-e57e7e28c0e5)

- [Discover Functional Programming in JavaScript with this Thorough Introduction](https://www.freecodecamp.org/news/discover-functional-programming-in-javascript-with-this-thorough-introduction-a2ad9af2d645/)

- [How Point-free Composition will Make You a Better Functional Programmer](https://www.freecodecamp.org/news/how-point-free-composition-will-make-you-a-better-functional-programmer-33dcb910303a/)

- [From Imperative to Functional JavaScript](https://codeburst.io/from-imperative-to-functional-javascript-5dc9e16d9184)

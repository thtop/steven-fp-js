## Function Composition

### First Class Functions

- Why are JavaScript Functions Frist Class?
- Michael Fogus from his book _Functional JavaScript_

> The term "first-class" means that something is just a value. A first-class function is one that can go anywhere that any other value can go--there are few to no restrictions.

```js
// 1) store in variable
var num = 28;
var funct = function() {
  console.log("Hello");
};

funct(); // Hello

// 2) store in array
var arr = [
  28,
  function() {
    console.log("Hi from an array");
  }
];
arr[1](); // Hi from an array

// 3) store in property in an object
var obj = {
  num: 20,
  funct: function() {
    console.log("Hello from an object");
  }
};
obj.funct(); // Hello from an object

// 4) part of an expression
console.log(
  28 +
    (function() {
      return 10;
    })()
); // 38

// 5) pass fn in to another fn
var addTwo = function(num, fn) {
  console.log(num + fn());
};

addTwo(20, function() {
  return 28;
}); // 48

// 6) return from a fn
var returnNumber = function() {
  return 20;
};

var returnFun = function() {
  return function() {
    console.log("Hello for last tiem.");
  };
};

var newFun = returnFun();
newFun(); // Hello for last tiem.
```

### Higher Order Functions

- How fo first class funcitons relate to higher order functions and why are both concepts necessary?

> Higher order functions are functions that operate on other functions by either taking them as arguments or returning them. The fact that JavaScript supports first-clss functions makes it possible to create higher order functions.

```js
let things = ["Building", "car", "bycycle", "autobobile", "Tree", "house"];

//things.sort();

//console.log(things);
// [ 'Building', 'Tree', 'autobobile', 'bycycle', 'car', 'house' ]

things.sort(function(a, b) {
  let x = a.toLowerCase();
  let y = b.toLowerCase();

  if (x < y) {
    return -1;
  }

  if (y < x) {
    return 1;
  }

  return 0;
});

console.log(things);
// [ 'autobobile', 'Building', 'bycycle', 'car', 'house', 'Tree' ]
```

### Demystifying JavaScript Closure

- JavaScript uses function scope
- Scope refers to the rules that determine where within out program, variables and functions are accessible.

```js
var funct = function funct() {
  var a = 2;
  var b = 3;

  var funct2 = function funct2() {
    console.log(a + b);
  };

  funct2();
};

var funct3 = function funct3() {
  console.log(a + b);
};

funct(); // 5

funct3(); // ReferenceError: a Error: is not defined
```

### Closure Defined

- A closure is the local variables for a function - kept alive after the funciton has returned. (javascriptkit.com)
- closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexixal scope. (Kyle Simpson)
- A closure is a function having access to the parent scope, even after the parent function has closed. (W3C Schools)

```js
var funct = function functi() {
  var a = 2;
  var b = 3;

  var funct2 = function funct2() {
    console.log(a + b);
  };

  setTimeout(funct2, 1000);
};

funct(); // 5
```

```js
(function counter() {
  var cnt = 0;
  var item1 = document.querySelector("#item1");
  var item2 = document.querySelector("#item2");

  var proint = function print() {
    console.log(cnt);
  };

  item1.addEventListener("click", function count1() {
    cnt++;
    print();
  });

  item2.addEventListener("click", function count2() {
    cnt++;
    print();
  });
})();
```

```js
var APP = (function module() {
  var a = 3;

  var printIt = function printIt(val) {
    console.log(val);
  };

  var sumIt = function sumIt(b) {
    printIt(a + b);
  };

  var multiplyIt = function multipleIt(b) {
    printIt(a * b);
  };

  return {
    sumIt: sumIt,
    multiplyIt: multiplyIt
  };
})();

console.log(APP); // { sumIt: [Function: sumIt], multiplyITt: [Function: multipleIt] }

APP.sumIt(10); // 13
APP.multiplyIt(5); // 15
```

### Function Composition Part 1

> Function composition is the combining of functions for the purpose of producing a new function.

- Functions in Functional Programming
  - Function have an input
  - Function return a value
  - Function are simplified to a single task
-

```js
check(splits(tim(str)));
```

**Kyle Simpson**

> A function programmer sees every function in their program like a little simple lego piece. They recognize the blue 2x2 brick at a glance, and know exactly how it workd and what they can do with it. As they go about building a bigger comples lego model, as they need each next piece, they already have an instinct for which if their many spare pieces to grab.

> But sometimes you take the blue 2x2 brick and the gray 4x1 brick and put them together in a certain way, and you realize, "that's a useful piece that I need often."

### Arrow Functions

```js
var sum = function(num1, num2) {
  return num1 + num2;
};

// Arrow Function
var sum = (num1, num2) => num1 + num2;

var funct1 = num => num * num;

var funct2 = () => 100;
```

### Exercise 5 Start

```js
const scores = [50, 6, 100, 0, 10, 75, 8, 60, 90, 80, 0, 30, 110];

//Any scores that are below 10 needs to be multiplied by 10 and the new value included.
const boostSingleScores = scores.map(function(val) {
  return val < 10 ? val * 10 : val;
});

//Remove any scores that are over 100.
const rmvOverScores = boostSingleScores.filter(function(val) {
  return val <= 100;
});

//Remove any scores that are 0 or below.
const rmvZeroScores = rmvOverScores.filter(function(val) {
  return val > 0;
});

//Sum the scores.
const scoresSum = rmvZeroScores.reduce(function(sum, val) {
  return sum + val;
}, 0);

//Provide a count for the number of scores still remaining.
const scoresCnt = rmvZeroScores.reduce(function(cnt, val) {
  return cnt + 1;
}, 0);
```

### Exercise 5 Finish

```js
const scores = [50, 6, 100, 0, 10, 75, 8, 60, 90, 80, 0, 30, 110];

//Any scores that are below 10 needs to be multiplied by 10 and the new value included.
const boostSingleScores = scores.map(val => (val < 10 ? val * 10 : val));

//Remove any scores that are over 100.
const rmvOverScores = boostSingleScores.filter(val => val <= 100);

//Remove any scores that are 0 or below.
const rmvZeroScores = rmvOverScores.filter(val => val > 0);

//Sum the scores.
const scoresSum = rmvZeroScores.reduce((sum, val) => sum + val, 0);

//Provide a count for the number of scores still remaining.
const scoresCnt = rmvZeroScores.reduce((cnt, val) => cnt + 1, 0);
```

### Function Composition Part 2

**Version 1**

```js
const str = "Innovation distinguishes between a leader and a follower.";

let prepareString = function() {
  let str1 = str.trim();
  let str2 = str1.replace(/[?.,!]/g, "");
  let str3 = str2.toUpperCase();
  let arr = str3.split(" ");

  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === "A" || arr[i] === "AN" || arr[i] === "THE") {
      arr.splice(i, 1);
    }
  }
  return arr;
};

console.log(prepareString(str));

/*

Info: Start process (8:13:07 PM)
[
  'INNOVATION',
  'DISTINGUISHES',
  'BETWEEN',
  'LEADER',
  'AND',
  'FOLLOWER'
]
Info: End process (8:13:07 PM)

*/
```

**Version 2**

```js
const str = "Innovation distinguishes between a leader and a follower.";

let prepareString = function() {
  let arr = str
    .trim()
    .replace(/[?.,!]/g, "")
    .toUpperCase()
    .split(" ");

  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === "A" || arr[i] === "AN" || arr[i] === "THE") {
      arr.splice(i, 1);
    }
  }
  return arr;
};

console.log(prepareString());

/*

Info: Start process (8:15:36 PM)
[
  'INNOVATION',
  'DISTINGUISHES',
  'BETWEEN',
  'LEADER',
  'AND',
  'FOLLOWER'
]

*/
```

**Varsion 3**

```js
const str = "Innovation distinguishes between a leader and a follower.";

const trim = str => str.replace(/^\s*|\s*$/g, "");

const noPunct = str => str.replace(/[?.,!]/g, "");

const capitalize = str => str.toUpperCase();

const breakout = str => str.split(" ");

const noArticles = str => str !== "A" && str !== "AN" && str !== "THE";

const filterArticles = arr => arr.filter(noArticles);

//console.log(filterArticles(breakout(capitalize(noPunct(trim(str))))));

/*

Info: Start process (8:17:50 PM)
[
  'INNOVATION',
  'DISTINGUISHES',
  'BETWEEN',
  'LEADER',
  'AND',
  'FOLLOWER'
]
Info: End process (8:17:50 PM)

*/

// pipe (L -> R) and compose (R -> L)

// Compose (R -> L)
const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight((v, f) => {
      return f(v);
    }, x);
  };
};

const prepareString1 = compose(
  filterArticles,
  breakout,
  capitalize,
  noPunct,
  trim
);

console.log(prepareString1(str));

/*

Info: Start process (8:22:24 PM)
[
  'INNOVATION',
  'DISTINGUISHES',
  'BETWEEN',
  'LEADER',
  'AND',
  'FOLLOWER'
]
Info: End process (8:22:25 PM)

*/

///******************************

// Pipe (L -> R)
const pipe = function(...fns) {
  return function(x) {
    return fns.reduce((v, f) => {
      return f(v);
    }, x);
  };
};

const prepareString2 = pipe(
  trim,
  noPunct,
  capitalize,
  breakout,
  filterArticles
);

console.log(prepareString2(str));

/*

Info: Start process (8:34:28 PM)
[
  'INNOVATION',
  'DISTINGUISHES',
  'BETWEEN',
  'LEADER',
  'AND',
  'FOLLOWER'
]
Info: End process (8:34:28 PM)

*/
```

### Exercise 6 Start

```js
const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

const scores = [50, 6, 100, 0, 10, 75, 8, 60, 90, 80, 0, 30, 110];

const boostSingleScores = scores.map(val => (val < 10 ? val * 10 : val));

const rmvOverScores = boostSingleScores.filter(val => val <= 100);

const rmvZeroScores = rmvOverScores.filter(val => val > 0);

const scoresSum = rmvZeroScores.reduce((sum, val) => sum + val, 0);

const scoresCnt = rmvZeroScores.reduce((cnt, val) => cnt + 1, 0);

//Convert each statement to a function that can accept and act on any array.

//Compose a function that will remove both zero or lower scores and scores over 100. Test it using the scores array.

//Compose a function that will do all the modifications to an array. Test it using the scores array.

//Create a function that will accept an array and return the average. Use the function that sums scores and the function that counts scores or the length property.

//Compose a function that will do all the modifications on an array and return an average.
```

### Exercise 6 Finish

```js
const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

const scores = [50, 6, 100, 0, 10, 75, 8, 60, 90, 80, 0, 30, 110];

const singleScoresByTen = function(arry) {
  return arry.map(val => (val < 10 ? val * 10 : val));
};

const rmvOverScores = function(arry) {
  return arry.filter(val => val <= 100);
};

const rmvZeroScores = function(arry) {
  return arry.filter(val => val > 0);
};

const sumScores = function(arry) {
  return arry.reduce((sum, val) => sum + val, 0);
};

const countScores = function(arry) {
  return arry.reduce((cnt, val) => cnt + 1, 0);
};

//Convert each statement to a function that can accept and act on any array.

//Compose a function that will remove both zero or lower scores and scores over 100. Test it using the scores array.
const rmvBothHighLow = pipe(rmvOverScores, rmvZeroScores);

const noHighLowArray = rmvBothHighLow(scores);

//Compose a function that will do all the modifications to an array. Test it using the scores array.

const prepareScores = pipe(rmvBothHighLow, singleScoresByTen);

const preparedArray = prepareScores(scores);

//Create a function that will accept an array and return the average. Use the function that sums scores and the function that counts scores or the length property.
const computeAverage = function(arry) {
  return sumScores(arry) / arry.length;
};

//Compose a function that will prepare an array and return an average.

const prepareAndComputeAve = pipe(prepareScores, computeAverage);

const ave = prepareAndComputeAve(scores);

console.log(ave);

/*
Info: Start process (8:40:40 PM)
63.5
Info: End process (8:40:40 PM)

*/
```

### Understanding Arity

```js
const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  return arr.map(function(val) {
    if (val.name.toLowerCase() === user.name.toLowerCase()) {
      return user;
    } else {
      return val;
    }
  });
};

//Pure Functions
const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

var getUser = function(arr, name) {
  return arr.reduce(function(obj, val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      return val;
    }
  }, null);
};

var updateScore = function(user, newAmt) {
  if (user) {
    user.score += newAmt;
    return user;
  }
};

var updateTries = function(user) {
  if (user) {
    user.tries++;
    return user;
  }
};

const usr = getUser(users, "Henry");
const usr1 = updateScore(cloneObj(usr), 30);
const usr2 = updateTries(cloneObj(usr1));
const newArray = storeUser(users, usr2);
```

**version 2**

```js
const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  return arr.map(function(val) {
    if (val.name.toLowerCase() === user.name.toLowerCase()) {
      return user;
    } else {
      return val;
    }
  });
};

//Pure Functions
const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

var getUser = function(arr, name) {
  return arr.reduce(function(obj, val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      return val;
    }
  }, null);
};

var updateScore = function(newAmt, user) {
  if (user) {
    user.score += newAmt;
    return user;
  }
};

var updateTries = function(user) {
  if (user) {
    user.tries++;
    return user;
  }
};

const partGetUser = getUser.bind(null, users);
const partUpdateScore30 = updateScore.bind(null, 30);

/*const usr = getUser(users, "Henry");
const usr1 = updateScore(cloneObj(usr), 30);
const usr2 = updateTries(cloneObj(usr1));
const newArray = storeUser(users, usr2);*/

const updateUser = pipe(partGetUser, cloneObj, partUpdateScore30, updateTries);

const newestUser = updateUser("Henry");

console.log(newestUser);

/*

Info: Start process (9:03:09 PM)
{ name: 'Henry', score: 110, tries: 4 }
Info: End process (9:03:09 PM)

*/
```

### JavaScript Concepts for Underatanding Currying

- Arity
  - function test(num1, num2) {...}
  - let test = function(arr, val, index) {...}
  - const sumlt = (val, arr) => ...
- Partial Application

```js
greeting("Welcome", "Steve");
greeting("Welcome", "Lexa");
greeting("Welcome", "Clark");

//

const curryGreeting = function(greeting) {
  return function(name) {
    console.log(greeting + " " + name);
  };
};

const welcomeGreet = curryGreeting("Welcome");

welcomeGreet("Steve");
welcomeGreet("Mary");

/*

Info: Start process (2:55:43 AM)
Welcome Steve
Welcome Mary
Info: End process (2:55:43 AM)

*/
```

### Deep Dive into Currying

- Why?
- Advantages
  - Currying: it will take a function that expects multiple arguments and reduce it to function multiple function that expect only a single argument.
  - This will allow us to use high arity functions as a part of composition because composition needs functions that expect a single argument.
  - We can call them unitary functions.

```js
const ffun = function(a, b, c) {
  return a + b + c;
};

const gfun = function(d, e) {
  return d + e;
};

const hfun = function(f, g, h) {
  return f + g + h;
};

// can't
const newFun = pipe(ffun, gfun);
```

**compose**

```js
function curry(fn, arity = fn.length) {
  return (function nextCurried(prevArgs) {
    return function curried(nextArg) {
      var args = [...prevArgs, nextArg];

      if (args.length >= arity) {
        return fn(...args);
      } else {
        return nextCurried(args);
      }
    };
  })([]);
}

const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

//**************

const ffun = function(a, b, c) {
  return a + b + c;
};

const gfun = function(d, e) {
  return d + e;
};

const hfun = function(f, g, h) {
  return f + g + h;
};

//************

const curriedF = curry(ffun);
const curriedG = curry(gfun);
const curriedH = curry(hfun);

//const newFun = pipe(curriedF(a)(b), curriedG(d), curriedH(f)(g));

const newFun = pipe(curriedF(1)(2), curriedG(4), curriedH(5)(6));

console.log(newFun(3));

/*

Info: Start process (3:21:25 AM)
21
Info: End process (3:21:25 AM)

*/

//************

/*
const newFun = pipe(curry(ffun)(a)(b), curry(gfun)(d), curry(hfun)(f)(g));
*/

const newFun = pipe(curry(ffun)(1)(2), curry(gfun)(4), curry(hfun)(5)(6));

//console.log(newFun(3));

/*

Info: Start process (3:16:30 AM)
21
Info: End process (3:16:30 AM)

*/
```

**Another example**

```js
function curry(fn, arity = fn.length) {
  return (function nextCurried(prevArgs) {
    return function curried(nextArg) {
      var args = [...prevArgs, nextArg];

      if (args.length >= arity) {
        return fn(...args);
      } else {
        return nextCurried(args);
      }
    };
  })([]);
}

const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

//**************

const doubleNum = function(num) {
  return num + num;
};

const totalIt = function(n1, n2, n3, n4) {
  return n1 + n2 + n3 + n4;
};

const doArray = function(num1, num2) {
  return [num1, num2];
};

const newFunction = pipe(
  doubleNum,
  curry(totalIt)(3)(2)(1),
  curry(doArray)(50)
);

console.log(newFunction(5));

/*

Info: Start process (3:27:45 AM)
[ 50, 16 ]
Info: End process (3:27:45 AM)

*/
```

### Partial application

> A partial application is a function which has been applied to some, but not yet all of its arguments. In other words, it's a function which has some atguments fixed inside its closure scope.

> Partial application is when we call a function with fewer arguments than it expects and that function returns a function that takes the remaining arguments.

### Currying

> A curried function is a function that taked multiple arguments one at a time.

> Currying is where a function that expects multiple arguments is broken down into successive functions that each take a single argument and return another function to accept the next argument.

### What's the Difference?

> Partial applications can take as many or as few arguments a time as desired. Curried funcitons on the other hand always return a unary function: a function which takes one argument.

### Advantages of Currying

1. Currying can be used to specialize functions.
2. Currying simplifies function composition.

### Dissecting the Curry Function

```js
function curry(fn, arity = fn.length) {
  return (function nextCurried(prevArgs) {
    return function curried(nextArg) {
      var args = [...prevArgs, nextArg];

      if (args.length >= arity) {
        return fn(...args);
      } else {
        return nextCurried(args);
      }
    };
  })([]);
}

const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

//// APP ////

const doubleNum = function(num) {
  return num + num;
};

const totalIt = function(n1, n2, n3, n4) {
  return n1 + n2 + n3 + n4;
};

const doArray = function(num1, num2) {
  return [num1, num2];
};

const newFunction1 = pipe(
  doubleNum,
  curry(totalIt)(3)(2)(1),
  curry(doArray)(50)
);

// console.log(newFunction(5));

/// ********* ///

const curriedTotalIt = curry(totalIt);
const curriedDoArray = curry(doArray);

const newFunction2 = pipe(
  doubleNum,
  curriedTotalIt(3)(2)(1),
  curriedDoArray(50)
);

// console.log("curriedTotalIt: ", curriedTotalIt.toString());

/*

Info: Start process (3:47:44 AM)
curriedTotalIt:  function curried(nextArg) {
      var args = [...prevArgs, nextArg];

      if (args.length >= arity) {
        return fn(...args);
      } else {
        return nextCurried(args);
      }
    }
Info: End process (3:47:45 AM)

*/

console.log(curriedDoArray.toString());

/*

Info: Start process (3:50:26 AM)
function curried(nextArg) {
      var args = [...prevArgs, nextArg];

      if (args.length >= arity) {
        return fn(...args);
      } else {
        return nextCurried(args);
      }
    }
Info: End process (3:50:26 AM)

*/
```

### Exercise 7 Start

```js
function curry(fn, arity = fn.length) {
  return (function nextCurried(prevArgs) {
    return function curried(nextArg) {
      var args = [...prevArgs, nextArg];
      if (args.length >= arity) {
        return fn(...args);
      } else {
        return nextCurried(args);
      }
    };
  })([]);
}

const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

//*****************

/// APP ///

const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  return arr.map(function(val) {
    if (val.name.toLowerCase() === user.name.toLowerCase()) {
      return user;
    } else {
      return val;
    }
  });

  /*for (let i = 0; i < arr.length; i++) {
        if (arr[i].name.toLowerCase() === user.name.toLowerCase()) {
            arr[i] = user;
            break;
        }
    }*/
};

//Pure Functions
const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

var getUser = function(arr, name) {
  return arr.reduce(function(obj, val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      return val;
    } else {
      return obj;
    }
  }, null);

  /*for (let i = 0; i < arr.length; i++) {
        if (arr[i].name.toLowerCase() === name.toLowerCase()) {
            return arr[i];
        }
    }
    return null;*/
};

var updateScore = function(user, newAmt) {
  if (user) {
    user.score += newAmt;
    return user;
  }
};

var updateTries = function(user) {
  if (user) {
    user.tries++;
    return user;
  }
};

const usr = getUser(users, "Henry");
const usr1 = updateScore(cloneObj(usr), 30);
const usr2 = updateTries(cloneObj(usr1));
const newArray = storeUser(users, usr2);

//Using currying and composition create a specialized function that always acts on the users array but allows you to enter a user name. Have it  return a clone of that user.

//Using your curried function, compose a new specialized function that will be used to update Henry. (Only invoked if you want to update Henry). It should accepts a new score and then return a new array that contains the updated score and tries. To compose this function you may need to create other functions.
```

### Exercise 7 Finish

```js
function curry(fn, arity = fn.length) {
  return (function nextCurried(prevArgs) {
    return function curried(nextArg) {
      var args = [...prevArgs, nextArg];
      if (args.length >= arity) {
        return fn(...args);
      } else {
        return nextCurried(args);
      }
    };
  })([]);
}

const pipe = function(...fns) {
  return function(x) {
    return fns.reduce(function(v, f) {
      return f(v);
    }, x);
  };
};

const compose = function(...fns) {
  return function(x) {
    return fns.reduceRight(function(v, f) {
      return f(v);
    }, x);
  };
};

/// ******* APP **********

const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  return arr.map(function(val) {
    if (val.name.toLowerCase() === user.name.toLowerCase()) {
      return user;
    } else {
      return val;
    }
  });

  /*for (let i = 0; i < arr.length; i++) {
        if (arr[i].name.toLowerCase() === user.name.toLowerCase()) {
            arr[i] = user;
            break;
        }
    }*/
};

//Pure Functions
const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

var getUser = function(arr, name) {
  return arr.reduce(function(obj, val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      return val;
    } else {
      return obj;
    }
  }, null);

  /*for (let i = 0; i < arr.length; i++) {
        if (arr[i].name.toLowerCase() === name.toLowerCase()) {
            return arr[i];
        }
    }
    return null;*/
};

var updateScore = function(user, newAmt) {
  if (user) {
    user.score += newAmt;
    return user;
  }
};

var updateTries = function(user) {
  if (user) {
    user.tries++;
    return user;
  }
};

/*const usr = getUser(users, "Henry");
const usr1 = updateScore(cloneObj(usr), 30);
const usr2 = updateTries(cloneObj(usr1));
const newArray = storeUser(users, usr2);*/

//Using currying and composition create a specialized function that always acts on the users array but allows you to enter a user name. Have it  return a clone of that user.

const getUsersUser = pipe(curry(getUser)(users), cloneObj);

const getHenry = function() {
  return getUsersUser("Henry");
};

//Using your curried function, compose a new specialized function that will be used to update Henry. (Only invoked if you want to update Henry). It should accepts a new score and then return a new array that contains the updated score and tries. To compose this function you may need to create other functions.

const updateHenry = pipe(
  curry(updateScore)(getHenry()),
  cloneObj,
  updateTries,
  curry(storeUser)(users)
);

console.log(getUsersUser("James"));
/*

Info: Start process (4:05:36 AM)
{ name: 'James', score: 30, tries: 1 }
Info: End process (4:05:36 AM)

*/

console.log(updateHenry(100));
console.log(users);

/*

Info: Start process (4:06:53 AM)
{ name: 'James', score: 30, tries: 1 }
[
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 180, tries: 4 }
]
[
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 80, tries: 3 }
]
Info: End process (4:06:54 AM)

*/
```

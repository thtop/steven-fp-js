## Avoiding Shared State and Mutations

### What state is?

> A program is considered stateful if it is designed to remember data from events or user interactions. The remembered information is called the state of program.

> A JavaScript program stores data in variables and objects. The contents of these storage locations at any given moment while the program is running is considered its state.

### Shared State

> **Shared state** is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes. A shared scope can include global scope or closure scopes.

**by Eric Elliott**

### Avoiding Mutable Data

- Data is mutable if it can be changed after has been created.
- If it can't changed effort has been created so in functional programming we are striving for immutablillty.
- Advantage
  - more predictable
  - easier to reason about
  - mutable data can have side effects.

```js
// num is immutable
const num = 50;
num = 100; // TypeError: Assignment to constant variable.

// Objects in JavaScript are mutable
// meaning they can be changed.
const arr = [3, 4, 2, 5, 1, 6];

console.log("ORIGINAL: ", arr);

arr.sort();

console.log("UPDATED: ", arr);

/*
Info: Start process (1:55:25 PM)
ORIGINAL:  [ 3, 4, 2, 5, 1, 6 ]
UPDATED:  [ 1, 2, 3, 4, 5, 6 ]
Info: End process (1:55:25 PM)
*/
```

```js
// create funciton
// objects pass by reference

const arr = [3, 4, 2, 5, 1, 6];

const sortArray = function(arr1) {
  return arr1.sort();
};

const newNums = sortArray(arr);

console.log("ORIGINAL: ", arr);
console.log("UPDATED: ", newNums);

/*
Info: Start process (1:59:26 PM)
ORIGINAL:  [ 1, 2, 3, 4, 5, 6 ]
UPDATED:  [ 1, 2, 3, 4, 5, 6 ]
Info: End process (1:59:26 PM)
*/
```

**Object.freeze()**

```js
"use strict";

const arr = [3, 4, 2, 5, 1, 6];
Object.freeze(arr);

const sortArray = function(arr1) {
  return arr1.sort(); // TypeError: Cannot assign to read only property '0' of object
};

const newNums = sortArray(arr);

console.log("ORIGINAL: ", arr);
console.log("UPDATED: ", newNums);
```

### Cloning Object

```js
let obj = {
  firstName: "Steven",
  lastName: "Hancock",
  score: 85,
  completion: true
};

let obj2 = Object.assign({}, obj);

obj2.score = 90;

console.log("ORIGINAL:", obj);
console.log("UPDATED: ", obj2);

/*

Info: Start process (2:43:05 PM)
ORIGINAL: {
  firstName: 'Steven',
  lastName: 'Hancock',
  score: 85,
  completion: true
}
UPDATED:  {
  firstName: 'Steven',
  lastName: 'Hancock',
  score: 90,
  completion: true
}
Info: End process (2:43:05 PM)

*/
```

**Object inside object** (Object.assign doesn't clone)

```js
let obj = {
  firstName: "Steven",
  lastName: "Hancock",
  score: 85,
  completion: true,
  questions: {
    q1: { success: true, value: 1 },
    q2: { success: false, value: 2 }
  }
};

let obj2 = Object.assign({}, obj);

obj2.score = 90;
obj2.questions.q1.value = 5;

console.log("ORIGINAL:", obj);
console.log("UPDATED: ", obj2);

/*

Info: Start process (2:45:26 PM)
ORIGINAL: {
  firstName: 'Steven',
  lastName: 'Hancock',
  score: 85,
  completion: true,
  questions: { q1: { success: true, value: 5 }, q2: { success: false, value: 2 } }
}
UPDATED:  {
  firstName: 'Steven',
  lastName: 'Hancock',
  score: 90,
  completion: true,
  questions: { q1: { success: true, value: 5 }, q2: { success: false, value: 2 } }
}
Info: End process (2:45:26 PM)

*/
```

**JSON**

```js
let obj = {
  firstName: "Steven",
  lastName: "Hancock",
  score: 85,
  completion: true,
  questions: {
    q1: { success: true, value: 1 },
    q2: { success: false, value: 2 }
  }
};

let obj3 = JSON.parse(JSON.stringify(obj));

obj3.questions.q1.value = 5;

console.log("ORIGINAL:", obj);
console.log("UPDATED: ", obj3);

/*

Info: Start process (2:49:29 PM)
ORIGINAL: {
  firstName: 'Steven',
  lastName: 'Hancock',
  score: 85,
  completion: true,
  questions: { q1: { success: true, value: 1 }, q2: { success: false, value: 2 } }
}
UPDATED:  {
  firstName: 'Steven',
  lastName: 'Hancock',
  score: 85,
  completion: true,
  questions: { q1: { success: true, value: 5 }, q2: { success: false, value: 2 } }
}
Info: End process (2:49:29 PM)

*/
```

**Array**

```js
let array = [{ a: 1 }, { b: 2 }];

let obj2 = Object.assign({}, array);

let obj3 = JSON.parse(JSON.stringify(array));

array[0].a = 5;

console.log("ORIGINAL:", array);
console.log("UPDATED Object.assign: ", obj2);
console.log("UPDATED JSON: ", obj3);

/*

Info: Start process (2:53:14 PM)
ORIGINAL: [ { a: 5 }, { b: 2 } ]
UPDATED Object.assign:  { '0': { a: 5 }, '1': { b: 2 } }
UPDATED JSON:  [ { a: 1 }, { b: 2 } ]
Info: End process (2:53:15 PM)

*/
```

### Exercise sort()

```js
"use strict";

const arr = [3, 4, 2, 5, 1, 6];

const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

const newNums = cloneObj(arr).sort();

console.log("ORIGINAL: ", arr);
console.log("UPDATED: ", newNums);

/*

Info: Start process (2:58:37 PM)
ORIGINAL:  [ 3, 4, 2, 5, 1, 6 ]
UPDATED:  [ 1, 2, 3, 4, 5, 6 ]
Info: End process (2:58:37 PM)

*/
```

### Exercise 2 Start

- 2A

```js
var users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].name.toLowerCase() === user.name.toLowerCase()) {
      arr[i] = user;
      break;
    }
  }
};

//Pure Functions
var getUser = function(arr, name) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].name.toLowerCase() === name.toLowerCase()) {
      return arr[i];
    }
  }
  return null;
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

let usr = getUser(users, "Henry");
let usr1 = updateScore(usr, 30);
let usr2 = updateTries(usr1);
//storeUser(usr2);
```

- 2B

```js
var users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

var newScore = function(arr, name, amt) {
  arr.forEach(function(val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      val.score = val.score + amt;
    }
  });
  return arr;
};

var newTries = function(arr, name) {
  arr.forEach(function(val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      val.tries++;
    }
  });
  return arr;
};

var newArray1 = newScore(users, "Henry", 30);
var newArray2 = newTries(users, "Henry");
```

### Exercise 2 Finish

- 2A: Answer

```js
const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].name.toLowerCase() === user.name.toLowerCase()) {
      arr[i] = user;
      break;
    }
  }
};

//Pure Functions
const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

var getUser = function(arr, name) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].name.toLowerCase() === name.toLowerCase()) {
      return arr[i];
    }
  }
  return null;
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

let usr = getUser(users, "Henry");
let usr1 = updateScore(cloneObj(usr), 30);
let usr2 = updateTries(cloneObj(usr1));
storeUser(users, usr2);

console.log("ORIGINAL users: ", usr);
console.log("ORIGINAL usr : ", usr);
console.log("UPDATED usr1: ", usr1);
console.log("UPDATED usr2: ", usr2);

/*

Info: Start process (3:40:29 PM)
ORIGINAL users:  { name: 'Henry', score: 80, tries: 3 }
ORIGINAL usr :  { name: 'Henry', score: 80, tries: 3 }
UPDATED usr1:  { name: 'Henry', score: 110, tries: 3 }
UPDATED usr2:  { name: 'Henry', score: 110, tries: 4 }
Info: End process (3:40:29 PM)

*/
```

- 2B: Answer

```js
const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

var newScore = function(arr, name, amt) {
  arr.forEach(function(val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      val.score = val.score + amt;
    }
  });
  return arr;
};

var newTries = function(arr, name) {
  arr.forEach(function(val) {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      val.tries++;
    }
  });
  return arr;
};

const newArray1 = newScore(cloneObj(users), "Henry", 30);
const newArray2 = newTries(cloneObj(newArray1), "Henry");

console.log("ORIGINAL", users);
console.log("UPDATED newArray1", newArray1);
console.log("UPDATED newArray2", newArray2);

/*

Info: Start process (3:48:28 PM)
ORIGINAL [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 80, tries: 3 }
]
UPDATED newArray1 [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 110, tries: 3 }
]
UPDATED newArray2 [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 110, tries: 4 }
]
Info: End process (3:48:28 PM)

*/
```

### Using Reduce, Map and Filter

- `reduce` and `reduceRight`: combines the elements of an array using the funciton you specify.
- `map`: passes each element of the array to the function you provided and returns a new array that consists of the values returned by that funciton.
- `filter`: returns a new array that is a subset of the existing array.

```js
let arr = [1, 2, 3, 4, 5];

// reduce
let total = arr.reduce(function(accumulator, element) {
  return accumulator + element;
}, 0);

// console.log(total); // 15

// map
let newArray = arr.map(function(val) {
  return val ** 2;
});

let newArray2 = arr.map(function(val, index, array) {
  console.log(val);
  console.log(index);
  console.log(array);
  return val ** 2;
});

// console.log(newArray); // [ 1, 4, 9, 16, 25 ]

// filter
let filterArray = arr.filter(function(val) {
  return val < 3;
});

// console.log("ORIGINAL: ", arr); // ORIGINAL:  [ 1, 2, 3, 4, 5 ]
// console.log(filterArray); // [ 1, 2 ]
```

### Exercise 3 Start

```js
const scores = [50, 6, 100, 0, 10, 75, 8, 60, 90, 80, 0, 30, 110];

//Any scores that are below 10 needs to be multiplied by 10 and the new value included.

//Remove any scores that are over 100.

//Remove any scores that are 0 or below.

//Sum the scores.

//Provide a count for the number of scores still remaining.
```

### Exercise 3 Finish

```js
const scores = [50, 6, 100, 0, 10, 75, 8, 60, 90, 80, 0, 30, 110];

//Any scores that are below 10 needs to be multiplied by 10 and the new value included.
const boostSingleScores = scores.map(val => {
  return val < 10 ? val * 10 : val;
});
// console.log("boostSingleScores: ", boostSingleScores);
/*
Info: Start process (4:16:57 PM)
boostSingleScores:  [
   50, 60, 100,  0, 10, 75,
   80, 60,  90, 80,  0, 30,
  110
]
Info: End process (4:16:57 PM)
*/

//Remove any scores that are over 100.
const filterScoresOver100 = scores.filter(score => score <= 100);
// console.log("filterScoresOver100: ", filterScoresOver100);
/*
Info: Start process (4:20:10 PM)
filterScoresOver100:  [
  50,  6, 100,  0, 10,
  75,  8,  60, 90, 80,
   0, 30
]
Info: End process (4:20:10 PM)
*/

//Remove any scores that are 0 or below.
const filterScores0orBelow = scores.filter(score => score > 0);
// console.log("filterScores0orBelow: ", filterScores0orBelow);
/*
Info: Start process (4:12:05 PM)
filterScores0orBelow:  [
   50,  6, 100, 10, 75,
    8, 60,  90, 80, 30,
  110
]
Info: End process (4:12:06 PM)
*/

//Sum the scores.
const sumScores = scores.reduce((acc, element) => {
  return acc + element;
}, 0);
// console.log("sumScores: ", sumScores);
/*
Info: Start process (4:13:57 PM)
sumScores:  619
Info: End process (4:13:58 PM)
*/

//Provide a count for the number of scores still remaining.
const countScores = scores.reduce((count, element) => {
  return count + 1;
}, 0);
console.log("countScores: ", countScores);
/*
Info: Start process (4:24:57 PM)
countScores:  13
Info: End process (4:24:57 PM)
*/
```

### Exercise 4 Start

```js
const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].name.toLowerCase() === user.name.toLowerCase()) {
      arr[i] = user;
      break;
    }
  }
};

//Pure Functions
const cloneObj = function(obj) {
  return JSON.parse(JSON.stringify(obj));
};

var getUser = function(arr, name) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].name.toLowerCase() === name.toLowerCase()) {
      return arr[i];
    }
  }
  return null;
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
storeUser(users, usr2);
```

### Exercise 4 Finish

```js
const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  return arr.map(val => {
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
  return arr.reduce((obj, val) => {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      return val;
    } else {
      return obj;
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

console.log("ORIGINAL users: ", users);
console.log("UPDATED usr: ", usr);
console.log("UPDATED usr1: ", usr1);
console.log("UPDATED usr2: ", usr2);
console.log("UPDATED newArray: ", newArray);

/*

Info: Start process (4:36:15 PM)
ORIGINAL users:  [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 80, tries: 3 }
]
UPDATED usr:  { name: 'Henry', score: 80, tries: 3 }
UPDATED usr1:  { name: 'Henry', score: 110, tries: 3 }
UPDATED usr2:  { name: 'Henry', score: 110, tries: 4 }
UPDATED newArray:  [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 110, tries: 4 }
]
Info: End process (4:36:15 PM)

*/
```

### Exercise 4 Follow Up

```js
const users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

//Modifies Data
var storeUser = function(arr, user) {
  return arr.map(val => {
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
  return arr.reduce((obj, val) => {
    if (val.name.toLowerCase() === name.toLowerCase()) {
      return val;
    } else {
      return obj;
    }
  }, null);
};

// not pure
var updateScore = function(user, newAmt) {
  if (user) {
    user.score += newAmt;
    return user;
  }
};

// not pure
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

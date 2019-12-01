## Side Effects and Pure Functions

### Avoiding Side Effects and Using Pure Functions

```js
// side effects
let cnt = 0;

let increment = function() {
  cnt++;
  return cnt;
};

// side effects
var x = 1;

fun1();

console.log(x);

func2();

console.log(x);

func3();

console.log(x);

// side effects
var MAINAPP = (function(nsp) {
  var currentUser = 0,
    users = [
      { name: "James", score: 30, tries: 1 },
      { name: "Mary", score: 110, tries: 4 },
      { name: "Henry", score: 80, tries: 3 }
    ];

  var updateScore = function(newAmt) {
    users[currentUser].score += newAmt;
  };

  var returnUsers = function() {
    return users;
  };

  var updateTries = function() {
    users[currentUser].tries++;
  };

  var updateUser = function(newUser) {
    currentUser = newUser;
  };

  nsp.updateUser = updateUser;
  nsp.updateTries = updateTries;
  nsp.updateScore = updateScore;
  nsp.returnUsers = returnUsers;
  return nsp;
})(MAINAPP || {});

setTimeout(function() {
  MAINAPP.updateUser(2);
}, 300);
setTimeout(function() {
  MAINAPP.updateScore(20);
}, 100);
setTimeout(function() {
  MAINAPP.updateTries();
}, 200);
```

```js
// no side effects
// let cnt = 0;

// let increment = function() {
//   cnt++;
//   return cnt;
// };

// pure function
let increment = function(num) {
  return num + 1;
};

let average = function(scores) {
  let total = 0;

  for (let i = 0; i < scores.length; i++) {
    total += scores[i];
  }

  return total / scores.length;
};

const result = average([90, 30, 40, 50, 60]);
console.log(result); // 54
```

### What are Pure Functions?

- The function depends on the input provided and not on extenrnal data that changes.
- The function doesn't cause side effects. It doesn't cause change beyond its scope.
- Given the same input, the funciton will always return the same output.

### Side Effects

- Changing a value globally (variable, property or data structure).
- Changing the original value of a functions argument.
- Throwing an exception.
- Printing to the Screen or Logging.
- Triggering an external process.
- Invoking other funcitons that have side-effects.

### Excercise 1 Start

```js
// not pure
var currentUser = 0;

var users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

var updateScore = function(newAmt) {
  users[currentUser].score += newAmt;
};

var returnUsers = function() {
  return users;
};

var updateTries = function() {
  users[currentUser].tries++;
};

var updateUser = function(newUser) {
  currentUser = newUser;
};
```

### Ecercise 1 Funish

```js
// making pure functions

var users = [
  { name: "James", score: 30, tries: 1 },
  { name: "Mary", score: 110, tries: 4 },
  { name: "Henry", score: 80, tries: 3 }
];

// Modifies Data
var storeUser = function(arr, user) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].name.toLowerCase() === user.name.toLowerCase()) {
      arr[i] = user;
      break;
    }
  }
};

// Pure Functions
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
storeUser(usr2);

console.log(users);

/*
Info: Start process (12:54:12 PM)
[
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 110, tries: 4 }
]
Info: End process (12:54:12 PM)
*/

console.log(usr);
// { name: 'Henry', score: 110, tries: 4 }
```

### Exercise 1 Follow Up: Other Possible Solutions and the Problems

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

console.log("users: ", users);
console.log("newArray1: ", newArray1);
console.log("newArray2", newArray2);

/*
Info: Start process (1:19:51 PM)
users:  [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 110, tries: 4 }
]
newArray1:  [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 110, tries: 4 }
]
newArray2 [
  { name: 'James', score: 30, tries: 1 },
  { name: 'Mary', score: 110, tries: 4 },
  { name: 'Henry', score: 110, tries: 4 }
]
Info: End process (1:19:51 PM)
*/
```

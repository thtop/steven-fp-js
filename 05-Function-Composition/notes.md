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

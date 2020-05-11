# ECMAScript Fundamentals

[ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) State of the Union


<!-- .slide: class="left" -->
## What is ECMAScript (ES)

* Basis of JavaScript
* Standardized by [ECMA International](http://www.ecma-international.org/)
* ECMAScript 3 (Dec. 1999)
  * Supported in practically all browsers
* ECMAScript 5 (Dec. 2009)
  * Very good supported in all major browsers ([compat. table](http://kangax.github.io/compat-table/es5/))
  * Common basis for most web applications in the public internet
* ECMAScript 2015 aka 6 (June 2015)
  * Good support in modern browsers ([compat. table](http://kangax.github.io/compat-table/es6/))
  * Commonly assumed in controlled environments (e.g. intranet, closed groups, embedded browser)
* ECMAScript 2016+ (June 2016)
  * Partly supported in latest browsers ([compat. table](http://kangax.github.io/compat-table/es2016plus/))


<!-- .slide: class="left" -->
## Highlights of ES2015 aka ES6

* [Constants](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) (`const`)
* [Block-scoped-variables](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) (`let`) and [-functions](http://es6-features.org/#BlockScopedFunctions)

```
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // different variable
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```


<!-- .slide: class="left" -->
## Highlights of ES2015 aka ES6 (cont.)

* [Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

```
class Polygon {
  constructor(height, width) {
    this.name = 'Polygon';
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  constructor(length) {
    super(length, length);
    this.name = 'Square';
  }
}
```


<!-- .slide: class="left" -->
## Highlights of ES2015 aka ES6 (cont.)

* [`for...of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)

```
let iterable = [10, 20, 30];

for (const value of iterable) {
  console.log(value);
}

for (let value of iterable) {
  value += 1;
  console.log(value);
}
```


<!-- .slide: class="left" -->
## Highlights of ES2015 aka ES6 (cont.)

* [Arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
* Function parameters
  * [Default parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
  * [Rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
* [Template literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
* [Destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
* [Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
* [Keyed collections](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#Keyed_collections) [`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) and [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
* [`Object.assign`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) (see next slides)


<!-- .slide: class="left" -->
## "Callback hell"

```
function slowDiv(x, y, resultCallback) {
  if (!y) {
    callback('Div by zero', null);
  } else {
    // Simulate long-running calculation by waiting 25ms
    setTimeout(() => resultCallback(null, x / y), 25);
  }
}

// Calculate 10 / 5 + 20 / 10 without printing errors
slowDiv(10, 5, (err, res1) => {
  if (!err) {
    slowDiv(20, 10, (err, res2) => {
      if (!err) {
        console.log(res1 + res2);
      }
    });
  }
})
```


<!-- .slide: class="left" -->
## Promises

```
function slowDiv(x, y) {
  return new Promise((resolve, reject) => {
    if (!y) {
      reject('Div by zero');
    } else {
      // Simulate long-running calculation by waiting 25ms
      setTimeout(() => resolve(x / y), 25);
    }
  });
}

// Calculate 10 / 5 + 20 / 10 without printing errors
slowDiv(10, 5)
  .then(res1 => slowDiv(20, 10)
  .then(res2 => console.log(res1 + res2)))
  .finally(() => console.log('done')); // since ES2018
```


<!-- .slide: class="left" -->
## Highlight of ES2016+

* [`Array.prototype.includes`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
* Exponentiation Operator `**`
* String padding (`padStart`, `padEnd`)
* [`Array.prototype.flat`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)
* [`Array.prototype.flatMap`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)


<!-- .slide: class="left" -->
## Highlight of ES2016+

* Trailing commas (no more comma at the start of the line)
* See also [*Rest parameters*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)

```
function sum(...theArgs) {
    return theArgs.reduce((previous, current) => previous + current);
}

const result = sum(1 * 2 ** 5,
    0 * 2 ** 4,
    1 * 2 ** 3,
    0 * 2 ** 2,
    1 * 2 ** 1,
    0 * 2 ** 0,);
console.log(result);
```


<!-- .slide: class="left" -->
## Highlight of ES2016+

* [Async functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) with `async/await` similar to C# (will be covered later in more details)

```
function slowDiv(x, y) {
  return new Promise((resolve, reject) => {
    if (!y) {
      reject('Div by zero');
    } else {
      // Simulate long-running calculation by waiting 25ms
      setTimeout(() => resolve(x / y), 25);
    }
  });
}

async function run() {
  // Calculate 10 / 5 + 20 / 10 without printing errors
  console.log(await slowDiv(10, 5) + await slowDiv(20, 10));
}

run();
```


<!-- .slide: class="left" -->
## Highlight of ES2016+

* [Asynchronous iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for-await...of) with `for await ... of`
* Also note use of [generator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)

```
function delay(ms) {
    return new Promise((res) => { setTimeout(() => res(), ms); });
}

const asyncIterable = {
    async*[Symbol.asyncIterator]() {
        for (let i = 0; i < 42; i++) {
            await delay(100);
            yield 1;
        }
    }
};

(async () => {
    let result = 0;
    for await (let num of asyncIterable) {
        result += num;
        console.log(`Running sum: ${result}`);
    }
})();
```


<!-- .slide: class="left" -->
## [babel](https://babeljs.io/)

* "Transpiler"
* Transforms modern JavaScript so that you can run it in old environments
* Especially important for building consumer-facing websites
* Huge ecosystems of [plugins](https://babeljs.io/docs/plugins/)
* Can handle *.ts* files, too [*babel-preset-typescript*](https://babeljs.io/docs/en/next/babel-preset-typescript.html)
* [*babel* demo](https://github.com/rstropek/ts-angular-workshop/blob/master/ecmascript-fundamentals/0030-babel/readme.md)


<!-- .slide: class="left" -->
## Takeaways

* ECMAScript/JavaScript is *no trivial scripting language* anymore
* Powerful, modern programming language
* Standard is now moving faster than in the past
* Use [*babel*](https://babeljs.io/) to enable modern JavaScript in all projects
* What is missing? Types!

> Enter: TypeScript


<!-- .slide: class="left" -->
## Further Readings and Exercises

* Want to know more? Read/watch...
  * [Learn JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) in *MDN web docs*
  * [w3schools.com JavaScript Tutorial](https://www.w3schools.com/js/)
  * [What Is AMD, CommonJS, and UMD?](http://davidbcalhoun.com/2014/what-is-amd-commonjs-and-umd/)
* Exercises
  * [*Promise exercise*](https://github.com/rstropek/ts-angular-workshop/blob/master/ecmascript-fundamentals/9010-create-promise/readme.md)
  * [*babel* demo](https://github.com/rstropek/ts-angular-workshop/blob/master/ecmascript-fundamentals/0030-babel/readme.md)
  * [w3schools.com JavaScript Quiz](https://www.w3schools.com/quiztest/quiztest.asp?qtest=JavaScript)
  * [*Battleship exercise*](https://github.com/rstropek/ts-angular-workshop/blob/master/ecmascript-fundamentals/9020-battleship/readme.md)

<h1 align="center">JavaScript-Principles</p>

## Destructuring
```js
var tmp = [
  {
    name: "vlad",
    email: "vlados@gmail.com"
  },
  {
    name: "vlad",
    email: "vlados@gmail.com"
  }
];

// Old version
var first = tmp[0];
var second = tmp[1];
var firstName = first.name;
var firstEmail = (first.email !== undefined) ? first.email: "nobody@none.tld";
var secondName = second.name;
var secondEmail = (second.email !== undefined) ? second.email : "nobody@none.tld";

// With destructuring
var [
    {
        name: firstName,
        email: firstEmail = "nobody@none.tld"
    },
    {
        name: secondName,
        email: secondEmail = "nobody@none.tld"
    }
] = tmp;

console.log(firstName);
```

## Promises & Microtask Queue
Based on docs Promises are going to Microtask Queue, setTimeout to Callback Queue. Microtask has more priority over Callback queue and gets into call stack faster
```js
function display(data){console.log(data)}
function printHello(){console.log("Hello")}
function blockFor300ms(){/* blocks js thread for 300ms with long for loop */}

setTimeout(printHello, 0);

const futureData = fetch('https://twitter.com/will/tweets/1');
futureData.then(display);

blockFor300ms()

// Which will run first?

console.log("Me first!");
```

## Callback vs. Higher-order function
The function we pass in is a callback function. The outer function that takes in the function (our callback) is a higher-order function
```js
// Example 1
const copyArrayAndManipulate = (array, instructions) => {
    const output = [];
    for (let i = 0; i < array.length; i++) {
        output.push(instructions(array[i]));
    }
    return output;
}

const multiplyBy2(input) => {
    return input * 2;
}

const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
```
```js
// Example 2
function unary(fn) {
  return function one(arg) {
    return fn(arg);
  }
}

function binary(fn) {
  return function two(arg1, arg2) {
    return fn(arg1, arg2);
  }
}

function f(...args) {
  return args;
}

const result1 = unary(f); // [1]
const result2 = binary(f); // [1,2]

```

## Function copmosition
— **Easier to add features** - This is the _essential_ aspect of functional javascript - being able to list of our units of code by name and have them run one by one as independent, self-contained pieces<br />
— **More readable** - reduce here is often wrapped in compose to say ‘combine up’ the functions to run our data through them one by one. The style is ‘point free’<br />
— **Easier to debug** - I know exactly the line of code my bug is in - it’s got a label
```js
// Example 1
const multiplyBy2 = x => x*2 
const add3 = x => x+3 
const divideBy5 = x => x/5
const subtract4 = x => x-4

const reduce = (array, howToCombine, buildingUp) => {
    for (let i = 0; i < array.length; i++){
        buildingUp = howToCombine(buildingUp, array[i])
    }
    return buildingUp
}

const runFunctionOnInput = (input,fn) => { return fn(input) }

const output = reduce([
        multiplyBy2,
        add3,
        divideBy5,
        subtract4
    ],
    runFunctionOnInput, 11 
)
```
```js
// Example 2
const multiplyBy2 = x => x * 2
const add3 = x => x + 3
const divideBy5 = x => x / 5
const subtract4 = x => x - 4

const reduce = (buildingUp, fns) => {
    for (let fn of fns) {
        buildingUp = fn(buildingUp);
    }
    return buildingUp
}

const output = reduce(11, [
    multiplyBy2,
    add3,
    divideBy5,
    subtract4
])
console.log(output);
```
```js
// Example 3
const multiplyBy2 = x => x * 2
const add3 = x => x + 3
const divideBy5 = x => x / 5
const subtract4 = x => x - 4

function compose(...fns) {
  return function(v) {
    return fns.reduce((val, fn) => {
      return fn(val); 
    }, v);
  }
}

const output = compose(multiplyBy2, add3, divideBy5, subtract4)(6);
console.log(output);
```

## Closure
The Closed over Variable Environment (COVE) or ‘Closure’<br>
This ‘backpack’ of live data that gets returned out with incrementCounter is known as the ‘closure’.<br>
The ‘backpack’ (or ‘closure’) of live data is attached incrementCounter (then to myNewFunction) through a hidden property known as [[scope]] which persists when the inner function is returned ou
```js
const outer = () => {
    let counter = 0;
    const incrementCounter = () => {
        counter++;
    }
    return incrementCounter
}

const newFunction = outer();
newFunction() 
newFunction()
```

## Function decoration
To add a permanent memory to an existing function we have to create a new function that will run the existing function inside of itself
```js
const oncify = (convertMe) => {
    let counter = 0
    const inner = (input) => {
        if (counter === 0) {
            const output = convertMe(input) 
        counter++
            return output
        }
        return "Sorry"
    }
    return inner
}

const multiplyBy2 = num => num * 2

const oncifiedMultiplyBy2 = oncify(multiplyBy2)

oncifiedMultiplyBy2(10) // 20 
oncifiedMultiplyBy2(7) // Sorry
```

## Partial application
— In practice we may have to prefill one, two... multiple arguments at different times<br/>
— We can convert (‘decorate’) any function to a function that will accept arguments one by one and only run the function in full once it has all the argument
```js
const multiply = (a, b) => a * b

function prefillFunction(fn, prefilledValue) {
    const inner = (liveInput) => {
        const output = fn(liveInput, prefilledValue) 
    return output
    }
    return inner
}

const multiplyBy2 = prefillFunction(multiply, 2)

const result = multiplyBy2(5) // 10
```

## Currying
```js
const multiply = (a, b) => a * b

function prefillFunction(fn, prefilledValue) {
    const inner = (liveInput) => {
        const output = fn(liveInput, prefilledValue) 
    return output
    }
    return inner
}

const multiplyBy2 = prefillFunction(multiply, 2)(5) // 10
```

## Memoization
Memoization used to store function result of previous call and return result of previous call on the next call. It's useful when result the same everytime
```js
function repeater(count) {
    return memoize(function allTheAs() {
        return "".padStart(count, "A");
    });
}

vat A = repeater(10);
A(); // "AAAAAAAAAA"
A(); // "AAAAAAAAAA"
```

## Programming
<details>
<summary>Example of Microtask Queue, Callback Queue and which is faster:</summary>

```js
function display(data){console.log(data)}
function printHello(){console.log("Hello")}
function blockFor300ms(){/* blocks js thread for 300ms with long for loop */}

setTimeout(printHello, 0);

const futureData = fetch('https://twitter.com/will/tweets/1');
futureData.then(display);

blockFor300ms()

// Which will run first?

console.log("Me first!");
```
</details>

<details>
<summary>Async/await using generators:</summary>

```js
function doWhenDataReceived(value) {
    returnNextElement.next(value)
}

function* createFlow() {
    const data = yield fetch('http://twitter.com/will/tweets/1') console.log(data)
}

const returnNextElement = createFlow()
const futureData = returnNextElement.next()

futureData.then(doWhenDataReceived)
```
</details>

<details>
<summary>Factorial</summary>

```js
const factorial = (n) => n <= 1 ? n : n*factorial(n-1);

factorial(5); // 20
```
</details>

<details>
<summary>Infinite curry (https://theanubhav.com/2019/02/03/js-currying-in-interview/)</summary>

```js
function curry(fn, totalArgs) {
  totalArgs = totalArgs || fn.length
      return function recursor(){
          return arguments.length < totalArgs
            ? recursor.bind(this, ...arguments)
            : fn.call(this, ...arguments);
      }
}

const sum = (a,b) => a + b;
const inc = curry(sum)(1);

console.log("Output1: ", inc(2)) // 3
console.log("Output2: ", inc()(1)) // 2
console.log("Output3: ", curry(sum)(1,2)) // 3
```
</details>
  
<details>
<summary>Check is array with uniq objects</summary>

```js
function isUniq(array) {
  const arrayOfValues = array.reduce((acc, field) => {
      const sortedKeys = Object.keys(field).sort();
      const sortedField = sortedKeys.reduce((_acc, key) => ({..._acc, [key]: field[key]}), {});

      return [...acc, Object.values(sortedField)];
  }, []);

  const arrayOfStringValues = arrayOfValues.map((value) => value.join(''));
  const arrayWithoutDuplicates = [...new Set(arrayOfStringValues)];

  return arrayWithoutDuplicates.length === array.length;
}
```
</details>
 
<details>
<summary>Check is array with uniq objects (dynamic keys)</summary>

```js
function isUniq(array) {
  const keys = params.each
    ? Object.keys(array[0])
    : params.keys || [];  

  array.every((item, index, array) => {
      const old = array.slice(0, index);
      
      return old.every((oldItem) => {            
          const everyEqual = keys.every((uniqueKey) => oldItem[uniqueKey] === item[uniqueKey]);
          
          return !everyEqual;
      });
  });
}
```
</details>

<h1 align="center">TypeScript</p>

## Interface && Type
Type similar as interface but have different implementation
```js
// Interface && Type for functions
type Contact = (
  name: string,
  email: string,
  age: number
) => void;

interface Contact {
  (name: string, email: string, age: number): void
};

const sendInfo: Contact = (name, email, age) => {
  console.log(name);
  console.log(email);
  console.log(age);
}

sendIfno('Vlad', 'sirotkin96@gmail.com', 24);
```
```js
// Extending interfaces
interface Age { age: number };
interface Email { email: string };
interface Person extends Email, Age { name: string; }

const info: Person = {
  age: 24,
  name: 'Vlad',
  email: 'sirotkin96@gmail.com'
}
```
## Generics
Generics used to throw type to interface
Naming convention: T
```js
interface Person<T> {
  name: T;
  age: number;
}

const obj: Person<string> = {
  name: 'Vladislav',
  age: 24
};
```
```js
// How to pass default value (T = any)
interface FilterFunction<T = any> {
  (val: T): boolean;
}

const result: FilterFunction<string> = val => typeof val === 'string';
result(0); // Error
result('Vlad'); // Good
```
```js
function startTuple<T>(str: T) {
  return function finishTuple<U>(num: U) {
    return [str, num] as [T, U];
  }
}
const result = startTuple(["first"])(42); // [string[], number]
```

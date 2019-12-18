<h3 align="center">JavaScript-Principles</p>

## Promises & Microtask Queue
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
## Function copmosition
— **Easier to add features** - This is the _essential_ aspect of functional javascript - being able to list of our units of code by name and have them run one by one as independent, self-contained pieces<br />
— **More readable** - reduce here is often wrapped in compose to say ‘combine up’ the functions to run our data through them one by one. The style is ‘point free’<br />
— **Easier to debug** - I know exactly the line of code my bug is in - it’s got a label
```js
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

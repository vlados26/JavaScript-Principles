<h3 align="center">JavaScript-Principles</p>

<details>
  <summary>Click to toggle contents of `code`</summary>
  ```
  CODE!
  ```
</details>

## Callback vs. Higher-order function
The function we pass in is a callback function. The outer function that takes in the function (our callback) is a higher-order function
```
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
```
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

# Functions and Callbacks

So far we have mentioned that we use functions to store code and can take in values (called parameters) that we can use in the function.

```javascript
function multiplyBy2(inputNumber){
    const result = inputNumber * 2;
    return result;
}
```

Well we can take in more that just values in our parameters, we can also take in code as a parameter, but how would we do this?

The answer is simple, we can pass a function in as a parameter.

```javascript
function doSomethingToArray(array, operationFunction){
    newArray = [];
    for (var i = 0; i < newArray.length; i++){
        newArray.push(operationFunction(newArray[i]));
    }
    return newArray;
}

function add2(num){return num + 2; }

const result = doSomethingToArray([1,2,3], add2);

console.log(result); // prints [ 3, 4, 5 ]
```
> The code we have here is what we call a _**Higher Order Function**_.

> The function we insert as a parameter is called a _**Callback Function**_.

<h2>Arrow Functions</h2>

Arrow functions are a new way of creating new functions that was introduced in ES6. They look something like this: 

```javascript
//Standard arrow function
const multiplyBy2 = (input) => {return input * 2}

//only works when there is one parameter
const multiplyBy2 = (input) => input * 2

const multiplyBy2 = input => input * 2
```
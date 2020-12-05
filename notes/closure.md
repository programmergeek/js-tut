# Closure

Closure is key concept in javascript that allows to create function such as 'once', which is a program that will only run once and thereafter cannot be run again. This is enabled by memorization which allows our program to remember that a task has already been run.

> Closure gives our functions persistent memory.

<h2>Returning Functions</h2>
<br/>

> Javascript allows us to return a function from a function.

```javascript
function createFunction(){
    function multiplyBy2(num){
        return num * 2;
    }

    return multiplyBy2;
}

const generateFunc = createFunction();
const result = generateFunc(3); // returns 6
```


In our code we declare a constant called `generateFunc` that is initialized as the return value of the function createFunction, which is the function `multiplyBy2`. Under the hood of javascript what is happening is that the instructions of the function `multiplyBy2` is being stored in the generateFunc. By doing this the `generateFunc` no longer has any link to the `createFunction` function, so when the execution context of `createFunction` is destroyed `generateFunc` is unaffected.

When we do this we take more than just the function instructions, we also take the surrounding data.

```javascript
function outer(){
    let counter = 0;
    function incrementCounter(){return ++counter ; }
    return incrementCounter;
}

const myNewFunction = outer();
myNewFunction();
myNewFunction();
```

In the above case the function definition of `incrementCounter` would not be the only thing returned, `counter` would also be returned. We say that the function brought a backpack of data along with it.

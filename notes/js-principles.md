# Javascript Principles 

Javascripts goes through code line by line executing it. This is called the "_Thread of Execution_". When the instructions are run, the data used in the program is stored in memory.

<br/>

# Functions

We can save code for later use in something call a function, which looks something like this:

```javascript
function multiplyBy2(inputNumber){
    const result = inputNumber * 2;
    return result;
}
```

The ```function``` keyword tells javascript that we are creating a function. The thing in the brackets, ```inputNumber```, is called the parameter. This is a value we expect to use within the function. Finally we have the ```return``` keyword that tells javascript to give us the value corresponding with the identifier (in this case, ```results```). The ```return``` also indicates the end of a function. Also keep in mind that all the instructions of the function are kept inside its curly braces.

When we run a function we create a thread of execution within the function while saving data in the function to memory, i.e we create a new execution context for the function which is inside the global execution context.

> Execution context: The combination of the thread of execution and the memory used the by the program.

To put it simply, you can just think of a function as a mini program within a bigger program.

<br/>

# Call Stack

> A mechanism that javascript uses to track which functions are running and they order the should run in.

The call stack orders our function calls in way that the first function we call is the last to be executed where is the last is the first executed. The base function in the call stack is the global function i.e the program itself.

![Call Stack](https://miro.medium.com/max/2478/1*rJ2sh-q1deQGGGVG5gYyIQ.png)


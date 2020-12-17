# Asynchronous Javascript

<h2>Overview</h2>

Javascript is actually a feature bare language. It lacks a lot of features and tools you would expect a programming language to have, like rendering, timer, etc, but that's because it doesn't need to have to those features. Javascript is normally run in a browser, which does have all these features you would expect from Javascript. Javascript has functions that interact with the browser to make use of those features when needed. Simply put, Javascript is a language that allows easy communication between you and the browser.

<h2>How It Works</h2>

As we've established Javascript offloads a lot of the functionality you would expect from a programming language onto the browser. This is one of Javascript's core features as this allows us to create programs that appear to be executing multiple lines of code at once although Javascript can only execute one line of code at a time. Under the hood, what's actually happening is that Javascript tells the browser to perform some task you've programmed in and return the result, this allows Javascript to then execute the following lines of code while waiting thus creating the illusion of running multiple line of code at once.

Example:

```Javascript
function printHello(){
    console.log("Hello");
}

setTimeout(printHello, 1000);

console.log("Me first");

//output:
//Me first
//Hello
```

In the above we've used the function `setTimeout` which does something, in this case call the function `printHello`, after a duration of 1000ms. The `setTimeout` function is actually offloading work onto the browser as Javascript does not have a timer. Because the browser is taking care of the timer function we can then move on to execute the console function, which is also executed by the browser, resulting in "Me first" being output before "Hello" although the console function was lower in the thread of execution.

<h2>Callback Queue and Event Loop</h2>

Working with the browser introduces some unpredictability to our code since different browsers will handle Javascript slightly differently and we don't want this, for this reason Javascript implements very strict rules when the browser is called. 

When we execute an instruction using the browser we would want the results returned back to our Javascript program, but how that happens could cause unforeseen errors in our code so Javascript makes use of something called an __*Callback Queue*__ that stores the results from the browser. Instructions in the __*Callback Queue*__ will only be run when all the code in the execution context and the call stack has been run and to make sure that this behavior is consistent Javascript uses an __*Event Loop*__ to check if all the conditions have been met to start executing what's in the __*Callback Queue*__.

<h2>Problems</h2>

This way of doing async programming has a major issue dubbed *__callback hell__*. Assume we are working with data from an api, the current method would force us to work with the data inside the off-loader function. We would not have the ability to return out the data from the function as this would be the last thing run in the program. This forces us to have functions within function just so we can work with the data at the expense of readability and ease of debugging. 

This issue was fixed in ES6 with the advent of *__promises__*.
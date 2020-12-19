# Promises

Promises are special objects in Javascript that act as a placeholder for the data you would expect to get back from the browser. You now have the ability to manipulate that **promise object** as you would if you had the data stored in memory.

> The **promise object** stands in place of the data you would get back from the browser.

```javascript
function display(data){
    console.log(data)
}

const futureData = fetch('https://twitter.com/will/tweets/1')
futureData.then(display)

console.log("me first")
```

This block will write "me first" to the console before displaying the twitter data but notice how we were able to assign that twitter data to the constant `futureData` using the `fetch` method. The `fetch` method is both a native javascript method as well as an off-loader method. Within Javascript the `fetch` method returns a **promise object** to the constant `futureData` and in the browser it gets the data we want from the internet. We can see what data we got using the `display` function we created. Notice how we reference the function within the `then` method, this method simply allows us to manipulate our data.

The promise object created by the `fetch` method in Javascript has 2 properties: 

* `value`, which is undefined
* `onFulfilled`, which is an empty array and is hidden

The `value` property is where the data will be returned to when Javascript gets the data.

Because of how closely linked together the `fetch` method makes Javascript and the browser, the browser can directly return data into the `value` property of the promise object as soon as it gets it. There is no need to wait for all the instructions in the thread of execution to be executed.

The `onFulfilled` property is an array that we can add functions to that automatically run using your data as soon as Javascript gets it. One small problem though, we don't have access to `onFulfilled`, its a hidden property. Thats where the `then` method comes in. The `then` method adds our functions to the `onFulfilled` array for us.

<h2>Micro-task Queue</h2>

<br/>

```javascript
function display(data){console.log(data)}
function printHello(){console.log("Hello")}
function blocFor300ms(){/* blocks js for 300ms */}

setTimeout(printHello, 0);

const futureData = fetch('https://twitter.com/will/1')
futureData.then(display)

blockFor300ms()
console.log("Me first")
```

As we've mentioned before, the fetch method returns a promise object with 2 properties `value` and `onFulfilled`. We also mentioned that the methods in the `onFulfilled` array are run as soon as the data property has been filled, by how does Javascript manage running these methods along with the other instructions in the global execution context?

If we think back to when we first talked about asynchronous Javascript we mentioned that asynchronous methods are put into the that callback queue when they are ready to be executed so it would be reasonable to assume that the same would apply with promises. So with that understanding you would assume that the program above would output something like this:

```
Me first
Hello
"Hi" 
```
\*Assuming the data we get back from twitter is the string "Hi".

This output makes sense because `printHello` would be added to the callback queue before `display`, but this is wrong. The actual output would be:

```
Me first
"Hi"
Hello
```
\*Assuming we get back the data from twitter before we execute `console.log('Me first')`.

But this doesn't make sense since `printHello` was added to the callback queue before `display`, that is assuming there is only one queue. 

We actually have another queue called the **_Micro-task Queue_**. The functions that we add to the `onFulfilled` array are added to this queue when they are ready to be run. The **_Micro-task Queue_** is given higher priority than the callback queue and so functions in there are executed before the ones in the callback queue.

<h2>Errors</h2>

Apart from the 2 properties we've talked about, promise objects have one more property called `onRejection`, an array, that stores functions that are run when value is filled with an error. 

There are 2 ways of adding functions to `onRejection`:

1. using the `catch` method
2. adding it as a second parameter in the `then` method
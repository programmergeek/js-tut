# Classes and Prototypes

As we develop our applications it becomes more and more difficult to keep track of what function is meant to be used where. For that reason we use programming paradigms. The most popular paradigm is the ***Object Oriented Programming*** (OOP) paradigm.

> A programming paradigm is the way we organize our code to make it easy to reason, add features to and be efficient and run well.

<h2>Object Dot Notation</h2>

As implied by the name the Object Oriented Programming paradigm makes use of objects, where all the data and the functionality are stored within the object. To access/add data and functionality to our Javascript object we use dot notation.

Dot notation allows us to access data or functionality:

```javascript
const user1 = {
    name: "Will",
    score: 3,
    increment: function() {user1.score++;}
}

user1.increment() // user1.score --> 4
console.log(user1.name) //prints Will
```
As well as allows us to add data or functionality

```javascript
const user2 = {};

user2.name = "Theodore";
user2.score = 420;
user2.increment = function() {
    user2.score++;
}
```

This approach to the Object Oriented Programming model appears to wok well. Our code is easy to reason, but remember there are 3 conditions we need to satisfy, we only satisfy one. The current approach would require us to manually add new functions if the need ever comes up and we violate the **Don't Repeat Yourself** (DRY) rule with the `increment` method lowering our codes efficiency. Clearly we need a new approach.

<h2>Prototype Chain</h2>

What if instead of defining new object with their own methods and values, we could just have objects containing their unique values but instead of having methods, they could have a link to the same methods. That is what the ***Prototype Chain*** is in a nutshell.

> A link in multiple objects to the same methods.

But that now brings up the question of how we could create that link. Thankfully Javascript has a function we can use for this, `Object.create(OBJECT_OF_FUNCTIONS)`.

The `Object.create(OBJECT_OF_FUNCTIONS)` function will create an empty object with a hidden property called `__proto__` that contains the link between the new object and `OBJECT_OF_FUNCTIONS`.

Example:

```javascript
function userCreator(name, score){
    const newUser = Object.create(userFunctionStore)
    newUser.name = name
    newUser.score = score
    return newUser
}

const userFunctionStore = {
    increment : function(){this.score++},
    login: function(){console.log("Logged in.")}
}

const user1 = userCreator("Will", 3)
user1.increment()  //user1.score --> 4
```

When we run the above code `userCreator` will return an object only containing values, no methods, so when we try call the method `increment` it would be easy to think that we would get an error but what actually happens is that Javascript just looks for the method in `userFunctionStore` after failing to find it in `user1`. But notice how in the increment method we don't specify `user1` but Javascript knows that we want to call the method on it. This is because of the `this` keyword which creates an ***implicit parameter*** (the parameter is filled in for use by Javascript) that tells the method on which object should the method be run on.

Something to note here is that all Javascript objects have a default value for the `__proto__` property called, ***Object Prototype***.

<h2>Quirks of "this"</h2>

```javascript
function userCreator(name, score) {
 const newUser = Object.create(userFunctionStore);
 newUser.name = name;
 newUser.score = score;
 return newUser;
};
const userFunctionStore = {
 increment: function() {
 function add1(){ this.score++; }
 add1()
 }
};
const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment(); 
```
In the above code, when we call `add1` you would expect that `this` would refer to `user1`. That assumption would be far too reasonable for Javascript, what actually happens is that `this` would look in the global memory for a label called `score` but since that label doesn't exist in the global memory you would instead get an error.

This problem is easily fixable. When we call the function `add1` we can specify what we want `this` to refer to using `.call()`, where the parameter is the object we want `this` to refer to.

```javascript
const userFunctionStore = {
 increment: function() {
 function add1(){ this.score++; }
 add1.call(this)
 }
```
\*The `this` in `add1.call(this)` would refer to the object that called the `increment` method.

This issue was addressed with the introduction of arrow functions ensuring that the `this` keyword in a subfunction referred to the `this` of the function above it.

Note: Due to the behavior of arrow functions it is not suitable to be used as methods in an object, but can be used inside an object method.

i.e this is okay
```javascript
const userFunctionStore = {
 increment: function() {
 function add1(){ this.score++; }
 add1.call(this)
 }
 ```
 This is not
 ```javascript
const userFunctionStore = {
 increment: () => {
 function add1(){ this.score++; }
 add1.call(this)
 }
 ```

<h2>The 'new' Keyword</h2>

As we can see above there is a lot of code we have to write to create our objects, but what if we could automate some of that work. That were the `new` keyword comes in. The `new` keyword automatically creates a an object for you called `this`, not to be confused with the `that` stores the name of the object calling a method, as well as creates a link for you to an object called `prototype`, which is an empty object, in its `__proto__` property. Something to note here is that:

> Functions are both objects and functions.

So when you create a function there is also an object being create and within that object there is a property called `prototype`, which is an empty object, and its this object that the `new` keyword create a `__proto__` link with.

<h2>The 'class' Keyword</h2>

The main purpose of the `class` keyword is simply to reduce the amount of typing and some confusion in creating an object, there is no change to the underlying logic of objects.

> Classes create a function-object combo.

So classes turn this:

```javascript
function userCreator(name, score){
 this.name = name;
 this.score = score;
}
userCreator.prototype.increment = function(){ this.score++; };
userCreator.prototype.login = function(){ console.log("login"); };
const user1 = new userCreator(“Eva”, 9)
user1.increment()
```
into this:
```javascript
class UserCreator {

    constructor (name, score){
        this.name = name;
        this.score = score;
    }

    increment (){ this.score++; }
    login (){ console.log("login"); }
}
const user1 = new UserCreator("Eva", 9);
user1.increment();
```

As you can see classes allow for a visual separation of working with the function part of a function and working with the object part.

The `constructor` keyword tells Javascript we are working with the function part and to work with the object part we can simply list out the functions we wish to add.
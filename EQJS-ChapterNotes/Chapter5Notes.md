# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Higher Order Functions 
Higher order functions allow developers to abstract over actions, not just values. Higher order functions encompass those such as functions that create new functions, functions that change other functions, functions that provide new types of control flow. One area where higher order functions are applicable is data processing. 

A higher order function is essentially a function that can take another function as an argument or that returns a function as a result. In javascript functions have the type of _Object_; meaning they can be assigned as the value of a variable, and they can be passed and returned. 

The strategy of passing in a function to be executed after the rest of the parent function's operations are complete is one of the basic characteristics of languages that support higher-order functions. It allows for asynchronous behavior, so a script can continue executing while waiting for a result. This asynchronous behavior is very useful in web programming environments. Consider the example below: 

```javascript
<button id="clicker">So Clickable</button>

document.getElementById("clicker").addEventListener("click", function() {
  alert("you triggered " + this.id);
});
```
The script uses an anonymous inline function to display an alert. But it could have also used a separately defined function, and passed that named function to the `addEventListener` method. Like this: 

```javascript
var proveIt = function() {
  alert("you triggered " + this.id);
};

document.getElementById("clicker").addEventListener("click", proveIt);
```

Having the ability to replace an inline function with a separately defined and named function, opens up a world of possibilities. Aiming to develop pure functions that don't alter external data, and return the same result for the same input every time, will allow for a library of small, targeted functions that can be used generically in any application. 

### Returning Functions as Results 
Defining a function as return value of another functions allows you to create functions that can be used as templates to create new functions. An example of a template function that returns a function as a result can be seen below: 

```javascript 
var attitude = function(original, replacement, source) {
  return function(source) {
    return source.replace(original, replacement);
  };
};

var snakify = attitude(/millenials/ig, "Snake People");
var hippify = attitude(/baby boomers/ig, "Aging Hippies");

console.log(snakify("The Millenials are always up to something."));
// The Snake People are always up to something.
console.log(hippify("The Baby Boomers just look the other way."));
// The Aging Hippies just look the other way.
```
Javascript functions don't care whether they are passed the same number of arguments as they were originally defined to take. If an argument is missing, the function will treat the missing arguments as undefined. 

> Being able to pass function values to other functions is a deeply useful aspect of JavaScript. It allows us to write functions that model computations with “gaps” in them. The code that calls these functions can fill in the gaps by providing function values.
> Arrays provide a number of useful higher-order methods. You can use forEach to loop over the elements in an array. The filter method returns a new array with the elements that didn’t pass the predicate function filtered out. Transforming an array by putting each element through a function is done with map. You can use reduce to combine all the elements in an array into a single value. The some method tests whether any element matches a given predicate function. And findIndex finds the position of the first element that matches a predicate.

(_EloquentJavascript_, Ch.5)

### Arrow Functions 
One of the reasons for utilizing arrow functions is shorter syntax. They are commonly seen represented in javascript in the following format: 

```javascript 
(parameters) => { statements } // if there is only one parameter you don't have to use parenthesis  
() => { statements }
```

Arrow functions also allow for `this` to be bound lexically. With arrow functions, the `this` binding keeps its original binding from the context. An in depth example of this can be seen from the example below: 

```javascript 
function Counter() {
  this.num = 0;

  this.timer = setInterval(() => {
    this.num++;
    console.log(this.num);
  }, 1000);
}

var b = new Counter();
// 1
// 2
// 3...
```

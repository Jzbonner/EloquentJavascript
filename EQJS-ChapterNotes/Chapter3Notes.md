# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Functions 
The most obvious application of functions is defining new vocabulary. Creating new words in regular, human-language prose is usually bad style. But in programming it is indispensable. 

### Defining a Function 
A function definition is just a regular variable definition where the value given to the variable happens to be a function. Example: 

```javascript 
var square = function(x) {
  return x * x;
};
console.log(square(12));
// → 144
```
Functions have a set of parameters and a body, which contains the statements that are to be executed when the function is called. The function body must always be wrapped in braces. 

A *return* statement determines the value the function returns. When control comes across such a statement, it immediately jumps out of the current function and gives the returned value to the code that called the function. The *return* keyword without an expression after it will cause the function to return undefined. 

### Parameters and Scope
The parameters to a function behave like regular variables, but their initial values are given by the caller of the function, not the code in the function itself. 

> An important property of functions is that the variables created inside of them, including their parameters, are local to the function...The "localness" of variables applies only to the parameters and to variables declared with the var keyword inside the function body. Variables declared outside of any function are called global, because they are visible throughout the program. It is possible to access such variables from inside a function, as long as you haven't declared a local variable with the same name. 

(Eloquent JavaScript, Ch.3) 

By treating function-local variables as existing only within the function, the language makes it possible to read and understand functions as small universes, without having to worry about all the code at once. 

### Nested Scope
With nested functions each local scope can also see all the local scopes that contain it. The set of variables visible inside a function is determined by the place of that function in the program text. All variables from blocks around a function's definitions are visible - meaning both those in function bodies that enclose it and those at the top level of the program. 

Lexical Scoping: Javascript starts at the innermost scope and searches outward until it finds the variable it was looking for. It is useful because you can always easily figure out what the value of a variable will be by looking at the code; whereas in dynamic scoping, the meaning of a variable can change at runtime, making it more difficult.

```javascript
var outerFunction  = function(){
   if(true){
      var x = 5;
      //console.log(y); //line 1, ReferenceError: y not defined
   }

   var nestedFunction = function() {
      if(true){
         var y = 7;
         console.log(x); //line 2, x will still be known prints 5
      }
      if(true){
         console.log(y); //line 3, prints 7
       }
   }
   return nestedFunction;
};
 
var myFunction = outerFunction();
myFunction();
```

### Function as Values 
A function value can do all things that other values can do - you can use it in arbitrary expressions, not just call it. It is possible to store a function value in a new place, pass it as an argument to a function, etc. 

### Declaration Notation
The *function* keyword can also be used at the start of a statement. In a function declaration the statement defines the variable and points it at the given function. Function declarations are not part of the regular top-to-bottom flow of control. They are conceptually moved to the top of their scope and can be used by all the code in that scope. This allows for the freedom to order code in a way that seems meaningful, without worrying about having to define all functions above their first use. 

### Call Stack 
Every time a function is called, the current context is put on top of the call stack. When the function returns, it removes the top context from the stack and uses it to continue execution. Storing this stack, however, requires space in the computer's memory. When the stack grows too big, the computer will fail with a message like "out of stack space" or "too much recursion". 

### Optional Arguments 
JavaScript is extremely broad-minded about the number of arguments you pass to a function. If you pass too many, the extra ones are ignored. If you pass too few, the missing parameters simply get assigned the value *undefined*. Example: 

```javascript 
function power(base, exponent) {
  if (exponent == undefined) {
    exponent = 2;
  }

  var result = 1;
  for (var count = 0; count < exponent; count++) {
    result *= base;
  }
    
  return result;
};

console.log(power(4)); // → 16
console.log(power(4, 3)); // → 64
```

### Closure 
Being able to reference a specific instance of local variables in an enclosing function is called *closure*. 

```javascript
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
};

var twice = multiplier(2);
console.log(twice(5)); // → 10
```

> Thinking about programs like this takes some practice. A good mental model is to think of the *function* keyword as "freezing" the code in its body and wrapping it into a package (the function value). So when you read *return function(...){...}*, think of it as returning a handle to a piece of computation, frozen for later use. 

(Eloquent JavaScript, Ch.3)

### Recursion 
A function that calls itself is called *recursive*. However, recursion requires far more time than looping; "Running through a simple loop is a lot cheaper than calling a function multiple times" (Eloquent JavaScript, Ch.3). In attempting to find that balance between recursion and simplicity, do not worry about efficiency until you know for sure that the program is too slow. If it is to slow find out which parts are taking up the most time, and start exchanging elegance for efficiency in those parts. An example and explanation of Recursion: 

```javascript 
function findSolution(target) {
  function find(current, history) {
    if (current == target) {
      return history;
    } else if (current > target) {
      return null;
    } else {
      return find(current + 5, "(" + history + " + 5)") || 
             find(current * 3, "(" + history + " * 3)");
    };
  };
  return find(1, "1");
};

console.log(findSolution(24)); // → (((1 * 3) + 5) * 3)
```
> The inner function find does the actual recursing. It takes two arguments—the current number and a string that records how we reached this number—and returns either a string that shows how to get to the target or null. To do this, the function performs one of three actions. If the current number is the target number, the current history is a way to reach that target, so it is simply returned. If the current number is greater than the target, there’s no sense in further exploring this history since both adding and multiplying will only make the number bigger. And finally, if we’re still below the target, the function tries both possible paths that start from the current number, by calling itself twice, once for each of the allowed next steps. If the first call returns something that is not null, it is returned. Otherwise, the second call is returned—regardless of whether it produces a string or null.

### Growing Functions
How smart and versatile should our function be? We could write anything from a terribly simple function that simply pads a number so that it's three characters wide to a complicated generalized number-formatting system. A useful principle is not to add cleverness unless you are absolutely sure you're going to need it.It can be tempting to write general "frameworks" for every little bit of functionality you come across. Resist that urge. You won't get any real work done, and you'll end up writing a lot of code that no one will ever use. 

### Functions and Side Effects 
Functions can be roughly divided into those that are called for their side effects and those that are called for their return value. (Though it is definitely also possible to have both side effects and return a value). A *pure function* is a specific kind of value-producing function that not only has no side effects but also doesn't rely on side effects from other code. A pure function has the pleasant property that, when called with the same arguments, it always produces the same value. Nonpure functions might return different values based on all kinds of factors and have side effects that might be hard to test and think about. 
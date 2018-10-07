# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Asynchronous Programming 
An asynchronous model allows multiple things to happen at the same time. When you start an action, your program continues to run. Below is a visual comparison of the differences in control flow between a synchronous environment and an asynchronous environment: 

![Diagram1](https://eloquentjavascript.net/img/control-io.svg)

### Callbacks 
One approach to asynchronous programming is to make functions that perform a slow action take an extra argument, a _callback function_. The action is started, and when it finishes, the callback function is called with the result. However calling a callback is somewhat more involved and error-prone then simply returning a value, so needing to structure large parts of your program that way is not great. 

### Promises 
A _promise_ is an asynchronous action that may complete at some point and produce a value. It is able to notify anyone who is interested when its value is available. The easiest way to create a promise is by calling `Promise.resolve`: 

```javascript 
let fifteen = Promise.resolve(15);
fifteen.then(value => console.log(`Got ${value}`));
// → Got 15
```
It is useful to think of promises as a device to move values into an asynchronous reality. A normal value is simply there. A promised value is a value that might already be there or might appear at some point in the future. Computations defined in terms of promises act on such wrapped values and are executed asynchronously as the value becomes available. 

### Async Functions 
An _async function_ is a special kind of generator. It produces a promise when called, which is resolved when it returns and rejected when it throws an exception. Whenever it `yields` (awaits) a promise, the result of that promise is the result of the `await` expression. 

### Deeper Dive: Getting to Know Asynchronous JavaScript 
[Medium@Getting to Know Asynchronous JavaScript](https://medium.com/codebuddies/getting-to-know-asynchronous-javascript-callbacks-promises-and-async-await-17e0673281ee)

Javascript is by nature single threaded and all code is executed in a sequence not in parallel. In JavaScript this is handled by using __asynchronous non-blocking I/O model__. What that means is while the execution of Javascript is blocking I/O operations are not. 

Registering event listeners in a browser with `addEventListener`, reading a files content with `fs.ReadFile` or registering a middleware an express web server with `server.use` are examples of common APIs that use Callbacks. 

Here is an example of fetching data from an URL using a module called "request": 

```javascript 
const request = require(‘request’);
function handleResponse(error, response, body){
    if(error){
        // Handle error.
    }
    else {
        // Successful, do something with the result.
    }
}
request('https://www.somepage.com', handleResponse);
```
A promise is an object that wraps an asynchronous operation and notifies when it's done. Instead of providing a callback, a promise has its own methods which you call to tell the promise what will happen when it is successful or when it fails. The methods a promise provides are `then(...)` for when a successful result is available and `catch(...)`  for when something went wrong. Promises provide an advantage over callbacks because instead of nesting callbacks inside callbacks, you then chain `.then()` calls together making it more readable and easier to follow. Typically you must have at least one `.catch()` at the end of your promise chain for you to be able to handle errors that occur. 

You can wrap a callback based asynchronous operation with a Promise like this: 

```javascript 
function getAsyncData(someValue){
    return new Promise(function(resolve, reject){
        getData(someValue, function(error, result){
            if(error){
                reject(error);
            }
            else{
                resolve(result);
            }
        })
    });
}

//

getAsyncData(“someValue”)
// Calling resolve in the Promise will get us here, to the first then(…)
.then(function(result){
    // Do stuff
})
// Calling reject in the Promise will get us here, to the catch(…)
// Also if there is an error in any then(..) it will end up here
.catch(function(error){
    // Handle error
});
```
In comparison there is a way to wrap a callback based asynchronous function inside a Promise and return that promise instead, this is known as "promisification". Version 8 of Node.js had a built in helper called "util.promisify" for doing exactly that. An example of the aforementioned: 

```javascript 
const { promisify } = require(‘util’);
const getAsyncData = promisify(getData);
getAsyncData(“someValue”)
.then(function(result){
    // Do stuff
})
.catch(function(error){
    // Handle error
});
```
> Async/Await is the next step in the evolution of handling asynchronous operations in Javascript. Async is for declaring that a function will handle asynchronous operations and await is used to declare that we want to "await" the result of an asynchronous operation inside a function that has the async keyword. 

Adding the functionality of Async/Await to our previous example would result in: 

```javascript 
function fetchTheData(someValue){
    return new Promise(function(resolve, reject){
        getData(someValue, function(error, result){
            if(error){
                reject(error);
            }
            else{
                resolve(result);
            }
        })
    });
}; 

async function getSomeAsyncData(value){
    const result = await fetchTheData(value);
    return result;
}
```
Inside the scope os an Async/Await function you can use try/catch for error handling and even though you await an asynchronous operation, any errors will end up in that catch block. 

```javascript 
async function getSomeData(value){
    try {
        const result = await fetchTheData(value);
        return result;
    }
    catch(error){
        // Handle error
    }
};
```



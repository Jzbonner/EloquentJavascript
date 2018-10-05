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
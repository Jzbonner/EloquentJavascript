# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Asynchronous Programming 
An asynchronous model allows multiple things to happen at the same time. When you start an action, your program continues to run. Below is a visual comparison of the differences in control flow between a synchronous environment and an asynchronous environment: 

![Diagram1](https://eloquentjavascript.net/img/control-io.svg)

### Callbacks 
One approach to asynchronous programming is to make functions that perform a slow action take an extra argument, a _callback function_. The action is started, and when it finishes, the callback function is called with the result. 
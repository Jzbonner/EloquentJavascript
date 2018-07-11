# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Bugs and Errors 
An important part of programming is finding, diagnosing and fixing bugs. Problems can become easier to notice if you have an automated test suite or add assertions to your programs. Typically problems caused by factors outside the program's control should be handled gracefully; special return values are a good way to track them as well as coding __exceptions__. 

Throwing an _exception_ causes the call stack to be unwound until the next enclosing `try/catch` block or until the bottom of the stack. The exception value will be given to the `catch` block that catches it, which should verify that it is actually the expected kind of exception and then do something with it. To help address the unpredictable control flow caused by exceptions, `finally` blocks can be used to ensure that a piece of code always runs when a block finishes. 

### Strict Mode and Types
JavaScript can be made  a little stricter by enabling __strict mode__, which is done by using the string `"use strict"` at the top of a file or function. 

```javascript 
function canYouSpotTheProblem() {
  "use strict";
  for (counter = 0; counter < 10; counter++) {
    console.log("Happy happy");
  }
}

canYouSpotTheProblem();
// → ReferenceError: counter is not defined
// not using the keyword let before counter is what causes the error when in strict mode 
```

* Another change in strict mode is that the `this` binding holds the value of _undefined_ in functions that are not called as methods. 
* Strict Mode also disallows giving a function multiple parameters with the same name and removes certain problematic language features entirely 

When the _Types_ of a program are known, it is possible for the computer to check them for you, pointing out mistakes before the program is run. There are several JavaScript dialects that add types to the language, the most popular being [TypeScript](https://www.typescriptlang.org/). 

### Testing
Automated testing is the process of writing a program that test another program. 

> Writing tests is a bit more work than testing manually, but once you’ve done it, you gain a kind of superpower: it takes you only a few seconds to verify that your program still behaves properly in all the situations you wrote tests for. When you break something, you’ll immediately notice, rather than randomly running into it at some later time.

(_Eloquent Javascript_, Ch. 8)

Some code is easier to test than other code. Generally the more external objects that the code interacts with, the harder it is to set up the context in which to test. 

#### Debugging
Coding Example: 

```javascript 
function numberToString(n, base = 10) {
  let result = "", sign = "";
  if (n < 0) {
    sign = "-";
    n = -n;
  }
  do {
    result = String(n % base) + result;
    n /= base;
  } while (n > 0);
  return sign + result;
}
console.log(numberToString(13, 10));
// → 1.5e-3231.3e-3221.3e-3211.3e-3201.3e-3191.3e-3181.3…
```

Coding Error Explanation: 

> Right. Dividing 13 by 10 does not produce a whole number. Instead of n /= base, what we actually want is n = Math.floor(n / base) so that the number is properly “shifted” to the right.

> An alternative to using console.log to peek into the program’s behavior is to use the debugger capabilities of your browser. Browsers come with the ability to set a breakpoint on a specific line of your code. When the execution of the program reaches a line with a breakpoint, it is paused, and you can inspect the values of bindings at that point. I won’t go into details, as debuggers differ from browser to browser, but look in your browser’s developer tools or search the Web for more information.

> Another way to set a breakpoint is to include a debugger statement (consisting of simply that keyword) in your program. If the developer tools of your browser are active, the program will pause whenever it reaches such a statement.

(_Eloquent JavaScript_, Ch. 8)

### Exceptions 
_Exceptions_ are mechanisms that make it possible for code that runs into a problem to raise or _throw_ an exception. An exception can be any value. Raising one somewhat resembles a super-charged return from a function: it jumps out of not just the current function but also it's callers, all the way down to the first call that started the current execution. This is called __unwinding the stack__. Their power lies in the fact that you can set "obstacles" along the stack to _catch_ the exception as it is zooming down. 

> The throw keyword is used to raise an exception. Catching one is done by wrapping a piece of code in a try block, followed by the keyword catch. When the code in the try block causes an exception to be raised, the catch block is evaluated, with the name in parentheses bound to the exception value. After the catch block finishes—or if the try block finishes without problems—the program proceeds beneath the entire try/catch statement.

(_Eloquent JavaScript_, Ch. 8)

If you want to catch a specific kind of exception, then you can do this by checking in the catch block whether the exception we got is the one we are interested in and rethrowing it otherwise. 

```javascript 
class InputError extends Error {}

function promptDirection(question) {
  let result = prompt(question);
  if (result.toLowerCase() == "left") return "L";
  if (result.toLowerCase() == "right") return "R";
  throw new InputError("Invalid direction: " + result);
}

for (;;) {
  try {
    let dir = promptDirection("Where?");
    console.log("You chose ", dir);
    break;
  } catch (e) {
    if (e instanceof InputError) {
      console.log("Not a valid direction. Try again.");
    } else {
      throw e;
    }
  }
}

/*
    This will catch only instances of InputError and let unrelated exceptions through. If you reintroduce the typo, the undefined binding error will be properly reported.
*/
```

### Assertions 
__Assertions__ are checks inside a program that verify that something is the way it is supposed to be. They are not used to handle situations that can come up in normal operation but to find programmer mistakes. 
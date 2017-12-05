# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Values, Types and Operators
A very good resource for reviewing basic building tools of JavaScript data types can be found at: [JavaScript W3School] (https://www.w3schools.com/js/) and at [MDN JavaScript] (https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics). Both of these resources will prepare you for understanding some of the more detailed concepts discussed later in this book. 

## Program Structure 
Taking a look at the overall form of a program through its individual components and their interrelationships. 

> At a finer level a well structured program employs appropriate data structures and program units with a single entry point and a single exit point, while a poorly structured program has arbitrary data structures and flow of control.

(Dictionary of Computing, 2004)

### Expressions and Statements 
A fragment of code that produces a value is called an expression. Expressions can nest in a very similar way to the subsenteces in human languages are nested. If an expression corresponds to a sentence fragment, a Javascript statement corresponds to a full sentence in a human language. A program is a list of statements. The simplest kind of statement is an expression with a semicolon after it. 

### Variables
To catch and hold values, Javascript provides a thing called variables. Typically,the special keyword (var) indicates that a proceeding statement is going to define a variable. After a variable has been defined its name can be used as an expression. **Variables names can be any word that isn't a reserved word; they may not include spaces; digits cannot start the name of a variable; the name cannot include punctuation** 

When a variable points at a value, that does not mean it is tied to that value forever. The = operator can be used at any time on exisitng variables to disconnect them from their current value and have them point to a new one. 

```javascript 
var luigisDebt = 140;
luigisDebt = luigisDebt - 35;
console.log(luigisDebt);
// → 105
```
A single *var* statement may define multiple variables. The definitions must be separated by commas. 

### Keywords and Reserved Words 
The full list of keywords and reserved words is as follows: 

> break case catch class const continue debugger default delete do else enum export extends false finally for function if implements import in instanceof interface let new null package private protected public return static super switch this throw true try typeof var void while with yield

### The Environment 
The collection of variables and their values that exist at a given time is called the environment. Programs always contain variables that are part of language standards as well as ways to interact with the surrounding system. For instance, in a browser there are variables and functions to inspect and influence the loaded webpage as well as read mouse and keyboard input. 

### Functions 
A function is a piece of program wrapped in a value. Executing a function is called invoking, calling or applying it. You can call a function by putting parentheses after an expression that produces a function value. The values between the parentheses are given to the program inside the function. Values given to functions are called *arguments*. 

#### Console.Log Function 
Most JavaScript systems provide a console.log function that writes out its arguments to some text output device (In browsers, the output lands in the JavaScript console). 

#### Return Values 
When a function produces a value it is said to return that value. Anything that produces a value is an expression in JavaScript, which means function calls can be used within larger expressions. 

#### Prompt and Confirm 
You can ask the user an OK/Cancel question using **confirm**. This returns a Boolean: *true* if the user clicks OK and *false* if the user clicks Cancel. The **prompt** function can be used to ask an "open" question. The first argument is the question and the second one is the text that the user starts with. A line of text can be typed into the dialog window and the function will return this text as a string. 

#### Control Flow
When your program contains more than one statement, the statements are executed from top to bottom. The execution from top to bottom could be represented in a number of ways depending on the associated statements. The simplest control flow is known as Straight control flow, but there is also Conditional Execution; where there is a choice between two different routes that is based on a Boolean value. Conditional Execution is written with the *if* keyword in JavaScript. The keyword *if* executes or skips a statement depending on the value of the Boolean expression. The deciding expression is written after the keyword, between parentheses, followed by the statement to execute. The *else* keyword can be used, together with if, to create two separate, alternative execution paths. Looping control flow allows us to go back to some point in the program where we were before and repeat it with our current program state. A statement starting with the keyword *while* creates a loop. The word *while* is followed by an expression in parentheses and then a statement. The loop executes that statement as long as the expression produces a value that is true when converted to Boolean type. Whenever we need to execute multiple statements inside a loop, we wrap them in curly braces ({}). Braces do for statements what parentheses do for expressions: they group them together, making them count as a single statement. A sequence of statements wrapped in braces is called a block. The *do* loop is a control structure similar to the *while* loop. It differs only on one point: a *do* loop always executes its body at least once, and it starts testing whether it should stop only after that first execution. 

#### Indenting Code 
The role of the indentation inside blocks is to make the structure of the code stand out. With proper indentation, the visual shape of a program corresponds to the shape of the blocks inside it. 

#### For Loops 
The parentheses after a *for* keyword must contain two semicolons. The part before the first semicolon initializes the loop, usually be defining a variable. The second part is the expression that checks whether the loop must continue. The final part updates the state of the loop after every iteration. 

#### Breaking Out of a Loop 
There is a special statement called *break* that has the effect of immediately jumping out of the enclosing loop. Example: 

```javascript 
for (var current = 20; ; current++) {
  if (current % 7 == 0)
    break;
}
console.log(current);
// → 21
```
Using the remainder (%) operator is an easy way to test whether a number is divisible by another number. If it is, the remainder of their division is zero. The *for* construct in the example does not have a part that checks for the end of the loop. This means that the loop will never stop unless the *break* statement inside is executed. The *continue* keyword is similar to *break*, in that it influences the progress of a loop. When *continue* is encountered in a loop body, control jumps out of the body and continues with the loop's next iteration. 

#### Updating Variables Succinctly
For more information on JavaScript operators please refer to this [link] (https://www.w3schools.com/jsref/jsref_operators.asp). 

#### Dispatching on a Value with Switch 
There is a construct called *switch* that is intended to solve such a "dispatch" in a more direct way. 

```javascript 
switch (prompt("What is the weather like?")) {
  case "rainy":
    console.log("Remember to bring an umbrella.");
    break;
  case "sunny":
    console.log("Dress lightly.");
  case "cloudy":
    console.log("Go outside.");
    break;
  default:
    console.log("Unknown weather type!");
    break;
}
```
The program will jump to the label that corresponds to the value that *switch* was given or to *default* if no matching value is found. It starts executing statements there, even if they're under another label, until it reaches a *break* statement. 

#### Capitalization 
> The standard JavaScript functions, and most JavaScript programmers, follow the bottom style—they capitalize every word except the first. 

(Eloquent JavaScript, Ch.2)

Example: fuzzyLittleTurtle

#### Comments 
A comment is a piece of text that is part of a program but is completely ignored by the computer. To write a single-line comment, you can use two slash characters (//) and then the comment text after it. 

A (//) comment goes only to the end of the line. A section of text between /* and */ will be ignored, regardless of whether it contains line breaks. This is often useful for adding blocks of information about a file or a chunk of program. 
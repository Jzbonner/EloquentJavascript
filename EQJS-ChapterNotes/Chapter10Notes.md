# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Modules 
A _module_ is a piece of program that specifies which other pieces it relies on and which functionality it provides for other modules to use. Module interfaces make part of the module available to the outside world and keep the rest private. The relations between modules are called _dependencies_. 

### General Guidelines of Modules 
* Each modules is a piece of code that is executed once it is loaded 
* In that code there may be declarations: 
    * By default these declarations stay local to the module
    * You can mark some of them as exports, then other modules can import them 
* A module can import things from other modules. It refers to those modules via *module specifiers*, strings that are either: 
    * Relative Paths `('../models/user')`: these paths are interpreted relative to the location of the importing module [file extensions can be omitted]
    * Absolute Paths `('lib/js/helpers')`: point directly to the file of the module to be imported
    * Names `('util')`: what module names refer to has to be configured 
* Modules are singletons. Even if a module is imported several times only a single instance of it exist 

### ECMAScript (ES6) Modules 
An ES module's interface is not a single value but a set of named bindings. When you import from another module, you import the binding, not the value, which means exporting module may change the value of the binding at any time, and the modules that import it will see its new value. When there is a binding named `default` it is treated as the module's main exported value. 

```javascript 
export default ["Winter", "Spring", "Summer", "Fall"];
```

It is possible to rename imported bindings using the word `as`: 

```javascript 
import { days as dayNames } from "date-names"; 
```

ES module imports happen before a module's script starts running. That means `import` declarations may not appear inside functions or blocks, and names of the dependencies must be quoted strings. 

The ES6 module standard has two parts: 
* Declarative Syntax (for importing and exporting)
* Programmatic loader API to configure how modules are loaded and to conditionally load modules 

### Bundling and Building 
Web programmers have started using tools that roll their programs back into a single big file before they publish it to the Web. Such tools are known as **bundlers**. The Javascript community has also created tools that take a Javascript program and make it smaller by automatically removing comments and whitespace, renaming bindings, and replacing pieces of code with equivalent code that takes up less space; these tools are known as **minifiers**. 

#### Module Design 
> One aspect of module design is ease of use. If you are designing something that is intended to be used by multiple people—or even by yourself, in three months when you no longer remember the specifics of what you did—it is helpful if your interface is simple and predictable.

(__Eloquent Javascript__, Ch. 10)

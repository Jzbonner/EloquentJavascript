# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Node.js 
One of the more difficult problems with writing systems that communicate over the network is managing inputs and outputs - that is, the reading and writing of data to and from the network and hard drive. Moving data around takes time and scheduling it cleverly can make a big difference in how quickly a system responds to the user or to network request. Node was initially conceived for the purpose of making asynchronous programming easy and convenient. 

### Modules 
If you want to access built-in functionality, you have to use the module system. The CommonJS module system, based on the `require` function, is built into Node and is used to load anything from built-in modules to downloaded packages to files that are part of your own program. When `require` is called Node has to resolve the given string to an actual file that it can load. Pathnames that start with __/,./, or ../__ are resolved relative to the current module's path where _._ stands for the current directory, _../_ for one directory up, and _/_ for the root file system. 
```javascript 
// Main.js 
const {reverse} = require("./reverse");

// Index 2 holds the first actual command line argument
let argument = process.argv[2];

console.log(reverse(argument));

// Reverse.js 
exports.reverse = function(string) {
  return Array.from(string).reverse().join("");
};
```

### NPM Install (Package.json and Versions)
When you run `npm install` without naming a package to install, NPM will install the dependencies listed in `package.json`. When you install a specific package that is not already listed as a dependency, NPM will add it to package.json. A `package.json` file list both the program's own version and versions for its dependencies. _Versions_ are a way to deal with the fact that packages evolve separately, and code written to work with a package as it existed at one point may not work with a later, modified, version of the package. 

### The File System Module 




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
One of the most commonly used built-in modules in Node is the `fs` module, which stands for _file system_. It exports functions for working with files and directories. Below is an example of a _readFile_ and a _writeFile_ function: 
```javascript 
let {readFile} = require("fs");
readFile("file.txt", "utf8", (error, text) => {
  if (error) throw error;
  console.log("The file contains:", text);
});

const {writeFile} = require("fs");
writeFile("graffiti.txt", "Node was here", err => {
  if (err) console.log(`Failed to write file: ${err}`);
  else console.log("File written.");
});
```

> The fs module contains many other useful functions: readdir will return the files in a directory as an array of strings, stat will retrieve information about a file, rename will rename a file, unlink will remove one, and so on. See the documentation at https://nodejs.org for specifics. 

(__EloquentJavascript__, Ch. 20)

### The HTTP Module 
The HTTP module provides functionality for running HTTP servers and making HTTP request. Below is an example for creating a simple HTTP server: 
```javascript 
const {createServer} = require("http");
let server = createServer((request, response) => {
  response.writeHead(200, {"Content-Type": "text/html"});
  response.write(`
    <h1>Hello!</h1>
    <p>You asked for <code>${request.url}</code></p>`);
  response.end();
});
server.listen(8000);
console.log("Listening! (port 8000)");
```

The function passed as argument to `createServer` is called every time a client connects to the server. The `request` and `response` bindings are objects representing the incoming and outgoing data. The first contains information about the request, such as it's `url` property, which tells us to what URL the request was made. So when you open that page in your browser, it sends a request to your own computer. This causes the server function to run and send back a response, which you can then see in the browser. 

Making request with Node's raw functionality is rather verbose. There are much more convenient wrapper packages available on NPM. For example, `node-fetch` provides the promise-based `fetch` interface that we know from the browser. 

### Full Example of a HTTP File Server Configuration 
```javascript 
const {createServer} = require("http");

const methods = Object.create(null);

createServer((request, response) => {
  let handler = methods[request.method] || notAllowed;
  handler(request)
    .catch(error => {
      if (error.status != null) return error;
      return {body: String(error), status: 500};
    })
    .then(({body, status = 200, type = "text/plain"}) => {
       response.writeHead(status, {"Content-Type": type});
       if (body && body.pipe) body.pipe(response);
       else response.end(body);
    });
}).listen(8000);

async function notAllowed(request) {
  return {
    status: 405,
    body: `Method ${request.method} not allowed.`
  };
}

var {parse} = require("url");
var {resolve, sep} = require("path");

var baseDirectory = process.cwd();

function urlPath(url) {
  let {pathname} = parse(url);
  let path = resolve(decodeURIComponent(pathname).slice(1));
  if (path != baseDirectory &&
      !path.startsWith(baseDirectory + sep)) {
    throw {status: 403, body: "Forbidden"};
  }
  return path;
}

const {createReadStream} = require("fs");
const {stat, readdir} = require("fs").promises;
const mime = require("mime");

methods.GET = async function(request) {
  let path = urlPath(request.url);
  let stats;
  try {
    stats = await stat(path);
  } catch (error) {
    if (error.code != "ENOENT") throw error;
    else return {status: 404, body: "File not found"};
  }
  if (stats.isDirectory()) {
    return {body: (await readdir(path)).join("\n")};
  } else {
    return {body: createReadStream(path),
            type: mime.getType(path)};
  }
};

const {rmdir, unlink} = require("fs").promises;

methods.DELETE = async function(request) {
  let path = urlPath(request.url);
  let stats;
  try {
    stats = await stat(path);
  } catch (error) {
    if (error.code != "ENOENT") throw error;
    else return {status: 204};
  }
  if (stats.isDirectory()) await rmdir(path);
  else await unlink(path);
  return {status: 204};
};

const {createWriteStream} = require("fs");

function pipeStream(from, to) {
  return new Promise((resolve, reject) => {
    from.on("error", reject);
    to.on("error", reject);
    to.on("finish", resolve);
    from.pipe(to);
  });
}

methods.PUT = async function(request) {
  let path = urlPath(request.url);
  await pipeStream(request, createWriteStream(path));
  return {status: 204};
};

const {mkdir} = require("fs").promises;

methods.MKCOL = async function(request) {
  let path = urlPath(request.url);
  let stats;
  try {
    stats = await stat(path);
  } catch (error) {
    if (error.code != "ENOENT") throw error;
    await mkdir(path);
    return {status: 204};
  }
  if (stats.isDirectory()) return {status: 204};
  else return {status: 400, body: "Not a directory"};
};
```



# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Data Structures: Objects and Arrays
Numbers, Booleans, and strings are the bricks that data structures are built from. But you can’t make much of a house out of a single brick. Objects allow us to group values—including other objects—together and thus build more complex structures.

### Data Sets 
An array is a data type in Javascript specifically for storing sequences of values. 

```javascript 
let listOfNumbers = [2, 3, 5, 7, 11];
console.log(listOfNumbers[2]);
// → 5
```

The first index of an array is zero, not one. The notation for getting at the elements inside an array also uses square brackets. A pair of square brackets immediately after an expression, with another expression inside of them, will look up the element in the left hand expression that corresponds to the index given by the expression in brackets. 

### Properties 
Almost all JavaScript values have properties (however, null and undefined do not). The typical way to access properties of data types is via dot notation and square bracket notation. For instance both **value.x** and **value[x]** both access a property of  **value** but they do not necessarily access the same property. This is due to interpretation. When using dot notation, the word after the value is the name of the property. When using square bracket notation, the expression between the brackets is evaluated to get the property. 

The elements in an array are stored as the array's properties, using numbers as property names. Because you can't use dot notation with numbers you have to use square bracket notation to get at those properties. 

### Methods 
Properties that contain functions are generally called **Methods** of the value they belong to (i.e. "toUpperCase" is a method of a _string_). In computer programming a _stack_ is an abstract data type that serves as a collection of elements, with two principal operations: 

```javascript
push() //which adds an element to the collection 
pop() //which removes the most recently added element that was not yet removed
``` 

**Stacks** are found in real world applications such as Backtracking, Time Memory Management, and Algorithmic Information Organization.

> Stack is a linear data structure which follows a particular order in which the operations are performed. The order may be LIFO(Last In First Out) or FILO(First In Last Out). 

([Geeks for Geeks](https://www.geeksforgeeks.org/stack-data-structure-introduction-program/))

### Objects 
Values of the type _object_ are arbitrary collections of properties. A developer can use curly braces to signify an object. Inside the braces, there is a list of properties that are separated by commas. Each property has a name separated by a colon and a value. Properties whose names are not valid binding names or valid numbers have to be quoted. Examples below: 

```javascript 
let day1 = {
  squirrel: false,
  events: ["work", "touched tree", "pizza", "running"]
};

let descriptions = {
  work: "Went to work",
  "touched tree": "Touched a tree"
};
```

In javascript curly brace notation can signify two important things. One, at the start of a statement the initiate a block of statements. In any other position they describe an object. 

Objects can also be manipulated via operators. Below is a list of special (i.e. non-arithmetic, non-conditional, non-logical) operators to make reference of, as well as common object functions and their defined uses: 

Operator | In-Code | Definition 
--- | --- | --- 
_delete_ | `delete` | deletes the value of the property and the property itself 
_in_ | `in` | returns true if the specified property is in the specified object
_instance of_ | `instanceof` | returns true if their is an instance of the specified object in the specified object 
_void_ | `void` | evaluates an expression and returns undefined 
_type of_ | `typeof` | return the type of variable, object, function or expression

Function | In-Code | Definition 
--- | --- | ---
_keys_ | `Object.keys()` | Provides only the key values for a specified object 
_assign_ | `Object.assign()` | Copies all the properties from one object into another object 

### Mutability 
Certain types of values in JavaScript are immutable (i.e. it is impossible to change values of those type) and they are numbers, strings and booleans. You can combine them and derive new values from them, but when you take a specific string value, that value will always remain the same. Bindings to such values can be changed as long as they are not constants. The content of an object value can be modified by changing its properties. 

When using the compare operator in JavaScript (**==**) it will produce _true_ only if both objects are precisely the same value. Comparing different objects will return _false_. 

### Arrays, Strings and their Properties and the Math Object 
Below are some important concepts that are useful when working with arrays and strings: 

Function/Method | In-Code | Definition 
--- | --- | --- 
_unshift_ | `.unshift()` | adds one or more elements to the beginning of the array and returns the new length 
_shift_ | `.shift()` | removes the first element of an array and returns that removed element 
_index of_ | `.indexOf()` | returns the index within the calling object of the first occurrence of the specified value 
_last index of_ | `lastIndexOf()` | returns the index within the calling object of the last occurrence of the specified value 
_slice_ | `.slice()` | returns a shallow copy of a portion of an array into a new array 
_trim_ | `.trim()` | removes whitespace from both ends of a string 
_pad start_ | `.padStart()` | pads the current string with another so that the resulting string reaches the given length  
_split_ | `.split()` | splits a string object into an array of strings using a specified separator 
_join_ | `.join()` | joins all elements of an array into a string and returns this string
_repeat_ | `.repeat()` | constructs and returns a new string which contains the specified number of copies

The Math Object is used as a container to group a bunch of related functionality. You can learn more about the Math Object from these resources: 
* [JavaScript Math Object - W3Schools] (https://www.w3schools.com/js/js_math.asp)
* [Math MDN] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)
* [JavaScript the Math Object] (https://www.tutorialspoint.com/javascript/javascript_math_object.htm)

### JSON 
A popular serialization format is called _JSON_ (JavaScript Object Notation). JSON is typically used as a data storage and communication format for the web. 

> JSON looks similar to JavaScript’s way of writing arrays and objects, with a few restrictions. All property names have to be surrounded by double quotes, and only simple data expressions are allowed—no function calls, bindings, or anything that involves actual computation. Comments are not allowed in JSON.

(EloquentJavascript, Ch. 4)

A representation of JSON data is as follows: 

```javascript 
{
  "squirrel": false,
  "events": ["work", "touched tree", "pizza", "running"]
}
```

JavaScript uses the functions `JSON.stringify()` and `JSON.parse()` to convert data to and from this format. The first of which takes a JavaScript value and returns a JSON-encoded string. The second takes such a string and converts it to the value it encodes.  

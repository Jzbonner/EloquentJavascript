# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## The Secret Life of Objects 
Object Oriented Programming, a set of techniques that uses objects (and related concepts) as central principle of program organization. 

## Encapsulation 
The core idea in object oriented programming is to divide programs into smaller pieces, and make each piece responsible for managing its own state. Different pieces of a program interact with each other through _interfaces_, which are limited sets of functions or bindings that provide useful functionality at a more abstract level. 

These program pieces are modeled using objects and their interfaces consists of a specific set of methods and properties. 
* Public Properties - are those that are part of the interface itself 
* Private Properties - are those that are not part of the interface and are utilized at a less abstract level 

This separation of interface from the implementation is generally known as encapsulation. 

> Encapsulation is one of the fundamental concepts in object-oriented programming (OOP). It describes the idea of bundling data and methods that work on that data within one unit, e.g., a class in Java. This concept is also often used to hide the internal representation, or state, of an object from the outside. This is called information hiding. The general idea of this mechanism is simple. If you have an attribute that is not visible from the outside of an object, and bundle it with methods that provide read or write access to it, then you can hide specific information and control access to the internal state of the object.

[OOP Concepts: Stackify](https://stackify.com/oop-concept-for-beginners-what-is-encapsulation/)

## Methods 
Methods are essentially properties that hold function values. Below is an example of a simple method in Javascript: 

```javascript 
let rabbit = {};
rabbit.speak = function(line) {
  console.log(`The rabbit says '${line}'`);
};

rabbit.speak("I'm alive.");

/*Below is an example of a method that utilizes the `this` binding of functions*/
function speak(line) {
  console.log(`The ${this.type} rabbit says '${line}'`);
}
let whiteRabbit = {type: "white", speak};
let hungryRabbit = {type: "hungry", speak};

whiteRabbit.speak("Oh my ears and whiskers, " +
                  "how late it's getting!");
// → The white rabbit says 'Oh my ears and whiskers, how
//   late it's getting!'
hungryRabbit.speak("I could use a carrot right now.");
// → The hungry rabbit says 'I could use a carrot right now.'
speak.call(hungryRabbit, "Burp!");
// → The hungry rabbit says 'Burp!'
```

When a function is called as a method the binding called `this` in its body automatically points at the object that it was called on. Typically if you want to pass the `this` binding explicitly, you can use a function's `call` method, which takes the `this` value as first argument, and treats further arguments as normal parameters. 

## Prototypes 
A prototype is another object that is used as a fallback source of properties. The prototype relations of JavaScript objects form a tree-shaped structure, and at the root of this structure sits `Object.prototype`. This enables a few methods that show up in all objects (i.e. toString, etc.)

Many objects don't directly have `Object.prototype` as their prototype, but instead they have another object, which provides its own default properties. (i.e. functions have Function.prototype and Arrays have Array.prototype)

`Object.create` allows you to create an object with a specific prototype. For instance: 

```javascript 
let protoRabbit = {
  speak(line) {
    console.log(`The ${this.type} rabbit says '${line}'`);
  }
};
let killerRabbit = Object.create(protoRabbit);
killerRabbit.type = "killer";
killerRabbit.speak("SKREEEE!");
// → The killer rabbit says 'SKREEEE!'
```

## Classes 
Classes are the more derived approach to prototypes in JavaScript. A class defines the shape of a type of object - what methods and properties it has. Such an object is called an `instance` of the class. 

JavaScript provides a way to define constructor functions easier through the `new` keyword. An object with the right prototype will be created, bound to `this` in the function, and _returned at the end of the function_. (:point_left: very important piece)

> Constructors (all functions, in fact) automatically get a property named prototype, which by default holds a plain, empty object that derives from Object.prototype. You can overwrite it with a new object if you want. Or you can add properties to the existing object, as the example does.

```javascript 
function Rabbit(type) {
  this.type = type;
}
Rabbit.prototype.speak = function(line) {
  console.log(`The ${this.type} rabbit says '${line}'`);
};

let weirdRabbit = new Rabbit("weird");
```

By convention the names of constructors are capitalized so that they can easily be distinguished from other functions. 

## Class Notation 
The `class` keyword starts a class declaration, which allows us to define a constructor and a set of methods all in a single place. Any number of methods may be written inside the declaration's curly braces. `Constructor` is obviously treated specifically and the others are packaged into that constructor's prototype. 

Class declarations only allow methods - properties that hold functions - to be added to the prototype. You can still create such properties by directly manipulating the prototype after you've defined the class. 

```javascript
class Rabbit {
  constructor(type) {
    this.type = type;
  }
  speak(line) {
    console.log(`The ${this.type} rabbit says '${line}'`);
  }
}

let killerRabbit = new Rabbit("killer");
let blackRabbit = new Rabbit("black");
```

When you add a property to an object, whether it is present in the prototype or not, the property is added to the object itself - which will have it as its own property. If there is a property by the same name in the prototype, this property will no longer affect the object, as it is now hidden behind the object's own property. 

```javascript 
Rabbit.prototype.teeth = "small";
console.log(killerRabbit.teeth);
// → small
killerRabbit.teeth = "long, sharp, and bloody";
console.log(killerRabbit.teeth);
// → long, sharp, and bloody
console.log(blackRabbit.teeth);
// → small
console.log(Rabbit.prototype.teeth);
// → small
```

Overriding properties that exist in a prototype can be a useful thing to do. As the rabbit teeth example shows, it can be used to express exceptional properties in instances of a more generic class of objects, while letting the non-exceptional objects simply take a standard value from their prototype. 

## Maps 
A map is a data structure that associates values with other values. Here is an example: 

```javascript
let ages = {
  Boris: 39,
  Liang: 22,
  Júlia: 62
};

console.log(`Júlia is ${ages["Júlia"]}`);
// → Júlia is 62
console.log("Is Jack's age known?", "Jack" in ages);
// → Is Jack's age known? false
console.log("Is toString's age known?", "toString" in ages);
// → Is toString's age known? true
```

Object property names must be strings. If you need a map whose keys can't easily be converted to strings - such as objects - you cannot use an object as your map. However you can use the class called `Map` to circumvent this restriction. It will store a mapping and allows for all types of key values. 

```javascript 
let ages = new Map();
ages.set("Boris", 39);
ages.set("Liang", 22);
ages.set("Júlia", 62);

console.log(`Júlia is ${ages.get("Júlia")}`);
// → Júlia is 62
console.log("Is Jack's age known?", ages.has("Jack"));
// → Is Jack's age known? false
```

> The methods set, get, and has are part of the interface of the Map object. Writing a data structure that can quickly update and search a large set of values isn’t easy, but we don’t have to worry about that. Someone else did it for us, and we can go through this simple interface to use their work.

## Polymorphism 
~ Refer to notes in the Supplemental Folder for a clearer definition. Will cover this topic in more detail in later Chapters as well. ~

## Symbols 
Symbols are values created with the `Symbol` function. Unlike strings newly created symbols are unique, you can't create the same symbol twice. Being both unique and useable as property names makes symbols suitable for defining interfaces that can peacefully live alongside other properties, no matter what their names are. 

```javascript 
const toStringSymbol = Symbol("toString");
Array.prototype[toStringSymbol] = function() {
  return `${this.length} cm of blue yarn`;
};

console.log([1, 2].toString());
// → 1,2
console.log([1, 2][toStringSymbol]());
// → 2 cm of blue yarn
```

## The Iterator Interface 
> The object given to a for/of loop is expected to be iterable. This means that it has a method named with the Symbol.iterator symbol (a symbol value defined by the language, stored as a property of the Symbol function). When called, that method should return an object that provides a second interface, iterator. This is the actual thing that iterates. It has a next method that returns the next result. That result should be an object with a value property, providing the next value, and a done property, which should be true when there are no more results and false otherwise.

```javascript 
let okIterator = "OK"[Symbol.iterator]();
console.log(okIterator.next());
// → {value: "O", done: false}
console.log(okIterator.next());
// → {value: "K", done: false}
console.log(okIterator.next());
// → {value: undefined, done: true}
```

## Getters, Setters and Statics 
Even properties that are accessed directly may hide a method call. Such methods are called `getters`, and they are defined by writing `get` in front of the method name in an object expression or class declaration. When a property is written to a specific method this is known as a `setter`, and they are defined by writing `set`. 

```javascript 
class Temperature {
  constructor(celsius) {
    this.celsius = celsius;
  }
  get fahrenheit() {
    return this.celsius * 1.8 + 32;
  }
  set fahrenheit(value) {
    this.celsius = (value - 32) / 1.8;
  }

  static fromFahrenheit(value) {
    return new Temperature((value - 32) / 1.8);
  }
}

let temp = new Temperature(22);
console.log(temp.fahrenheit);
// → 71.6
temp.fahrenheit = 86;
console.log(temp.celsius);
// → 30
```

The Temperature class allows you to read and write temperature in either degrees Celsius or degree Fahrenheit, but internally only stores Celsius, and automatically converts Celsius in the fahrenheit getter and setter. Inside a class declaration methods that have a `static` keyword written before their name are stored on the constructor. So the Temperature class allows you to write `Temperature.fromFahrenheit(100)` to create a temperature using degrees Fahrenheit. 

## Inheritance 
> Inheritance allows us to build slightly different data types from existing data types with relatively little work. It is a fundamental part of the object-oriented tradition, alongside encapsulation and polymorphism. But while the latter two are now generally regarded as wonderful ideas, inheritance is more controversial. Whereas encapsulation and polymorphism can be used to separate pieces of code from each other, reducing the tangledness of the overall program, inheritance fundamentally ties classes together, creating more tangle. When inheriting from a class, you usually have to know more about how it works than when simply using it. Inheritance can be a useful tool, and I use it now and then in my own programs, but it shouldn’t be the first tool you reach for, and you probably shouldn’t actively go looking for opportunities to construct class hierarchies (family trees of classes).

(_EloquentJavascript_, Ch. 6)


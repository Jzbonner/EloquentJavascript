# Object Oriented Programming ~ A Glance. 
Notes and Information taken from the book entitled *Practical Object-Oriented Design* in Ruby by Sandi Metz. 

## Chapter 1 - Object Oriented Design 
When referencing Object Oriented applications, developers see them as consisting of two concepts: parts and how those parts interact with each other. Typically, a part is an object. Some OO applications have numerous parts and others have only a few, but these parts typically communicate by sending messages; which is how they interact with each other. Software development as a whole is defined by how you, the developer, arrange your code. 
* Managing dependencies means arranging your code so that objects or parts of the application can tolerate change. Objects that are two embedded in other parts or other objects in the application can cause immense damage to the function of the program as a whole. 
* The purpose of good software design is to enable more good design and reduce the overall cost of change. 

Utilizing code patterns or easily identifiable semantics is beneficial because it allows you to solve common problems in simple and possibly reusable ways. When working in Agile settings (a software development ideology that advocates for adaptive planning, evolutionary development, early delivery, and continuous improvement), Big Up Front Designs do not work. The goal in corporate application is to keep the average cost per feature as low as possible. 

> An application can be deemed successful if it lives a long time. Applications survive by being able to deal with change. Good software design enables this change. 

(*Practical Object-Oriented Design*, Ch.1)

## Chapter 2 - Single Purpose Classes 
Technical knowledge and organization are integral parts of the Object Oriented Programming. Typically your code should be easy to use and defined by simple yet useful classes that are primarily single use functions. Multi-use variables are useable but should not be overused. Their design makes them difficult to reuse and the overuse of explicit parts from outside the class imply entanglement from within the class.

Classes in applications should focus on and be defined by their behavior not necessarily the data. You can use encapsulation and message sending to access variables. Doing this creates clarity and promote scalability from within the project.  
* Methods, like classes, should also have a single responsibility
* Single responsibility exposes hidden qualities, removes the need for comments, and encourages reuse
* Single responsibility entities are easy to move around, are easy to isolate, and are easy to design 

Object Oriented Programming allows developers to focus on the larger aspects of the Software Development Life Cycle. Cost Analysis (current cost vs. future expense) and Time Management are just two areas where OOP can allow you to find the balance between pre-optimization and over-design. 

## Chapter 3 - Managing Dependencies

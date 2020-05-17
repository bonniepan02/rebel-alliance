The one constant in software development: change

The goal of the principles is the creation of mid-level software structures that: 
- Tolerate change,
- Are easy to unserstand, and 
- Are the basis of components that can be used in many software systems

Design principle: 
Identify the aspects of your application that vary and separate them from what stays the same
Here is another way to think about this principle: take the parts that vary and encapsulate them, so that later you can alter or extend the parts that vary without affecting those that don't. 


Take what varies and "encapsulate" it so it won't affect the rest of your code. 
The result? Fewer unintended consequences from code chnages and more flexibility in your systems! 


Intefaces to encapsulate a behavior. Code to behavior enables system's flexibility of dynamically use different behaviors at runtime. 

Creating system using composition gives you a lot more flexibility. Not only does it let you encapsulate a famility of algorithms into their own set of classes, but it also let you change behavior at runtime as long as the object you are composing with implements the correct behavior inteface. 

The strategy pattern defines a family of algorithms, encapsulate each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it. 

According to the strategy pattern, the behaviour of a class should be encapsulated using interfaces and should not be inherited. This is compatible with the open closed principle, one of the five SOLID rules.
The open closed principle:
Classes should be open for extension but closed for modification.

SOLID Principles: 
https://github.com/heykarimoff/solid.python

Single responsibility Principle:
each software module has one and only one reason to change.

Open closed principle: 
They must be designed to allow the behavior of those systems to be changed by adding new code, rather than changing existing code. 


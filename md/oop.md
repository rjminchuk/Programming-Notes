#### [<< readme.md](../README.md) 
# OOP Concepts

#### three pillars of oop
- Encapsulation - key pillar. helps hide complexity. classes are one way to achieve encapsulation. methods are another way to encapsulate logic. Encapsulation is all about hiding complexity and building models that logically group together functionality.
- Inheritance - 
- Polymorphism

#### Inheritance
You can only specify a single base class after the collon, but the inheritance chain is unlimmited. 
In c# you can take a derived type and assign it to a variable of it's base type.
A protected member of a base class is accessible from the base or any of it's derrived classes. 
Inheritance binds two classes together into a relationship that cannot be broken, and can cause a program to be harder to change or harder to understand.

#### Polymorphism
The concept of many shapes.
A derived class IS A base class or inherits from a base class.
The c# compiler will invoke a method of a type based on the type the variable is defined as.
IE: BaseClass b = new DerivedClass(); b.MyMethod() -> "I executed the base method"
IE: DerivedClass d = new DerivedClass(); d.MyMethod() -> "I executed the derived method"
The virtual keyword allows a derived class to override the base method if the base method is marked virtual.
All Classes inherit from Object and object makes ToString() available to be overriden by declaring it virtual.

#### Abstract Classes
Abstract classes cannot be instantiated. Abstract classes need to be inherited.
You can override an abstract class's method by using the keyword virtual in its method signature.
Abstract methods define no body of code to execute and must have an implementation provided by a concrete class. A concrete class defines the concrete implementation of an abstract class and defines all abstract members. You can only instantiate a concrete type.
- An abstract class might be used to represent the similarities between two different concrete classes.
- An abstract class cannot be instatiated but it's members may be implemented and extended.

#### Interfaces
An interface defines the signatures of methods, events and properies. An interface is a type just like a class, struct, and delegates are types. A Type can implement as many interfaces as it needs.
you can't define private signatures of methods because the class that implements an interface must have access to the methods of properties.
- An interface can be used to represent similar functionalities across concrete classes that are very much dissimilar.
- An interface can be implemented.
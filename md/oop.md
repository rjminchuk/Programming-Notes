#### [<< readme.md](../README.md) 

# Three pillars of oop

## Encapsulation 
Helps hide complexity. Classes are one way to achieve encapsulation. methods are another way to encapsulate logic. Encapsulation is all about hiding complexity and building models that logically group together functionality. IE: an Abstraction.

## Inheritance
You can only specify a single base class after the collon, but the inheritance chain is unlimmited. 
In c# you can take a derived type and assign it to a variable of it's base type.
A protected member of a base class is accessible from the base or any of it's derrived classes. 
Inheritance binds two classes together into a relationship that cannot be broken, and can cause a program to be harder to change or harder to understand.

## Polymorphism
The concept of many shapes.
A derived class IS A base class or inherits from a base class.
The c# compiler will invoke a method of a type based on the type the variable is defined as.
IE: BaseClass b = new DerivedClass(); b.MyMethod() -> "I executed the base method"
IE: DerivedClass d = new DerivedClass(); d.MyMethod() -> "I executed the derived method"
The virtual keyword allows a derived class to override the base method if the base method is marked virtual.
All Classes inherit from Object and object makes ToString() available to be overriden by declaring it virtual.

## Abstract Classes
Abstract classes cannot be instantiated. Abstract classes need to be inherited. You can override an abstract class's method by using the keyword virtual in its method signature. Abstract methods define no body of code to execute and must have an implementation provided by a concrete class. A concrete class defines the concrete implementation of an abstract class and defines all abstract members. You can only instantiate a concrete type.
- An abstract class might be used to represent the similarities between two different concrete classes.
- An abstract class cannot be instatiated but it's members may be implemented and extended.

## Interfaces
An interface defines the signatures of methods, events and properies. An interface is a type just like a class, struct, and delegates are types. A Type can implement as many interfaces as it needs. You can't define private signatures of methods because the class that implements an interface must have access to the methods of properties. 
- An interface can be used to represent similar functionalities across concrete classes that are very much dissimilar.
- An interface can be implemented.

# Computer Science Fundamentals

- `UML`: Unified Modeling Language. A general-purpose, developmental, modeling language, that is intended to provide a standard way to visualize the design of a system.
- `Joel Spolsky`: [Good CS articles from the late 90s and early 00s](https://www.joelonsoftware.com/category/reading-lists/top-10/)
   - leaky abstractions
   - character encoding
   - [12 steps to better code](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/)

# SOLID Principles

## Single Responsibility Principle

Strive for low coupling and high cohesion. Creating smaller classes with distinct responsibilities result in a more flexible design.

- `Cohesion`: How strongly-related and focused are the various responsibilities of a module
- `Coupling`: The degree to which each program module relies on each one of the other modules.

## Open / Closed Principle
Software entities should be opened for extension, but closed for modification. 
- `Open to extension`: New behavior can be added in the future 
- `Closed to modification`: Changes to source code are limited.

adding additional behavior is best handled by adding new classes that have no dependencies yet.

There are three approaches to this:
1. parameter - Allow client to control behavior specifics via a prameter.

```c
public decimal CalculatePrice(CalculatorTypeEnum calcType, List<OrderItems> itmes) {
    decimal ttl = 0;
    swith (calcType)
        case: BogoCalculator
            ttl += ((int)i.Qty / 2) * i.Amt;
            ttl += (i.Qty % 2) * i.Amt;
            break;
        case: ....
    }
    return ttl;
}
```

2. inheritance / template pattern - child types override behavoir of a base class or interface
3. composition / strategy pattern 
   - Use smaller classes that implement an interface to abstract away the logic of each operation. "Creating an abstraction" is like coming up with an interface.

```c
interface ICalculator
decimal calculatePrice(OrderItem i)

// standard price calculator
public class StandardCalculator : ICalculator {
    public decimal calculatePrice(OrderItem i) {
        return i.Qty * i.Amt;
    }
}

// takes 10% off
public class SaleCalculator : ICalculator {
    public decimal calculatePrice(OrderItem i) {
        return i.Qty * i.Amt * .9;
    }
}

public class BogoCalculator : ICalculator {
    public decimal calculatePrice(OrderItem i) {
        decimal ttl = 0;
        ttl += ((int)i.Qty / 2) * i.Amt;
        ttl += (i.Qty % 2) * i.Amt;
        return ttl;
    }
}

public class OrderItem {
    public string Nm;
    public decimal Amt;
    public int Qty;
}

public class Cart {
    private ICalculator _calc; 
    public Cart(ICalculator calc) { 
        _calc = calc; 
    }
    pubblic string Checkout(list<OrderItem> items) {
        decimal total = 0;
        foreach (OrderItem item in items)
            total += _calc.calculatePrice(item);
        return total;
    }
}

public static Main(string[] args) {
    Console.WriteLine("1: Standard Calculator 2: Sale Calculator 3: BOGO Calculator");
    string s = Console.ReadLine();

    Cart c;
    if (s == "1") 
        c = new Cart(new StandardCalculator());
    else if (s == "2")
        c = new Cart(new SaleCalculator());
    else 
        c = new Cart(new BogoCalculator());

    List<OrderItems> items = new List<OrderItem>() { 
        new OrderItem() { Nm = "item1", Amt = 4.29, Qty = 2 },
        new OrderItem() { Nm = "item2", Amt = 9.33, Qty = 1 },
        new OrderItem() { Nm = "item3", Amt = 7.35, Qty = 3 }
    };

    string result = c.CheckOut(items);
    console.WriteLine(result);
}
```

## Liskov Substitution Principle

States that client code (calling code) expects that all implementations of a base class should be interchangeable with the base class. That means that all methods of the base class should be implemented.

<!-- Child clases must not remove behavior from their base class. -->

- calling code should not know that there is any difference between a derived type and its base type.
- the Liskov Substitution Principle suggests that IS-A should be replaced with IS-SUBSTITUTABLE-FOR

<!-- Invariants - are things that have to do with the integrity of the model your classes represent. They consist of reasonable assumptions of behavior by client code. -->

### How do you know you have a LSP issue? 

The Liskov Substitution Principle helps avoid bad abstractions (and avoids Bad Code Smells):

- notice bad code smells. i.e.: switch case statements for child objects of a base class to account for issues with the base class not abstracting the functionality away.

### example of a bad code smell

IStaff should abstract away how to print a Staff Member, and should not rely on an external class to implement how each Staff (employee, manager, etc) prints.

```c
static class Printer {
    static void PrintManager(Manager m) { ... }
    static void PrintEmployee(Employee e) { ... }
} 

interface IStaff { public string fullname; ... }

class Employee : IStaff { ... }
class Manager : IStaff { ... }

class Program() {
    static List<IStaff> _staffers = new List<IStaff>(
        new Employee("Tom"),
        new Manager ("Ralphie")
    )
    static void Main()
    {
        foreach (IStaff staff in _staffers)
        {
            if (staff is Manager)
                Printer.PrintManager(staff as Manager);
            else
                Printer.PrintEmployee(staff as Employee);
        }
    }
}
```

### LSP Tips

States that client code (calling code) expects that all implementations of a base class should be interchangeable with the base class. That means that all methods of the base class should be implemented.

<!-- Tell Don't Ask

- Don't interigate objects for their internals: move behavior to the object
- tell the object what you want it to do.

Consider refactoring to a new Base Class -->

- Given two classes that share a lot of behavior, create a third class (an abstraction) that both can derive from. Ensure substitutability is retained between each class and the new base.
- Non-substitutable code breaks polymorphism.
- fixing substitutability problems with a switch case (and not LSP) quickly become a maintenance nightmare and violates the Open / Closed Principle. The more cases you add to your switch, the more you realize you need an abstraction.

## Interface Segregation Principle 
- ISP can help you create projects or applications that have fewer hidden dependencies and are more cohesive and easier to maintain. 
- keep interfaces separate to avoid lenghty implementations of the interface. (avoid leaving behind methods that `throw new NotImplementedException();`)
- throwing NotImplementedException breaks the Liskov Substitution Principle because any non implemented methods cannot be substituted for their base class methods. I.E.: The clients expects that the entire interface be implemented.
- avoid "Fat" interfaces (an interface becomes fat when you realize that you don't want to implement all the methods of an interface for your new implementations purposes). Clients should not be forced to depend on methods that they do not use (IE: implement a bunch of methods of an interface they don't need.)
- The facade pattern lets you take a large set of complex classes and replace them with a much simpler class that offers only the subset or interface that the client actually needs.
- if you find yourself depending on your own Fat interface, then fix with ISP
- if you find yourself depending on a Fat interface that you do not own, then create a smaller interface using only what you need, then implement that interface using an adapter that implements the full interface.

## Dependency Inversion Principle

The Dependency Inversion Principle states that High-level modules should not depend on low-level modules, and abstractions should not depend on details, but details shoudl depend on abstractions.

- would you solder a lamp to the electical outlet?

what is a dependency?
- third party libraries
- database - wrap in such a way that is not an implicit dependency in your code.
- file system
- email
- web service
- system resources - clock, ie datetime.now
- configuration - a file
- the NEW keyword - limit the number of places you allow your application to instantiate new objects.
- static methods - any time you add a static method to your code that cannot easily be separated from the calling code.
- thread.sleep and random - very dificult to write tests for

Traditional programming and dependencies
- high level modules call low level modules
- a User Interface depends on 
  - buisines logic depends on
    - infrastructure
    - utility 
    - data access
- static methods are used for convenience or as a facade
- class instantiation / cal stack logic is scattered through all modules
  - violates the Single Responsibility Principle 
    - because every class that decides who it's colaborators are (through the use of static methods or use of the new keyword) is now responsible for its actual work but also for who determining who it's working with
    - these are actually separte responsibilities that the single responsibility principle dictates we should separate into seprate classes.

Class Dependencies:
- class constructors should require any dependencies the class needs
  - these are called expicit dependencies
  - classes that do not have Implicit (or hidden) dependencies

Holywood Principle - "don't call us, we'll call you"

DI types
- constructor injector 
    - dependencies are passed in via the consturctor 
    - explicitly stating what it needs to run properly 
    - classes are always in a valid state once constructed
- propery injection (setter injection)
  - dependencies can be changed at any time during object lifetime
  - objects may be in an invalid state between construction and setting
  - less intuitive.
- parameter injection
  - flexible
  - breaking the method signature might be costly

Refactoring
- extract dependencies into interfaces
- inject implementations of interfaces into the class being refactored 
- reduce responsibilities

DIP Smells
- use of new keyword.
- use of static methods or properties

where do we instantiate these objects
- default contsructor
  - create a default constructor that news up a default implementation of each explicit dependency.
  - also known as a poor man's IOC
- main
  - manually instantiate each object at the start

IOC Containers
- responsible for object graph instantiation - determines what is going to be used when an interface is called for.
- init'd at app startup or in config
- managed interfaces and the implementation to be used are registered with the container
  - register a specific concrete class to be resolved for an interface
- dependencies are resolved at app startup or runtime

- examples of IOC containers
  - microsoft Unity
  - structureMap
  - ninject
  - windsor
  - funq / munq

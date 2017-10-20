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

substypes must be substitutable for their base types

- if it looks like a duck and quacks like a duck, but it needs batteries, you probably have the wrong abstraction.

Child clases must not remove behavior from their base class.

- calling code should not know that there is any difference between a derived type and its base type.
- the Liskov Substitution Principle suggests that IS-A should be replaced with IS-SUBSTITUTABLE-FOR

Invariants - are things that have to do with the integrity of the model your classes represent. They consist of reasonable assumptions of behavior by client code.

### How do you know you have a LSP issue? 

The Liskov Substitution Principle helps avoid bad abstractions (and avoids Bad Code Smells):

- notice bad code smells. i.e.: switch case statements for child objects of a base class to account for issues with the base class not abstracting the functionality away.

### example of a bad code smell

```c
static class Printer {
    static PrintManager(Manager m)
    static PrintEmployee(Employee e)
} 

class Employee : IStaff { }
class Manager : IStaff { }

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

Tell Don't Ask

- Don't interigate objects for their internals: move behavior to the object
- tell the object what you want it to do.

Consider refactoring to a new Base Class

- given two classes that share a lot of behavior but are not substitutable
- create a third class that both can derive from
- ensure substitutability is retained between each class and the new base.



- Non-substitutable code breaks polymorphism.
- Client code expects child classes to work in place of their base class
- fixing substitutability problems with a switch case quickly become a maintenance nightmare and violates the Open / Closed Principle.

## Interface Segregation Principle 
- keep interfaces separate to avoid lenghty implementations of the interface. (avoid `throws new NotImplementedException();`)
- 
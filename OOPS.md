1️⃣ Inheritance vs Polymorphism

Inheritance:
Inheritance allows a subclass to inherit the properties and methods of a superclass. It promotes code reuse and establishes an “is-a” relationship.
Example: A Dog class can inherit from an Animal class.

Polymorphism:
Polymorphism allows the same method to behave differently depending on the object that calls it.
Types of polymorphism:

Compile-time polymorphism (method overloading): Same method name but different parameters.

Run-time polymorphism (method overriding): A subclass provides a specific implementation of a method already defined in the parent class.

2️⃣ Method Overloading vs Method Overriding

Overloading: Same method name but different parameter list (number or type of parameters). Happens in the same class.

Overriding: Subclass provides its own implementation of a parent class method with the same name and same parameters.

3️⃣ Encapsulation

Encapsulation is the concept of bundling data (variables) and methods that operate on that data into a single unit, usually a class.
It also hides the internal state of the object and only exposes it through controlled methods (getters and setters).
This ensures better security and control over the data.

4️⃣ Abstraction

Abstraction means showing only the essential features of an object while hiding the background details.
It allows you to focus on what an object does rather than how it does it.
Example: Using a car—you just use the steering and pedals without knowing the internal engine details.

5️⃣ Class vs Object

Class: A blueprint or template that defines properties and behaviors.

Object: A real-world instance of a class created in memory.

6️⃣ Access Modifiers

Access modifiers define the visibility of classes, methods, and variables.

In C++

Public: Accessible from anywhere.

Protected: Accessible within the same class and its derived classes.

Private: Accessible only within the same class.

In Java

Public: Accessible from anywhere.

Protected: Accessible within the same package and also by subclasses in other packages.

Private: Accessible only within the same class.

Default (package-private): No modifier → accessible only within the same package.

7️⃣ Difference between Protected and Default (Java)

Protected: Accessible within the same package and also by subclasses outside the package.

Default: Accessible only within the same package.

8️⃣ Interface vs Abstract Class (short)

Abstract class: Can have both abstract (unimplemented) and concrete (implemented) methods.

Interface: Only contains abstract methods (Java 8+ can have default/static methods). A class can implement multiple interfaces.

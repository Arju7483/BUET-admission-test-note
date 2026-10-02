# 📘 OOP — Comprehensive Notes for BUET MSc Exam
### Single Source of Truth | MCQ + Written Exam Ready

> **Coverage**: Four Pillars • Classes & Objects • Constructors/Destructors • Inheritance • Polymorphism & Virtual Functions • Abstract Classes & Interfaces • SOLID Principles • Design Patterns • UML Class Diagrams • Exception Handling • C++ vs Java Key Differences

---

## 📑 Table of Contents
1. [OOP Philosophy & Advantages](#1-oop-philosophy--advantages)
2. [Classes & Objects](#2-classes--objects)
3. [Encapsulation & Access Specifiers](#3-encapsulation--access-specifiers)
4. [Abstraction](#4-abstraction)
5. [Constructors & Destructors](#5-constructors--destructors)
6. [Inheritance](#6-inheritance)
7. [Polymorphism](#7-polymorphism)
8. [Virtual Functions & vtable (C++)](#8-virtual-functions--vtable-c)
9. [Abstract Classes & Interfaces](#9-abstract-classes--interfaces)
10. [SOLID Principles](#10-solid-principles)
11. [Key Design Patterns](#11-key-design-patterns)
12. [UML Class Diagrams](#12-uml-class-diagrams)
13. [Exception Handling](#13-exception-handling)
14. [C++ vs Java — Key Differences](#14-c-vs-java--key-differences)
15. [MCQ Quick-Fire Facts](#15-mcq-quick-fire-facts)

---

## 1. OOP Philosophy & Advantages

### What is OOP?
Object-Oriented Programming is a **programming paradigm** that organizes software design around **objects** (data + behavior) rather than functions and logic.

### OOP vs Procedural Programming

| Feature | Procedural | OOP |
|---|---|---|
| Focus | Functions/procedures | Objects (data + behavior) |
| Data Access | Global data, easy to access | Encapsulated, controlled access |
| Code Reuse | Functions reused | Classes, Inheritance |
| Maintenance | Harder to modify | Easier via encapsulation |
| Real-world modeling | Poor | Excellent |
| Example Languages | C, Pascal | C++, Java, Python |

### Four Core Pillars of OOP
```
+-----------------------------------------------------+
|                   OOP PILLARS                        |
|                                                      |
|  +------------+  +------------+  +----------------+ |
|  |Encapsulation|  | Abstraction|  |  Inheritance   | |
|  |(Data Hiding)|  |(Hide Impl) |  |(Code Reuse)    | |
|  +------------+  +------------+  +----------------+ |
|                  +--------------+                    |
|                  | Polymorphism |                    |
|                  | (Many Forms) |                    |
|                  +--------------+                    |
+-----------------------------------------------------+
```

---

## 2. Classes & Objects

### Class
- A **blueprint/template** for creating objects
- Defines **attributes** (data members) and **methods** (member functions)
- Does NOT occupy memory until instantiated

### Object
- An **instance** of a class
- Occupies memory
- Has its own copy of instance variables (but shares methods)

```cpp
// C++ Example
class Car {
private:
    string brand;    // attribute
    int speed;

public:
    void accelerate() { speed += 10; }  // method
    string getBrand() { return brand; }
};

Car myCar;            // object — instance of Car
Car* ptr = new Car(); // object via pointer (heap allocation)
```

### Static vs Instance Members

| | Instance Member | Static Member |
|---|---|---|
| Belongs to | Each object | Class itself |
| Memory | Separate per object | Shared (one copy) |
| Access | `obj.member` | `ClassName::member` (C++) or `ClassName.member` (Java) |
| Use case | Object-specific data | Common data / utility functions |

```java
class Counter {
    static int count = 0;    // static — shared
    int id;                  // instance — per object

    Counter() {
        count++;
        id = count;
    }
}
```

> **MCQ Trap**: Static methods **cannot** access instance variables directly (no `this` pointer).

---

## 3. Encapsulation & Access Specifiers

### Encapsulation
**Definition**: Bundling data (attributes) and the methods that operate on that data into a single unit (class), **restricting direct access** to the internal state.

**Key principle**: Data should be `private`; access provided via `public` getter/setter methods.

```
  +----------------------------------+
  |           Class (Capsule)        |
  |  +----------------------------+  |
  |  |  Private Data (hidden)     |  |  <- Protected from outside
  |  |  - name, age, salary       |  |
  |  +----------------------------+  |
  |  +----------------------------+  |
  |  |  Public Methods (exposed)  |  |  <- Interface to the world
  |  |  - getName(), setAge()     |  |
  |  +----------------------------+  |
  +----------------------------------+
```

### Access Specifiers

| Specifier | Same Class | Same Package/Subclass | Other Classes |
|---|---|---|---|
| `private` | YES | NO | NO |
| `protected` | YES | YES | NO |
| `public` | YES | YES | YES |
| (default/package) Java | YES | YES (same pkg) | NO |

> **C++ Note**: `protected` means accessible in derived classes. `private` is NOT accessible in derived classes.

### Benefits of Encapsulation
1. **Data hiding** — prevents accidental corruption
2. **Validation** — setters can enforce constraints
3. **Flexibility** — change internal implementation without affecting external code
4. **Maintainability** — reduces coupling

---

## 4. Abstraction

### Definition
Hiding **implementation details** and exposing only **essential features** (what an object does, not how it does it).

### Abstraction vs Encapsulation

| | Abstraction | Encapsulation |
|---|---|---|
| Focus | **What** an object does | **How** data is protected |
| Achieved via | Abstract classes, Interfaces | Access specifiers (private/protected) |
| Goal | Simplify complexity | Data security |
| Level | Design level | Implementation level |

### Example
```java
// Abstraction — user knows WHAT it does, not HOW
abstract class Shape {
    abstract double area();   // "what" — just declared
}

class Circle extends Shape {
    double r;
    double area() { return 3.14 * r * r; }  // "how" — hidden in subclass
}
```

---

## 5. Constructors & Destructors

### Constructors
Special methods called automatically when an object is created.

**Rules**:
- Same name as class
- **No return type** (not even void)
- Called automatically on object creation
- Can be overloaded

#### Types of Constructors

| Type | Description | Example |
|---|---|---|
| **Default** | No parameters, compiler provides if none defined | `Student()` |
| **Parameterized** | Takes arguments to initialize | `Student(int id, String name)` |
| **Copy** | Creates object as copy of another (C++) | `Student(const Student& s)` |

```cpp
class Student {
    int id;
    string name;
public:
    Student() { id = 0; name = ""; }                       // Default
    Student(int i, string n) { id = i; name = n; }        // Parameterized
    Student(const Student& s) { id = s.id; name = s.name; } // Copy
};
```

### Constructor Execution Order in Inheritance

```
Base constructor -> Derived constructor (top-down on creation)
Derived destructor -> Base destructor  (bottom-up on destruction)

Example:
class A { A() { cout << "A"; } ~A() { cout << "~A"; } };
class B : public A { B() { cout << "B"; } ~B() { cout << "~B"; } };
class C : public B { C() { cout << "C"; } ~C() { cout << "~C"; } };

C obj;
// Output on creation:    A -> B -> C
// Output on destruction: ~C -> ~B -> ~A
```

### Destructors
- Called automatically when object goes out of scope or `delete` is called
- **No parameters**, **no return type**, **cannot be overloaded**
- Only **one destructor** per class
- **Must be `virtual`** in base class if polymorphism is used (prevents resource leaks)

```cpp
class Base {
public:
    virtual ~Base() { cout << "Base destructor\n"; }  // MUST be virtual!
};

class Derived : public Base {
public:
    ~Derived() { cout << "Derived destructor\n"; }
};

Base* ptr = new Derived();
delete ptr;  // Without virtual: only Base destructor called (memory leak!)
             // With virtual: Derived destructor -> Base destructor (correct!)
```

> **MCQ Trap**: Always use `virtual` destructor in base class when using polymorphism in C++.

### Java: No Destructors
- Java uses **Garbage Collection (GC)** — no manual memory management
- `finalize()` method was deprecated (Java 9+)
- Use `try-with-resources` / `AutoCloseable` for resource cleanup

---

## 6. Inheritance

### Definition
A mechanism where a new class (**derived/child class**) inherits properties and behaviors from an existing class (**base/parent class**).

**IS-A relationship**: A Dog IS-A Animal -> inheritance is appropriate.

### Types of Inheritance

```
1. Single Inheritance        2. Multilevel Inheritance
   A -> B                       A -> B -> C

3. Multiple Inheritance      4. Hierarchical Inheritance
   A, B -> C                    A -> B, A -> C, A -> D

5. Hybrid Inheritance
   Combination of above (can cause Diamond Problem)
```

```cpp
// C++ — Multiple Inheritance
class Flyable { public: void fly() {} };
class Swimmable { public: void swim() {} };
class Duck : public Flyable, public Swimmable {};  // Multiple inheritance
```

> **Java does NOT support multiple inheritance** with classes (to avoid the Diamond Problem). It uses **interfaces** instead.

### The Diamond Problem (C++)

```
      A (int data)
     / \
    B   C    <- B and C both inherit A
     \ /
      D       <- D inherits both -> ambiguity: which A's data?
```

**Solution in C++**: Virtual inheritance
```cpp
class B : virtual public A {};
class C : virtual public A {};
class D : public B, public C {};  // Only one copy of A
```

### Access Control in Inheritance (C++)

| Base Member | public inheritance | protected inheritance | private inheritance |
|---|---|---|---|
| `public` | public | protected | private |
| `protected` | protected | protected | private |
| `private` | not accessible | not accessible | not accessible |

### Method Overriding
- Derived class provides **specific implementation** of a method already defined in base class
- Same method signature (name + parameters)
- Enables **runtime polymorphism**

```java
class Animal {
    void sound() { System.out.println("Some sound"); }
}
class Dog extends Animal {
    @Override
    void sound() { System.out.println("Woof!"); }  // Overriding
}
```

### `super` and `this` Keywords (Java)

| Keyword | Purpose |
|---|---|
| `this` | Refers to current object |
| `super` | Refers to parent class; used to call parent constructor/method |

---

## 7. Polymorphism

### Definition
Ability of an entity to take **many forms**. One interface, many implementations.

### Two Types

```
           Polymorphism
          /             \
   Compile-time        Run-time
   (Static Dispatch)   (Dynamic Dispatch)
   /         \              \
Method     Operator      Method
Overloading Overloading   Overriding
                        (via virtual functions)
```

### A) Compile-time Polymorphism (Method Overloading)
Same method name, **different parameter list** (number, type, or order).
Resolved at **compile time** by the compiler.

```java
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }    // different type
    int add(int a, int b, int c) { return a + b + c; }  // different count
}
```

> **MCQ Trap**: You **cannot** overload by return type alone!
> ```java
> int foo() {}
> double foo() {}  // COMPILE ERROR — same signature
> ```

### B) Operator Overloading (C++ only)
Giving new meaning to existing operators for user-defined types.

```cpp
class Complex {
    double real, imag;
public:
    Complex operator+(const Complex& c) {
        return Complex(real + c.real, imag + c.imag);
    }
};

Complex a, b, c;
c = a + b;  // calls operator+
```

> **Java does NOT support operator overloading** (except `+` for String concatenation, built-in).

### C) Runtime Polymorphism (Method Overriding + Virtual Functions)
Correct method version determined at **runtime** based on actual object type.

```java
Animal a = new Dog();   // Base class reference, derived object
a.sound();              // Calls Dog's sound() — runtime decision
```

### Overloading vs Overriding Summary

| Feature | Overloading | Overriding |
|---|---|---|
| Class | Same class | Different classes (parent-child) |
| Method signature | Different | Same |
| Return type | Can differ | Must be same (or covariant) |
| Binding | Compile-time | Runtime |
| Access modifier | Any | Cannot reduce visibility |
| `static` methods | Can be overloaded | **Cannot** be overridden |

---

## 8. Virtual Functions & vtable (C++)

### Virtual Functions
- Declared with `virtual` keyword in base class
- Enable **runtime polymorphism** — correct version called based on actual object type, not pointer type
- Without `virtual`, method resolution is based on **pointer type** (static binding)

```cpp
class Animal {
public:
    virtual void speak() { cout << "..."; }    // virtual
    void breathe() { cout << "inhale"; }        // non-virtual
};

class Dog : public Animal {
public:
    void speak() override { cout << "Woof!"; } // overrides
    void breathe() { cout << "pant"; }          // hides, not overrides
};

Animal* a = new Dog();
a->speak();    // "Woof!" — virtual, runtime dispatch
a->breathe();  // "inhale" — non-virtual, compile-time dispatch (Animal's version)
```

### How vtable Works — The Mechanism

```
+-------------------------------------------------------------+
|                    VTABLE MECHANISM                          |
|                                                              |
|  Each class with virtual functions has a vtable (array of   |
|  function pointers). Each object has a hidden vptr.         |
|                                                              |
|  Animal class vtable:          Dog class vtable:            |
|  +----------------------+      +---------------------+      |
|  | speak -> Animal::speak|      | speak -> Dog::speak |      |
|  +----------------------+      +---------------------+      |
|                                                              |
|  Animal* a = new Dog();                                      |
|                                                              |
|  Object in memory:                                           |
|  +------+--------------------+                              |
|  | vptr | -> Dog's vtable    |  <- Points to Dog's vtable   |
|  | data |                    |                              |
|  +------+--------------------+                              |
|                                                              |
|  a->speak()  -> lookup vptr -> Dog's vtable -> Dog::speak    |
+-------------------------------------------------------------+
```

### `override` and `final` Keywords (C++11)

```cpp
class Base {
    virtual void foo() {}
    virtual void bar() {}
};

class Derived : public Base {
    void foo() override {}        // OK — tells compiler this overrides
    void foo(int x) override {}   // COMPILE ERROR — no matching base method
    void bar() final {}           // bar cannot be overridden further
};
```

### Pure Virtual Functions
- Declared with `= 0`
- **No implementation** in base class
- Makes the class **abstract** (cannot instantiate)
- Derived class **must** implement it (or it too becomes abstract)

```cpp
class Shape {
public:
    virtual double area() = 0;    // pure virtual
    virtual void draw() = 0;      // pure virtual
};

class Circle : public Shape {
    double r;
public:
    double area() override { return 3.14 * r * r; }  // must implement
    void draw() override { /* draw circle */ }
};

Shape s;       // ERROR — cannot instantiate abstract class
Circle c;      // OK
```

---

## 9. Abstract Classes & Interfaces

### Abstract Class

**Definition**: A class that **cannot be instantiated** directly and may contain pure virtual methods (C++) or abstract methods (Java).

#### C++ Abstract Class
```cpp
class Vehicle {       // Abstract class
public:
    virtual void start() = 0;             // pure virtual (must override)
    void refuel() { cout << "Refueling..."; }  // concrete method (can override)
};
```

#### Java Abstract Class
```java
abstract class Vehicle {
    abstract void start();                      // abstract method (must override)
    void refuel() { System.out.println("Refueling"); }  // concrete method
    int year;                                   // can have fields
}
```

### Interface (Java)

**Definition**: A **contract** specifying what a class must do (not how). All methods are implicitly `public abstract` (before Java 8).

```java
interface Drawable {
    void draw();          // implicitly public abstract
    double area();
}

interface Resizable {
    void resize(double factor);
}

// A class can implement MULTIPLE interfaces
class Circle implements Drawable, Resizable {
    public void draw() { /* ... */ }
    public double area() { return 3.14 * r * r; }
    public void resize(double f) { r *= f; }
}
```

### Abstract Class vs Interface — Critical Comparison

| Feature | Abstract Class | Interface |
|---|---|---|
| Instantiation | Cannot | Cannot |
| Constructors | YES | NO |
| Fields/Variables | Any type | Only `public static final` (constants) |
| Method types | Abstract + Concrete | Abstract (Java 8+: default/static) |
| Multiple inheritance | Only one class | Multiple interfaces |
| Access modifiers | Any | `public` only |
| When to use | Shared code + partial impl | Define a contract/capability |
| IS-A vs CAN-DO | IS-A relationship | CAN-DO capability |

```
Abstract Class -> "Dog IS-A Animal" (use extends)
Interface      -> "Duck CAN-DO Flyable, Swimmable" (use implements)
```

### Java 8+ Interface Features
```java
interface Logger {
    void log(String msg);                       // abstract
    default void warn(String msg) {             // default method (Java 8)
        log("WARN: " + msg);
    }
    static Logger getDefault() {               // static method (Java 8)
        return msg -> System.out.println(msg);
    }
}
```

---

## 10. SOLID Principles

### Why SOLID?
- Avoid **code rot** (rigid, fragile, immobile code)
- Makes code **maintainable, extensible, testable**
- Frequently tested in BUET exam as scenario-based MCQs

```
S — Single Responsibility Principle (SRP)
O — Open/Closed Principle (OCP)
L — Liskov Substitution Principle (LSP)
I — Interface Segregation Principle (ISP)
D — Dependency Inversion Principle (DIP)
```

### S — Single Responsibility Principle (SRP)
> **"A class should have only ONE reason to change."**

```java
// VIOLATES SRP — two responsibilities
class Employee {
    void calculateSalary() { }    // HR responsibility
    void generateReport() { }     // Reporting responsibility
}

// FOLLOWS SRP
class Employee { void calculateSalary() { } }
class EmployeeReport { void generateReport(Employee e) { } }
```

### O — Open/Closed Principle (OCP)
> **"Open for extension, closed for modification."**
> Add new behavior by adding new code, not changing existing code.

```java
// VIOLATES OCP — must modify class to add new shape
class AreaCalculator {
    double area(Object shape) {
        if (shape instanceof Circle) return ...;
        if (shape instanceof Square) return ...;  // must edit here for new shapes
    }
}

// FOLLOWS OCP — extend by adding new class
interface Shape { double area(); }
class Circle implements Shape { public double area() { return 3.14 * r * r; } }
class Square implements Shape { public double area() { return side * side; } }
// Add Triangle? Just create new class — don't touch existing code
```

### L — Liskov Substitution Principle (LSP)
> **"Subclass objects must be substitutable for base class objects without breaking behavior."**

```java
// VIOLATES LSP — Square is NOT substitutable for Rectangle
class Rectangle {
    void setWidth(int w) { this.w = w; }
    void setHeight(int h) { this.h = h; }
    int area() { return w * h; }
}

class Square extends Rectangle {
    void setWidth(int w) { this.w = w; this.h = w; }   // breaks contract!
    void setHeight(int h) { this.w = h; this.h = h; }  // breaks contract!
}

// Client code expecting Rectangle breaks with Square:
Rectangle r = new Square();
r.setWidth(5); r.setHeight(3);
r.area();  // Expected 15, got 9 — VIOLATION!
```

**Fix**: Square and Rectangle should not have IS-A relationship; use a common `Shape` interface.

### I — Interface Segregation Principle (ISP)
> **"Clients should not be forced to depend on methods they do not use."**
> Prefer small, specific interfaces over one large "fat" interface.

```java
// VIOLATES ISP — fat interface
interface Worker {
    void work();
    void eat();   // Robots don't eat!
}

// FOLLOWS ISP — segregated interfaces
interface Workable { void work(); }
interface Eatable { void eat(); }

class Human implements Workable, Eatable { }
class Robot implements Workable { }    // doesn't implement Eatable
```

### D — Dependency Inversion Principle (DIP)
> **"Depend on abstractions, not concretions."**
> High-level modules should not depend on low-level modules; both should depend on abstractions.

```java
// VIOLATES DIP — high-level depends on low-level
class MySQLDatabase { void save(String data) { } }
class UserService {
    MySQLDatabase db = new MySQLDatabase();  // tightly coupled!
    void saveUser(User u) { db.save(u.toString()); }
}

// FOLLOWS DIP — depend on abstraction
interface Database { void save(String data); }
class MySQLDatabase implements Database { public void save(String data) { } }
class MongoDatabase implements Database { public void save(String data) { } }

class UserService {
    Database db;   // depends on abstraction
    UserService(Database db) { this.db = db; }  // Dependency Injection
    void saveUser(User u) { db.save(u.toString()); }
}
```

---

## 11. Key Design Patterns

Design patterns are **proven, reusable solutions** to commonly occurring problems. Three categories:

```
+---------------------------------------------------------+
|                  DESIGN PATTERN TYPES                    |
|                                                          |
|  Creational          Structural        Behavioral        |
|  (Object Creation)   (Composition)     (Communication)   |
|                                                          |
|  - Singleton         - Adapter         - Strategy        |
|  - Factory           - Decorator       - Observer        |
|  - Abstract Factory  - Facade          - Command         |
|  - Builder           - Composite       - Iterator        |
|  - Prototype         - Proxy           - Template Method |
+---------------------------------------------------------+
```

---

### Pattern 1: Singleton (Creational) [HIGH PRIORITY]

**Intent**: Ensure a class has **only one instance** and provide a global access point.

**When to use**: Logger, Configuration manager, Database connection pool, Thread pools.

```java
public class Logger {
    private static Logger instance;     // single instance

    private Logger() { }               // private constructor — prevents new Logger()

    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger();   // lazy initialization
        }
        return instance;
    }

    public void log(String msg) { System.out.println(msg); }
}

// Usage:
Logger.getInstance().log("Starting app");
Logger.getInstance().log("App running");
// Both calls return the SAME object
```

**UML Diagram**:
```
+----------------------------------+
|            Logger                |
+----------------------------------+
| - instance: Logger (static)      |
+----------------------------------+
| - Logger()                       |  <- private constructor
| + getInstance(): Logger (static) |  <- global access point
| + log(msg: String)               |
+----------------------------------+
```

**Thread-safe Singleton** (important for written exam):
```java
// Double-checked locking
public static Logger getInstance() {
    if (instance == null) {
        synchronized(Logger.class) {
            if (instance == null) instance = new Logger();
        }
    }
    return instance;
}
```

> **MCQ from BUET 21**: "In a database management system, we can use sequence number for identification of rows. What pattern for multiple users generating sequence number?" -> **Singleton**

---

### Pattern 2: Factory Method (Creational) [HIGH PRIORITY]

**Intent**: Define an interface for creating an object, but let subclasses decide which class to instantiate.

**When to use**: When you don't know ahead of time what class you need to instantiate.

```java
// Product interface
interface Shape {
    void draw();
}

// Concrete products
class Circle implements Shape {
    public void draw() { System.out.println("Drawing Circle"); }
}
class Rectangle implements Shape {
    public void draw() { System.out.println("Drawing Rectangle"); }
}

// Factory class
class ShapeFactory {
    public Shape getShape(String type) {
        if (type.equals("CIRCLE")) return new Circle();
        if (type.equals("RECTANGLE")) return new Rectangle();
        return null;
    }
}

// Client
ShapeFactory factory = new ShapeFactory();
Shape s = factory.getShape("CIRCLE");
s.draw();   // Drawing Circle
```

**UML Diagram**:
```
+-----------------+        +---------------+
|  ShapeFactory   |        |    Shape      | <<interface>>
+-----------------+        +---------------+
| + getShape(type)|------->| + draw()      |
+-----------------+        +---------------+
                                   ^
                          +--------+---------+
                     +----+----+       +-----+-----+
                     | Circle  |       | Rectangle |
                     +---------+       +-----------+
```

> **Advantages**: Follows OCP, promotes loose coupling, centralizes object creation.
> **MCQ from BUET 21**: "Which is NOT the advantage of factory pattern?"

---

### Pattern 3: Strategy (Behavioral) [HIGH PRIORITY]

**Intent**: Define a family of algorithms, encapsulate each one, make them interchangeable at runtime.

**When to use**: Multiple sorting algorithms, payment methods, compression strategies.

> **This pattern appeared directly in BUET MSc 24 written exam!**

```java
// Strategy interface
interface SortStrategy {
    void sort(int[] arr);
}

// Concrete strategies
class BubbleSort implements SortStrategy {
    public void sort(int[] arr) { /* bubble sort impl */ }
}
class QuickSort implements SortStrategy {
    public void sort(int[] arr) { /* quick sort impl */ }
}
class MergeSort implements SortStrategy {
    public void sort(int[] arr) { /* merge sort impl */ }
}

// Context class
class Sorter {
    private SortStrategy strategy;

    public void setStrategy(SortStrategy s) { this.strategy = s; }
    public void sort(int[] arr) { strategy.sort(arr); }
}

// Client
Sorter sorter = new Sorter();
int[] data = {5, 2, 8, 1};

// Small list -> Bubble Sort
if (data.length < 10) sorter.setStrategy(new BubbleSort());
// Large list -> Merge Sort
else sorter.setStrategy(new MergeSort());

sorter.sort(data);
```

**UML Class Diagram** (as asked in BUET 24):
```
+---------------------------+        +---------------------+
|          Sorter           |        |   <<interface>>      |
|  (Context)                |        |   SortStrategy       |
+---------------------------+        +---------------------+
| - strategy: SortStrategy  |------->| + sort(arr: int[])  |
+---------------------------+        +---------------------+
| + setStrategy(SortStrategy)|                 ^
| + sort(arr: int[])        |        +---------+-----------+
+---------------------------+   +----+----+ +--+-----+ +---+----+
                                 |Bubble  | |  Quick | |  Merge |
                                 |Sort    | |  Sort  | |  Sort  |
                                 +--------+ +--------+ +--------+
```

---

### Pattern 4: Observer (Behavioral)

**Intent**: One-to-many dependency — when **Subject** state changes, all **Observers** are notified automatically.

**When to use**: Event systems, notifications, MVC pattern (Model notifies View).

```java
import java.util.*;

// Observer interface
interface Observer {
    void update(String event);
}

// Subject
class EventSystem {
    private List<Observer> observers = new ArrayList<>();

    public void subscribe(Observer o) { observers.add(o); }
    public void unsubscribe(Observer o) { observers.remove(o); }

    public void notifyObservers(String event) {
        for (Observer o : observers) o.update(event);
    }

    public void triggerEvent(String event) {
        System.out.println("Event: " + event);
        notifyObservers(event);
    }
}

// Concrete observers
class EmailNotifier implements Observer {
    public void update(String event) { System.out.println("Email: " + event); }
}
class SMSNotifier implements Observer {
    public void update(String event) { System.out.println("SMS: " + event); }
}

// Usage
EventSystem system = new EventSystem();
system.subscribe(new EmailNotifier());
system.subscribe(new SMSNotifier());
system.triggerEvent("New order placed");
// Output:
// Email: New order placed
// SMS: New order placed
```

---

### Pattern 5: Decorator (Structural)

**Intent**: Attach additional responsibilities to an object dynamically. Wraps the object.

**When to use**: Adding features without modifying class (follows OCP).

```java
interface Coffee {
    String getDescription();
    double getCost();
}

class SimpleCoffee implements Coffee {
    public String getDescription() { return "Coffee"; }
    public double getCost() { return 1.0; }
}

// Decorator
class MilkDecorator implements Coffee {
    private Coffee coffee;
    MilkDecorator(Coffee c) { this.coffee = c; }
    public String getDescription() { return coffee.getDescription() + ", Milk"; }
    public double getCost() { return coffee.getCost() + 0.5; }
}

class SugarDecorator implements Coffee {
    private Coffee coffee;
    SugarDecorator(Coffee c) { this.coffee = c; }
    public String getDescription() { return coffee.getDescription() + ", Sugar"; }
    public double getCost() { return coffee.getCost() + 0.25; }
}

// Usage
Coffee c = new SimpleCoffee();
c = new MilkDecorator(c);
c = new SugarDecorator(c);
System.out.println(c.getDescription() + " $" + c.getCost());
// Coffee, Milk, Sugar $1.75
```

---

## 12. UML Class Diagrams

### Notation Summary

```
+-----------------------+
|      ClassName        |   <- Class name (bold for concrete, italic for abstract)
+-----------------------+
| - privateAttr: Type   |   <- Attributes section
| # protectedAttr: Type |   <- # = protected, - = private, + = public
| + publicAttr: Type    |
+-----------------------+
| + method(): RetType   |   <- Methods section
| - helper(): void      |
+-----------------------+
```

### Relationship Types

```
A ----------> B     Association    "A uses B"
A - - - - -> B     Dependency     "A depends on B (temporarily)"
A -------<>  B     Aggregation    "A has B (B can exist without A)"
A -------<*> B     Composition    "A contains B (B dies with A)"
A -------/\  B     Inheritance    "A extends B" (open triangle to parent)
A - - - -/\  B     Realization    "A implements B" (dashed, interface)
```

### Multiplicity Notation
```
1         — exactly one
0..1      — zero or one (optional)
*  or 0..*— zero or more
1..*      — one or more
n         — exactly n
```

### Example: Online Shopping Class Diagram
```
+----------+         +--------------+         +-----------+
| Customer |1       *|    Order     |1       *| OrderItem |
+----------+*--------+--------------+*--------+-----------+
| -id      |         | -orderId     |         | -qty      |
| -name    |         | -date        |         | -price    |
| -email   |         | -status      |         +-----------+
+----------+         +--------------+         |+getTotal()|
|+placeOrder|        |+calcTotal()  |         +-----+-----+
+----------+         |+cancel()     |               |
                     +--------------+               | association
                                               +----+----+
                                               | Product |
                                               +---------+
                                               | -name   |
                                               | -price  |
                                               +---------+
```

---

## 13. Exception Handling

### Purpose
Handle **runtime errors** gracefully without crashing the program.

### Java Exception Hierarchy

```
                   Throwable
                  /          \
            Error           Exception
           (JVM errors)    /          \
                    RuntimeException   Checked Exceptions
                    (Unchecked)        (must be handled)
                    - NullPointerEx    - IOException
                    - ArrayIndexEx     - SQLException
                    - ClassCastEx      - FileNotFoundException
```

### Java Exception Handling

```java
try {
    // risky code
    int result = 10 / 0;           // ArithmeticException
    String s = null;
    s.length();                    // NullPointerException
} catch (ArithmeticException e) {
    System.out.println("Math error: " + e.getMessage());
} catch (NullPointerException e) {
    System.out.println("Null error");
} catch (Exception e) {           // catch all (most general — always last)
    System.out.println("General error");
} finally {
    System.out.println("Always runs");  // cleanup code
}
```

### Checked vs Unchecked Exceptions

| | Checked | Unchecked |
|---|---|---|
| Extends | `Exception` (not Runtime) | `RuntimeException` |
| Must handle? | **Yes** — compile error if not | No — optional |
| Examples | `IOException`, `SQLException` | `NullPointerException`, `ArrayIndexOutOfBounds` |
| Declared with | `throws` in method signature | Not required |

### Custom Exceptions
```java
class InsufficientFundsException extends Exception {
    double amount;
    InsufficientFundsException(double amount) {
        super("Insufficient funds: need " + amount + " more");
        this.amount = amount;
    }
}

class BankAccount {
    double balance;
    void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance)
            throw new InsufficientFundsException(amount - balance);
        balance -= amount;
    }
}
```

### C++ Exception Handling
```cpp
try {
    throw runtime_error("Something went wrong");
} catch (runtime_error& e) {
    cout << e.what();
} catch (...) {
    cout << "Unknown exception";
}
```

---

## 14. C++ vs Java — Key Differences

| Feature | C++ | Java |
|---|---|---|
| **Memory** | Manual (`new`/`delete`) | Automatic GC |
| **Pointers** | Full pointer support | No explicit pointers (references only) |
| **Multiple Inheritance** | YES (with virtual) | Classes: No; Interfaces: Yes |
| **Operator Overloading** | YES | NO |
| **Virtual Functions** | `virtual` keyword needed | All methods virtual by default |
| **Destructor** | YES | NO (`finalize()` deprecated) |
| **Compilation** | Compiled to machine code | Compiled to bytecode (JVM) |
| **Platform** | Platform-dependent | Platform-independent (WORA) |
| **Templates** | YES (Templates) | YES (Generics, type erasure) |
| **Abstract Class** | Pure virtual function | `abstract` keyword |
| **Interface** | No keyword (pure abstract class) | `interface` keyword |
| **`final`** | No class `final` (use `sealed`) | `final` class/method keyword |

### Key Java-Specific Concepts

#### String in Java
```java
String s1 = "hello";             // String pool (reused if same value)
String s2 = "hello";
String s3 = new String("hello"); // New object in heap

s1 == s2       // true (same pool reference)
s1 == s3       // false (different object)
s1.equals(s3)  // true (content comparison)
```
> **MCQ Trap**: Always use `.equals()` for String comparison in Java, not `==`.

#### `this` in Java vs C++

| Language | `this` Type | Can be null? |
|---|---|---|
| Java | Reference | No |
| C++ | Pointer | No (but can be null if misused) |

---

## 15. MCQ Quick-Fire Facts

### Constructor Facts
- Constructors have **no return type**
- Constructors **cannot be virtual** (in C++)
- Constructors **cannot be static**
- Constructors **can be overloaded**
- Default constructor: no args (compiler provides if you write none)
- If you define ANY constructor, compiler **does not** auto-generate default

### Polymorphism Facts
- Overloading -> Compile-time (early binding)
- Overriding -> Runtime (late binding)
- `static` methods -> **Cannot** be overridden (can be hidden)
- `private` methods -> **Cannot** be overridden
- `final` methods -> **Cannot** be overridden

### Inheritance Facts
- Java: `extends` for class, `implements` for interface
- C++: `:public` for public inheritance
- Constructors and destructors are **NOT inherited**
- `private` members are inherited but **not accessible** in derived class
- Multiple inheritance in Java: only via interfaces

### Virtual Function Facts
- Virtual function -> runtime dispatch (via vtable)
- Pure virtual (`= 0`) -> abstract class
- **Always** make base class destructor `virtual` in C++
- Java: all non-static, non-final, non-private methods are virtual by default

### Design Pattern Facts
- **Singleton**: One instance, private constructor, `getInstance()`
- **Factory**: Centralized object creation, hides implementation
- **Strategy**: Interchangeable algorithms, avoids if-else chains
- **Observer**: 1-to-many notification, loose coupling
- **Decorator**: Add behavior without changing class (wrapping)

### SOLID Quick Reference
- **S**: One reason to change
- **O**: Extend, don't modify
- **L**: Subclass substitutable for base
- **I**: Small specific interfaces
- **D**: Depend on abstractions

### Common MCQ Traps

| Question | Answer |
|---|---|
| Can we overload by return type alone? | **NO** |
| Can static methods be overridden? | **NO** (they can be hidden) |
| Can abstract class have constructor? | **YES** (called by subclass) |
| Can interface have constructor? | **NO** |
| Can interface have fields? | Only `public static final` (constants) |
| Can we instantiate abstract class? | **NO** |
| What makes a class abstract in C++? | At least one pure virtual function |
| Can destructor be overloaded in C++? | **NO** |
| Can constructor be virtual? | **NO** (in C++) |
| Java multiple inheritance solution? | **Interfaces** |
| Who calls the constructor? | The `new` operator / runtime |
| Difference between `==` and `.equals()` in Java? | `==` compares references, `.equals()` compares content |

---

## Summary Cheat Sheet

```
OOP PILLARS:
  Encapsulation  -> Bundle data + methods, hide internal state (private fields)
  Abstraction    -> Hide HOW, show WHAT (abstract class / interface)
  Inheritance    -> IS-A relationship, code reuse (extends / :public)
  Polymorphism   -> Many forms: Overloading (compile) + Overriding (runtime)

VIRTUAL FUNCTIONS (C++):
  virtual -> runtime dispatch via vtable
  = 0     -> pure virtual -> abstract class
  override -> tells compiler this is an override (safety check)
  Always virtual destructor in base class!

SOLID (one liner each):
  S -> One reason to change
  O -> Extend without modifying
  L -> Subclass behaves like base class
  I -> Small focused interfaces
  D -> Abstractions over concretions

DESIGN PATTERNS (category & intent):
  Singleton  -> Creational -> One instance
  Factory    -> Creational -> Centralized creation
  Strategy   -> Behavioral -> Swap algorithms at runtime
  Observer   -> Behavioral -> Notify many on state change
  Decorator  -> Structural -> Wrap to add behavior

UML ARROWS:
  --------/\   Inheritance (solid, open triangle)
  - - - - /\   Realization/Interface (dashed, open triangle)
  -------<*>   Composition (filled diamond, strong)
  -------<>    Aggregation (empty diamond, weak)
  -------->    Association
```

---

*Prepared for BUET MSc Admission Exam — OOP Section | Last Updated: October 2026*

# Abstract Class vs Interface in Java

Java provides **abstract classes** and **interfaces** to define **contracts** and **common behavior** for classes.  
Understanding their differences is crucial for **OOP design and API development**.

---
### ✅ Key Takeaways
### Use Abstract Classes When:
- Classes are closely related.
- You want to share code (concrete methods and fields).

### Use Interfaces When:
- You want to define a contract for unrelated classes.
- You want multiple inheritance of behavior.

> Since Java 8+, interfaces can have default methods, reducing the need for abstract classes in some scenarios.

---

## 1. Abstract Class

### Definition
- A class that **cannot be instantiated**.
- Can have **abstract methods** (without implementation) and **concrete methods** (with implementation).
- Can have **fields, constructors, and static methods**.

### Example
```java
abstract class Vehicle {
    String name;

    Vehicle(String name) {
        this.name = name;
    }

    abstract void start(); // abstract method

    void display() {       // concrete method
        System.out.println("Vehicle: " + name);
    }
}

class Car extends Vehicle {
    Car(String name) {
        super(name);
    }

    @Override
    void start() {
        System.out.println(name + " started");
    }
}
```

## 2. Interface

### Definition

- A **pure contract** specifying methods without implementation (before Java 8).
- From Java 8 onwards:
    - Can have **default methods** with implementation.
    - Can have **static methods**.
    - Fields are implicitly **public static final**.

### Example
```java
interface Drivable {
    void start(); // abstract by default

    default void honk() {
        System.out.println("Honking...");
    }
}

class Bike implements Drivable {
    @Override
    public void start() {
        System.out.println("Bike started");
    }
}

```
### 3. Comparison Table

| **Feature**           | **Abstract Class**                              | **Interface**                                      |
|------------------------|------------------------------------------------|---------------------------------------------------|
| **Multiple Inheritance** | ❌ Java supports only single class inheritance | ✅ A class can implement multiple interfaces       |
| **Fields**             | Can have instance and static fields            | Only **public static final** fields (constants)   |
| **Methods**            | Can have abstract and concrete methods         | Abstract methods (**default/static methods allowed**) |
| **Constructor**        | ✅ Can have constructors                        | ❌ Cannot have constructors                        |
| **Access Modifiers**   | Can be private, protected, public              | Methods are implicitly **public** (fields are **public static final**) |
| **When to Use**        | Share code among closely related classes       | Define a contract for unrelated classes           |
| **Flexibility**        | Less flexible in multiple inheritance scenarios | More flexible due to multiple interface implementation |

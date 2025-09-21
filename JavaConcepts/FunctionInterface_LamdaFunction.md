# Functional Interfaces, Lambdas, and Default Methods in Java

Java 8 introduced **functional programming features**, which simplify writing concise, readable, and expressive code.

---
### ✅ Key Takeaways

- **Functional Interfaces + Lambdas** → Concise, readable functional-style code.
- **Default Methods** → Maintain backward compatibility when evolving interfaces.
- Together, they form the core of Java 8 functional programming enhancements.

---

## 1. Functional Interfaces

### Definition
- An interface with **exactly one abstract method**.
- Can have **default methods**, **static methods**, and **Object methods**.
- Used as the **target type for lambda expressions** and method references.

### Examples
```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

### Annotation `@FunctionalInterface`

- **Optional but recommended**: Ensures the interface has exactly one abstract method.
- Compiler enforces the single abstract method rule.

---

### 2. Lambda Expressions

#### Definition
- Anonymous functions (without a name) that can be treated as objects of a functional interface.

#### Syntax
- `(parameters) -> expression`
- `(parameters) -> { statements }`

#### Examples
```
Calculator add = (a, b) -> a + b;
Calculator multiply = (a, b) -> a * b;

System.out.println(add.calculate(5, 3));      // 8
System.out.println(multiply.calculate(5, 3)); // 15
```

### Key Points

- **Reduces boilerplate code.**
- **Can be assigned to functional interface references.**
- **Supports method references:**

```
List<String> names = List.of("Alice", "Bob", "Charlie");
names.forEach(System.out::println); // method reference
```

### 3. Default Methods
#### Definition

- Interfaces can have methods with a **default implementation**.
- This allows adding new methods to existing interfaces without breaking old implementations.
Example
```java
interface Vehicle {
void start();

    default void honk() {
        System.out.println("Honking...");
    }
}

class Car implements Vehicle {
public void start() {
System.out.println("Car started");
}
}

public class Demo {
public static void main(String[] args) {
Vehicle car = new Car();
car.start(); // Car started
car.honk();  // Honking...
}
}
```
### Key Points

- **Enables interface evolution** in Java 8+.
- Can be **overridden** in implementing classes.
- **Multiple inheritance** of default methods may require explicit overriding.

---

### ⚖️ Comparison Table

| **Feature**           | **Description**                                      |
|------------------------|------------------------------------------------------|
| **Functional Interface** | Single abstract method, target type for lambda       |
| **Lambda Expression**  | Anonymous function, implements functional interface  |
| **Default Method**     | Interface method with implementation, helps evolve APIs without breaking existing code |

---

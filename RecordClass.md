# java.lang.Record

## What is java.lang.Record?

In **Java 14 (preview)** and **Java 16 (final)**, Records were introduced as a special kind of class.

A **Record** is a transparent data carrier: a class designed to hold immutable data with very little boilerplate.

All record classes implicitly extend `java.lang.Record` (the base class in the JDK).

### ✅ Summary:
- `java.lang.Record` = base class for all records.
- Records are **immutable**, **final**, and **concise** classes for holding data.
- They reduce boilerplate and improve readability.
- Best used for **DTOs**, **config objects**, and **value objects** in Java.

👉 **Key Features**:
- The record is **final** (can’t be subclassed).
- Fields are **final** and **private**.
- You automatically get:
    - **Constructor**
    - **equals()**
    - **hashCode()**
    - **toString()**

---

## Syntax Example

### Traditional Class (Before Records)
```java
public final class Person {
    private final String name;
    private final int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String name() { return name; }
    public int age() { return age; }

    @Override
    public String toString() {
        return "Person[name=" + name + ", age=" + age + "]";
    }

    @Override
    public boolean equals(Object o) {
        // boilerplate code
    }

    @Override
    public int hashCode() {
        // boilerplate code
    }
}
```
### Using a Record
```java
public record Person(String name, int age) {}
```

That’s it!  
Java automatically generates:

- **Constructor**: `new Person("Malkin", 35)`
- **Accessors**: `person.name()`, `person.age()`
- **equals()**, **hashCode()**, **toString()`

---

## When and Why Use Records

### ✅ Use Records when:
- You want a **data carrier class** (like DTOs, configuration objects, API payloads).
- The class is **immutable** (fields won’t change after creation).
- You want **less boilerplate** (no need to manually write getters, equals, hashCode, toString).

### ❌ Don’t use Records when:
- You need **mutable fields**.
- You want to **inherit** from another class (records can only extend `java.lang.Record`, though they can implement interfaces).
- You need **complex behavior** — records are meant to model data, not behavior-heavy objects.

---

## Example Use Case

### DTO (Data Transfer Object)

#### Instead of:
```java
public class EmployeeDTO {
    private final int id;
    private final String name;
    private final double salary;
    // getters, constructor, equals, hashCode, toString...
}
```

### With Records:
```java
public record EmployeeDTO(int id, String name, double salary) {}
```

### Customization Example

Records can include:

- **Custom constructors**
- **Validation logic**
- **Additional methods**

```java
public record Product(String name, double price) {
    public Product {
        if (price < 0) {
            throw new IllegalArgumentException("Price cannot be negative");
        }
    }

    public String label() {
        return name + " - $" + price;
    }
}
```


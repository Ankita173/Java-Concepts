# Reflection in Java

## 📌 What is Reflection?

Reflection in Java is a feature of the **`java.lang.reflect`** API that allows a program to:
- **Inspect classes, methods, fields, and constructors** at runtime.
- **Access and modify private members** dynamically.
- **Instantiate objects**, **invoke methods**, and **change field values** without knowing the class at compile time.

Reflection leverages the **JVM's metadata** about loaded classes.

---

## ✅ Summary

Reflection in Java is a powerful but dangerous tool:
- Great for building frameworks and dynamic libraries.
- Comes with performance costs and security risks.
- Should be used sparingly and only when there’s no simpler alternative.

---

## 📝 Example: Using Reflection
```java
import java.lang.reflect.*;

class Person {
    private String name = "Malkin";

    private void greet() {
        System.out.println("Hello, " + name);
    }
}

public class ReflectionDemo {
    public static void main(String[] args) throws Exception {
        Class<?> cls = Class.forName("Person");

        // Create instance dynamically
        Object obj = cls.getDeclaredConstructor().newInstance();

        // Access private field
        Field field = cls.getDeclaredField("name");
        field.setAccessible(true);
        field.set(obj, "Saransh");

        // Invoke private method
        Method method = cls.getDeclaredMethod("greet");
        method.setAccessible(true);
        method.invoke(obj); // prints: Hello, Saransh
    }
}
```

## ⚖️ Trade-offs of Reflection

### ✅ Advantages

1. **Dynamic Behavior**
    - Useful for frameworks (e.g., Spring, Hibernate, JUnit) where classes/methods are discovered at runtime.

2. **Generic Code**
    - Enables building tools, libraries, and ORMs without compile-time knowledge of domain classes.

3. **Flexibility**
    - Supports dependency injection, serialization, object mapping, and testing frameworks.

---

### ❌ Disadvantages

1. **Performance Overhead**
    - Reflective calls are slower than direct calls due to security checks and dynamic resolution.

2. **Compile-time Safety Lost**
    - No type-checking → errors are found at runtime (e.g., wrong method name).

3. **Complexity & Maintainability**
    - Code using reflection is harder to read and debug.

4. **Limited Tool Support**
    - IDEs and static analyzers may not detect usage errors in reflection-based code.

---

### 🔐 Security Issues with Reflection

1. **Accessing Private Members**
    - `setAccessible(true)` breaks encapsulation.
    - Can modify sensitive fields (e.g., bypass immutability of `String`).

2. **Potential for Exploits**
    - Malicious code can manipulate application internals if reflection is not restricted.

3. **SecurityManager Restrictions**
    - Older Java versions allowed restricting reflection via a `SecurityManager`.
    - In newer Java (17+), strong encapsulation with the module system (JPMS) helps prevent illegal reflective access.

---

### 📌 Best Practices

1. Avoid reflection in performance-critical code.
2. Use reflection only in frameworks, libraries, or utilities where flexibility is required.
3. Restrict reflective access using:
    - Java Module System (`module-info.java`).
4. Avoid `setAccessible(true)` unless absolutely necessary.
   5. Prefer alternatives (e.g., generics, lambdas, interfaces) whenever possible.


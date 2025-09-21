# `equals()` vs `==` and `hashCode()` Contract in Java

### ✅ Summary

- Use `==` to check if two references point to the **same object**.
- Use `equals()` to check if two objects are **logically equal**.
- Always override `hashCode()` when overriding `equals()` to maintain collection consistency.
- Violating the `hashCode()` contract leads to unexpected behavior in `HashMap` / `HashSet`.

## 📌 1. `==` Operator

- **Purpose:** Compares **references** (memory addresses) for objects.
- For **primitive types**, it compares actual **values**.
- For objects, `==` is `true` only if both references point to the **same object**.
```
### Example
String s1 = new String("Hello");
String s2 = new String("Hello");

System.out.println(s1 == s2); // false (different objects)
System.out.println(s1.equals(s2)); // true (contents are equal)
```

### 2. `equals()` Method

- **Purpose:** Compares logical equality of objects (contents).
- **Defined in:** `java.lang.Object` (default implementation uses `==`).
- **Common Use:** Overridden in classes to compare fields.
- 
### 📝 Example
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Person)) return false;
        Person other = (Person) obj;
        return this.age == other.age && this.name.equals(other.name);
    }

    @Override
    public int hashCode() {
        return name.hashCode() + age;
    }
}

Person p1 = new Person("Malkin", 35);
Person p2 = new Person("Malkin", 35);

System.out.println(p1 == p2);       // false (different references)
System.out.println(p1.equals(p2));  // true (logical equality)
```
---

### 📌 3. `hashCode()` Contract

- **Usage:** Used in hash-based collections (e.g., `HashMap`, `HashSet`).
- **Contract:**
  1. If two objects are equal (`equals()` returns `true`), they must have the same `hashCode()`.
  2. If two objects are unequal, their `hashCode()` may or may not be different (collisions are allowed).
  3. Repeated calls to `hashCode()` must consistently return the same value during the object's lifetime.

### 📝 Example
Person p1 = new Person("Malkin", 35);
Person p2 = new Person("Malkin", 35);

System.out.println(p1.equals(p2));      // true
System.out.println(p1.hashCode() == p2.hashCode()); // true
```
---

### ⚖️ Comparison Table: `==` vs `equals()`

| **Feature**           | **`==`**                          | **`equals()`**                     |
|------------------------|------------------------------------|-------------------------------------|
| **Type of Comparison** | Reference (memory address)        | Logical equality (content)         |
| **Can Be Overridden**  | ❌ Cannot override                | ✅ Usually overridden in classes   |
| **Default Behavior**   | `true` if same object             | `true` if same object (`Object` class) |
| **Use Case**           | Check identity                   | Check equivalence of content       |

---

# String Pool & Immutability in Java

### ✅ Key Takeaways

- **String Pool**: Reuses string literals to save memory.
- **Immutability**: Strings cannot be modified; operations create new objects.
- Strings are **safe for sharing**, **good for caching**, and **suitable as keys** in collections.
- Use `intern()` to explicitly add dynamically created strings to the pool if needed.

## 📌 1. String Pool

### What is the String Pool?
- A **special memory area in the JVM heap** that stores **unique string literals**.
- Helps **save memory** by reusing strings with the same content.

### How it Works
```
String s1 = "Hello";  // stored in String Pool
String s2 = "Hello";  // reuses same object from pool
String s3 = new String("Hello"); // new object in heap, NOT in pool

System.out.println(s1 == s2); // true (same reference)
System.out.println(s1 == s3); // false (different objects)

//intern() method: Adds string to pool if not already present.

String s4 = new String("World").intern(); // goes to String Pool
```

### Key Points

- **String literals** are automatically pooled.
- Using `new String()` creates a new object on the heap.
- Pooling improves **memory efficiency** and **performance**.

---

### 📌 2. String Immutability

#### What is Immutability?

- Strings in Java are **immutable**, meaning their content cannot be changed after creation.
- Any operation that seems to modify a string actually creates a **new string**.

#### 📝 Examples

```
String s1 = "Hello";
String s2 = s1.concat(" World"); // creates a new string
System.out.println(s1); // Hello (unchanged)
System.out.println(s2); // Hello World (new object)
```

### Why Strings are Immutable

#### 1. Security
- Strings are used in **class loading**, **database connections**, and **network URLs**.
- Immutability prevents **malicious modifications**.

#### 2. Thread Safety
- Immutable objects can be **shared concurrently** without requiring synchronization.

#### 3. Performance
- The **String Pool** safely reuses objects, improving memory efficiency.

#### 4. Hashing
- Immutable strings are ideal as **keys in HashMap/HashSet**, ensuring consistent behavior.

---

### ⚖️ String Pool + Immutability Relationship

- **Safety**: No thread can modify a pooled string.
- **Optimization**: Memory and performance are improved.
- **New Objects**: Any modification creates a **new string**, leaving the original intact.

---



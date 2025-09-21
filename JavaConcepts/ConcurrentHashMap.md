# HashMap vs Hashtable vs ConcurrentHashMap in Java

Java provides several **Map implementations** for storing key-value pairs.  
Understanding their differences is important for **performance, thread-safety, and API design**.

---

### ✅ Key Takeaways

- Use **HashMap** when thread-safety is not required (default choice).
- Avoid **Hashtable**; it is legacy, replaced by **ConcurrentHashMap**.
- Use **ConcurrentHashMap** for multi-threaded applications where high concurrency is required.
- Always prefer modern concurrent utilities (**ConcurrentHashMap**) over full synchronization (**Hashtable**) for scalability.

---

## 1. HashMap

### Definition
- Part of **`java.util`** package.
- **Non-synchronized** → **not thread-safe**.
- Allows **one null key** and multiple null values.
- Iteration is **fail-fast**: detects structural modifications during iteration.

### Example
```
Map<String, Integer> hashMap = new HashMap<>();
hashMap.put("A", 1);
hashMap.put(null, 100);
System.out.println(hashMap.get("A")); // 1
System.out.println(hashMap.get(null)); // 100
```

## 2. Hashtable

### Definition

- Part of **`java.util`** package.
- **Synchronized** → thread-safe for individual operations.
- Does **not allow null keys or null values**.
- Legacy class, rarely used in modern code.

### Example

```
Map<String, Integer> hashtable = new Hashtable<>();
hashtable.put("A", 1);
// hashtable.put(null, 100); // Throws NullPointerException
System.out.println(hashtable.get("A")); // 1
```

## 3. ConcurrentHashMap

### Definition

- Part of **`java.util.concurrent`** package.
- **Thread-safe**, supports high concurrency.
- Uses **segment-level locking** or **CAS operations** instead of locking the entire map.
- **Does not allow null keys or null values**.

### Example

```
ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("A", 1);
System.out.println(concurrentMap.get("A")); // 1
```

## 4. Comparison Table

| **Feature**           | **HashMap**                     | **Hashtable**                 | **ConcurrentHashMap**         |
|------------------------|---------------------------------|-------------------------------|--------------------------------|
| **Thread Safety**      | ❌ Not thread-safe             | ✅ Synchronized               | ✅ Thread-safe (concurrent)   |
| **Null Keys / Values** | 1 null key, multiple null values | ❌ No null keys/values        | ❌ No null keys/values        |
| **Synchronization Level** | None                        | Entire map synchronized       | Segment-level / CAS for concurrency |
| **Performance**        | Fast (non-threaded)           | Slower due to full synchronization | Faster than Hashtable in multi-threaded environment |
| **Iterators**          | Fail-fast                     | Fail-fast                     | Weakly consistent (no `ConcurrentModificationException`) |
| **Modern Usage**       | ✅ Preferred for single-threaded | ❌ Legacy class               | ✅ Preferred for multi-threaded |

---


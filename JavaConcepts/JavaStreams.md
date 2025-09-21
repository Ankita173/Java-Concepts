# Stream API, Lazy Evaluation, and Parallel Streams in Java

Java 8 introduced the **Stream API** for functional-style operations on collections and other data sources. Streams enable **declarative processing** with **better readability, parallelism, and efficiency**.

---

### ⚖️ Summary Table

| **Feature**       | **Description**                                                                 |
|--------------------|---------------------------------------------------------------------------------|
| **Stream API**     | Functional, declarative operations on collections or data sources              |
| **Lazy Evaluation**| Intermediate operations executed only on terminal operation                    |
| **Parallel Stream**| Automatically splits work across threads using ForkJoinPool for multi-core execution |

---

## 1. Stream API

### Definition

- A **sequence of elements** supporting **aggregate operations**.
- Allows **filtering, mapping, and reducing** elements without modifying the underlying data structure.
- Key classes: `java.util.stream.Stream`, `IntStream`, `LongStream`, `DoubleStream`.

### Example

```
List<String> names = List.of("Alice", "Bob", "Charlie", "David");

// Filter names starting with 'A', convert to uppercase, collect
List<String> result = names.stream()
        .filter(s -> s.startsWith("A"))
        .map(String::toUpperCase)
        .toList();

System.out.println(result); // [ALICE]
```

### Key Points

- **Declarative approach** instead of loops.
- Can be **finite or infinite streams**.
- Supports **intermediate (lazy)** and **terminal (eager)** operations.

---

## 2. Lazy Evaluation

### What is Lazy Evaluation?

- Intermediate operations (`filter`, `map`, `sorted`) are **not executed immediately**.
- Only executed when a **terminal operation** (`collect`, `forEach`, `reduce`) is called.

### Example

```java
Stream<String> stream = names.stream()
                             .filter(s -> {
                                 System.out.println("Filtering " + s);
                                 return s.length() > 3;
                             })
                             .map(String::toUpperCase);

// Terminal operation triggers evaluation
List<String> result = stream.toList();
```

***Output:***
```
Filtering Alice
Filtering Bob
Filtering Charlie
Filtering David
```

Filtering and mapping happen only when the terminal operation executes.

### Benefits

- **Performance optimization**: Avoids unnecessary computation.
- **Short-circuiting operations**: Operations like `findFirst()` or `anyMatch()` further improve efficiency.

---

## 3. Parallel Streams

### Definition

- Streams can be **parallelized** to utilize multiple CPU cores.
- Uses `ForkJoinPool.commonPool()` internally.

### Example

```
List<Integer> numbers = IntStream.range(1, 10).boxed().toList();

// Sequential stream
int sumSeq = numbers.stream().reduce(0, Integer::sum);
System.out.println(sumSeq);

// Parallel stream
int sumPar = numbers.parallelStream().reduce(0, Integer::sum);
System.out.println(sumPar);
```

### Key Points

- Use **parallel streams** for **CPU-intensive tasks** on large datasets.
- Avoid parallel streams when:
    - **Shared mutable state** exists.
    - **Small datasets** (overhead may outweigh benefit).
    - Operations are **I/O bound**.

---

### ✅ Best Practices

1. Prefer streams for **readable, concise code** over loops when processing collections.
2. Use **lazy evaluation** to improve performance.
3. Use **parallel streams** for compute-intensive tasks with large datasets.
4. Avoid **shared mutable state** with parallel streams to prevent race conditions.
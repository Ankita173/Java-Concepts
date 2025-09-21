### 1. Java Memory Model (JMM)

The Java Memory Model (JMM) defines:

- How threads interact through memory.
- When writes by one thread are visible to another thread.
- How the JVM, CPU, and compiler are allowed to reorder instructions.

Without the JMM, multithreaded programs would behave unpredictably because modern CPUs and JVMs reorder instructions for performance.

#### ✅ Summary for Interviews:
- **JMM** defines visibility, atomicity, and ordering across threads.
- `volatile` ensures **visibility** and **ordering**, but not **atomicity**.
- `synchronized` ensures **visibility**, **atomicity**, and **ordering**.
- Memory barriers are the low-level mechanics that make these work.

#### Key Concepts:

- **Main Memory vs Thread Local Caches**:
    - Each thread has a working memory (CPU cache + registers).
    - Shared variables live in main memory (heap).
    - Threads may not immediately see updates made by others unless properly synchronized.

- **Happens-Before Relationship**:
    - Defines visibility guarantees.
    - Example: If A happens-before B, then changes made in A are visible to B.
    - Built-in happens-before rules include:
        - Unlock → Lock.
        - Thread start → Code inside `run()`.
        - Write to `volatile` → Subsequent read of `volatile`.

---

### 2. `volatile` Keyword

- Declares a variable’s value is always read from main memory (not cached in registers).
- Guarantees **visibility**: writes by one thread are visible to others.
- Does **NOT** guarantee **atomicity** (except for 64-bit `long`/`double`).

#### Example:
```java
class SharedData {
    volatile boolean running = true;

    void runLoop() {
        while (running) {
            // busy work
        }
        System.out.println("Stopped!");
    }
}
```

Without `volatile`, another thread setting `running = false;` might never be seen due to caching. With `volatile`, the update is immediately visible.

### 3. `synchronized`

- Ensures **mutual exclusion**: only one thread executes the critical section at a time.
- Establishes **happens-before relationships**:
    - A **release** (unlock) flushes changes to main memory.
    - An **acquire** (lock) invalidates caches, ensuring visibility of fresh values.

#### Example:
```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++; // atomic + visible to all threads
    }

    public synchronized int getCount() {
        return count;
    }
}
```

Here:

- Only one thread increments at a time.
- Updates to `count` are visible to all threads after the method exits.

---

### 4. Memory Barriers (Under the Hood)

Modern CPUs and JVMs reorder instructions for speed. Memory barriers (fences) prevent unsafe reorderings. The JMM uses them to implement `volatile` and `synchronized`.

#### Types of Memory Barriers:
- **Load Barrier**: Forces re-read from memory (prevents stale reads).
- **Store Barrier**: Flushes writes to memory before continuing.

#### Mapping:
- `volatile` write → Insert **Store Barrier** (flush changes before continuing).
- `volatile` read → Insert **Load Barrier** (fetch latest value).
- `synchronized` → Implicitly uses both **Load** and **Store Barriers** when entering/exiting a monitor.

---

### 5. Comparison: `volatile` vs `synchronized`

| **Feature**         | **volatile**                  | **synchronized**               |
|----------------------|-------------------------------|---------------------------------|
| **Visibility**       | ✅ Yes                        | ✅ Yes                          |
| **Atomicity**        | ❌ No (except single reads/writes) | ✅ Yes                          |
| **Mutual Exclusion** | ❌ No                         | ✅ Yes                          |
| **Performance**      | Lightweight                  | Heavier (blocking, context switches) |
| **Use Case**         | Flags, status variables      | Counters, shared mutable state |

---

### 6. Example: `volatile` vs `synchronized`

```java
class Example {
    volatile boolean flag = false;
    int count = 0;

    void writer() {
        count = 42;
        flag = true; // volatile write -> guarantees visibility
    }

    void reader() {
        if (flag) {          // volatile read -> sees latest flag
            System.out.println(count); // guaranteed to print 42
        }
    }
}
```
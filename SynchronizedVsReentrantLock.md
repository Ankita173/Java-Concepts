# Thread Safety in Java

## 📌 What is Thread Safety?
A class or method is **thread-safe** if it behaves correctly when accessed concurrently by multiple threads, without unexpected results or data corruption.

### Key techniques for thread safety:
- **Synchronization** (`synchronized` keyword)
- **Locks** (`ReentrantLock`, `ReadWriteLock`, etc.)
- **Atomic variables** (`AtomicInteger`, `AtomicReference`)
- **Immutable objects** (e.g., `String`)
- **Concurrent collections** (`ConcurrentHashMap`, `CopyOnWriteArrayList`)

---

# 🔒 `synchronized` vs `ReentrantLock`

---

### ✅ When to Use What?

- **Use `synchronized`:**
    - Simpler scenarios.
    - When you just need basic mutual exclusion.
    - For readability and less error-prone code.

- **Use `ReentrantLock`:**
    - When you need advanced features:
        - Fairness policy.
        - Try-lock with timeout.
        - Multiple condition variables.
        - Interruptible locking.
    - When dealing with highly concurrent systems.

---

## 1. `synchronized`
- Built-in mechanism for **mutual exclusion**.
- Applied on:
    - Methods (`synchronized void method()`)
    - Code blocks (`synchronized(this) { ... }`).
- Ensures only one thread executes the synchronized block at a time.
- Automatically releases lock when block exits (even if exception occurs).

### Example
```java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

### 2. ReentrantLock

**Part of** `java.util.concurrent.locks`.

**Offers more control than** `synchronized`.

---

### 🔑 Key Features:

1. **Explicit Lock/Unlock**
    - Must call `lock()` and `unlock()` manually.

2. **Fairness Policy**
    - Can construct with fairness: `new ReentrantLock(true)` to give preference to the longest-waiting thread.

3. **Try Locking**
    - `tryLock()` allows non-blocking attempts.

4. **Interruptible Lock Acquisition**
    - `lockInterruptibly()` allows breaking out of waiting if the thread is interrupted.

5. **Condition Variables**
    - Supports multiple `Condition` objects (like advanced `wait/notify`).

---

### 📝 Example:

```java
import java.util.concurrent.locks.ReentrantLock;

class Counter {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public int getCount() {
        return count;
    }
}
```
⚖️ Comparison Table

| **Feature**                     | **`synchronized`**                     | **`ReentrantLock`**                     |
|----------------------------------|----------------------------------------|-----------------------------------------|
| **Ease of Use**                  | Simple, implicit locking               | Verbose, explicit `lock()`/`unlock()` calls |
| **Automatic Lock Release**       | Yes (on method/block exit)             | No (must use `try-finally`)             |
| **Fairness**                     | No fairness guarantee                  | Optional fairness (`new ReentrantLock(true)`) |
| **Try Lock (non-blocking attempt)** | ❌ Not available                      | ✅ `tryLock()` supported                 |
| **Interruptible Lock Acquisition** | ❌ Not supported                      | ✅ `lockInterruptibly()` supported       |
| **Condition Variables**          | Single (`wait`/`notify`/`notifyAll`)   | Multiple (`newCondition()`)             |
| **Performance**                  | Good for most cases                    | Slightly better under high contention   |
| **Reentrancy**                   | ✅ Allowed                             | ✅ Allowed                               |

---

### 🔐 Best Practice

- Prefer `synchronized` for simplicity unless you need the advanced control of `ReentrantLock`.
- Always use `try-finally` when using `ReentrantLock` to prevent lock leaks.
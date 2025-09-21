# `wait()` vs `sleep()` in Java

## 📌 Overview
Both `wait()` and `sleep()` cause a thread to **pause execution temporarily**, but they differ in **where they come from**, **how they release locks**, and **their purpose**.

---

### ✅ When to Use

- **Use `sleep()`:**
    - When you just want to pause execution for a fixed time.
    - Examples: retry logic, polling, scheduled delays.

- **Use `wait()`:**
    - For thread communication and coordination.
    - Examples: producer-consumer problem, inter-thread signaling.
---

## 1. `sleep()`
- **Defined in:** `java.lang.Thread`
- **Static method** → called on the current thread.
- **Does NOT release any lock/monitor** the thread holds.
- Used for **pausing execution** for a fixed time.

### Example
```java
class SleepExample {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Start");

        Thread.sleep(2000); // Pauses for 2 seconds

        System.out.println("End");
    }
}
```
## 2. `wait()`

- **Defined in:** `java.lang.Object`
- **Synchronization Requirement:** Must be called from a synchronized block/method.
- **Lock Release:** Releases the lock on the object, allowing other threads to acquire it.
- **Waits Until:**
    1. Another thread calls `notify()` / `notifyAll()` on the same object.
    2. The thread is interrupted.
    3. Timeout expires (if `wait(timeout)` is used).

### Example
```java
class WaitExample {
    private static final Object lock = new Object();

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> {
            synchronized (lock) {
                try {
                    System.out.println("Thread 1 waiting...");
                    lock.wait();  // Releases lock and waits
                    System.out.println("Thread 1 resumed!");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock) {
                System.out.println("Thread 2 notifying...");
                lock.notify(); // Wakes up one waiting thread
            }
        });

        t1.start();
        Thread.sleep(1000); // Ensure t1 starts first
        t2.start();
    }
}

```

## ⚖️ Comparison Table: `sleep()` vs `wait()`

| **Feature**                  | **`sleep()`**                          | **`wait()`**                          |
|------------------------------|----------------------------------------|---------------------------------------|
| **Defined In**               | `Thread` class                        | `Object` class                       |
| **Lock Release**             | ❌ Does not release lock               | ✅ Releases lock while waiting       |
| **Synchronization Requirement** | ❌ Can be called anywhere             | ✅ Must be inside synchronized block/method |
| **Purpose**                  | Pause execution for a given time      | Thread communication (coordination)  |
| **Resume Condition**         | After timeout                         | After `notify()` / `notifyAll()` / timeout |
| **Static / Instance**        | `static` (affects current thread)     | Instance method (called on monitor object) |

---
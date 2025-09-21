# Virtual Threads (Project Loom)

## What are Virtual Threads?

Virtual Threads are lightweight threads introduced in **Java 19 (preview)** under **Project Loom**.

### ✅ Summary:
- **Virtual threads**: Lightweight, millions possible, great for I/O-bound tasks.
- **OS threads**: Heavyweight, good for CPU-bound tasks.
- Java 19+ makes using them trivial with the same old Thread API.

---

### Key Characteristics:
- **Managed by the JVM**: Virtual Threads are managed by the JVM, not directly by the operating system.
- **OS Thread Usage**: Each Virtual Thread uses an OS thread when actively running. However, when blocked (e.g., waiting on I/O, DB, or network), the JVM parks the Virtual Thread and frees the OS thread, enabling much higher concurrency.
- **Lightweight**: Virtual Threads are cheap to create. Millions of Virtual Threads can be created, compared to thousands of OS threads.
- 👉 Think of them as user-mode threads (like Go goroutines or Kotlin coroutines), but integrated directly into Java’s concurrency model.

---

## How do they differ from OS threads?

| **Feature**         | **Platform/OS Thread**          | **Virtual Thread (Project Loom)** |
|----------------------|----------------------------------|------------------------------------|
| **Creation cost**    | Expensive (stack allocation in MBs) | Very cheap (small footprint)      |
| **Max count**        | Thousands (limited by memory)   | Millions (lightweight)            |
| **Blocking I/O**     | Blocks the OS thread            | JVM parks the virtual thread (OS thread reused) |
| **Scheduling**       | Done by OS kernel              | Done mostly by JVM (cooperates with OS) |
| **API**              | Thread, Executors              | Same APIs (Thread, ExecutorService) – but under the hood lightweight |

---

## Code Examples

### A. Creating an OS (Platform) Thread
```java
public class OSThreadExample {
    public static void main(String[] args) throws InterruptedException {
        Thread platformThread = new Thread(() -> {
            System.out.println("Running in a platform (OS) thread: " + Thread.currentThread());
        });

        platformThread.start();
        platformThread.join();
    }
}
```
Output (example):

Running in a platform (OS) thread: Thread[#21,Thread-0,5,main]


Here the Thread is backed directly by an OS thread.

### B. Creating a Virtual Thread (Java 19+)
```java
public class VirtualThreadExample {
    public static void main(String[] args) throws InterruptedException {
        Thread virtualThread = Thread.ofVirtual().start(() -> {
            System.out.println("Running in a virtual thread: " + Thread.currentThread());
        });

        virtualThread.join();
    }
}
```

Output (example):

Running in a virtual thread: VirtualThread[#22]/runnable@ForkJoinPool-1-worker-1


Notice the difference: it says VirtualThread, and it runs on a ForkJoinPool worker underneath.

### C. Running Many Virtual Threads
```java
import java.util.stream.IntStream;

public class ManyVirtualThreads {
    public static void main(String[] args) throws InterruptedException {
        IntStream.range(0, 1_000_000).forEach(i -> {
            Thread.startVirtualThread(() -> {
                // Simulate some blocking operation
                try {
                    Thread.sleep(1000); // doesn't block OS thread
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            });
        });

        System.out.println("Started 1 million virtual threads!");
    }
}
```

💡 This is impossible with OS threads due to memory limits but runs fine with Virtual Threads.
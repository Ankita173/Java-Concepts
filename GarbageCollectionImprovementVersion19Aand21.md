# Garbage Collector Improvements in Modern Java (Java 17 & 21)

Java’s Garbage Collection (GC) has evolved significantly in recent releases, focusing on **low latency**, **high throughput**, and **scalability**.  
Below are the major improvements introduced in **Java 17 (LTS)** and **Java 21 (LTS)**.
---

## 📝 Summary Table

| Java Version | GC Improvements |
|--------------|-----------------|
| **Java 17**  | G1 pause reduction, ZGC production-ready (low-latency, TB-scale heaps), Shenandoah production-ready, better string deduplication |
| **Java 21**  | Generational ZGC (JEP 439), G1 region pinning, Shenandoah load balancing, enhanced GC logging |

---

## 🚀 Java 17 (LTS) – Released Sept 2021

### 1. G1 Improvements
- **Reduced GC Pause Times**: Better region-based memory management.
- **More Efficient Parallel Marking**: Optimized SATB (Snapshot-at-the-Beginning) algorithm.
- **Improved String Deduplication**: Lower memory footprint by reusing identical strings.
- **More predictable latency** for large heaps.

### 2. ZGC (Production Ready)
- Z Garbage Collector (**ZGC**) graduated from experimental to production in Java 15 and was **heavily improved in Java 17**.
- **Key Features**:
    - Handles **multi-terabyte heaps** with low-latency.
    - **Pause times < 1 ms**, regardless of heap size.
    - **Concurrent Class Unloading**.
    - **Improved NUMA-awareness** (better performance on multi-socket systems).

### 3. Shenandoah (Production Ready)
- Another low-latency GC option, first experimental in earlier releases, **production-ready in Java 17**.
- **Pause times are independent of heap size**.
- Supports **concurrent heap compaction** (reduces fragmentation).

---

## 🚀 Java 21 (LTS) – Released Sept 2023

### 1. Generational ZGC (JEP 439)
- Extends ZGC with **generational collection** (young + old generations).
- Benefits:
    - **Reduced allocation stalls**.
    - **Improved throughput** for typical applications (where most objects die young).
    - Still maintains **sub-millisecond pause times**.

### 2. G1 Improvements
- **Region Pinning**: Better support for JNI critical regions and humongous objects.
- **Improved remembered set handling** → reduces memory overhead.

### 3. Shenandoah Improvements
- **Better load balancing** across GC threads.
- **Reduced pause time variability**.

### 4. Unified Logging Enhancements
- More **detailed GC logs** and **diagnostics**.
- Easier tuning and analysis with `-Xlog:gc*`.

---

## ✅ When to Use Which GC?

- **G1 (Default in Java 9+)**
    - General-purpose, balanced throughput & latency.
- **ZGC (Java 17+)**
    - Large heaps, low latency (<1 ms pause).
- **Generational ZGC (Java 21+)**
    - Modern workloads, better throughput with same low latency.
- **Shenandoah**
    - Low-latency requirements, especially on Linux-based systems.

---

## 📌 Key Takeaway
- **Java 17** made **ZGC and Shenandoah production-ready**.
- **Java 21** introduced **Generational ZGC**, making it one of the most advanced collectors for modern cloud-scale apps.
- Together, these changes make Java highly competitive for **low-latency services, high-throughput systems, and massive in-memory applications**.

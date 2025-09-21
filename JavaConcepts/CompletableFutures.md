# CompletableFuture: allOf vs anyOf vs thenCombine

Java’s `CompletableFuture` API provides powerful ways to compose and combine asynchronous tasks.  
This document explains the differences between **`allOf()`**, **`anyOf()`**, and **`thenCombine()`** with code examples.
---

### 📊 Summary Table

| **Method**               | **Behavior**                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| `allOf(f1, f2, …)`       | Completes when **all futures finish**. Returns `CompletableFuture<Void>`. Results must be retrieved separately. |
| `anyOf(f1, f2, …)`       | Completes when **any one finishes**. Returns `CompletableFuture<Object>` with the first completed result.         |
| `thenCombine(f1, f2, fn)` | Waits for **both f1 and f2**, then applies function `fn`. Returns a combined result.                              |

---

## 1. `CompletableFuture.allOf()`

### ✅ Purpose
- Waits for **all given futures** to complete.
- Returns a `CompletableFuture<Void>` that completes once *all* tasks finish.
- You must collect results manually.

### 📝 Example
```java
CompletableFuture<String> f1 = CompletableFuture.supplyAsync(() -> "Task 1");
CompletableFuture<String> f2 = CompletableFuture.supplyAsync(() -> "Task 2");
CompletableFuture<String> f3 = CompletableFuture.supplyAsync(() -> "Task 3");

CompletableFuture<Void> all = CompletableFuture.allOf(f1, f2, f3);

// Wait for all tasks to finish
all.join();

System.out.println(f1.join() + ", " + f2.join() + ", " + f3.join());
// Output: Task 1, Task 2, Task 3
```
🔑 Notes

Good when you need all tasks to finish before proceeding.

Return type is `Void`, so you need to query results from each future individually.

---

### 2. `CompletableFuture.anyOf()`

#### ✅ Purpose

- Completes when **any one** of the given futures completes.
- Returns the result of the **first completed future**.

#### 📝 Example
```
CompletableFuture<String> f1 = CompletableFuture.supplyAsync(() -> "Task 1");
CompletableFuture<String> f2 = CompletableFuture.supplyAsync(() -> "Task 2");

CompletableFuture<Object> any = CompletableFuture.anyOf(f1, f2);

// Result of whichever finishes first
System.out.println(any.join());
``` 


**Output** (varies depending on which finishes first):

```Task 1```
or
```Task 2```

🔑 Notes

- Useful for **fastest response wins** scenarios (e.g., querying multiple services).
- Return type is `CompletableFuture<Object>` (may require casting).

---

### 3. `CompletableFuture.thenCombine()`

#### ✅ Purpose

- Combines results of **two independent futures** once both complete.
- Applies a function to their results and returns a new future with the combined value.

#### 📝 Example
```
CompletableFuture<Integer> f1 = CompletableFuture.supplyAsync(() -> 10);
CompletableFuture<Integer> f2 = CompletableFuture.supplyAsync(() -> 20);

CompletableFuture<Integer> result = f1.thenCombine(f2, (a, b) -> a + b);

System.out.println(result.join());
```

**Output**
```30```

🔑 Notes

- Works **pairwise** (two futures at a time).
- Unlike `allOf()`, it directly returns the combined result instead of `Void`.

---

### 📌 When to Use

- **`allOf()`** → Batch processing (e.g., fetch data from multiple services and combine results after all finish).
- **`anyOf()`** → Race condition (e.g., query multiple mirrors/CDNs, use whichever responds first).
- **`thenCombine()`** → Aggregate results of two independent computations (e.g., price + tax, first name + last name).
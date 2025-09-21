# Checked vs Unchecked Exceptions in Java & API Design

## 📌 Overview
Java exceptions are divided into **Checked** and **Unchecked** exceptions, and choosing the right type is critical for **robust API design**.

---
### ✅ Key Takeaways

- **Checked** → Forces handling; use for recoverable errors.
- **Unchecked** → Flexible; use for programming mistakes or illegal API usage.

---

## 1. Checked Exceptions

### ✅ Definition
- Subclasses of `Exception` **excluding `RuntimeException`**.
- The compiler **forces the caller** to handle or declare them using `try-catch` or `throws`.

### 📝 Example
```java
import java.io.*;

public class CheckedExample {
    public void readFile(String path) throws IOException {
        FileReader fr = new FileReader(path); // may throw IOException
        fr.close();
    }
}
```

### 🔑 Characteristics of Checked Exceptions

- **Compile-time checking**: Ensures errors are handled.
- **Recoverable conditions**: Typically used for errors like file not found or network issues.
- **Mandatory handling**: Caller cannot ignore without a `throws` declaration.

---

### 2. Unchecked Exceptions

#### ✅ Definition
- Subclasses of `RuntimeException` (and `Error`).
- Not checked by the compiler; handling is optional.

#### 📝 Example
```java
public class UncheckedExample {
    public int divide(int a, int b) {
        return a / b; // may throw ArithmeticException
    }
}
```

### 🔑 Characteristics of Unchecked Exceptions

- Represent **programming errors**: e.g., null pointer, array index out of bounds, illegal argument.
- Caller **may or may not handle them**.
- Indicate **bugs or incorrect API usage**, not recoverable situations.

---

### 3. Checked vs Unchecked: Comparison Table

| **Feature**            | **Checked Exception**            | **Unchecked Exception**            |
|-------------------------|-----------------------------------|-------------------------------------|
| **Compiler Check**      | ✅ Compile-time checked          | ❌ Not checked by compiler          |
| **Subclass**            | `Exception` (except `Runtime`)  | `RuntimeException` / `Error`       |
| **Handling**            | Mandatory (`try-catch` / `throws`) | Optional                          |
| **Typical Use Case**    | Recoverable errors (IO, SQL)     | Programming errors (NPE, illegal args) |
| **API Impact**          | Forces caller to handle/declare | Does not force caller              |

---

### 4. Implications for API Design

#### Use Checked Exceptions When:
- Error is **recoverable** and the client can reasonably handle it.
- **Examples**: `IOException` for file operations, `SQLException` for database operations.

#### Use Unchecked Exceptions When:
- Error is a **programming bug** or **illegal API usage**.
- **Examples**: `IllegalArgumentException`, `NullPointerException`.

#### Guidelines:
- Avoid excessive checked exceptions; they clutter the API.
- Use unchecked exceptions for most runtime issues to simplify the API.
- Document exceptions in JavaDocs even if they are unchecked.

---

### 5. Example API Design
```java
// Checked exception approach
public interface FileReaderAPI {
    String readFile(String path) throws IOException;
}

// Unchecked exception approach
public interface SafeFileReaderAPI {
    String readFile(String path); // may throw RuntimeException like IllegalStateException
}

```
### Checked: Caller must handle `IOException`.

### Unchecked: Cleaner API; caller decides whether to handle errors.

---

### 📌 API Design Tip

- Favor **unchecked exceptions** for simpler, modern APIs.
- Use **checked exceptions** only when recovery is meaningful.
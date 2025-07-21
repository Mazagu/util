# 🚦 Multithreading Guide with Design Patterns

Multithreading enables concurrent execution of tasks, improving performance and responsiveness, especially on multi-core processors.

---

## 🔄 Basics of Multithreading

- **Thread**: A lightweight unit of execution within a process.
- **Concurrency**: Multiple tasks in progress (not necessarily simultaneously).
- **Parallelism**: Multiple tasks executing simultaneously (on multiple cores).
- **Race condition**: When threads access shared data without proper synchronization.
- **Deadlock**: When threads wait on each other indefinitely.
- **Thread-safe**: Code that functions correctly when accessed by multiple threads concurrently.

---

## 🧰 Key Design Patterns for Multithreading

### 1. Thread Pool Pattern

Reuses a fixed set of threads for executing tasks instead of spawning a new thread per task. Reduces overhead and increases performance.

```
// Java
ExecutorService executor = Executors.newFixedThreadPool(5);
executor.submit(() -> { /* task */ });
```

---

### 2. Producer-Consumer Pattern

A producer creates data and puts it into a buffer; a consumer takes the data. They synchronize via queues.

```
// Python
import queue, threading

q = queue.Queue()

def producer():
    q.put("data")

def consumer():
    data = q.get()
    print(data)
```

---

### 3. Future/Promise Pattern

Allows a thread to compute a result in the future, which another thread can retrieve once available.

```
// Java
Future<String> future = executor.submit(() -> "result");
String result = future.get(); // blocks until ready
```

---

### 4. Read-Write Lock Pattern

Allows multiple readers or one writer at a time, improving concurrency when reads are more frequent than writes.

```
// Java
ReadWriteLock lock = new ReentrantReadWriteLock();
lock.readLock().lock();
// reading...
lock.readLock().unlock();
```

---

### 5. Fork-Join Pattern

Splits tasks into smaller subtasks that are processed in parallel and then joined for final result.

```
// Java
ForkJoinPool pool = new ForkJoinPool();
pool.invoke(new RecursiveTaskExample());
```

---

### 6. Active Object Pattern

Encapsulates method execution and its data in a separate thread, decoupling the caller from the execution.

```
// Caller submits task, the active object runs it on its own thread
```

---

### 7. Double-Checked Locking

Used for lazy initialization with better performance by minimizing synchronization.

```
// Java
if (instance == null) {
  synchronized(MyClass.class) {
    if (instance == null) {
      instance = new MyClass();
    }
  }
}
```

---

## 🧪 Tips for Safe Multithreading

- Always use thread-safe data structures (e.g., `ConcurrentHashMap`)
- Avoid shared mutable state when possible
- Use immutability and message-passing where applicable
- Watch out for deadlocks and starvation
- Prefer higher-level concurrency abstractions (like `ExecutorService`, `Channel`, `Coroutine`, etc.)

---

## 🔍 Recommended Tools and Libraries

| Language | Library / Tool                |
|----------|-------------------------------|
| Java     | `java.util.concurrent`        |
| Python   | `threading`, `concurrent.futures`, `asyncio` |
| Go       | Goroutines + Channels         |
| Rust     | Tokio, async/await            |
| C#       | Tasks, async/await            |

# 📈 Big O Notation: Writing Efficient Algorithms

Big O Notation is a way to describe the **upper bound** of an algorithm’s time or space complexity as a function of the input size **n**. It helps developers measure **scalability**, **performance**, and make informed choices.

---

## 🔹 O(1) – Constant Time

- The runtime doesn't change as input grows.
- Fastest possible performance.

**Examples**:
```
arr[5] // Access element at index
hashMap.put(key, value) // Insert in hash map
```

✅ **Use Case**: Hash table lookup, setting a value in a fixed-size array.

---

## 🔹 O(n) – Linear Time

- Runtime grows proportionally with the input size.

**Examples**:
```
for (int i = 0; i < n; i++) {
    sum += arr[i];
}
```

✅ **Use Case**: Looping through an array or list.

---

## 🔹 O(log n) – Logarithmic Time

- Each step reduces the problem size in half.

**Examples**:
```
// Binary Search
int binarySearch(int[] arr, int target) {
    int low = 0, high = arr.length - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```

✅ **Use Case**: Binary search, operations on balanced BSTs.

---

## 🔹 O(n log n) – Linearithmic Time

- Combines linear and logarithmic growth.
- Most efficient sorting algorithms fall here.

**Examples**:
```
Merge Sort, Quick Sort, Heap Sort
```

✅ **Use Case**: Large-scale data sorting.

---

## 🔹 O(n²) – Quadratic Time

- Performance drops fast as input grows.
- Typically found in nested loops.

**Examples**:
```
for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        // Do something
    }
}
```

✅ **Use Case**: Simple comparison-based sorting, brute-force string comparison.

---

## 🔹 O(n³) – Cubic Time

- Even more nested loops — often in matrix operations.

**Examples**:
```
// Naive matrix multiplication
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        for (int k = 0; k < n; k++)
            C[i][j] += A[i][k] * B[k][j];
```

✅ **Use Case**: Dense matrix multiplication.

---

## 🔹 O(√n) – Square Root Time

- Often seen in mathematical algorithms or optimizations.

**Examples**:
```
// Check for prime numbers
for (int i = 2; i <= sqrt(n); i++) {
    if (n % i == 0) return false;
}
```

✅ **Use Case**: Sieve of Eratosthenes, divisibility checks.

---

## 🔹 O(2ⁿ) – Exponential Time

- Performance becomes infeasible quickly.
- Each step creates 2 new subproblems.

**Examples**:
```
// Fibonacci Recursive
int fib(int n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
```

❌ **Avoid** unless input is very small.

---

## 🔹 O(n!) – Factorial Time

- Grows faster than any other — used for permutations or brute-force.

**Examples**:
```
// Generating permutations of an array
```

❗ **Warning**: Explodes with small increases in input size.

---

## 📊 Big O Comparison Chart

| Big O     | Input Size 10 | Input Size 100 |
|-----------|---------------|----------------|
| O(1)      | 1             | 1              |
| O(log n)  | ~3            | ~7             |
| O(n)      | 10            | 100            |
| O(n log n)| ~30           | ~700           |
| O(n²)     | 100           | 10,000         |
| O(2ⁿ)     | 1024          | Huge           |
| O(n!)     | 3.6M          | Massive        |

---

## 🧠 Final Tips

- Always strive for the lowest **time complexity** possible.
- Optimize **nested loops**, **recursion**, and **data structures** used.
- Remember: space complexity matters too (Big O for memory).

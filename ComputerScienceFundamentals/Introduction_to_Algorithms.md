# 📘 Introduction to Algorithms

---

## 🤖 What is an Algorithm?

An **algorithm** is a step-by-step procedure or formula for solving a problem or performing a task. Algorithms are at the heart of Computer Science, driving everything from basic calculations to complex systems like search engines or AI models.

---

## 🔍 Why Algorithms Matter

- **Efficiency**: Good algorithms reduce time and memory consumption.
- **Scalability**: Essential for handling large input sizes.
- **Correctness**: Ensure your program always produces valid output.
- **Reusability**: Generic algorithms can solve multiple problems.

---

## 🧠 Algorithm Classification

Algorithms can be classified in many ways. Here are the most important categories:

---

### 1. **By Approach/Paradigm**

| Paradigm            | Description                                                      | Example Algorithms              |
|---------------------|------------------------------------------------------------------|----------------------------------|
| **Divide & Conquer**| Break the problem into subproblems, solve them, and combine.     | Merge Sort, Quick Sort, Binary Search |
| **Dynamic Programming (DP)** | Store subproblem results to avoid recomputation.         | Fibonacci, Knapsack, LCS        |
| **Greedy**          | Make the locally optimal choice at each step.                    | Dijkstra, Huffman Coding        |
| **Backtracking**    | Try all solutions recursively, backtrack if needed.              | N-Queens, Sudoku Solver         |
| **Brute Force**     | Try every possibility exhaustively.                              | Linear Search, Permutations     |
| **Recursion**       | Solve by calling itself on smaller input.                        | Factorial, Towers of Hanoi      |
| **Iterative**       | Use loops instead of recursion.                                  | Counting, Simple Loops          |

---

### 2. **By Problem Type**

| Problem Type       | Examples                                  |
|--------------------|-------------------------------------------|
| **Sorting**        | Quick Sort, Merge Sort, Bubble Sort       |
| **Searching**      | Binary Search, Linear Search              |
| **Graph Algorithms** | BFS, DFS, Dijkstra, Kruskal             |
| **String Algorithms** | KMP, Rabin-Karp, Trie Search            |
| **Numerical**      | GCD, Sieve of Eratosthenes                |
| **Geometric**      | Convex Hull, Line Intersections           |

---

### 3. **By Time/Space Complexity**

- **Constant Time**: `O(1)`
- **Linear Time**: `O(n)`
- **Logarithmic**: `O(log n)`
- **Linearithmic**: `O(n log n)`
- **Quadratic/Cubic**: `O(n²)`, `O(n³)`
- **Exponential/Factorial**: `O(2^n)`, `O(n!)`

---

## 🛠️ Example: Linear Search in Go

```
package main

import "fmt"

func linearSearch(arr []int, target int) int {
	for i, v := range arr {
		if v == target {
			return i
		}
	}
	return -1
}

func main() {
	arr := []int{1, 4, 6, 9, 10}
	target := 6
	index := linearSearch(arr, target)

	if index != -1 {
		fmt.Printf("Found %d at index %d\n", target, index)
	} else {
		fmt.Println("Not found")
	}
}
```

---

## 🔁 Deterministic vs Non-Deterministic Algorithms

| Type               | Description                                                  |
|--------------------|--------------------------------------------------------------|
| **Deterministic**  | Produces the same output for the same input every time.      |
| **Non-Deterministic** | Might behave differently across runs (e.g., randomized). |

---

## 📏 Performance Metrics

| Metric         | Description                                |
|----------------|--------------------------------------------|
| **Time Complexity** | How execution time scales with input size |
| **Space Complexity**| How much memory is required               |
| **Best/Average/Worst Case** | Performance under different conditions |

---

## 🧪 Analyzing Algorithms

1. **Understand the input size (`n`)**
2. **Identify loops/recursion**
3. **Track memory allocation**
4. **Use asymptotic notation (Big O)**

---

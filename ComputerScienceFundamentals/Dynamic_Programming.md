# 🧠 Dynamic Programming (DP)

Dynamic Programming is a method for solving complex problems by **breaking them down into overlapping subproblems**, solving each subproblem just once, and storing the solution.

---

## 🔑 Key Characteristics

- **Optimal Substructure**: The optimal solution can be built from optimal solutions of its subproblems.
- **Overlapping Subproblems**: The problem can be broken down into smaller, reusable subproblems.

---

## 🧰 Approaches

### 1. **Top-Down (Memoization)**
- Recursive
- Cache results to avoid recomputation

### 2. **Bottom-Up (Tabulation)**
- Iterative
- Build up a table from base cases

---

## 🧪 Pros and Cons

| Pros                              | Cons                                 |
|-----------------------------------|--------------------------------------|
| Efficient with overlapping subproblems | Higher memory usage (can optimize) |
| Avoids recomputation              | Requires identifying optimal substructure |
| Can be applied to many problems   | Debugging recursive logic can be tricky |

---

## 🧩 Classic Problems (DP)

- Fibonacci Numbers
- 0/1 Knapsack
- Longest Common Subsequence
- Longest Increasing Subsequence
- Coin Change
- Edit Distance
- Matrix Chain Multiplication

---

## 📘 Examples in Go

### 1. Fibonacci (Top-Down)

<codexample>

```go
package main

import "fmt"

var memo = map[int]int{}

func fib(n int) int {
	if n <= 1 {
		return n
	}
	if val, ok := memo[n]; ok {
		return val
	}
	memo[n] = fib(n-1) + fib(n-2)
	return memo[n]
}

func main() {
	fmt.Println("Fib(10):", fib(10))
}
```

### 2. Fibonacci (Bottom-Up)

```go
package main

import "fmt"

func fib(n int) int {
	if n <= 1 {
		return n
	}
	dp := make([]int, n+1)
	dp[0], dp[1] = 0, 1
	for i := 2; i <= n; i++ {
		dp[i] = dp[i-1] + dp[i-2]
	}
	return dp[n]
}

func main() {
	fmt.Println("Fib(10):", fib(10))
}
```

### 3. 0/1 Knapsack

```go
package main

import "fmt"

func knapsack(weights, values []int, W int) int {
	n := len(weights)
	dp := make([][]int, n+1)
	for i := range dp {
		dp[i] = make([]int, W+1)
	}

	for i := 1; i <= n; i++ {
		for w := 0; w <= W; w++ {
			if weights[i-1] <= w {
				dp[i][w] = max(dp[i-1][w], dp[i-1][w-weights[i-1]]+values[i-1])
			} else {
				dp[i][w] = dp[i-1][w]
			}
		}
	}
	return dp[n][W]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}

func main() {
	weights := []int{1, 3, 4, 5}
	values := []int{1, 4, 5, 7}
	W := 7
	fmt.Println("Max value:", knapsack(weights, values, W))
}
```

## 🧠 Tips for DP
- Identify the subproblem and state transition
- Use memoization to reduce time complexity
- Prefer bottom-up for iterative space/time optimization
- Use 1D arrays for space-optimized versions when possible
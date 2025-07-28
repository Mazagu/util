# 📘 Greedy Algorithms

Greedy algorithms make the **locally optimal choice** at each step with the hope of finding the **global optimum**. They don’t always produce the best solution for every problem but are **efficient and effective** for many.

---

## ✅ When to Use Greedy Algorithms

- **Greedy Choice Property**: A global optimum can be arrived at by choosing a local optimum.
- **Optimal Substructure**: The problem can be broken into subproblems that can be solved optimally.

---

## 🧠 Common Use Cases

- Activity Selection
- Fractional Knapsack
- Huffman Encoding
- Minimum Spanning Tree (Kruskal’s, Prim’s)
- Dijkstra’s Algorithm (Shortest path)
- Coin Change (Greedy version — not always optimal)

---

## 🧪 Pros and Cons

| Pros                        | Cons                                |
|-----------------------------|-------------------------------------|
| Simple and easy to implement| Doesn’t guarantee optimal result    |
| Fast and efficient (often O(n log n))| May require sorting or preprocessing |
| Useful in approximation     | Only works if greedy conditions are met |

---

## 📌 Examples in Go

### 1. Activity Selection Problem

**Problem**: Given start and end times, choose max number of activities that don't overlap.

<codexample>

```go
package main

import (
	"fmt"
	"sort"
)

type Activity struct {
	start, end int
}

func maxActivities(activities []Activity) int {
	sort.Slice(activities, func(i, j int) bool {
		return activities[i].end < activities[j].end
	})

	count := 1
	lastEnd := activities[0].end

	for i := 1; i < len(activities); i++ {
		if activities[i].start >= lastEnd {
			count++
			lastEnd = activities[i].end
		}
	}

	return count
}

func main() {
	activities := []Activity{{1, 3}, {2, 5}, {4, 6}, {6, 7}, {5, 9}, {8, 9}}
	fmt.Println("Max non-overlapping activities:", maxActivities(activities))
}
```

### 2. Fractional Knapsack Problem
Problem: Maximize profit given weights and values — items can be broken.

```go
package main

import (
	"fmt"
	"sort"
)

type Item struct {
	value, weight int
	ratio         float64
}

func fractionalKnapsack(capacity int, items []Item) float64 {
	for i := range items {
		items[i].ratio = float64(items[i].value) / float64(items[i].weight)
	}

	sort.Slice(items, func(i, j int) bool {
		return items[i].ratio > items[j].ratio
	})

	totalValue := 0.0
	for _, item := range items {
		if capacity >= item.weight {
			totalValue += float64(item.value)
			capacity -= item.weight
		} else {
			totalValue += float64(capacity) * item.ratio
			break
		}
	}

	return totalValue
}

func main() {
	items := []Item{{60, 10}, {100, 20}, {120, 30}}
	capacity := 50
	fmt.Printf("Max value: %.2f\n", fractionalKnapsack(capacity, items))
}
```

### 3. Coin Change (Greedy)
Problem: Minimize number of coins to make a value.

⚠️ Only works correctly when the denominations are canonical (e.g., {1, 5, 10, 25}).

```go
package main

import (
	"fmt"
)

func minCoins(coins []int, amount int) int {
	count := 0
	for i := len(coins) - 1; i >= 0; i-- {
		for amount >= coins[i] {
			amount -= coins[i]
			count++
		}
	}
	return count
}

func main() {
	coins := []int{1, 5, 10, 25}
	amount := 63
	fmt.Println("Minimum coins:", minCoins(coins, amount))
}
```

🛠️ Tips for Implementing Greedy Algorithms
- Sort inputs if necessary (by weight, deadline, etc.)
- Use a greedy decision-making rule (maximize, minimize, earliest end, etc.)
- Always verify that greedy conditions apply for your problem

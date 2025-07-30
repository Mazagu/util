# 🧠 Divide and Conquer

**Divide and Conquer** is a fundamental algorithm design paradigm that solves a problem by:

1. **Dividing** it into smaller subproblems,
2. **Conquering** each subproblem recursively, and
3. **Combining** the solutions to form the final answer.

---

## 🔧 Key Characteristics

- Problems are divided into non-overlapping subproblems.
- Each subproblem is solved independently.
- Subproblem solutions are combined.

---

## 🔁 Steps

1. **Divide**: Split the problem into smaller instances.
2. **Conquer**: Solve each instance recursively.
3. **Combine**: Merge the results from subproblems.

---

## 🧠 Time Complexity

Often represented by the recurrence:

```
T(n) = aT(n/b) + O(n^d)
```


Apply the **Master Theorem** to solve recurrence relations.

---

## ✅ Classic Use Cases

- Merge Sort
- Quick Sort
- Binary Search
- Closest Pair of Points
- Fast Fourier Transform
- Matrix Multiplication (Strassen's Algorithm)

---

## 📘 Examples in Go

### 1. Merge Sort

```go
package main

import "fmt"

func mergeSort(arr []int) []int {
	if len(arr) <= 1 {
		return arr
	}
	mid := len(arr) / 2
	left := mergeSort(arr[:mid])
	right := mergeSort(arr[mid:])
	return merge(left, right)
}

func merge(a, b []int) []int {
	result := []int{}
	i, j := 0, 0
	for i < len(a) && j < len(b) {
		if a[i] < b[j] {
			result = append(result, a[i])
			i++
		} else {
			result = append(result, b[j])
			j++
		}
	}
	result = append(result, a[i:]...)
	result = append(result, b[j:]...)
	return result
}

func main() {
	arr := []int{5, 2, 9, 1, 3, 6}
	fmt.Println("Sorted:", mergeSort(arr))
}
```

### 2. Quick Sort

```go
package main

import "fmt"

func quickSort(arr []int) []int {
	if len(arr) <= 1 {
		return arr
	}
	pivot := arr[0]
	left, right := []int{}, []int{}
	for _, v := range arr[1:] {
		if v < pivot {
			left = append(left, v)
		} else {
			right = append(right, v)
		}
	}
	return append(append(quickSort(left), pivot), quickSort(right)...)
}

func main() {
	arr := []int{8, 4, 7, 3, 10, 1}
	fmt.Println("Sorted:", quickSort(arr))
}
```

### 3. Binary Search

```go
package main

import "fmt"

func binarySearch(arr []int, target int) int {
	low, high := 0, len(arr)-1
	for low <= high {
		mid := (low + high) / 2
		if arr[mid] == target {
			return mid
		} else if arr[mid] < target {
			low = mid + 1
		} else {
			high = mid - 1
		}
	}
	return -1
}

func main() {
	arr := []int{1, 2, 3, 4, 5, 6}
	fmt.Println("Index of 4:", binarySearch(arr, 4))
}
```

## 🧭 Differences from DP

| Aspect           | Divide & Conquer            | Dynamic Programming            |
|------------------|-----------------------------|--------------------------------|
| Subproblems      | Independent                 | Overlapping                    |
| Combination step | Required                    | Often not required             |
| Reuse of results | No                          | Yes (memoization/tabulation)   |

---

## 🧠 Tips

- Choose divide size wisely (halving is common)
- Recursion depth can be optimized via tail recursion
- Combine step must be efficient to avoid extra cost

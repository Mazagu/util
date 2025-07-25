# 📘 Deep Dive into Sorting Algorithms

---

## 📦 What is Sorting?

**Sorting** is the process of arranging data in a particular format or order — typically ascending or descending. It’s one of the most fundamental operations in computing, enabling faster searches, easier visualization, and efficient data structures.

---

## 🧠 Why Sorting Matters

- Improves **search efficiency** (e.g., binary search)
- Required for **data normalization**
- Crucial for **algorithms like merge joins** or deduplication
- Foundation for **interview questions** and **competitive programming**

---

## 🧰 Classification of Sorting Algorithms

| Criteria           | Type                        | Examples                         |
|-------------------|-----------------------------|----------------------------------|
| **Time Complexity** | Comparison-based vs Non-Comparison | Merge Sort, Counting Sort       |
| **Stability**       | Stable vs Unstable          | Merge Sort (stable), Quick Sort (unstable) |
| **In-place**        | In-place vs Not             | Quick Sort (in-place), Merge Sort (not) |
| **Adaptiveness**    | Adaptive vs Non-Adaptive    | Insertion Sort (adaptive), Merge Sort (not) |

---

## 🔄 Common Sorting Algorithms

### 1. **Bubble Sort**
- **Time**: O(n²)
- **Space**: O(1)
- **Stable**: ✅
- **In-Place**: ✅
- **Description**: Repeatedly swap adjacent elements if they’re in the wrong order.

```
func bubbleSort(arr []int) {
	n := len(arr)
	for i := 0; i < n-1; i++ {
		for j := 0; j < n-i-1; j++ {
			if arr[j] > arr[j+1] {
				arr[j], arr[j+1] = arr[j+1], arr[j]
			}
		}
	}
}
```

---

### 2. **Selection Sort**
- **Time**: O(n²)
- **Space**: O(1)
- **Stable**: ❌
- **In-Place**: ✅
- **Description**: Select the smallest element and move it to the front.

---

### 3. **Insertion Sort**
- **Time**: O(n²), but O(n) best-case
- **Space**: O(1)
- **Stable**: ✅
- **In-Place**: ✅
- **Adaptive**: ✅ (fast for nearly sorted data)

---

### 4. **Merge Sort**
- **Time**: O(n log n)
- **Space**: O(n)
- **Stable**: ✅
- **In-Place**: ❌
- **Divide & Conquer**

```
func mergeSort(arr []int) []int {
	if len(arr) <= 1 {
		return arr
	}
	mid := len(arr) / 2
	left := mergeSort(arr[:mid])
	right := mergeSort(arr[mid:])
	return merge(left, right)
}

func merge(left, right []int) []int {
	result := []int{}
	i, j := 0, 0
	for i < len(left) && j < len(right) {
		if left[i] <= right[j] {
			result = append(result, left[i])
			i++
		} else {
			result = append(result, right[j])
			j++
		}
	}
	result = append(result, left[i:]...)
	result = append(result, right[j:]...)
	return result
}
```

---

### 5. **Quick Sort**
- **Time**: O(n log n) average, O(n²) worst-case
- **Space**: O(log n)
- **Stable**: ❌
- **In-Place**: ✅
- **Divide & Conquer**

---

### 6. **Heap Sort**
- **Time**: O(n log n)
- **Space**: O(1)
- **Stable**: ❌
- **In-Place**: ✅
- Uses a binary heap to sort

---

### 7. **Counting Sort**
- **Time**: O(n + k)
- **Space**: O(k)
- **Stable**: ✅
- **Only for integers or limited range**

---

### 8. **Radix Sort**
- **Time**: O(nk)
- **Stable**: ✅
- **Used for sorting numbers by individual digits**

---

### 9. **Bucket Sort**
- Divide input into buckets and sort each individually (often with insertion sort or another sorting algorithm).

---

## 🧠 Stability in Sorting

| Algorithm       | Stable |
|----------------|--------|
| Bubble Sort     | ✅      |
| Selection Sort  | ❌      |
| Insertion Sort  | ✅      |
| Merge Sort      | ✅      |
| Quick Sort      | ❌      |
| Heap Sort       | ❌      |
| Counting Sort   | ✅      |
| Radix Sort      | ✅      |

---

## 📊 When to Use What?

| Case                                     | Use                              |
|------------------------------------------|-----------------------------------|
| Small dataset, nearly sorted             | Insertion Sort                   |
| Large dataset, consistent performance    | Merge Sort                       |
| Space is a concern                       | Quick Sort, Heap Sort            |
| Sorting integers in a known small range  | Counting Sort, Radix Sort        |
| Need stable sorting                      | Merge Sort, Insertion Sort       |

---

## 🧪 Benchmarking Tips

- Test with sorted, reversed, and random input
- Measure both time and space complexity
- Consider worst-case, average-case, and best-case performance

---

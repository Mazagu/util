# 📚 Quick Guide to Data Structures

---

## 📌 What are Data Structures?

A **data structure** is a way of organizing, managing, and storing data efficiently so that it can be accessed and modified effectively.

They are the foundation of efficient algorithms and system design.

---

## 🧱 Basic Data Structures

### 1. **Array**
- Fixed-size collection of elements indexed by position.
- Good for **indexed access**.
- Poor for **insertion/deletion** in the middle.

```
arr = [1, 2, 3]
arr[0]  # Access element at index 0
```

---

### 2. **Linked List**
- A linear collection of nodes where each node points to the next.
- Great for **insertions/deletions**.
- Poor for **indexed access**.

```
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None
```

---

### 3. **Stack**
- Last-In, First-Out (LIFO)
- Used in undo operations, call stacks, parsing

```
stack = []
stack.append(1)
stack.pop()  # Removes 1
```

---

### 4. **Queue**
- First-In, First-Out (FIFO)
- Used in scheduling, message passing

```
from collections import deque
queue = deque()
queue.append(1)
queue.popleft()
```

---

### 5. **Hash Table (Dictionary/Map)**
- Key-value pairs
- Fast access, insertion, and deletion

```
map = {"a": 1, "b": 2}
map["a"]  # Returns 1
```

---

### 6. **Set**
- Collection of unique elements
- Great for membership tests

```
s = {1, 2, 3}
1 in s  # True
```

---

## 🌲 Advanced Data Structures

### 7. **Binary Tree**
- Each node has at most two children
- Used in hierarchical data

### 8. **Binary Search Tree (BST)**
- Ordered binary tree
- Left < Root < Right

### 9. **Heap (Min/Max)**
- Complete tree structure
- Fast access to min/max element

### 10. **Trie**
- Prefix tree, used for auto-complete and dictionary word storage

### 11. **Graph**
- Nodes connected by edges
- Used in networks, social graphs, routing

---

## 🧠 Choosing the Right Data Structure

| Task                         | Suggested Structure         |
|------------------------------|-----------------------------|
| Fast lookup                  | Hash Table (Map)            |
| Insertion/deletion at ends   | Stack/Queue                 |
| Ordered data                 | Binary Search Tree, Array   |
| Prefix-based search          | Trie                        |
| Hierarchical data            | Tree                        |
| Complex relationships        | Graph                       |

---

## 📈 Time Complexities

| Operation     | Array | LinkedList | HashMap | BST (Balanced) |
|---------------|-------|------------|---------|----------------|
| Access        | O(1)  | O(n)       | O(1)    | O(log n)       |
| Search        | O(n)  | O(n)       | O(1)    | O(log n)       |
| Insert/Delete | O(n)  | O(1)       | O(1)    | O(log n)       |

---

Understanding data structures is critical to designing efficient algorithms and systems. Choose the right one based on the problem requirements!

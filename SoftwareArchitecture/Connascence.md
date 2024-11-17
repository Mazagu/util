# Connascence: A Guide to Understanding and Applying It

## What is Connascence?

Connascence is a term used in software design and architecture to describe the degree of interdependence between components or modules. The concept is pivotal in understanding and improving code quality, as it helps assess how changes in one part of the codebase might affect others.

The term was introduced by Meilir Page-Jones in *What Every Programmer Should Know About Object-Oriented Design*. It emphasizes **manageability and maintainability** over time.

---

## Types of Connascence

Connascence can be categorized into **static** and **dynamic**, depending on when the interdependencies occur.

### Static Connascence
Occurs at design or compile time.

- **Connascence of Name (CoN)**: Two components depend on using the same identifier (e.g., variable names or function names).
  - **Example**: A class and its instances reference the same variable name.

- **Connascence of Type (CoT)**: Components must agree on the type of data.
  - **Example**: A function expects a specific type for its argument.

- **Connascence of Meaning (CoM)**: Components rely on an agreed-upon semantic interpretation.
  - **Example**: A specific range of values is interpreted as statuses in a shared enum.

- **Connascence of Position (CoP)**: The position of data matters in communication between components.
  - **Example**: Positional arguments in a function call.

---

### Dynamic Connascence
Occurs at runtime.

- **Connascence of Execution (CoE)**: Components must execute in a specific order.
  - **Example**: A database connection must be established before executing a query.

- **Connascence of Timing (CoT)**: Components depend on specific timing or delays.
  - **Example**: Two services must synchronize their clocks.

- **Connascence of Values (CoV)**: Components depend on exact values.
  - **Example**: Magic numbers or hard-coded configurations shared across modules.

---

## Principles of Managing Connascence

1. **Minimize Connascence Across Boundaries**: Reduce dependencies between modules, classes, or systems, especially those owned by different teams.
   
2. **Prefer Weaker Connascence**: Choose connascence types that are easier to manage or refactor (e.g., name vs. execution).

3. **Promote Encapsulation**: Encapsulate high connascence within a single module or class to localize the impact of changes.

4. **Increase Explicitness**: Use documentation and type systems to make interdependencies clear.

---

## Evaluating Connascence

When evaluating connascence, consider these dimensions:

- **Strength**: How tightly coupled the components are.
- **Degree**: How many components are affected by the dependency.
- **Direction**: Which components depend on which (unidirectional or bidirectional).

---

## Connascence in Practice

### Example 1: Refactoring Connascence of Position
**Before**:
```python
def calculate(a, b, c):
    return a + b - c

calculate(10, 5, 2)
```
**After:**

```python
def calculate(a=0, b=0, c=0):
    return a + b - c

calculate(a=10, b=5, c=2)
```
### Example 2: Breaking Connascence of Name
**Before:**

```javascript
let globalCounter = 0;
function incrementCounter() {
    globalCounter++;
}
```
**After:**

```javascript
class Counter {
    constructor() {
        this.counter = 0;
    }
    increment() {
        this.counter++;
    }
}
const myCounter = new Counter();
myCounter.increment();
```
---
## Summary
Understanding connascence helps you:

- Write maintainable and modular code.
- Evaluate and improve design quality.
- Facilitate team collaboration by reducing interdependencies.

By systematically reducing and managing connascence, you create systems that are more robust, flexible, and easier to evolve.
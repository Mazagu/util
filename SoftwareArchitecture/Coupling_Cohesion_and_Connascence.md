# 🧠 Guide: Coupling, Cohesion, and Connascence in Software Architecture

In software design, understanding how parts of a system depend on and relate to one another is critical. This guide explores:

- **Coupling**: How tightly components depend on each other
- **Cohesion**: How well elements inside a module belong together
- **Connascence**: A deeper framework for analyzing interdependencies

---

## 🔗 1. Coupling: How Tightly Modules Are Connected

**Coupling** is the degree of **interdependence** between modules or components.

### Types of Coupling (from worst to best):
```
| Type             | Description                                      |
|------------------|--------------------------------------------------|
| Content Coupling | One module directly accesses internal data of another |
| Common Coupling  | Modules share global variables                   |
| Control Coupling | One module controls the flow of another          |
| Stamp Coupling   | Modules share only part of a data structure      |
| Data Coupling    | Modules communicate by passing only needed data  |
| Message Coupling | Modules interact only via well-defined messages  |
```
### 🔴 Example: Tight Coupling

```
// Bad: tight coupling to internal structure
class Order {
  public List<Item> items;
}

class Invoice {
  public double calculateTotal(Order order) {
    return order.items.stream().mapToDouble(Item::getPrice).sum();
  }
}
```

### 🟢 Better: Looser Coupling via Interface

```
interface Order {
  double getTotal();
}
```

---

## 📦 2. Cohesion: How Focused a Module Is

**Cohesion** measures how **functionally related** the responsibilities of a single module are.

### Types of Cohesion (from worst to best):
```
| Type               | Description                                 |
|--------------------|---------------------------------------------|
| Coincidental       | Unrelated tasks grouped together            |
| Logical            | Similar tasks grouped but selected by flags |
| Temporal           | Grouped by timing (e.g. init functions)     |
| Procedural         | Ordered steps grouped together              |
| Communicational    | Functions operate on same data              |
| Functional         | All operations contribute to one goal       |
```
### 🔴 Low Cohesion Example

```
// Bad: unrelated responsibilities
class Utility {
  void sendEmail() {}
  void generatePDF() {}
  void calculateDiscount() {}
}
```

### 🟢 High Cohesion Example

```
class InvoiceService {
  void generatePDFInvoice() {}
  void calculateInvoiceTotal() {}
}
```

---

## 🔁 3. Connascence: How Changes Propagate

**Connascence** is a more nuanced measure of dependency: if one part changes, **must** another?

Introduced by Meilir Page-Jones, it breaks down interdependency into **types** and **strengths**.

### 📊 Categories of Connascence
```
| Type                 | Meaning                                            | Example                                 |
|----------------------|----------------------------------------------------|-----------------------------------------|
| Name                 | Agree on names                                     | Method parameters, variable names       |
| Type                 | Agree on data types                                | Passing a `String` vs `int`             |
| Meaning              | Agree on semantic meaning                          | `"Y"` = yes, `"N"` = no                 |
| Position             | Order of parameters matters                        | `update(name, id)` vs `update(id, name)`|
| Algorithm            | Modules rely on the same algorithm                 | Hashing, compression                    |
| Execution            | Must run in specific order                         | Init → Validate → Process               |
| Timing               | Must happen within time bounds                     | Race conditions, timeout logic          |
```
### 🔴 High Connascence Example

```
// Position & Type Connascence
void createUser(String name, int age, boolean active);
```

A small change (e.g. swapping order or changing a type) breaks all callers.

### 🟢 Refactored to Reduce Connascence

```
record UserInput(String name, int age, boolean active);

void createUser(UserInput input);
```

---

## ⚖️ Comparison Summary
```
| Concept      | Definition                                      | Ideal Direction                  |
|--------------|--------------------------------------------------|----------------------------------|
| Coupling     | Degree of interdependence between modules        | Lower                            |
| Cohesion     | Degree of internal relatedness within a module   | Higher                           |
| Connascence  | How tightly two parts must change together       | Reduce strength, increase locality |
```
---

## 🧪 When to Refactor
```
| Smell                       | What It Means                            | What to Do                       |
|-----------------------------|-------------------------------------------|----------------------------------|
| Changing one module breaks many others | High coupling                     | Introduce interfaces, decouple  |
| Class does too many unrelated things   | Low cohesion                      | Split into cohesive components  |
| A small change affects distant modules | High connascence, low locality    | Encapsulate and reduce reach    |
```
---

## 📚 Further Reading

- [Connascence (connascence.io)](https://connascence.io/)
- [Clean Architecture – Uncle Bob](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Layering Principles (Fowler)](https://martinfowler.com/bliki/LayeringPrinciples.html)

---

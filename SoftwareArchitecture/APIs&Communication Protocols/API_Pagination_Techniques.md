# 📚 API Pagination Techniques – A Comprehensive Guide

Pagination is a technique used to divide large datasets into manageable chunks (pages), ensuring faster responses, better performance, and reduced system load.

---

## 📌 Why Use Pagination?

- Prevent **overloading** the backend or database
- Enhance **API performance** and response time
- Improve **UX** for clients and consumers
- Enable **scalability** for growing datasets

---

## 🔢 1. Offset-based Pagination

**Definition:** Uses `offset` and `limit` query parameters to return a slice of results.

**Example:**
```
GET /orders?offset=0&limit=3
```

### ✅ Pros
- Easy to implement and understand
- Works with most SQL databases

### ❌ Cons
- Poor performance on large datasets (e.g., `offset=100000`)
- Risk of **inconsistent data** if rows are inserted/deleted during paging

---

## 🔁 2. Cursor-based Pagination

**Definition:** Uses a unique identifier (often a primary key or encoded cursor) to fetch the next set of records.

**Example:**
```
GET /orders?cursor=YXNka2pfaWQ6MTAwMg==
```

### ✅ Pros
- High performance for large datasets
- Avoids skipping rows; more **consistent**

### ❌ Cons
- Requires consistent sort order (e.g., `ORDER BY created_at`)
- More complex implementation
- Requires opaque cursors (often base64 encoded)

---

## 📄 3. Page-based Pagination

**Definition:** Uses page number and size per page.

**Example:**
```
GET /items?page=2&size=3
```

### ✅ Pros
- Familiar to most developers
- Straightforward implementation

### ❌ Cons
- Suffers from same limitations as offset-based pagination
- Can return duplicate/missing records if underlying data changes

---

## 🔑 4. Keyset-based Pagination

**Definition:** Uses the last item’s key (typically an indexed field) as a starting point for the next page.

**Example:**
```
GET /items?after_id=102&limit=3
```

### ✅ Pros
- Fast and efficient for large datasets
- Stable in dynamic datasets (new records don’t shift offsets)

### ❌ Cons
- Requires indexed, unique sorting key
- Doesn't support jumping to arbitrary pages
- Slightly more complex than offset

---

## ⏱️ 5. Time-based Pagination

**Definition:** Uses timestamps to filter data in a date/time window.

**Example:**
```
GET /items?start_time=2024-01-01T00:00:00Z&end_time=2024-01-02T00:00:00Z
```

### ✅ Pros
- Ideal for time-series data
- Guarantees chronological order
- New data doesn’t interfere with previous pages

### ❌ Cons
- Depends on reliable timestamps
- May return overlapping data with inconsistent clocks

---

## 🧬 6. Hybrid Pagination

**Definition:** Combines multiple pagination strategies (e.g., cursor + time, or offset + keyset).

**Example:**
```
GET /items?cursor=abc123&start_time=2024-06-01T00:00:00Z
```

### ✅ Pros
- Flexible and robust
- Scales well with complex datasets

### ❌ Cons
- Increased implementation complexity
- Requires careful design to avoid data inconsistency

---

## 🧪 Comparison Table

| Method             | Efficiency | Complexity | Jump to Page | Use Case                              |
|--------------------|------------|------------|--------------|----------------------------------------|
| Offset-based       | ❌ Slow on large data | ✅ Simple     | ✅ Yes        | General pagination                    |
| Cursor-based       | ✅ Fast     | ⚠️ Medium   | ❌ No         | Infinite scroll, real-time feeds      |
| Page-based         | ❌ Slow     | ✅ Simple   | ✅ Yes        | UI with "pages", legacy APIs          |
| Keyset-based       | ✅ Very fast | ⚠️ Medium   | ❌ No         | Ordered datasets with primary keys    |
| Time-based         | ✅ Fast     | ⚠️ Medium   | ❌ No         | Time-series, event logs               |
| Hybrid             | ✅✅ Best    | ❌ Complex  | ✅ Custom     | High-performance + accurate datasets  |

---

## 🛠️ Best Practices

- **Always define sorting rules** (e.g., `ORDER BY created_at DESC`)
- Use **opaque cursors** to abstract implementation
- Consider **pagination metadata** (e.g., `hasNext`, `totalCount`, `nextCursor`)
- Avoid using `LIMIT` with high `OFFSET` for real-time data
- Choose technique based on **dataset size**, **query cost**, and **user interaction pattern**

---

## 📚 Resources

- [RFC 8288: Web Linking (rel=next)](https://datatracker.ietf.org/doc/html/rfc8288)
- [GraphQL Connections + Relay Cursors](https://relay.dev/graphql/connections.htm)
- [Best practices for cursor-based pagination](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2020/cursor-pagination)

---

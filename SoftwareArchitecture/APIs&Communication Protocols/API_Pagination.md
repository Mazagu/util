# 📄 API Pagination – Quick Guide

Pagination helps manage large datasets by splitting results into smaller, manageable chunks.

---

## 📌 1. Offset-based Pagination

Uses `offset` and `limit` to retrieve pages.

```
Example:
GET /users?offset=20&limit=10
```

**Pros:**
- Simple and widely supported  
**Cons:**
- Inefficient for large offsets (slow skips)
- Risk of missing/duplicating data if data changes between pages

---

## 📌 2. Page-based Pagination

Specifies `page` and `size` of each page.

```
Example:
GET /users?page=3&size=10
```

**Pros:**
- Intuitive for end users  
**Cons:**
- Suffers same issues as offset-based for large datasets

---

## 📌 3. Cursor-based Pagination

Uses a unique identifier (cursor) to fetch next results.

```
Example:
GET /users?cursor=eyJpZCI6MTAw
```

**Pros:**
- Efficient for real-time or changing data
- Consistent performance  
**Cons:**
- Slightly harder to implement
- Cursors must be encoded and managed

---

## 📌 4. Keyset-based Pagination

Paginates using an indexed key like `id`, `created_at`.

```
Example:
GET /users?after_id=100&limit=10
```

**Pros:**
- High performance
- Avoids skipping records  
**Cons:**
- Requires sortable, unique key
- No random access to arbitrary pages

---

## 📌 5. Time-based Pagination

Uses timestamps to paginate chronologically.

```
Example:
GET /events?start=2024-01-01T00:00&end=2024-01-02T00:00
```

**Pros:**
- Great for logs, time-series, activity feeds  
**Cons:**
- Requires accurate and consistent time data

---

## 📌 6. Hybrid Pagination

Combines techniques (e.g., cursor + timestamp).

```
Example:
GET /logs?cursor=abc123&start_time=2024-01-01
```

**Pros:**
- Best of both worlds  
**Cons:**
- Most complex to implement and maintain

---

## ✅ Best Practices

- Always return pagination metadata (total count, next/prev links, cursors)
- Document the pagination strategy clearly in your API docs
- Avoid relying on offset for changing data sets

---

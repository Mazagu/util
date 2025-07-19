# 🧬 System Design: Low-Level Design (LLD)

Low-Level Design (LLD) focuses on the **internal structure** of components defined in High-Level Design. It captures **class diagrams**, **database schemas**, **API definitions**, and **detailed logic**.

---

## 🎯 Purpose of LLD

- Blueprint for **developers** to implement features
- Reveals detailed behavior of each module
- Clarifies **business logic**, data models, and interfaces
- Enables **unit testing**, code reviews, and scalability validation

---

## 🧩 Key Components of LLD

### 1. Class Design / Object Model

Define internal data structures and classes with responsibilities.

```
class Post {
    int id;
    int userId;
    string content;
    datetime createdAt;

    bool isVisible();
    List<Comment> getComments();
}
```

---

### 2. Database Schema Design

Tables, indexes, relationships, constraints.

```
Table: posts
- id (PK)
- user_id (FK)
- content TEXT
- created_at TIMESTAMP
- visibility BOOLEAN
```

---

### 3. API Contract

Detailed request and response structure.

```
POST /api/v1/posts

Request:
{
  "userId": 1,
  "content": "Hello world!"
}

Response:
{
  "id": 123,
  "createdAt": "2025-07-15T12:00:00Z"
}
```

---

### 4. Sequence Diagrams (Optional)

Defines the step-by-step interaction between components.

```
Client -> API Gateway -> Post Service -> DB
Post Service -> Notification Service (async)
```

---

### 5. Data Flow and Validation

Explicitly show:

```
- How data is validated at each step
- How errors are handled
- What business rules apply
```

---

### 6. Pseudocode or Logic Workflows

When logic is complex, include step-by-step logic.

```
function createPost(userId, content):
    if content.length == 0:
        throw ValidationError("Content required")
    
    save to DB
    publish event to Kafka
```

---

### 7. Error Handling

Define how to handle:

```
- Invalid inputs
- DB constraint violations
- Timeouts or downstream service errors
```

---

### 8. Test Cases and Edge Conditions

Identify key paths and edge cases to test.

```
- Content is empty (400 error)
- User doesn’t exist (404)
- System crash retry logic
```

---

### 9. Performance Constraints

If needed, include:

```
- Query optimizations (e.g., using proper indexes)
- Caching strategy (e.g., Redis for hot content)
```

---

## 🧠 Example: Comment Feature for Social App

### Class Diagram
```
class Comment {
    int id;
    int postId;
    int userId;
    string message;
    datetime createdAt;

    bool isOffensive();
}
```

### API Definition
```
GET /api/v1/posts/:postId/comments

Response:
[
  {
    "id": 1,
    "userId": 42,
    "message": "Nice post!",
    "createdAt": "2025-07-15T12:00:00Z"
  }
]
```

### DB Schema
```
Table: comments
- id (PK)
- post_id (FK)
- user_id (FK)
- message TEXT
- created_at TIMESTAMP
```

---

## ✅ Best Practices

- Follow **SOLID** principles
- Separate **logic** from **data access**
- Use **design patterns** where appropriate (Factory, Strategy, etc.)
- Keep LLD **traceable to HLD**
- Use clear and consistent **naming conventions**

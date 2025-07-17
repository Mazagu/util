# ⚡ CQRS Quick Guide

**CQRS (Command Query Responsibility Segregation)** is a software architecture pattern that separates **read** operations (queries) from **write** operations (commands), enabling better scalability, flexibility, and performance for complex systems.

---

## ✨ Core Concepts

- **Commands**: Change the system state (CreateUser, UpdateOrder, etc.)
- **Queries**: Return data, without side effects (GetUserById, ListOrders, etc.)
- **Command Model**: Optimized for business logic and validation.
- **Query Model**: Optimized for data retrieval, may use denormalized views.

---

## ✅ When to Use CQRS

- Complex domains with lots of business rules.
- High-performance systems with heavy read/write asymmetry.
- When you need different models for reads vs. writes.
- Event-driven architectures.

---

## 🧱 Simple Structure

```
+-----------+           +----------------+
|  Client   |  ----->   |  Command API   |
+-----------+           +----------------+
                             |
                             v
                      Write/Command Model
                             |
                        (Database Write)
                             |
                      Event Store / Bus
                             |
                             v
+-----------+           +----------------+
|  Client   |  <-----   |   Query API    |
+-----------+           +----------------+
                             |
                      Read/Query Database
```

---

## 🛠 Command Example (Write)

```
POST /users

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

This goes through validations, domain logic, and emits a `UserCreatedEvent`.

---

## 🔍 Query Example (Read)

```
GET /users/123

Response:
{
  "id": "123",
  "name": "Alice",
  "email": "alice@example.com"
}
```

This hits a **read-optimized store**, possibly denormalized for performance.

---

## 🔁 Eventual Consistency

CQRS often uses **eventual consistency** between the write and read models.

- Commands trigger domain events.
- A background process updates the read store from events.

---

## 🧩 CQRS vs CRUD

| Feature         | CRUD              | CQRS                         |
|-----------------|------------------|-------------------------------|
| Read/Write Model| Shared            | Separated                     |
| Simplicity      | Simple apps       | Complex apps                  |
| Scaling         | Less flexible     | Easier to scale independently|
| Consistency     | Strong            | Often eventual                |

---

## 🔄 Combine with Event Sourcing

- Use **Event Sourcing** to persist a stream of events instead of storing current state.
- Rebuild state by replaying events.

---

## 🔐 Pros

- Optimized models for read/write.
- Better separation of concerns.
- Easier to scale parts independently.
- Great for systems with complex business logic.

## ⚠️ Cons

- More complexity than traditional CRUD.
- Eventual consistency is harder to manage.
- Higher learning curve and operational overhead.

---

## 📦 Common Use Cases

- E-commerce order and inventory systems.
- Banking and financial apps.
- Real-time analytics dashboards.
- Microservices and event-driven systems.

---

# 📦 CQRS Implementation Appendix: Laravel & Java

---

## 🚀 Laravel CQRS Example

### 📁 Directory Structure (Simplified)
```
app/
├── Commands/
│   └── CreateUserCommand.php
├── Handlers/
│   └── CreateUserHandler.php
├── Queries/
│   └── GetUserQuery.php
├── Controllers/
│   └── UserController.php
```

### 🧩 CreateUserCommand.php
```
namespace App\Commands;

class CreateUserCommand
{
    public string $name;
    public string $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }
}
```

### 🧠 CreateUserHandler.php
```
namespace App\Handlers;

use App\Commands\CreateUserCommand;
use App\Models\User;

class CreateUserHandler
{
    public function handle(CreateUserCommand $command)
    {
        return User::create([
            'name' => $command->name,
            'email' => $command->email,
        ]);
    }
}
```

### 📥 Controller Usage
```
public function store(Request $request)
{
    $command = new CreateUserCommand($request->name, $request->email);
    $handler = new CreateUserHandler();
    $user = $handler->handle($command);

    return response()->json($user);
}
```

### 🔍 GetUserQuery (Simple)
```
namespace App\Queries;

use App\Models\User;

class GetUserQuery
{
    public static function byId(int $id): ?User
    {
        return User::find($id);
    }
}
```

---

## ☕ Java CQRS Example (Spring Boot)

### 📁 Project Structure (Simplified)
```
src/
├── commands/
│   └── CreateUserCommand.java
├── handlers/
│   └── CreateUserHandler.java
├── queries/
│   └── GetUserQuery.java
├── controllers/
│   └── UserController.java
```

### 🧩 CreateUserCommand.java
```
public class CreateUserCommand {
    private String name;
    private String email;

    public CreateUserCommand(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // getters
}
```

### 🧠 CreateUserHandler.java
```
@Service
public class CreateUserHandler {
    private final UserRepository userRepository;

    public CreateUserHandler(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User handle(CreateUserCommand command) {
        User user = new User(command.getName(), command.getEmail());
        return userRepository.save(user);
    }
}
```

### 📥 UserController.java
```
@RestController
@RequestMapping("/users")
public class UserController {
    private final CreateUserHandler createUserHandler;

    public UserController(CreateUserHandler createUserHandler) {
        this.createUserHandler = createUserHandler;
    }

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody CreateUserCommand command) {
        User user = createUserHandler.handle(command);
        return ResponseEntity.ok(user);
    }
}
```

### 🔍 GetUserQuery.java
```
@Service
public class GetUserQuery {
    private final UserRepository userRepository;

    public GetUserQuery(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public Optional<User> byId(Long id) {
        return userRepository.findById(id);
    }
}
```

---

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

✅ Weak CoN Example:
The controller and view both use the name user, but this is low-impact to change and expected.
```php
// In a controller
public function show(User $user)
{
    return view('user.profile', ['user' => $user]);
}

// In Blade view: resources/views/user/profile.blade.php
<p>{{ $user->name }}</p>
```
❌ Strong CoN Example:
Both parts depend on specific arbitrary keys (usr, pwd). A name change breaks both sides.
```php
// In two services tightly coupled by hardcoded property names
$userData = [
    'usr' => 'john',
    'pwd' => 'secret123'
];
$authService->login($userData);

```

- **Connascence of Type (CoT)**: Components must agree on the type of data.
  - **Example**: A function expects a specific type for its argument.

✅ Weak CoT Example:
Type expectations are clear and enforced. The code remains decoupled and easily testable.
```php
public function getDiscount(float $price): float
{
    return $price * 0.9;
}
```

❌ Strong CoT Example: Caller and callee must both know exact internal structure and types. Very brittle.
```php
// Using arrays as DTOs instead of dedicated classes
public function processOrder(array $order)
{
    // expects: ['id' => int, 'price' => float, 'items' => array]
    ...
}
```

- **Connascence of Meaning (CoM)**: Components rely on an agreed-upon semantic interpretation.
  - **Example**: A specific range of values is interpreted as statuses in a shared enum.

✅ Weak CoM Example:
The semantic mapping is centralized and declared explicitly. Easy to maintain.
```php
public function getStatusColor(string $status): string
{
    return match($status) {
        'success' => 'green',
        'error' => 'red',
        default => 'gray',
    };
}
```

❌ Strong CoM Example:
Both sides must know that '1' means active. Changes to meaning require full coordination.
```php
// Using '1' and '0' to mean 'active' and 'inactive'
$isActive = $user->status === '1';
```
- **Connascence of Position (CoP)**: The position of data matters in communication between components.
  - **Example**: Positional arguments in a function call.

✅ Weak CoP Example:
The parameter order is well-known and enforced by Laravel routing; change impact is low.
```php
Route::get('/users/{id}', [UserController::class, 'show']);
```

❌ Strong CoP Example:
Caller must remember exact parameter order. Swapping just one breaks everything.
```php
public function registerUser($name, $email, $password, $role)
{
    // internal logic
}

// Called as:
registerUser('John', 'john@example.com', 'secret', 'admin');
```
---

### Dynamic Connascence
Occurs at runtime.

- **Connascence of Execution (CoE)**: Components must execute in a specific order.
  - **Example**: A database connection must be established before executing a query.

✅ Weak CoE Example:
The execution order (auth before controller) is managed by Laravel’s middleware system. It’s modular and change-tolerant.
```php
// Laravel Middleware: Ensure the user is authenticated
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index']);
});
```

❌ Strong CoE Example:
If User::find() fails (returns null), calling $user->profile will trigger an error. The second line depends on the first succeeding. Implicit dependency.
```php
$user = User::find($id);
$user->profile->load(['settings']);
```
- **Connascence of Timing (CoT)**: Components depend on specific timing or delays.
  - **Example**: Two services must synchronize their clocks.

✅ Weak CoT Example:
Even if the job dispatch is slightly delayed, the system remains consistent and functional.
```php
// Queue a job after saving a model
$user->save();
SendWelcomeEmail::dispatch($user);
```

❌ Strong CoT Example:
There’s a race condition: the cache may hold stale data unless manually synced. Timing between the cache and DB matters.
```php
// Cache expiration and DB update out of sync
Cache::put("user:{$user->id}", $user, 60);
sleep(1); // simulate some delay
$user->update(['name' => 'New Name']);
```

- **Connascence of Values (CoV)**: Components depend on exact values.
  - **Example**: Magic numbers or hard-coded configurations shared across modules.

✅ Weak CoV Example:
The feature flag is centralized in config, and changes are easy to coordinate system-wide.
```php
// Feature flags used consistently
if (config('features.new_dashboard')) {
    // load new dashboard
} else {
    // fallback
}
```

❌ Strong CoV Example: The number 3 must be known system-wide to mean “admin”. Changing its meaning requires touching all related code.
```php
// Hidden coupling via magic values
if ($user->role === 3) {
    // admin access
}
```

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
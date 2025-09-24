# Lua: A Hands-On Guide for Experienced Programmers

## 1. Getting Started
Lua is lightweight and embeddable. You can run files with:

```
lua myfile.lua
```

or open a REPL with just:

```
lua
```

---

## 2. Variables and Types
All values are dynamically typed. Only `nil` and `false` are falsy.

```
x = 10          -- number
s = "hello"     -- string
b = true        -- boolean
n = nil         -- nil

print(type(x))  --> number
```

---

## 3. Tables: The Core Data Structure
Tables are **arrays, dictionaries, objects, and records** all at once.

```
t = { "a", "b", "c" }       -- array-like
map = { name="Mario", age=34 }  -- dictionary-like

print(t[1])   --> "a"   (1-based indexing!)
print(map.name)
```

Tables are mutable and passed by reference.

---

## 4. Functions
Functions are **first-class values**.

```
function add(a, b)
  return a + b
end

-- anonymous function
mul = function(a, b) return a * b end

print(add(2,3), mul(2,3))
```

---

## 5. Control Structures
Pretty standard, but **no `++` operator**.

```
for i=1,5 do
  print(i)
end

while true do
  break
end

if x > 5 then
  print("big")
elseif x > 0 then
  print("small")
else
  print("zero or negative")
end
```

---

## 6. Multiple Return Values
A common Lua idiom.

```
function divmod(a, b)
  return math.floor(a/b), a % b
end

q, r = divmod(13, 5)
print(q, r)  --> 2, 3
```

---

## 7. Variable Scope
By default, variables are **global** unless declared `local`.

```
x = 10
local y = 20
```

---

## 8. Metatables and Operator Overloading
Metatables let you override behavior (like operator overloading).

```
vec = {x=1, y=2}
mt = {
  __add = function(a, b)
    return {x = a.x + b.x, y = a.y + b.y}
  end
}
setmetatable(vec, mt)

v2 = {x=2, y=3}
setmetatable(v2, mt)

sum = vec + v2
print(sum.x, sum.y)  --> 3, 5
```

---

## 9. Object-Oriented Style
There’s no class system, but you can emulate objects.

```
Account = { balance = 0 }

function Account:new(o)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  return o
end

function Account:deposit(amount)
  self.balance = self.balance + amount
end

a = Account:new()
a:deposit(100)
print(a.balance)  --> 100
```

---

## 10. Coroutines
Lightweight cooperative threads.

```
co = coroutine.create(function()
  for i=1,3 do
    print("co", i)
    coroutine.yield()
  end
end)

coroutine.resume(co) --> co 1
coroutine.resume(co) --> co 2
```

---

## 11. Error Handling
```
function risky()
  error("something went wrong")
end

ok, msg = pcall(risky)
print(ok, msg)
```

---

## 12. Modules (Lua 5.1–5.4 differences)
In modern Lua (5.3+), just return a table.

```
-- mymodule.lua
local M = {}

function M.greet(name)
  print("Hello, "..name)
end

return M

-- usage
local my = require("mymodule")
my.greet("Mario")
```

---

## 13. Idioms & Gotchas
- **Indexing starts at 1**, not 0.  
- Concatenate strings with `..`.  
- Use `#t` to get table length (only reliable for sequence-like tables).  
- Functions can return multiple values (used often in iterators).  

```
s = "Lua"
print("Hello " .. s)

t = {1,2,3}
print(#t)  --> 3
```

---

## 14. Quick Tips for Experienced Devs
- Think of **tables as JSON objects** with extra powers.  
- Embrace **functional style** with higher-order functions.  
- Coroutines are like **generators/fibers** in other languages.  
- Always `local` variables to avoid polluting global namespace.  

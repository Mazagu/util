# 🚀 Go (Golang) Quick Guide for Software Engineers

---

## 💻 Setup and Environment

### 1. Installation

1.  **Download and Install:** Download the latest Go distribution from the official website.
2.  **Verify:** Open your terminal and run:
    ```bash
    go version
    ```

### 2. Workspace and Modules

Go uses **Modules** for dependency management. A module is a collection of packages defined by a `go.mod` file.

1.  **Initialize a new module (in a new project directory):**
    ```bash
    mkdir myapp
    cd myapp
    go mod init [example.com/myapp](https://example.com/myapp)
    ```
    (Replace `example.com/myapp` with your module path, often a repository URL.)

2.  **Add dependencies (imports):** When you import a package not in the standard library and run `go build` or `go test`, Go will automatically add the dependency to your `go.mod` file.
    ```bash
    go mod tidy # Cleans up dependencies by removing unused ones and adding missing ones.
    ```

---

## ⚙️ Core Commands

| Command | Description |
| :--- | :--- |
| `go run <file>.go` | Compiles and runs the specified source file(s). |
| `go build` | Compiles the packages and their dependencies. Creates an executable binary in the current directory (for a `package main`). |
| `go install <package-path>` | Compiles and installs the package's binary into the `$GOPATH/bin` or `$GOBIN` directory. |
| `go test` | Runs the tests specified in files named `*_test.go`. |
| `go fmt` | Formats Go source code according to the official style guide (very opinionated, often run on save in IDEs). |
| `go get <package-path>` | Adds the package as a dependency and downloads it. |

---

## ✨ Core Concepts

### 1. Packages and Imports

* Every Go program is made up of **packages**.
* The entry point for an executable program is the `main` function in the `main` package.
* **Visibility:** A name (variable, function, struct, etc.) is **exported** (public) if it starts with a **capital letter**. Otherwise, it's unexported (private) to the package.

```go
package main // The entry point package

import (
    "fmt" // Standard library package
    "log" // Standard library package
)

// Exported function (accessible from other packages)
func Hello(name string) {
    fmt.Printf("Hello, %s\n", name)
}

// unexportedFunction (private to the 'main' package)
func unexportedFunction() {
    log.Println("This is private.")
}

func main() {
    Hello("Engineer")
}
```

### 2. Variables and Types
Go is statically typed.

```go
// Short variable declaration (Type inferred)
greeting := "Hello" 

// Explicit declaration (Use 'var' for package-level or no initial value)
var count int = 10 
var isActive bool // Defaults to false (zero value)

// Constants
const Pi = 3.14159

// Multiple declaration
var a, b int = 1, 2

// Pointers: Use '&' to get the address, '*' to dereference
x := 42
p := &x // p is a pointer to x
*p = 21 // Change x through the pointer     // Explicit type declaration
```

### 3. Concurrency (Goroutines and Channels)
Go's superpower is its simple model for concurrency.

- Goroutines: Lightweight threads managed by the Go runtime.
- Channels: Typed conduits used to communicate between Goroutines.

```go
func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // Send sum to channel c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    c := make(chan int) // Create a channel
    
    // Launch a goroutine
    go sum(s[:len(s)/2], c) 
    go sum(s[len(s)/2:], c)

    // Receive from the channel (blocking operation)
    x, y := <-c, <-c 
    
    fmt.Println(x, y, x+y) // Prints the two sub-sums and the total
}
```

### 4. Structs and Methods
Go uses structs (similar to classes/objects) and attaches methods to them. It does not have traditional classes or inheritance.

```go
// Define a Struct
type Vertex struct {
    X int
    Y int
}

// Value Receiver Method: Works on a copy of the struct
func (v Vertex) Area() int {
    return v.X * v.Y
}

// Pointer Receiver Method: Works on the original struct (can modify it)
func (v *Vertex) Scale(i int) {
    v.X = v.X * i
    v.Y = v.Y * i
}

func main() {
    v := Vertex{3, 4}
    fmt.Println(v.Area()) // 12
    v.Scale(10) // Changes the values of v.X and v.Y
    fmt.Println(v.X, v.Y) // 30 40
}
```

### 5. Error Handling
Go functions often return a value and an error. The convention is to check the error immediately. Go does not have try-catch blocks.

```go
import "errors"

func divide(a, b float64) (float64, error) {
    if b == 0 {
        // Return zero value and a new error
        return 0, errors.New("cannot divide by zero") 
    }
    // Return result and nil (no error)
    return a / b, nil 
}

func main() {
    result, err := divide(10, 0)
    
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    
    fmt.Println("Result:", result)
}
```

---

## 🧪 Testing
Go has built-in testing support via the `testing` package and the `go test` command.

### 1. Naming Convention
Test files must end with `_test.go` (e.g., `main_test.go`).

Test functions must start with the word `Test` and have the signature `func TestXxx(t *testing.T)`.

### 2. Example Test File (`math_test.go`)
Assume you have a `function Add(a, b int) int` in `math.go`.

```go
package main

import "testing"

// Basic Test
func TestAdd(t *testing.T) {
    got := Add(1, 2)
    want := 3

    if got != want {
        // t.Errorf reports an error but continues the test
        t.Errorf("Add(1, 2) = %d; want %d", got, want)
    }
}

// Table-Driven Test (Good practice for multiple scenarios)
func TestSubtract(t *testing.T) {
    tests := []struct {
        name string
        a    int
        b    int
        want int
    }{
        {"positive", 10, 5, 5},
        {"negative", 5, 10, -5},
        {"zero", 5, 5, 0},
    }
    
    for _, tt := range tests {
        // Run as a subtest for better output separation
        t.Run(tt.name, func(t *testing.T) { 
            got := Subtract(tt.a, tt.b)
            if got != tt.want {
                t.Errorf("Subtract(%d, %d) got %d, want %d", tt.a, tt.b, got, tt.want)
            }
        })
    }
}
```

### 3. Run Tests
Run all tests in the current directory:
```bash
go test
```
Run tests with verbose output:
```bash
go test -v
```
Run a specific test function (e.g., only `TestAdd`):
```bash
go test -run TestAdd
```
Run tests and check code coverage:
```bash
go test -cover
```
---
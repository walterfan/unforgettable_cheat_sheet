# Golang Cheat Sheet

<!-- TOC -->
- [Golang Cheat Sheet](#golang-cheat-sheet)
		- [**Go (Golang) Cheat Sheet** 🚀🐹](#go-golang-cheat-sheet-)
	- [**1. Installation \& Setup**](#1-installation--setup)
		- [**Install Go**](#install-go)
		- [**Initialize a New Module**](#initialize-a-new-module)
	- [**2. Basic Go Program**](#2-basic-go-program)
	- [**3. Variables \& Constants**](#3-variables--constants)
	- [**4. Data Types**](#4-data-types)
	- [**5. Control Structures**](#5-control-structures)
		- [**Conditional Statements**](#conditional-statements)
		- [**Looping (No While in Go)**](#looping-no-while-in-go)
	- [**6. Functions**](#6-functions)
		- [**Multiple Return Values**](#multiple-return-values)
	- [**7. Structs \& Methods**](#7-structs--methods)
	- [**8. Pointers**](#8-pointers)
	- [**9. Concurrency (Goroutines \& Channels)**](#9-concurrency-goroutines--channels)
		- [**Channels**](#channels)
	- [**10. Error Handling**](#10-error-handling)
	- [**11. File Handling**](#11-file-handling)
	- [**12. HTTP Server**](#12-http-server)

<!-- /TOC -->

## **1. Installation & Setup**  
### **Install Go**  
- Download from: [https://golang.org/dl/](https://golang.org/dl/)  
- Verify installation:  
  ```bash
  go version
  ```
  
### **Initialize a New Module**  
```bash
go mod init myproject
```

---

## **2. Basic Go Program**  
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```
Run the program:  
```bash
go run main.go
```
Build the program:  
```bash
go build main.go
```

---

## **3. Variables & Constants**  
```go
var name string = "Gopher" // Explicit type
age := 10                  // Implicit type
const pi = 3.14            // Constant
```

---

## **4. Data Types**  
```go
// Basic Types
var i int = 42
var f float64 = 3.14
var s string = "GoLang"
var b bool = true

// Composite Types
var arr [3]int = [3]int{1, 2, 3}
slice := []int{4, 5, 6} // Dynamic size
m := map[string]int{"apple": 5, "banana": 7} // Dictionary
```

---

## **5. Control Structures**  
### **Conditional Statements**  
```go
if x := 10; x > 5 { // Short declaration inside if
    fmt.Println("x is big")
} else {
    fmt.Println("x is small")
}
```
### **Looping (No While in Go)**  
```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

for i < 5 { // Acts like while loop
    i++
}
```

---

## **6. Functions**  
```go
func add(a int, b int) int {
    return a + b
}
```
### **Multiple Return Values**  
```go
func divide(a, b int) (int, bool) {
    if b == 0 {
        return 0, false
    }
    return a / b, true
}
```

---

## **7. Structs & Methods**  
```go
type Person struct {
    Name string
    Age  int
}

func (p Person) Greet() {
    fmt.Println("Hello,", p.Name)
}
```

---

## **8. Pointers**  
```go
func update(x *int) {
    *x = 20
}

num := 10
update(&num)
fmt.Println(num) // 20
```

---

## **9. Concurrency (Goroutines & Channels)**  
```go
func hello() {
    fmt.Println("Hello from Goroutine")
}

go hello()  // Run concurrently
time.Sleep(time.Second) // Prevent main thread from exiting early
```
### **Channels**  
```go
ch := make(chan int)
go func() { ch <- 42 }()
fmt.Println(<-ch) // Receive from channel
```

---

## **10. Error Handling**  
```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("cannot divide by zero")
    }
    return a / b, nil
}
```

---

## **11. File Handling**  
```go
content, err := os.ReadFile("file.txt")
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(content))
```

---

## **12. HTTP Server**  
```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello, Web!")
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}
```


# Go Notes

## Packages & Modules

### Go Packages:

A package is a collection of Go source files in the same directory that are compiled together. Each package has a name, usually reflected in the directory name. Packages help organize and reuse code.

**Example:**

```go
// File: mymath/maths.go
package mymath

func Add(a, b int) int {
    return a + b
}
```

### Go Modules:

A module is a collection of related Go packages that are versioned together as a single unit. Modules allow for dependency management and are defined by a `go.mod` file.

**Example:**

1.  Create a new directory and initialize a module:

    ```sh
    mkdir mymodule
    cd mymodule
    go mod init example.com/mymodule
    ```
2.  Create a package in the module:

    ```go
    // File: mymodule/mymath/maths.go
    package mymath

    func Add(a, b int) int {
        return a + b
    }
    ```
3.  Use the package in another file:

    ```go
    // File: main.go
    package main

    import (
        "fmt"
        "example.com/mymodule/mymath"
    )

    func main() {
        result := mymath.Add(5, 3)
        fmt.Println(result)
    }
    ```

In summary, packages are collections of files, while modules are collections of related packages that help manage dependencies in Go.

## Go Types & Null Values

### **Basic Types**

Go provides several built-in basic types:

* **int**: A number without decimal places (e.g., `-5`, `10`, `12`).
* **float64**: A number with decimal places (e.g., `-5.2`, `10.123`, `12.9`).
* **string**: A text value, created with double quotes or backticks (e.g., `"Hello World"`, `` `Hi everyone` ``).
* **bool**: Represents a Boolean value, either <mark style="color:green;">`true`</mark> or <mark style="color:red;">`false`</mark>.

**Noteworthy "Niche" Types**

In addition to the basic types, Go includes some niche types:

* **uint**: An unsigned integer, strictly non-negative (e.g., `0`, `10`, `255`).
* **int32**: A 32-bit signed integer, range from `-2,147,483,648` to `2,147,483,647` (e.g., `-1234567890`, `1234567890`).
* **rune**: An alias for `int32`, representing a Unicode code point (e.g., `'a'`, `'ñ'`, `'世'`).
* **uint32**: A 32-bit unsigned integer, range from `0` to `4,294,967,295`.
* **int64**: A 64-bit signed integer, range from `-9,223,372,036,854,775,808` to `9,223,372,036,854,775,807`.

(Types like `int8` and `uint8` operate similarly but feature a smaller range.)

### Null Values

Each value type in Go has a default "null value," which is assigned to a variable when no value is explicitly set:

* **int**: `0`
* **float64**: `0.0`
* **string**: `""` (an empty string)
* **bool**: `false`

**Example of Integer Default Value:**

```go
var age int = 0 // age is 0
```

### Declaring vars and const

```go
var investmentAmount float64 = 1000
expectedReturnRate := 5.5 // := is used to declare type by GO
var years float64 = 10
const inflationRate float64 = 2.5

// Same syntax as
investmentAmount, expectedReturnRate, years := 1000.0, 5.5, 10.0

// Getting values from user
fmt.Scan(&expectedReturnRate) // Get the value from user
```

#### Printf statement

```go
name := "Bob"
age := 30
fmt.Printf("%s is %d years old.\n", name, age)
// Output: Bob is 30 years old.

```

### Format text with sprintf

`sprintf` in Go is a function from the `fmt` package used to format strings without printing them to the output. Instead, it returns the formatted string for further use.

#### Usage

`sprintf` works similarly to `printf`, but returns the formatted string rather than printing it.

#### Example

1.  **Basic String Formatting:**

    ```go
    import "fmt"

    name := "Alice"
    formattedString := fmt.Sprintf("Hello, %s!", name)
    fmt.Println(formattedString)
    // Output: Hello, Alice!
    ```
2.  **Combining Different Types:**

    ```go
    age := 30
    height := 5.9
    result := fmt.Sprintf("%s is %d years old and %.1f feet tall.", name, age, height)
    fmt.Println(result)
    // Output: Alice is 30 years old and 5.9 feet tall.
    ```

`sprintf` is particularly useful when you need a formatted string for logging, concatenation, or storing in variables for later use.

## Control Structures (If else)

In Go, `if` and `else` statements are used to execute code blocks based on conditions.

#### Syntax

```go
if condition {
    // Code to execute if condition is true
} else if anotherCondition {
    // Code to execute if anotherCondition is true
} else {
    // Code to execute if all conditions are false
}
```

#### Example

```go
package main

import "fmt"

func main() {
    age := 18

    if age < 18 {
        fmt.Println("Minor")
    } else if age == 18 {
        fmt.Println("Just turned adult")
    } else {
        fmt.Println("Adult")
    }
}
```

**Key Points:**

* No parentheses are needed around conditions.
* Braces `{}` are required for code blocks.
* `else` and `else if` must be on the same line as the closing brace of the preceding `if` block.

## Errors & Read and Write Files

#### Reading and Writing a File in Go

**Writing to a File**

Here's how to write to a file with error handling:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    data := []byte("Hello, Go!\n")
    
    err := os.WriteFile("example.txt", data, 0644)
    if err != nil {
        fmt.Printf("Error writing to file: %v\n", err)
        return
    }
    fmt.Println("File written successfully.")
}
```

**Reading from a File**

Here's how to read from a file with error handling:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    content, err := os.ReadFile("example.txt")
    if err != nil {
        fmt.Printf("Error reading from file: %v\n", err)
        return
    }
    fmt.Printf("File content:\n%s", content)
}
```

#### Panic in Go

A `panic` in Go is used to signal that something has gone wrong. It stops the normal execution of the current goroutine.

**Example of Panic**

```go
package main

import (
    "fmt"
)

func main() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
        }
    }()

    panic("This is a panic message!")
    fmt.Println("This line will not be executed.")
}
```

#### Summary

* Use `os.WriteFile` for writing to a file and handle errors.
* Use `os.ReadFile` for reading a file and handle errors.
* Use `panic` to indicate a serious error, with `recover` to handle it gracefully.

## Import Export from Packages

In Go, you can import and export functionality from different packages using the package system.

### Example Structure

Assume we have the following directory structure:&#x20;

```
/myproject
    ├── main.go
    └── mymath
        ├── add.go
        └── subtract.go
```

#### <mark style="color:orange;">Note:</mark> <mark style="color:red;">Package other than main should be in its own folder</mark>

### Creating a Package

**File: mymath/add.go**

```go
package mymath

// Add returns the sum of two integers.
func Add(a, b int) int {
    return a + b
}
```

**File: mymath/subtract.go**

```go
package mymath

// Subtract returns the difference of two integers.
func Subtract(a, b int) int {
    return a - b
}
```

### Importing the Package

**File: main.go**

```go
package main

import (
    "fmt"
    "myproject/mymath" // Importing the mymath package
)

func main() {
    sum := mymath.Add(5, 3)
    diff := mymath.Subtract(5, 3)
   
    fmt.Println("Sum:", sum)        // Output: Sum: 8
    fmt.Println("Difference:", diff) // Output: Difference: 2
}
```

#### Key Points

* You define a package using the `package` keyword.
* Functions or variables are <mark style="color:yellow;">exported</mark> when their names <mark style="color:yellow;">start</mark> with an <mark style="color:green;">uppercase letter</mark>.
* You import a package using the `import` statement, specifying the path to the package.
* Use the package name as a prefix when calling functions from that package.

## Pointers

### Understanding Pointers in Go

**Definition**: A pointer is a variable that holds the memory address of another variable. Pointers are useful for modifying values in place without creating copies.

#### Example Explained

```go
const adultAge = 18

func main () {

	var age = 32
	var modifiedAge = age

	fmt.Println("Current age is: ", age) // 32

	calculateAdultAge(&modifiedAge)

	fmt.Println("Adult age is: ", age) // 32
	fmt.Println("Modified Adult age is: ", modifiedAge) // 14
}

func calculateAdultAge(age *int){
	*age = *age - adultAge
}
```

#### Key Points

1. **Declaration**:
   * `var age = 32`: Creates an integer variable `age`.
   * `age *int`: Indicates that the parameter `age` in `calculateAdultAge` is a pointer to an integer.
2. **Address-of Operator (`&`)**:
   * `&modifiedage`: Passes the memory address of `modifiedage` to the `calculateAdultAge` function. This allows the function to access and modify the original variable.
3. **Dereferencing Operator (`*`)**:
   * `*age`: Dereferences the pointer, allowing access to the value stored at that memory address. Here, it modifies the original `age` by subtracting `adultAge`.

#### Benefits of Pointers

* **Efficiency**: Using pointers avoids copying large structures, saving memory and processing time.
* **Mutability**: Functions can modify the caller's variables directly via pointers, which is particularly useful for large data structures or when multiple values need to be modified.

#### Summary

Pointers allow for efficient and direct manipulation of variables in Go. By passing a pointer to a function, you can change the value of the original variable, as demonstrated in this example.

## Structs in Go

**What Are Structs?**

Think of **structs** as blueprints for creating complex data types in Go! They allow you to group related data together in a neat package, making it easier to manage and operate on.

**Key Features**

* **Fields**: Structs can have multiple fields that can be of different types. Each field represents a piece of data related to that struct.
* **Custom Types**: You can create your own data structures tailored to your needs.
* **Value Types**: When you work with structs, you’re dealing with value types. This means that copying one struct to another will provide a separate copy.

**How to Define a Struct**

You define a struct using the `type` keyword followed by the struct name and the fields inside curly braces:

```go
type StructName struct {
    Field1 Type1
    Field2 Type2
    // Add more fields as needed
}
```

**Let's See an Example!**

```go
package main

import "fmt"

// Defining a struct called Person
type Person struct {
    Name string // A field for the person's name
    Age  int    // A field for the person's age
}

// Method to greet using a pointer receiver
func (p *Person) Greet() {
    fmt.Printf("Hello, my name is %s and I am %d years old!\n", p.Name, p.Age)
}

func main() {
    // Creating an instance of the Person struct
    alice := Person{Name: "Alice", Age: 30}
    alice.Greet()
    
    // Accessing and printing the fields
    fmt.Println("Name:", alice.Name) // Output: Name: Alice
    fmt.Println("Age:", alice.Age)   // Output: Age: 30

    // Modifying a field
    alice.Age = 31
    fmt.Println("Updated Age:", alice.Age) // Output: Updated Age: 31
}
```

**Key Points to Remember**

* **Creating Instances**: You can use composite literals to create instances quickly (e.g., `Person{Name: "Alice", Age: 30}`).
* **Field Access**: Use the dot (`.`) notation to get or set field values easily.
* **Zero Values**: If you create a struct without initializing its fields, they'll automatically be set to default values (like `""` for strings and `0` for integers).
* **Pointer Receiver**: The methods `Greet` and `HaveBirthday` are defined with a pointer receiver (`*Person`). This means they can modify the `Person` struct's fields directly without making a copy.

**Adding Behavior with Methods**

You can also give your structs some personality by associating methods with them:

```go
func (p Person) Greet() {
    fmt.Printf("Hello, my name is %s!\n", p.Name)
}
```

**Why Use Structs?**

* **Organization**: Grouping related data together keeps your code clean and organized.
* **Simplicity**: Structs let you create complex data types without the overhead of inheritance.
* **Composition**: Instead of inheriting, Go encourages you to combine structs, making your code flexible and easy to maintain.

### Struct Embedding&#x20;

**Struct embedding** in Go is a powerful feature that allows you to include one struct within another. This provides a form of composition, enabling code reuse and a way to extend the functionality of structs without inheritance.

#### Key Benefits

* **Code Reuse**: Easily reuse fields and methods from one struct in another.
* **Simplicity**: Simplifies access to embedded fields and methods as if they are part of the containing struct.

#### Usage Example

Let’s illustrate struct embedding with an example:

**Base Struct**

```go
type Address struct {
    City    string
    ZipCode string
}
```

**Embedded Struct**

```go
type Person struct {
    Name string
    Age  int
    Address // Embedding Address struct
}
```

**Using Embedded Struct**

```go
package main

import "fmt"

func main() {
    // Create an instance of Person
    person := Person{
        Name: "Alice",
        Age:  30,
        Address: Address{
            City:    "Wonderland",
            ZipCode: "12345",
        },
    }

    // Access fields from the embedded Address struct directly
    fmt.Printf("Name: %s, Age: %d, City: %s, ZipCode: %s\n",
        person.Name, person.Age, person.City, person.ZipCode)
    // Output: Name: Alice, Age: 30, City: Wonderland, ZipCode: 12345
}
```

### Interfaces

#### Easy Explanation of Interfaces in Go

**What is an Interface?**

An interface in Go is like a contract that defines certain behaviors without specifying how those behaviors are implemented. It allows different types to be treated the same way if they fulfill the requirements of the interface, making code more flexible and reusable.

#### Why Use Interfaces?

* **Flexibility**: You can substitute different types as long as they satisfy the interface.
* **Decoupling**: It allows you to change implementations without changing the code that uses the interface.

#### How to Define and Use an Interface

**Step 1: Define an Interface**

Let’s create an interface called `Greeter` that requires a method `Greet()`.

```go
type Greeter interface {
    Greet() string
}
```

**Step 2: Create Types That Implement the Interface**

Next, we’ll create different types that implement the `Greeter` interface.

**Example Types:**

```go
type Dog struct {}
type Cat struct {}

// Dog's implementation of the Greet method
func (d Dog) Greet() string {
    return "Woof! I'm a dog."
}

// Cat's implementation of the Greet method
func (c Cat) Greet() string {
    return "Meow! I'm a cat."
}
```

**Step 3: Use the Interface**

Now, we can write a function that accepts any type that implements the `Greeter` interface.

```go
func greetAnimal(g Greeter) {
    fmt.Println(g.Greet())
}

func main() {
    myDog := Dog{}
    myCat := Cat{}

    greetAnimal(myDog) // Output: Woof! I'm a dog.
    greetAnimal(myCat) // Output: Meow! I'm a cat.
}
```

#### Summary of the Example

1. **Interface Definition**: We defined an interface `Greeter` that requires a `Greet()` method.
2. **Implementing Types**: We created two types, `Dog` and `Cat`, each implementing their version of the `Greet` method.
3. **Using the Interface**: The `greetAnimal` function can accept any type that satisfies the `Greeter` interface, allowing us to call `Greet()` on `myDog` and `myCat`, demonstrating polymorphism.

## Arrays: Append and Cap

**Introduction to Arrays**

In Go, an **array** is a fixed-size sequence of elements of a specific type. Once you declare an array, its size cannot be changed. Arrays are useful for situations where you know the exact number of elements in advance.

**Example of an Array Declaration**:

```go
var fruits [3]string // An array of strings with size 3
fruits[0] = "Apple"
fruits[1] = "Banana"
fruits[2] = "Cherry"
```

#### Working with Slices

While arrays have a fixed size, **slices** are more commonly used in Go as they offer flexibility. A slice is a reference type that points to an array, and you can dynamically resize it using the built-in `append` function.

**Using `append`**

The `append` function adds elements to a slice. If the slice has enough capacity, it simply adds the element; otherwise, it creates a new underlying array with a larger capacity.

**Example of Appending Elements**:

```go
package main

import (
    "fmt"
)

func main() {
    // Starting with a slice
    numbers := []int{1, 2, 3}
    
    // Appending to the slice
    numbers = append(numbers, 4) // Append a single element
    numbers = append(numbers, 5, 6, 7) // Append multiple elements

    fmt.Println("Numbers:", numbers)
}
```

**Output**:

```
Numbers: [1 2 3 4 5 6 7]
```

#### Using `cap`

The `cap` function returns the capacity of a slice, which is the number of elements the slice can hold without reallocating.

**Example of Capacity**:

```go
package main

import (
    "fmt"
)

func main() {
    // Declare a slice with an initial capacity
    nums := make([]int, 0, 5) // Length 0, Capacity 5

    fmt.Println("Capacity before appending:", cap(nums)) // Output: 5

    // Appending elements
    nums = append(nums, 10, 20, 30)
    fmt.Println("Capacity after appending:", cap(nums)) // Output: 5

    // Appending more elements
    nums = append(nums, 40, 50, 60)
    fmt.Println("Capacity after more appending:", cap(nums)) // Output: 10 (slice expanded)

    fmt.Println("Final Slice:", nums) // Output: [10 20 30 40 50 60]
}
```

#### Key Points

* **Arrays**: Fixed size and type; less commonly used for dynamic data.
* **Slices**: More flexible; dynamic size and can be resized using `append`.
* **`append` Function**: Used to add new elements to a slice. It may change the underlying array if the capacity is exceeded.
* **`cap` Function**: Returns the capacity of a slice, indicating how many elements it can hold before needing to allocate a new underlying array.

## Maps

#### Maps in Go

**Introduction**\
A **map** in Go is an unordered collection of key-value pairs. Maps are dynamic and provide a way to associate values with unique keys, making data retrieval efficient.

#### Key Characteristics of Maps

* **Unordered**: The order of elements in a map is not guaranteed.
* **Dynamic Size**: Maps can grow and shrink in size as needed.
* **Key-Value Pairs**: Each key is unique within a map and is used to access the corresponding value.
* **Reference Type**: Maps are reference types, similar to slices.

#### Declaring and Initializing Maps

**Declaration**:\
You can declare a map using the `make` function or a map literal.

**Example of Declaring a Map**:

```go
// Using make
ages := make(map[string]int) // A map with string keys and int values

// Using a map literal
grades := map[string]string{
    "Alice": "A",
    "Bob":   "B",
}
```

#### Example of Using Maps

Here's how to add, access, and delete elements from a map:

```go
package main

import "fmt"

func main() {
    // Declare and initialize a map
    inventory := map[string]int{}

    // Adding items
    inventory["apples"] = 10
    inventory["bananas"] = 20
    inventory["oranges"] = 15

    // Accessing values
    fmt.Println("Apples:", inventory["apples"]) // Output: Apples: 10

    // Updating a value
    inventory["bananas"] = 25
    fmt.Println("Updated Bananas:", inventory["bananas"]) // Output: Updated Bananas: 25

    // Deleting a key
    delete(inventory, "oranges")
    fmt.Println("After deleting Oranges:", inventory) // Output: After deleting Oranges: map[apples:10 bananas:25]

    // Check if a key exists
    value, exists := inventory["grapes"]
    if exists {
        fmt.Println("Grapes:", value)
    } else {
        fmt.Println("Grapes do not exist") // Output: Grapes do not exist
    }
}
```

#### Key Operations on Maps

1. **Adding/Updating Elements**: Use the syntax `map[key] = value` to add or update.
2. **Retrieving Values**: Access values using `map[key]`.
3. **Deleting Elements**: Use the `delete(map, key)` function to remove a key-value pair.
4. **Checking Existence**: Use the comma-ok idiom to check if a key exists in the map.

#### Key Points

* Maps use hash tables and provide average-case constant time complexity for lookups, inserts, and deletions.
* It’s vital to ensure keys are of a type that is comparable (e.g., strings, integers).
* Maps are nil when declared but uninitialized (i.e., `nil` maps cannot be used to store values; you must initialize them with `make` or a map literal).

#### Summary

Maps in Go are powerful data structures for managing collections of key-value pairs. They offer efficient access and modifications, making them ideal for scenarios like counting occurrences or grouping data. Use maps when you need to associate specific keys with values and require quick lookups.

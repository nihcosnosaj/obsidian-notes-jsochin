go version go1.24.2 darwin/arm64

Define GOPATH and put $GOPATH/bin in your executable path.

For bash, find your .bash_profile and add the following to it:
```
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

and then source that .bash_profile file to apply the changes.


`go run` --> just run the .go file, temporarily builds the binary, runs it, and deletes it. Useful for running go code like a script and for testing.
`go build` --> build the binary for later uses. most common.

Each of the above commands takes either a single Go file, a list of Go files, or the name of a package. 


`go install`  --> takes an argument, the location of the source code repository you want to install, followed by an @ and the version of the tool you want (use @latest for the latest version)

`go fmt` --> automatically reformst your code to match the standard format.

---------

## Packages for import
- image
- fmt
	- fmt.Println
	- fmt.Sprintf
	- fmt.Printf
- strings
	- strings.NewReader
- io
	- io.Reader
	- io.Writer
- net
	- net.LookupIP
	- net.LookupHost
- 



Comments 
-- `/* */` block comments or `//` line comments

Printing the type:
- a formatted string, use %T and then plug the variable into the corresponding formatting slot. 
### Packages
- Every go program is made up of packages
- Programs start in package `main`
- By convention, the package name is the same as the last element of the import path
	- For instance, `math/rand` package comprises files that begin with `package rand`

### Imports

### Exported Names
- A name is exported if it begins with a capital letter. 
- Pizza is an exported name, as is Pi, which is exported from the math package.

### Functions
- A function can take zero or more arguments.
- In the following example, `add` takes two parameters of type `int` 
 ```
 package main
import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

NOTE: the type comes AFTER the variable name in function declarations! 

- A function can return any number of results. 

```
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

NOTE: `:=` is used for short variable delcaration. it allows you to declare and initialize variables in one step without explicitly specifying their type. 


### Named Return values
- Go's return values may be named. If so, they are treated as variables defined at the top of the function. 
- These names should be used to document the meaning of the return values.
- A `return` statement without arguments returns the named return values. this is known as a "naked" return. 
- Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions. 


### Variables
- The `var` statement declares a list of variables; as in function argument lists, the type is last!
- A `var` statement can be at package OR function level.

```
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

 - A `var` declaration can include initializers, one per variable. If an initializer is present, the type can be omitted; the variable will take the type of the initializer. 
```
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}

```


### Short Variable Declarations
- Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type. 
Outside a function, every statement begins with a keyword (`var`, `func`, and so on) and so the `:=` construct is NOT available outside of a function. 

## Go's Basic Types
- Go's basic types are:
	- `bool`
	- `string`
	- `int int8 int16 int32 int64`
	- `uint uint8 uint16 uint32 uint 64 uintptr`
	- `byte // alias for uint8`
	- `rune // alias for int32, represents a unicode point`
	- `float32 float 64`
	- `complex64 complex128`

- Variable delcaration can also be "factored" into blocks, as with import statements
- The `int`, `uint`, and `uintptr` types are usually 32bits wide on 32-bit systems, and 64-bits wide on 64-bit systems. When you need an integer value you should use `int` unless you have a specifc need to use sized or unsigned integer type. 

### Zero Values
- Variables can be declared without an explicit initial value. When they are, their value is zero.

### Type Conversions
- The expression `T(v)` converts the value `v` to the type `T`
- Some numeric conversions:
```
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

- In Go, assignemtn between items of different type requires an explicit conversion. Try removing the float64 or uint conversion and see what happens. 

### Type Inference
- When declaring a variable without specifying an explicit type (either by using the := syntax or var = expression syntax), the variables type is inferred from the value on the right hand side.
- `var i int`
- `j := i // j is an int`
- But when the right hand side contains an untyped numeric constant, the new variable may be an `int` `float64` or `complex128` depending on the precision of the constant:
	- `i := 42 // int`
	- `f := 3.142 // float64`
	- `g := 0.867 + 0.5i //complex128`

### Constants
Constants are declared like variables, but with the `const` keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the := syntax.

```
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

### Numeric Constants
- Numeric constants are HIGH PRECISION VALUES!
- An untyped constant takes the type needed by its context.


### For
- Go has only one looping construct, the `for` loop.
- The basic `for` loop has three components separated by semicolons:
	- the init statement: executed before the first iteration (OPTIONAL)
	- the condition expression: evaluated before every iteration
	- the post statement: executed at the end of every iteration (OPTIONAL)
- The init statement will often be short variable delcaration, and the variables declared there are visible ONLY in the scope of the for statement. 
- The loop will stop iterating once the boolean conditions evalute to False.
#### For is Go's "While"

```
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}

```

If you omit the loop condition, it loops forever. 
```
package main

func main() {
	for {
	}
}

```

NOTE: You can do `for {` and then it starts an infinite loop until you break it in the loop depending on a certain condition.

### If
- Go's `if` statements are like its `for` loops; the expression need not be surrounded by parentheses () but the braces are required
```
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}
```

### If with a short statement
- Like `for`, the `if` statement can start with a short statement to execute before the condition.
- Variables delcared here are also only in the scope of the if block.

#### If and else
- Variables declared inside an `if` short statement are also available inside any of the `else` blocks.

### Switch 
- A `switch` is a shorter way to write an `if-else` statement. 
	- It runs the first case whose value is equal to the condition expression. 
- Go only runs the selected case, not all the cases that follow. `break` is provided automatically in Go
- They evaulatue from top to bottom, stopping when a case succeeds. 
```

switch <case_to_match>:
case <test case 1>:
	<do action>
case <test case 2>:
	<do action>
default:
	<do action if no other case matches>
```


- A `switch` with no condition is the same thing as `switch true` 
```
switch {
case t.Hour() < 12:
	fmt.Println("Good morning!")
case t.Hour() < 17:
	fmt.Println("Good afternoon!")
default:
	fmt.Println("Good evening!")
}

```

#### Defer
- A `defer` statement defers the execution of a function until the surrounding function returns. 
- The deferred calls arguments are evaluated immediately, but the function call is not executed until the surrounding function returns. 
```
func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
```

- Deferred function calls are pushed onto a stack. When a function returns, the deferred calls are executed in a last-in-first-out order.

```
func main()

```


### Pointers
- Go has pointers. 
- Pointers hold the memory address of a value. 
- The type `*T` is a pointer to a `T` value. Its zero value is `nil`

`var p *int`

The `&` operator generates a pointer to its operand. 

```
i := 42
p = &i
```

The `*` operator denotes the pointer's underlying value.
```
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

This is known as "dereferencing" or "indirecting"

Unlike C, Go has no pointer arithmetic. 


### Struct
- A `struct` is a collection of fields
```
type Vertex struct {
	X int
	Y int
}

fmt.Println(Vertex{1, 2})
```

- Struct fields are accessed using a dot `.` 

```
v := Vertex{1, 2}
v.X = 4
```

- Struct fields can be accessed through a struct pointer.
- To access the field `x` of a struct when we have the struct pointer `p` , we can write `(*p).X` However, that notation is cumbersome. The language lets us actually just write `p.X` without the explicit dereference.

#### Struct Literals
- A struct literal denotes a newly allocated struct value by listing the values of its fields. 
- Basically, you are defining the values as well as the struct in the same statement
```
type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2} // has type Vertex
	v2 = Vertex{X: 1} // Y:0 is implicit
	v3 = Vertex{} // X:0 and Y:0
	p = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, p, v2, v3)
}

```

### Arrays
- The type `[n]T` is an array of `n` values of type `T`
- The expression: `var a [10]int` declares a variable `a` as an array of ten integers.
- An arrays length is part of its type, so arrays CANNOT be resized. This seem limiting, but don't worry, Go provides a way of working with arrays that is convenient.
- Arrays are not common, as they are TOO RIGID!
- You cannot read or write past the end of an array or use a negative index. 
- You can't use arrays unless you know the exact size of the dataset. 
- Arrays provide the backing for `slices` which are one of the most useful go features.

### Slices
- An array has a fixed size. 
- A slice, on the other hand, is a dynamically-sized, flexible view into the elements of an array. 
- When you want a data structure that holds a sequence of values, a slice is what you should use. 
- The length is not part of the type of a slice, which is the major limitation of arrays.
- Slices are MUCH MORE COMMON than arrays in everyday Go
- `[]T` is a slice with elements of type `T`
- A slice is formed by specifying two indices, a low and high bound, separated by a colon. 
- `a[low : high]`
- This selects a half-open range which includes the first element, but excludes the last one.
- This: `a[1:4]` creates a slice which includes elements 1 through 3 of a. 
- `var x = []int{10, 20 ,30}` 
- Using `[...]` makes an array, using `[]` makes a slice!

##### Slices are like references to arrays
- A slice does not store any data, it just describes a section of an underlying array.
- Changing the elements of a slice modifies the corresponding elements of its underlying array. 
- Other slices that share the same underlying array will see those changes as well.

#### Slice Literals
- A slice literal is like an array literal without the length.
- This is an array literal:
	- `[3]bool{true, true, false}` 
- This creates creates the same array as above, and then builds a slice that references it:
	- `[]bool{true, true, false}` 
- You can create a slice of structs as well like so:
```
s := []struct {
	i int
	b bool
}{
	{2, true},
	{3, false},
	{5, true},
	{7, true},
	{11, false},
	{13, true},
}
fmt.Println(s)
```


#### Slice Defaults
- When slicing, you may omit the high or low bounds to use their defaults instead.
- The default is zero for the low bound and the length of the slice for the high bound.
- `var a [10]int` 
	- These slice expressions are equivalent:
		- `a[0:10]`
		- `a[:10]`
		- `a[0:]`
		- `a[:]`

#### Slice length and capacity
- A slice has both:
	- length --> the number of elements it contains
	- capacity --> the number of elements in the underlying array, counting from the first element in the slice.
	- can be obtained with `len(s)` and `cap(s)` 

Slices have a zero value of `nil` --> this means len and cap of 0 as well as no underlying array.

You can also use append to grow a slice:
	`var x []int`
	`x = append(x, 10)`
	and this will add 10 to the slice `x` 

```
The Go Runtime
---------------
The go runtime provides memory allocation and garbage collection, concurrency support, networking, and implementations of built-in types and functions. 

The go runtime is compiled into every go binary. Including the runtime in the binary makes it easier to distribute Go programs and avoids worries about compatibility issues between the runtime and the program.
```

### Make
- Slices can be created with the built-in `make` function; this is how you create dynamically sized arrays. 
- Make allos us to create an empty slice that already has a length or capacity specified. 
- The `make` function allocates a zeroed array and returns a slice that refers to that array. 
`a := make([]int, 5)  // len(a)=5`
To specify a capacity, pass a third argument

##### Slices of slices
- Slices can contain any type, including other slices.
- 


#### Range
The `range` form of the `for` loop iterates over a slice or map
- When ranging over a slice, two values are returned for each iteration: the index, and the copy of the element at that index.

```[[
]]var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}

```

- You can skip the index or value by assigning to `_`:
```

for i, _ := range pow
for _, value := range pow
```

- If you only want the index, you can omit second variable.

the `<<` operator --> left shift operator; shifts the bits of an integer to the left by a specified number of positions, effectively multiplying the number by 2 raised to the power of the shift count. 

### Maps
- A map maps keys to values, like a dictionary in Python.
	- The zero value of a map is `nil` . A `nil` map has no keys, nor can keys be added.
- The `make` function returns a map of the given type, initialized and ready for use.
```
type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}

```
- Map literals are like struct literals, but the keys are required. 
- You can omit the top-level name if it is just a type name. 

#### Mutating Maps
- Insert or update an element in map `m` :
	- `m[key] = elem`
- Retrieve an element:
	- `elem = m[key]`
- Delete an element:
	- `delete(m, key)` 
- Test that a key is present with a two-value assignment:
	- `elem, ok = m[key]`
- If `key` is present in `m`, `ok` is `true`. 

#### Function values
- Functions are values too. They can be passed around just like other values.
- Function values may be used as function arguments and return values.

### Function closures
- Go functions may be closures.
- A closure is a function value that references variables from outside its body. 
- The function may access and assign to the referenced variables; in this sense the function is "bound" to the variables. 


### Methods
- Go doesn't have classes. However, you can define methods on types.
- A method is a function with a special receiver argument.
- The receiver appears in its own argument list between the `func` keyword and the method name. 
- A method declaration:
```
type Vertex struct {
	X, Y float64
}

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() {
	v := Vertex{3, 4}
	fmt.Println(v.Abs())
}

```


NOTE: 
`func (p Person) String() string {`
- The (p Person) is called a receiver.
- It allows the method to be associated with the Person type, enabling you to call the method on instances of Person
#### Methods are functions
- Remember: a method is just a function with a receiver argument.
- You can just shift the argument declaration with name and type to its own parentheses outside of the function name and then you can call it with `v.Method()`

- You can declare a method on a non-struct type, too
- Also, you can declare a method with a pointer reciever. 
```
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
} // note, you don't need to return here because its updating the values of v.
```


### Interfaces
- An `interface` type is defined as a set of method signatures.
	- just a set of method signatures. 
	- Can be viewed as a CONTRACT that a type may satisfy.
```
type Shape interface {
	Area() float64
}
```
- A value of interface type can hold any value that implements those methods. 
- Under the hood, an interface value can be thought of as tuple of a value and a concrete type:
	- (value, type)
- An interface value holds a value of a specific underlying concrete type.
- Calling a method on an interface value executes the method of the same name on its underlying type. 

So lets say we have this:
```Go

package main 

import (
	"math"
)


// This basically says that anything, any type, that has an area() method
// is of type shape.
// It implements the interface of shape
// so rect implements the interface of shape since it has an area() method.
// if a 
type shape interface {
	area() float64
}

type circle struct {
	radius float64
}

type rect struct {
	width float64
	height float64
}

func (r rect) area() float64 {
	return r.width * r.height
}

func (c circle) area() float64 {
	return math.Pi * c.radius * c.radius
}


```

A type is said to IMPLEMENT an interface when it has all the methods defined in the interface.
SO, both types rect and circle are implementing the interface of type Shape i.e. the same methods that are defined in the Shape interface can be done on the 


- #### Interface Values with nil underlying values
	- If the concrete value insde the interface itself is nil, the method will be called with a nil reciever. 
	- This would trigger a null pointer execption in most languages, but Go can gracefully handle it. 


- a type **implements** an interface by implementing its methods. There is no explicit declaration or implements keyword.
- ESSENTIALLY, I can create an interface like so:
```
type I interface {
	M()
}
```

and that method inside of it, M() is now a method of that interface that can be called when I instantiate an instance of that interface. 
Lets look at M()
```
func (f F) M() {
	fmt.Println(f)
}
```

and then I can declare a variable `i` with type of that interface `I`:
`var i I` 
and now I can call the method M() with this syntax:
`i.M()`


##### The empty interface
- The interface type that specifies zero methods is known as the empty interface.
	- `interface{}`
- An empty interface may hold values of any type. 

#### Type assertions
- A type assertion provides access to an interface value's underlying concrete value.
- `t := i.(T)`
- This statement asserts that the interface value `i` holds the concrete type `T` and assigns the underlying `T` value to the variable `t` 
- If `i` does not hold a `T`, the statement will trigger a panic.
- To test whether an interface value holds a specific type, a type assertion can return two values: the UNDERLYING VALUE and a BOOLEAN VALUE that reports whether the assertion succeeded.
	- `t, ok := i.(T)`
- If `i` holds a `T` , then `t` will be the underlying value and `ok` will be true. 
- If not, `ok` will be false and `t` will be the zero value of type `T` and no panic occurs. 

#### Type Switches
- A type switch is a construct that permits several type assertions in series.
- A type switch is like a regular switch statement, but the cases in a type switch specify types (not values), and those values are compared against the type of the value held by the given interface value. 
```
func do(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Printf("Twice is %v is %v\n", v, v*2)
	case string:
		fmt.Printf("%q is %v bytes long\n", v, len(v))
	default:
		fmt.Printf("I don't know about type %T!\n", v)
	}
}



```

```
switch v := i.(type) {
case T:
	// here v has type T
case S:
	// here v has type S
default:
	// no match; here v has the same type as i
}
```

##### Stringers
- one of the most ubiquitous interfaces --> `Stringer` defined by the `fmt` package
```
type Stringer interface {
	String() string
}
```

A `Stringer` is a type that can describe itself as a string. 


NOTE: `fmt.Sprintf` formats the input and returns it as a string. DOES NOT PRINT TO CONSOLE. Useful for when you need to create a formatted string for further use (writing to file or storing in variable)

NOTE: You can use `%d` in formatted strings to convert bytes to decimal representations. 



## Errors
- Go programs express error state with `error` values
- The `error` type is a built-in interface simliar to `fmt.Stringer`
```
type error interface {
	Error() string
}
```

- Functions often return an error value, and calling code should handle errors by testing whether the error equals `nil` 
```
i, err := strconv.Atoi("42)
if err != nil {
	fmt.Printf("Couldn't convert number: %v\n", err)
	return
}
fmt.Println("Converted Integer:", i)
```

- A `nil` error denotes success! A non-nill error denotes failure

#### Panic and Recover
- Go generates a panic whenever there is a situation where the Go runtime is unable to figure out what should happen next. This could be a programming error (trying to read past the end of a slice) or environmental problem (no more memory)
- As soon as a panic happens, the current function EXITS and anydefers attached start running.
- You can create your own panics with `panic(msg)`
- It prints out a msg followed by a stack trace.
- Go provides a way to capture a panic to provide a more graceful shutdown or to prevent shutdown at all
- `recover` is called from within a defer to check if a panic happened
- We want to reserve panics for fatal situations and use recover to gracefully handle these fatalities. 
- We don't rely on `panic` and `recover` because `recover` doesn't make it clear what could fail, it just ensures that IF something fails, we can print out a message and continue. 


## Readers
- The `io` package specifies the `io.Reader` interface, which represents the read end of a stream of data. 
- The `io.Reader` interface has a `Read` method:
	- `func (T) Read(b []byte) (n int, err error)`
	- `Read` populates the given byte slice with data and returns the number of bytes populated and an error value. It returns an `io.EOF` error when the stream ends. 

- A common pattern is an `io.Reader` that wraps another `io.Reader`, modifying the stream in some way. 

### Images
- Package image defines the Image interface
```
package image

type Image interface {
	ColorModel() color.Model
	Bounds() Rectangle
	At(x, yint) color.Color
}
```


### Type Parameters
- Go functions can be written to work on multiple types using type parameters.
- The type parameters of a function appear between brackets, before the function's arguments
- `func FuncName[T comparable](s []T, x T) int`
	- This means that s is a slice of any type T that fulfills the built-in constraint `comparable` 
	- `comparable` is a useful constraint that makes it possible to use the == and != operators on values of this type

### Generic types
- In addition to generic functions, Go also supports generic types. 
- A type can be parameterized with a type parameter, which could be useful for implementing generic data structures.


### Goroutines
- A goroutine is a lightweight thread managed by the Go runtime. 
- `go f(x, y, z)`
	- Starts a new goroutine running `f(x, y, z)`
- The execution of f, x, y, and z happens in the current goroutine and the execution of f happens in the new go routine
- Goroutines run in the same address space, so access to shared memory must be synchronized. the `sync` package provides useful primitives, although you won't need them much in Go as there are other prmitivves.

### Channels
- Channels are a typed conduit through which you can send and recieve values with the channel operator, `<-` 
```
ch <- v // send v to channel ch
v := <-ch // Recieve from ch, and assign value to v
```

- Data flows in the direction of the arrow
- Like maps and slices, channels must be created before use:
- `ch := make(chan int)`
- By default, sends and recieves block until the other side is ready. This allows goroutines to synchornize without explicit locks or condition variables.

#### Buffered Channels
- Channels can be buffered. 
- Provide the buffer length as the second argument to `make` to initalized a buffered channels.
- `ch := make(chan int, 100)`
- Sends to a buffered channel block only when the buffer is full. recieves block when the buffer is empty.

#### Range and Close
- A sender can `close` a channel to indicate that no more values will be sent. 
- Recievers can test whether a channel has been closed by assigning a second parameter to the recieve expression
- `v, ok := <-ch`
	- after ok is false, there are no more values to recieve and the channel is closed.
- the loop: `for i := range c` recieves values from the channel repeatedly until its closed.
- NOTE: only the sender should close a channel, never the reciever. Sending on a closed channel will cause a panic.

#### Select
- the `select` statement lets a goroutine wait on multiple communication operations. 

#### Default Selection
- The default case in a select is run if no other case is ready.



### Exercise: Equivalent Binary Trees
- Binary Tree
	- A binary tree is a hierarchical data structure in which each node has AT MOST two children, the left child and right child. 
	- Nodes
		- Each node contains a value (data) and pointers to its left and right children.
	- Root
		- The topmost node in the tree is called the root
	- Leaves
		- Nodes with no children are called leaf nodes
	- Height
		- The height of a binary tree is the number of edges on the longest path from the root to a leaf. 

```
type Tree struct {
	Left *Tree // pointers to a tree struct
	Value int
	Right *Tree // pointers to a tree struct
}
```

NOTE:
- & (address-of operator)
	- used to get the memory address of a variable
	- creates a pointer to the variable
	- x := 42
	- `p := &x`
	- valueAtX42 := *p
- * (dereference operator)
	- used to dereference a pointer
	- this means it access the value stored at the memory address the pointer refers to.
	- can also be used in a type declaration to indicate that a variable is a pointer to a specific type. 
when i declare a variable 



### Naming Conventions
- Packages --> should be lower-case, single-word names; no need for underscores or mixedCaps
- Getters --> neither idiomati nor necessary to put `Get` into the getter's name. If you have a field called `owner` (lowercase, unexported) , the getter method should be called Owner (uppercase, exported), not `GetOwner` 
	- The use of uppercasenames for export provides the hoook to discriminate the field from the method. 
	- A Setter function will likely be called SetOwner
- Interfaces --> By convention, interface names are named by the method name plus an -er suffix or simliar modification to construct agent noun
- MixedCaps or mixedCaps rather than underscores to write mulitword names

## Cobra and Go CLI Tools

Using confi



Quick HTTP server

```Go
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hi there, I love %s!", r.URL.Path[1:])
}

func main() {
	http.HandleFunc("/", handler)
	fmt.Println("server listening on port 8080...")
	http.ListenAndServe(":8080", nil)
}

```


### Go's Concurrency model
- Do multiple things at the same time. 
- Go follows a model proposed by Tony Hoare `Communicating Sequential Processes` (CSP)
- Two concepts make Go's concurrency model work:
	- `Goroutines` --> a function that runs independently of the function that started it. Sometimes explained as a function that runs as though it were on its own thread.
	- `channels` --> pipelines for sending and recieving data. A socket that runs inside your program and accepts and/or sends signals.
		- Provide a way for goroutines to send structured data to another
		- Main data stream for allowing goroutines to communicate with each other
- Technically, any functon called after the keyword `go` is a goroutine
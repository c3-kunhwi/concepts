# Golang Tips 
### About Go 
---
##### Overview 
```
1) Strong / Static Typing
2) Compiled / Object Oriented Language 
3) Concurrency (built for multicores) + Compilation Efficient 
4) Pass by Value, allows for Pass by Pointer 
5) Automatic Garbage Collection 

* although OOP aspects exist, note the following : 
1) Go uses "types", not "classes"
2) Go does not "instantiate", you create a "value of a type"  
```

##### Workspace 
```
bin : holds compiled executable programs 
pkg : holds package objects so recompiling is unnecessary  
src : holds source files organized as packages  
```

##### Commands 
```
go version : version check
go env : environment check 
go help : help tool 
go fmt : format a file 
go fmt ./... : format all files under the current directory 
go run <filename> : run file 
go build <filename> : build an executable file in the current directory
go install <filename> : build executable file on the bin path 
```

##### Modules (learn more about modules, dependencies, packages)
```
1) a way to manage different dependencies 
2) collection of packages stored in a file tree with go.mod as the root 

go.mod 
1) defines the module path  
2) defines dependency requirements 
```

##### Identifiers 
```
name used to identify a variable, function, and any other user-defined item 
```


<br />


### Basics 
---
##### Declaring and Assigning 
```go
// short declaration operator cannot be declared globally 
func main() {
    x := 3
}

// var can be declared globally 
var y int8 = 1           // used to save memory space    

func main() {
    var z = 1            // untyped variable  
}
``` 

##### Numeric Types 
```
unsigned integers : uint8, 16, 32, 64
signed integers : int8, 16, 32, 64
float numbers: float32, 64
complex numbers : complex64, 128
byte = uint8
rune = int32
```

##### String Types
```go
s := "hello world"   // must use double quotes 
s := `
hello          // ` is equivalent of Python """
world
`

c := 'A'             // ascii number for char A 
b := []byte(s)       // ascii number for each char in s
```

##### Multiple Type Assignment 
```go
// untyped constants 
const (
    // untyped constants 
    a = 48 
    b = "Hello" 

    // untyped constants 
    a int8 = 48 
    b string = "Hello" 
)

// alternatively 
var a, b int 
a = 30 
b = 30 
```

##### Looping
```go
for i := 0; i < 100; i++   // for loop 
for i < 10                 // while loop 
for                        // eternal loop 
```

##### If Statement 
```go
// this limits x's scope to the if loop 
if x := 42; x == 42 {
    fmt.Println(x)      // 42
}
fmt.Println(x)          // fails 
```

##### Switch Statement 
```go
switch {                   // find the first true expression
case true:
    fmt.Println("First")   // prints and breaks here   
case 2==2:
    fmt.Println("Second")  // correct but does not print
default:
    fmt.Println("Does not exist")
}

switch x := "Bond"; x {
case "James", "Bond", "Mark":   // James or Bond or Mark 
    fmt.Println("First")        // fallthrough proceeds to next statement 
    fallthrough           
case "Alice", "Mary":
    fmt.Println("Second")       // fallthrough prints even if statement is wrong 
    fallthrough           
default:
    fmt.Println("Does not exist")

}
```


<br />


### [Array/Slices]
---
##### Initialization
```go
// Array
var x [5]int 
y := [5]int{4, 5, 6, 7, 8}
z := [5][5]int{}

// Slices (ArrayList)
var x []int 
y := []int{4, 5, 6, 7, 8}
z := [][]int{{1,2,3,4}, {5,6,7,8}}

// Slices with Make Function 
// if a slice is full, append operations copy existing elements to a larger slice 
// as this takes O(n) time, we can specify the capacity beforehand   
x := make([]int, 10, 100)
y := [][]int{x, x}

// Length and Capacity 
len(x)        // length is 10 
cap(x)        // total capacity is 100 
```

##### Composite Literal 
```
any array, slice, map, struct initialized with type and braces of elements 
[5]int{3,4,5,6}
map[string]int{"x1" : 1}
Person{first : "Alex", last : "Junior"}
```

##### Operations 
```go 
// Enhanced For Loop 
for i, v := range x {
    // i is index 
    // v is value 
}

// Others 
y := x[1:]
z := append(x, 77, 101, 123)
k := append(x, z...)
```


<br />


{Map}
---
##### Initialization 
```go 
// maps in Go are unsorted, default maps  
hm := map[string]int{"x1":1, "x2":-1, "x3":4} 
hm["x1"]               // 1
hm["x4"]               // 0 (does not give key error) 

var hm2 map[string]int // hm2 currently points to nil 
hm2["x1"] = 1          // entry to nil error 
hm2 = make(map[string]int)  
```

##### Comma Ok Idiom 
```go
v, ok := hm["x4"]
v     // the value mapping 
ok    // whether or not the key is in the map 

if v, ok := hm["x4"]; ok {
  // do something 
} 
```

##### Operations 
```go
for k, v := range hm {
    // do something 
}

delete(hm, "x1")
delete(hm, "x4")   // no error for deleting unknown key 
```


<br />


### {Structs} 
---
##### Initialization 
```go
type Person struct {
    first string 
    last string 
}

p1 := Person{first : "Alex", last : "Junior"}
p1.first 
p1.last
```

##### Embedded Structs 
```go
type Student struct {
    Person         // do not specify a field name 
    school string 
}

s1 := Student{
    Person : Person{first : "Alex", last : "Junior"},
    school : "NYU",
} 
s1.Person.first    // returns Alex 
s1.first           // also returns Alex 
```

##### Anonymous Structs 
```go
p1 := struct{
    first string 
    last string 
}{
    first : "Alex",
    last : "Junior",
}
```


<br />


### Golang Things 
---
##### Printf 
```
%d    : integer 
%f    : float 
%v    : value 
%b    : binary 
%#x   : hexadecimal 
%T    : type 
```


<br />


### Logic Operations / Bit Manipulation 
---
##### iota 
```go 
// iota is only possible for constants, and auto increments by 1 
const (
    a = iota           // a = 0 
    b = iota           // b = 1
    c                  // c = 2 (don't have to specify iota)
)

const (
    d = 2016 + iota   // back to 0 --> 2016 
    e                 // e = 2017 
)
```

##### Utilizing iota 
```go
const (
    _ = iota               // unassigned variable 
    kb = 1 << (iota * 10)
    mb = 1 << (iota * 10)
    gb = 1 << (iota * 10)
)
```

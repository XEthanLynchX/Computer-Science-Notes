## Pros of using Go 
- Go has a fast run time and compilation time
- Go supports concurrency (Performing multiple things out of order or at the same time without affecting the final outcome)
- Go has memory management 
- Go works on different platforms (Windows, Mac, Linux, Raspberry Pi, etc)

## About Go 
- Go is statically typed 
- Fast Runtime 
- Compiled
- Concurrency is supported through 'gorountines and channel'
- automatic garbage collection 
- Does not support classes and objects
- Does not support inheritance
- Go is statically typed, meaning that once a variable type is defined, it can only store data of that type.

## Go Syntax 
A Go file consists of the following parts:
- Package declaration (any excuable code belongs to the main package)
- Import packages
- Functions
- Statements and expressions
#### Go Statements
- In Go, statements are separated by ending a line (hitting the Enter key) or by a semicolon "`;`" (doesn't show in src code)
- The left curly bracket `{` cannot come at the start of a line. Will throw an error

#### Go Comments 
- comments are written usin '//' 
- multi line comments are written using '/*     and     */'

#### Go variables 
In Go, there are different **types** of variables, for example:
- `int`- stores integers (whole numbers), such as 123 or -123
- `float32`- stores floating point numbers, with decimals, such as 19.99 or -19.99
- `string` - stores text, such as "Hello World". String values are surrounded by double quotes
- `bool`- stores values with two states: true or false

**Declaring Variables**
Use the `var` keyword followed by the name and then type (best practice)
```go 
var name string = "Ethan"
```
- You can also make inferred type (Determined by the compiler) 
- ```go
  // dont't need var (inferred)
  name := "Ethan" 
  // No type (inferred)
  var name = "Ethan"
  // No value given just initialized 
  var name string 
  
  ```

- When a variable is initialized without a value it will be given the falsy value 
- String = "", int = 0, bool = false
- You can declare multiple variables on the same line 
- ```go 
  var x, y, z = 5, 10, 15
  ```

#### Differences between var and := 
**var** 
- Can be used **inside** and **outside** of functions
- Variable declaration and value assignment **can be done separately**
**:=** 
- Can only be used **inside** functions
- Variable declaration and value assignment **cannot be done separately** (must be done in the same line)

#### Go Constants 
- If a variable should have a fixed value that cannot be changed, you can use the `const` keyword.
- The `const` keyword declares the variable as "constant", which means that it is **unchangeable and read-only**.


## Go Outputs
Go has three functions to output text:
- `Print()` - Prints the text exactly as value is 
- `Println()` - Adds whitespace between two arguments (values) and a newline is added the end
- `Printf()` - first formats its argument based on the given formatting verb and then prints them.
	- `%v` is used to print the **value** of the arguments
	- `%T` is used to print the **type** of the arguments
	- `%#v` Prints the value in Go-syntax format
	- `%%` prints the % sign
- The "\n" wildcard can be used to make a new line
- There is a bunch of other ones @ https://www.w3schools.com/go/go_formatting_verbs.php

#### Go Arrays
Arrays have a fixed size after creation and length cannot change after creation
Basics of Array
```go 
//var array = [length]datatype{values}
var array = [3]int{1,2,3}
array2 := [5]int{4,5,6,7,8}
/* Default value of given type 
[0, 0, 0, 0, 0] */
array3 := [5]int{} //not initialized

// initilizes specific elements [0, 10, 40, 0, 0]
arr1 := [5]int{1:10,2:40}

//Will return '3'
array[2]

//change value
array[2] = 50 

//find the length of an array 
len(array)

```

#### Go Slices 
Like arrays, slices are also used to store multiple values of the same type in a single variable.
However, unlike arrays, the length of a slice can grow and shrink as you see fit.

In Go, there are several ways to create a slice:
- Using the `[]datatype{values}` format
- Create a slice from an array
- Using the make() function
- `len()` function - returns the length of the slice (the number of elements in the slice)
- `cap()` function - returns the capacity of the slice (the number of elements the slice can grow or shrink to)
```go
  package main  
import ("fmt")  
  
func main() {  
  myslice1 := []int{}  
  fmt.Println(len(myslice1))  
  fmt.Println(cap(myslice1))  
  fmt.Println(myslice1)  
  
  myslice2 := []string{"Go", "Slices", "Are", "Powerful"}  
  fmt.Println(len(myslice2))  
  fmt.Println(cap(myslice2))  
  fmt.Println(myslice2)  
}

/* Will Print: 
0  
0  
[]  
4  
4  
[Go Slices Are Powerful] 
*/
```

- You can create a slice by slicing an array:
- ```go 
var myarray = [length]datatype{values} // An array  
myslice := myarray[start:end] // A slice made from the array (use index for start and end)
slice_name := make([]type, length, capacity)
  ```
- Use the `append()` function to add elements to a slice
- ```go
  //Append one slice to another 
  // The ... at end is necessary to carry over elements 
  slice3 = append(slice1, slice2...)
  ```

#### Memory Efficiency
When using slices, Go loads all the underlying elements into the memory.
If the array is large and you need only a few elements, it is better to copy those elements using the `copy()` function.

The `copy()` function creates a new underlying array with only the required elements for the slice. This will reduce the memory used for the program.


```go
package main  
import ("fmt")  
  
func main() {  
  numbers := []int{1,2,3,4,5,6,7,8,9,10,11,12,13,14,15}  
  // Original slice  
  fmt.Printf("numbers = %v\n", numbers)  
  fmt.Printf("length = %d\n", len(numbers))  
  fmt.Printf("capacity = %d\n", cap(numbers))  
  
  // Create copy with only needed numbers  
  neededNumbers := numbers[:len(numbers)-10]  
  numbersCopy := make([]int, len(neededNumbers))  
  copy(numbersCopy, neededNumbers)  
  
  fmt.Printf("numbersCopy = %v\n", numbersCopy)  
  fmt.Printf("length = %d\n", len(numbersCopy))  
  fmt.Printf("capacity = %d\n", cap(numbersCopy))  
}
```

## Go Operators 
- && Logical AND
- || Logical OR


## Go Switch Statements 
syntax 
```go 
switch expression {  
case x,y:  
   // code block if expression is evaluated to x or y_  
case v,w:  
   // code block if expression is evaluated to v or w_  
case z:  
...  
default:  
   _// code block if expression is not found in any cases_  
}
```


## Go Loops
`for statement1; statement2; statement3 {  
   // code to be executed for each iteration_  
}`
```go
func main() {  
  for i:=0; i < 5; i++ {  
    fmt.Println(i)  
  }  
}
```
- `continue` - used to skip iteration under certain condition 
- `break` - terminate the loop execution
- The `range` keyword is used to more easily iterate through the elements of an array, slice or map. It returns both the index and the value.
- ```go 
  // The `range` keyword is used like this:
  for index, value := range array|slice|map {  
   // code to be executed for each iteration
   
func main() {  
  fruits := [3]string{"apple", "orange", "banana"}  
  for idx, val := range fruits {  
     fmt.Printf(idx, val)  
  }  
} 
/*prints
0      apple  
1      orange  
2      banana
*/ 
}
  ```

## Go Functions 
To create (often referred to as declare) a function, do the following:
- Use the `func` keyword.
- Specify a name for the function, followed by parentheses ().
- Finally, add code that defines what the function should do, inside curly braces {}.
```go 
func familyName(fname string) {  
  fmt.Println("Hello", fname, "Refsnes")  
}  
  
func main() {  
  familyName("Liam")  
  familyName("Jenny")  
  familyName("Anja")  
}

/* Prints: 
Hello Liam Refsnes  
Hello Jenny Refsnes  
Hello Anja Refsnes
*/
```


#### Recursion Functions
Go accepts recursion functions. A function is recursive if it calls itself and reaches a stop condition.

In the following example, `testcount()` is a function that calls itself. We use the `x` variable as the data, which increments with 1 (`x + 1`) every time we recurse. The recursion ends when the `x` variable equals to 11 (`x == 11`).
```go 
func testcount(x int) int {  
  if x == 11 {  
    return 0  
  }  
  fmt.Println(x)  
  return testcount(x + 1)  
}  
  
func main(){  
  testcount(1)  
}
```

## Go Structures
A struct (short for structure) is used to create a collection of members of different data types, into a single variable.

While arrays are used to store multiple values of the same data type into a single variable, structs are used to store multiple values of different data types into a single variable.

A struct can be useful for grouping data together to create records.

```go 
type Person struct {  
  name string  
  age int  
  job string  
  salary int  
}

func main() {  
  var pers1 Person  
  
  // Pers1 specification  
  pers1.name = "Hege"  
  pers1.age = 45  
  pers1.job = "Teacher"  
  pers1.salary = 6000  
  
  
  // Print Pers1 info by calling a function  
  printPerson(pers1)  
  
  // Print Pers2 info by calling a function  
  printPerson(pers2)  
} 
  
func printPerson(pers Person) {  
  fmt.Println("Name: ", pers.name)  
  fmt.Println("Age: ", pers.age)  
  fmt.Println("Job: ", pers.job)  
  fmt.Println("Salary: ", pers.salary)  
}
```

## Go Maps

- Maps are used to store data values in key:value pairs.
- Each element in a map is a key:value pair.
- A map is an unordered and changeable collection that does not allow duplicates.
- The length of a map is the number of its elements. You can find it using the `len()` function.
- The default value of a map is nil.
- Maps hold references to an underlying hash table.
```go 
var a = map[KeyType]ValueType{key1:value1, key2:value2,...}  
b := map[KeyType]ValueType{key1:value1, key2:value2,...}
//example
var a = map[string]string{"brand": "Ford", "model": "Mustang", "year": "1964"}  
b := map[string]int{"Oslo": 1, "Bergen": 2, "Trondheim": 3, "Stavanger": 4}

//using make()
var a = make(map[KeyType]ValueType)  
b := make(map[KeyType]ValueType)
//example 
 var a = make(map[string]string) 
 a["brand"] = "Ford"  
 a["model"] = "Mustang"  
 a["year"] = "1964"
```
- if you are initializes a new empty map prefer make() because if you create an empty map without make() it will evaluate to true even though its empty

The map key can be of any data type for which the equality operator (`==`) is defined. These include:
- Booleans
- Numbers
- Strings
- Arrays
- Pointers
- Structs
- Interfaces (as long as the dynamic type supports equality)

Invalid key types are:
- Slices
- Maps
- Functions
These types are invalid because the equality operator (`==`) is not defined for them.

- The map values can be **any** type.

To access a map
```go 
 var a = make(map[string]string)  
  a["brand"] = "Ford"  
  a["model"] = "Mustang"  
  a["year"] = "1964"  
  //print Ford
  fmt.Printf(a["brand"])

```

- To remove an element from a map 
```go 
  func main() {  
  var a = make(map[string]string)  
  a["brand"] = "Ford"  
  a["model"] = "Mustang"  
  a["year"] = "1964"  
  
  fmt.Println(a)  
  
  delete(a,"year")  
  
  fmt.Println(a)  
}
```

- Maps are references to hash tables.
- If two map variables refer to the same hash table, changing the content of one variable affect the content of the other.

#### Looping over a Map 
```go 
package main  
import ("fmt")  
  
func main() {  
  a := map[string]int{"one": 1, "two": 2, "three": 3, "four": 4}  
  
  var b []string             _// defining the order_  
  b = append(b, "one", "two", "three", "four")  
  
  for k, v := range a {        _// loop with no order_  
    fmt.Printf("%v : %v, ", k, v)  
  }  
  
  fmt.Println()  
  
  for _, element := range b {  _// loop with the defined order_  
    fmt.Printf("%v : %v, ", element, a[element])  
  }  
}

//Result:
//two : 2, three : 3, four : 4, one : 1,  
//one : 1, two : 2, three : 3, four : 4,
```
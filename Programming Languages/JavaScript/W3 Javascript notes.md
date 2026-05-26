## Variables 
### Let 
- variables with let always have block-scope
- variables that use let cannot be redeclared within the same scope 
- variables using let can be reassigned
### Const 
- const also has block-scope 
- cannot be redeclared or assigned 
- advanced types such as objects or arrays can be updated after creation 
- const doesn't define a constant value it defines a constant reference to a value. 
- This why advanced type can be changed but primitive types cannot (referencing the array/object itself not values inside)
### Var 
- Avoid in modern code (use const & let)
- Does not have scope always global 

## Logical Assignment Operators 
### The ??= Operator 
- If the first value is undefined or null then the second value is assigned 
- If the val
``` js 
let x = 10 
x ??= 15 
// X will equal 10 
```

``` js 
let x = undefined 
x ??= 15 
// X will equal 15 
```

### The &&= Operator 
- If the first value is true then the second value is assigned 
```js
  let x = true
  let y = x &&= 5
  // y = 5
  ```

```js
let x = false 
let y = x &&= 10 
// y = false
```

### The ||= Operator
- If the first value is false then the second value is assigned 
```js
  let x = true
  let y = x ||= 5
  // y = true
  ```

```js
let x = false 
let y = x ||= 10 
// y = 10
```

### The Spread (...) Operator
- Will split out iterables into individual elements 
- ```js
let text = "12345";  
  
let min = Math.min(...text); 
// min will be 1
let max = Math.max(...text);
//max will be 5
  ```

## Comparisons 
**Given x = 5**
## The == Comparison Operator 
- Checks value only not type
![[W3 Javascript notes.png]]

## The === Comparison Operator 
- Checks the value and the type ![[W3 Javascript notes2.png]]

## Comparing strings 
- Strings are compared alphabetically 
- ```js 
let text1 = "20";  
let text2 = "5";  
let result = text1 < text2;
// Results = true 
// This is because you are comparing strings for example A < B = true 
  ```

## Comparing Different Types
- When comparing a string with a number, JavaScript will convert the string to a number when doing the comparison. An empty string converts to 0. A non-numeric string converts to `NaN` which is always `false`.

# Condtionals

### Switch Statements 
- Switch uses the === Operator when comparing cases 

### Conditonal Ternary Operator (? :)
- condintion ? expression1 : expression2 
- (If) ? (do this if true) : (do this if false) 
- ```js
  let age = 17
  let status = (age < 18) ? "Too young" : "An adult"
  
  ```

### Conditionals of Boolean values
- The Boolean value of 0 is false 
- Everything that is not defined (without a value or undefined/null) evaluates to false 
- Everything with a value excluding 0 is true 

### The Nullish Coalescing Operator (??)
- The ?? operator returns the right hand side when the left value is nullish (null or undefined) otherwise it returns the righthand side 
- If the string is empty or the value is 0 it will return that as those arent nullish rather they are falsey 


## Loops 
### The For Loop 
- for(exp1; exp2; exp3){ ...code... }
- **exp 1** is executed (one time) before the execution of the code block.
- **exp 2** defines the condition for executing the code block.
- **exp 3** is executed (every time) after the code block has been executed.
```js 
for(let i = 0; i < 10; i++){ 
print(`This loop has ran ${i} times.`)
}
```

### The Do While Loop 
- The `do while` loop is a variant of the while loop. This loop will execute the code block once, before checking if the condition is true, then it will repeat the loop as long as the condition is true.
```js
  do {  
  // code block to be executed_}  
while (_condition_);
```
- The `do while` runs at least once, even if the condition is false from the start.

### Labels and breaks
- Labels can be used to label a statement for it to referenced to 
- ```js
let text = "";  
  
loop1: for (let j = 1; j < 5; j++) {  
  loop2: for (let i = 1; i < 5; i++) {  
    if (i === 3) { break loop1; }  
    text += i;  
   }  
}
  ```

- With labels break can jump out of any code block 
- ```js 
const cars = ["BMW", "Volvo", "Saab", "Ford"];  
list: {  
  text += cars[0] + "<br>";  
  text += cars[1] + "<br>";  
  break list;  
  text += cars[2] + "<br>";  
  text += cars[3] + "<br>";  
}
  ```

### Strings 
- To insert quotes into a string you can use \" word \" format 
- if you want to insert a backslash character into a string just use two \\ and one will display 
- other useful characters to know for string are 
- \n makes the string be on a new line 
- \b for backspace
- \t for Horizontal tab 
-  v for vertical tab
#### String Methods 
- 'string'.length - returns the length of given string
- charAt() - returns the character at passed index 
- charCodeAt() - returns the code (UTF-16)
E022{ 
- Access chars from strings using string[i] or string.at() method
}
- concat() - join two or MORE strings together 
- slice() - extract part of a string and return extracted part as a new string first param is the start index and second param is the end index(not included) if you omit second param will 
- substring() - same as slice() but The difference is that start and end values less than 0 are treated as 0 in `substring()`.
- `toUpperCase()`: make stringg all upper
- `toLowerCase()`: make string all lower
- trim(): removes whitespace from both sides of a string:
- trimStart(): removes whitespace from start of string
- trimEnd(): removes the whitespace from the end of the string
- padStart(): pad a string with another string until it reaches given length first param is length second param is the string (useful for making longer number strings )
- padEnd(): pads a string with another string (multiple times) until it reaches a given length.
- repeat()

### Objects 
- ** Example of an object 
- ```js
  const car = { 
  type: "Sudan", 
  model: "Honda", 
  make: "Accord"
  }
  ```

- **Accessing an object can be done in a few ways 
```js
// Set a variable from an object's property 
let age = person.age;

// or use
let age = person["age"];

const name = 
// Use bracket notation when using a variable 
person[name]

```

In general, **dot notation is preferred** for readability and simplicity.

**Bracket notation is necessary in some cases:**
- The property name is stored in a variable:  
    person[myVariable]
- The property name is not a valid identifier:  
    person["last-name"]

- **Changing/Adding Properties within an object 
```js 
//Will change Person's age prop to "10"
person.age = 10 

//Add properties by simply giving a new property a value 
person.nationality = "English";
```

- **Deleting properties 
- The `delete` keyword deletes a property from an object:
```js 
const person = {  
  firstName: "John",  
  lastName: "Doe",  
  age: 50,  
};  
  
delete person.age;
```

In JavaScript, almost "everything" is an object:
- Objects are objects
- Maths are objects
- Dates are objects
- Arrays are objects
- Maps are objects
- Sets are objects
- RegExp are Objects
- Errors are Objects

**Check if property exists  (in)
```js 
const person = {  
  firstName: "John",  
  lastName: "Doe"  
};  
  
let result = ("firstName" in person); //returns true
```

**Nested objects and accessing them 
```js 
myObj = {  
  name:"John",  
  age:30,  
  myCars: {  
    car1:"Ford",  
    car2:"BMW",  
    car3:"Fiat"  
  }  
}

const FancyCar = myObj.myCars.car2;
```

- **Objects methods 
In an object method, `this` refers to the object.
```js 
const person = {  
  firstName: "John",  
  lastName: "Doe",  
  id: 5566,  
  getId: function() {  
    return **this**.id;  
  }  
};  
  
let number = person.getId();
```

In the example above, **`this`** refers to the **person object**.
`this.id` means the **id property** of the **person object**.

- If you call a method **with parentheses**, it will **execute as a function**
- If you call a method **without parentheses**, it will return the **function definition**
### Adding a Method to an Object

You can add a method to an object by **assigning a function to a property**:
```js

// Assign person.name to a function  
person.name = function () {  
  return this.firstName + " " + this.lastName;  
};
```

### Getting Key names / values using indexs 
- use the obj.keys
```js 
const car = { 
make: "Honda", 
model: "Accord"
}

const key = Object.keys(car)[0]) //value is set to make
```

### Object.values() 
- If you put an object in here it'll convert it to an array
```js 
const person = { 
  name: "Ethan", 
  age: 22
} 

const arr = Object.values(person)

console.log(arr)

//Output: [ 'Ethan', 22 ]
```

### Object.entries()
```js 
const person = { 
  name: "Ethan", 
  age: 22,
  job: "SWE"
} 

for (let [attr, val] of Object.entries(person)){ 
console.log(`${attr}: ${val}`)
} 

/*
Outputs:
name: Ethan
age: 22
job: SWE
*/
```

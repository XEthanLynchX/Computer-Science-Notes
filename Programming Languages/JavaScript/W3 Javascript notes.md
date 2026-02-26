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
- (If) (do this if true) (do this if false) 
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
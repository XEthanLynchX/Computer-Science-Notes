# Using Prefix and Postfix (Two pointer) 
## Applying it to a stack
- To solve the equation use a stack. 
- First you apply the prefix and/or postfix math conversion then you go from right to left (prefix) and left to right (postfix) 
- The only thing that get added to the stack is operands (numbers) then when you come across an operator it is applied to the top 2 values of the stack. 
- When applying the operator to the top two numbers of the stack you pop them from the stack and assign them to a variable then apply the operator and push the new number on top of the stack. 
- The prefix algo applies the operation top variable - > operator -> bottom variable when computing 
- The postfix algo applies the operation bottom variable -> operator -> top variable when computing 
- The final value on the stack is the final computation 
- A good thing to note is you NEVER start with the side that begins with an operator (else you have nothing to apply the operator to)
## Example: ![[394D5EE5-46C8-4AEE-B647-CAF3B4B4810F_4_5005_c.jpeg]]

## Infix to Postfix Algorithm
1. Create an empty stack and an empty output string.
2. Scan infix expression input string from left to right.
3. If the current input token is an operand, append it to the output string.
4. If the current input token is an operator, pop off all operators that have
equal or higher precedence and append them to the output string;
push the current operator onto the stack.
5. If the current input token is '(', push it onto the stack.
6. If the current input token is ')', pop off all operators and append them
to the output string until first '(' is popped; discard the ‘)'.
7. If the end of the input string is found, pop all operators and append
them to the output string.

## Infix to Prefix Algorithm
Note the infix is created by reading the expression backwords and writing it left to right then apply the algo to it
1. Create an empty stack and an empty output string.
2. Scan infix input string from left to right.
3. If the current input token is an operand, append it to the output string.
4. If the current input token is an operator, pop off all operators that have
higher precedence and append them to the output string; push the
current operator onto the stack.
5. If the current input token is ‘)', push it onto the stack.
6. If the current input token is ‘(', pop off all operators and append them
to the output string until a ‘)' is popped; discard the ‘('.
7. If the end of the input string is found, pop all operators and append
them to the output string.
## Example: ![[71EDA44E-92C0-4552-B68E-E992F5E7DEA9_1_102_o.jpeg]]


## Common Stack Errors 
- Common errors that occur are stack overflow errors and stack empty error
- Stack overflow happens when you try to add an element when the stack is full
- Stack empty error is when you try to pop an element on an empty stack 


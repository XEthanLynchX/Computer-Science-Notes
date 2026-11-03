[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 6: Evaluating & Converting Prefix/Postfix

Builds on [[Data Structures & Algos/Class Notes/Lecture 5|Lecture 5]] (stacks and manual conversion).

## Evaluating prefix and postfix with one stack

Only **operands** (numbers) go on the stack. Operators are applied as soon as you reach them.

| | Postfix | Prefix |
| --- | --- | --- |
| Scan direction | **Left → right** | **Right → left** |
| Number | Push it | Push it |
| Operator | Pop top two, compute, push result | Pop top two, compute, push result |
| Operand order | **bottom op top** (second popped op first popped) | **top op bottom** (first popped op second popped) |
| Answer | The one value left on the stack | The one value left on the stack |

- You always start scanning from the end that **begins with an operand**. If you started on an operator, there would be nothing on the stack to apply it to.
- Operand order only matters for **−** and **/**, but getting it backwards is the most common exam mistake.

### Postfix example

`a b c * + d -` with a = 2, b = 3, c = 4, d = 5 (that's `2 + 3 * 4 - 5` = **9**)

| Token | Action | Stack (bottom → top) |
| --- | --- | --- |
| 2 | push | [2] |
| 3 | push | [2, 3] |
| 4 | push | [2, 3, 4] |
| * | 3 * 4 = 12 | [2, 12] |
| + | 2 + 12 = 14 | [14] |
| 5 | push | [14, 5] |
| − | 14 − 5 = 9 (bottom − top) | [9] |

### Prefix example

`- + a * b c d`, same values, scanned **right to left**

| Token | Action | Stack (bottom → top) |
| --- | --- | --- |
| 5 (d) | push | [5] |
| 4 (c) | push | [5, 4] |
| 3 (b) | push | [5, 4, 3] |
| * | 3 * 4 = 12 | [5, 12] |
| 2 (a) | push | [5, 12, 2] |
| + | 2 + 12 = 14 | [5, 14] |
| − | 14 − 5 = 9 (top − bottom) | [9] |

### Example from class

![[394D5EE5-46C8-4AEE-B647-CAF3B4B4810F_4_5005_c.jpeg]]

---

## Infix → Postfix algorithm

1. Create an empty **operator stack** and an empty **output** string.
2. Scan the infix expression **left to right**.
3. **Operand** → append to the output.
4. **Operator** → pop every operator on the stack with **equal or higher** precedence and append it to the output; then push the current operator.
5. **`(`** → push it.
6. **`)`** → pop operators to the output until you pop the matching `(`; discard both parentheses.
7. **End of input** → pop all remaining operators to the output.

Precedence: `*` `/` are higher than `+` `-`.

### Example: `a + b * c - d`

| Token | Action | Stack | Output |
| --- | --- | --- | --- |
| a | append | [] | a |
| + | push | [+] | a |
| b | append | [+] | ab |
| * | nothing higher/equal on top; push | [+, *] | ab |
| c | append | [+, *] | abc |
| − | pop * (higher), pop + (equal); push − | [−] | abc*+ |
| d | append | [−] | abc*+d |
| end | pop all | [] | **abc\*+d−** |

## Infix → Prefix algorithm

1. **Reverse** the infix expression (read it backwards and write it left to right).
2. Create an empty operator stack and an empty output string.
3. Scan the reversed string **left to right**.
4. **Operand** → append to the output.
5. **Operator** → pop operators with **strictly higher** precedence (not equal) and append them; then push the current operator.
6. **`)`** → push it (in the reversed string, `)` acts like an opening parenthesis).
7. **`(`** → pop operators to the output until a `)` is popped; discard both.
8. **End of input** → pop all remaining operators to the output.
9. **Reverse the output.** That's the prefix expression.

Differences from postfix: reverse at the start and end, swap the roles of `(` and `)`, and pop only **strictly higher** precedence.

### Example: `a + b * c - d`

Reversed: `d - c * b + a`

| Token | Action | Stack | Output |
| --- | --- | --- | --- |
| d | append | [] | d |
| − | push | [−] | d |
| c | append | [−] | dc |
| * | − is not higher; push | [−, *] | dc |
| b | append | [−, *] | dcb |
| + | pop * (higher); − is equal, stays; push + | [−, +] | dcb* |
| a | append | [−, +] | dcb*a |
| end | pop all | [] | dcb\*a+− |

Reverse the output: **−+a\*bcd**

### Example from class

![[71EDA44E-92C0-4552-B68E-E992F5E7DEA9_1_102_o.jpeg]]

---

## Common stack errors

| Error | Cause | Prevention |
| --- | --- | --- |
| **Stack overflow** | `push` when the stack is full | Check `isFull()` before pushing |
| **Stack empty / underflow** | `pop` or `peek` on an empty stack | Check `isEmpty()` before popping |

When evaluating an expression, an underflow means the expression is **malformed** (too many operators). More than one value left at the end means **too many operands**.

## Self-Check

1. Evaluate the postfix `6 2 / 3 -`. Then evaluate the prefix `- / 6 2 3`.
2. Convert `(a + b) * (c - d)` to postfix with the algorithm, showing the stack at each step.
3. Why does the prefix algorithm pop only **strictly higher** precedence?

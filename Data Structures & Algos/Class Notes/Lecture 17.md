[[Data Structures & Algos/Class Notes/Lectures|← All lectures]]

# Lecture 17: Tree Traversals & Expression Trees

Builds on [[Data Structures & Algos/Class Notes/Lecture 15|Lecture 15]] (traversals intro) and [[Data Structures & Algos/Class Notes/Lecture 5|Lecture 5]] / [[Data Structures & Algos/Class Notes/Lecture 6|Lecture 6]] (prefix/postfix with stacks).

### Lecture Notes
![[66C8CC24-D945-4D6A-A901-E7913DFA10A9.jpeg]]
![[B3500CCE-1B74-40F0-9AEF-052BCE99D96C.jpeg]]

---

## Part 1: Depth-First Traversals

A **traversal** visits every node of a tree exactly once. The three depth-first traversals all go **left before right**. The only difference is **when you visit (V) the node itself**.

| Traversal | Order | Visit the node… | Memory trick |
| --- | --- | --- | --- |
| **Preorder** | **V** L R | **before** its subtrees | "**Pre** = first" |
| **Inorder** | L **V** R | **between** its subtrees | "**In** the middle" |
| **Postorder** | L R **V** | **after** its subtrees | "**Post** = last" |

### The recursive definition

Each traversal is recursive: "traverse the left subtree" means **run the whole traversal again** on that subtree, with its own root. The base case is an empty subtree (`null`): do nothing.

```java
void preorder(Node n) {
    if (n == null) return;     // base case
    visit(n);                  // V
    preorder(n.left);          // L
    preorder(n.right);         // R
}

void inorder(Node n) {
    if (n == null) return;
    inorder(n.left);           // L
    visit(n);                  // V
    inorder(n.right);          // R
}

void postorder(Node n) {
    if (n == null) return;
    postorder(n.left);         // L
    postorder(n.right);        // R
    visit(n);                  // V
}
```

The code is identical except for **where `visit(n)` sits**.

### Example

```
        A
      /   \
     B     C
    / \     \
   D   E     F
```

| Traversal | Result |
| --- | --- |
| Preorder (VLR) | A B D E C F |
| Inorder (LVR) | D B E A C F |
| Postorder (LRV) | D E B F C A |

### How to work one out by hand (inorder example)

Treat every node as a mini-problem: **"left subtree, me, right subtree."**

```
inorder(A) = inorder(B)          A   inorder(C)
           = [inorder(D) B inorder(E)]   A   [inorder(null) C inorder(F)]
           = [D B E]                     A   [C F]
           = D B E A C F
```

Common mistake: visiting a node as soon as you reach it in inorder. You can't visit B until its **entire left subtree** (D) is done.

### Shortcut: the flag trick

Draw a small flag on each node, then trace a line around the outside of the tree, counterclockwise from the root's left side. Write each node down when the line passes its flag:

| Traversal | Flag position |
| --- | --- |
| Preorder | **Left** side of the node |
| Inorder | **Bottom** of the node |
| Postorder | **Right** side of the node |

### Patterns to check your answer

- Preorder **starts** with the root. Postorder **ends** with the root.
- In all three, leaves appear in the **same left-to-right order** (D, E, F above).
- Inorder of a **BST** comes out **sorted**.

### Cost

All three are **O(n) time** (each node visited once) and **O(h) space** for the recursion stack, where h is the tree's height.

---

## Part 2: Expression Trees

An **expression tree** is a binary tree that stores an arithmetic expression:

- **Leaves** are **operands** (numbers or variables).
- **Internal nodes** are **operators** (+ − * /). Each operator's **left child** is its left operand and its **right child** is its right operand.
- **No parentheses are stored.** The **shape** of the tree already fixes the order of operations: deeper operators run first.

Example: `(a + b) * c - d / e`

```
          -
        /   \
       *     /
      / \   / \
     +   c d   e
    / \
   a   b
```

`+` is deepest, so it runs first. `-` is the root, so it runs last.

### Traversals of an expression tree = the three notations

This is the link between this lecture and Lectures 5–6. **Preorder** is a traversal; **prefix** is the notation it produces when the tree holds an expression.

| Traversal | Output | Notation |
| --- | --- | --- |
| Preorder | `- * + a b c / d e` | **Prefix** |
| Inorder | `a + b * c - d / e` | **Infix** (wrong without parentheses!) |
| Postorder | `a b + c * d e / -` | **Postfix** |

**Gap to watch: inorder loses the parentheses.** `a + b * c - d / e` would compute `b * c` first, which is a different expression. To get correct infix, print `(` before each operator's left subtree and `)` after its right subtree:

```java
void printInfix(Node n) {
    if (n == null) return;
    if (n.isOperator()) System.out.print("(");
    printInfix(n.left);
    System.out.print(n.value);
    printInfix(n.right);
    if (n.isOperator()) System.out.print(")");
}
// Output: (((a+b)*c)-(d/e))
```

Prefix and postfix never need parentheses, which is why the tree can be stored without them.

### Evaluating an expression tree (postorder)

To compute an operator node, you need **both children's values first**, so evaluation is a **postorder** traversal (L R V):

```java
int evaluate(Node n) {
    if (n.isOperand()) return n.value;         // leaf: just a number
    int left  = evaluate(n.left);              // L
    int right = evaluate(n.right);             // R
    return apply(n.operator, left, right);     // V: combine
}
```

With a = 2, b = 3, c = 4, d = 10, e = 5:

| Node finished | Computes | Value |
| --- | --- | --- |
| + | 2 + 3 | 5 |
| * | 5 * 4 | 20 |
| / | 10 / 5 | 2 |
| − (root) | 20 − 2 | **18** |

This is the same thing the **postfix stack evaluation** from Lecture 6 does. The postfix string is just the postorder of this tree.

---

## Part 3: Building an Expression Tree from Infix (Two Stacks)

This follows the class handout, *Building Expression Tree Algorithm Using Stacks*.

- **S:** the input infix string
- **Opstack:** operator stack (holds operator nodes and `(`)
- **Tstack:** tree stack (holds subtrees: single operand nodes at first, bigger subtrees later)

### The algorithm

If S is empty, return an empty tree. Otherwise scan S **left to right**:

1. **Operand** → put it in a node and push it onto **Tstack**.
2. **`(`** → push it onto **Opstack**.
3. **Operator** →
    - 3.1 If Opstack is **empty** or its top is **`(`**, push the operator.
    - 3.2 Else if the current operator's precedence is **strictly greater** than the top's, push it. Otherwise **Process** the top of Opstack.
    - 3.3 Repeat 3.1/3.2 until the current operator has been pushed.
4. **`)`** →
    - 4.1 While the top of Opstack is not `(`, **Process**.
    - 4.2 Pop the matching `(` and throw it away.
5. **End of S** → while Opstack is not empty, **Process**.

The tree's root is the one subtree left on Tstack.

### Process (helper function)

1. Take the **top operator** of Opstack. It becomes the **root** of a new subtree.
2. Pop the top **two** subtrees from Tstack: the **first popped is the RIGHT child**, the **second popped is the LEFT child**.
3. Push the new subtree back onto Tstack.
4. Pop the operator off Opstack.

### Clarifications for the handout

- **Step 4.2:** the handout says "Pop matching `)`". It means the matching **`(`**, which is what's on the stack. The `)` itself is never pushed.
- **Process step 2:** "Tpstack" is a typo for **Tstack**.
- **Steps 3.2–3.3:** after a Process, re-check **3.1** as well. If the stack is now empty or the top is `(`, push.
- **Why "strictly greater":** on a **tie** (like `a - b + c`), the operator already on the stack is processed first. That builds `(a - b) + c`, which is the correct left-to-right order. Pushing on a tie would build `a - (b + c)`, which is wrong.
- **Why the first pop is the right child:** the right operand was read **last**, so it's on **top** of Tstack. Getting this backwards flips `a - b` into `b - a`.
- **Parentheses never become nodes.** They only control when Process runs.
- **Precedence:** `*` `/` are higher than `+` `-`; `(` is not compared (step 3.1 handles it).

### Full trace: `(a + b) * c - d / e`

Tstack is written bottom → top. A subtree is written in parentheses for short.

| # | Ch | Action | Opstack | Tstack |
| --- | --- | --- | --- | --- |
| 1 | `(` | push (step 2) | [ ( ] | [ ] |
| 2 | a | push node | [ ( ] | [a] |
| 3 | + | top is `(` → push (3.1) | [ (, + ] | [a] |
| 4 | b | push node | [ (, + ] | [a, b] |
| 5 | `)` | Process `+`: right = b, left = a | [ ( ] | [(a+b)] |
|  |  | pop `(` (4.2) | [ ] | [(a+b)] |
| 6 | * | empty → push (3.1) | [ * ] | [(a+b)] |
| 7 | c | push node | [ * ] | [(a+b), c] |
| 8 | − | − is not > * → Process `*`: right = c, left = (a+b) | [ ] | [((a+b)*c)] |
|  |  | now empty → push − (3.1) | [ − ] | [((a+b)*c)] |
| 9 | d | push node | [ − ] | [((a+b)*c), d] |
| 10 | / | / > − → push (3.2) | [ −, / ] | [((a+b)*c), d] |
| 11 | e | push node | [ −, / ] | [((a+b)*c), d, e] |
| 12 | end | Process `/`: right = e, left = d | [ − ] | [((a+b)*c), (d/e)] |
|  |  | Process `−`: right = (d/e), left = ((a+b)*c) | [ ] | [(((a+b)*c)-(d/e))] |

Result: the tree drawn in Part 2.

### How this connects to Lecture 6

This is the **infix → postfix algorithm** from Lecture 6, with one change. Instead of **appending** an operator to an output string, **Process** makes it the **root of a subtree**. That's why the postorder of the finished tree equals the postfix string.

### Easier case: building from postfix (one stack)

If you're given **postfix**, you only need Tstack:

1. Scan left to right.
2. **Operand** → push a node.
3. **Operator** → pop **right**, pop **left**, make the operator their parent, push the new subtree.
4. At the end, the one subtree left is the tree.

For **prefix**, do the same scanning **right to left**, but the **first** pop is the **left** child.

---

## Summary

| Task | Use |
| --- | --- |
| Get prefix from an expression tree | Preorder |
| Get infix | Inorder + add parentheses |
| Get postfix | Postorder |
| Evaluate the tree | Postorder (children before parent) |
| Build a tree from infix | Two stacks: Opstack + Tstack |
| Build a tree from postfix | One stack: Tstack |

---

## Practice

1. Write the preorder, inorder, and postorder of this tree:

```
        5
      /   \
     2     9
      \   /
       3 7
```

> [!example]- Answer
> Preorder: 5 2 3 9 7 · Inorder: 2 3 5 7 9 · Postorder: 3 2 7 9 5

2. Build the expression tree for `a * (b + c) - d` using the two-stack algorithm. Show Opstack and Tstack after each character.

> [!example]- Answer (final tree)
> ```
>         -
>        / \
>       *   d
>      / \
>     a   +
>        / \
>       b   c
> ```
> Prefix: `- * a + b c d` · Postfix: `a b c + * d -`

3. Evaluate the tree from #2 with a = 3, b = 4, c = 1, d = 5.

> [!example]- Answer
> b + c = 5 → a * 5 = 15 → 15 − 5 = **10**

4. Build the tree for `a - b + c`. Why does the "strictly greater" rule matter here?

> [!example]- Answer
> `-` is processed when `+` arrives (equal precedence), so the root is `+` with left child `(a - b)` and right child `c`. That's `(a - b) + c`. Pushing on a tie would give `a - (b + c)`, which is a different value.

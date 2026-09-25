# Unit 3- Stack
 Array implementation of stack, push and pop operations on stack, applications of stack:- managing function calls, recursion (recursive algorithms for factorial calculation, Tower of Hanoi, etc.), conversion of infix to postfix expression and its evaluation using stack data structure, balanced parenthesis checking, implementing backtracking algorithms, handling undo/redo operations, back and forward buttons in a web browser, Matching HTML tags in web development.

---

# 1. What is a Stack?

### Definition

A **Stack** is a linear data structure in which insertion and deletion are performed from **one end only**, called the **TOP**.

A stack follows:

> **LIFO — Last In, First Out**

This means the element inserted **last** is removed **first**.

### Real-life example

Think of a stack of plates:

```text
       ┌───────┐
       │ Plate │ ← TOP
       ├───────┤
       │ Plate │
       ├───────┤
       │ Plate │
       └───────┘
```

If you want to remove a plate, you remove the **top plate first**.

### Example

Suppose we insert:

```text
10 → 20 → 30
```

Stack becomes:

```text
       30 ← TOP
       20
       10
```

If we perform `POP`:

```text
30 is removed
```

Remaining:

```text
       20 ← TOP
       10
```

Therefore:

```text
Last inserted = 30
First removed = 30
```

---

# 2. Basic Operations of Stack

The main stack operations are:

| Operation   | Meaning                      |
| ----------- | ---------------------------- |
| `PUSH`      | Insert an element            |
| `POP`       | Remove an element            |
| `PEEK/TOP`  | View top element             |
| `isEmpty()` | Check whether stack is empty |
| `isFull()`  | Check whether stack is full  |

---

# 3. Array Implementation of Stack

A stack can be implemented using an **array**.

Suppose we have:

```cpp
int stack[5];
```

The stack can store 5 elements.

We use a variable:

```cpp
top
```

to keep track of the top element.

Initially:

```cpp
top = -1;
```

This means the stack is empty.

### Empty stack

```text
Index:   0    1    2    3    4
       ┌────┬────┬────┬────┬────┐
       │    │    │    │    │    │
       └────┴────┴────┴────┴────┘
        top = -1
```

---

# 4. PUSH Operation

### Definition

**PUSH** means inserting an element into the stack.

The new element is always inserted at the **TOP**.

### Important condition

Before pushing, check whether the stack is full.

If:

```text
top == MAX - 1
```

then the stack is full.

This condition is called **Stack Overflow**.

---

## PUSH Algorithm

```text
PUSH(value)

1. Check if top == MAX - 1
2. If yes, print "Stack Overflow"
3. Otherwise:
      top = top + 1
      stack[top] = value
```

---

## Example

Suppose:

```text
MAX = 5
top = -1
```

Push `10`.

### Step 1

```text
top = top + 1
```

Therefore:

```text
top = 0
```

### Step 2

```text
stack[0] = 10
```

Stack:

```text
Index:    0    1    2    3    4
         ┌────┬────┬────┬────┬────┐
         │ 10 │    │    │    │    │
         └────┴────┴────┴────┴────┘
           ↑
          TOP
```

Now push `20`.

```text
top = 1
stack[1] = 20
```

Stack:

```text
         ┌────┐
TOP →    │ 20 │
         ├────┤
         │ 10 │
         └────┘
```

Push `30`:

```text
         ┌────┐
         │ 30 │ ← TOP
         ├────┤
         │ 20 │
         ├────┤
         │ 10 │
         └────┘
```

---

# 5. POP Operation

### Definition

**POP** means removing the top element from the stack.

### Important condition

Before popping, check whether the stack is empty.

If:

```text
top == -1
```

then the stack is empty.

This condition is called **Stack Underflow**.

---

## POP Algorithm

```text
POP()

1. Check if top == -1
2. If yes, print "Stack Underflow"
3. Otherwise:
      value = stack[top]
      top = top - 1
      return value
```

---

## Example

Suppose stack is:

```text
       30 ← TOP
       20
       10
```

Perform:

```text
POP()
```

The top element is:

```text
30
```

So `30` is removed.

Stack becomes:

```text
       20 ← TOP
       10
```

---

# 6. PUSH and POP Example — Complete Solving

Consider:

```text
PUSH(10)
PUSH(20)
PUSH(30)
POP()
PUSH(40)
POP()
```

Let's solve step-by-step.

### Step 1: PUSH(10)

```text
10 ← TOP
```

### Step 2: PUSH(20)

```text
20 ← TOP
10
```

### Step 3: PUSH(30)

```text
30 ← TOP
20
10
```

### Step 4: POP()

Remove `30`.

```text
20 ← TOP
10
```

### Step 5: PUSH(40)

```text
40 ← TOP
20
10
```

### Step 6: POP()

Remove `40`.

Final stack:

```text
20 ← TOP
10
```

### Popped elements

```text
30, 40
```

---

# 7. C++ Program — Stack Using Array

```cpp
#include <iostream>
using namespace std;

#define MAX 5

class Stack {
    int arr[MAX];
    int top;

public:
    Stack() {
        top = -1;
    }

    void push(int value) {
        if (top == MAX - 1) {
            cout << "Stack Overflow\n";
            return;
        }

        top++;
        arr[top] = value;
    }

    void pop() {
        if (top == -1) {
            cout << "Stack Underflow\n";
            return;
        }

        cout << "Popped: " << arr[top] << endl;
        top--;
    }

    void peek() {
        if (top == -1) {
            cout << "Stack is empty\n";
            return;
        }

        cout << "Top element: " << arr[top] << endl;
    }
};

int main() {
    Stack s;

    s.push(10);
    s.push(20);
    s.push(30);

    s.peek();

    s.pop();
    s.pop();

    return 0;
}
```

---

# 8. Applications of Stack

Stacks are widely used in computer science.

Important applications are:

1. Managing function calls
2. Recursion
3. Factorial calculation
4. Tower of Hanoi
5. Infix to postfix conversion
6. Postfix expression evaluation
7. Balanced parentheses checking
8. Backtracking
9. Undo/Redo
10. Browser Back/Forward
11. Matching HTML tags

Let's understand each one.

---

# 9. Stack in Managing Function Calls

Whenever a function is called, the computer needs to remember information about that function.

This information is stored in a special area called the **Call Stack**.

### Example

```cpp
void C() {
}

void B() {
    C();
}

void A() {
    B();
}

int main() {
    A();
}
```

Execution:

```text
main()
   ↓
A()
   ↓
B()
   ↓
C()
```

The call stack looks like:

```text
       C() ← TOP
       B()
       A()
       main()
```

When `C()` finishes:

```text
       B() ← TOP
       A()
       main()
```

When `B()` finishes:

```text
       A() ← TOP
       main()
```

Therefore, function calls follow **LIFO**.

### Why stack?

The last function called is normally the first function to finish.

---

# 10. Recursion

### Definition

**Recursion** is a technique in which a function calls itself.

A recursive function generally contains:

1. **Base case**
2. **Recursive case**

### Example

```cpp
factorial(n) = n × factorial(n-1)
```

Base case:

```text
factorial(0) = 1
```

---

# 11. Factorial Using Recursion

We know:

```text
5! = 5 × 4 × 3 × 2 × 1
```

Therefore:

```text
5! = 120
```

### Recursive formula

```text
factorial(n) = n × factorial(n-1)
```

Base case:

```text
factorial(0) = 1
```

### C++ Code

```cpp
int factorial(int n) {
    if (n == 0)
        return 1;

    return n * factorial(n - 1);
}
```

---

# 12. Solving factorial(4)

Let's calculate:

```text
factorial(4)
```

First:

```text
factorial(4)
= 4 × factorial(3)
```

Then:

```text
= 4 × 3 × factorial(2)
```

Then:

```text
= 4 × 3 × 2 × factorial(1)
```

Then:

```text
= 4 × 3 × 2 × 1 × factorial(0)
```

Since:

```text
factorial(0) = 1
```

Therefore:

```text
4! = 4 × 3 × 2 × 1
   = 24
```

---

## How Stack is Used

During the calls:

```text
factorial(4)
factorial(3)
factorial(2)
factorial(1)
factorial(0)
```

Stack becomes:

```text
factorial(0) ← TOP
factorial(1)
factorial(2)
factorial(3)
factorial(4)
```

When `factorial(0)` returns, calls are removed in reverse order.

```text
factorial(0) → return 1
factorial(1) → return 1
factorial(2) → return 2
factorial(3) → return 6
factorial(4) → return 24
```

This is another example of **LIFO**.

---

# 13. Tower of Hanoi

### Definition

**Tower of Hanoi** is a recursive problem involving three rods and `n` disks.

The three rods are generally named:

```text
A = Source
B = Auxiliary
C = Destination
```

### Rules

1. Only one disk can be moved at a time.
2. Only the top disk can be moved.
3. A larger disk cannot be placed on a smaller disk.

### For 3 disks

Initial:

```text
A        B        C

███
█████
███████
```

Goal:

```text
A        B        C

                  ███
                 █████
                ███████
```

---

# 14. Tower of Hanoi Formula

For `n` disks:

```text
T(n) = 2T(n-1) + 1
```

Number of moves:

```text
2ⁿ - 1
```

### Example

For 1 disk:

```text
2¹ - 1 = 1
```

For 2 disks:

```text
2² - 1 = 3
```

For 3 disks:

```text
2³ - 1 = 7
```

For 4 disks:

```text
2⁴ - 1 = 15
```

---

# 15. Tower of Hanoi — 3 Disks Solving

Move disks from `A` to `C`.

### Move 1

```text
A → C
```

### Move 2

```text
A → B
```

### Move 3

```text
C → B
```

### Move 4

```text
A → C
```

### Move 5

```text
B → A
```

### Move 6

```text
B → C
```

### Move 7

```text
A → C
```

Total:

```text
7 moves
```

---

# 16. Infix Expression

An **infix expression** is an expression where the operator is written between operands.

Examples:

```text
A + B
A - B
A * B
A + B * C
```

Example:

```text
A + B
```

Here `+` is between `A` and `B`.

---

# 17. Postfix Expression

In **postfix notation**, the operator comes after the operands.

Example:

```text
A + B
```

becomes:

```text
AB+
```

Another example:

```text
A + B * C
```

becomes:

```text
ABC*+
```

Postfix expressions are useful because they don't require parentheses or precedence rules during evaluation.

---

# 18. Infix to Postfix Using Stack

We use a stack to temporarily store **operators**.

### Operator precedence

Remember:

```text
^       highest
* / %
+ -     lowest
```

For example:

```text
A + B * C
```

`*` has higher precedence than `+`.

Therefore:

```text
A + (B * C)
```

Postfix:

```text
ABC*+
```

---

# 19. Rules for Infix to Postfix

Read the expression from **left to right**.

### Rule 1 — Operand

If it is an operand such as:

```text
A, B, C, 1, 2, 3
```

put it directly into the postfix expression.

### Rule 2 — `(`

Push it onto the stack.

### Rule 3 — `)`

Pop operators until `(` is found.

Remove the `(`.

### Rule 4 — Operator

Compare its precedence with the top of stack.

Pop higher/equal-precedence operators before pushing the new operator, subject to associativity.

### Rule 5 — End

Pop all remaining operators.

---

# 20. Example: Convert `A+B*C` to Postfix

Expression:

```text
A + B * C
```

Let's make a table.

| Symbol | Stack | Postfix |
| ------ | ----- | ------- |
| A      | —     | A       |
| +      | +     | A       |
| B      | +     | AB      |
| *      | + *   | AB      |
| C      | + *   | ABC     |
| End    | —     | ABC*+   |

Final answer:

```text
ABC*+
```

---

# 21. Example: `(A+B)*C`

Let's solve.

| Symbol | Stack | Postfix |
| ------ | ----- | ------- |
| `(`    | `(`   |         |
| A      | `(`   | A       |
| +      | `( +` | A       |
| B      | `( +` | AB      |
| `)`    | —     | AB+     |
| *      | *     | AB+     |
| C      | *     | AB+C    |
| End    | —     | AB+C*   |

### Answer

```text
AB+C*
```

---

# 22. Postfix Expression Evaluation

A stack is also used to **evaluate postfix expressions**.

### Rules

Read from left to right.

### If operand

Push it onto the stack.

### If operator

1. Pop the second operand.
2. Pop the first operand.
3. Apply the operator.
4. Push the result back.

---

# 23. Example: Evaluate `23*5+`

Postfix:

```text
2 3 * 5 +
```

### Step 1

Read `2`.

Push:

```text
2
```

### Step 2

Read `3`.

Push:

```text
3
2
```

### Step 3

Read `*`.

Pop:

```text
3
2
```

Calculate:

```text
2 × 3 = 6
```

Push `6`.

```text
6
```

### Step 4

Read `5`.

Push:

```text
5
6
```

### Step 5

Read `+`.

Calculate:

```text
6 + 5 = 11
```

Final:

```text
11
```

Therefore:

```text
23*5+ = 11
```

---

# 24. More Important Postfix Example

Evaluate:

```text
5 6 2 + * 12 4 / -
```

Let's solve.

| Symbol | Operation | Stack     |
| ------ | --------- | --------- |
| 5      | Push      | 5         |
| 6      | Push      | 5, 6      |
| 2      | Push      | 5, 6, 2   |
| +      | 6+2=8     | 5, 8      |
| *      | 5×8=40    | 40        |
| 12     | Push      | 40, 12    |
| 4      | Push      | 40, 12, 4 |
| /      | 12÷4=3    | 40, 3     |
| -      | 40-3=37   | 37        |

### Answer

```text
37
```

---

# 25. Balanced Parentheses

### Definition

Balanced parentheses means every opening bracket has a corresponding closing bracket in the correct order.

Examples of balanced expressions:

```text
()
{}
[]
```

and:

```text
{[()]}
```

Unbalanced:

```text
([)]
```

because the brackets are incorrectly matched.

---

# 26. Balanced Parentheses Using Stack

We use a stack.

### Rules

Read characters from left to right.

### Opening bracket

For:

```text
(
[
{
```

push it onto the stack.

### Closing bracket

For:

```text
)
]
}
```

check the top of the stack.

If it matches, pop it.

If it doesn't match, the expression is unbalanced.

### At the end

If stack is empty:

```text
Balanced
```

Otherwise:

```text
Not Balanced
```

---

# 27. Example: `{[()]}`

Read:

```text
{
[
(
)
]
}
```

Stack operations:

```text
{       → PUSH
{ [     → PUSH
{ [ (   → PUSH
{ [     → POP (
{       → POP [
empty   → POP {
```

Stack becomes empty.

Therefore:

```text
{[()]} = Balanced
```

---

# 28. Example: `{[(])}`

Read:

```text
{
[
(
]
```

When `]` appears, top is:

```text
(
```

But `]` should match:

```text
[
```

They don't match.

Therefore:

```text
{[(])} = Not Balanced
```

---

# 29. Backtracking

### Definition

**Backtracking** is an algorithmic technique where we try one possible solution and, if it doesn't work, we **go back to a previous state** and try another possibility.

A stack is useful because the most recent decision is undone first.

This follows:

```text
LIFO
```

### Examples

Backtracking is used in:

* Maze solving
* N-Queens
* Sudoku
* Finding paths
* Puzzle solving

---

# 30. Maze Example

Imagine:

```text
S → → ↓
      ↓
      → X
```

Suppose we reach a dead end.

We need to go back to the previous position.

The path can be stored in a stack:

```text
Start
 ↓
Position 1
 ↓
Position 2
 ↓
Position 3
```

If Position 3 is a dead end:

```text
POP Position 3
```

Go back to Position 2.

Then try another direction.

### Main idea

```text
Make choice
     ↓
Store choice in stack
     ↓
Continue
     ↓
Dead end?
     ↓
POP previous choice
     ↓
Try another choice
```

---

# 31. Undo and Redo Operations

Stack is commonly used to implement **Undo/Redo**.

Usually two stacks are used:

```text
Undo Stack
Redo Stack
```

### Example

Suppose you type:

```text
A
B
C
```

Undo stack:

```text
C ← TOP
B
A
```

Press **Undo**.

`C` is removed from the undo stack and placed into the redo stack.

```text
Undo Stack          Redo Stack

B ← TOP             C ← TOP
A
```

Press **Undo** again:

```text
Undo Stack          Redo Stack

A ← TOP             B ← TOP
                    C
```

Press **Redo**:

```text
Undo Stack          Redo Stack

B ← TOP             C ← TOP
A
```

Thus, stacks naturally support undo/redo.

---

# 32. Browser Back and Forward Buttons

Web browsers can also use stacks.

Usually:

```text
Back Stack
Forward Stack
```

Suppose you visit:

```text
Google
   ↓
YouTube
   ↓
Wikipedia
```

Back stack:

```text
Wikipedia ← TOP
YouTube
Google
```

Click **Back**.

Wikipedia is removed from the current history stack and placed into the forward stack.

Now you can use **Forward** to return.

### Example

```text
Google → YouTube → Wikipedia
```

Click Back:

```text
Current page = YouTube
Forward = Wikipedia
```

Click Back again:

```text
Current page = Google
Forward = YouTube, Wikipedia
```

Click Forward:

```text
Current page = YouTube
```

This is a practical application of stacks.

---

# 33. Matching HTML Tags

Stacks are also useful for checking whether HTML/XML tags are properly nested.

For example:

```html
<html>
    <body>
        <h1>Hello</h1>
    </body>
</html>
```

The tags are properly nested.

---

# 34. HTML Tag Matching Using Stack

Whenever we see an **opening tag**, push it.

Example:

```html
<html>
```

Push:

```text
html
```

Then:

```html
<body>
```

Push:

```text
body
html
```

Then:

```html
<h1>
```

Push:

```text
h1
body
html
```

When:

```html
</h1>
```

appears:

```text
POP h1
```

Then:

```html
</body>
```

matches `body`.

Finally:

```html
</html>
```

matches `html`.

Stack becomes empty.

Therefore, the HTML tags are properly nested.

---

# 35. Incorrect HTML Example

Consider:

```html
<html>
<body>
<h1>
</body>
</h1>
</html>
```

After reading:

```html
<html>
<body>
<h1>
```

stack:

```text
h1 ← TOP
body
html
```

Now we get:

```html
</body>
```

But top is:

```text
h1
```

It should be `</h1>` first.

Therefore, the tags are **not properly nested**.

---

# 36. Why Stack is Used in All These Applications?

The common idea is **LIFO**.

| Application        | Why Stack?                                    |
| ------------------ | --------------------------------------------- |
| Function calls     | Last called function returns first            |
| Recursion          | Last recursive call returns first             |
| Factorial          | Function calls return in reverse order        |
| Tower of Hanoi     | Recursive calls need call stack               |
| Infix → Postfix    | Operators are temporarily stored              |
| Postfix evaluation | Operands/results are processed LIFO           |
| Parentheses        | Most recent opening bracket must close first  |
| Backtracking       | Most recent decision is undone first          |
| Undo               | Most recent action is undone first            |
| Redo               | Most recently undone action is restored first |
| Browser Back       | Most recent page is visited backward first    |
| HTML tags          | Most recent opening tag must close first      |

---

# 37. Important Stack Terms for Exam

### Stack Overflow

When we try to **PUSH into a full stack**.

```text
Stack is full
      ↓
PUSH
      ↓
OVERFLOW
```

### Stack Underflow

When we try to **POP from an empty stack**.

```text
Stack is empty
      ↓
POP
      ↓
UNDERFLOW
```

### TOP

`TOP` points to the most recently inserted element.

### LIFO

```text
Last In → First Out
```

---

# 38. Time Complexity of Stack Operations

For an array implementation:

| Operation | Time Complexity |
| --------- | --------------: |
| PUSH      |            O(1) |
| POP       |            O(1) |
| PEEK      |            O(1) |
| isEmpty   |            O(1) |
| isFull    |            O(1) |

Why?

Because we directly access the `top` position.

For example:

```cpp
top++;
```

or:

```cpp
top--;
```

takes constant time.

---

# 39. One Complete Example for Practice

Consider the following operations:

```text
PUSH(10)
PUSH(20)
PUSH(30)
POP()
PUSH(40)
PUSH(50)
POP()
POP()
```

Let's solve.

| Operation | Stack          |
| --------- | -------------- |
| PUSH(10)  | 10             |
| PUSH(20)  | 10, 20         |
| PUSH(30)  | 10, 20, 30     |
| POP()     | 10, 20         |
| PUSH(40)  | 10, 20, 40     |
| PUSH(50)  | 10, 20, 40, 50 |
| POP()     | 10, 20, 40     |
| POP()     | 10, 20         |

### Final stack

```text
       20 ← TOP
       10
```

### Popped elements

```text
30, 50, 40
```

---

# 40. Quick Revision — Stack

```text
                 STACK
                   │
             ┌─────┴─────┐
             │           │
            LIFO       TOP
             │
       Last In First Out
             │
     ┌───────┼────────┐
     ↓       ↓        ↓
   PUSH     POP      PEEK
     │       │        │
   Insert   Delete   View
```

### Applications

```text
Stack
 │
 ├── Function Calls
 ├── Recursion
 │    ├── Factorial
 │    └── Tower of Hanoi
 │
 ├── Infix → Postfix
 ├── Postfix Evaluation
 ├── Balanced Parentheses
 ├── Backtracking
 ├── Undo / Redo
 ├── Browser Back / Forward
 └── HTML Tag Matching
```

### Most important formulas/rules

```text
LIFO = Last In First Out

Empty stack:
top = -1

Full array stack:
top = MAX - 1

Overflow:
PUSH on full stack

Underflow:
POP on empty stack

Tower of Hanoi:
Moves = 2ⁿ - 1

Factorial:
n! = n × (n-1)!

Postfix:
Operands → output
Operators → stack
```

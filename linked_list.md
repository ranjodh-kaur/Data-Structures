# Unit 5: Linked Lists

## 1. What is a Linked List?

A **linked list** is a linear data structure in which data is stored in separate objects called **nodes**.

Each node generally contains:

1. **Data** – the actual value
2. **Link/Pointer** – address of the next node

For example:

```text
10        20        30
┌──────┐  ┌──────┐  ┌──────┐
│ 10   │  │ 20   │  │ 30   │
│  next├─→│  next├─→│ next ├─→ NULL
└──────┘  └──────┘  └──────┘
```

The last node points to `NULL`.

### Array vs Linked List

**Array:**

```text
10 | 20 | 30 | 40
```

Elements are stored in **continuous memory locations**.

**Linked List:**

```text
10 → 20 → 30 → 40 → NULL
```

Nodes can be stored anywhere in memory. Pointers connect them.

### Why do we need linked lists?

Arrays have a major limitation: their size is usually fixed.

For example:

```cpp
int a[5];
```

This array can store only 5 elements.

A linked list can grow and shrink dynamically.

---

# 2. Node of a Linked List

In C++, we create a node using a `struct`.

```cpp
struct Node
{
    int data;
    Node* next;
};
```

Here:

```cpp
int data;
```

stores the value.

```cpp
Node* next;
```

stores the address of the next node.

For example:

```text
       Node
   ┌───────────┐
   │ data = 10 │
   │ next ─────┼────→ another node
   └───────────┘
```

---

# 3. Implementation of Linear/Singly Linked List

A **linear linked list** is also commonly called a **singly linked list**.

It looks like:

```text
HEAD
 ↓
10 → 20 → 30 → 40 → NULL
```

There is only one direction:

```text
→
```

---

## Creating a Node

```cpp
struct Node
{
    int data;
    Node* next;
};
```

We can create a node dynamically:

```cpp
Node* newNode = new Node;

newNode->data = 10;
newNode->next = NULL;
```

Now:

```text
newNode
   ↓
┌─────────────┐
│ data = 10   │
│ next = NULL │
└─────────────┘
```

---

# 4. Traversal of Linked List

**Traversal** means visiting every node one by one.

Suppose:

```text
HEAD
 ↓
10 → 20 → 30 → NULL
```

We start from `HEAD`.

```cpp
Node* temp = head;

while (temp != NULL)
{
    cout << temp->data << " ";
    temp = temp->next;
}
```

### Working

Initially:

```text
temp
 ↓
10 → 20 → 30 → NULL
```

Print:

```text
10
```

Then:

```cpp
temp = temp->next;
```

Now:

```text
10 → temp → 20 → 30 → NULL
```

Print:

```text
20
```

Then:

```text
10 → 20 → temp → 30 → NULL
```

Print:

```text
30
```

Finally:

```text
temp = NULL
```

Loop stops.

Output:

```text
10 20 30
```

---

# 5. Insertion in Linear Linked List

Insertion means **adding a new node**.

There are three important cases:

1. Insertion at beginning
2. Insertion at end
3. Insertion at a specific position

---

## A. Insertion at Beginning

Suppose we have:

```text
HEAD
 ↓
20 → 30 → 40 → NULL
```

We want to insert `10`.

First create:

```text
10
```

Then connect it to the old first node:

```text
10 → 20 → 30 → 40 → NULL
```

Finally make `head` point to 10:

```text
HEAD
 ↓
10 → 20 → 30 → 40 → NULL
```

### Code

```cpp
newNode->next = head;
head = newNode;
```

### Why these two statements?

Initially:

```text
newNode → 10

head → 20
```

After:

```cpp
newNode->next = head;
```

we get:

```text
newNode
   ↓
  10 ─────→ 20
            ↑
           head
```

Then:

```cpp
head = newNode;
```

Now:

```text
HEAD
 ↓
10 → 20 → 30 → NULL
```

---

# 6. Insertion at End

Suppose:

```text
HEAD
 ↓
10 → 20 → 30 → NULL
```

We want:

```text
40
```

We first reach the last node.

```text
10 → 20 → 30 → NULL
          ↑
        last
```

Then:

```cpp
last->next = newNode;
```

Result:

```text
10 → 20 → 30 → 40 → NULL
```

### Code

```cpp
Node* temp = head;

while (temp->next != NULL)
{
    temp = temp->next;
}

temp->next = newNode;
newNode->next = NULL;
```

---

# 7. Insertion at a Specific Position

Suppose:

```text
10 → 20 → 40 → NULL
```

We want to insert `30` between 20 and 40.

Before:

```text
10 → 20 → 40
```

Create node:

```text
30
```

We need:

```text
20 → 30 → 40
```

The important statements are:

```cpp
newNode->next = temp->next;
temp->next = newNode;
```

Result:

```text
10 → 20 → 30 → 40 → NULL
```

---

# 8. Deletion in Linear Linked List

Deletion means removing a node.

Three common cases:

1. Delete from beginning
2. Delete from end
3. Delete a specific node

---

## A. Delete from Beginning

Before:

```text
HEAD
 ↓
10 → 20 → 30 → NULL
```

We want to delete `10`.

Simply move `head` to the second node:

```cpp
head = head->next;
```

Now:

```text
HEAD
 ↓
20 → 30 → NULL
```

The old node can then be deleted:

```cpp
delete temp;
```

---

## B. Delete from End

Before:

```text
10 → 20 → 30 → NULL
```

We reach the second-last node:

```text
10 → 20 → 30 → NULL
     ↑
    temp
```

Then:

```cpp
temp->next = NULL;
```

and delete the old last node.

Result:

```text
10 → 20 → NULL
```

---

## C. Delete a Specific Node

Suppose:

```text
10 → 20 → 30 → 40 → NULL
```

Delete `30`.

We find the previous node:

```text
10 → 20 → 30 → 40
     ↑
    temp
```

Then:

```cpp
temp->next = temp->next->next;
```

So:

```text
10 → 20 ─────→ 40 → NULL
```

Node `30` is removed from the chain.

---

# 9. Complete Singly Linked List Program

```cpp
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

Node* head = NULL;

// Insert at beginning
void insertBeginning(int value)
{
    Node* newNode = new Node;

    newNode->data = value;
    newNode->next = head;

    head = newNode;
}

// Insert at end
void insertEnd(int value)
{
    Node* newNode = new Node;

    newNode->data = value;
    newNode->next = NULL;

    if (head == NULL)
    {
        head = newNode;
        return;
    }

    Node* temp = head;

    while (temp->next != NULL)
    {
        temp = temp->next;
    }

    temp->next = newNode;
}

// Display
void display()
{
    Node* temp = head;

    while (temp != NULL)
    {
        cout << temp->data << " ";
        temp = temp->next;
    }

    cout << endl;
}

// Delete from beginning
void deleteBeginning()
{
    if (head == NULL)
        return;

    Node* temp = head;

    head = head->next;

    delete temp;
}

// Delete from end
void deleteEnd()
{
    if (head == NULL)
        return;

    if (head->next == NULL)
    {
        delete head;
        head = NULL;
        return;
    }

    Node* temp = head;

    while (temp->next->next != NULL)
    {
        temp = temp->next;
    }

    delete temp->next;

    temp->next = NULL;
}

int main()
{
    insertBeginning(20);
    insertBeginning(10);

    insertEnd(30);
    insertEnd(40);

    cout << "Linked List: ";
    display();

    deleteBeginning();

    cout << "After deleting beginning: ";
    display();

    deleteEnd();

    cout << "After deleting end: ";
    display();

    return 0;
}
```

---

# 10. Circular Linked List

In a normal linked list:

```text
10 → 20 → 30 → NULL
```

The last node points to `NULL`.

In a **circular linked list**, the last node points back to the first node.

```text
       ┌────────────────────┐
       ↓                    │
10 → 20 → 30 → 40 ──────────┘
↑
HEAD
```

There is **no NULL at the end**.

### Important difference

Linear:

```text
10 → 20 → 30 → NULL
```

Circular:

```text
10 → 20 → 30
↑         ↓
└─────────┘
```

---

## Traversing Circular Linked List

We cannot use:

```cpp
while(temp != NULL)
```

because `temp` will never become `NULL`.

Instead:

```cpp
do
{
    cout << temp->data << " ";
    temp = temp->next;

} while(temp != head);
```

We stop when we reach `head` again.

---

# 11. Doubly Linked List

A singly linked list has:

```text
data | next
```

A doubly linked list has:

```text
prev | data | next
```

Example:

```text
NULL ← 10 ⇄ 20 ⇄ 30 → NULL
```

Each node has **two links**:

- `prev` → previous node
- `next` → next node

### Structure

```cpp
struct Node
{
    int data;
    Node* prev;
    Node* next;
};
```

### Advantage

We can move:

```text
forward →
```

and

```text
← backward
```

### Example

```text
        forward
           →
10 ⇄ 20 ⇄ 30
           ←
        backward
```

---

# 12. Skip List

A **skip list** is a special linked-list-based data structure designed to make searching faster.

A normal linked list:

```text
10 → 20 → 30 → 40 → 50 → 60 → 70 → 80
```

If we search for `70`, we may have to check:

```text
10
20
30
40
50
60
70
```

That's slow for a large list.

A skip list adds extra levels.

```text
Level 2:

10 ─────────→ 30 ─────────→ 50 ─────────→ 70

Level 1:

10 ──→ 20 ──→ 30 ──→ 40 ──→ 50 ──→ 60 ──→ 70

Level 0:

10 → 20 → 30 → 40 → 50 → 60 → 70 → 80
```

When searching for `70`, we can **jump over many elements**.

This is why it is called a **skip list**.

### Searching idea

Suppose we want `70`.

Start at:

```text
10
```

At upper level:

```text
10 → 30 → 50 → 70
```

We jump:

```text
10 → 30 → 50 → 70
```

Instead of checking every element.

### Complexity

Average search:

```text
O(log n)
```

Average insertion:

```text
O(log n)
```

Average deletion:

```text
O(log n)
```

Worst case can become:

```text
O(n)
```

---

# 13. Stack Using Linked List

A **stack** follows:

> **LIFO — Last In, First Out**

Example:

```text
       30 ← TOP
       20
       10
```

If we remove one element:

```text
30
```

comes out first.

### Real-life example

Think of plates:

```text
Plate 3 ← first removed
Plate 2
Plate 1
```

The last plate placed is the first plate removed.

---

## Stack Operations

Two main operations:

### Push

Add an element.

```text
10
```

Then:

```text
20
10
```

Then:

```text
30
20
10
```

### Pop

Remove the top element.

```text
30
20
10
```

After pop:

```text
20
10
```

---

## Stack Using Linked List

We use the **head as TOP**.

```text
TOP
 ↓
30 → 20 → 10 → NULL
```

### Push

```cpp
newNode->next = top;
top = newNode;
```

### Pop

```cpp
Node* temp = top;
top = top->next;
delete temp;
```

---

# 14. Queue Using Linked List

A **queue** follows:

> **FIFO — First In, First Out**

Real-life example:

```text
Person 1 → Person 2 → Person 3
```

Person 1 comes first, so person 1 is served first.

```text
FRONT                    REAR
 ↓                         ↓
10 → 20 → 30 → 40 → NULL
```

### Queue operations

**Enqueue** = insert

**Dequeue** = delete

---

## Enqueue

Add at the rear:

```text
10 → 20 → 30
             ↓
            40
```

Result:

```text
10 → 20 → 30 → 40
```

## Dequeue

Remove from the front:

```text
10 → 20 → 30 → 40
↑
remove
```

Result:

```text
20 → 30 → 40
```

---

# 15. Stack vs Queue Using Linked List

| Feature | Stack | Queue |
|---|---|---|
| Principle | LIFO | FIFO |
| Insertion | Push | Enqueue |
| Deletion | Pop | Dequeue |
| Main pointer(s) | Top | Front + Rear |
| Example | Stack of plates | Waiting line |
| First removed | Last inserted | First inserted |

---

# 16. Important Comparison of Linked Lists

| Type | Structure | Movement |
|---|---|---|
| Singly/Linear | `10 → 20 → 30 → NULL` | Forward |
| Circular | `10 → 20 → 30 → 10` | Forward, continuously |
| Doubly | `10 ⇄ 20 ⇄ 30` | Forward + backward |
| Skip List | Multiple linked levels | Fast searching |

---

# 17. Time Complexity You Should Remember

For a **singly linked list**:

| Operation | Time |
|---|---:|
| Access/search by position | O(n) |
| Insert at beginning | O(1) |
| Delete at beginning | O(1) |
| Insert at end | O(n)* |
| Delete at end | O(n) |
| Search | O(n) |

\*If we maintain a `tail` pointer, insertion at the end can be **O(1)**.

For a **skip list**, average search/insert/delete is approximately:

```text
O(log n)
```

---

# 18. Very Important Exam Understanding

You can remember the whole topic like this:

```text
                    LINKED LIST
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
       Singly         Circular       Doubly
          │              │              │
       → → →          ↺ cycle         ⇄ ⇄ ⇄
          │
          ↓
    Insertion / Deletion
          │
          ↓
      Skip List
          │
          ↓
   Faster Searching
```

And linked lists can also be used to implement:

```text
              Linked List
                   │
             ┌─────┴─────┐
             ↓           ↓
           Stack       Queue
            LIFO         FIFO
```

**The most important pointer concepts to master first are:** `head`, `next`, `prev`, `top`, `front`, and `rear`. Once these are clear, the programs become much easier.


________

Yes. If you want to **create an empty linked list**, initially we simply make `head = NULL`.

### Simplest program

```cpp
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

int main()
{
    // Create an empty linked list
    Node* head = NULL;

    cout << "Linked list is empty.";

    return 0;
}
```

### What does this mean?

```cpp
Node* head = NULL;
```

It means **there is currently no node in the list**.

```text
HEAD
 ↓
NULL
```

There is no `10`, `20`, or `30` yet.

### Then if we want to insert 10

We create a node:

```cpp
Node* newNode = new Node;

newNode->data = 10;
newNode->next = NULL;

head = newNode;
```

Now the empty list becomes:

```text
HEAD
 ↓
10 → NULL
```

So remember:

```text
Empty list:

head → NULL

After inserting 10:

head → 10 → NULL
```

If you want, I can next show you **a very simple program where the user enters 3 nodes one by one and creates the linked list**.

____

Yes. If you want to **create only the first node** of a linked list, keep it very simple:

```cpp id="71384"
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

int main()
{
    Node* head = new Node;

    head->data = 10;
    head->next = NULL;

    cout << head->data;

    return 0;
}
```

### What happens?

First:

```cpp
Node* head = new Node;
```

A new node is created:

```text
head
 ↓
┌─────────────┐
│             │
│             │
└─────────────┘
```

Then:

```cpp
head->data = 10;
```

stores `10`:

```text
head
 ↓
┌─────────────┐
│ data = 10   │
│             │
└─────────────┘
```

Then:

```cpp
head->next = NULL;
```

means this is currently the **last/only node**:

```text
HEAD
 ↓
10 → NULL
```

So the first node is created.
________

Yes. Starting from the **first node**, let's create the **second node** only.

```cpp id="87135"
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

int main()
{
    // First node
    Node* head = new Node;
    head->data = 10;
    head->next = NULL;

    // Second node
    Node* newNode = new Node;
    newNode->data = 20;
    newNode->next = NULL;

    // Connect first node to second node
    head->next = newNode;

    // Display
    cout << head->data << " ";
    cout << head->next->data;

    return 0;
}
```

### Step by step

Initially:

```text
HEAD
 ↓
10 → NULL
```

Create second node:

```cpp
Node* newNode = new Node;

newNode->data = 20;
newNode->next = NULL;
```

Now there are **two separate nodes**:

```text
HEAD
 ↓
10 → NULL

newNode
   ↓
  20 → NULL
```

Now connect them:

```cpp
head->next = newNode;
```

Result:

```text
HEAD
 ↓
10 → 20 → NULL
```

So the **important line for connecting the second node** is:

```cpp
head->next = newNode;
```

This means:

> "The next node after the first node is `newNode`."

_______

1. Circular Linked List
2. Doubly Linked List
3. Skip List — efficient searching
4. Stack using Linked List
5. Queue using Linked List

# 1. Implementation of Circular Linked List

## 1.1 What is a circular linked list?

In a normal singly linked list, the last node points to `NULL`.

```
10 → 20 → 30 → NULL
```

In a circular linked list, the last node points back to the first node.

The last node links back to the first node.

### Where is it used in real life?

Imagine players standing in a circle. After the last player takes a turn, the first player takes another turn.

Examples include:

- Round-robin CPU scheduling.
- Repeating music playlists.
- Turn-based games.
- Cycling through items repeatedly.

## 1.2 How do we create a circular linked list?

We use the same node structure:

```
struct Node
{
    int data;
    Node* next;
};
```

Suppose we want three nodes containing `10`, `20`, and `30`.

### Step 1: Create the first node

```
Node* head = new Node;
head->data = 10;
head->next = head;
```

Initially:

```
     ┌──────────┐
     ↓          │
    10 ─────────┘
```

Why `head->next = head`?

Because there is only one node, so it points back to itself.

### Step 2: Create the second node

```
Node* second = new Node;
second->data = 20;
second->next = head;
head->next = second;
```

Now:

```
10 → 20
↑     │
└─────┘
```

The new node points to the first node, and the first node points to the second node.

### Step 3: Create the third node

```
Node* third = new Node;
third->data = 30;
third->next = head;
second->next = third;
```

Now:

```
10 → 20 → 30
↑          │
└──────────┘
```

This is a circular linked list containing three nodes.

## 1.3 Complete C++ program

This program creates three nodes and displays them.

```

#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

int main()
{
    // Create first node
    Node* head = new Node;
    head->data = 10;
    head->next = head;

    // Create second node
    Node* second = new Node;
    second->data = 20;
    second->next = head;
    head->next = second;

    // Create third node
    Node* third = new Node;
    third->data = 30;
    third->next = head;
    second->next = third;

    // Display circular linked list
    Node* temp = head;

    do
    {
        cout << temp->data << " ";
        temp = temp->next;
    }
    while (temp != head);

    return 0;
}

```

Output:

```
10 20 30
```

### 1.4 Why do we use `do-while`?

In a circular linked list, the last node never points to `NULL`. Therefore, this loop is unsuitable:

```
while (temp != NULL)
```

It would keep running around the circle forever.

Instead, we stop when `temp` reaches `head` again.

```
do
{
    cout << temp->data << " ";
    temp = temp->next;
}
while (temp != head);
```

The steps are:

| Step | `temp` points to | Printed |
| ---- | ---------------- | ------- |
| 1    | First node       | 10      |
| 2    | Second node      | 20      |
| 3    | Third node       | 30      |
| 4    | First node again | Stop    |

Important: A circular list can also be empty, with `head = NULL`. Always handle that case before starting traversal.

# 2. Implementation of Doubly Linked List

## 2.1 What is a doubly linked list?

A singly linked list contains a pointer to the next node.

```
10 → 20 → 30 → NULL
```

A doubly linked list contains two pointers in each node:

- `prev` points to the previous node.
- `next` points to the next node.

```
NULL ← 10 ⇄ 20 ⇄ 30 → NULL
```

Here, we can move both forward and backward.

### Real-life example

Think of a web browser:

- Click Next to move forward.
- Click Back to return to the previous page.

A doubly linked list is useful for representing this type of navigation.

## 2.2 Node structure

```
struct Node
{
    int data;
    Node* prev;
    Node* next;
};
```

Compare the two structures:

| Singly linked list  | Doubly linked list             |
| ------------------- | ------------------------------ |
| `data`              | `data`                         |
| `next`              | `prev`                         |
| No previous pointer | `next`                         |
| Forward traversal   | Forward and backward traversal |

A doubly linked list uses more memory because every node stores two pointers.

## 2.3 Creating two connected nodes

Suppose we want:

```
NULL ← 10 ⇄ 20 → NULL
```

### Step 1: Create the first node

```
Node* head = new Node;

head->data = 10;
head->prev = NULL;
head->next = NULL;
```

Initially:

```
NULL ← 10 → NULL
```

### Step 2: Create the second node

```
Node* second = new Node;

second->data = 20;
second->prev = head;
second->next = NULL;

head->next = second;
```

Now:

```
NULL ← 10 ⇄ 20 → NULL
```

Understand these two statements:

```
second->prev = head;
head->next = second;
```

- The first statement connects node 20 backward to node 10.
- The second statement connects node 10 forward to node 20.

Both connections are necessary.

## 2.4 Complete C++ program for doubly linked list

This program creates three nodes and displays the list in both directions.

```

#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* prev;
    Node* next;
};

int main()
{
    // First node
    Node* head = new Node;
    head->data = 10;
    head->prev = NULL;
    head->next = NULL;

    // Second node
    Node* second = new Node;
    second->data = 20;
    second->prev = head;
    second->next = NULL;
    head->next = second;

    // Third node
    Node* third = new Node;
    third->data = 30;
    third->prev = second;
    third->next = NULL;
    second->next = third;

    // Forward traversal
    cout << "Forward: ";
    Node* temp = head;

    while (temp != NULL)
    {
        cout << temp->data << " ";
        temp = temp->next;
    }

    // Backward traversal
    cout << "\nBackward: ";
    temp = third;

    while (temp != NULL)
    {
        cout << temp->data << " ";
        temp = temp->prev;
    }

    delete third;
    delete second;
    delete head;

    return 0;
}

```

Output:

```
Forward: 10 20 30
Backward: 30 20 10
```

### How does backward traversal work?

We start from the last node:

```
temp = third;
```

Then we follow:

```
temp = temp->prev;
```

The movement is:

```
30 → 20 → 10 → NULL
```

In memory, the links point backward from 30 to 20, then from 20 to 10.

Remember: Insertion and deletion in a doubly linked list require updating both `prev` and `next` links where applicable.

# 3. Efficient Searching Using a Skip List

## 3.1 What is a skip list?

A skip list is a linked-list-based data structure that adds extra levels of links to make searching faster.

First, consider a normal linked list:

```
10 → 20 → 30 → 40 → 50 → 60 → 70 → 80
```

Suppose you want to search for `70`.

You may have to check:

```
10 → 20 → 30 → 40 → 50 → 60 → 70
```

That means seven nodes are visited.

A skip list adds shortcut links.

Skip list example

Level 2

10 ─────→ 40 ─────→ 70

Level 1

10 → 20 → 40 → 50 → 70

Level 0

10 → 20 → 30 → 40 → 50 → 60 → 70 → 80

Level 0 contains all elements. Higher levels provide shortcuts over some elements.

The higher levels are additional links, not separate copies of the entire data structure.

## 3.2 How does searching work?

Suppose we want to find `60`.

Start at the highest available level.

1. Start at `10`.
2. Move to `40`, because `40` is less than `60`.
3. At the next shortcut, `70` is greater than `60`, so do not move to `70`.
4. Move down to the lower level.
5. Continue through `50`, then reach `60`.

The basic rule is:

- If the next value is smaller than the target, move forward.
- If the next value is greater than the target, move down a level.
- If the next value equals the target, the element is found.

## 3.3 Why is it faster?

| Structure            | Average search time |
| -------------------- | ------------------- |
| Ordinary linked list | \\(O(n)\\)          |
| Skip list            | \\(O(\log n)\\)     |

For example, searching a large list of about 1,024 elements may require checking hundreds of nodes in an ordinary linked list. A well-formed skip list can typically find the element in roughly a small number of levels and forward steps.

Skip lists usually use randomly selected node levels. Their performance depends on how those levels are built.

## 3.4 Simple C++ implementation of skip-list searching

To understand the main idea first, the following program creates a small, fixed skip list with two levels and searches for an element. This is a simplified demonstration, not a complete dynamically inserting skip-list implementation.

```

#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
    Node* down;
};

int main()
{
    // Bottom level: 10 -> 20 -> 30 -> 40 -> 50
    Node* n10 = new Node{10, NULL, NULL};
    Node* n20 = new Node{20, NULL, NULL};
    Node* n30 = new Node{30, NULL, NULL};
    Node* n40 = new Node{40, NULL, NULL};
    Node* n50 = new Node{50, NULL, NULL};

    n10->next = n20;
    n20->next = n30;
    n30->next = n40;
    n40->next = n50;

    // Upper level: 10 -> 30 -> 50
    Node* u10 = new Node{10, NULL, n10};
    Node* u30 = new Node{30, NULL, n30};
    Node* u50 = new Node{50, NULL, n50};

    u10->next = u30;
    u30->next = u50;

    int key = 40;
    Node* temp = u10;
    bool found = false;

    while (temp != NULL)
    {
        while (temp->next != NULL &&
               temp->next->data <= key)
        {
            temp = temp->next;

            if (temp->data == key)
            {
                found = true;
                break;
            }
        }

        if (found)
            break;

        if (temp->down != NULL)
            temp = temp->down;
        else
            break;
    }

    if (found)
        cout << "Element found";
    else
        cout << "Element not found";

    delete u10;
    delete u30;
    delete u50;
    delete n10;
    delete n20;
    delete n30;
    delete n40;
    delete n50;

    return 0;
}

```

Output:

```
Element found
```

### Understand the important pointers

- `next` moves horizontally to another value on the same level.
- `down` moves vertically to the corresponding node on a lower level.
- `key` is the value we are searching for.
- `found` tells us whether the value has been located.

In this example, the search starts at the upper-level node `10`. It can move to `30`, then descend to the lower-level node `30`, and finally reach `40`.

# 4. Implementation of Stack Using Linked List

## 4.1 What is a stack?

A stack follows the principle:

LIFO = Last In, First Out

Imagine a stack of plates. The plate placed last is removed first.

```
       TOP
        ↓
      ┌────┐
      │ 30 │ ← Last inserted
      ├────┤
      │ 20 │
      ├────┤
      │ 10 │ ← First inserted
      └────┘
```

The two main operations are:

- Push: insert an element.
- Pop: remove the top element.

Other useful operations include `peek` (view the top value) and `isEmpty` (check whether the stack is empty).

## 4.2 How do we implement a stack using a linked list?

We use the `head` pointer as `top`.

```
TOP
 ↓
30 → 20 → 10 → NULL
```

### Push operation

Suppose we push `40`.

First create a new node:

```
40 → NULL
```

Connect it to the old top:

```
newNode->next = top;
```

Move the top pointer:

```
top = newNode;
```

Result:

```
TOP
 ↓
40 → 30 → 20 → 10 → NULL
```

### Pop operation

Suppose we pop the stack.

```
TOP
 ↓
40 → 30 → 20 → 10 → NULL
```

Save the top node, then move `top` forward:

```
Node* temp = top;
top = top->next;
delete temp;
```

Result:

```
TOP
 ↓
30 → 20 → 10 → NULL
```

The value `40` has been removed.

## 4.3 Complete C++ program

```

#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

Node* top = NULL;

void push(int value)
{
    Node* newNode = new Node;

    newNode->data = value;
    newNode->next = top;

    top = newNode;
}

void pop()
{
    if (top == NULL)
    {
        cout << "Stack is empty\n";
        return;
    }

    Node* temp = top;

    cout << "Deleted: " << temp->data << endl;

    top = top->next;
    delete temp;
}

void display()
{
    Node* temp = top;

    cout << "Stack: ";

    while (temp != NULL)
    {
        cout << temp->data << " ";
        temp = temp->next;
    }

    cout << endl;
}

int main()
{
    push(10);
    push(20);
    push(30);

    display();

    pop();

    display();

    return 0;
}

```

Output:

```
Stack: 30 20 10
Deleted: 30
Stack: 20 10
```

### Dry run

| Operation  | Stack from top to bottom |
| ---------- | ------------------------ |
| Initially  | Empty                    |
| `push(10)` | 10                       |
| `push(20)` | 20, 10                   |
| `push(30)` | 30, 20, 10               |
| `pop()`    | 20, 10                   |

Notice that `push(30)` places 30 at the beginning. We do not need to travel to the end of the list.

Time complexity: Push = \\(O(1)\\), Pop = \\(O(1)\\).

# 5. Implementation of Queue Using Linked List

## 5.1 What is a queue?

A queue follows the principle:

FIFO = First In, First Out

Imagine people standing in a line to buy tickets. The person who arrives first gets served first.

```
FRONT                         REAR
  ↓                             ↓
┌────┐    ┌────┐    ┌────┐
│ 10 │ →  │ 20 │ →  │ 30 │ → NULL
└────┘    └────┘    └────┘
```

The two main operations are:

- Enqueue: insert an element at the rear.
- Dequeue: remove an element from the front.

## 5.2 Why do we need two pointers?

We use two pointers:

- `front` points to the first node.
- `rear` points to the last node.

Initially, the queue is empty:

```
front = NULL
rear  = NULL
```

Suppose we insert `10`.

```
front, rear
     ↓
   10 → NULL
```

Both pointers point to the same node because it is the only node.

Now insert `20`:

```
front             rear
  ↓                 ↓
 10 ─────────────→ 20 → NULL
```

Now insert `30`:

```
front                       rear
  ↓                           ↓
 10 ─────────→ 20 ─────────→ 30 → NULL
```

The front stays at 10, while the rear moves to 30.

## 5.3 Enqueue operation — insertion

Suppose the queue is:

```
front                 rear
  ↓                     ↓
 10 → 20 → NULL
```

We want to insert `30`.

First create a new node:

```
Node* newNode = new Node;
newNode->data = 30;
newNode->next = NULL;
```

Then connect the current rear to the new node:

```
rear->next = newNode;
```

Finally, update the rear pointer:

```
rear = newNode;
```

Result:

```
front                       rear
  ↓                           ↓
 10 → 20 → 30 → NULL
```

Important: If the queue is initially empty, both `front` and `rear` must point to the newly created node.

## 5.4 Dequeue operation — deletion

Suppose:

```
front                 rear
  ↓                     ↓
 10 → 20 → 30 → NULL
```

We remove `10`, which is at the front.

First save the front node:

```
Node* temp = front;
```

Move `front` to the next node:

```
front = front->next;
```

Delete the old front node:

```
delete temp;
```

Result:

```
front            rear
  ↓                ↓
 20 → 30 → NULL
```

### What if we delete the last remaining node?

Suppose the queue contains only:

```
front, rear
     ↓
    10 → NULL
```

After deleting 10, both pointers must become `NULL`.

```
if (front == NULL)
{
    rear = NULL;
}
```

Otherwise, `rear` would still point to a node that has been deleted.

## 5.5 Complete C++ program

```

#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

Node* front = NULL;
Node* rear = NULL;

void enqueue(int value)
{
    Node* newNode = new Node;

    newNode->data = value;
    newNode->next = NULL;

    if (front == NULL)
    {
        front = rear = newNode;
    }
    else
    {
        rear->next = newNode;
        rear = newNode;
    }
}

void dequeue()
{
    if (front == NULL)
    {
        cout << "Queue is empty\n";
        return;
    }

    Node* temp = front;

    cout << "Deleted: " << temp->data << endl;

    front = front->next;
    delete temp;

    if (front == NULL)
    {
        rear = NULL;
    }
}

void display()
{
    Node* temp = front;

    cout << "Queue: ";

    while (temp != NULL)
    {
        cout << temp->data << " ";
        temp = temp->next;
    }

    cout << endl;
}

int main()
{
    enqueue(10);
    enqueue(20);
    enqueue(30);

    display();

    dequeue();

    display();

    return 0;
}

```

Output:

```
Queue: 10 20 30
Deleted: 10
Queue: 20 30
```


# Unit-4 Queue 7(L) hrs
Implementation of linear queue using array, limitations of linear queue, importance of circular queue in terms of efficient storage of data, implementation of circular queue and performing operations of insertion and deletion in it. Concept of double-ended queue and its applications in palindrome checking, undo-redo operations, Adaptive-Steal (A-Steal) job-scheduling algorithm. Implementation of priority queue using one-way list and separate queue for each priority level.
________________


# 1. What is a Queue?

A **Queue** is a linear data structure that follows:

> **FIFO = First In, First Out**

The element inserted **first** is removed **first**.

### Real-life example

Think about people standing in a ticket line:

```text
Person A → Person B → Person C → Person D
   ↑                                 ↑
 FRONT                              REAR
```

Person A came first, so A will get the ticket first.

Therefore:

```text
ENQUEUE → Add element at REAR
DEQUEUE → Remove element from FRONT
```



---

# 2. Linear Queue Using Array

Suppose we create an array of size 5:

```cpp
int queue[5];
```

We need two variables:

```cpp
int front = -1;
int rear = -1;
```

### Why two variables?

* **front** → tells us where the first element is.
* **rear** → tells us where the last element is.

Initially:

```text
Array:

[ ][ ][ ][ ][ ]
 ↑
front = -1
rear  = -1
```

The queue is empty.

---

# 3. Enqueue Operation

**Enqueue** means inserting an element into the queue.

Suppose:

```text
enqueue(10)
```

Since the queue is empty:

```cpp
front = 0;
rear = 0;
queue[rear] = 10;
```

Now:

```text
[10][ ][ ][ ][ ]
 ↑
 F,R
```

---

### Enqueue 20

```cpp
rear++;
queue[rear] = 20;
```

Now:

```text
[10][20][ ][ ][ ]
 ↑    ↑
 F    R
```

---

### Enqueue 30

```text
[10][20][30][ ][ ]
 ↑        ↑
 F        R
```

So the queue is:

```text
FRONT → 10  20  30 ← REAR
```

---

# 4. Dequeue Operation

**Dequeue** means removing an element from the **front**.

Current queue:

```text
FRONT → 10  20  30 ← REAR
```

Perform:

```text
dequeue()
```

`10` will be removed because it entered first.

We move `front`:

```cpp
front++;
```

Now:

```text
        FRONT
          ↓
[10][20][30][ ][ ]
     20  30
```

Logically, queue contains:

```text
20  30
```

Then another dequeue:

```text
20 is removed
```

Now:

```text
[10][20][30][ ][ ]
         ↑
       FRONT
```

Queue contains only:

```text
30
```

---

# 5. Complete C++ Program

```c++
#include <iostream>
using namespace std;

#define SIZE 5

int queueArr[SIZE];
int front = -1;
int rear = -1;

// ENQUEUE
void enqueue(int value)
{
    if (rear == SIZE - 1)
    {
        cout << "Queue Overflow\n";
        return;
    }

    if (front == -1)
    {
        front = 0;
    }

    rear++;
    queueArr[rear] = value;

    cout << value << " inserted into queue\n";
}

// DEQUEUE
void dequeue()
{
    if (front == -1 || front > rear)
    {
        cout << "Queue Underflow\n";
        return;
    }

    cout << queueArr[front] << " deleted from queue\n";

    front++;

    // Queue becomes empty
    if (front > rear)
    {
        front = -1;
        rear = -1;
    }
}

// DISPLAY
void display()
{
    if (front == -1)
    {
        cout << "Queue is empty\n";
        return;
    }

    cout << "Queue elements: ";

    for (int i = front; i <= rear; i++)
    {
        cout << queueArr[i] << " ";
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

    enqueue(40);
    enqueue(50);

    display();

    return 0;
}
```

---

# 6. Understand the Program Flow

Initially:

```text
front = -1
rear = -1
```

### `enqueue(10)`

```text
[10][ ][ ][ ][ ]
 ↑
 F,R
```

### `enqueue(20)`

```text
[10][20][ ][ ][ ]
 ↑    ↑
 F    R
```

### `enqueue(30)`

```text
[10][20][30][ ][ ]
 ↑        ↑
 F        R
```

### `dequeue()`

`10` is removed.

```text
[10][20][30][ ][ ]
     ↑       ↑
    F        R
```

### `enqueue(40)`

```text
[10][20][30][40][ ]
     ↑          ↑
     F          R
```

### `enqueue(50)`

```text
[10][20][30][40][50]
     ↑             ↑
     F             R
```

---

# 7. Limitations of Linear Queue

This is the **most important part** of your question.

Suppose queue size is 5.

Initially:

```text
[10][20][30][40][50]
 ↑                  ↑
 F                  R
```

The queue is full.

Now perform two dequeue operations:

```text
dequeue()
dequeue()
```

10 and 20 are removed.

Logically, the queue is:

```text
20 → 30 → 40 → 50
```

Actually after two deletions:

```text
[ ][ ][30][40][50]
       ↑          ↑
      FRONT      REAR
```

There are **two empty spaces at the beginning**:

```text
[ ][ ][30][40][50]
 ↑ ↑
empty
```

Now suppose we try:

```text
enqueue(60)
```

Can we insert 60 into index `0`?

In a simple **linear queue implementation**, `rear` is already at the last index.

```cpp
if (rear == SIZE - 1)
```

So it says:

> ❌ Queue Overflow

But notice something interesting:

**There is actually empty space in the array!**

This is called **wastage of memory/storage space**.

---

# 8. Main Limitation

The biggest limitation of a linear queue is:

> **The empty spaces created by dequeue operations at the beginning cannot be reused when rear has reached the last position.**

Example:

```text
Before deletion:

[10][20][30][40][50]
 ↑                  ↑
 F                  R
```

After deleting 10 and 20:

```text
[ ][ ][30][40][50]
       ↑          ↑
      F           R
```

There is free space:

```text
[ ][ ]
```

But `rear` cannot move backward.

Therefore, the available memory at the beginning is wasted.

---

# 9. Circular Queue

This problem is solved by using a **Circular Queue**.

In a circular queue, the last position is connected back to the first position.

Think of it like a circle:

```text
       0
    ↗     ↘
   4       1
   ↑       ↓
   3 ←──── 2
```

After reaching index `4`, we can go back to index `0`.

That's why it is called a **Circular Queue**.

---

# 10. Why Circular Queue is Important?

Consider:

```text
[ ][ ][30][40][50]
       ↑          ↑
      FRONT      REAR
```

A circular queue can reuse the empty spaces:

```text
[60][70][30][40][50]
 ↑ ↑    ↑
new     FRONT
```

So the storage is used efficiently.

### Linear Queue

```text
[ ][ ][30][40][50]
 ↑ ↑
Wasted space
```

### Circular Queue

```text
[60][70][30][40][50]
 ↑ ↑
Reused space
```

Therefore:

> **Circular queue eliminates the wastage of storage space that occurs in a linear queue.**

---

# 11. How Circular Queue Moves

The important formula is:

```cpp
rear = (rear + 1) % SIZE;
```

Suppose `SIZE = 5`.

If:

```text
rear = 3
```

then:

```text
(3 + 1) % 5 = 4
```

So rear moves to `4`.

If:

```text
rear = 4
```

then:

```text
(4 + 1) % 5 = 0
```

So it **wraps around** to index `0`.

That's the main idea behind circular queues.

---

# 12. Linear Queue vs Circular Queue

| Feature               | Linear Queue   | Circular Queue        |
| --------------------- | -------------- | --------------------- |
| Principle             | FIFO           | FIFO                  |
| Insertion             | Rear           | Rear                  |
| Deletion              | Front          | Front                 |
| Space utilization     | Less efficient | More efficient        |
| Empty spaces reused?  | ❌ No           | ✅ Yes                 |
| Memory wastage        | Possible       | Minimized             |
| Rear can wrap around? | ❌ No           | ✅ Yes                 |
| Implementation        | Simpler        | Slightly more complex |

---

# Circular Queue Using Array

## 1. What is a Circular Queue?

A **circular queue** is a queue in which the last position of the array is connected back to the first position.

It follows:

> **FIFO — First In, First Out**

The main advantage is that **empty spaces can be reused**.



---

# 2. Why do we need a Circular Queue?

Suppose we have an array of size 5:

```text
[10][20][30][40][50]
 ↑                  ↑
front              rear
```

Now delete `10` and `20`.

```text
[ ][ ][30][40][50]
       ↑          ↑
     front       rear
```

There are two empty spaces at the beginning.

In a **linear queue**, we may not be able to use these spaces.

But in a **circular queue**, `rear` can move from the last position back to the first position:

```text
[60][70][30][40][50]
 ↑  ↑   ↑
new    front
```

So storage is used efficiently.

---

# 3. Important Variables

For an array of size 5:

```cpp
int queue[5];

int front = -1;
int rear = -1;
```

### `front`

Points to the element that will be **deleted next**.

### `rear`

Points to the element that was **inserted last**.

Initially:

```text
front = -1
rear = -1
```

means the queue is empty.

---

# 4. Insertion — ENQUEUE

**Enqueue** means inserting an element into the queue.

The important condition for a **full circular queue** is:

```cpp
(rear + 1) % SIZE == front
```

If this is true:

> Queue is full → **Overflow**

Otherwise, we move `rear`:

```cpp
rear = (rear + 1) % SIZE;
```

and insert the element:

```cpp
queue[rear] = value;
```

---

# 5. Deletion — DEQUEUE

**Dequeue** means removing an element from the **front**.

First check:

```cpp
if (front == -1)
```

If true:

> Queue is empty → **Underflow**

Otherwise, remove:

```cpp
queue[front]
```

Then move `front`:

```cpp
front = (front + 1) % SIZE;
```

---

# 6. Complete C++ Program

```cpp
#include <iostream>
using namespace std;

#define SIZE 5

int queue[SIZE];

int front = -1;
int rear = -1;

// INSERTION / ENQUEUE
void enqueue(int value)
{
    // Check if queue is full
    if ((rear + 1) % SIZE == front)
    {
        cout << "Queue Overflow\n";
        return;
    }

    // First element
    if (front == -1)
    {
        front = 0;
        rear = 0;
    }
    else
    {
        rear = (rear + 1) % SIZE;
    }

    queue[rear] = value;

    cout << value << " inserted\n";
}

// DELETION / DEQUEUE
void dequeue()
{
    // Check if queue is empty
    if (front == -1)
    {
        cout << "Queue Underflow\n";
        return;
    }

    cout << queue[front] << " deleted\n";

    // Only one element was present
    if (front == rear)
    {
        front = -1;
        rear = -1;
    }
    else
    {
        front = (front + 1) % SIZE;
    }
}

// DISPLAY
void display()
{
    if (front == -1)
    {
        cout << "Queue is empty\n";
        return;
    }

    cout << "Queue: ";

    int i = front;

    while (true)
    {
        cout << queue[i] << " ";

        if (i == rear)
            break;

        i = (i + 1) % SIZE;
    }

    cout << endl;
}

int main()
{
    enqueue(10);
    enqueue(20);
    enqueue(30);
    enqueue(40);
    enqueue(50);

    display();

    dequeue();
    dequeue();

    display();

    enqueue(60);
    enqueue(70);

    display();

    return 0;
}
```

---

# 7. Let's Trace the Program

Initially:

```text
front = -1
rear = -1
```

### `enqueue(10)`

Since queue is empty:

```cpp
front = 0;
rear = 0;
```

Array:

```text
[10][ ][ ][ ][ ]
 ↑
 F,R
```

---

### `enqueue(20)`

```cpp
rear = (0 + 1) % 5;
```

Therefore:

```text
rear = 1
```

Array:

```text
[10][20][ ][ ][ ]
 ↑    ↑
 F    R
```

---

### `enqueue(30)`

```text
[10][20][30][ ][ ]
 ↑        ↑
 F        R
```

---

### `enqueue(40)`

```text
[10][20][30][40][ ]
 ↑             ↑
 F             R
```

---

### `enqueue(50)`

```text
[10][20][30][40][50]
 ↑                  ↑
 F                  R
```

Queue is now full.

---

# 8. Now Perform Deletion

### `dequeue()`

`10` is removed.

```cpp
front = (front + 1) % SIZE;
```

So:

```text
front = 1
```

```text
[10][20][30][40][50]
     ↑             ↑
     F             R
```

Logical queue:

```text
20 → 30 → 40 → 50
```

---

### Another `dequeue()`

`20` is removed.

```text
front = 2
```

```text
[10][20][30][40][50]
         ↑         ↑
         F         R
```

Logical queue:

```text
30 → 40 → 50
```

---

# 9. Now the Important Circular Part ⭐

Now we insert `60`.

Current:

```text
[10][20][30][40][50]
         ↑         ↑
         F         R
```

`rear = 4`.

Normally, there is no position after index 4.

But circular queue uses:

```cpp
rear = (rear + 1) % SIZE;
```

So:

```text
rear = (4 + 1) % 5
rear = 0
```

**Rear goes back to index 0!**

Now:

```text
[60][20][30][40][50]
 ↑       ↑         ↑
 R       F
```

Logical queue is:

```text
30 → 40 → 50 → 60
```

---

# 10. Insert 70

Again:

```cpp
rear = (0 + 1) % 5;
```

Therefore:

```text
rear = 1
```

Insert 70:

```text
[60][70][30][40][50]
 ↑   ↑   ↑
     R   F
```

Logical queue:

```text
30 → 40 → 50 → 60 → 70
```

Notice that **index 0 and index 1 were reused**.

That's the biggest advantage of circular queue.

---

# 11. The Most Important Formula

You should remember this formula:

```cpp
(rear + 1) % SIZE
```

It makes the queue **circular**.

For `SIZE = 5`:

```text
rear = 0 → 1
rear = 1 → 2
rear = 2 → 3
rear = 3 → 4
rear = 4 → 0  ← circular movement
```

Similarly, `front` moves using:

```cpp
front = (front + 1) % SIZE;
```

---

# 12. When is Circular Queue Empty?

```cpp
front == -1
```

Example:

```text
front = -1
rear = -1
```

means:

> Queue is empty.

---

# 13. When is Circular Queue Full?

The condition is:

```cpp
(rear + 1) % SIZE == front
```

For example:

```text
[60][70][30][40][50]
 ↑   ↑   ↑
 R       F
```

If the next position of `rear` is `front`, there is no free space.

Therefore:

> **Queue is full.**

---

# 14. One Important Special Case

Suppose there is only **one element**:

```text
[10][ ][ ][ ][ ]
 ↑
 F,R
```

Here:

```cpp
front == rear
```

When we delete this element:

```cpp
front = -1;
rear = -1;
```

The queue becomes empty again.

That's why we have:

```cpp
if (front == rear)
{
    front = -1;
    rear = -1;
}
```

---

# 15. Linear Queue vs Circular Queue

| Operation             | Linear Queue   | Circular Queue |
| --------------------- | -------------- | -------------- |
| Insert                | Rear           | Rear           |
| Delete                | Front          | Front          |
| Principle             | FIFO           | FIFO           |
| Reuses deleted spaces | ❌              | ✅              |
| Wrap-around           | ❌              | ✅              |
| Efficient storage     | Less efficient | More efficient |
| Enqueue complexity    | O(1)           | O(1)           |
| Dequeue complexity    | O(1)           | O(1)           |

### Easy way to remember

```text
LINEAR QUEUE

[ ][ ][30][40][50]
 ↑ ↑
Wasted spaces
```

```text
CIRCULAR QUEUE

[60][70][30][40][50]
 ↑  ↑
Reused spaces
```

So the main idea is:

> **Circular Queue = FIFO + Array + Reuse of empty spaces + Wrap-around**

And the two operations are:

```text
ENQUEUE → Insert at REAR
DEQUEUE → Delete from FRONT
```

The key formula is:

```cpp
(rear + 1) % SIZE
```

which allows `rear` to move from the **last array position back to the first position**.

_______

# Double-Ended Queue (Deque)

Let's understand this topic in **very easy language**, because it has three important applications:

1. **Palindrome checking**
2. **Undo–Redo operations**
3. **Adaptive-Steal (A-Steal) job-scheduling algorithm**

---

# 1. What is a Double-Ended Queue?

A **Double-Ended Queue**, commonly called a **Deque** (pronounced *deck*), is a queue in which we can **insert and delete elements from both ends**.

The two ends are:

* **Front**
* **Rear**

Normal queue:

```text
Insertion → REAR
Deletion  → FRONT
```

But Deque:

```text
       FRONT                    REAR
         ↓                       ↓
      [10][20][30][40][50]
         ↑                       ↑
    Insert/Delete          Insert/Delete
```

So in a deque, we can:

### Insert from front

```text
insertFront()
```

### Insert from rear

```text
insertRear()
```

### Delete from front

```text
deleteFront()
```

### Delete from rear

```text
deleteRear()
```

---

# 2. Why is Deque Different from a Normal Queue?

### Normal Queue

A normal queue allows:

```text
INSERT → REAR
DELETE → FRONT
```

For example:

```text
10 → 20 → 30 → 40
↑                ↑
FRONT            REAR
```

We cannot normally delete `40` directly.

---

### Deque

Deque allows operations at **both ends**:

```text
       FRONT                 REAR
         ↓                     ↓
       [10][20][30][40]
         ↑                     ↑
      Insert                  Insert
      Delete                  Delete
```

Therefore:

> **Deque provides more flexibility than a normal queue.**

---

# 3. Types of Deque

There are two common types.

## A. Input Restricted Deque

Insertion is allowed only at **one end**, but deletion is allowed at **both ends**.

```text
Insertion:
       ↓
FRONT [10][20][30] REAR
                       ↑
                 No insertion
```

More simply:

```text
Insert → one end
Delete → both ends
```

---

## B. Output Restricted Deque

Deletion is allowed only at **one end**, but insertion is allowed at **both ends**.

```text
Insert → both ends
Delete → one end
```

---

# 4. Basic Deque Operations

Suppose:

```text
[20][30][40]
 ↑         ↑
FRONT     REAR
```

### Insert at Front

Insert `10`:

```text
[10][20][30][40]
 ↑             ↑
FRONT         REAR
```

### Insert at Rear

Insert `50`:

```text
[10][20][30][40][50]
 ↑                 ↑
FRONT             REAR
```

### Delete from Front

Delete `10`:

```text
[20][30][40][50]
 ↑             ↑
FRONT         REAR
```

### Delete from Rear

Delete `50`:

```text
[20][30][40]
 ↑         ↑
FRONT     REAR
```

---

# Application 1: Palindrome Checking

## 5. What is a Palindrome?

A **palindrome** is a word or sequence that reads the same from both directions.

Examples:

```text
MADAM
LEVEL
RADAR
121
```

For example:

```text
MADAM
```

From left:

```text
M A D A M
```

From right:

```text
M A D A M
```

Same → **Palindrome**.

---

# 6. Why can we use Deque for Palindrome Checking?

A deque allows us to access **both ends**.

Suppose:

```text
M A D A M
↑         ↑
FRONT    REAR
```

We compare:

```text
FRONT → M
REAR  → M
```

They are equal.

Remove both:

```text
A D A
↑   ↑
```

Compare:

```text
A == A
```

Remove both:

```text
D
```

Only one character remains.

Therefore:

> **MADAM is a palindrome.**

---

## 7. Algorithm for Palindrome Checking

### Step 1

Insert all characters into a deque.

```text
M A D A M
```

### Step 2

Compare:

```text
front character
       with
rear character
```

### Step 3

If they are different:

```text
Not Palindrome
```

### Step 4

If they are same:

```text
deleteFront()
deleteRear()
```

### Step 5

Continue until the deque has zero or one element.

If all pairs match:

```text
Palindrome
```

---

# Application 2: Undo–Redo Operations

This is a very important real-life application.

Think about **MS Word**, a text editor, Photoshop, etc.

Suppose you type:

```text
Hello
```

Then:

```text
Hello World
```

Then:

```text
Hello World!
```

Each operation can be stored so that we can go backward.

---

# 8. Undo

Suppose operations are:

```text
A → B → C
```

where:

```text
A = Type Hello
B = Type World
C = Add !
```

If we press **Undo**, we want to remove the **most recent operation**:

```text
C
```

Then:

```text
A → B
```

This is similar to a stack.

But when we also need to move between **undo and redo states**, a deque-like structure can be useful because operations can be managed from either end depending on the implementation.

---

# 9. Simple Undo–Redo Idea

Imagine:

```text
        UNDO HISTORY
             ↓
       [A][B][C]
             ↑
        latest action
```

Press Undo:

```text
C is removed
```

and placed into the redo history:

```text
UNDO: [A][B]

REDO: [C]
```

Now press Redo:

```text
C is restored
```

So:

```text
UNDO: [A][B][C]

REDO: [ ]
```

### Examples of applications

Deque-like double-ended structures can be useful in:

* Text editors
* Web browsers
* Image editing software
* Document editing
* Drawing applications
* IDEs

**Note:** In many practical undo/redo implementations, **two stacks** are more common than one deque. The important concept is that both undo and redo need efficient management of recent history.

---

# Application 3: Adaptive-Steal (A-Steal) Job Scheduling

This is a little more advanced, so let's first understand **job scheduling**.

## 10. What is Job Scheduling?

Suppose we have several processors/worker threads:

```text
CPU 1
CPU 2
CPU 3
CPU 4
```

and many jobs:

```text
J1 J2 J3 J4 J5 J6 J7 J8
```

We need to distribute jobs among processors so that the work is completed efficiently.

---

# 11. Work-Stealing Idea

Suppose CPU 1 has many jobs:

```text
CPU 1:
[J1][J2][J3][J4][J5][J6]
```

But CPU 2 has no jobs:

```text
CPU 2:
empty
```

CPU 2 can **steal a job** from CPU 1.

```text
CPU 1:
[J1][J2][J3][J4]

CPU 2:
[J5][J6]
```

This helps keep processors busy.

---

# 12. Why a Deque?

A worker can manage its own jobs in a **deque**.

For example:

```text
        Worker 1 Deque

      FRONT             REAR
        ↓                 ↓
      [J1][J2][J3][J4][J5]
```

The worker itself may take jobs from one end:

```text
Worker takes → FRONT
```

While another worker that needs work can **steal from the other end**:

```text
Thief takes → REAR
```

So two different activities can happen at opposite ends.

This is where the deque becomes very useful.

---

# 13. Adaptive-Steal (A-Steal)

The basic idea of **adaptive stealing** is that a worker does not blindly steal work all the time.

It can adapt its stealing behavior depending on the workload and system conditions.

Imagine:

```text
Worker A:
[J1][J2][J3][J4][J5][J6]
```

Worker B becomes idle.

It can steal work from the opposite end:

```text
Worker A:
[J1][J2][J3][J4]

Worker B:
[J5][J6]
```

If the workload changes, the stealing strategy can adapt to avoid unnecessary stealing and improve load balancing.
_______

Consider a parallel processing system. processors: P1, P2, P3 and P4. The Adaptive-steal job scheduling algorithm is to be applied for scheduling the jobs:-  J1, J2, J3, J4, J5, J6, J7 using double-ended queue data structure. Time stamps (in seconds) of Jobs are:- < J1, J2, J3, J4, J5, J6,  J7> is < 5, 7, 4, 2 ,4, 1, 2 > respectively. Initial State of system is given as follows:-

Develop the Gantt chart representing the scheduling of the given processor based on A-Steal job scheduling algorithm.

### Given data

| Job            | J1 | J2 | J3 | J4 | J5 | J6 | J7 |
| -------------- | -: | -: | -: | -: | -: | -: | -: |
| Execution time |  5 |  7 |  4 |  2 |  4 |  1 |  2 |

Initial deques:

| Processor | Deque              |
| --------- | ------------------ |
| P1        | J1, J2, J3, J4, J5 |
| P2        | J6                 |
| P3        | J7                 |
| P4        | Empty              |

We take the **left end as the processor's execution end** and the **right end as the stealing end**.

---

## Step-by-step scheduling

### Time 0

All processors with work start executing:

* **P1 → J1** : 0–5
* **P2 → J6** : 0–1
* **P3 → J7** : 0–2
* **P4** is idle, so it becomes a thief.

P4 steals **J5** from the rear of P1's deque.

So:

```text
P1: J1 (executing), J2, J3, J4
P2: J6 (executing)
P3: J7 (executing)
P4: J5 (executing)
```

### Time 1

P2 finishes J6.

P2's deque is empty, so P2 becomes a thief and steals **J4** from P1.

```text
P1: J1 (executing), J2, J3
P2: J4 (executing)
P3: J7 (executing)
P4: J5 (executing)
```

### Time 2

P3 finishes J7.

P3 becomes a thief and steals **J3** from P1.

```text
P1: J1 (executing), J2
P2: J4 (executing)
P3: J3 (executing)
P4: J5 (executing)
```

### Time 3

P2 finishes J4.

P2 becomes a thief and steals **J2** from P1.

```text
P1: J1 (executing)
P2: J2 (executing)
P3: J3 (executing)
P4: J5 (executing)
```

### Time 4

P4 finishes J5. P1 is still executing J1, and no other ready job remains for P4 to steal.

Therefore, P4 becomes idle.

### Time 5

P1 finishes J1.

### Time 6

P3 finishes J3 and becomes idle.

### Time 10

P2 finishes J2.

Thus, **all jobs are completed at time 10 seconds**.

---

# Gantt Chart

```text
Time →     0    1    2    3    4    5    6    7    8    9   10
           |----|----|----|----|----|----|----|----|----|----|

P1         | J1 | J1 | J1 | J1 | J1 |    |    |    |    |    |
           |<------------- 5 seconds ------------->|

P2         | J6 | J4 | J4 | J2 | J2 | J2 | J2 | J2 | J2 | J2 |
           |--1--|--2--|<--------- 7 seconds ----------->|

P3         | J7 | J7 | J3 | J3 | J3 | J3 |    |    |    |    |
           |--2--|<--------- 4 seconds --------->|

P4         | J5 | J5 | J5 | J5 |    |    |    |    |    |    |
           |<--- 4 seconds --->|
```

A cleaner interval representation is:

| Processor | Scheduled jobs | Time interval                   |
| --------- | -------------- | ------------------------------- |
| **P1**    | J1             | 0–5                             |
| **P2**    | J6, J4, J2     | 0–1, 1–3, 3–10                  |
| **P3**    | J7, J3         | 0–2, 2–6                        |
| **P4**    | J5             | 0–4                             |
| **Idle**  | P4, P3, P1     | 4–5, 6–10, after 5 respectively |

### Final answer

Total completion time = 10 seconds

The important **stealing sequence** is:

P4:J5, P2:J4, P3:J3,P2:J2

---

# 14. Why Deque is Suitable for A-Steal?

Because a deque provides:

```text
Worker's own work
       ↓
   one end

Other worker stealing
       ↓
   opposite end
```

For example:

```text
             DEQUE
      ┌─────────────────┐
      │ J1 J2 J3 J4 J5  │
      └─────────────────┘
       ↑               ↑
    Worker          Thief
    takes            steals
```

This provides efficient access from both ends.

---

# 15. Three Applications Together

| Application             | Why Deque is useful                                                              |
| ----------------------- | -------------------------------------------------------------------------------- |
| **Palindrome checking** | Compare and remove characters from both ends                                     |
| **Undo–Redo**           | Manage history of operations and recent states                                   |
| **A-Steal scheduling**  | Worker processes jobs from one end while another worker can steal from the other |

---

# 16. Easy Diagram to Remember

```text
                 DEQUE
                   
        FRONT              REAR
          ↓                  ↓
       [10][20][30][40][50]
          ↑                  ↑
       Insert              Insert
       Delete              Delete
```

### Palindrome

```text
M A D A M
↑       ↑
F       R

Compare F and R
```

### Job Scheduling

```text
[J1][J2][J3][J4][J5]
 ↑                 ↑
Worker            Thief
takes             steals
```

### Undo/Redo

```text
Recent operation history
[A][B][C][D]

Undo → recent action
Redo → restore action
```

---



# Adaptive-Steal (A-Steal) Job-Scheduling Algorithm

## 1. Introduction

In a **parallel computer system**, many jobs or tasks need to be executed at the same time by multiple processors or worker threads.

Suppose we have four processors:

```text
Processor P1
Processor P2
Processor P3
Processor P4
```

and several jobs:

```text
J1, J2, J3, J4, J5, J6, J7, J8
```

The objective of a scheduling algorithm is to distribute these jobs among processors so that:

* processors remain busy,
* workload is balanced,
* idle time is reduced,
* jobs are completed efficiently.

One approach is **work stealing**.

In work stealing, every processor maintains its own collection of tasks. When a processor finishes all its tasks and becomes idle, it can **steal a task from another processor**.

**Adaptive-Steal (A-Steal)** refers to an adaptive work-stealing strategy in which the stealing behavior can adjust according to the current workload/system situation.

---

# 2. Basic Idea of Work Stealing

Consider four processors.

Initially:

```text
P1 → J1 J2 J3 J4 J5
P2 → J6 J7
P3 → J8
P4 → Empty
```

Processor `P4` has no work.

Instead of waiting:

```text
P4 → IDLE
```

it looks for another processor having work.

For example:

```text
P4 ────────────► P1
                Steal
```

After stealing:

```text
P1 → J1 J2 J3 J4
P2 → J6 J7
P3 → J8
P4 → J5
```

Now `P4` can execute `J5`.

This is the fundamental idea behind work stealing.

---

# 3. Why Use a Deque?

A **deque (double-ended queue)** is particularly useful for work-stealing systems because work can be accessed from both ends.

A worker can maintain its tasks as:

```text
Front                         Rear
  ↓                             ↓
+----+----+----+----+----+
| J1 | J2 | J3 | J4 | J5 |
+----+----+----+----+----+
```

Typically, the **owner worker** performs operations at one end, while a **thief worker** obtains work from the other end.

For example:

```text
Owner
  ↓
[J1][J2][J3][J4][J5]
                    ↑
                  Thief
```

The exact owner/thief ends can vary by implementation. The important point is that they access opposite ends to reduce interference.

---

# 4. What is Adaptive-Steal?

The word **adaptive** means that the scheduler does not necessarily use a fixed stealing behavior in every situation.

The scheduler can consider factors such as:

* amount of work available,
* number of idle processors,
* workload distribution,
* previous stealing success,
* number of available victims,
* system conditions.

For example, suppose:

```text
P1 → 10 jobs
P2 → 8 jobs
P3 → 1 job
P4 → 0 jobs
```

P4 should not repeatedly steal from P3 because P3 has very little work.

Instead, it may select a processor with a larger workload:

```text
P4 → steal from P1
```

Thus, the stealing behavior adapts to the workload.

---

# 5. Components of A-Steal Scheduling

An A-Steal style system generally has the following components.

### 1. Workers / Processors

These are the entities that execute jobs.

```text
P1, P2, P3, P4
```

### 2. Local Work Queues

Each worker maintains its own jobs.

```text
P1 → [J1 J2 J3]
P2 → [J4 J5]
P3 → [J6]
P4 → []
```

### 3. Owner

The worker that owns a particular work queue.

### 4. Thief

An idle worker that attempts to obtain work from another worker.

### 5. Victim

The worker whose queue is selected by the thief.

```text
Thief → P4
          |
          | steals from
          ↓
Victim → P1
```

---

# 6. Working of Adaptive-Steal

Consider:

```text
P1 → [A B C D E]
P2 → [F G]
P3 → [H]
P4 → []
```

P4 becomes idle.

### Step 1 — Detect idle worker

```text
P4 → IDLE
```

P4 needs work.

### Step 2 — Select a victim

P4 selects a worker that appears to have available work.

For example:

```text
P4 → P1
```

### Step 3 — Steal work

P4 obtains work from P1's queue.

```text
Before:

P1 → [A B C D E]
P4 → []
```

After:

```text
P1 → [A B C D]
P4 → [E]
```

### Step 4 — Execute stolen work

P4 executes:

```text
E
```

### Step 5 — Continue

After completing E, P4 may:

* execute another local task,
* attempt another steal if it becomes idle,
* or terminate if no work remains.

---

# 7. Adaptive Behavior

Suppose P4 repeatedly tries to steal from P3:

```text
P3 → [H]
```

and P3 has almost no work.

Repeatedly selecting P3 would be inefficient.

An adaptive scheduler can change its strategy.

For example:

```text
Attempt 1 → P3
Attempt 2 → P1
Attempt 3 → P2
```

The exact adaptive policy depends on the particular implementation.

The important concept is:

> **The scheduler adjusts its stealing decisions according to the availability and distribution of work rather than blindly following a fixed victim-selection rule.**

---

# 8. A-Steal Example

Suppose:

```text
             Jobs
P1 → [A B C D E F]
P2 → [G H]
P3 → [I]
P4 → []
```

Initially:

```text
P1 = 6 jobs
P2 = 2 jobs
P3 = 1 job
P4 = 0 jobs
```

P4 is idle.

It selects P1 as the victim.

```text
P4 steals F
```

Now:

```text
P1 → [A B C D E]
P2 → [G H]
P3 → [I]
P4 → [F]
```

Suppose P3 later becomes idle.

It can also steal work:

```text
P3 → steal from P1
```

For example:

```text
P1 → [A B C D]
P3 → [E]
```

Now work is more evenly distributed.

---

# 9. Advantages of Adaptive-Steal

### 1. Dynamic Load Balancing

Work can move from busy processors to idle processors.

### 2. Reduced Idle Time

An idle processor can search for available work.

### 3. Suitable for Parallel Processing

It is useful when many independent tasks must be executed concurrently.

### 4. Handles Irregular Workloads

Some jobs may take much longer than others.

For example:

```text
J1 → 1 second
J2 → 1 second
J3 → 20 seconds
J4 → 1 second
```

Static distribution can result in one processor being busy much longer than others.

Work stealing can help redistribute available work.

### 5. Decentralized Scheduling

There does not have to be one central queue controlling every task.

---

# 10. Disadvantages

### 1. Stealing Overhead

Selecting a victim and transferring work has a cost.

### 2. Synchronization

Multiple workers may try to access a queue simultaneously.

### 3. Poor Victim Selection

If the thief repeatedly selects a worker with little or no work, time may be wasted.

### 4. Concurrent Data Structure Complexity

Implementing a thread-safe deque is considerably more complicated than a normal sequential queue.

---

# 11. Simplified A-Steal Algorithm

A conceptual algorithm is:

```text
A-STEAL(worker)

1. Execute jobs from the worker's local queue.

2. If the local queue becomes empty:
      a. Identify a suitable victim worker.
      b. Attempt to steal work from the victim.
      c. If stealing succeeds:
             execute the stolen job(s).
      d. If stealing fails:
             adapt the victim-selection/search strategy.

3. Repeat until there is no remaining work.

4. Terminate.
```

### Pseudocode

```text
while system has unfinished work:

    if local_queue is not empty:
        execute(local_queue.pop())

    else:
        victim = select_victim_adaptively()

        if victim has work:
            job = steal(victim)
            execute(job)
        else:
            update_stealing_strategy()
```

This is a **conceptual model**, rather than a specification of one universal A-Steal implementation.

---

# 12. Priority Queue

Now let's discuss the second topic.

A **priority queue** is a data structure in which every element is associated with a priority.

The element with the highest priority is processed first.

For example:

```text
Job       Priority
-------------------
A            2
B            5
C            1
D            4
```

If a **larger number means higher priority**, then:

```text
B → D → A → C
```

The priority determines the service order.

---

# 13. Normal Queue vs Priority Queue

## Normal Queue

A normal queue follows:

```text
FIFO
```

which means:

> First In, First Out.

Example:

```text
A → B → C
```

The order is:

```text
A → B → C
```

---

## Priority Queue

A priority queue follows priority.

```text
A → Priority 2
B → Priority 5
C → Priority 1
```

Service order:

```text
B → A → C
```

If two elements have the same priority, we can use FIFO ordering among those elements.

---

# 14. Implementation of Priority Queue Using One-Way List

A **one-way list** is another name for a **singly linked list**.

Each node contains:

```text
+----------+----------+------+
|   Data   | Priority | Next |
+----------+----------+------+
```

For example:

```text
        +----------------+
        | 20 | 5 |   ----|----+
        +----------------+    |
                              ↓
        +----------------+
        | 40 | 4 |   ----|----+
        +----------------+    |
                              ↓
        +----------------+
        | 10 | 2 |   ----|----+
        +----------------+    |
                              ↓
        +----------------+
        | 30 | 1 | NULL |
        +----------------+
```

Here:

```text
5 > 4 > 2 > 1
```

Therefore, the list is maintained in **descending priority order**.

---

# 15. Operations

A priority queue using a one-way list generally supports:

### Enqueue

Insert an element according to its priority.

### Dequeue

Delete the highest-priority element.

### Peek

Return the highest-priority element without deleting it.

### Display

Display all elements in priority order.

---

# 16. Enqueue Operation

Suppose the current list is:

```text
20(P5) → 40(P4) → 10(P2) → 30(P1)
```

Now insert:

```text
50(P6)
```

Since priority `6` is highest:

```text
50(P6) → 20(P5) → 40(P4) → 10(P2) → 30(P1)
```

Now insert:

```text
60(P3)
```

It should be inserted between priority 4 and priority 2:

```text
50(P6) → 20(P5) → 40(P4) → 60(P3) → 10(P2) → 30(P1)
```

---

# 17. Dequeue Operation

Because the linked list is already sorted by priority, the highest-priority node is at the front.

For:

```text
50(P6) → 20(P5) → 40(P4) → 60(P3)
```

`dequeue()` removes:

```text
50(P6)
```

Result:

```text
20(P5) → 40(P4) → 60(P3)
```

Therefore:

```text
Dequeue = O(1)
```

when the list is maintained in sorted order.

---

# 18. C++ Implementation Using One-Way List

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    int priority;
    Node* next;
};

// Insert element according to priority
void enqueue(Node*& front, int data, int priority) {
    Node* newNode = new Node;

    newNode->data = data;
    newNode->priority = priority;
    newNode->next = nullptr;

    // If queue is empty or new element has highest priority
    if (front == nullptr || priority > front->priority) {
        newNode->next = front;
        front = newNode;
        return;
    }

    Node* temp = front;

    // Find correct position
    // >= maintains FIFO order for equal priorities
    while (temp->next != nullptr &&
           temp->next->priority >= priority) {
        temp = temp->next;
    }

    newNode->next = temp->next;
    temp->next = newNode;
}

// Delete highest-priority element
void dequeue(Node*& front) {
    if (front == nullptr) {
        cout << "Priority Queue is empty.\n";
        return;
    }

    Node* temp = front;

    cout << "Deleted: " << temp->data
         << " | Priority: " << temp->priority << endl;

    front = front->next;

    delete temp;
}

// Display highest-priority element
void peek(Node* front) {
    if (front == nullptr) {
        cout << "Priority Queue is empty.\n";
        return;
    }

    cout << "Highest Priority Element: "
         << front->data
         << " | Priority: "
         << front->priority << endl;
}

// Display complete queue
void display(Node* front) {
    if (front == nullptr) {
        cout << "Priority Queue is empty.\n";
        return;
    }

    cout << "\nPriority Queue:\n";

    while (front != nullptr) {
        cout << "[Data = " << front->data
             << ", Priority = " << front->priority
             << "]";

        if (front->next != nullptr)
            cout << " -> ";

        front = front->next;
    }

    cout << " -> NULL\n";
}

// Delete all remaining nodes
void clearQueue(Node*& front) {
    while (front != nullptr) {
        Node* temp = front;
        front = front->next;
        delete temp;
    }
}

int main() {

    Node* front = nullptr;

    // Insert elements
    enqueue(front, 10, 2);
    enqueue(front, 20, 5);
    enqueue(front, 30, 1);
    enqueue(front, 40, 4);
    enqueue(front, 50, 3);

    // Display
    display(front);

    // Peek
    cout << "\n";
    peek(front);

    // Delete
    cout << "\nDeleting element:\n";
    dequeue(front);

    // Display after deletion
    display(front);

    // Free memory
    clearQueue(front);

    return 0;
}
```

---

# 19. Output

The elements are inserted as:
```
Priority Queue:
[Data = 20, Priority = 5] ->
[Data = 40, Priority = 4] ->
[Data = 50, Priority = 3] ->
[Data = 10, Priority = 2] ->
[Data = 30, Priority = 1] -> NULL

Highest Priority Element: 20 | Priority: 5

Deleting element:
Deleted: 20 | Priority: 5

Priority Queue:
[Data = 40, Priority = 4] ->
[Data = 50, Priority = 3] ->
[Data = 10, Priority = 2] ->
[Data = 30, Priority = 1] -> NULL
```

Main idea
```
Insert according to priority
          ↓
P5 → P4 → P3 → P2 → P1
          ↓
Delete from FRONT
```
---

# 20. Complexity of One-Way List Implementation

Let `n` be the number of elements.

| Operation | Complexity |
| --------- | ---------: |
| Enqueue   |     `O(n)` |
| Dequeue   |     `O(1)` |
| Peek      |     `O(1)` |
| Display   |     `O(n)` |

### Why is Enqueue `O(n)`?

Because we may have to traverse the list to find the correct position.

For example:

```text
P10 → P9 → P8 → P7 → P6 → P5 → ...
```

If we insert a low-priority element, we may have to travel through many nodes.

---

# 21. Priority Queue Using Separate Queue for Each Priority Level

Another method is to create a **separate ordinary queue for each priority level**.

Suppose we have three priority levels:

```text
Priority 3 → Highest
Priority 2 → Medium
Priority 1 → Lowest
```

We create:

```text
Q1 → Priority 1
Q2 → Priority 2
Q3 → Priority 3
```

Diagram:

```text
              Priority Queue
                    |
        +-----------+-----------+
        ↓           ↓           ↓
       Q3          Q2          Q1
    Priority 3  Priority 2  Priority 1
      [A][B]      [C][D]       [E][F]
```

The highest-priority non-empty queue is served first.

---

# 22. Example

Suppose:

```text
Q3 → A B
Q2 → C D
Q1 → E F
```

Since priority 3 is highest:

```text
A
B
```

are processed first.

Then:

```text
C
D
```

Then:

```text
E
F
```

So the complete order is:

```text
A → B → C → D → E → F
```

Notice something important:

Within the same priority level, FIFO is preserved.

For example:

```text
Q3 = A → B
```

means:

```text
A is processed before B
```

---

# 23. Enqueue in Separate-Queue Implementation

Suppose:

```text
Priority 3 → A B
Priority 2 → C
Priority 1 → D E
```

We insert:

```text
X with priority 2
```

We simply insert X into `Q2`:

```text
Priority 3 → A B
Priority 2 → C X
Priority 1 → D E
```

This is simple because we don't need to search for the correct position within one large list.

---

# 24. Dequeue

To dequeue:

1. Check the highest-priority queue.
2. If it is not empty, remove its front element.
3. Otherwise check the next priority.
4. Continue until a non-empty queue is found.

For:

```text
Q3 → Empty
Q2 → C X
Q1 → D E
```

we check:

```text
Q3 → Empty
Q2 → C X
```

Therefore:

```text
C
```

is removed.

After deletion:

```text
Q2 → X
```

---

# 25. C++ Implementation

```cpp
#include <iostream>
#include <queue>
using namespace std;

const int MAX_PRIORITY = 3;

// Insert element
void enqueue(queue<int> q[], int data, int priority) {

    if (priority < 1 || priority > MAX_PRIORITY) {
        cout << "Invalid priority.\n";
        return;
    }

    q[priority].push(data);

    cout << "Inserted: " << data
         << " | Priority: " << priority << endl;
}

// Delete highest-priority element
void dequeue(queue<int> q[]) {

    // Start from highest priority
    for (int priority = MAX_PRIORITY;
         priority >= 1;
         priority--) {

        if (!q[priority].empty()) {

            cout << "Deleted: "
                 << q[priority].front()
                 << " | Priority: "
                 << priority << endl;

            q[priority].pop();

            return;
        }
    }

    cout << "Priority Queue is empty.\n";
}

// Display highest-priority element
void peek(queue<int> q[]) {

    for (int priority = MAX_PRIORITY;
         priority >= 1;
         priority--) {

        if (!q[priority].empty()) {

            cout << "Highest Priority Element: "
                 << q[priority].front()
                 << " | Priority: "
                 << priority << endl;

            return;
        }
    }

    cout << "Priority Queue is empty.\n";
}

// Display all queues
void display(queue<int> q[]) {

    cout << "\nPriority Queues:\n";

    for (int priority = MAX_PRIORITY;
         priority >= 1;
         priority--) {

        cout << "Priority "
             << priority
             << ": ";

        queue<int> temp = q[priority];

        while (!temp.empty()) {
            cout << temp.front() << " ";
            temp.pop();
        }

        cout << endl;
    }
}

int main() {

    // Array of queues
    queue<int> q[MAX_PRIORITY + 1];

    // Insert elements
    enqueue(q, 10, 2);
    enqueue(q, 20, 3);
    enqueue(q, 30, 1);
    enqueue(q, 40, 3);
    enqueue(q, 50, 2);

    // Display
    display(q);

    // Peek
    cout << "\n";
    peek(q);

    // Delete elements
    cout << "\nDeleting elements:\n";

    dequeue(q);
    dequeue(q);
    dequeue(q);

    // Display after deletion
    display(q);

    return 0;
}
```

---

# 26. Output Explanation
```
Inserted: 10 | Priority: 2
Inserted: 20 | Priority: 3
Inserted: 30 | Priority: 1
Inserted: 40 | Priority: 3
Inserted: 50 | Priority: 2

Priority Queues:
Priority 3: 20 40
Priority 2: 10 50
Priority 1: 30

Highest Priority Element: 20 | Priority: 3

Deleting elements:
Deleted: 20 | Priority: 3
Deleted: 40 | Priority: 3
Deleted: 10 | Priority: 2

Priority Queues:
Priority 3:
Priority 2: 50
Priority 1: 30
```
---

# 27. Complexity of Separate Queues

Suppose:

* `n` = total number of elements
* `P` = number of priority levels

### Enqueue

```text
O(1)
```

We directly access the queue corresponding to the priority.

### Dequeue

If we scan all priority levels:

```text
O(P)
```

In some implementations, this can be improved by maintaining additional information about the highest non-empty priority.

### Peek

```text
O(P)
```

if we scan from the highest priority.

### Display

```text
O(n + P)
```

approximately, because we inspect the priority queues and their elements.

---

# 28. Comparison of the Two Implementations

| Feature            | One-Way List            | Separate Queue for Each Priority |
| ------------------ | ----------------------- | -------------------------------- |
| Basic structure    | Singly linked list      | Multiple FIFO queues             |
| Priority stored    | In every node           | Represented by queue             |
| Enqueue            | `O(n)`                  | `O(1)`                           |
| Dequeue            | `O(1)`                  | `O(P)` with priority scan        |
| Peek               | `O(1)`                  | `O(P)` with priority scan        |
| Same-priority FIFO | Can be maintained       | Naturally maintained             |
| Memory             | Dynamic nodes           | Multiple queues                  |
| Best suited for    | Many/dynamic priorities | Fixed/small number of priorities |

---

# 29. Simple Real-Life Example of Priority Queue

Consider a hospital emergency system:

```text
Patient     Priority
---------------------
P1          2
P2          5
P3          1
P4          4
```

If `5` is the highest priority:

```text
P2 → P4 → P1 → P3
```

The patient with the most urgent priority is handled first.

Similarly, in an operating system:

```text
Process     Priority
--------------------
P1             3
P2             1
P3             5
P4             2
```

Processing order:

```text
P3 → P1 → P4 → P2
```

assuming a larger number means higher priority.

---

# 30. Relationship Between A-Steal and Deque

These topics are closely related.

In a work-stealing scheduler, each worker can maintain a **deque**:

```text
Worker 1 → Deque
Worker 2 → Deque
Worker 3 → Deque
Worker 4 → Deque
```

For example:

```text
Worker 1:
[A][B][C][D][E]

Worker 2:
[F][G]

Worker 3:
[H]

Worker 4:
Empty
```

Worker 4 becomes idle.

It can attempt to steal work:

```text
Worker 4
    |
    | steal
    ↓
Worker 1
```

After stealing:

```text
Worker 1:
[A][B][C][D]

Worker 4:
[E]
```

Thus, the **deque provides the data structure**, while **work stealing/A-Steal provides the scheduling strategy**.

---

# 31. Important Exam Points

### Adaptive-Steal

**Definition:**

> Adaptive-Steal is a work-stealing scheduling approach in which an idle worker attempts to obtain work from another worker, with the stealing behavior adapting to the current workload or scheduling conditions.

**Main idea:**

```text
Idle Worker
     ↓
Find Work
     ↓
Select Victim
     ↓
Steal Work
     ↓
Execute Work
     ↓
Become Idle Again
```

**Main benefit:**

> Dynamic load balancing and reduction of processor idle time.

---

### Priority Queue Using One-Way List

**Definition:**

> A priority queue can be implemented using a singly linked list by storing each element with its priority and maintaining the list in priority order.

If larger number = higher priority:

```text
P5 → P4 → P3 → P2 → P1
```

Complexities:

```text
Enqueue  = O(n)
Dequeue  = O(1)
Peek     = O(1)
```

---

### Priority Queue Using Separate Queues

**Definition:**

> A priority queue can be implemented using a separate FIFO queue for every priority level.

Example:

```text
Q3 → Highest priority
Q2 → Medium priority
Q1 → Lowest priority
```

Operations:

```text
Enqueue → Insert into corresponding queue
Dequeue → Remove from highest non-empty queue
```

Complexities with a straightforward priority scan:

```text
Enqueue  = O(1)
Dequeue  = O(P)
Peek     = O(P)
```

where `P` is the number of priority levels.

---

## One-Line Memory Trick

```text
A-STEAL
A → Adaptive
S → Search for work
T → Take/steal work
E → Execute
A → Again if idle
L → Load balancing
```

```text
PRIORITY QUEUE

One-Way List:
Insert according to Priority → Delete from Front

Separate Queues:
One Queue per Priority → Serve Highest Non-Empty Queue
```



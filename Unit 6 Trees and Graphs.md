# Tree Data Structures — Easy Notes with Examples

Let us learn each topic step by step in simple English, with diagrams, real-life examples, and C++ code.

## 1. What is a Tree Data Structure?

A tree is a non-linear data structure used to store data in a hierarchical form. Unlike an array, where elements are arranged one after another, a tree connects elements through relationships.

Real-life example: Think about a family tree. Grandparents are at the top, their children are below them, and grandchildren are further down.

[Struktur Data Tree: Pengertian, Jenis, dan Kegunaannya - Trivusi](https://images.openai.com/static-rsc-4/0efVEaWJzeAVBUnKRzme7A1eDVrwUtlShbFpc_DsxMmSBE2Y42A1YD6ltdFZw2jpa10hxSM3agwIiBu1QOETs1d3T83RviOfe-pPy58M-hvVBf7KAD1AonMk-RJMUp10GHFI8KWRDXJYeVB8SA8kHf0assN9Q6wip2isqlr1IV8?purpose=inline)

[trivusi.web.id](https://www.trivusi.web.id/2022/07/struktur-data-tree.html?utm_source=chatgpt.com)

### Important terms

Consider this tree:

```
          A
         / \
        B   C
       / \
      D   E
```

| Term    | Meaning                      | Example                   |
| ------- | ---------------------------- | ------------------------- |
| Root    | Topmost node                 | A                         |
| Parent  | Node having children         | A is parent of B and C    |
| Child   | Node below another node      | B and C are children of A |
| Leaf    | Node with no children        | C, D, E                   |
| Edge    | Connection between nodes     | A–B, A–C                  |
| Subtree | A smaller tree within a tree | B with children D and E   |

Remember: A tree consists of nodes and edges, and it has a hierarchical structure.

## 2. Implementation of a Tree in C++

Implementation means creating the tree in a program using memory and pointers.

We can create each node using a `struct`.

```
struct Node
{
    int data;
    Node* left;
    Node* right;
};
```

Let's understand each line.

- `int data;` stores the value of the node.
- `Node* left;` stores the address of the left child.
- `Node* right;` stores the address of the right child.

Here, `Node*` is a pointer that can store the address of another node.

For example, to create a node containing `10`:

```
Node* root = new Node;

root->data = 10;
root->left = NULL;
root->right = NULL;
```

The tree initially looks like this:

```
      10
     /  \
   NULL NULL
```

The node contains `10`, but it does not yet have any children.

To add two children containing `5` and `20`:

```
root->left = new Node;
root->left->data = 5;
root->left->left = NULL;
root->left->right = NULL;

root->right = new Node;
root->right->data = 20;
root->right->left = NULL;
root->right->right = NULL;
```

Now the tree is:

```
        10
       /  \
      5    20
```

This is a simple example of a Binary Tree implementation.

## 3. What is a Binary Tree?

A Binary Tree is a tree in which every node can have a maximum of two children:

1. Left child
2. Right child

A node may have no children, one child, or two children.

Example:

```
          10
         /  \
        5    20
       / \
      3   8
```

Here:

- `10` is the root.
- `5` is the left child of `10`.
- `20` is the right child of `10`.
- `3` and `8` are children of `5`.
- `3`, `8`, and `20` are leaf nodes.

Important: A Binary Tree does not require the values to be sorted. For example, `5` can be the left child of `10` and `20` can be its right child, but a general Binary Tree does not enforce this arrangement.

### C++ implementation of a Binary Tree

```
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* left;
    Node* right;
};

int main()
{
    Node* root = new Node;
    root->data = 10;
    root->left = new Node;
    root->right = new Node;

    root->left->data = 5;
    root->left->left = NULL;
    root->left->right = NULL;

    root->right->data = 20;
    root->right->left = NULL;
    root->right->right = NULL;

    cout << "Root: " << root->data << endl;
    cout << "Left child: " << root->left->data << endl;
    cout << "Right child: " << root->right->data << endl;

    return 0;
}
```

Output:

```
Root: 10
Left child: 5
Right child: 20
```

This program manually creates three nodes and connects them to form a Binary Tree.

## 4. What is a Binary Search Tree (BST)?

A Binary Search Tree (BST) is a special type of Binary Tree in which values are arranged according to a rule.

- Values smaller than the current node go to the left.
- Values greater than the current node go to the right.

For this example, we will use distinct values.

### Example: Insert 50, 30, 70, 20, 40

Let's insert these numbers one by one.

### Step 1: Insert 50

The tree is empty, so 50 becomes the root.

```
    50
```

### Step 2: Insert 30

Compare 30 with 50. Since 30 is smaller, insert it on the left.

```
     50
    /
   30
```

### Step 3: Insert 70

Compare 70 with 50. Since 70 is greater, insert it on the right.

```
     50
    /  \
   30   70
```

### Step 4: Insert 20

Compare 20 with 50: go left. Compare 20 with 30: go left again.

```
      50
     /  \
    30   70
   /
  20
```

### Step 5: Insert 40

Compare 40 with 50: go left. Compare 40 with 30: go right.

```
      50
     /  \
    30   70
   / \
  20  40
```

### Try inserting values yourself

Insert in order30104020

left301040

\\(20 < 30\\)

The left child of 30 is 10; compare 20 there next.

MixedBalanced orderAscending order

Give feedback

### C++ program to insert values into a BST

```
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* left;
    Node* right;
};

Node* createNode(int value)
{
    Node* newNode = new Node;
    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

Node* insertNode(Node* root, int value)
{
    if (root == NULL)
        return createNode(value);

    if (value < root->data)
        root->left = insertNode(root->left, value);
    else if (value > root->data)
        root->right = insertNode(root->right, value);

    return root;
}

int main()
{
    Node* root = NULL;

    root = insertNode(root, 50);
    root = insertNode(root, 30);
    root = insertNode(root, 70);
    root = insertNode(root, 20);
    root = insertNode(root, 40);

    cout << "BST created successfully.";

    return 0;
}
```

What does `insertNode()` do?

Suppose we insert `40`.

1. `root` is 50. Since 40 is smaller than 50, go left.
2. The current node is 30. Since 40 is greater than 30, go right.
3. The right position of 30 is empty, so create a new node containing 40.

This is how BST insertion works.

## 5. What is Tree Traversal?

Tree traversal means visiting every node of a tree in a particular order.

We will use this same tree for all three techniques:

```
          1
         / \
        2   3
       / \
      4   5
```

The root is `1`, its children are `2` and `3`, and the children of `2` are `4` and `5`.

There are three important traversal techniques:

1. Preorder
2. Inorder
3. Postorder

The only difference is when we visit or print the root node.

## 6. Preorder Traversal (Root → Left → Right)

In Preorder, we visit the root first, then the left subtree, and finally the right subtree.

The order is:

1. Visit Root
2. Traverse Left
3. Traverse Right

### Example with step-by-step explanation

```
          1  ← Visit first
         / \
        2   3
       / \
      4   5
```

Follow the steps:

- Visit `1` (root).
- Move left and visit `2`.
- Move left and visit `4`.
- Node `4` has no children, so return to `2`.
- Visit `5`, the right child of `2`.
- Return to `1` and visit `3`.

PREORDER VISITING SEQUENCE

## 1

1st

## 2

2nd

## 4

3rd

## 5

4th

## 3

5th

Output: `1 2 4 5 3`

### C++ code for Preorder

```
void preorder(Node* root)
{
    if (root == NULL)
        return;

    cout << root->data << " ";  // Root
    preorder(root->left);       // Left
    preorder(root->right);      // Right
}
```

Easy trick: Preorder means print the parent before its children.

Where is it useful? Preorder traversal is useful when creating a copy of a tree or recording a tree structure with appropriate null markers.

## 7. Inorder Traversal (Left → Root → Right)

In Inorder, we visit the left subtree first, then the root, and finally the right subtree.

The order is:

1. Traverse Left
2. Visit Root
3. Traverse Right

Use the same tree:

```
          1
         / \
        2   3
       / \
      4   5
```

### Step-by-step explanation

- Start at `1`, but go left first.
- At `2`, go left again and visit `4`.
- Return to `2` and visit it.
- Move to the right child of `2` and visit `5`.
- Return to `1` and visit it.
- Finally, visit `3`.

INORDER VISITING SEQUENCE

## 4

1st

## 2

2nd

## 5

3rd

## 1

4th

## 3

5th

Output: `4 2 5 1 3`

### C++ code for Inorder

```
void inorder(Node* root)
{
    if (root == NULL)
        return;

    inorder(root->left);        // Left
    cout << root->data << " ";  // Root
    inorder(root->right);       // Right
}
```

Important exam point: When a BST is traversed using Inorder, its values appear in ascending order, assuming distinct keys and the standard BST ordering rule.

For example, the BST:

```
        50
       /  \
      30   70
     / \
    20  40
```

Its Inorder output is:

```
20 30 40 50 70
```

Easy trick: Inorder means print the parent in between the left and right subtrees.

## 8. Postorder Traversal (Left → Right → Root)

In Postorder, we visit the left subtree first, then the right subtree, and print the root last.

The order is:

1. Traverse Left
2. Traverse Right
3. Visit Root

Use the same tree again:

```
          1
         / \
        2   3
       / \
      4   5
```

### Step-by-step explanation

- Start at `1` and go left to `2`.
- Go left again and visit `4`.
- Return to `2`, then visit its right child `5`.
- Now both children of `2` have been visited, so print `2`.
- Return to `1` and visit `3`.
- Both subtrees of `1` are now complete, so print `1` last.

POSTORDER VISITING SEQUENCE

## 4

1st

## 5

2nd

## 2

3rd

## 3

4th

## 1

5th

Output: `4 5 2 3 1`

### C++ code for Postorder

```
void postorder(Node* root)
{
    if (root == NULL)
        return;

    postorder(root->left);      // Left
    postorder(root->right);     // Right
    cout << root->data << " ";  // Root
}
```

Easy trick: Postorder means print the parent after both of its children.

Where is it useful? Postorder is useful when deleting a tree because child nodes can be processed before their parent.

## 9. Difference Between Preorder, Inorder and Postorder

Remember the same tree:

```
          1
         / \
        2   3
       / \
      4   5
```

| Technique | Rule                | Output      |
| --------- | ------------------- | ----------- |
| Preorder  | Root → Left → Right | `1 2 4 5 3` |
| Inorder   | Left → Root → Right | `4 2 5 1 3` |
| Postorder | Left → Right → Root | `4 5 2 3 1` |

The tree remains the same. Only the order in which we print the nodes changes.

## 10. Complete C++ Program: All Three Traversals

Here is a complete program that you can compile and run in Code::Blocks, Dev-C++, or another C++ compiler.

```

#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* left;
    Node* right;
};

// Create a new node
Node* createNode(int value)
{
    Node* newNode = new Node;

    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;

    return newNode;
}

// Preorder: Root, Left, Right
void preorder(Node* root)
{
    if (root == NULL)
        return;

    cout << root->data << " ";
    preorder(root->left);
    preorder(root->right);
}

// Inorder: Left, Root, Right
void inorder(Node* root)
{
    if (root == NULL)
        return;

    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}

// Postorder: Left, Right, Root
void postorder(Node* root)
{
    if (root == NULL)
        return;

    postorder(root->left);
    postorder(root->right);
    cout << root->data << " ";
}

int main()
{
    // Create the tree
    Node* root = createNode(1);

    root->left = createNode(2);
    root->right = createNode(3);

    root->left->left = createNode(4);
    root->left->right = createNode(5);

    cout << "Preorder: ";
    preorder(root);

    cout << "\nInorder: ";
    inorder(root);

    cout << "\nPostorder: ";
    postorder(root);

    cout << endl;

    return 0;
}

```

### Output

```
Preorder: 1 2 4 5 3
Inorder: 4 2 5 1 3
Postorder: 4 5 2 3 1
```

### How does recursion work here?

Consider this statement:

```
preorder(root->left);
```

It means: call the same function to traverse the left subtree.

The condition:

```
if (root == NULL)
    return;
```

is the base case. It stops the function when there is no node to visit. Without this stopping condition, recursive traversal would not terminate correctly.

## 11. Time Complexity of Tree Traversals

If the tree contains \\(n\\) nodes, all three traversals visit each node exactly once.

| Traversal | Time complexity |
| --------- | --------------- |
| Preorder  | \\(O(n)\\)      |
| Inorder   | \\(O(n)\\)      |
| Postorder | \\(O(n)\\)      |

The recursive call stack requires \\(O(h)\\) extra space, where \\(h\\) is the height of the tree.

## 12. Practice Questions

Try these questions to check your understanding.

1\. Which traversal visits the root first?

Inorder

Preorder

Postorder

2\. Which traversal gives sorted values for a BST with distinct keys?

Preorder

Inorder

Postorder

3\. In which traversal is the root visited last?

Preorder

Inorder

Postorder

4\. What is the maximum number of children a binary tree node can have?

1

2

3



## Final revision

- Tree: Hierarchical arrangement of nodes.
- Binary Tree: Each node has at most two children.
- BST: Smaller values go left; larger values go right.
- Preorder: Root first.
- Inorder: Root in the middle.
- Postorder: Root last.

One-line memory trick: 
Preorder = Parent first; 
Inorder = Parent in between; 
Postorder = Parent last.

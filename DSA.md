** ✅ What is Time Complexity? **
>Time Complexity tells how the running time of an algorithm increases as the size of input (n) increases.
It does NOT measure actual seconds → it measures growth rate.

>Example:
If algorithm takes 2 seconds for 10 inputs,
It may take 4 seconds for 20 inputs (just an example of growth).

**✅ What is Space Complexity?**
>Space Complexity tells how much extra memory an algorithm uses.


| Notation          | Meaning         | Represents                      | What Interviewers Expect                                                         | Simple Example                                                     |
| ----------------- | --------------- | ------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Big-O (O)**     | **Upper bound** | **Worst-case time**             | The algorithm will **at most** take this much time; guaranteed performance limit | Linear Search → **O(n)** (worst case: element at end or not found) |
| **Big-Θ (Theta)** | **Tight bound** | **Average / exact growth rate** | Shows the algorithm’s **typical running time**, neither faster nor slower        | Merge Sort → **Θ(n log n)**                                        |
| **Big-Ω (Omega)** | **Lower bound** | **Best-case time**              | Minimum time algorithm will take; describes **best scenario**                    | Insertion Sort → **Ω(n)** (already sorted array)                   |


** Time Com of Sorting algo **

| Sorting Algorithm  | Best Time  | Average Time | Worst Time | Space Complexity | Notes                                       |
| ------------------ | ---------- | ------------ | ---------- | ---------------- | ------------------------------------------- |
| **Bubble Sort**    | O(n)       | O(n²)        | O(n²)      | O(1)             | Stable, simple, rarely used in real apps    |
| **Selection Sort** | O(n²)      | O(n²)        | O(n²)      | O(1)             | Not stable, always quadratic                |
| **Insertion Sort** | O(n)       | O(n²)        | O(n²)      | O(1)             | Stable, good for nearly sorted arrays       |
| **Merge Sort**     | O(n log n) | O(n log n)   | O(n log n) | O(n)             | Stable, used in real-world libraries        |
| **Quick Sort**     | O(n log n) | O(n log n)   | O(n²)      | O(log n)         | Fastest in practice, not stable             |
| **Heap Sort**      | O(n log n) | O(n log n)   | O(n log n) | O(1)             | Not stable, used in priority queues         |
| **Counting Sort**  | O(n + k)   | O(n + k)     | O(n + k)   | O(k)             | Stable, works only for small integer ranges |
| **Tim Sort**       | O(n)       | O(n log n)   | O(n log n) | O(n)             | Stable, used in Python & Java sorting       |


| Type               | Characters    | ASCII Range |
| ------------------ | ------------- | ----------- |
| Digits             | '0' – '9'     | 48 – 57     |
| Uppercase Letters  | 'A' – 'Z'     | 65 – 90     |
| Lowercase Letters  | 'a' – 'z'     | 97 – 122    |
| Special Symbols    | ' ' – '/'     | 32 – 47     |
|                    | ':' – '@'     | 58 – 64     |
|                    | '[' – '`'     | 91 – 96     |
|                    | '{' – '~'     | 123 – 126   |
| Control Characters | Non-printable | 0 – 31      |
| Delete (DEL)       | —             | 127         |


## Linked List Overview: 
A linked list is a linear data structure consisting of nodes. Each node contains two parts: the data and a pointer (or reference) to the next node in the sequence. This forms a chain-like structure.
Types of Linked Lists:
Singly Linked List: Each node points only to the next node.
Doubly Linked List: Each node points to both the next and the previous node.
Circular Linked List: The last node points back to the first node, forming a circle.
Circular Doubly Linked List: Each node points to both the next and the previous node, and the last node points back to the first.

# Advantages:
Dynamic size: Linked lists can easily grow or shrink in size without the need for reallocation.

Efficient insertions and deletions: These operations are generally faster than in arrays, especially when inserting or deleting in the middle of the list.

# Disadvantages:
Extra memory usage: Each node requires additional memory for pointers.

Sequential access: To access a particular element, you need to traverse the list from the head, resulting in O(n) time complexity.

# Time Complexity:
Traversal: O(n), since you may need to visit each node once.

Insertion: O(1) if inserting at the beginning (with direct access to the head), but O(n) in general if you need to find the insertion point.

Deletion: O(1) if you have direct access to the node to delete, but O(n) if you need to find it first.



## Difference Between Linked List and Array:

# Memory Allocation:
Array: Stores elements in a contiguous block of memory, meaning all elements are adjacent in memory.

Linked List: Consists of nodes that are stored in non-contiguous memory locations, and each node points to the next node.
 
# Size:

Array: Has a fixed size that must be defined at the time of creation and cannot be changed.

Linked List: Is dynamic in size, allowing it to grow or shrink as needed without requiring reallocation of the entire structure.

# Access Time:

Array: Offers constant time access, O(1), because you can directly access any element using its index.

Linked List: Requires sequential traversal, meaning the access time is O(n) in the worst case, as you may need to go through multiple nodes.

# Insertion and Deletion:

Array: Inserting or deleting elements, especially in the middle, can be costly since it may require shifting elements, resulting in O(n) time complexity.

Linked List: Insertion and deletion are more efficient (typically O(1)) if you have direct access to the node, since you only need to update pointers.

# Memory Overhead:

Array: Generally has lower memory overhead because it only stores the elements.

Linked List: Has extra memory overhead due to the pointers stored in each node.



## A binary tree 
A binary tree is a non-linear hierarchical data structure where each node has at most two children, typically referred to as the left child and the right child. The topmost node is called the root, and nodes with no children are called leaf nodes.

# Key Concepts:
Root Node: The root node is the topmost node of the binary tree and serves as the entry point to the tree. It has no parent.

Node Structure: Each node in a binary tree contains data and up to two pointers or references: one to the left child and one to the right child.

Children and Parent: Every node, except the root, has one parent. Nodes that have children are referred to as internal nodes.

Leaf Nodes: Leaf nodes are nodes that do not have any children. They are at the bottom of the tree.

Height and Depth:
Height of a Tree: The height of a binary tree is the number of edges on the longest path from the root to a leaf.

Depth of a Node: The depth of a node is the number of edges from the root to that node.

Types of Binary Trees:
Full Binary Tree: Every node other than the leaves has exactly two children.
Complete Binary Tree: All levels are fully filled except possibly the last level, and the last level has all nodes as left as possible.
Perfect Binary Tree: All internal nodes have two children, and all leaves are at the same level.
Degenerate Tree: Each parent node has only one child, essentially resembling a linked list


## Binary Search Tree (BST) Overview:
A binary search tree (BST) is a special type of binary tree that maintains a sorted order of elements. For every node in a BST:

The left subtree contains only nodes with values less than the node’s value.

The right subtree contains only nodes with values greater than the node’s value.

This property is applied recursively to all nodes in the tree, ensuring that for any given node, its left subtree and right subtree follow this rule.

# Key Points:
Efficiency:
Search Operation: In a balanced BST, the average time complexity for searching is O(log n), where n is the number of nodes.

Insertion and Deletion: Similarly, insertion and deletion operations also have an average time complexity of O(log n) in a balanced BST.

# Sorting Algorithms – Interview Notes (C++)

These notes are prepared for **service-based company interviews (TCS, Wipro, Cognizant)**.  
Focus is on **how to explain first**, then **write clean C++ code**.

---

## 1. Bubble Sort

### Explanation (What to say)
Bubble sort compares adjacent elements and swaps them if they are in the wrong order.  
After each pass, the largest element moves to the end of the array.

### Example
Input: 5 1 4 2  
Pass 1 → 1 4 2 5  
Pass 2 → 1 2 4 5

### C++ Code
```cpp
#include <bits/stdc++.h>
using namespace std;

void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for(int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for(int j = 0; j < n - i - 1; j++) {
            if(arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if(!swapped) break; // optimization
    }
}

2. Insertion Sort
Explanation

Insertion sort works like sorting cards in hand.
We take one element at a time and insert it into its correct position in the sorted left part.

Example

Input: 5 3 4 1
Steps →
5
3 5
3 4 5
1 3 4 5

C++ Code
#include <bits/stdc++.h>
using namespace std;

void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for(int i = 1; i < n; i++) {
        
        int j = i;

        while(j > 0 && arr[j] < arr[j-1]) {
            swap(arr[j],arr[j-1]);
            j--;
        }
    }
}

3. Selection Sort
Explanation

Selection sort selects the minimum element from the unsorted part and places it at the beginning.

Example

Input: 64 25 12 22
Min = 12 → swap with 64
Result → 12 22 25 64

C++ Code
#include <bits/stdc++.h>
using namespace std;

void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for(int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for(int j = i + 1; j < n; j++) {
            if(arr[j] < arr[minIndex])
                minIndex = j;
        }
        swap(arr[i], arr[minIndex]);
    }
}

4. Merge Sort
Explanation

Merge sort follows divide and conquer.
The array is divided into halves until single elements remain, then merged back in sorted order.

Example

Input: 8 3 2 9
Divide → [8 3] [2 9]
Merge → [2 3 8 9]

C++ Code
#include <bits/stdc++.h>
using namespace std;

void merge(vector<int>& arr, int l, int m, int r) {
    vector<int> left(arr.begin() + l, arr.begin() + m + 1);
    vector<int> right(arr.begin() + m + 1, arr.begin() + r + 1);

    int i = 0, j = 0, k = l;
    while(i < left.size() && j < right.size()) {
        if(left[i] <= right[j])
            arr[k++] = left[i++];
        else
            arr[k++] = right[j++];
    }

    while(i < left.size()) arr[k++] = left[i++];
    while(j < right.size()) arr[k++] = right[j++];
}

void mergeSort(vector<int>& arr, int l, int r) {
    if(l >= r) return;
    int m = l + (r - l) / 2;
    mergeSort(arr, l, m);
    mergeSort(arr, m + 1, r);
    merge(arr, l, m, r);
}

5. Quick Sort
Explanation

Quick sort picks a pivot element and partitions the array so that smaller elements go left and larger go right, then recursively sorts both sides.

Example

Input: 10 7 8 9 1
Pivot = 8
Left → 7 1
Right → 10 9

C++ Code
#include <bits/stdc++.h>
using namespace std;

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for(int j = low; j < high; j++) {
        if(arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}
int partition(vector<int>& arr, int low, int high) {
    int pivt = low;
    int st = low + 1;
    int end = high;

    while (st <= end) {
        while (st <= high && arr[st] <= arr[pivt])
            st++;
        while (arr[end] > arr[pivt])
            end--;

        if (st < end)
            swap(arr[st], arr[end]);
    }
    swap(arr[pivt], arr[end]);
    return end;
}

void quickSort(vector<int>& arr, int low, int high) {
    if(low < high) {   // or if(l>=h) return
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```



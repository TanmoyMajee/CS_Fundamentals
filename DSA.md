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



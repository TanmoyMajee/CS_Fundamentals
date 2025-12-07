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

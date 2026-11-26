# Java vs. C++: Strings & Type Conversions Cheat Sheet

A quick-reference guide for C++ developers transitioning to Java, highlighting syntax differences, mutability rules, and common pitfalls in strings and type conversions.

---

## 1. Core String Differences (`std::string` vs. `java.lang.String`)

| Feature | C++ (`std::string`) | Java (`String`) | Key Difference / Gotcha |
| :--- | :--- | :--- | :--- |
| **Mutability** | Mutable (can edit in place) | **Immutable** (cannot be changed) | Every modification creates a new `String` object. Use `StringBuilder` for loops. |
| **Length** | `s.length()` or `s.size()` | `s.length()` | Only `.length()` exists (with parentheses). |
| **Empty Check** | `s.empty()` | `s.isEmpty()` / `s.isBlank()` | `isBlank()` (Java 11+) also returns `true` for whitespace-only strings. |
| **Access Char** | `s[i]` | `s.charAt(i)` | Bracket indexing `s[i]` does **not** compile in Java. |
| **Modify Char** | `s[i] = 'x';` | *Not possible on `String`* | Convert to `char[]` via `s.toCharArray()` or use `StringBuilder`. |
| **Equality** | `s1 == s2` | `s1.equals(s2)` | `==` checks **memory reference**, not string content! |
| **Compare Order**| `s1 < s2` or `s1.compareTo(s2)` | `s1.compareTo(s2)` | `<`, `>`, `<=`, `>=` operators do not work on Java `String`. |
| **Substring** | `s.substr(start, len)` | `s.substring(start, end)` | **Trap:** Java's 2nd param is the **exclusive end index**, not length (`s.substring(i, i + len)`). |
| **Find Substring**| `s.find("ab")` (`string::npos`) | `s.indexOf("ab")` (`-1`) | Java returns `-1` if not found. Also has `s.lastIndexOf("ab")`. |
| **Contains** | `s.find("ab") != string::npos` | `s.contains("ab")` | Built-in boolean check in Java. |
| **Prefix/Suffix**| `s.starts_with("a")` (C++20) | `s.startsWith("a")` / `s.endsWith("z")` | Built-in boolean checks in Java. |
| **Concatenation** | `s += "abc";` (O(1) amortized) | `s += "abc";` (**O(N) copy!**) | `+=` on Java `String` inside a loop is **O(N²)**. Use `StringBuilder`. |
| **Split String** | Manual `stringstream` loop | `s.split(" ")` | Takes a **regex** (escape special chars like `s.split("\\.")`). |
| **Join Strings** | Manual loop | `String.join(",", list)` | Joins an array or `Iterable<String>` with a delimiter. |
| **Trim Spaces** | Manual `erase` / `find_first_not_of` | `s.trim()` or `s.strip()` | `strip()` (Java 11+) is Unicode-aware; both remove leading/trailing whitespace. |
| **Case Change** | `transform(..., ::tolower)` | `s.toLowerCase()` / `s.toUpperCase()` | Returns a new `String`; does not mutate original. |
| **Replace** | `s.replace(pos, len, str)` | `s.replace("old", "new")` | Replaces **all** literal occurrences. (`replaceAll` uses regex). |
| **Repeat** | `string(n, 'a')` | `"a".repeat(n)` | Java 11+ built-in method to repeat a string `n` times. |

---

## 2. Mutable Strings: `StringBuilder` (C++ `std::string` Equivalent)

Whenever you build, modify, or reverse a string dynamically, use `StringBuilder` to achieve **O(1) amortized** appends instead of O(N²) string copying.

```java
StringBuilder sb = new StringBuilder("hello");

// Append (C++: s += " world"; s.push_back('!');)
sb.append(" world");
sb.append('!');

// Read & Modify Character (C++: char c = s; s = 'H';)
char c = sb.charAt(1);
sb.setCharAt(0, 'H');

// Delete (C++: s.pop_back(); s.erase(0, 5);)
sb.deleteCharAt(sb.length() - 1);
sb.delete(0, 5); // Note: end index is exclusive!

// Insert & Reverse (C++: s.insert(0, "Hi"); reverse(s.begin(), s.end());)
sb.insert(0, "Hi");
sb.reverse();

// Convert back to immutable String
String finalStr = sb.toString();
```

---

## 3. Sorting a String (C++ `sort(s.begin(), s.end())`)

Because Java `String` is immutable, you cannot sort it directly. Convert it to a `char[]` first:

```java
String s = "dcba";
char[] arr = s.toCharArray();
Arrays.sort(arr);
String sorted = new String(arr); // "abcd"
```

---

## 4. Character Operations (`<cctype>` vs. `java.lang.Character`)

C++ uses global functions on `char`; Java uses static methods on the `Character` wrapper class.

| Operation | C++ (`<cctype>`) | Java (`Character`) |
| :--- | :--- | :--- |
| **Is Digit (`0-9`)** | `isdigit(ch)` | `Character.isDigit(ch)` |
| **Is Letter (`a-z, A-Z`)** | `isalpha(ch)` | `Character.isLetter(ch)` |
| **Is Alphanumeric** | `isalnum(ch)` | `Character.isLetterOrDigit(ch)` |
| **Is Whitespace** | `isspace(ch)` | `Character.isWhitespace(ch)` |
| **Is Upper / Lower** | `isupper(ch)` / `islower(ch)` | `Character.isUpperCase(ch)` / `Character.isLowerCase(ch)` |
| **To Upper / Lower** | `toupper(ch)` / `tolower(ch)` | `Character.toUpperCase(ch)` / `Character.toLowerCase(ch)` |
| **Char Digit to Int** | `ch - '0'` | `ch - '0'` or `Character.getNumericValue(ch)` |
| **Int Digit to Char** | `digit + '0'` | `(char) (digit + '0')` *(explicit cast required!)* |

---

## 5. Type Conversions (C++ vs. Java)

Java is strictly typed and does **not** allow implicit narrowing conversions (e.g., `long` to `int` or `double` to `int` requires an explicit cast).

### A. String $\leftrightarrow$ Numbers

| Conversion | C++ | Java | Notes |
| :--- | :--- | :--- | :--- |
| **`String` $\rightarrow$ `int`** | `stoi(s)` | `Integer.parseInt(s)` | Throws `NumberFormatException` if invalid. |
| **`String` $\rightarrow$ `long`** | `stoll(s)` | `Long.parseLong(s)` | Use for 64-bit integers. |
| **`String` $\rightarrow$ `double`**| `stod(s)` | `Double.parseDouble(s)` | Parses floating-point strings. |
| **Number $\rightarrow$ `String`**| `to_string(num)` | `String.valueOf(num)` | Also works via `Integer.toString(num)` or `"" + num`. |
| **`char` $\rightarrow$ `String`** | `string(1, ch)` | `String.valueOf(ch)` | Also `Character.toString(ch)`. |
| **`char[]` $\rightarrow$ `String`**| `string(arr, len)` | `new String(arr)` | Or `String.valueOf(arr)`. |
| **Binary/Hex String $\rightarrow$ `int`** | `stoi(s, nullptr, 2)` | `Integer.parseInt(s, 2)` | Pass radix (`2`, `8`, `16`) as 2nd argument. |
| **`int` $\rightarrow$ Binary/Hex String** | `bitset<32>(n).to_string()` | `Integer.toBinaryString(n)` | Also `Integer.toHexString(n)`, `Integer.toOctalString(n)`. |

# C++ vs. Java: Custom Sorting, Maps & PriorityQueues

A side-by-side reference comparing C++ STL and Java Collections for custom sorting, iterating over maps, converting map entries to lists, and using custom `PriorityQueue` heaps.

---

## 1. Custom Sorting Rules: C++ vs. Java

Before writing comparators, remember the fundamental mental model shift between C++ and Java:

* **C++ (`bool` comparator):** Return `true` if `a` should come **before** `b` (`a < b` for ascending).
* **Java (`int` comparator):**
  * Return **negative (`< 0`)** if `a` comes **before** `b`.
  * Return **positive (`> 0`)** if `a` comes **after** `b`.
  * Return **`0`** if `a` and `b` are equal.
  * *Safe shortcut:* Always use `Integer.compare(a, b)` instead of `a - b` to prevent integer overflow.

---

## 2. Sorting Lists / Vectors (`int` and `String`)

### A. Basic Ascending & Descending Sort

**C++ (`std::vector`)**
```cpp
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

// Integers
vector<int> nums = {5, 1, 4, 2};
sort(nums.begin(), nums.end());                  // Ascending: {1, 2, 4, 5}
sort(nums.begin(), nums.end(), greater<int>());  // Descending: {5, 4, 2, 1}

// Strings
vector<string> words = {"banana", "apple", "cherry"};
sort(words.begin(), words.end());                 // Lexicographical ascending
sort(words.begin(), words.end(), greater<string>()); // Descending
```

**Java (`List` / `ArrayList`)**
```java
import java.util.*;

// Integers
List<Integer> nums = new ArrayList<>(Arrays.asList(5, 1, 4, 2));
Collections.sort(nums);                            // Ascending: [1, 2, 4, 5]
nums.sort(Collections.reverseOrder());             // Descending: [5, 4, 2, 1]

// Strings
List<String> words = new ArrayList<>(Arrays.asList("banana", "apple", "cherry"));
Collections.sort(words);                           // Lexicographical ascending
words.sort(Collections.reverseOrder());            // Descending
```

### B. Custom Sorting (e.g., Sort Strings by Length, Tie-break Lexicographically)

**C++**
```cpp
vector<string> words = {"cat", "apple", "bat", "an"};

sort(words.begin(), words.end(), [](const string& a, const string& b) {
    if (a.length() != b.length()) {
        return a.length() < b.length(); // Shorter length first
    }
    return a < b;                       // Tie-breaker: alphabetical
});
// Result: {"an", "bat", "cat", "apple"}
```

**Java**
```java
List<String> words = new ArrayList<>(Arrays.asList("cat", "apple", "bat", "an"));

words.sort((a, b) -> {
    if (a.length() != b.length()) {
        return Integer.compare(a.length(), b.length()); // Shorter length first
    }
    return a.compareTo(b);                              // Tie-breaker: alphabetical
});
// Result: ["an", "bat", "cat", "apple"]
```

---

## 3. Iterating Over a Map

**C++ (`unordered_map<char, int>`)**
```cpp
unordered_map<char, int> mp = {{'a', 3}, {'b', 1}, {'c', 2}};

// 1. Structured bindings (C++17 - cleanest)
for (auto& [key, val] : mp) {
    cout << key << " -> " << val << "\n";
}

// 2. Pair reference
for (auto& it : mp) {
    cout << it.first << " -> " << it.second << "\n";
}
```

**Java (`Map<Character, Integer>`)**
```java
Map<Character, Integer> map = new HashMap<>();
map.put('a', 3); map.put('b', 1); map.put('c', 2);

// 1. entrySet() loop (Best & fastest way)
for (Map.Entry<Character, Integer> it : map.entrySet()) {
    System.out.println(it.getKey() + " -> " + it.getValue());
}

// 2. keySet() loop (Slightly slower due to extra .get() lookup)
for (Character key : map.keySet()) {
    System.out.println(key + " -> " + map.get(key));
}

// 3. forEach lambda (Java 8+)
map.forEach((key, val) -> System.out.println(key + " -> " + val));
```

---

## 4. Storing Map Entries in a List & Custom Sorting

### C++ (`vector<pair<char, int>>`)
```cpp
unordered_map<char, int> mp = {{'b', 2}, {'a', 2}, {'c', 5}};

// 1. Copy map entries into vector<pair<char, int>>
vector<pair<char, int>> vec(mp.begin(), mp.end());

// OR manually push_back:
// vector<pair<char, int>> vec;
// for (auto& [k, v] : mp) vec.push_back({k, v});

// 2. Sort: Higher value first; tie-breaker: smaller key first
sort(vec.begin(), vec.end(), [](const pair<char, int>& a, const pair<char, int>& b) {
    if (a.second != b.second) {
        return a.second > b.second; // Higher frequency first
    }
    return a.first < b.first;       // Smaller char first
});

// 3. Print
for (auto& [k, v] : vec) {
    cout << k << ": " << v << "\n";
}
```

### Java (`List<Map.Entry<Character, Integer>>`)
```java
Map<Character, Integer> map = new HashMap<>();
map.put('b', 2); map.put('a', 2); map.put('c', 5);

// 1. Convert entrySet() directly in constructor (Fastest)
List<Map.Entry<Character, Integer>> li = new ArrayList<>(map.entrySet());

// OR populate manually in a loop (3 ways):
List<Map.Entry<Character, Integer>> manualList = new ArrayList<>();
for (Map.Entry<Character, Integer> it : map.entrySet()) {
    manualList.add(it);                                            // Option A: Add existing entry
    // manualList.add(Map.entry(it.getKey(), it.getValue()));      // Option B: Immutable copy (Java 9+)
    // manualList.add(new AbstractMap.SimpleEntry<>(it.getKey(), it.getValue())); // Option C: Mutable copy
}

// 2. Sort: Higher value first; tie-breaker: smaller key first
// CRITICAL: Use .equals() for Integer comparison, NOT ==
li.sort((a, b) -> {
    if (!a.getValue().equals(b.getValue())) {
        return Integer.compare(b.getValue(), a.getValue()); // Higher frequency first
    }
    return Character.compare(a.getKey(), b.getKey());       // Smaller char first
});

// 3. Print
for (Map.Entry<Character, Integer> entry : li) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

#### Bonus: Storing Swapped `<Frequency, Character>` Entries in Java
If you want the frequency to be the key (`Map.Entry<Integer, Character>`):
```java
List<Map.Entry<Integer, Character>> swappedList = new ArrayList<>();
for (Map.Entry<Character, Integer> it : map.entrySet()) {
    swappedList.add(Map.entry(it.getValue(), it.getKey()));
}

swappedList.sort((a, b) -> {
    if (a.getKey().equals(b.getKey())) {
        return Character.compare(a.getValue(), b.getValue()); // Smaller char first
    }
    return Integer.compare(b.getKey(), a.getKey());           // Higher freq first
});
```

---

## 5. PriorityQueue Basics (Min-Heap vs. Max-Heap)

> **Crucial Difference:** C++ `priority_queue` defaults to a **Max-Heap**. Java `PriorityQueue` defaults to a **Min-Heap**.

### C++ (`std::priority_queue`)
```cpp
#include <queue>
#include <vector>
using namespace std;

// 1. Default Max-Heap (Largest on top)
priority_queue<int> maxHeap;
maxHeap.push(10);
maxHeap.push(30);
int topMax = maxHeap.top(); // 30
maxHeap.pop();              // Removes 30

// 2. Min-Heap (Smallest on top)
priority_queue<int, vector<int>, greater<int>> minHeap;
minHeap.push(10);
minHeap.push(30);
int topMin = minHeap.top(); // 10
minHeap.pop();              // Removes 10
```

### Java (`java.util.PriorityQueue`)
```java
import java.util.*;

// 1. Default Min-Heap (Smallest on top)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(10);
minHeap.offer(30);
int topMin = minHeap.peek(); // 10
minHeap.poll();              // Removes & returns 10

// 2. Max-Heap (Largest on top)
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
// Alternative lambda: new PriorityQueue<>((a, b) -> Integer.compare(b, a));
maxHeap.offer(10);
maxHeap.offer(30);
int topMax = maxHeap.peek(); // 30
maxHeap.poll();              // Removes & returns 30
```

---

## 6. Custom PriorityQueue & PQ with Map Entries

> **C++ vs. Java Heap Comparator Trap:**
> * In **C++**, a custom `struct` comparator behaves opposite to `sort()` (`a.val < b.val` creates a **Max-Heap**).
> * In **Java**, a `PriorityQueue` comparator behaves **identically** to `list.sort()`—whatever comes "first" in the comparator sits at the **top (`peek()`)** of the heap!

### C++ (`priority_queue` with `pair<char, int>`)
```cpp
struct Compare {
    bool operator()(const pair<char, int>& a, const pair<char, int>& b) {
        // Return true if 'a' has LOWER priority than 'b' (goes deeper in heap)
        if (a.second != b.second) {
            return a.second < b.second; // Higher frequency stays at top
        }
        return a.first > b.first;       // Smaller char stays at top on tie
    }
};

unordered_map<char, int> mp = {{'b', 2}, {'a', 2}, {'c', 5}};
priority_queue<pair<char, int>, vector<pair<char, int>>, Compare> pq;

for (auto& [k, v] : mp) {
    pq.push({k, v});
}

while (!pq.empty()) {
    auto [ch, freq] = pq.top();
    pq.pop();
    cout << ch << " -> " << freq << "\n";
}
```

### Java (`PriorityQueue` with `Map.Entry<Character, Integer>`)
```java
Map<Character, Integer> map = new HashMap<>();
map.put('b', 2); map.put('a', 2); map.put('c', 5);

// Top of PQ = element that comes FIRST according to comparator
PriorityQueue<Map.Entry<Character, Integer>> pq = new PriorityQueue<>((a, b) -> {
    if (a.getValue().equals(b.getValue())) {
        return Character.compare(a.getKey(), b.getKey());   // Smaller char at top
    }
    return Integer.compare(b.getValue(), a.getValue());     // Higher freq at top
});

// Insert all entries at once:
pq.addAll(map.entrySet());


#### 1. Using `namespace std`
In C++, the standard library is in the `std` namespace. Writing `using namespace std;` means you don't have to write `std::` before standard components like `vector`, `string`, or `cout`.

#### 2. The `::` (Scope Resolution Operator)
The `::` operator is the scope resolution operator. It tells the compiler where to look for a name. For example, `std::vector` means "look for `vector` in the `std` namespace."

#### 3. Iterators
An iterator is basically like a pointer used to go through a list of elements in a container, like a vector or a map

```cpp
vector<int> myVec = {10, 20, 30};
for (vector<int>::iterator it = myVec.begin(); it != myVec.end(); ++it) {
    cout << *it << " ";
}
```


#### Call by Value
When you use call by value, you pass a copy of the variable to the function. This means any changes made inside the function do not affect the original variable outside the function.

#### Call by Reference
When you use call by reference, you pass the actual variable by reference using the `&` operator. This means any changes made inside the function will affect the original variable outside the function.

#### Example Code

```cpp
#include <iostream>

void callByValue(int x) {
    x = x + 10; // Does not affect the original variable
}

void callByReference(int &y) {
    y = y + 10; // Affects the original variable
}
```

# Memory Management: Stack vs. Heap

## 1. Stack Memory
**Definition:** Stack memory is used for **static memory allocation**. It follows the **LIFO** (Last-In, First-Out) principle.


 **Example:** > `int x = 10; // Stored on the stack`



## 2. Heap Memory
**Definition:** Heap memory is used for **dynamic memory allocation**. It is a large pool of memory used for objects and data that need to outlive a single function call.




* **Allocation:** Manual (managed by the programmer). In C++, use `new` to allocate and `delete` to free. In languages like Java/Python, the Garbage Collector handles the freeing.


### Stack vs Heap (Very Short)

| Stack Memory                         | Heap Memory                              |
|--------------------------------------|-------------------------------------------|
| Small, fixed-size data               | Large or variable-size data               |
| Known at compile time                | Size decided at runtime                   |
| Short lifetime (inside function)     | Can live beyond function                  |
| Very fast                            | Slower                                    |
| Automatic allocation/deallocation    | Manual or smart pointer needed            |
| Examples: local variables, params    | Examples: dynamic arrays, linked lists    |


## 🔹 Pointer vs Reference in C++

### ✅ Pointer — Definition
A **pointer** is a variable that stores the **memory address of another variable**.

### 👉 Key Points
- Declared using `*`
- Needs dereferencing (`*ptr`) to access value
- Can be reassigned to point to another variable
- Can be `NULL`

### 🎯 Simple Use Case (Very common in interviews)
Used in **linked list, trees, graphs** where we store **addresses** of nodes.

---

### ✅ Reference — Definition
A **reference** is an **alias (second name)** of an existing variable.

### 👉 Key Points
- Declared using `&` in function parameter or reference declaration
- Cannot be reseated (must be initialized once)
- No need to dereference
- Cannot be `NULL`

### 🎯 Simple Use Case
Used to **avoid copying large variables** when passing to functions  
→ very useful in **recursion & competitive programming**

---
### 🔍 Key Difference Between Pointer and Reference

- **Pointer can be reassigned**
  - A pointer can change what it points to.
  - Example: it can first point to `a`, then later to `b`.

- **Reference cannot be reassigned**
  - Once a reference is bound to a variable, it cannot refer to another variable.
  - It is like a permanent alias (second name) of that variable.

```cpp
#include <iostream>
using namespace std;

// function using reference (no copy happens)
void incrementByRef(int &refVar) {
    refVar++; // changes original variable
}

int main() {

    // ----- Pointer Example -----
    int a = 10;
    int b = 20;

    int *ptr;   // pointer declaration
    ptr = &a;   // pointer points to a
    cout << "Pointer pointing to a: " << *ptr << endl;

    ptr = &b;   // pointer now points to b (can change target)
    cout << "Pointer now pointing to b: " << *ptr << endl;


    // ----- Reference Example -----
    int x = 5;
    cout << "Before increment x = " << x << endl;

    incrementByRef(x);   // pass by reference (no copy)
    cout << "After increment x = " << x << endl;

    return 0;
}
````

## 🔹 Memory Leak in C++

### ✅ Definition
A **memory leak** occurs when dynamically allocated memory is not properly deallocated, causing the program to lose the reference to that memory. Over time, this leads to increased memory usage and can eventually cause the program to crash.

### 🎯 Simple Use Case
- Happens when `new` is used to allocate memory but `delete` is never called.
- Common in long-running applications where memory is continuously allocated without freeing it.

### 📝 How to Prevent Memory Leaks
- Always pair `new` with `delete`.
- Use smart pointers like `std::unique_ptr` or `std::shared_ptr` which automatically manage memory.

---

### 🧩 Simple Example

```cpp
#include <iostream>

int main() {
    int* ptr = new int(10); // Memory allocated on the heap

    // Forgetting to delete ptr leads to a memory leak
    // delete ptr; // This line is missing

    return 0; // ptr goes out of scope, but the allocated memory is not freed
}
```
## 🔹 smart pointer
A smart pointer is an object in C++ that automatically manages memory for a pointer. It frees the memory on its own when it is no longer needed, so we don’t have to remember to call delete, which helps prevent memory leaks.
std::unique_ptr , std::shared_ptr


## ✅ 10. Inline Function

### 🔹 Definition
An **inline function** is a function whose code is **expanded at the place where it is called**, instead of performing a normal function call.  
👉 This helps to **avoid function call overhead  / fast execution **.

### 🔹 Example (simple)

```cpp
inline int square(int x) {
    return x * x;
}

int main() {
    int result = square(5);  // code expanded here
}
```


## ✅ 11. Friend Function & Friend Class

### 🔹 Friend Function — Definition
A **friend function** is a non-member function that is allowed to **access private and protected data** of a class.

### 🔹 Simple Example

```cpp
class Student {
private:
    int marks;

public:
    Student(int m) : marks(m) {}

    friend void show(Student s);   // friend function
};

void show(Student s) {
    // allowed to access private member
    cout << s.marks;
}
````
### 🔹🔹 Friend Class — Definition

A friend class can access private, protected, and public members of another class.

🔹 Simple Example
```cpp
class Student {
private:
    int marks = 95;

    friend class Teacher;   // Teacher is friend
};

class Teacher {
public:
    void revealMarks(Student s) {
        cout << s.marks;   // allowed private access
    }
};
````




Shalow & deep copy

copy constructor

 What are the static members and static member functions?


OOPS Concepts: Encapsulation, Inheritance, Polymorphism, Abstraction in C++

Constructors and Destructors: How they work, overloading constructors, object lifetimes

Polymorphism: Both compile-time (overloading) and runtime (virtual functions)

Memory Management: new and delete, stack vs. heap, avoiding memory leaks

STL Basics: We've covered iterators, but also know about vector, list, map, and their use cases

Data Structures & Algorithms: Arrays, linked lists, sorting and searching basics

Pointers and References: Pointer arithmetic, differences between pointers and references

Exception Handling: Basic try, catch, throw in C++

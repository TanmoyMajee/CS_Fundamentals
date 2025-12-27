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


## 🔹 What is a Lambda Function in C++?

### Definition
A **lambda function** in C++ is an anonymous, inline function that you can define right where you need it. It’s typically used for short operations like custom sorting or passing a small function to an algorithm.

### Lambda Syntax
```cpp
[capture](parameters) -> return_type {
    // function body
};
int main() {
    vector<pair<int, int>> vec = {{1, 4}, {2, 3}, {3, 2}, {4, 1}};

    // Sort by the second element of the pair using a lambda function
    sort(vec.begin(), vec.end(), [](const pair<int, int>& a, const pair<int, int>& b) {
        return a.second < b.second;
    });

    for (const auto &p : vec) {
        cout << p.first << ", " << p.second << endl;
    }

    return 0;
}
```

## 🔹 What is a `stringstream`?

A **`std::stringstream`** is a C++ class that allows you to treat a string as a stream. It is commonly used to split a string into tokens based on a delimiter.

### How to Use It
```cpp
while (ss >> word) {
        tokens.push_back(word);
    }

    while (getline(ss, word, ' ')) {
        tokens.push_back(word);
    }

````

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
## ✅ Static data members & static member functions (C++)

Static is a keyword in C++ that means a variable or function belongs to the class itself, not to any specific object. Basically, it makes sure there's only one copy of that variable for the whole class, shared by all objects

### 🔹 Static Data Member (static variable inside class)
A **static data member** is shared by **all objects of a class** — only **one common copy** exists for the entire class.

### 🔹 Static Member Function
A **static member function** can be **called without creating an object** and it can access **only static members** of the class.
````cpp
#include <iostream>
using namespace std;

class Student {
public:
    static int count;   // static data member

    Student() {
        count++;        // increases for every object
    }

    static void showCount() {   // static member function
        cout << "Total students: " << count << endl;
    }
};

// definition of static data member
int Student::count = 0;

int main() {
    Student s1;
    Student s2;
    Student s3;

    Student::showCount();   // called without object
    return 0;
}
````

# ✅ C++ Exception Handling — Complete Interview Notes

## ⭐ What is Exception Handling?
An exception is basically an unexpected situation or error that happens at runtime and can interrupt the normal flow of the program. To handle it, we use a try-catch block. We put any code that might cause an exception inside the try block. If something goes wrong, we throw an exception, and then we catch it in the catch block to handle it gracefully



---



## 🧩 Basic Example

```cpp
#include <iostream>
#include <exception>
using namespace std;

class NegativeNumberException : public exception {
public:
    const char* what() const noexcept override {
        return "Number cannot be negative";
    }
};

int main() {
    try {
        int n = -5;

        if (n == 0)
            throw "Number is zero";               // caught as const char*

        if (n < 0)
            throw NegativeNumberException();      // caught as std::exception
    }
    catch (const char* msg) {  //const char* in the catch block because the thrown error message is a constant C-style string, which is just a sequence of characters
        cout << "String exception: " << msg << endl;
    }
    catch (const exception& e) { // exception type is object
        cout << "Exception object: " << e.what();
    }
}

```

 ## Operator Overloading?
Operator overloading means giving a new meaning to an operator (like +, -, *, []) when it is used with objects.  // it is used to perform secipif task with 2 diff objects
```cpp
#include <iostream>
using namespace std;

class Point {
private:
    int x, y;

public:
    Point(int a, int b) : x(a), y(b) {}

    bool operator==(const Point& p) const {
        return (x == p.x && y == p.y);
    }
};

int main() {
    Point p1(2, 3);
    Point p2(2, 5);

    if (p1 == p2)
        cout << "Both points are equal";
    else
        cout << "Points are not equal";

    return 0;
}

```

## copy constructor
 A Copy constructor is an overloaded
constructor used to declare and initialize an object from another
object. It is of two types - default copy constructor and user
defined copy constructor  , it take a ref of an object in the paramets , then copy from it 

## Shallow Copy vs Deep Copy

### Shallow Copy
- A shallow copy duplicates the object, but both the original and the copied object share the same memory for any referenced fields (e.g., pointers).
- Changes made to one object will reflect in the other because they both point to the same memory location.

### Deep Copy
- A deep copy creates a completely independent copy of the original object.
- Both objects have their own separate memory, so changes in one object do not affect the other.

# Virtual Functions, Pure Virtual Functions, Abstract Class – C++

## 1. What is a Virtual Function?
A virtual function is a function in a base class that is declared using the `virtual` keyword and is meant to be overridden in the derived class.

### Key Points
- Enables **run-time polymorphism**
- Function call is resolved at **runtime** (dynamic binding)
- Declared with `virtual` in the base class
- Overridden in the derived class

### Interview Line
A virtual function allows a derived class to provide its own implementation, and the correct function is called based on the object type at runtime.

---

## 2. What is a Pure Virtual Function?
A pure virtual function is a virtual function that has **no definition in the base class** and is assigned `= 0`.

### Key Points
- Only declared, not defined in base class
- Must be overridden in derived class
- Makes the class **abstract**
- Syntax: `virtual void func() = 0;`

### Interview Line
A pure virtual function forces derived classes to implement that function.

---

## 3. What is an Abstract Class?
An abstract class is a class that contains at least **one pure virtual function**.

### Key Points
- Cannot be instantiated (no objects allowed)
- Used as a **base class**
- Derived class must implement pure virtual functions
- Supports **polymorphism**

### Interview Line
An abstract class works as a blueprint for other classes.

---

## 4. Difference: Virtual vs Pure Virtual Function

| Virtual Function | Pure Virtual Function |
|------------------|-----------------------|
| Can have body/definition | No body in base class |
| Optional to override | Must be overridden |
| Does not make class abstract | Makes class abstract |
| Object of class can be created | Object cannot be created |

---

## 5. How Polymorphism is Achieved Using Virtual Functions?
- Base class pointer points to derived class object
- Function call resolves at runtime
- Ensures correct overridden method is executed

### Interview Line
Virtual functions enable runtime polymorphism by deciding which function to call during execution instead of compile time.

---

## 6. Can We Instantiate an Abstract Class?
No, we **cannot create objects** of an abstract class.

### Why?
Because implementation is incomplete (pure virtual function not defined).

### What we can do?
- Use pointer/reference of abstract class
- Create objects of derived class instead

---

## 7. What is a Virtual Destructor and Why is it Needed?
A virtual destructor ensures the **derived class destructor is called** when deleting an object through a base class pointer.

### Interview Line
A virtual destructor prevents memory leaks in polymorphic base classes.

---

## 8. How is Interface Implemented in C++?
C++ does not have a separate `interface` keyword like Java.

### Interface in C++ is implemented using:
- **Abstract class**
- **All pure virtual functions**

### Key Points
- No data members usually
- Only function declarations
- Derived class must implement all methods

### Interview Line
In C++, an interface is implemented using an abstract class with all pure virtual functions.

---






OOPS Concepts: Encapsulation, Inheritance, Polymorphism, Abstraction in C++

Constructors and Destructors: How they work, overloading constructors, object lifetimes

Polymorphism: Both compile-time (overloading) and runtime (virtual functions)

Memory Management: new and delete, stack vs. heap, avoiding memory leaks

STL Basics: We've covered iterators, but also know about vector, list, map, and their use cases

Data Structures & Algorithms: Arrays, linked lists, sorting and searching basics

Pointers and References: Pointer arithmetic, differences between pointers and references

Exception Handling: Basic try, catch, throw in C++

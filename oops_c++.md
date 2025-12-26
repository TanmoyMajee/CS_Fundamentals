# 🛒 OOPS Concepts in C++ — E-Commerce Example

---

## 1️⃣ Encapsulation

### ✔ Definition
Encapsulation means **wrapping data and methods together** and **restricting direct access** using `private` members and `public` getters/setters.

### ✔ Example — Product Class

```cpp
#include <iostream>
#include <string>
using namespace std;

class Product {
private:
    string name;
    double price;

public:
    Product(string name, double price) {
        this->name = name;
        this->price = price;
    }

    // Getters and setters
    string getName() {
        return name;
    }

    void setName(string name) {
        this->name = name;
    }

    double getPrice() {
        return price;
    }

    void setPrice(double price) {
        this->price = price;
    }

    // virtual -> to allow overriding
    virtual string getProductType() {
        return "General Product";
    }
};

🗨 Interview Explanation

    “Encapsulation protects data by making variables private and exposing them through public getter and setter functions.”

2️⃣ Inheritance
✔ Definition

Inheritance allows a class to reuse properties and behavior of another class.

👉 promotes code reusability
✔ Example — Derived Product Types

class ElectronicProduct : public Product {
private:
    int warrantyPeriod;

public:
    ElectronicProduct(string name, double price, int warrantyPeriod)
        : Product(name, price) {
        this->warrantyPeriod = warrantyPeriod;
    }

    int getWarrantyPeriod() {
        return warrantyPeriod;
    }

    string getProductType() override {
        return "Electronic Product";
    }
};

class ClothingProduct : public Product {
private:
    string size;

public:
    ClothingProduct(string name, double price, string size)
        : Product(name, price) {
        this->size = size;
    }

    string getSize() {
        return size;
    }

    string getProductType() override {
        return "Clothing Product";
    }
};

🗨 Interview Explanation

    “Inheritance lets us reuse Product and create specialized products like ElectronicProduct and ClothingProduct.”

3️⃣ Polymorphism
✔ Definition

Polymorphism means same function name behaving differently depending on object.

Types:

    Compile-time polymorphism → function overloading

    Runtime polymorphism → function overriding (via virtual)

🅰 Runtime Polymorphism (Overriding)

Product* p1 = new ElectronicProduct("Laptop", 80000, 2);
Product* p2 = new ClothingProduct("T-Shirt", 1500, "M");

cout << p1->getProductType() << endl;  // Electronic Product
cout << p2->getProductType() << endl;  // Clothing Product

👉 same function → different output → runtime decision
🅱 Compile-time Polymorphism (Overloading)

class Order {
public:
    void placeOrder(Product &product) {
        cout << "Order placed for " << product.getName() << endl;
    }

    void placeOrder(Product &product, int quantity) {
        cout << "Order placed for " << quantity
             << " x " << product.getName() << endl;
    }
};

🗨 Interview Explanation

    “Overriding gives runtime polymorphism, overloading gives compile-time polymorphism.”

4️⃣ Abstraction
✔ Definition

Abstraction means hiding implementation details and exposing only essential behavior.

👉 achieved using abstract classes (pure virtual functions) in C++
✔ Example — Payment Abstraction

class Payment {
public:
    virtual bool processPayment(double amount) = 0; // pure virtual
};

✔ Implementation Example

class CreditCardPayment : public Payment {
private:
    double balance;

public:
    CreditCardPayment(double balance) {
        this->balance = balance;
    }

    bool processPayment(double amount) override {
        if (amount <= balance) {
            balance -= amount;
            cout << "Payment successful: " << amount << endl;
            cout << "Remaining balance: " << balance << endl;
            return true;
        } else {
            cout << "Payment failed: Insufficient balance" << endl;
            return false;
        }
    }
};

🗨 Interview Explanation

    “Abstraction hides payment implementation. User just calls processPayment without knowing internal logic.”

🧪 Demo Program (Main Function)

int main() {
    Product* product = new ElectronicProduct("Laptop", 80000, 2);

    Order order;
    order.placeOrder(*product, 1);

    Payment* payment = new CreditCardPayment(100000);
    payment->processPayment(product->getPrice());

    delete product;
    delete payment;

    return 0;
}
```

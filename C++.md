# C++ — Complete Notes for AI/ML Engineers
### Interview-Ready | Placement-Focused | Python Comparisons Throughout

> **How to use this:** Every section is written so the concept explanation *is* the answer to the interview question at the top of each block. Python comparisons appear wherever they add clarity. As an AI/ML engineer you know Python deeply — use that as your mental anchor for C++ concepts.

---

## Table of Contents

1. [Why C++ for AI/ML Engineers?](#1-why-c-for-aiml-engineers)
2. [C++ vs Python — The Master Comparison](#2-c-vs-python--the-master-comparison)
3. [Basics — Data Types, Variables, I/O](#3-basics--data-types-variables-io)
4. [Pointers & References](#4-pointers--references)
5. [Memory Management — Stack, Heap, RAII, Smart Pointers](#5-memory-management--stack-heap-raii-smart-pointers)
6. [Object-Oriented Programming (OOP)](#6-object-oriented-programming-oop)
7. [Inheritance & Polymorphism](#7-inheritance--polymorphism)
8. [Templates & Generic Programming](#8-templates--generic-programming)
9. [STL — Standard Template Library](#9-stl--standard-template-library)
10. [Move Semantics & Rvalue References (C++11)](#10-move-semantics--rvalue-references-c11)
11. [Lambdas, auto, range-for (Modern C++)](#11-lambdas-auto-range-for-modern-c)
12. [Exception Handling](#12-exception-handling)
13. [Multithreading & Concurrency](#13-multithreading--concurrency)
14. [Compilation, Linking & Build Systems](#14-compilation-linking--build-systems)
15. [C++ in ML/AI — Where It Actually Appears](#15-c-in-mlai--where-it-actually-appears)
16. [Interview Q&A — Fundamentals](#16-interview-qa--fundamentals)
17. [Interview Q&A — OOP & Polymorphism](#17-interview-qa--oop--polymorphism)
18. [Interview Q&A — Memory & Pointers](#18-interview-qa--memory--pointers)
19. [Interview Q&A — STL & Templates](#19-interview-qa--stl--templates)
20. [Interview Q&A — Modern C++ & Advanced](#20-interview-qa--modern-c--advanced)
21. [Quick Reference Cheat Sheet](#21-quick-reference-cheat-sheet)

---

## 1. Why C++ for AI/ML Engineers?

> **Interview Q:** *Why should an ML engineer know C++?*

You write Python. PyTorch, TensorFlow, NumPy, OpenCV, LightGBM — all written in C++. When you call `torch.matmul()`, you're calling C++ (with CUDA) under the hood. When you build a real-time inference engine, deploy on edge hardware, write a custom CUDA kernel, extend PyTorch with a C++ extension, or optimise a model for sub-millisecond latency — you're in C++ territory.

**C++ shows up in AI/ML in these places:**
- **Inference engines:** TensorRT, ONNX Runtime, llama.cpp — all C++
- **Deep learning frameworks:** PyTorch C++ API (LibTorch), TensorFlow C++
- **Computer vision:** OpenCV is C++; Python bindings are thin wrappers
- **Custom ops:** Writing a custom CUDA kernel for a novel attention mechanism → C++/CUDA
- **Edge/embedded AI:** TFLite runtime, Coral, Arduino ML, robotics (ROS) — C++
- **Data pipelines at extreme scale:** Apache Arrow, Apache Parquet internals — C++
- **Chess / game engines:** (Relevant for your DeepCastle — Stockfish is C++)

---

## 2. C++ vs Python — The Master Comparison

> **Interview Q:** *Compare C++ and Python. When would you choose one over the other?*

| Dimension | C++ | Python |
|---|---|---|
| **Execution** | Compiled → machine code → runs directly on CPU | Interpreted (CPython) → bytecode → interpreted at runtime |
| **Speed** | 10–100× faster for CPU-bound tasks | Slow for raw compute; fast via NumPy/C extensions |
| **Memory** | Manual management (or smart pointers); full control | Automatic garbage collection (reference counting + cyclic GC) |
| **Type system** | Statically typed — types checked at compile time | Dynamically typed — types checked at runtime |
| **Syntax** | Verbose, explicit, semicolons, braces | Concise, readable, whitespace-based |
| **Compilation** | Must compile before running; catches type errors early | No compilation step; errors only at runtime |
| **Pointers** | Yes — direct memory address manipulation | No raw pointers; everything is a reference |
| **OOP** | Full OOP + multi-inheritance + operator overloading | Full OOP but simpler; dunder methods for overloading |
| **Concurrency** | True parallelism with threads (no GIL) | GIL limits true thread parallelism; use `multiprocessing` |
| **Templates** | Compile-time generics (zero overhead) | Duck typing — generics via duck typing, no overhead |
| **STL** | Rich: vector, map, set, priority_queue, algorithms | Rich: list, dict, set, heapq, collections, itertools |
| **Use in ML** | Frameworks internals, inference engines, edge | Model training, experimentation, data pipelines, scripts |
| **Learning curve** | Steep — UB, memory bugs, complex syntax | Gentle — readable, forgiving |
| **Ecosystem** | Smaller ML ecosystem | Dominant ML ecosystem (PyPI, HuggingFace, Jupyter) |

### The Speed Story

```
Python raw loop:    10 million iterations → ~1 second
C++ loop:           10 million iterations → ~0.01 seconds (100× faster)

But:
NumPy vectorised:   10 million element multiply → ~10ms (calls C under the hood)
C++ with SIMD:      10 million element multiply → ~2ms

Rule: Python's speed bottleneck is the interpreter overhead per operation.
      Vectorised Python (NumPy, Pandas, PyTorch) sidesteps this by calling C/C++.
      For custom logic not covered by these libraries → C++ wins.
```

### Memory Model Comparison

```python
# Python: everything is a heap object; GC manages lifetime
x = [1, 2, 3]       # allocated on heap, x is a reference
y = x                # y points to SAME list (no copy)
y.append(4)          # x is now [1, 2, 3, 4] — shared reference
```

```cpp
// C++: you choose stack vs heap; you manage lifetime
int arr[] = {1, 2, 3};    // stack: auto-freed when scope ends
int* p = new int[3];      // heap: you must call delete[]
std::vector<int> v = {1, 2, 3};  // heap-managed by vector's destructor
```

### GIL — The Critical Difference for Parallelism

Python has the **Global Interpreter Lock (GIL)** — only one thread runs Python bytecode at a time. True CPU parallelism requires `multiprocessing` (separate processes). C++ has no GIL — threads run truly in parallel on multiple cores. This matters for:
- Real-time inference servers handling concurrent requests
- Parallel data preprocessing pipelines
- Training custom operators

---

## 3. Basics — Data Types, Variables, I/O

### Data Types

```cpp
// Primitive types
int     x = 42;            // 4 bytes, -2.1B to 2.1B
long    y = 1234567890L;   // 8 bytes on most systems
float   f = 3.14f;         // 4 bytes, ~7 decimal digits precision
double  d = 3.14159265;    // 8 bytes, ~15 decimal digits (use for ML floats)
char    c = 'A';           // 1 byte
bool    b = true;          // 1 byte
```

**Python equivalent mental model:**
```
C++ int    ↔  Python int (but Python int is unlimited precision)
C++ double ↔  Python float (Python float IS a C double under the hood)
C++ char   ↔  Python str of length 1
C++ bool   ↔  Python bool
C++ void   ↔  Python None (as a return type meaning "returns nothing")
```

### Type Modifiers

```cpp
unsigned int u = 4294967295U;  // 0 to 4.2B (no negative)
long long ll = 9223372036854775807LL;  // 64-bit int
const int MAX = 100;            // like Python's ALL_CAPS constant (but enforced)
```

### Input / Output

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name;
    int age;

    std::cout << "Enter name: ";   // print (no newline)
    std::cin >> name;              // read word

    std::cout << "Enter age: ";
    std::cin >> age;

    std::cout << "Hello " << name << ", age " << age << std::endl;
    // endl flushes buffer; "\n" is faster (no flush)
    return 0;
}
```

```python
# Python equivalent
name = input("Enter name: ")
age = int(input("Enter age: "))
print(f"Hello {name}, age {age}")
```

### Namespaces

```cpp
// std is the standard namespace — contains cout, cin, string, vector, etc.
using namespace std;    // import everything (not recommended in headers)
std::cout << "Hi";     // explicit — preferred in production code

// Like Python: from math import * (bad) vs import math; math.sqrt() (good)
```

### `const` vs `#define`

```cpp
const double PI = 3.14159;    // type-safe, scoped, preferred
#define MAX_SIZE 100           // preprocessor text substitution — no type, no scope

// Python equivalent: PI = 3.14159 (convention: UPPER_CASE for constants)
```

---

## 4. Pointers & References

> **Interview Q:** *What is the difference between a pointer and a reference in C++?*

This is the most distinctly C++ concept. Python has neither — everything in Python is a reference implicitly. In C++, you explicitly choose.

### Pointers

A pointer is a variable that **stores a memory address**. It can be null, reassigned, and can do pointer arithmetic.

```cpp
int x = 10;
int* p = &x;    // p holds the address of x (& = "address of")
*p = 20;        // dereference: modify the value AT the address p points to
                // x is now 20

std::cout << p;   // prints memory address (e.g., 0x7ffd5c4b3abc)
std::cout << *p;  // prints 20 (dereference)

int* q = nullptr; // null pointer — points to nothing (use nullptr, not NULL)
```

**Pointer operations:**
```cpp
int arr[] = {10, 20, 30};
int* p = arr;        // p points to arr[0]
p++;                 // p now points to arr[1] (pointer arithmetic: advances by sizeof(int))
*(p + 1) = 99;       // arr[2] = 99
```

### References

A reference is an **alias** for an existing variable. Must be initialised at declaration, cannot be null, cannot be reseated (reassigned to point elsewhere).

```cpp
int x = 10;
int& r = x;    // r is an alias for x; they share the same memory
r = 20;        // x is now 20 — modifying r modifies x
```

### Pointer vs Reference Comparison

| | Pointer | Reference |
|---|---|---|
| Null allowed | Yes (`nullptr`) | No — must be initialised |
| Reseat (reassign) | Yes | No — always refers to same object |
| Syntax to use | `*p` to dereference | `r` directly (no special syntax) |
| Memory address | Can do arithmetic | Cannot |
| Use case | Dynamic memory, optional values, arrays | Function parameters, return values, aliases |

### Pass by Value vs Pointer vs Reference

```cpp
void byValue(int x)   { x = 99; }   // copy; original unchanged
void byPointer(int* p){ *p = 99; }  // modifies original via address
void byRef(int& x)    { x = 99; }   // modifies original; cleaner syntax

int a = 10;
byValue(a);    // a still 10
byPointer(&a); // a = 99
byRef(a);      // a = 99 (cleaner; preferred over pointers for aliases)
```

```python
# Python equivalent: everything is passed by reference to the object
def modify_list(lst):
    lst.append(99)  # modifies original

def reassign(lst):
    lst = [1, 2, 3]  # rebinds local name; original unchanged

# Immutable types (int, str, tuple): behave like pass-by-value
# Mutable types (list, dict): mutations are visible to caller
```

> **Interview Q:** *When would you use a pointer vs a reference?*
> Use **references** for function parameters and return values when the object always exists — cleaner syntax, no null risk. Use **pointers** when the object might be absent (nullptr check), when you need to reassign what the variable points to, when doing dynamic memory allocation, or when dealing with arrays/buffers.

---

## 5. Memory Management — Stack, Heap, RAII, Smart Pointers

> **Interview Q:** *Explain stack vs heap memory. What is a memory leak?*

### Stack Memory

Automatically managed. Variables are allocated when a scope is entered and freed when it exits. Fast (just move a stack pointer). Limited size (~1–8 MB).

```cpp
void foo() {
    int x = 10;          // allocated on stack
    double arr[100];     // 100 doubles on stack
}   // x and arr automatically freed when foo() returns
```

### Heap Memory

Manually managed. `new` allocates, `delete` frees. Large size (GBs). Slower than stack (system call). Used for objects whose size is unknown at compile time or lifetime exceeds the function scope.

```cpp
int* p = new int(42);       // allocate single int on heap
int* arr = new int[100];    // allocate array on heap
delete p;                   // free single int
delete[] arr;               // free array (MUST use delete[], not delete)
```

### Memory Leak

Happens when heap-allocated memory is never freed:
```cpp
void leak() {
    int* p = new int[1000000];
    // forgot delete[] p; → 4MB leaked every call
}
// Python: GC handles this automatically — no leaks from normal code
```

### Dangling Pointer

A pointer that still holds an address after the memory has been freed:
```cpp
int* p = new int(10);
delete p;
*p = 20;   // UNDEFINED BEHAVIOUR — dangling pointer dereference
p = nullptr;  // good practice: null the pointer after delete
```

### RAII — Resource Acquisition Is Initialisation

The fundamental C++ idiom for safe resource management. Bind resource lifetime to object lifetime: acquire in the constructor, release in the destructor. When the object goes out of scope, the destructor runs automatically — no leaks possible.

```cpp
class FileHandle {
    FILE* f;
public:
    FileHandle(const char* path) { f = fopen(path, "r"); }  // acquire
    ~FileHandle() { if (f) fclose(f); }                      // release automatically
};

void process() {
    FileHandle fh("data.txt");  // file opened
    // ... use fh ...
}   // fh destroyed here → destructor closes file automatically
    // even if an exception is thrown
```

```python
# Python equivalent: context managers (with statement)
with open("data.txt") as f:
    data = f.read()
# file automatically closed when with block exits — same RAII concept
```

### Smart Pointers (Modern C++ — use always)

Smart pointers are RAII wrappers around raw pointers. They automatically delete the managed object when appropriate. **Never use raw `new/delete` in modern C++.**

#### `unique_ptr` — Exclusive Ownership

Only one `unique_ptr` can own an object at a time. When it goes out of scope, the object is deleted. Non-copyable; movable.

```cpp
#include <memory>

std::unique_ptr<int> p = std::make_unique<int>(42);
std::cout << *p;        // use like a regular pointer
// NO delete needed — freed automatically when p goes out of scope

// Ownership transfer
std::unique_ptr<int> q = std::move(p);  // p is now null; q owns the int
```

#### `shared_ptr` — Shared Ownership

Multiple `shared_ptr`s can own the same object. Uses **reference counting** — object deleted when the last `shared_ptr` owning it is destroyed.

```cpp
std::shared_ptr<int> a = std::make_shared<int>(42);
std::shared_ptr<int> b = a;  // both own the same int; ref count = 2
// object deleted when BOTH a and b go out of scope (ref count → 0)
```

```python
# Python uses reference counting natively — shared_ptr is conceptually the same
a = [1, 2, 3]
b = a         # ref count = 2; list freed when both a and b go out of scope
```

#### `weak_ptr` — Non-owning Observer

Holds a non-owning reference to a `shared_ptr`-managed object. Does not increment the ref count. Used to break **circular references** (which cause memory leaks in shared_ptr).

```cpp
std::shared_ptr<int> sp = std::make_shared<int>(42);
std::weak_ptr<int> wp = sp;     // wp doesn't own; doesn't increment count

if (auto locked = wp.lock()) {  // check if object still alive
    std::cout << *locked;
}
```

### Smart Pointer Quick Reference

| | `unique_ptr` | `shared_ptr` | `weak_ptr` |
|---|---|---|---|
| Ownership | Exclusive (1 owner) | Shared (N owners) | None (observer) |
| Copy | ❌ (move only) | ✓ | ✓ |
| Overhead | Zero | Ref count | Zero (+ lock) |
| Use when | Default choice | Shared ownership needed | Break circular refs |

---

## 6. Object-Oriented Programming (OOP)

> **Interview Q:** *Explain the four pillars of OOP with C++ examples.*

### Classes & Objects

```cpp
class Animal {
private:                        // encapsulation: not accessible outside
    std::string name;
    int age;

public:                         // accessible everywhere
    // Constructor
    Animal(std::string n, int a) : name(n), age(a) {}
    // Initialiser list (: name(n), age(a)) is preferred over assignment in body

    // Destructor
    ~Animal() { std::cout << name << " destroyed\n"; }

    // Member functions
    void speak() { std::cout << name << " speaks\n"; }
    std::string getName() const { return name; }  // const: doesn't modify object
    void setName(std::string n) { name = n; }
};

// Creating objects
Animal a("Dog", 3);         // stack-allocated
a.speak();                   // "Dog speaks"
Animal* p = new Animal("Cat", 2);   // heap-allocated
p->speak();                  // arrow operator for pointer to object
delete p;
```

```python
# Python equivalent
class Animal:
    def __init__(self, name, age):
        self.__name = name   # private by convention
        self.age = age

    def speak(self):
        print(f"{self.__name} speaks")

    @property
    def name(self): return self.__name
```

### The 4 Pillars of OOP

**1. Encapsulation** — Bundle data and methods; restrict direct access.
```cpp
class BankAccount {
    double balance;           // private (inaccessible outside)
public:
    void deposit(double amt) {
        if (amt > 0) balance += amt;  // validation logic inside
    }
    double getBalance() const { return balance; }
};
// External code can't set balance = -1000 directly
```

**2. Abstraction** — Expose only what's needed; hide implementation.
```cpp
class NeuralNetwork {
public:
    void train(Data& d);    // user just calls train() and predict()
    float predict(Input& x);
private:
    void forwardPass();     // implementation details hidden
    void backprop();
    std::vector<Layer> layers;
};
```

**3. Inheritance** — Derive new classes from existing ones (see Section 7).

**4. Polymorphism** — Same interface, different behaviour (see Section 7).

### Constructors & Destructors

```cpp
class Model {
    float* weights;
    int size;
public:
    Model(int n) : size(n) {
        weights = new float[n];      // allocate in constructor
    }

    // Copy constructor — deep copy
    Model(const Model& other) : size(other.size) {
        weights = new float[size];
        std::copy(other.weights, other.weights + size, weights);
    }

    // Copy assignment operator
    Model& operator=(const Model& other) {
        if (this != &other) {        // self-assignment check
            delete[] weights;
            size = other.size;
            weights = new float[size];
            std::copy(other.weights, other.weights + size, weights);
        }
        return *this;
    }

    ~Model() { delete[] weights; }  // free in destructor (RAII)
};
```

**Rule of Three/Five:** If you define any of destructor, copy constructor, copy assignment — define all three. In modern C++, also define move constructor and move assignment (Rule of Five). Or use smart pointers and follow the Rule of Zero (define none).

### Static Members

```cpp
class Counter {
    static int count;   // shared across ALL instances
public:
    Counter() { ++count; }
    ~Counter() { --count; }
    static int getCount() { return count; }  // static method: no 'this' pointer
};
int Counter::count = 0;  // define outside class

Counter a, b, c;
std::cout << Counter::getCount();  // 3
```

```python
# Python equivalent
class Counter:
    count = 0          # class variable (shared)
    def __init__(self): Counter.count += 1
    @classmethod
    def get_count(cls): return cls.count
```

### Operator Overloading

```cpp
class Vector2D {
public:
    float x, y;
    Vector2D(float x, float y) : x(x), y(y) {}

    Vector2D operator+(const Vector2D& other) const {
        return Vector2D(x + other.x, y + other.y);
    }

    bool operator==(const Vector2D& other) const {
        return x == other.x && y == other.y;
    }

    // Output operator (friend function)
    friend std::ostream& operator<<(std::ostream& os, const Vector2D& v) {
        os << "(" << v.x << ", " << v.y << ")";
        return os;
    }
};

Vector2D a(1, 2), b(3, 4);
Vector2D c = a + b;          // calls operator+
std::cout << c;              // (4, 6)
```

```python
# Python: dunder methods
class Vector2D:
    def __add__(self, other): return Vector2D(self.x + other.x, self.y + other.y)
    def __eq__(self, other): return self.x == other.x and self.y == other.y
    def __repr__(self): return f"({self.x}, {self.y})"
```

---

## 7. Inheritance & Polymorphism

> **Interview Q:** *What is the difference between compile-time and runtime polymorphism?*

### Inheritance

```cpp
class Animal {
public:
    std::string name;
    Animal(std::string n) : name(n) {}
    virtual void speak() {           // virtual: enables runtime polymorphism
        std::cout << name << " makes a sound\n";
    }
    virtual ~Animal() {}             // ALWAYS make destructor virtual in base class
};

class Dog : public Animal {
public:
    Dog(std::string n) : Animal(n) {}
    void speak() override {          // override: tells compiler we're overriding
        std::cout << name << " barks\n";
    }
};

class Cat : public Animal {
public:
    Cat(std::string n) : Animal(n) {}
    void speak() override {
        std::cout << name << " meows\n";
    }
};
```

**Access specifiers in inheritance:**
```cpp
class Derived : public Base    // public members stay public
class Derived : protected Base // public → protected
class Derived : private Base   // everything becomes private
```

### Virtual Functions & Runtime Polymorphism

```cpp
Animal* animals[] = { new Dog("Rex"), new Cat("Whiskers"), new Animal("Generic") };

for (Animal* a : animals) {
    a->speak();   // calls the CORRECT speak() based on actual object type
}
// Output:
// Rex barks
// Whiskers meows
// Generic makes a sound
```

**How it works — vtable (virtual table):**
Every class with at least one virtual function has a vtable — a compile-time table of function pointers. Each object carries a hidden vptr (virtual pointer) pointing to its class's vtable. At runtime, `a->speak()` follows the vptr → looks up vtable → calls the right function. This is **dynamic dispatch** — the function is resolved at runtime, not compile time.

```
Dog object: [vptr] → Dog's vtable → [&Dog::speak, &Animal::~Animal, ...]
Cat object: [vptr] → Cat's vtable → [&Cat::speak, &Animal::~Animal, ...]
```

### Abstract Classes & Pure Virtual Functions

```cpp
class Shape {
public:
    virtual double area() = 0;       // pure virtual: = 0 makes this abstract
    virtual double perimeter() = 0;
    virtual ~Shape() {}
};
// Shape cannot be instantiated; it defines an interface (like Python's ABC)

class Circle : public Shape {
    double r;
public:
    Circle(double r) : r(r) {}
    double area() override { return 3.14159 * r * r; }
    double perimeter() override { return 2 * 3.14159 * r; }
};
```

```python
# Python equivalent
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self): pass

class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self): return 3.14159 * self.r ** 2
```

### Compile-Time vs Runtime Polymorphism

| | Compile-Time (Static) | Runtime (Dynamic) |
|---|---|---|
| Mechanism | Function overloading, templates | Virtual functions, vtable |
| Resolution | At compile time | At runtime |
| Speed | No overhead | Tiny vtable lookup overhead |
| Flexibility | Less flexible | More flexible |
| C++ example | `sqrt(int)` vs `sqrt(double)` | `animal->speak()` dispatch |

### Function Overloading (Compile-Time)

```cpp
// Same function name, different parameter types — resolved at compile time
void print(int x)    { std::cout << "int: " << x; }
void print(double x) { std::cout << "double: " << x; }
void print(std::string s) { std::cout << "string: " << s; }

print(42);       // calls print(int)
print(3.14);     // calls print(double)
print("hello");  // calls print(string)
```

### Multiple Inheritance & Diamond Problem

```cpp
class A { public: virtual void hello() { std::cout << "A\n"; } };
class B : public A {};
class C : public A {};
class D : public B, public C {};  // Diamond: D inherits A twice!

// Solution: virtual inheritance
class B : virtual public A {};
class C : virtual public A {};
class D : public B, public C {};  // now D has only one copy of A
```

> **Interview Q:** *Why should the destructor be virtual in a base class?*
> If you delete a derived object through a base class pointer and the destructor is NOT virtual, only the base class destructor runs — the derived class destructor is skipped, causing memory leaks. With a virtual destructor, the correct (most-derived) destructor is called first, then base destructors in order.

---

## 8. Templates & Generic Programming

> **Interview Q:** *What are templates in C++ and what problem do they solve?*

Templates allow writing type-generic code that is instantiated at compile time for each type used. Zero runtime overhead — the compiler generates separate code for each type.

```python
# Python: duck typing achieves generics naturally
def add(a, b): return a + b
add(1, 2)       # works
add(1.5, 2.5)   # works
add("a", "b")   # works
# No template needed; Python checks types at runtime
```

```cpp
// C++: need templates for type-generic functions
template <typename T>
T add(T a, T b) {
    return a + b;
}

add<int>(1, 2);         // compiler generates add_int(int, int)
add<double>(1.5, 2.5);  // compiler generates add_double(double, double)
add(1, 2);              // template argument deduction (compiler infers T=int)
```

### Function Templates

```cpp
template <typename T>
T maxOf(T a, T b) {
    return (a > b) ? a : b;
}

template <typename T>
void swap(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}
```

### Class Templates

```cpp
template <typename T>
class Stack {
    std::vector<T> data;
public:
    void push(const T& val) { data.push_back(val); }
    void pop() { data.pop_back(); }
    T& top() { return data.back(); }
    bool empty() const { return data.empty(); }
    int size() const { return data.size(); }
};

Stack<int> intStack;
Stack<std::string> strStack;
Stack<float> floatStack;
```

### Template Specialisation

```cpp
// General template
template <typename T>
class Printer {
public:
    void print(T val) { std::cout << val; }
};

// Specialisation for bool — different behaviour
template <>
class Printer<bool> {
public:
    void print(bool val) { std::cout << (val ? "true" : "false"); }
};
```

> **Interview Q:** *What is the difference between templates and runtime polymorphism?*
> Templates are resolved at **compile time** — the compiler generates a separate function/class for each type. No virtual dispatch overhead but increases binary size. Runtime polymorphism (virtual functions) is resolved at **runtime** via vtable — one function, multiple behaviours via inheritance. Templates = "compile-time OOP"; virtual = "runtime OOP". For ML: STL containers use templates (zero overhead); plugin-style model architectures use virtual functions.

---

## 9. STL — Standard Template Library

> **Interview Q:** *What is the STL? Name the main containers and when you'd use each.*

The STL is C++'s standard generic library — containers, algorithms, and iterators. The Python equivalent is the combination of built-in types + `collections` + `itertools` + `functools`, but STL is more systematic and performance-predictable.

### Containers Overview

| Container | Python Equivalent | Underlying DS | Access | Insert/Delete |
|---|---|---|---|---|
| `vector<T>` | `list` | Dynamic array | O(1) random | O(1) end / O(n) middle |
| `deque<T>` | `collections.deque` | Double-ended array | O(1) random | O(1) both ends |
| `list<T>` | Linked list (no built-in) | Doubly linked list | O(n) | O(1) anywhere (given iterator) |
| `stack<T>` | N/A (use list) | deque adapter | LIFO | O(1) |
| `queue<T>` | `collections.deque` | deque adapter | FIFO | O(1) |
| `priority_queue<T>` | `heapq` | Binary heap | O(1) top | O(log n) |
| `set<T>` | — | Red-black tree | O(log n) | O(log n) |
| `unordered_set<T>` | `set` | Hash table | O(1) avg | O(1) avg |
| `map<K,V>` | — | Red-black tree | O(log n) | O(log n) |
| `unordered_map<K,V>` | `dict` | Hash table | O(1) avg | O(1) avg |
| `pair<T1,T2>` | `tuple` of 2 | — | O(1) | — |

### vector — Your Default Container

```cpp
#include <vector>

std::vector<int> v = {1, 2, 3, 4, 5};
v.push_back(6);              // append — amortised O(1)
v.pop_back();                // remove last — O(1)
v[2] = 99;                   // random access — O(1)
v.insert(v.begin() + 2, 10); // insert at index 2 — O(n)
v.erase(v.begin() + 2);      // remove at index 2 — O(n)
v.size();                    // number of elements
v.empty();                   // check if empty
v.reserve(100);              // pre-allocate capacity (avoid reallocation)

// Iterate
for (int x : v) std::cout << x << " ";  // range-for (like Python for x in v)
for (int i = 0; i < v.size(); i++) v[i];  // index-based
for (auto it = v.begin(); it != v.end(); ++it) *it;  // iterator-based
```

```python
# Python list equivalent
v = [1, 2, 3, 4, 5]
v.append(6)
v.pop()
v[2] = 99
v.insert(2, 10)
del v[2]
len(v)
not v
```

### map vs unordered_map

```cpp
#include <map>
#include <unordered_map>

// map: sorted by key, O(log n) all ops
std::map<std::string, int> word_count;
word_count["hello"] = 1;
word_count["world"] = 2;
// Iterating gives keys in SORTED order

// unordered_map: hash table, O(1) average, no order
std::unordered_map<std::string, int> ht;
ht["hello"] = 1;
ht.count("hello");   // 0 or 1 (check existence)
ht.find("hello");    // returns iterator; ht.end() if not found
```

```python
# Python: dict is hash map (unordered_map equivalent)
d = {"hello": 1, "world": 2}
d.get("hello", 0)   # 1 (with default)
"hello" in d        # True
```

**Use `map` when you need sorted order. Use `unordered_map` when you don't — it's faster.**

### priority_queue — Max-Heap by Default

```cpp
#include <queue>

std::priority_queue<int> maxHeap;  // max at top
maxHeap.push(3); maxHeap.push(1); maxHeap.push(4);
maxHeap.top();   // 4 (max)
maxHeap.pop();   // removes 4

// Min-heap
std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
```

```python
import heapq
heap = []
heapq.heappush(heap, 3)   # min-heap by default in Python
heapq.heappop(heap)
```

### STL Algorithms

```cpp
#include <algorithm>
#include <numeric>

std::vector<int> v = {5, 3, 1, 4, 2};

// Sorting
std::sort(v.begin(), v.end());                     // ascending
std::sort(v.begin(), v.end(), std::greater<int>()); // descending
std::sort(v.begin(), v.end(), [](int a, int b){ return a > b; }); // custom

// Searching
auto it = std::find(v.begin(), v.end(), 3);        // linear search
auto it2 = std::binary_search(v.begin(), v.end(), 3); // binary search (sorted!)

// Aggregation
int sum = std::accumulate(v.begin(), v.end(), 0);  // sum
int prod = std::accumulate(v.begin(), v.end(), 1, std::multiplies<int>());

// Transform (like Python's map)
std::transform(v.begin(), v.end(), v.begin(), [](int x){ return x * 2; });

// Count
int cnt = std::count(v.begin(), v.end(), 3);
int cnt_if = std::count_if(v.begin(), v.end(), [](int x){ return x > 2; });

// Min/Max
int mx = *std::max_element(v.begin(), v.end());
auto [mn, mx2] = std::minmax_element(v.begin(), v.end()); // C++17 structured binding
```

```python
# Python equivalents
v = [5, 3, 1, 4, 2]
sorted(v)
sorted(v, reverse=True)
sum(v)
list(map(lambda x: x*2, v))
v.count(3)
sum(1 for x in v if x > 2)
max(v)
min(v), max(v)
```

### Iterators

Iterators are the glue between containers and algorithms — pointer-like objects for traversing containers.

```cpp
std::vector<int> v = {1, 2, 3};
auto it = v.begin();   // iterator to first element
++it;                  // advance
*it;                   // dereference (value = 2)
v.end();               // one past last element (sentinel)

// Reverse iterate
for (auto it = v.rbegin(); it != v.rend(); ++it)
    std::cout << *it;  // 3 2 1
```

---

## 10. Move Semantics & Rvalue References (C++11)

> **Interview Q:** *What are move semantics and why are they important for performance?*

### The Problem Without Move Semantics

```cpp
std::vector<int> makeVector() {
    std::vector<int> v(1000000);  // 1M ints allocated
    return v;
}
// Pre-C++11: v is COPIED into the caller's variable → 4MB copied unnecessarily
// C++11+: MOVED (pointer transfer) → O(1), no copy
```

### Lvalue vs Rvalue

- **Lvalue:** Has a name, has a persistent address. `int x = 5;` — `x` is an lvalue.
- **Rvalue:** Temporary, no name, address may not persist. `5`, `x + y`, `makeVector()` — all rvalues.

### Move Constructor & Move Assignment

```cpp
class Matrix {
    float* data;
    int rows, cols;
public:
    // Move constructor: steal the resource from the temporary
    Matrix(Matrix&& other) noexcept
        : data(other.data), rows(other.rows), cols(other.cols) {
        other.data = nullptr;  // leave source in valid but empty state
    }

    // Move assignment operator
    Matrix& operator=(Matrix&& other) noexcept {
        if (this != &other) {
            delete[] data;
            data = other.data;
            other.data = nullptr;
            rows = other.rows; cols = other.cols;
        }
        return *this;
    }
};
```

### `std::move`

Forces an lvalue to be treated as an rvalue, enabling move semantics:

```cpp
std::vector<int> a = {1, 2, 3};
std::vector<int> b = std::move(a);  // a's contents moved to b
// a is now empty (valid but unspecified state)
// No 3-element copy — just pointer swap
```

```python
# Python: no equivalent needed — objects are references
# "Moving" is just rebinding a name
a = [1, 2, 3]
b = a          # b and a BOTH reference the same list (no copy)
a = None       # a no longer references the list; b still does
```

**Why this matters for ML:** When building inference engines or custom operators, avoid copying large weight matrices. `std::move` when passing ownership to/from containers.

---

## 11. Lambdas, auto, range-for (Modern C++)

### Lambdas (C++11)

Inline anonymous functions — like Python's `lambda` but far more powerful.

```cpp
// Basic lambda
auto square = [](int x) { return x * x; };
square(5);  // 25

// Lambda with capture: access outer scope variables
int factor = 3;
auto multiply = [factor](int x) { return x * factor; };  // capture by value
auto multiply_ref = [&factor](int x) { return x * factor; };  // capture by reference

// [=] captures all by value; [&] captures all by reference; [this] captures member

// Lambda with STL algorithms
std::vector<int> v = {3, 1, 4, 1, 5};
std::sort(v.begin(), v.end(), [](int a, int b){ return a > b; });  // descending sort
std::for_each(v.begin(), v.end(), [](int x){ std::cout << x << " "; });
auto count = std::count_if(v.begin(), v.end(), [](int x){ return x > 3; });
```

```python
# Python lambda
square = lambda x: x * x
multiply = lambda x: x * 3   # captured by closure

sorted(v, key=lambda x: -x)
list(filter(lambda x: x > 3, v))
```

### `auto` — Type Inference

```cpp
auto x = 42;                     // int
auto y = 3.14;                   // double
auto s = std::string("hello");   // string
auto v = std::vector<int>{1,2,3};

// Essential for complex iterator types
auto it = myMap.find("key");      // saves typing std::map<string,int>::iterator
for (auto& [key, val] : myMap) {  // structured bindings (C++17)
    std::cout << key << ": " << val;
}
```

### Range-For Loop

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};

for (int x : v)    std::cout << x;  // by value (copy)
for (int& x : v)   x *= 2;          // by reference (modify in place)
for (const int& x : v) std::cout << x;  // by const ref (read, no copy)
```

```python
for x in v: print(x)         # Python range-for equivalent
```

### `nullptr` (C++11)

```cpp
int* p = nullptr;    // type-safe null pointer
// vs old:  int* p = NULL;    // NULL is just 0 — not type-safe
// vs old:  int* p = 0;
```

### `constexpr` — Compile-Time Constants

```cpp
constexpr int MAX_LAYERS = 100;          // evaluated at compile time
constexpr double PI = 3.14159265358979;
constexpr int square(int x) { return x * x; }  // compile-time function
```

---

## 12. Exception Handling

> **Interview Q:** *How does exception handling work in C++? How does it compare to Python?*

```cpp
#include <stdexcept>

double safeDivide(double a, double b) {
    if (b == 0) throw std::invalid_argument("Division by zero");
    return a / b;
}

try {
    double result = safeDivide(10, 0);
} catch (const std::invalid_argument& e) {
    std::cerr << "Error: " << e.what() << "\n";
} catch (const std::exception& e) {   // base class for std exceptions
    std::cerr << "Std exception: " << e.what() << "\n";
} catch (...) {                        // catch everything
    std::cerr << "Unknown exception\n";
}
```

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except Exception as e:
    print(f"Exception: {e}")
```

### Standard Exception Hierarchy

```
std::exception
├── std::logic_error
│   ├── std::invalid_argument
│   ├── std::out_of_range
│   └── std::domain_error
└── std::runtime_error
    ├── std::overflow_error
    └── std::underflow_error
```

### `noexcept`

Declare that a function will not throw exceptions — allows compiler optimisations:

```cpp
void criticalPath() noexcept {  // compiler can optimise; std::terminate if exception thrown
    // ...
}
```

**Key difference from Python:** In C++, exceptions carry significant overhead because the compiler generates cleanup code (stack unwinding) for every scope. For performance-critical code, use error codes or `std::expected` (C++23) instead of exceptions. Python exceptions are cheaper and more commonly used for control flow.

---

## 13. Multithreading & Concurrency

> **Interview Q:** *How does C++ multithreading compare to Python's threading?*

### C++ Threads (No GIL — True Parallelism)

```cpp
#include <thread>
#include <mutex>

std::mutex mtx;
int counter = 0;

void increment(int n) {
    for (int i = 0; i < n; i++) {
        std::lock_guard<std::mutex> lock(mtx);  // RAII lock: auto-unlocks when out of scope
        ++counter;
    }
}

int main() {
    std::thread t1(increment, 10000);
    std::thread t2(increment, 10000);
    t1.join();  // wait for t1 to finish
    t2.join();  // wait for t2 to finish
    std::cout << counter;  // 20000
}
```

### C++ vs Python Concurrency

| | C++ threads | Python threads | Python multiprocessing |
|---|---|---|---|
| True parallelism | ✓ Yes | ✗ GIL limits | ✓ Yes (separate processes) |
| Shared memory | Easy | Easy | Requires Pipes/Queue |
| Overhead | Low | Low | High (fork/spawn) |
| Use case | Parallel compute, I/O | I/O-bound tasks | CPU-bound Python |

```python
# Python threading (I/O bound only due to GIL)
import threading
t = threading.Thread(target=my_func, args=(10,))
t.start(); t.join()

# True CPU parallelism in Python
from multiprocessing import Pool
with Pool(4) as p:
    results = p.map(process_chunk, data_chunks)
```

### `std::async` — High-Level Concurrency

```cpp
#include <future>

auto future = std::async(std::launch::async, [](){
    return heavy_computation();   // runs in a new thread
});

int result = future.get();  // blocks until computation is done
```

---

## 14. Compilation, Linking & Build Systems

> **Interview Q:** *Explain the C++ compilation process. What is the difference between a compiler and a linker?*

### C++ Build Pipeline

```
Source files (.cpp, .h)
       ↓
[Preprocessor]
  - Expand #include, #define, #ifdef
  → Expanded source
       ↓
[Compiler]
  - Parse, type-check, optimise
  → Object files (.o / .obj) — machine code with unresolved symbols
       ↓
[Linker]
  - Resolve external symbols (function calls across files)
  - Link object files + libraries
  → Executable or shared library (.so / .dll)
```

```python
# Python has NO explicit compilation step
# .py → bytecode (.pyc) at runtime via interpreter
# No linking phase — imports resolved at runtime
```

### Header Files vs Source Files

```cpp
// animal.h — DECLARATION (what exists)
#pragma once           // header guard: prevents double inclusion
class Animal {
public:
    Animal(std::string name);
    virtual void speak();
private:
    std::string name_;
};

// animal.cpp — DEFINITION (how it works)
#include "animal.h"
Animal::Animal(std::string name) : name_(name) {}
void Animal::speak() { std::cout << name_ << " speaks\n"; }
```

### Common Compilation Commands

```bash
# Compile single file
g++ -std=c++17 -O2 -o program main.cpp

# Compile multiple files
g++ -std=c++17 -O2 -o program main.cpp animal.cpp utils.cpp

# With debug info
g++ -std=c++17 -g -o program main.cpp

# Optimisation flags: -O0 (none), -O1, -O2 (standard), -O3 (aggressive), -Os (size)
```

### CMake (Industry Standard Build System)

```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.16)
project(MyMLProject)
set(CMAKE_CXX_STANDARD 17)

add_executable(train src/train.cpp src/model.cpp src/data.cpp)
target_include_directories(train PRIVATE include/)
target_link_libraries(train PRIVATE OpenCV::Core torch)
```

```bash
mkdir build && cd build
cmake ..
make -j4          # parallel build with 4 cores
./train
```

---

## 15. C++ in ML/AI — Where It Actually Appears

### PyTorch C++ Extension (pybind11)

Write a CUDA/C++ operator, expose to Python:

```cpp
// my_op.cpp
#include <torch/extension.h>

torch::Tensor my_relu(torch::Tensor input) {
    return torch::clamp_min(input, 0);
}

PYBIND11_MODULE(TORCH_EXTENSION_NAME, m) {
    m.def("my_relu", &my_relu, "A custom ReLU");
}
```

```python
# In Python after building
import my_extension
out = my_extension.my_relu(tensor)
```

### LibTorch — PyTorch C++ API

```cpp
#include <torch/torch.h>

// Load a TorchScript model
torch::jit::script::Module model = torch::jit::load("model.pt");

// Run inference
std::vector<torch::jit::IValue> inputs;
inputs.push_back(torch::ones({1, 3, 224, 224}));
auto output = model.forward(inputs).toTensor();
```

### ONNX Runtime (C++ API)

Used in production inference — mobile, edge, server:

```cpp
Ort::Env env(ORT_LOGGING_LEVEL_WARNING, "inference");
Ort::Session session(env, "model.onnx", Ort::SessionOptions{});
auto output_tensors = session.Run(run_options, input_names, input_tensors, output_names);
```

### llama.cpp Pattern (Relevant to Your Work)

llama.cpp loads quantised GGUF model files in C++, does token-by-token generation entirely in C++. Key patterns used:
- Raw memory-mapped file loading
- SIMD intrinsics for vectorised matrix multiplication
- Custom memory arenas (not using `new`/`delete` per token)
- Thread pool for layer-parallel computation

---

## 16. Interview Q&A — Fundamentals

**Q: What is the difference between C and C++?**
> C is a procedural language — no classes, no OOP, no templates. C++ extends C with OOP (classes, inheritance, polymorphism), templates, exception handling, STL, references, and modern features (smart pointers, lambdas, constexpr). C++ is backward-compatible with C — valid C code is mostly valid C++. Key added safety: smart pointers (vs raw malloc/free), references (safer than raw pointers), RAII (automatic resource management).

**Q: What is `undefined behaviour` in C++?**
> Undefined behaviour (UB) is code that the C++ standard makes no guarantee about — the compiler can do anything: crash, return garbage, silently corrupt data, or "work fine" on one platform but break on another. Common causes: dereferencing a null/dangling pointer, array out-of-bounds access, signed integer overflow, use after free, reading an uninitialised variable. Python has no UB — it raises exceptions. UB in C++ is dangerous precisely because it often doesn't immediately crash.

**Q: What is the difference between `struct` and `class` in C++?**
> Identical in capability — both support methods, inheritance, constructors. The only difference is default access: `struct` members are `public` by default; `class` members are `private`. Convention: use `struct` for plain data aggregates (no invariants, no complex logic); use `class` for entities with behaviour and enforced invariants.

**Q: What is `inline` in C++?**
> Suggests to the compiler to insert the function's code directly at the call site rather than making a function call (saving call overhead). Modern compilers mostly ignore the `inline` hint and make their own inlining decisions based on profiling. More importantly, `inline` functions defined in headers allow multiple translation units to include them without violating the One Definition Rule (ODR).

**Q: What is the difference between `#include <header>` and `#include "header"`?**
> Angle brackets (`<>`) search the system/standard include paths first (for standard library headers like `<vector>`, `<iostream>`). Quotes (`""`) search the current directory first, then fall back to system paths. Use `<>` for system and third-party headers, `""` for your own project headers.

**Q: What does `const` do in different contexts?**
```cpp
const int x = 5;          // x cannot be modified
int* const p = &x;        // pointer p cannot be reassigned (const pointer)
const int* p = &x;        // value pointed to cannot be modified (pointer to const)
const int* const p = &x;  // both pointer and value are const
void foo() const;         // member function: doesn't modify the object (const method)
```

---

## 17. Interview Q&A — OOP & Polymorphism

**Q: What is the difference between overloading and overriding?**
> **Overloading** (compile-time): same function name, different parameter types/count in the same class. Resolved at compile time. **Overriding** (runtime): derived class provides a new implementation of a virtual function declared in the base class. Resolved at runtime via vtable. Python doesn't have function overloading (only the last definition wins); Python's method override works like C++ but without needing `virtual`.

**Q: What is a virtual destructor and why is it needed?**
> When you delete a derived object through a base class pointer and the base destructor is NOT virtual, only the base destructor runs — the derived destructor is skipped, causing a memory leak. Making the base destructor `virtual` ensures the correct (most-derived) destructor is called, then the chain of base destructors. Rule: if a class has any virtual function, make the destructor virtual too.

**Q: What is the difference between composition and inheritance?**
> **Inheritance** ("is-a"): Dog is an Animal. Creates tight coupling — changes to base affect all derived classes. Use when the derived class truly specialises the base. **Composition** ("has-a"): Car has an Engine. Prefer composition for flexibility — you can swap the Engine implementation without changing Car. The general advice: **prefer composition over inheritance** (also the Gang of Four principle). Python follows this too.

**Q: What is an abstract class?**
> A class with at least one pure virtual function (`= 0`). Cannot be instantiated — only derived classes that implement all pure virtuals can be instantiated. Defines an interface/contract. Python equivalent: `ABC` with `@abstractmethod`. Use abstract classes to define interfaces that multiple implementations must follow (e.g., a `Model` abstract class with `train()` and `predict()` that `LinearModel`, `NeuralNet`, `RandomForest` all implement).

**Q: Explain the vtable mechanism.**
> Every class with virtual functions has a vtable — a static table of function pointers, one per virtual function. Each object instance contains a hidden vptr (pointer to its class's vtable). At runtime, `ptr->virtualFunc()` follows the vptr to the vtable and calls the function pointer stored there. This adds one pointer indirection per virtual call (~1–2 ns) — negligible for most code but may matter in tight loops on critical paths.

**Q: What is the difference between `public`, `protected`, and `private` inheritance?**
> `public` inheritance: public/protected members stay public/protected in derived class — the most common, models "is-a". `protected` inheritance: public members become protected in derived class — rarely used. `private` inheritance: all inherited members become private — models "implemented in terms of" without exposing the base class interface. When in doubt, use `public`.

---

## 18. Interview Q&A — Memory & Pointers

**Q: What is a memory leak and how do you prevent it?**
> A memory leak is heap memory that is allocated but never freed, causing the program to gradually consume more RAM until it crashes or the OS kills it. Prevention in modern C++: use RAII — always use smart pointers (`unique_ptr`, `shared_ptr`) instead of raw `new/delete`. If using raw pointers, ensure every code path (including exception paths) calls `delete`. Tools: Valgrind, AddressSanitizer (`-fsanitize=address`) to detect leaks.

**Q: What is a dangling pointer?**
> A pointer that references memory that has been freed or gone out of scope. Dereferencing it is undefined behaviour — may crash, may return garbage, may silently corrupt data. Prevention: set pointer to `nullptr` after `delete`; use smart pointers (they null themselves); prefer references over pointers; use RAII so lifetime is automatic.

**Q: What is the difference between `delete` and `delete[]`?**
> `delete` frees a single object allocated with `new`. `delete[]` frees an array allocated with `new[]`. Mixing them is undefined behaviour. In modern C++, use `std::vector` or `std::unique_ptr<T[]>` instead of raw arrays — they handle this automatically.

**Q: What are smart pointers and when do you use each?**
> `unique_ptr`: exclusive ownership — one owner, freed when it goes out of scope. Zero overhead. Default choice for heap allocation. `shared_ptr`: shared ownership via reference counting — freed when last owner is gone. Small overhead (atomic reference count). Use when multiple objects need to co-own a resource. `weak_ptr`: non-owning observer — doesn't prevent deletion. Use to break circular `shared_ptr` chains that would cause memory leaks. Python's GC handles these scenarios automatically.

**Q: What is the stack vs heap?**
> Stack: automatic storage, fast (just move the stack pointer), small (~1–8 MB), LIFO order, variables freed when scope exits. Heap: dynamic storage, slower (system call), large (GBs), manual lifetime management. Use stack for small, short-lived, fixed-size data (local variables, function frames). Use heap for large objects, variable-size data, or objects that must outlive their creating scope. Python puts almost everything on the heap, managed by the GC.

**Q: What is RAII?**
> Resource Acquisition Is Initialisation — tie resource lifetime to object lifetime. Acquire in the constructor; release in the destructor. When the object goes out of scope (normally or via exception), the destructor runs automatically. This makes resource leaks impossible as long as you follow the pattern. Examples: `std::fstream` (closes file in destructor), `std::unique_ptr` (deletes in destructor), `std::lock_guard` (releases mutex in destructor). Python's `with` statement is the same concept.

---

## 19. Interview Q&A — STL & Templates

**Q: What is the difference between `vector` and `array`?**
> `std::array<T,N>` is a fixed-size, stack-allocated array — size must be known at compile time, no overhead, no reallocation. `std::vector<T>` is dynamically sized, heap-allocated, grows automatically. Use `std::array` when size is fixed and known (e.g., 4×4 matrix for 3D graphics); use `vector` for everything else.

**Q: What is the difference between `map` and `unordered_map`?**
> `std::map` uses a Red-Black Tree — keys are always sorted, O(log n) for insert/lookup/delete. `std::unordered_map` uses a hash table — O(1) average for insert/lookup/delete, no ordering. Use `map` when you need sorted iteration or range queries on keys. Use `unordered_map` when you need fast lookups and don't care about order — it's the Python `dict` equivalent.

**Q: When would you use a `set` vs `unordered_set`?**
> Same trade-off as map vs unordered_map. `set` (tree): O(log n), sorted order, can do range operations (`lower_bound`, `upper_bound`). `unordered_set` (hash): O(1) avg, no order. Use `unordered_set` for existence checks (like Python `set`). Use `set` when you need sorted order or range queries.

**Q: What are iterators and why are they useful?**
> Iterators provide a uniform interface for traversing any container — algorithms like `std::sort`, `std::find`, `std::accumulate` work on any container via iterators without knowing the container type. This separation of concerns is the core STL design. Iterator categories: input, output, forward, bidirectional (list), random access (vector). Python's iterator protocol (`__iter__`, `__next__`) is the same concept.

**Q: What is a function template and when would you use it vs runtime polymorphism?**
> A function template generates type-specific code at compile time — zero runtime overhead, no vtable. Use templates when: (1) behaviour is identical for all types, just the type varies (generic sort, max, min); (2) maximum performance is needed; (3) type is known at compile time. Use virtual functions when: (1) the type is determined at runtime (user input, config-driven model type); (2) you want to swap implementations without recompilation (plugin pattern).

---

## 20. Interview Q&A — Modern C++ & Advanced

**Q: What is move semantics and why does it matter?**
> Pre-C++11, returning large objects from functions involved expensive copies. Move semantics (C++11) allows "stealing" resources from a temporary (rvalue) instead of copying: swap the internal pointers, set the source to null. `std::move` casts an lvalue to an rvalue reference to enable moves. Key benefit: returning `std::vector` from a function is O(1) pointer transfer, not O(n) copy. Critical for ML: moving large weight tensors between functions without copying.

**Q: What is the difference between `const T&`, `T&`, and `T&&`?**
> `T&` (lvalue reference): non-const reference — can modify the object; won't bind to temporaries. `const T&` (const lvalue reference): read-only reference — binds to both lvalues and rvalues; no copy for temporaries (extends their lifetime). `T&&` (rvalue reference): binds only to rvalues/temporaries — enables move semantics. Function parameter guidelines: pass by value for small types; `const T&` for large read-only; `T&` to modify caller's object; `T&&` for move-enabled setters.

**Q: What is `std::move` vs `std::forward`?**
> `std::move` unconditionally casts to rvalue reference — use when you want to transfer ownership of a named object. `std::forward<T>` is used in templates to forward a parameter maintaining its value category (perfect forwarding) — used in generic code where you don't know if the argument is lvalue or rvalue. `std::move` is for moving; `std::forward` is for forwarding in template contexts.

**Q: What are lambdas and what is a capture list?**
> Lambdas are inline anonymous functions. The capture list `[...]` specifies which outer-scope variables the lambda can access: `[=]` capture all by value, `[&]` capture all by reference, `[x]` capture `x` by value, `[&x]` capture `x` by reference, `[this]` capture the current object. Capture by value takes a snapshot; capture by reference uses the live variable (careful with dangling references if lambda outlives the scope).

**Q: What new features did C++11/14/17/20 add?**
> C++11: auto, nullptr, range-for, lambdas, move semantics, rvalue references, smart pointers, `std::thread`, `constexpr`, initialiser lists, variadic templates. C++14: generic lambdas, `std::make_unique`. C++17: structured bindings (`auto [k,v] = pair`), `if constexpr`, `std::optional`, `std::variant`, parallel STL algorithms. C++20: concepts (constrain templates), coroutines, ranges, `std::format` (like Python f-strings), modules.

**Q: What is `std::optional` and when would you use it?**
> `std::optional<T>` represents a value that may or may not be present — a type-safe alternative to returning null pointers or sentinel values (`-1`, `""`, `nullptr`).
```cpp
std::optional<int> findUser(std::string name) {
    if (found) return userId;
    return std::nullopt;    // not found
}
auto result = findUser("Alice");
if (result.has_value()) use(*result);
```
Python equivalent: returning `None` when a value is absent, or using `Optional[int]` in type hints.

---

## 21. Quick Reference Cheat Sheet

### C++ vs Python at a Glance
```
Compiled vs Interpreted:   C++ compiled → machine code | Python interpreted → bytecode
Type System:               C++ static (compile-time)   | Python dynamic (runtime)
Memory:                    C++ manual/smart pointers    | Python GC (ref counting)
Speed:                     C++ 10-100× faster raw       | Python fast via C extensions
Threads:                   C++ true parallelism         | Python GIL → use multiprocessing
Templates:                 C++ compile-time generics    | Python duck typing
Null:                      C++ nullptr                  | Python None
```

### Memory Quick Reference
```
Stack:  auto lifetime, fast, small (~8MB), local vars
Heap:   manual lifetime, slower, large, new/delete or smart ptrs

unique_ptr → exclusive ownership, zero overhead, move-only
shared_ptr → shared ownership, ref-counted, copyable
weak_ptr   → non-owning, no ref count, use to break cycles

RAII: acquire in constructor, release in destructor
      → automatic cleanup, exception-safe
```

### OOP Quick Reference
```
class: private by default | struct: public by default
virtual func  → runtime polymorphism via vtable
pure virtual  → abstract class (= 0), cannot instantiate
override      → tell compiler you're overriding (catches typos)
virtual ~Dtor → always in base class with virtual methods
final         → prevent further overriding/inheritance
```

### STL Container Selection
```
Need random access + dynamic size    → vector (default)
Need fast front/back insert          → deque
Need fast insert anywhere            → list
Need sorted unique keys              → set
Need fast key lookup (unordered)     → unordered_map / unordered_set
Need sorted key-value pairs          → map
Need max/min in O(1)                 → priority_queue
FIFO                                 → queue
LIFO                                 → stack
```

### Smart Pointer Rules
```
Default heap allocation              → unique_ptr
Multiple owners needed               → shared_ptr
Non-owning reference to shared_ptr   → weak_ptr
Never use raw new/delete in modern C++
```

### Modern C++ Essentials
```cpp
auto x = expr;                   // type inference
for (auto& x : container) {}    // range-for
auto fn = [capture](args){};    // lambda
auto [a, b] = pair;              // structured binding (C++17)
T&& rval                         // rvalue reference
std::move(x)                     // cast to rvalue (enable move)
std::make_unique<T>(args)        // preferred over new
nullptr                          // type-safe null (not NULL or 0)
constexpr                        // compile-time constant/function
```

### Compilation Command Reference
```bash
g++ -std=c++17 -O2 -Wall -o prog main.cpp   # standard build
g++ -std=c++17 -g  -o prog main.cpp          # debug build
g++ -fsanitize=address -o prog main.cpp      # detect memory bugs
```

### Common Pitfalls to Avoid
```
❌ Raw new/delete → use smart pointers
❌ Virtual functions without virtual destructor → memory leaks
❌ NOT IN with NULLs (SQL mindset) → use unique_ptr check
❌ Dangling references (return ref to local var) → UB
❌ Forgetting delete[] for arrays → use vector or unique_ptr<T[]>
❌ Self-assignment in operator= without check
❌ Capturing this by [=] in lambda that outlives the object
❌ signed integer overflow → UB (use unsigned or check bounds)
```

### C++ for ML — Key Libraries
```
PyTorch C++:   #include <torch/torch.h>   — LibTorch for inference/training
ONNX Runtime:  ort::Session               — production inference
OpenCV:        #include <opencv2/...>     — computer vision
Eigen:         #include <Eigen/Dense>     — linear algebra (like NumPy)
pybind11:      PYBIND11_MODULE(...)       — expose C++ to Python
llama.cpp:     ggml backend               — LLM inference on CPU/GPU
TensorRT:      nvinfer1::IBuilder         — NVIDIA optimised inference
```

---

*Sources: GeeksforGeeks C++ Interview Questions 2025, InterviewBit C++, InterviewKickstart Advanced C++, PWSkills STL/Memory, HiPeople Advanced C++, Shadecoder Coding Interview Templates, Toptal C++ Interview Questions, Devinterview.io — question banks from 2024–2026 technical interviews at FAANG, Qualcomm, NVIDIA, robotics and ML infrastructure roles.*

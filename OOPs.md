# Object-Oriented Programming (OOP) — Complete Notes
### Python & C++ Side-by-Side | Interview-Ready | Placement-Focused

> **How to use:** Every section opens with the interview question it answers. Python and C++ code appears side-by-side so you build one unified mental model, not two separate ones. Read once → answer any OOP question in any placement interview.

---

## Table of Contents

**PART 1 — FOUNDATIONS**
1. [What is OOP? Why does it exist?](#1-what-is-oop-why-does-it-exist)
2. [Classes & Objects](#2-classes--objects)
3. [Constructors & Destructors](#3-constructors--destructors)
4. [Access Modifiers & Encapsulation](#4-access-modifiers--encapsulation)
5. [The `this` / `self` Pointer](#5-the-this--self-pointer)
6. [Static Members](#6-static-members)

**PART 2 — THE FOUR PILLARS**
7. [Encapsulation](#7-encapsulation)
8. [Abstraction](#8-abstraction)
9. [Inheritance](#9-inheritance)
10. [Polymorphism](#10-polymorphism)

**PART 3 — ADVANCED OOP**
11. [Abstract Classes & Interfaces](#11-abstract-classes--interfaces)
12. [Operator Overloading & Magic Methods](#12-operator-overloading--magic-methods)
13. [Multiple Inheritance & Diamond Problem](#13-multiple-inheritance--diamond-problem)
14. [Composition vs Inheritance](#14-composition-vs-inheritance)
15. [Special Python OOP Features](#15-special-python-oop-features)
16. [Special C++ OOP Features](#16-special-cpp-oop-features)

**PART 4 — DESIGN PRINCIPLES**
17. [SOLID Principles](#17-solid-principles)
18. [Design Patterns](#18-design-patterns)
19. [Relationships: Association, Aggregation, Composition](#19-relationships-association-aggregation-composition)

**PART 5 — INTERVIEW Q&A**
20. [Interview Q&A — Fundamentals](#20-interview-qa--fundamentals)
21. [Interview Q&A — Pillars Deep Dive](#21-interview-qa--pillars-deep-dive)
22. [Interview Q&A — Advanced & Tricky](#22-interview-qa--advanced--tricky)
23. [Interview Q&A — Python vs C++ Specific](#23-interview-qa--python-vs-c-specific)
24. [Scenario-Based Questions](#24-scenario-based-questions)

---

# PART 1 — FOUNDATIONS

---

## 1. What is OOP? Why does it exist?

> **Interview Q:** *What is Object-Oriented Programming? How is it different from procedural programming?*

**Object-Oriented Programming (OOP)** is a programming paradigm that organises code around **objects** — bundles of data (attributes) and behaviour (methods) — rather than around functions and logic alone.

### OOP vs Procedural

| Dimension | Procedural | Object-Oriented |
|---|---|---|
| Core unit | Function | Object (data + methods) |
| Data | Shared globally or passed around | Encapsulated inside objects |
| Real-world mapping | Poor — actions and data are separate | Natural — mirrors real entities |
| Code reuse | Copy-paste or libraries | Inheritance and composition |
| Maintenance | Hard to scale | Easier — changes localised to classes |
| Example | C, early FORTRAN | Python, C++, Java |

**Why OOP was invented:** As software grew larger in the 1970s–1980s, procedural programs became impossible to maintain. Data was global, functions modified state from anywhere, bugs were hard to isolate. OOP encapsulates data inside objects so only the object's own methods can change it — reducing bugs and improving maintainability.

### Real-world analogy

A car is an object. It has:
- **Attributes:** colour, speed, fuel level
- **Methods:** accelerate(), brake(), refuel()

You don't need to know how the engine works to drive (abstraction). Nobody can directly set fuel = -100 from outside (encapsulation). A Tesla is a Car with extra features (inheritance). Both a Car and a Truck respond to accelerate() differently (polymorphism).

---

## 2. Classes & Objects

> **Interview Q:** *What is the difference between a class and an object?*

| | Class | Object |
|---|---|---|
| What | Blueprint / template / type | Instance created from a class |
| Memory | No memory allocated | Memory allocated |
| Analogy | Cookie cutter | Actual cookie |
| Example | `class Dog` | `rex = Dog("Rex")` |

**Python:**
```python
class Dog:
    # Class variable — shared by ALL instances
    species = "Canis lupus familiaris"

    def __init__(self, name, age):
        # Instance variables — unique to each object
        self.name = name
        self.age = age

    def bark(self):
        return f"{self.name} says woof!"

# Creating objects (instances)
rex = Dog("Rex", 3)
buddy = Dog("Buddy", 5)

print(rex.name)        # "Rex"    — instance variable
print(Dog.species)     # "Canis…" — class variable (access via class)
print(rex.bark())      # "Rex says woof!"

# Every object has its own __dict__ of instance variables
print(rex.__dict__)    # {'name': 'Rex', 'age': 3}
```

**C++:**
```cpp
class Dog {
public:
    // Class variable (static)
    static std::string species;

    // Instance variables
    std::string name;
    int age;

    // Constructor
    Dog(std::string n, int a) : name(n), age(a) {}

    std::string bark() {
        return name + " says woof!";
    }
};

// Define static member outside class
std::string Dog::species = "Canis lupus familiaris";

// Creating objects
Dog rex("Rex", 3);         // stack-allocated
Dog* buddy = new Dog("Buddy", 5);  // heap-allocated — must delete
std::cout << rex.bark();   // "Rex says woof!"
std::cout << rex.name;     // "Rex"
delete buddy;
```

### Key difference — memory allocation

```
Python:  ALL objects live on the heap. Variables are references.
         rex = Dog("Rex") → rex is a reference to a heap object.

C++:     You CHOOSE: stack (Dog rex) or heap (new Dog).
         Stack objects are auto-destroyed when scope ends.
         Heap objects need manual delete (or smart pointers).
```

---

## 3. Constructors & Destructors

> **Interview Q:** *What is a constructor? How many types are there? What is a destructor?*

A **constructor** is a special method called automatically when an object is created. It initialises the object's state.
A **destructor** is called automatically when an object is destroyed — used for cleanup.

### Types of Constructors

| Type | Description | Python | C++ |
|---|---|---|---|
| Default | No arguments | `__init__(self)` | `Dog()` |
| Parameterised | With arguments | `__init__(self, name)` | `Dog(string n)` |
| Copy | Creates copy of existing object | manual / `copy.copy()` | `Dog(const Dog&)` |
| Move | Transfers ownership (C++11) | N/A (GC handles it) | `Dog(Dog&&)` |

**Python:**
```python
import copy

class Model:
    def __init__(self, name, weights=None):    # Default + Parameterised
        self.name = name
        self.weights = weights or []

    # Python has no built-in copy constructor but you can simulate it
    @classmethod
    def copy_from(cls, other):
        new = cls(other.name)
        new.weights = copy.deepcopy(other.weights)   # deep copy
        return new

    def __del__(self):                          # Destructor (called by GC)
        print(f"{self.name} model cleaned up")
        # Warning: __del__ timing is non-deterministic in Python
        # Prefer context managers (__enter__/__exit__) for resource cleanup

m1 = Model("ResNet", [0.1, 0.2, 0.3])
m2 = Model.copy_from(m1)       # deep copy
m2.weights.append(0.4)
print(m1.weights)              # [0.1, 0.2, 0.3] — unaffected
```

**C++:**
```cpp
class Model {
    std::string name;
    float* weights;
    int size;

public:
    // Default constructor
    Model() : name("unnamed"), weights(nullptr), size(0) {}

    // Parameterised constructor (initialiser list preferred over body assignment)
    Model(std::string n, int s) : name(n), size(s) {
        weights = new float[s]();  // allocate + zero-init
    }

    // Copy constructor (deep copy)
    Model(const Model& other) : name(other.name), size(other.size) {
        weights = new float[size];
        std::copy(other.weights, other.weights + size, weights);
    }

    // Copy assignment operator
    Model& operator=(const Model& other) {
        if (this == &other) return *this;    // self-assignment guard
        delete[] weights;
        name = other.name;
        size = other.size;
        weights = new float[size];
        std::copy(other.weights, other.weights + size, weights);
        return *this;
    }

    // Move constructor (C++11) — transfers, doesn't copy
    Model(Model&& other) noexcept
        : name(std::move(other.name)), weights(other.weights), size(other.size) {
        other.weights = nullptr;   // prevent double-delete
        other.size = 0;
    }

    // Destructor
    ~Model() {
        delete[] weights;          // RAII: free heap memory
        std::cout << name << " destroyed\n";
    }
};
```

### Constructor Initialiser List (C++ only)

```cpp
// Preferred: initialiser list (before constructor body)
Dog(string n, int a) : name(n), age(a) {}

// vs body assignment (worse: default-constructs then assigns)
Dog(string n, int a) { name = n; age = a; }  // extra default-construct step

// REQUIRED for:
// - const members    (const int id;)
// - reference members (int& ref;)
// - base class constructors
// - members with no default constructor
```

### Python `__init__` vs `__new__`

```python
class Singleton:
    _instance = None

    def __new__(cls):
        # __new__ CREATES the object (allocates memory)
        # called before __init__
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

    def __init__(self):
        # __init__ INITIALISES the object (sets attributes)
        self.value = 42

a = Singleton()
b = Singleton()
print(a is b)   # True — same object, Singleton pattern!
```

> **Rule of Three/Five/Zero (C++):**
> If you define **any** of: destructor, copy constructor, copy assignment → define **all three** (Rule of Three).
> With move semantics: add move constructor + move assignment → **Rule of Five**.
> **Rule of Zero:** Use smart pointers (`unique_ptr`, `shared_ptr`) — define none, compiler handles everything correctly.

---

## 4. Access Modifiers & Encapsulation

> **Interview Q:** *What are access modifiers? How does Python differ from C++ in access control?*

Access modifiers control **who** can access a class member.

| Modifier | C++ | Python | Accessible from |
|---|---|---|---|
| **Public** | `public:` | `self.name` (no underscore) | Anywhere |
| **Protected** | `protected:` | `self._name` (single underscore) | Class + subclasses (convention only in Python) |
| **Private** | `private:` | `self.__name` (double underscore) | Class only (name-mangled in Python) |

**C++ — True enforcement:**
```cpp
class BankAccount {
private:
    double balance;         // TRULY inaccessible from outside
    std::string pin;

protected:
    int accountId;         // accessible in this class + derived classes

public:
    BankAccount(double b) : balance(b) {}

    void deposit(double amt) {
        if (amt > 0) balance += amt;   // validation inside
    }

    double getBalance() const { return balance; }  // controlled access
};

BankAccount acc(1000.0);
acc.balance = -9999;   // COMPILE ERROR — private member
acc.deposit(500);      // OK — through public interface
```

**Python — Convention-based (not enforced by compiler):**
```python
class BankAccount:
    def __init__(self, balance):
        self.owner = "public"          # anyone can access
        self._balance = balance        # "please don't touch" — protected by convention
        self.__pin = "1234"            # name-mangled to _BankAccount__pin

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount

    @property
    def balance(self):
        return self._balance

acc = BankAccount(1000)
print(acc._balance)         # Works — Python doesn't block it, just convention
print(acc.__pin)            # AttributeError — name mangled!
print(acc._BankAccount__pin)  # "1234" — but THIS works (mangling, not true private)
```

> **Key interview insight:** Python has NO true access control. Single underscore `_` is a convention meaning "internal use". Double underscore `__` triggers name-mangling (renames to `_ClassName__attr`) making accidental access unlikely — but determined access is always possible. C++ enforces access at compile time.

### Python `@property` — Controlled Access

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius

    @property
    def celsius(self):           # getter
        return self._celsius

    @celsius.setter
    def celsius(self, value):    # setter with validation
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value

    @celsius.deleter
    def celsius(self):           # deleter
        del self._celsius

    @property
    def fahrenheit(self):        # computed property (no setter)
        return self._celsius * 9/5 + 32

t = Temperature(25)
print(t.celsius)      # 25   — calls getter
t.celsius = 30        # calls setter (with validation)
t.celsius = -300      # raises ValueError!
print(t.fahrenheit)   # 86.0 — computed on the fly
```

**C++ equivalent — getter/setter:**
```cpp
class Temperature {
    double celsius;
public:
    double getCelsius() const { return celsius; }
    void setCelsius(double val) {
        if (val < -273.15) throw std::invalid_argument("Below absolute zero");
        celsius = val;
    }
    double getFahrenheit() const { return celsius * 9.0/5 + 32; }
};
```

---

## 5. The `this` / `self` Pointer

> **Interview Q:** *What is `self` in Python and `this` in C++? Are they the same?*

Both refer to the **current instance** of the class — but they work differently.

| | Python `self` | C++ `this` |
|---|---|---|
| Type | Regular first parameter | Hidden pointer parameter (`ClassName*`) |
| Explicit? | Yes — must explicitly write `self` | No — automatically available |
| Naming | Convention: `self` (could be anything) | Fixed keyword |
| Type | The object itself | Pointer to the object |

**Python:**
```python
class Vector:
    def __init__(self, x, y):
        self.x = x        # self refers to THIS specific instance
        self.y = y

    def scale(self, factor):
        self.x *= factor
        self.y *= factor
        return self       # returning self enables method chaining!

v = Vector(1, 2)
v.scale(2).scale(3)      # method chaining: v.x=6, v.y=12
```

**C++:**
```cpp
class Vector {
public:
    float x, y;
    Vector(float x, float y) : x(x), y(y) {}

    Vector& scale(float factor) {
        this->x *= factor;   // this-> accesses members via pointer
        this->y *= factor;
        return *this;        // return reference to self (method chaining)
    }

    // Self-assignment check using this
    Vector& operator=(const Vector& other) {
        if (this == &other) return *this;  // detect self-assignment
        x = other.x;
        y = other.y;
        return *this;
    }
};

Vector v(1, 2);
v.scale(2).scale(3);    // method chaining works in C++ too
```

---

## 6. Static Members

> **Interview Q:** *What is a static member? How is it different from an instance member?*

Static members belong to the **class**, not any specific instance. All objects share the same static member.

**Python:**
```python
class DatabaseConnection:
    _connection_count = 0          # class variable (static)
    MAX_CONNECTIONS = 10           # class constant

    def __init__(self, host):
        if DatabaseConnection._connection_count >= DatabaseConnection.MAX_CONNECTIONS:
            raise Exception("Too many connections!")
        self.host = host           # instance variable
        DatabaseConnection._connection_count += 1

    def __del__(self):
        DatabaseConnection._connection_count -= 1

    @classmethod
    def get_count(cls):            # classmethod — gets cls not self
        return cls._connection_count

    @staticmethod
    def is_valid_host(host: str):  # staticmethod — no self or cls
        return host.startswith("db.")

db1 = DatabaseConnection("db.main")
db2 = DatabaseConnection("db.replica")
print(DatabaseConnection.get_count())    # 2
print(DatabaseConnection.is_valid_host("db.main"))  # True
```

**C++:**
```cpp
class DatabaseConnection {
    static int connectionCount;       // static: shared across all instances
    std::string host;

public:
    static const int MAX_CONNECTIONS = 10;

    DatabaseConnection(std::string h) : host(h) {
        if (connectionCount >= MAX_CONNECTIONS)
            throw std::runtime_error("Too many connections!");
        ++connectionCount;
    }
    ~DatabaseConnection() { --connectionCount; }

    static int getCount() { return connectionCount; }  // static method
    static bool isValidHost(const std::string& h) {
        return h.substr(0, 3) == "db.";
    }
};

int DatabaseConnection::connectionCount = 0;   // define static member outside class

DatabaseConnection db1("db.main");
std::cout << DatabaseConnection::getCount();    // 1
```

### Method Types Comparison

| Type | Python | C++ | Has `self`/`this`? | Can access instance vars? |
|---|---|---|---|---|
| Instance method | `def f(self)` | `void f()` | Yes | Yes |
| Class method | `@classmethod def f(cls)` | N/A (use static) | Has `cls` | Class vars only |
| Static method | `@staticmethod def f()` | `static void f()` | No | No |

---

# PART 2 — THE FOUR PILLARS

---

## 7. Encapsulation

> **Interview Q:** *What is encapsulation? Give a real-world example. How is it achieved?*

**Encapsulation** = bundling data (attributes) + methods that operate on that data into one unit (a class), AND restricting direct access to internal state.

**Why:** Prevents external code from putting an object in an invalid state. Like a medicine capsule — contents protected, you only interact through the defined interface.

```
Without encapsulation:           With encapsulation:
  patient.heartRate = -50  ← BUG!    patient.setHeartRate(-50)  → raises error
  bank.balance = 99999     ← FRAUD   bank.deposit(-99999)       → validation runs
```

**Python — Full example:**
```python
class MLModel:
    def __init__(self, name):
        self.name = name
        self._is_trained = False       # protected
        self.__weights = []            # private (name-mangled)
        self.__training_history = []   # private

    def train(self, data):
        # Only this method can modify __weights
        self.__weights = self._compute_weights(data)
        self._is_trained = True
        self.__training_history.append({"epoch": len(self.__training_history)})

    def predict(self, x):
        if not self._is_trained:
            raise RuntimeError("Model must be trained before prediction!")
        return self.__forward_pass(x)

    @property
    def num_weights(self):             # read-only access to private data
        return len(self.__weights)

    def _compute_weights(self, data):  # protected helper
        return [0.1] * len(data)

    def __forward_pass(self, x):       # private helper
        return sum(w * xi for w, xi in zip(self.__weights, x))

model = MLModel("SimpleNet")
# model.__weights = [99]    # AttributeError — can't bypass encapsulation
model.train([1, 2, 3])
print(model.predict([0.5, 0.5, 0.5]))  # works through public interface
```

**C++:**
```cpp
class MLModel {
private:
    std::string name;
    bool isTrained = false;
    std::vector<float> weights;

    std::vector<float> computeWeights(const std::vector<float>& data) {
        return std::vector<float>(data.size(), 0.1f);
    }

public:
    MLModel(std::string n) : name(n) {}

    void train(const std::vector<float>& data) {
        weights = computeWeights(data);
        isTrained = true;
    }

    float predict(const std::vector<float>& x) {
        if (!isTrained) throw std::runtime_error("Train first!");
        float result = 0;
        for (int i = 0; i < weights.size(); i++) result += weights[i] * x[i];
        return result;
    }

    int numWeights() const { return weights.size(); }
    // NO public setter for weights — encapsulation enforced at compile time
};
```

---

## 8. Abstraction

> **Interview Q:** *What is abstraction? How is it different from encapsulation?*

**Abstraction** = hiding **implementation complexity**, exposing only **what** something does, not **how**.
**Encapsulation** = hiding **internal data**, controlling access through methods.

```
Analogy: Car
  Abstraction:    You know accelerate() → car goes faster. HOW the engine works? Hidden.
  Encapsulation:  The engine parts are physically protected inside the hood. Can't touch them.
```

| | Abstraction | Encapsulation |
|---|---|---|
| Hides | Implementation complexity | Data / internal state |
| Exposed | Interface (what to call) | Controlled access (getters/setters) |
| Achieved via | Abstract classes, interfaces, APIs | Access modifiers, properties |
| Level | Design level | Implementation level |

**Python — Abstraction via ABC:**
```python
from abc import ABC, abstractmethod

class DataProcessor(ABC):          # abstract class — can't instantiate
    @abstractmethod
    def load(self, path: str):     # must be implemented by subclasses
        pass

    @abstractmethod
    def process(self, data):
        pass

    @abstractmethod
    def save(self, data, path: str):
        pass

    def run_pipeline(self, input_path, output_path):  # concrete method
        """Template method — orchestrates abstract steps"""
        data = self.load(input_path)
        processed = self.process(data)
        self.save(processed, output_path)
        # Caller doesn't know HOW load/process/save work — only that they do

class CSVProcessor(DataProcessor):
    def load(self, path):
        import pandas as pd
        return pd.read_csv(path)
    def process(self, data):
        return data.dropna()
    def save(self, data, path):
        data.to_csv(path, index=False)

# DataProcessor()  ← TypeError: can't instantiate abstract class
processor = CSVProcessor()
processor.run_pipeline("raw.csv", "clean.csv")  # abstraction in action
```

**C++:**
```cpp
class DataProcessor {
public:
    virtual void load(const std::string& path) = 0;   // pure virtual → abstract
    virtual void process() = 0;
    virtual void save(const std::string& path) = 0;
    virtual ~DataProcessor() {}

    void runPipeline(const std::string& in, const std::string& out) {
        load(in);
        process();
        save(out);
    }
};

class CSVProcessor : public DataProcessor {
public:
    void load(const std::string& path) override { /* read CSV */ }
    void process() override { /* drop NaN */ }
    void save(const std::string& path) override { /* write CSV */ }
};
```

---

## 9. Inheritance

> **Interview Q:** *What is inheritance? What are the types? What does `super()` do?*

**Inheritance** = a class (child/derived) acquires properties and behaviours of another class (parent/base). Enables code reuse and "is-a" relationships.

```
Animal (base)
  ├── Dog (derived)   — Dog IS-A Animal
  ├── Cat (derived)   — Cat IS-A Animal
  └── Bird (derived)  — Bird IS-A Animal
```

### Types of Inheritance

| Type | Description | Python | C++ |
|---|---|---|---|
| **Single** | One parent | `class B(A)` | `class B : public A` |
| **Multiple** | Multiple parents | `class D(B, C)` | `class D : public B, public C` |
| **Multilevel** | Chain A → B → C | `class C(B)` where `class B(A)` | Same |
| **Hierarchical** | Multiple children, one parent | Multiple classes inherit `Animal` | Same |
| **Hybrid** | Combination of above | MRO handles it | Virtual inheritance needed |

**Python:**
```python
class Animal:
    def __init__(self, name, sound):
        self.name = name
        self.sound = sound

    def speak(self):
        return f"{self.name} says {self.sound}!"

    def __repr__(self):
        return f"{type(self).__name__}(name={self.name!r})"

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "Woof")   # call parent constructor
        self.breed = breed               # add own attribute

    def fetch(self, item):               # Dog-specific method
        return f"{self.name} fetched the {item}!"

    def speak(self):                     # override parent method
        return f"{self.name} barks loudly: WOOF WOOF!"

class GuideDog(Dog):                    # multilevel: GuideDog → Dog → Animal
    def __init__(self, name, breed, owner):
        super().__init__(name, breed)
        self.owner = owner

    def guide(self):
        return f"{self.name} is guiding {self.owner}"

rex = Dog("Rex", "German Shepherd")
print(rex.speak())    # "Rex barks loudly: WOOF WOOF!"  ← overridden
print(rex.fetch("ball"))  # "Rex fetched the ball!"
print(isinstance(rex, Dog))    # True
print(isinstance(rex, Animal)) # True — Dog IS-A Animal
print(issubclass(Dog, Animal)) # True
```

**C++:**
```cpp
class Animal {
public:
    std::string name;
    std::string sound;
    Animal(std::string n, std::string s) : name(n), sound(s) {}
    virtual std::string speak() {
        return name + " says " + sound + "!";
    }
    virtual ~Animal() {}   // virtual destructor is mandatory with polymorphism
};

class Dog : public Animal {
public:
    std::string breed;
    Dog(std::string n, std::string b) : Animal(n, "Woof"), breed(b) {}
    std::string speak() override {
        return name + " barks loudly: WOOF WOOF!";
    }
    std::string fetch(std::string item) {
        return name + " fetched the " + item + "!";
    }
};

class GuideDog : public Dog {
public:
    std::string owner;
    GuideDog(std::string n, std::string b, std::string o)
        : Dog(n, b), owner(o) {}
};
```

### `super()` — Calling Parent Methods

**Python `super()` follows MRO, not just direct parent:**
```python
class A:
    def greet(self): return "Hello from A"

class B(A):
    def greet(self):
        base = super().greet()    # calls A.greet()
        return f"B says hi, and {base}"

class C(B):
    def greet(self):
        base = super().greet()    # calls B.greet() (follows MRO: C→B→A)
        return f"C says hi, and {base}"

print(C().greet())
# "C says hi, and B says hi, and Hello from A"
```

**C++ — call parent explicitly:**
```cpp
class C : public B {
public:
    std::string greet() override {
        return "C says hi, and " + B::greet();  // explicit scope resolution
    }
};
```

---

## 10. Polymorphism

> **Interview Q:** *What is polymorphism? Explain compile-time vs runtime polymorphism with examples.*

**Polymorphism** = same interface, different behaviour depending on the actual type.
Greek: *poly* (many) + *morph* (form). One name, many forms.

### Two Types

| | Compile-Time (Static) | Runtime (Dynamic) |
|---|---|---|
| Also called | Early binding / static dispatch | Late binding / dynamic dispatch |
| Resolved at | Compile time | Runtime |
| Mechanism | **Overloading**, templates | **Overriding** + virtual functions |
| Speed | Zero overhead | Tiny vtable lookup (~1 indirection) |
| Flexibility | Fixed at compile time | Adapts to actual type at runtime |

### Runtime Polymorphism (the important one)

**Python — duck typing + method overriding:**
```python
class Shape:
    def area(self):
        raise NotImplementedError

class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self): return 3.14159 * self.r ** 2
    def __str__(self): return f"Circle(r={self.r})"

class Rectangle(Shape):
    def __init__(self, w, h): self.w, self.h = w, h
    def area(self): return self.w * self.h
    def __str__(self): return f"Rectangle({self.w}x{self.h})"

class Triangle(Shape):
    def __init__(self, b, h): self.b, self.h = b, h
    def area(self): return 0.5 * self.b * self.h

# Polymorphism — same function call, different behaviour
shapes = [Circle(5), Rectangle(4, 6), Triangle(3, 8)]
for shape in shapes:
    print(f"{shape.__class__.__name__}: area = {shape.area():.2f}")

# Works for ANY object with an area() method — duck typing!
def total_area(shapes):
    return sum(s.area() for s in shapes)
```

**C++ — requires `virtual` keyword:**
```cpp
class Shape {
public:
    virtual double area() const = 0;   // pure virtual
    virtual std::string name() const = 0;
    virtual ~Shape() {}
};

class Circle : public Shape {
    double r;
public:
    Circle(double r) : r(r) {}
    double area() const override { return 3.14159 * r * r; }
    std::string name() const override { return "Circle"; }
};

class Rectangle : public Shape {
    double w, h;
public:
    Rectangle(double w, double h) : w(w), h(h) {}
    double area() const override { return w * h; }
    std::string name() const override { return "Rectangle"; }
};

// Polymorphism via base pointer — MUST use pointer or reference
std::vector<Shape*> shapes = {new Circle(5), new Rectangle(4, 6)};
for (Shape* s : shapes) {
    std::cout << s->name() << ": " << s->area() << "\n";
    // Correct virtual function called based on actual type
}
for (Shape* s : shapes) delete s;
```

### Compile-Time Polymorphism

**Function Overloading:**
```python
# Python — no true overloading; use default args or *args
def process(data, mode="default"):
    if isinstance(data, list): ...
    elif isinstance(data, str): ...
    elif isinstance(data, int): ...
```

```cpp
// C++ — true overloading (different signatures)
void print(int x)    { std::cout << "int: " << x; }
void print(double x) { std::cout << "double: " << x; }
void print(std::string s) { std::cout << "string: " << s; }
// Compiler picks the right one based on argument type — at compile time
print(42);      // print(int)
print(3.14);    // print(double)
```

**Templates (C++) / Generics:**
```cpp
template <typename T>
T maximum(T a, T b) { return (a > b) ? a : b; }
maximum(3, 5);       // T=int — deduced at compile time
maximum(3.14, 2.71); // T=double
```

```python
# Python — duck typing is effectively generic
def maximum(a, b):
    return a if a > b else b
# Works for int, float, str — any comparable type
```

### Duck Typing (Python's secret weapon)

```python
# Python doesn't check the TYPE — it checks if the METHOD exists
# "If it walks like a duck and quacks like a duck, it's a duck"

class PDF:
    def area(self): return 100    # not a Shape subclass at all!

class Tensor:
    def area(self): return 9999

# Works perfectly! Python just calls .area() — doesn't care about class hierarchy
for obj in [Circle(5), PDF(), Tensor()]:
    print(obj.area())   # polymorphism via duck typing
```

---

# PART 3 — ADVANCED OOP

---

## 11. Abstract Classes & Interfaces

> **Interview Q:** *What is an abstract class? What is an interface? How are they different?*

| | Abstract Class | Interface |
|---|---|---|
| Has concrete methods? | Yes (mix of abstract + concrete) | No (only method signatures) |
| Has state/attributes? | Yes | No (typically) |
| C++ | Class with ≥1 pure virtual fn | Class with ALL pure virtual fns |
| Python | ABC with `@abstractmethod` | Protocol (duck-typed, C++20-style) |
| Instantiable? | No | No |
| Use case | Shared base with some implementation | Define a contract/capability |

**Python — Abstract Base Class:**
```python
from abc import ABC, abstractmethod

class Animal(ABC):
    def __init__(self, name):    # concrete: shared implementation
        self.name = name

    @abstractmethod
    def speak(self) -> str:      # abstract: subclass MUST implement
        pass

    @abstractmethod
    def move(self) -> str:
        pass

    def describe(self):          # concrete: reuse across all animals
        return f"I am {self.name}, I say {self.speak()} and {self.move()}"

class Dog(Animal):
    def speak(self): return "Woof!"
    def move(self): return "run"

# Animal()  ← TypeError: Can't instantiate abstract class Animal
# (unless ALL abstract methods are implemented)
rex = Dog("Rex")
print(rex.describe())   # "I am Rex, I say Woof! and run"
```

**Python — Protocol (Structural subtyping, PEP 544):**
```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...    # structural interface

class Circle:
    def draw(self): print("Drawing circle")   # no explicit inheritance needed!

class Square:
    def draw(self): print("Drawing square")

def render(shape: Drawable):   # accepts anything with a draw() method
    shape.draw()

render(Circle())   # works — Circle structurally satisfies Drawable
render(Square())   # works — no inheritance required
```

**C++:**
```cpp
// Abstract class (has some concrete methods)
class Animal {
    std::string name;
public:
    Animal(std::string n) : name(n) {}
    virtual std::string speak() = 0;    // pure virtual → must override
    virtual std::string move() = 0;
    std::string getName() const { return name; }   // concrete
    virtual ~Animal() {}
};

// Pure interface (all pure virtual)
class ISerialisable {
public:
    virtual std::string serialise() const = 0;
    virtual void deserialise(const std::string&) = 0;
    virtual ~ISerialisable() {}
};

// Class implementing both
class Dog : public Animal, public ISerialisable {
public:
    Dog(std::string n) : Animal(n) {}
    std::string speak() override { return "Woof!"; }
    std::string move() override { return "run"; }
    std::string serialise() const override { return "Dog:" + getName(); }
    void deserialise(const std::string& s) override { /* parse */ }
};
```

---

## 12. Operator Overloading & Magic Methods

> **Interview Q:** *What is operator overloading? How does Python implement it?*

Operator overloading lets you define what `+`, `-`, `*`, `==`, `<` etc. mean for your custom types.

**Python — via dunder (magic) methods:**
```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    # Arithmetic
    def __add__(self, other):       # v1 + v2
        return Vector(self.x + other.x, self.y + other.y)
    def __sub__(self, other):       # v1 - v2
        return Vector(self.x - other.x, self.y - other.y)
    def __mul__(self, scalar):      # v * 3
        return Vector(self.x * scalar, self.y * scalar)
    def __rmul__(self, scalar):     # 3 * v  (reflected)
        return self.__mul__(scalar)
    def __neg__(self):              # -v
        return Vector(-self.x, -self.y)
    def __abs__(self):              # abs(v)
        return (self.x**2 + self.y**2)**0.5

    # Comparison
    def __eq__(self, other):        # v1 == v2
        return self.x == other.x and self.y == other.y
    def __lt__(self, other):        # v1 < v2 (for sorting)
        return abs(self) < abs(other)
    def __le__(self, other):
        return abs(self) <= abs(other)

    # String representation
    def __repr__(self):             # repr(v), used in REPL, debugging
        return f"Vector({self.x}, {self.y})"
    def __str__(self):              # str(v), print(v)
        return f"({self.x}, {self.y})"

    # Container behaviour
    def __len__(self):              # len(v)
        return 2
    def __getitem__(self, i):       # v[0], v[1]
        return (self.x, self.y)[i]
    def __iter__(self):             # for c in v: ...
        yield self.x; yield self.y
    def __contains__(self, val):    # val in v
        return val in (self.x, self.y)

    # Hashable (dict key / set member)
    def __hash__(self):             # hash(v)
        return hash((self.x, self.y))
        # NOTE: if you define __eq__, Python deletes __hash__ by default!

    # Callable
    def __call__(self, scale):      # v(2) → scales in-place
        self.x *= scale; self.y *= scale; return self

    # Context manager
    def __enter__(self): return self
    def __exit__(self, *args): return False

    # Boolean
    def __bool__(self):             # bool(v) — False if zero vector
        return self.x != 0 or self.y != 0

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)           # Vector(4, 6)
print(3 * v1)            # Vector(3, 6)
print(sorted([v2, v1]))  # [Vector(1,2), Vector(3,4)] — uses __lt__
d = {v1: "origin"}       # works as dict key — uses __hash__
```

**C++ — operator overloading:**
```cpp
class Vector {
public:
    float x, y;
    Vector(float x=0, float y=0) : x(x), y(y) {}

    Vector operator+(const Vector& o) const { return {x+o.x, y+o.y}; }
    Vector operator-(const Vector& o) const { return {x-o.x, y-o.y}; }
    Vector operator*(float s) const { return {x*s, y*s}; }
    bool operator==(const Vector& o) const { return x==o.x && y==o.y; }
    bool operator<(const Vector& o) const {
        return std::hypot(x,y) < std::hypot(o.x, o.y);
    }
    float operator[](int i) const { return i==0 ? x : y; }

    // Non-member (friend) for: 3.0f * v (scalar * vector)
    friend Vector operator*(float s, const Vector& v) { return v * s; }

    // Output stream operator
    friend std::ostream& operator<<(std::ostream& os, const Vector& v) {
        return os << "(" << v.x << ", " << v.y << ")";
    }
};

Vector v1(1, 2), v2(3, 4);
std::cout << v1 + v2;   // (4, 6)
std::cout << 3.0f * v1; // (3, 6)
```

### Complete Dunder Reference

```python
# Lifecycle
__new__(cls)              # object creation
__init__(self)            # initialisation
__del__(self)             # destruction (GC-managed)

# String
__repr__(self)            # repr(obj) — for developers
__str__(self)             # str(obj), print(obj) — for users
__format__(self, spec)    # format(obj, spec), f"{obj:.2f}"

# Arithmetic
__add__, __radd__         # + and reversed +
__sub__, __rsub__         # -
__mul__, __rmul__         # *
__truediv__, __floordiv__ # / and //
__mod__                   # %
__pow__                   # **
__neg__, __pos__, __abs__ # unary -, +, abs()

# Bitwise
__and__, __or__, __xor__, __lshift__, __rshift__

# Comparison
__eq__, __ne__            # ==, !=
__lt__, __le__, __gt__, __ge__  # <, <=, >, >=

# Container
__len__                   # len()
__getitem__, __setitem__, __delitem__  # [], []=, del []
__contains__              # in
__iter__, __next__        # for loop, next()
__reversed__              # reversed()

# Callable
__call__                  # obj()

# Context manager
__enter__, __exit__       # with obj:

# Attribute access
__getattr__, __setattr__, __delattr__  # dynamic attribute access
__getattribute__          # ALL attribute access (dangerous to override)

# Type checking
__instancecheck__         # isinstance()
__subclasscheck__         # issubclass()

# Memory
__sizeof__                # sys.getsizeof()
__slots__                 # restrict instance dict
```

---

## 13. Multiple Inheritance & Diamond Problem

> **Interview Q:** *What is the diamond problem? How does Python solve it? How does C++?*

```
Diamond Problem:
        A
       / \
      B   C
       \ /
        D

D inherits from B and C, both of which inherit from A.
If D calls a method from A, which path is used? B→A or C→A?
If A's __init__ is called twice, that's a bug.
```

**Python — C3 Linearisation (MRO):**
```python
class A:
    def __init__(self):
        print("A.__init__")
    def method(self): return "A"

class B(A):
    def __init__(self):
        print("B.__init__")
        super().__init__()       # super() follows MRO, not just direct parent
    def method(self): return "B → " + super().method()

class C(A):
    def __init__(self):
        print("C.__init__")
        super().__init__()
    def method(self): return "C → " + super().method()

class D(B, C):
    def __init__(self):
        print("D.__init__")
        super().__init__()       # follows D→B→C→A (MRO), A called ONCE
    def method(self): return "D → " + super().method()

print(D.__mro__)
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)

d = D()
# D.__init__
# B.__init__
# C.__init__
# A.__init__      ← called exactly once!

print(d.method()) # "D → B → C → A"
```

**C++ — Virtual Inheritance:**
```cpp
class A {
public:
    int value = 42;
    virtual void show() { std::cout << "A\n"; }
};

// WITHOUT virtual inheritance: D gets TWO copies of A
class B : public A {};   // B::A
class C : public A {};   // C::A
class D : public B, public C {};
// D d; d.value;   // AMBIGUOUS! Which A? B::A or C::A?

// WITH virtual inheritance: D gets exactly ONE copy of A
class B : virtual public A {};
class C : virtual public A {};
class D : public B, public C {};   // D has ONE shared A
D d;
d.value;   // OK — unambiguous, shared A
d.show();  // A's show — unambiguous
```

> **Key insight:** Python's `super()` + MRO makes cooperative multiple inheritance work correctly — each `super().__init__()` call in a chain is forwarded through MRO, ensuring each ancestor is initialised exactly once. In C++, `virtual` keyword on the inheritance specifier enables virtual base classes that are shared rather than duplicated.

---

## 14. Composition vs Inheritance

> **Interview Q:** *When do you prefer composition over inheritance?*

**Inheritance** = "IS-A" relationship. A Dog IS-A Animal.
**Composition** = "HAS-A" relationship. A Car HAS-A Engine.

> **Golden rule: "Prefer composition over inheritance"** — Gang of Four, Design Patterns (1994)

| | Inheritance | Composition |
|---|---|---|
| Relationship | IS-A | HAS-A |
| Coupling | Tight (child depends on parent internals) | Loose (via interface) |
| Flexibility | Rigid hierarchy | Flexible — swap components |
| Code reuse | Reuse parent's methods | Reuse component objects |
| Use when | True IS-A relationship | Want to reuse behaviour flexibly |

**Bad inheritance (misuse):**
```python
# BAD: Stack inheriting from List — Stack IS NOT A List
# (Stack should only have push/pop/peek, not insert/remove/sort...)
class Stack(list):
    def push(self, item): self.append(item)
    def pop(self): return super().pop()
    # But now Stack exposes insert(), extend(), sort() — violates Stack abstraction!

# GOOD: Composition — Stack HAS-A list internally
class Stack:
    def __init__(self):
        self._data = []          # composed object — HAS-A list

    def push(self, item):
        self._data.append(item)

    def pop(self):
        if not self._data:
            raise IndexError("Stack is empty")
        return self._data.pop()

    def peek(self):
        return self._data[-1]

    def __len__(self):
        return len(self._data)
```

**Composition with dependency injection:**
```python
class Logger:
    def log(self, msg): print(f"[LOG] {msg}")

class FileLogger(Logger):
    def log(self, msg):
        with open("app.log", "a") as f: f.write(f"{msg}\n")

class PaymentProcessor:
    def __init__(self, logger: Logger):     # injected dependency
        self.logger = logger

    def process(self, amount):
        self.logger.log(f"Processing ${amount}")
        # ... payment logic

# Flexible: swap logger without changing PaymentProcessor
p1 = PaymentProcessor(Logger())       # console logger
p2 = PaymentProcessor(FileLogger())   # file logger
```

---

## 15. Special Python OOP Features

### Dataclasses (Python 3.7+)

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class ModelConfig:
    name: str
    learning_rate: float = 0.001
    batch_size: int = 32
    layers: List[int] = field(default_factory=lambda: [64, 128, 64])
    # Auto-generates: __init__, __repr__, __eq__

    def total_params(self):
        return sum(self.layers)

cfg = ModelConfig("ResNet", learning_rate=0.01)
print(cfg)   # ModelConfig(name='ResNet', learning_rate=0.01, ...)
cfg2 = ModelConfig("ResNet", learning_rate=0.01)
print(cfg == cfg2)   # True — __eq__ compares all fields

# Frozen dataclass (immutable)
@dataclass(frozen=True)
class Point:
    x: float
    y: float
# p = Point(1.0, 2.0); p.x = 3.0  ← FrozenInstanceError
```

### Metaclasses

```python
# Metaclass: the class of a class. type is the default metaclass of all classes.
# class MyClass:  ← equivalent to: MyClass = type("MyClass", (object,), {})

class SingletonMeta(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    def __init__(self, url):
        self.url = url

db1 = Database("postgresql://localhost/mydb")
db2 = Database("something else")
print(db1 is db2)    # True — same instance
print(db2.url)       # "postgresql://localhost/mydb" — __init__ not called again
```

### Descriptors

```python
# Descriptor: object that defines __get__, __set__, __delete__
# This is how @property, @classmethod, @staticmethod are implemented!

class Validator:
    def __init__(self, min_val, max_val):
        self.min_val = min_val
        self.max_val = max_val

    def __set_name__(self, owner, name):    # called when class is created
        self.name = name
        self.private_name = f"_{name}"

    def __get__(self, obj, objtype=None):
        if obj is None: return self         # accessed on class, not instance
        return getattr(obj, self.private_name, None)

    def __set__(self, obj, value):
        if not (self.min_val <= value <= self.max_val):
            raise ValueError(f"{self.name} must be between {self.min_val} and {self.max_val}")
        setattr(obj, self.private_name, value)

class NeuralLayer:
    dropout = Validator(0.0, 1.0)    # descriptor instance
    neurons = Validator(1, 10000)

    def __init__(self, neurons, dropout):
        self.neurons = neurons       # calls Validator.__set__
        self.dropout = dropout

layer = NeuralLayer(128, 0.3)
layer.dropout = 1.5    # ValueError: dropout must be between 0.0 and 1.0
```

### `__slots__`

```python
class NormalClass:
    def __init__(self, x, y):
        self.x = x
        self.y = y
# Has __dict__ — flexible, can add any attribute, but uses ~400+ bytes per instance

class OptimisedClass:
    __slots__ = ("x", "y")     # pre-declares allowed attributes
    def __init__(self, x, y):
        self.x = x
        self.y = y
# NO __dict__ — only x and y allowed, uses ~56 bytes per instance
# Can't add new attributes: obj.z = 5 ← AttributeError

# Crucial for ML: if you have 10 million Feature objects, __slots__ saves gigabytes
```

---

## 16. Special C++ OOP Features

### `virtual` Destructor (Critical!)

```cpp
class Base {
public:
    ~Base() { std::cout << "Base destructor\n"; }  // non-virtual — BUG!
};
class Derived : public Base {
    int* data = new int[100];
public:
    ~Derived() { delete[] data; }
};

Base* b = new Derived();
delete b;   // Only Base::~Base() called! Derived::~Derived() SKIPPED → MEMORY LEAK

// Fix: make base destructor virtual
class Base {
public:
    virtual ~Base() {}   // now Derived::~Derived() is called first
};
```

> **Rule: If a class has any virtual functions, ALWAYS declare its destructor virtual.**

### `override` and `final` (C++11)

```cpp
class Base {
public:
    virtual void process() {}
    virtual void compute() final {}   // final: cannot be overridden
};

class Derived : public Base {
public:
    void process() override {}     // OK — override keyword catches typos at compile time
    // void proces() override {}  // COMPILE ERROR — typo caught!
    // void compute() override {} // COMPILE ERROR — final function
};

class Leaf final : public Derived { // final class: cannot be inherited
};
// class Child : public Leaf {}   // COMPILE ERROR
```

### Friend Functions and Classes

```cpp
class Matrix {
    double data[3][3];

public:
    // Friend function can access private members
    friend Matrix operator*(const Matrix& a, const Matrix& b);
    friend std::ostream& operator<<(std::ostream& os, const Matrix& m);

    // Friend class can access all private members
    friend class MatrixSolver;
};

Matrix operator*(const Matrix& a, const Matrix& b) {
    Matrix result;
    // access a.data and b.data directly (private!)
    return result;
}
```

---

# PART 4 — DESIGN PRINCIPLES

---

## 17. SOLID Principles

> **Interview Q:** *What are the SOLID principles? Give an example for each.*

SOLID is an acronym for five design principles that make software more maintainable, extensible, and testable.

### S — Single Responsibility Principle (SRP)

**A class should have only ONE reason to change.**

```python
# BAD — UserManager does too much
class UserManager:
    def create_user(self, data): ...       # user logic
    def send_welcome_email(self, user): ...  # email logic — different responsibility
    def log_activity(self, action): ...    # logging — different responsibility

# GOOD — each class has one responsibility
class UserService:
    def __init__(self, email_service, logger):
        self.email_service = email_service
        self.logger = logger

    def create_user(self, data):
        user = User(data)
        self.email_service.send_welcome(user)
        self.logger.log(f"Created user {user.id}")
        return user

class EmailService:
    def send_welcome(self, user): ...

class ActivityLogger:
    def log(self, action): ...
```

```cpp
// BAD
class Report {
public:
    void generateData() {}
    void formatAsHTML() {}   // formatting responsibility
    void saveToFile() {}     // persistence responsibility
};

// GOOD — separate responsibilities
class ReportData { void generate() {} };
class HTMLFormatter { std::string format(ReportData&) {} };
class FileWriter { void save(const std::string& content, const std::string& path) {} };
```

### O — Open/Closed Principle (OCP)

**Open for extension, closed for modification.**

```python
# BAD — must modify existing code to add new discount type
class DiscountService:
    def calculate(self, user_type, amount):
        if user_type == "student": return amount * 0.8
        elif user_type == "senior": return amount * 0.7
        # Adding "employee" → must modify this class (violates OCP)

# GOOD — extend by adding new class, not modifying existing
from abc import ABC, abstractmethod

class Discount(ABC):
    @abstractmethod
    def apply(self, amount: float) -> float: pass

class StudentDiscount(Discount):
    def apply(self, amount): return amount * 0.8

class SeniorDiscount(Discount):
    def apply(self, amount): return amount * 0.7

class EmployeeDiscount(Discount):     # New type — no existing code changed!
    def apply(self, amount): return amount * 0.6

class DiscountService:
    def calculate(self, discount: Discount, amount: float):
        return discount.apply(amount)
```

### L — Liskov Substitution Principle (LSP)

**Objects of a subclass must be substitutable for objects of the superclass without breaking the program.**

```python
# BAD violation: Square IS NOT substitutable for Rectangle
class Rectangle:
    def __init__(self, w, h): self.w, self.h = w, h
    def set_width(self, w): self.w = w
    def set_height(self, h): self.h = h
    def area(self): return self.w * self.h

class Square(Rectangle):    # VIOLATES LSP!
    def set_width(self, w): self.w = self.h = w   # unexpected side effect
    def set_height(self, h): self.w = self.h = h  # changes both dimensions

def test_rectangle(rect: Rectangle):
    rect.set_width(4)
    rect.set_height(5)
    assert rect.area() == 20   # Fails for Square (area = 25, not 20)!

# GOOD — don't force Square into Rectangle hierarchy
class Shape(ABC):
    @abstractmethod
    def area(self): pass

class Rectangle(Shape):
    def __init__(self, w, h): self.w, self.h = w, h
    def area(self): return self.w * self.h

class Square(Shape):            # Square IS-A Shape, NOT IS-A Rectangle
    def __init__(self, s): self.s = s
    def area(self): return self.s ** 2
```

### I — Interface Segregation Principle (ISP)

**No client should be forced to depend on interfaces it does not use.**

```python
# BAD — fat interface
class Worker(ABC):
    @abstractmethod
    def work(self): pass
    @abstractmethod
    def eat(self): pass    # Robot doesn't eat!
    @abstractmethod
    def sleep(self): pass  # Robot doesn't sleep!

# GOOD — segregated interfaces
class Workable(Protocol):
    def work(self): ...

class Eatable(Protocol):
    def eat(self): ...

class HumanWorker:          # implements all
    def work(self): print("Working")
    def eat(self): print("Eating")
    def sleep(self): print("Sleeping")

class RobotWorker:          # implements only what it needs
    def work(self): print("Robot working")
    # No eat() or sleep() — not forced to implement them
```

### D — Dependency Inversion Principle (DIP)

**High-level modules should not depend on low-level modules. Both should depend on abstractions.**

```python
# BAD — high-level depends directly on low-level
class MySQLDatabase:
    def query(self, sql): ...

class UserService:
    def __init__(self):
        self.db = MySQLDatabase()   # tightly coupled — can't swap database!

# GOOD — depend on abstraction
class Database(Protocol):
    def query(self, sql: str) -> list: ...

class MySQLDatabase:
    def query(self, sql): ...   # implements Database protocol

class PostgresDatabase:
    def query(self, sql): ...   # alternative implementation

class UserService:
    def __init__(self, db: Database):   # depends on abstraction!
        self.db = db

# Now you can inject any database
service = UserService(MySQLDatabase())
service = UserService(PostgresDatabase())    # swap easily
service = UserService(MockDatabase())        # easy to test!
```

---

## 18. Design Patterns

> **Interview Q:** *What is a design pattern? Explain Singleton, Factory, and Observer.*

Design patterns are reusable solutions to commonly occurring software design problems.

| Category | Purpose | Examples |
|---|---|---|
| **Creational** | Object creation | Singleton, Factory, Builder, Prototype |
| **Structural** | Object composition | Adapter, Decorator, Facade, Proxy |
| **Behavioural** | Object communication | Observer, Strategy, Command, Iterator |

### Singleton — One instance only

```python
class DatabasePool:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.connections = []
        return cls._instance

    def add_connection(self, conn):
        self.connections.append(conn)

db1 = DatabasePool()
db2 = DatabasePool()
print(db1 is db2)   # True
```

```cpp
class DatabasePool {
    DatabasePool() {}   // private constructor
    std::vector<std::string> connections;
public:
    static DatabasePool& getInstance() {
        static DatabasePool instance;   // thread-safe in C++11
        return instance;
    }
    DatabasePool(const DatabasePool&) = delete;   // no copy
    void addConn(const std::string& c) { connections.push_back(c); }
};
DatabasePool::getInstance().addConn("conn1");
```

### Factory — Create objects without specifying exact class

```python
from abc import ABC, abstractmethod

class Model(ABC):
    @abstractmethod
    def predict(self, x): pass

class LinearModel(Model):
    def predict(self, x): return sum(x)

class NeuralModel(Model):
    def predict(self, x): return max(x)   # simplified

class ModelFactory:
    @staticmethod
    def create(model_type: str) -> Model:
        models = {
            "linear": LinearModel,
            "neural": NeuralModel,
        }
        if model_type not in models:
            raise ValueError(f"Unknown model type: {model_type}")
        return models[model_type]()   # instantiate and return

model = ModelFactory.create("neural")
model.predict([1, 2, 3])
```

### Observer — Notify multiple objects on state change

```python
from abc import ABC, abstractmethod
from typing import List

class Observer(ABC):
    @abstractmethod
    def update(self, event: str, data): pass

class Subject:
    def __init__(self):
        self._observers: List[Observer] = []

    def subscribe(self, observer: Observer):
        self._observers.append(observer)

    def unsubscribe(self, observer: Observer):
        self._observers.remove(observer)

    def notify(self, event: str, data=None):
        for observer in self._observers:
            observer.update(event, data)

class MLTrainer(Subject):
    def __init__(self):
        super().__init__()
        self.epoch = 0
        self.loss = float('inf')

    def train_epoch(self, data):
        self.epoch += 1
        self.loss = self.loss * 0.9   # simplified
        self.notify("epoch_end", {"epoch": self.epoch, "loss": self.loss})

class TensorboardLogger(Observer):
    def update(self, event, data):
        if event == "epoch_end":
            print(f"[TensorBoard] Epoch {data['epoch']}: loss={data['loss']:.4f}")

class EarlyStopper(Observer):
    def __init__(self, patience=5):
        self.patience = patience
        self.best_loss = float('inf')
        self.counter = 0

    def update(self, event, data):
        if event == "epoch_end":
            if data['loss'] < self.best_loss:
                self.best_loss = data['loss']
                self.counter = 0
            else:
                self.counter += 1
                if self.counter >= self.patience:
                    print("Early stopping triggered!")

trainer = MLTrainer()
trainer.subscribe(TensorboardLogger())
trainer.subscribe(EarlyStopper(patience=3))
for _ in range(5): trainer.train_epoch(None)
```

### Decorator Pattern (structural — wraps objects)

```python
# Python's @decorator is syntax sugar, but Decorator PATTERN wraps an object
from abc import ABC, abstractmethod

class Coffee(ABC):
    @abstractmethod
    def cost(self): pass
    @abstractmethod
    def description(self): pass

class SimpleCoffee(Coffee):
    def cost(self): return 1.0
    def description(self): return "Coffee"

class CoffeeDecorator(Coffee):
    def __init__(self, coffee: Coffee):
        self._coffee = coffee

class MilkDecorator(CoffeeDecorator):
    def cost(self): return self._coffee.cost() + 0.25
    def description(self): return self._coffee.description() + ", Milk"

class SugarDecorator(CoffeeDecorator):
    def cost(self): return self._coffee.cost() + 0.10
    def description(self): return self._coffee.description() + ", Sugar"

coffee = SugarDecorator(MilkDecorator(SimpleCoffee()))
print(coffee.description())  # "Coffee, Milk, Sugar"
print(coffee.cost())         # 1.35
```

---

## 19. Relationships: Association, Aggregation, Composition

> **Interview Q:** *What is the difference between association, aggregation, and composition?*

| | Association | Aggregation | Composition |
|---|---|---|---|
| Relationship | "uses" | "has-a" (weak) | "has-a" (strong) |
| Ownership | No ownership | Part can exist without whole | Part cannot exist without whole |
| Lifecycle | Independent | Independent | Part dies with whole |
| UML | Solid line → | Hollow diamond ◇ | Filled diamond ◆ |
| Example | Teacher teaches Student | Department has Professors | House has Rooms |

```python
# Association — Teacher and Student exist independently
class Student:
    def __init__(self, name): self.name = name

class Teacher:
    def teach(self, student: Student):   # uses Student, doesn't own it
        print(f"Teaching {student.name}")

# Aggregation — Department has Professors, but Professors exist independently
class Professor:
    def __init__(self, name): self.name = name

class Department:
    def __init__(self, name):
        self.name = name
        self.professors: list = []       # holds references, doesn't own

    def add_professor(self, prof: Professor):
        self.professors.append(prof)

prof = Professor("Dr. Smith")       # exists independently
dept = Department("CS")
dept.add_professor(prof)
del dept                            # dept gone, prof still exists!

# Composition — House has Rooms; Rooms can't exist without House
class Room:
    def __init__(self, name): self.name = name

class House:
    def __init__(self, address):
        self.address = address
        self.rooms = [Room("Living"), Room("Bedroom"), Room("Kitchen")]
        # Rooms created inside House — owned by House
        # When House is destroyed, Rooms are destroyed too
```

---

# PART 5 — INTERVIEW Q&A

---

## 20. Interview Q&A — Fundamentals

**Q: What are the four pillars of OOP?**
> **Encapsulation** — bundling data and methods, restricting direct access to internal state. **Abstraction** — hiding implementation complexity, exposing only necessary interface. **Inheritance** — deriving new classes from existing ones, enabling code reuse and IS-A relationships. **Polymorphism** — same interface, different behaviour depending on actual type.

**Q: What is the difference between a class and an object?**
> A class is a blueprint or template — it defines attributes and methods but allocates no memory. An object is an instance of a class — it allocates memory and holds actual data. You can create many objects from one class, each with independent state.

**Q: What is the difference between `__init__` (Python) and a constructor (C++)?**
> Python's `__init__` initialises an already-created object (`__new__` creates it). C++ constructors both allocate (for stack) or initialise (for heap) and set up the object. Conceptually equivalent — called automatically when an object is created. Key difference: Python's `__del__` destructor timing is non-deterministic (GC decides when). C++ destructors are deterministic — called the moment scope ends (stack) or `delete` is called (heap).

**Q: What is the difference between shallow copy and deep copy?**
> A shallow copy creates a new container object but references the same inner objects. A deep copy creates a fully independent copy — all nested objects are also copied. In Python: `copy.copy()` = shallow, `copy.deepcopy()` = deep. In C++: the default copy constructor does a member-wise copy (shallow for pointers — you must write a deep copy constructor manually).

```python
import copy
original = [[1, 2], [3, 4]]
shallow  = copy.copy(original)
deep     = copy.deepcopy(original)

original[0].append(99)
print(shallow[0])   # [1, 2, 99] — affected (shared inner list)
print(deep[0])      # [1, 2]     — unaffected (independent copy)
```

**Q: What is method resolution order (MRO)?**
> The order in which Python searches classes for a method in multiple inheritance. Python uses the **C3 Linearisation algorithm** — left-to-right, depth-first, each class appearing only once, children before parents. Inspect it with `ClassName.__mro__`. `super()` follows MRO, not the direct parent — enabling cooperative multiple inheritance.

---

## 21. Interview Q&A — Pillars Deep Dive

**Q: Explain abstraction vs encapsulation with a real example.**
> Both hide things but at different levels. **Encapsulation:** A `BankAccount` class hides the `_balance` attribute and only exposes `deposit()` and `withdraw()`. External code can't set `balance = -1000` directly — internal data is protected. **Abstraction:** When you call `account.withdraw(100)`, you don't know whether it checks overdraft limits, logs the transaction, or updates a ledger — those details are abstracted away. You just know "withdraw deducts money." Encapsulation is about protecting data; abstraction is about hiding how something works.

**Q: What is the difference between method overloading and overriding?**
> **Overloading (compile-time polymorphism):** Multiple methods with the same name but different parameter types/counts in the same class. Resolved at compile time. C++ supports it natively. Python doesn't support true overloading — you simulate with default arguments, `*args`, or `isinstance` checks. **Overriding (runtime polymorphism):** A subclass provides a new implementation of a method inherited from the parent. The method signature is the same. Resolved at runtime via virtual dispatch (C++) or naturally (Python).

**Q: What is the Liskov Substitution Principle and why does it matter?**
> LSP states that wherever you use a base class object, you should be able to replace it with a subclass object without breaking the program. If a function works correctly with `Animal`, it must work correctly with `Dog` (a subclass). Violation: `Square extends Rectangle` breaks LSP because `set_width(4)` on a Square also changes height — a Rectangle user wouldn't expect this. LSP ensures inheritance is used correctly and subclasses truly ARE-A base class behaviourally, not just structurally.

**Q: What is duck typing and how does it relate to polymorphism?**
> "If it walks like a duck and quacks like a duck, it's a duck." Python checks whether an object has the required methods/attributes, not whether it's of a specific type. This enables polymorphism without inheritance — any object with an `area()` method can be passed to a function expecting a `Shape`, even if it doesn't extend `Shape`. Advantage: more flexible and less boilerplate. Disadvantage: errors caught at runtime, not compile time. Python's `typing.Protocol` adds optional structural type checking.

---

## 22. Interview Q&A — Advanced & Tricky

**Q: Can we achieve abstraction without inheritance?**
> Yes. In Python, via Protocols (structural subtyping) — any object implementing the required methods satisfies the protocol without inheriting from it. In C++, via templates — a template function works with any type that has the required operations (`operator<`, `size()` etc.) without inheriting from an interface. In both languages, via duck typing and composition.

**Q: What happens if you don't define a virtual destructor in C++?**
> Memory leak and undefined behaviour. If you `delete` a derived object through a base pointer, only the base destructor is called. The derived destructor is skipped — any resources the derived class allocated (heap memory, file handles, sockets) are leaked. Fix: always declare the destructor `virtual` in any class intended to be a base class.

**Q: What is object slicing in C++?**
> When a derived object is copied into a base object by value, the derived-specific data is "sliced off" — only the base class portion is copied. The derived methods are also lost. This is a common C++ bug. Fix: always use pointers or references to base class when working with polymorphism (`Animal*` or `Animal&`, never `Animal` by value).

```cpp
Dog dog("Rex");
Animal a = dog;    // SLICING! a is just an Animal — dog-specific data lost
a.speak();         // calls Animal::speak(), not Dog::speak()

Animal& ref = dog; // OK — reference, no slicing
ref.speak();       // calls Dog::speak() correctly
```

**Q: Can a constructor be virtual in C++? Can a destructor?**
> **Constructor: No.** Virtual functions require a vtable, and the vtable pointer is set up by the constructor. Before the constructor runs, there's no object — so virtual dispatch has nothing to dispatch to. **Destructor: Yes, and it SHOULD be** for base classes. Virtual destructor ensures the derived destructor is called when deleting through a base pointer.

**Q: What is the difference between `override` and `virtual` in C++?**
> `virtual` in the base class marks a function as overridable. `override` in the derived class explicitly tells the compiler "I intend to override a base class virtual function." The `override` keyword is not required but is highly recommended — it causes a compile error if you accidentally misspell the function name or use a different signature (catching typos that would otherwise silently create a new function instead of overriding).

**Q: What is name mangling in Python?**
> When you prefix an attribute with double underscores (`__attr`), Python renames it to `_ClassName__attr`. For example, `self.__pin` in `class BankAccount` becomes `_BankAccount__pin`. This prevents accidental access from subclasses (since the subclass would need to know the parent's name to access it) and avoids naming conflicts in complex inheritance hierarchies. It is NOT true privacy — you can always access `obj._BankAccount__pin` directly.

**Q: What is the difference between `@classmethod` and `@staticmethod`?**
> `@classmethod` receives the class (`cls`) as the first argument — it can create instances (factory methods), access class variables, and is inherited by subclasses (calling `cls()` creates the right subclass). `@staticmethod` receives no implicit argument — it's a regular function scoped to the class namespace, used for utility functions related to the class but not needing class or instance state.

---

## 23. Interview Q&A — Python vs C++ Specific

**Q: How does Python achieve OOP differently from C++?**

| Feature | Python | C++ |
|---|---|---|
| Access control | Convention only (`_`, `__` mangling) | Compiler-enforced (`private`, `protected`) |
| Virtual functions | ALL methods virtual by default | Must explicitly mark `virtual` |
| Abstract classes | `abc.ABC` + `@abstractmethod` | Pure virtual functions (`= 0`) |
| Multiple inheritance | MRO + `super()` for diamond | Virtual inheritance |
| Operator overloading | Dunder methods (`__add__`, etc.) | `operator+`, `operator==`, etc. |
| Type checking | Duck typing (runtime) | Static types (compile time) |
| Memory | Automatic GC | Manual / smart pointers |
| Constructor | `__init__` (initialises) | Constructor (creates + initialises) |
| Destructor | `__del__` (non-deterministic) | `~ClassName()` (deterministic) |
| Interfaces | `Protocol` or ABC | Pure abstract class |
| Class variables | Defined in class body | `static` member |

**Q: In Python, can you access private members from outside the class?**
> Yes, through name mangling. `obj.__attr` raises `AttributeError`, but `obj._ClassName__attr` works. Python's "private" is enforced by convention and mangling, not by the interpreter. C++ enforces truly at compile time — accessing a `private` member from outside is a compile error with no workaround.

**Q: What is the GIL and how does it affect OOP in Python?**
> The Global Interpreter Lock prevents multiple threads from executing Python bytecode simultaneously. This means instance methods called from multiple threads are NOT automatically thread-safe — you need explicit locking (`threading.Lock`) if multiple threads share objects. C++ has no GIL — methods are truly parallelisable, but you must handle synchronisation yourself.

**Q: What is RAII and how does Python differ?**
> Resource Acquisition Is Initialisation — in C++, resources are acquired in constructors and released in destructors. Since destructors run deterministically when objects go out of scope, resources are always released. Python uses context managers (`with` statement) as a substitute — `__enter__`/`__exit__` provide deterministic cleanup. Python's `__del__` is NOT reliable (GC timing is non-deterministic, and circular references can prevent it from running).

---

## 24. Scenario-Based Questions

**Q: Design a class hierarchy for a machine learning pipeline.**
```python
from abc import ABC, abstractmethod

class Transformer(ABC):           # abstract base
    @abstractmethod
    def fit(self, X): pass
    @abstractmethod
    def transform(self, X): pass
    def fit_transform(self, X):   # template method
        self.fit(X)
        return self.transform(X)

class Scaler(Transformer):
    def __init__(self): self.mean_ = self.std_ = None
    def fit(self, X):
        import numpy as np
        self.mean_ = np.mean(X, axis=0)
        self.std_ = np.std(X, axis=0) + 1e-8
    def transform(self, X): return (X - self.mean_) / self.std_

class PCA(Transformer):
    def __init__(self, n_components): self.n = n_components
    def fit(self, X): ...
    def transform(self, X): ...

class Pipeline:                   # composition — HAS-A list of Transformers
    def __init__(self, steps: list[Transformer]):
        self.steps = steps
    def fit_transform(self, X):
        for step in self.steps:
            X = step.fit_transform(X)
        return X

pipe = Pipeline([Scaler(), PCA(10)])
```

**Q: How would you implement a neural network layer using OOP?**
```python
import numpy as np
from abc import ABC, abstractmethod

class Layer(ABC):
    @abstractmethod
    def forward(self, x: np.ndarray) -> np.ndarray: pass
    @abstractmethod
    def backward(self, grad: np.ndarray) -> np.ndarray: pass

class Linear(Layer):
    def __init__(self, in_features, out_features):
        self.W = np.random.randn(in_features, out_features) * 0.01
        self.b = np.zeros(out_features)
        self._cache = None

    def forward(self, x):
        self._cache = x
        return x @ self.W + self.b

    def backward(self, grad):
        self.dW = self._cache.T @ grad
        self.db = grad.sum(axis=0)
        return grad @ self.W.T

class ReLU(Layer):
    def forward(self, x):
        self._cache = x
        return np.maximum(0, x)
    def backward(self, grad):
        return grad * (self._cache > 0)

# PyTorch does EXACTLY this — nn.Module is this Layer abstract class
# torch.nn.Linear is this Linear class
# The .forward() method override is METHOD OVERRIDING / RUNTIME POLYMORPHISM
```

**Q: What design pattern does PyTorch use for neural networks?**
> PyTorch's `nn.Module` is the **Template Method pattern** — `forward()` is an abstract method that every module must override, while `__call__()` in the base class wraps `forward()` with hooks, gradient tracking, and device handling. `nn.Sequential` is the **Composite pattern** — it composes multiple modules and treats them as one. When PyTorch's optimizer updates weights, it uses the **Visitor pattern** (iterating over all parameters). The `DataLoader` is an **Iterator pattern**. Modern ML frameworks are deeply OOP.

---

## Quick Reference Cheat Sheet

```
4 Pillars:
  Encapsulation  → data hidden inside class, accessed via methods
  Abstraction    → hide HOW, expose WHAT (ABC, interfaces)
  Inheritance    → IS-A, code reuse, class hierarchy
  Polymorphism   → same call, different behaviour (override / virtual)

Python-specific:
  self           → explicit first arg to instance methods
  __init__       → initialiser (not the creator — __new__ creates)
  @property      → getter/setter without () syntax
  @classmethod   → gets cls, not self (factory methods)
  @staticmethod  → no self or cls (utility functions)
  __slots__      → no __dict__, saves memory (~7x for many instances)
  super()        → follows MRO, not just direct parent
  MRO            → C3 linearisation: left-right, depth-first, children before parents
  ABC            → abstract base class; @abstractmethod forces override

C++-specific:
  virtual        → enables runtime polymorphism (dynamic dispatch)
  override       → compile-time check you're actually overriding
  final          → class/method cannot be subclassed/overridden
  = 0            → pure virtual → makes class abstract
  virtual ~Dtor  → ALWAYS do this in base classes
  this           → hidden pointer to current object
  initialiser list → preferred over body assignment in constructors
  Rule of 5      → if any of {dtor, copy ctor, copy=, move ctor, move=}, do all

SOLID in one line:
  S → one reason to change
  O → extend without modifying
  L → subclass substitutable for base
  I → don't force unused interface methods
  D → depend on abstractions, not concretions

Relationships:
  Association  → uses (independent)
  Aggregation  → has-a (weak, parts survive whole)
  Composition  → has-a (strong, parts die with whole)

Design Patterns:
  Singleton    → one global instance
  Factory      → create without specifying concrete class
  Observer     → notify subscribers on state change
  Decorator    → wrap object to add behaviour
  Strategy     → swap algorithm at runtime
  Template     → define skeleton, subclass fills steps
```

---

*Sources: GeeksforGeeks OOP interview bank (2025), InterviewBit OOP guide (2026), Edureka OOP interview questions, Interview Kickstart FAANG OOP guide, "Design Patterns" — Gang of Four (GoF), your uploaded C++ and Python notes.*

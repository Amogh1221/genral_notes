# Python — Complete Notes for Software/ML/AI Engineers
### Interview-Ready | Placement-Focused | Basics to Advanced

> **How to use this:** Read end-to-end once to build the mental model. Every section is written so the concept explanation *is* the answer to the interview question at the top of each block. Sections are ordered from foundational → advanced → system design patterns → interview Q&A.

---

## Table of Contents

**Part 1 — Python Fundamentals**
1. [Python Execution Model — How Python Actually Works](#1-python-execution-model--how-python-actually-works)
2. [Data Types & Type System](#2-data-types--type-system)
3. [Variables, Names & Memory Model](#3-variables-names--memory-model)
4. [Strings — Deep Dive](#4-strings--deep-dive)
5. [Lists, Tuples & Arrays](#5-lists-tuples--arrays)
6. [Dictionaries & Sets](#6-dictionaries--sets)
7. [Control Flow — if/for/while/comprehensions](#7-control-flow--ifforwhilecomprehensions)
8. [Functions — Arguments, Scoping, Closures](#8-functions--arguments-scoping-closures)
9. [Modules & Packages](#9-modules--packages)
10. [Exception Handling](#10-exception-handling)
11. [File I/O & Context Managers](#11-file-io--context-managers)

**Part 2 — Intermediate Python**
12. [Object-Oriented Programming (OOP)](#12-object-oriented-programming-oop)
13. [Inheritance, MRO & Polymorphism](#13-inheritance-mro--polymorphism)
14. [Magic Methods / Dunder Methods](#14-magic-methods--dunder-methods)
15. [Iterators & Generators](#15-iterators--generators)
16. [Decorators](#16-decorators)
17. [Context Managers — `with` Statement Deep Dive](#17-context-managers--with-statement-deep-dive)
18. [Comprehensions — List, Dict, Set, Generator](#18-comprehensions--list-dict-set-generator)
19. [Functional Programming — map, filter, functools](#19-functional-programming--map-filter-functools)
20. [Type Hints & Static Typing](#20-type-hints--static-typing)

**Part 3 — Advanced Python**
21. [Python Memory Management & Garbage Collection](#21-python-memory-management--garbage-collection)
22. [The GIL — Global Interpreter Lock](#22-the-gil--global-interpreter-lock)
23. [Concurrency — Threading, Multiprocessing, asyncio](#23-concurrency--threading-multiprocessing-asyncio)
24. [Metaclasses](#24-metaclasses)
25. [Descriptors & Properties](#25-descriptors--properties)
26. [Abstract Base Classes & Protocols](#26-abstract-base-classes--protocols)
27. [Dataclasses & Named Tuples](#27-dataclasses--named-tuples)
28. [Slots, `__dict__` & Memory Optimization](#28-slots-__dict__--memory-optimization)
29. [Profiling & Performance Optimization](#29-profiling--performance-optimization)
30. [Python Internals — CPython, Bytecode, Objects](#30-python-internals--cpython-bytecode-objects)

**Part 4 — Python for Data Science & ML**
31. [NumPy — The Foundation](#31-numpy--the-foundation)
32. [Pandas — Data Manipulation](#32-pandas--data-manipulation)
33. [Python in ML Pipelines](#33-python-in-ml-pipelines)

**Part 5 — Python Best Practices & Patterns**
34. [Design Patterns in Python](#34-design-patterns-in-python)
35. [Testing — unittest, pytest, mocking](#35-testing--unittest-pytest-mocking)
36. [Packaging, Virtual Environments & Dependency Management](#36-packaging-virtual-environments--dependency-management)

**Part 6 — Interview Q&A**
37. [Interview Q&A — Fundamentals](#37-interview-qa--fundamentals)
38. [Interview Q&A — OOP & Advanced](#38-interview-qa--oop--advanced)
39. [Interview Q&A — Concurrency & Performance](#39-interview-qa--concurrency--performance)
40. [Interview Q&A — Data Structures & Algorithms in Python](#40-interview-qa--data-structures--algorithms-in-python)
41. [Quick Reference Cheat Sheet](#41-quick-reference-cheat-sheet)

---

## 1. Python Execution Model — How Python Actually Works

> **Interview Q:** *What happens when you run a Python script? What is CPython?*

When you run `python script.py`, several stages happen before your code executes:

```
Source Code (.py)
       ↓
   Lexer / Tokenizer
       ↓
   Abstract Syntax Tree (AST)
       ↓
   Compiler → Bytecode (.pyc, stored in __pycache__)
       ↓
   Python Virtual Machine (PVM) — interprets bytecode
       ↓
   C runtime (CPython / OS calls)
```

**CPython** is the reference implementation of Python, written in C. When people say "Python is slow," they mean CPython's interpreted bytecode is slower than native compiled languages — but it's not slow for the vast majority of practical use cases.

Other implementations exist:
- **PyPy** — JIT-compiled Python, often 5-10× faster for long-running CPU code
- **Jython** — Python on the JVM
- **MicroPython** — Python for microcontrollers

### Bytecode

Python compiles source to bytecode — a low-level, platform-independent instruction set for the PVM. You can inspect it:

```python
import dis

def add(a, b):
    return a + b

dis.dis(add)
# LOAD_FAST 0 (a)
# LOAD_FAST 1 (b)
# BINARY_OP 0 (+)
# RETURN_VALUE
```

Bytecode is cached in `__pycache__/script.cpython-311.pyc` so it doesn't recompile on every run.

### The Python Data Model

Everything in Python is an **object** — integers, functions, classes, modules, even `None`. Every object has:
- **Identity** (`id(obj)`) — memory address in CPython
- **Type** (`type(obj)`) — the class it belongs to
- **Value** — the actual data

```python
x = 42
print(id(x))       # e.g., 140234567890
print(type(x))     # <class 'int'>
print(isinstance(x, int))  # True
```

---

## 2. Data Types & Type System

> **Interview Q:** *Is Python strongly typed or weakly typed? What's the difference between dynamic and static typing?*

Python is **dynamically typed** (types are checked at runtime, not compile time) and **strongly typed** (implicit type coercion is rare — you can't add a string to an int without explicit conversion).

```python
# Dynamic typing — variable can hold any type
x = 42
x = "hello"   # perfectly valid

# Strong typing — no implicit coercion
"3" + 3       # TypeError! (unlike JavaScript where this gives "33")
int("3") + 3  # 6 — explicit conversion required
```

### Built-in Types Overview

| Category | Types |
|---|---|
| **Numeric** | `int`, `float`, `complex`, `bool` |
| **Sequence** | `str`, `list`, `tuple`, `range`, `bytes`, `bytearray` |
| **Mapping** | `dict` |
| **Set** | `set`, `frozenset` |
| **Boolean** | `bool` (subclass of `int`) |
| **None** | `NoneType` |
| **Callable** | `function`, `method`, `lambda`, `class` |

### Mutability — Critical Concept

| Mutable (can change in place) | Immutable (cannot change) |
|---|---|
| `list`, `dict`, `set`, `bytearray` | `int`, `float`, `str`, `tuple`, `frozenset`, `bytes` |

```python
# Mutable
lst = [1, 2, 3]
lst[0] = 99       # OK — list changed in place

# Immutable
s = "hello"
s[0] = "H"        # TypeError! strings are immutable
s = "Hello"       # This creates a NEW string object
```

**Why does mutability matter?**
1. Mutable objects used as default arguments are a classic bug
2. Only immutable objects can be dictionary keys or set members
3. Immutable objects are thread-safe by default

### Integer Interning

CPython interns (caches) small integers (-5 to 256) and single-character strings. This is a CPython implementation detail, not a language guarantee:

```python
a = 256
b = 256
a is b   # True — same cached object

a = 257
b = 257
a is b   # False — new objects created (CPython-specific)
```

**Always use `==` to compare values, `is` only for identity/None checks.**

---

## 3. Variables, Names & Memory Model

> **Interview Q:** *Are Python variables containers or labels? What is the difference between `is` and `==`?*

Python variables are **names (labels/references)** bound to objects — they don't contain values directly. Multiple names can point to the same object.

```python
a = [1, 2, 3]
b = a          # b is ANOTHER NAME for the same list object
b.append(4)
print(a)       # [1, 2, 3, 4] — a sees the change!

b = a[:]       # NOW b is a shallow copy — different object
b.append(5)
print(a)       # [1, 2, 3, 4] — a is unaffected
```

### Shallow vs Deep Copy

```python
import copy

original = [[1, 2], [3, 4]]

shallow = original.copy()    # or original[:]
shallow[0].append(99)
print(original)  # [[1, 2, 99], [3, 4]] — inner lists are shared!

deep = copy.deepcopy(original)
deep[0].append(99)
print(original)  # [[1, 2, 99], [3, 4]] — unaffected
```

**Shallow copy:** New outer container, but inner objects still shared.
**Deep copy:** Recursively copies all nested objects.

### Scope — LEGB Rule

Python resolves names following the **LEGB** rule:
1. **L**ocal — inside the current function
2. **E**nclosing — in enclosing function scopes (for closures)
3. **G**lobal — module-level scope
4. **B**uilt-in — Python's built-in names (`len`, `print`, etc.)

```python
x = "global"

def outer():
    x = "enclosing"
    def inner():
        # x = "local"  # if this existed, it would be Local
        print(x)       # finds "enclosing" via E in LEGB
    inner()

outer()  # prints "enclosing"
```

### `global` and `nonlocal`

```python
count = 0

def increment():
    global count    # modify the global variable
    count += 1

def make_counter():
    count = 0
    def inc():
        nonlocal count   # modify enclosing scope variable
        count += 1
        return count
    return inc
```

---

## 4. Strings — Deep Dive

> **Interview Q:** *Why are Python strings immutable? How does string interning work?*

Strings are immutable sequences of Unicode code points. Immutability enables:
- Safe use as dictionary keys
- Memory sharing via interning
- Thread safety

### String Methods (must-know)

```python
s = "  Hello, World!  "

s.strip()               # "Hello, World!" — removes leading/trailing whitespace
s.lower() / s.upper()   # case conversion
s.split(",")            # ["  Hello", " World!  "]
s.replace("World", "Python")
s.startswith("  He")    # True
s.find("World")         # 9 (index) or -1 if not found
s.count("l")            # 3
s.join(["a","b","c"])   # NOT what you want — use ",".join(["a","b","c"]) → "a,b,c"

# Formatting
name, score = "Alice", 95.5
f"Name: {name}, Score: {score:.2f}"   # f-string (fastest, modern)
"Name: {}, Score: {:.2f}".format(name, score)
"Name: %s, Score: %.2f" % (name, score)  # old-style
```

### String Concatenation Performance

```python
# BAD — O(n²) because each + creates a new string
result = ""
for word in words:
    result += word

# GOOD — O(n) using join
result = "".join(words)
```

### Multiline Strings & Raw Strings

```python
multi = """
Line 1
Line 2
Line 3
"""

path = r"C:\Users\name\file.txt"   # raw string — backslashes not escaped
regex = r"\d+\.\d+"                # raw strings are ideal for regex
```

### Unicode & Encoding

```python
s = "café"
len(s)              # 4 — Python 3 strings are Unicode (code points)
s.encode("utf-8")   # b'caf\xc3\xa9' — bytes object (5 bytes)
b'caf\xc3\xa9'.decode("utf-8")  # "café"
```

---

## 5. Lists, Tuples & Arrays

> **Interview Q:** *When would you use a tuple instead of a list? What is the time complexity of list operations?*

### Lists

Dynamic arrays backed by a C array of pointers. Amortized O(1) append due to over-allocation (grows by ~1.125×).

```python
lst = [1, 2, 3, 4, 5]

# Slicing — O(k) where k is slice size
lst[1:4]        # [2, 3, 4]
lst[::2]        # [1, 3, 5] — every other element
lst[::-1]       # [5, 4, 3, 2, 1] — reversed

# Common operations
lst.append(6)     # O(1) amortized
lst.insert(0, 0)  # O(n) — shifts all elements
lst.pop()         # O(1) — removes last
lst.pop(0)        # O(n) — removes first, shifts all
lst.remove(3)     # O(n) — finds and removes first occurrence
lst.index(4)      # O(n) — finds index of first occurrence
lst.sort()        # O(n log n) — in-place Timsort
sorted(lst)       # O(n log n) — returns new sorted list
```

### List Time Complexities

| Operation | Time |
|---|---|
| `append(x)` | O(1) amortized |
| `pop()` | O(1) |
| `insert(i, x)` | O(n) |
| `pop(i)` | O(n) |
| `x in lst` | O(n) |
| `lst[i]` (index access) | O(1) |
| `len(lst)` | O(1) |
| `lst.sort()` | O(n log n) |

### Tuples

Immutable sequences. More memory-efficient than lists. Used for:
- Heterogeneous data (records): `person = ("Alice", 30, "Engineer")`
- Dictionary keys: `coords = {(0, 0): "origin"}`
- Multiple return values from functions
- Named tuples (see section 27)

```python
t = (1, 2, 3)
a, b, c = t           # unpacking
first, *rest = t      # extended unpacking: first=1, rest=[2, 3]
x, y = y, x          # swap — uses tuple under the hood

# Single-element tuple MUST have trailing comma
single = (42,)        # tuple
not_a_tuple = (42)    # just 42 — the parentheses are grouping
```

### `collections.deque` — When Lists Fail

For queues (O(1) left append/pop), use `deque` instead of list:

```python
from collections import deque

dq = deque([1, 2, 3])
dq.appendleft(0)   # O(1) — list.insert(0, x) is O(n)
dq.popleft()       # O(1) — list.pop(0) is O(n)
dq.rotate(1)       # rotate right by 1
```

---

## 6. Dictionaries & Sets

> **Interview Q:** *How is a Python dict implemented? What is the average time complexity of dict operations?*

### Dictionaries — Hash Tables

Since Python 3.7+, dicts maintain **insertion order** (guaranteed). Implemented as open-addressing hash tables.

```python
d = {"a": 1, "b": 2, "c": 3}

d["d"] = 4            # insert/update — O(1) average
d.get("e", 0)         # O(1) — returns 0 if key missing (no KeyError)
d.pop("a")            # O(1) — removes and returns
"b" in d              # O(1) — membership test
d.keys()              # dict_keys view
d.values()            # dict_values view
d.items()             # dict_items view — for k, v in d.items()

# Merging dicts (Python 3.9+)
merged = d1 | d2      # new merged dict
d1 |= d2              # in-place merge

# dict comprehension
squares = {x: x**2 for x in range(5)}  # {0:0, 1:1, 2:4, 3:9, 4:16}
```

### `collections.defaultdict` & `Counter`

```python
from collections import defaultdict, Counter

# defaultdict — no KeyError on missing key
dd = defaultdict(list)
dd["key"].append(1)   # creates empty list automatically

dd = defaultdict(int)
for word in words:
    dd[word] += 1     # word count without checking if key exists

# Counter — frequency count
c = Counter("abracadabra")
# Counter({'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
c.most_common(2)      # [('a', 5), ('b', 2)]
c1 + c2               # combine counters
```

### Sets

Hash sets — unordered, no duplicates, O(1) average membership test.

```python
s = {1, 2, 3, 4}
s.add(5)              # O(1)
s.remove(3)           # O(1) — raises KeyError if missing
s.discard(99)         # O(1) — no error if missing
3 in s                # O(1)

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
a | b    # union: {1,2,3,4,5,6}
a & b    # intersection: {3,4}
a - b    # difference: {1,2}
a ^ b    # symmetric difference: {1,2,5,6}
a <= b   # is a subset of b? False
```

**Key insight:** Use a `set` instead of `list` when you need fast membership checks.

```python
# O(n) per lookup
blacklist = ["spam@evil.com", "bad@evil.com", ...]
if email in blacklist:  # O(n) each time

# O(1) per lookup
blacklist = {"spam@evil.com", "bad@evil.com", ...}
if email in blacklist:  # O(1) each time
```

---

## 7. Control Flow — if/for/while/comprehensions

> **Interview Q:** *How does Python's `for` loop work under the hood? What is the `else` clause on loops?*

### `for` Loop — Iterator Protocol

Python's `for` loop calls `iter()` on the iterable to get an iterator, then repeatedly calls `next()` until `StopIteration` is raised.

```python
for x in [1, 2, 3]:
    print(x)

# Is exactly equivalent to:
it = iter([1, 2, 3])
while True:
    try:
        x = next(it)
        print(x)
    except StopIteration:
        break
```

### `else` Clause on Loops

The `else` block runs **only if the loop completes without a `break`**. Useful for "search and not found" patterns.

```python
for item in collection:
    if item == target:
        print("Found!")
        break
else:
    print("Not found")   # runs only if break was never hit

# Same for while:
while condition:
    ...
else:
    ...  # runs when condition becomes False (not on break)
```

### Useful Built-ins for Iteration

```python
enumerate(lst)                 # (0, item), (1, item), ...
zip(a, b)                      # (a[0], b[0]), (a[1], b[1]), ...
zip(a, b, strict=True)        # Python 3.10+ — raises if lengths differ
reversed(lst)                  # iterate in reverse
sorted(lst, key=fn, reverse=True)
map(fn, lst)                   # lazy — apply fn to each element
filter(fn, lst)                # lazy — keep elements where fn returns True
any(x > 0 for x in lst)       # True if any element satisfies condition
all(x > 0 for x in lst)       # True if all elements satisfy condition
```

### Walrus Operator `:=` (Python 3.8+)

Assigns and returns a value in an expression:

```python
# Without walrus
data = read_data()
while data:
    process(data)
    data = read_data()

# With walrus
while data := read_data():
    process(data)

# Useful in comprehensions
values = [y for x in data if (y := expensive(x)) > 0]
```

---

## 8. Functions — Arguments, Scoping, Closures

> **Interview Q:** *What is the mutable default argument bug? What are `*args` and `**kwargs`?*

### The Mutable Default Argument Bug

Default arguments are evaluated **once** at function definition time, not at each call:

```python
# BUG — all calls share the same list object
def append_to(item, lst=[]):
    lst.append(item)
    return lst

append_to(1)   # [1]
append_to(2)   # [1, 2]  ← not [2]!
append_to(3)   # [1, 2, 3]

# FIX — use None as sentinel
def append_to(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

### `*args` and `**kwargs`

```python
def func(*args, **kwargs):
    # args is a tuple of positional arguments
    # kwargs is a dict of keyword arguments
    print(args)    # (1, 2, 3)
    print(kwargs)  # {'name': 'Alice', 'age': 30}

func(1, 2, 3, name="Alice", age=30)

# Unpacking when calling
lst = [1, 2, 3]
dct = {"x": 10, "y": 20}
func(*lst, **dct)   # passes positional and keyword args from containers
```

### Keyword-Only & Positional-Only Arguments (Python 3.8+)

```python
def func(pos_only, /, normal, *, kw_only):
    pass

# pos_only — must be positional (before /)
# normal — can be either
# kw_only — must be keyword (after *)

func(1, 2, kw_only=3)          # OK
func(pos_only=1, normal=2, kw_only=3)  # TypeError for pos_only
```

### Closures

A closure is a function that **remembers** the variables from its enclosing scope even after that scope has finished executing:

```python
def make_multiplier(n):
    def multiplier(x):
        return x * n   # n is "closed over" from enclosing scope
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)
double(5)   # 10
triple(5)   # 15

# Inspecting a closure
double.__closure__[0].cell_contents  # 2
```

**Common closure gotcha with loops:**

```python
# BUG — all functions share the same i variable (loop variable)
fns = [lambda: i for i in range(3)]
[f() for f in fns]  # [2, 2, 2] — not [0, 1, 2]!

# FIX — capture i at definition time using default arg
fns = [lambda i=i: i for i in range(3)]
[f() for f in fns]  # [0, 1, 2]
```

---

## 9. Modules & Packages

> **Interview Q:** *What is the difference between a module and a package? How does Python's import system work?*

A **module** is any `.py` file. A **package** is a directory containing an `__init__.py` file (makes it a Python package).

```
mypackage/
    __init__.py
    module_a.py
    subpackage/
        __init__.py
        module_b.py
```

### Import Mechanics

When you `import foo`:
1. Python checks `sys.modules` cache (already imported? return cached)
2. Searches `sys.path` in order: current dir, PYTHONPATH, stdlib, site-packages
3. Loads and executes the module file
4. Caches in `sys.modules`

```python
import os                      # imports the module
from os import path            # imports a name from the module
from os.path import join, exists  # imports specific names
import os as operating_system  # alias

# Relative imports (inside packages only)
from . import sibling_module   # same package
from .. import parent_module   # parent package
```

### `__name__ == "__main__"`

```python
# This block runs only when the file is executed directly,
# NOT when it's imported as a module
if __name__ == "__main__":
    main()
```

---

## 10. Exception Handling

> **Interview Q:** *What is the difference between `Exception` and `BaseException`? When should you use bare `except:`?*

### Exception Hierarchy

```
BaseException
├── SystemExit          ← raised by sys.exit()
├── KeyboardInterrupt   ← Ctrl+C
├── GeneratorExit       ← generator.close()
└── Exception           ← all "normal" exceptions
    ├── ValueError
    ├── TypeError
    ├── KeyError
    ├── IndexError
    ├── AttributeError
    ├── RuntimeError
    ├── OSError (IOError, FileNotFoundError, PermissionError)
    ├── StopIteration
    └── ... (many more)
```

Never use bare `except:` — it also catches `SystemExit` and `KeyboardInterrupt`. Use `except Exception:` at minimum.

### Full Try/Except/Else/Finally

```python
try:
    result = risky_operation()
except ValueError as e:
    print(f"Value error: {e}")
except (TypeError, KeyError) as e:
    print(f"Type or Key error: {e}")
except Exception as e:
    # catch-all for unexpected errors
    logger.exception("Unexpected error")
    raise   # re-raise the same exception
else:
    # runs ONLY if NO exception was raised
    print("Success:", result)
finally:
    # ALWAYS runs — cleanup here (closing files, releasing locks)
    cleanup()
```

### Raising Exceptions

```python
raise ValueError("Invalid input")                     # raise new
raise ValueError("Wrapper") from original_exception   # exception chaining
raise   # re-raise current exception (inside except block)
```

### Custom Exceptions

```python
class AppError(Exception):
    """Base class for application errors."""
    pass

class ValidationError(AppError):
    def __init__(self, field, message):
        self.field = field
        super().__init__(f"Validation error on '{field}': {message}")

try:
    raise ValidationError("email", "Invalid format")
except ValidationError as e:
    print(e.field)   # "email"
```

---

## 11. File I/O & Context Managers

> **Interview Q:** *Why should you always use `with` for file operations?*

```python
# BAD — file may not be closed if exception occurs
f = open("file.txt", "r")
data = f.read()
f.close()

# GOOD — with guarantees __exit__ is called (closes file) even on exception
with open("file.txt", "r", encoding="utf-8") as f:
    data = f.read()          # read entire file
    # or
    lines = f.readlines()    # list of lines
    # or
    for line in f:           # memory-efficient line-by-line
        process(line)

# Writing
with open("out.txt", "w", encoding="utf-8") as f:
    f.write("Hello\n")
    f.writelines(["line1\n", "line2\n"])

# Appending
with open("log.txt", "a") as f:
    f.write("new log entry\n")
```

### File Modes

| Mode | Description |
|---|---|
| `r` | Read (default) |
| `w` | Write (truncates existing) |
| `a` | Append |
| `x` | Exclusive create (fails if exists) |
| `b` | Binary mode (add to above, e.g. `rb`) |
| `+` | Read and write (e.g. `r+`) |

### `pathlib` — Modern File Paths

```python
from pathlib import Path

p = Path("/home/user/data")
p / "file.txt"              # path joining — cross-platform
p.exists()
p.is_file() / p.is_dir()
p.suffix                    # ".txt"
p.stem                      # "file"
p.name                      # "file.txt"
p.parent                    # Path("/home/user/data")
list(p.glob("*.csv"))       # find all CSV files
p.read_text(encoding="utf-8")  # shorthand for open+read
p.write_text("content")
```

---

## 12. Object-Oriented Programming (OOP)

> **Interview Q:** *Explain the four pillars of OOP and how Python implements them.*

### Encapsulation

Bundling data and methods. Python uses **naming conventions** for access control (no true private):

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner          # public
        self._balance = balance     # protected (convention: don't touch)
        self.__pin = "1234"        # name-mangled to _BankAccount__pin

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount

    @property
    def balance(self):
        return self._balance       # controlled read access
```

### `__init__` vs `__new__`

`__new__` creates the instance (allocates memory). `__init__` initializes it. You almost never override `__new__` unless implementing singletons or immutable types.

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        print("Creating instance")
        instance = super().__new__(cls)
        return instance

    def __init__(self, value):
        print("Initializing instance")
        self.value = value
```

### Class vs Instance vs Static Methods

```python
class Dog:
    species = "Canis lupus familiaris"   # class variable

    def __init__(self, name):
        self.name = name                  # instance variable

    def bark(self):                       # instance method — gets self
        return f"{self.name} says woof!"

    @classmethod
    def from_string(cls, name_str):       # class method — gets cls, not self
        name = name_str.strip().title()
        return cls(name)                  # creates a new instance

    @staticmethod
    def is_domestic():                    # static method — no self or cls
        return True

# Usage
rex = Dog("Rex")
rex.bark()                    # "Rex says woof!"
Dog.from_string("buddy ")     # Dog("Buddy")
Dog.is_domestic()             # True
```

**When to use:**
- `@classmethod` — alternative constructors, factory methods
- `@staticmethod` — utility functions logically related to the class but not needing instance/class state

---

## 13. Inheritance, MRO & Polymorphism

> **Interview Q:** *What is Python's Method Resolution Order (MRO)? How does `super()` work with multiple inheritance?*

### Single Inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError

class Dog(Animal):
    def speak(self):
        return f"{self.name} says woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says meow!"

# Polymorphism — same interface, different behavior
animals = [Dog("Rex"), Cat("Whiskers")]
for a in animals:
    print(a.speak())   # works for any Animal subclass
```

### Multiple Inheritance & MRO

Python uses the **C3 Linearization algorithm** to determine MRO — the order in which classes are searched for a method.

```python
class A:
    def method(self): return "A"

class B(A):
    def method(self): return "B"

class C(A):
    def method(self): return "C"

class D(B, C):
    pass

D.__mro__  # (D, B, C, A, object)
D().method()  # "B" — follows MRO
```

**MRO rule of thumb:** Left-to-right, depth-first, with each class appearing only once and children before parents.

### `super()`

`super()` follows MRO, not just the direct parent:

```python
class Base:
    def __init__(self):
        print("Base.__init__")

class A(Base):
    def __init__(self):
        super().__init__()    # calls Base.__init__ (follows MRO)
        print("A.__init__")

class B(Base):
    def __init__(self):
        super().__init__()
        print("B.__init__")

class C(A, B):
    def __init__(self):
        super().__init__()    # follows C→A→B→Base MRO!
        print("C.__init__")

C()
# Output:
# Base.__init__   ← called only once despite diamond inheritance
# B.__init__
# A.__init__
# C.__init__
```

`super()` solves the **diamond problem** — ensures each class is initialized exactly once in cooperative multiple inheritance.

---

## 14. Magic Methods / Dunder Methods

> **Interview Q:** *What are dunder methods? How would you make a custom class sortable or usable as a dict key?*

Dunder (double underscore) methods let your objects integrate with Python's syntax and built-ins.

### Essential Dunders

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    # String representations
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"   # for developers — eval-able

    def __str__(self):
        return f"({self.x}, {self.y})"         # for users — print()

    # Arithmetic
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)

    def __rmul__(self, scalar):   # for scalar * vector (reversed)
        return self.__mul__(scalar)

    # Comparison — needed for sorting
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __lt__(self, other):
        return abs(self) < abs(other)

    # Length/magnitude
    def __abs__(self):
        return (self.x**2 + self.y**2)**0.5

    def __len__(self):
        return 2   # number of dimensions

    # Make it iterable
    def __iter__(self):
        yield self.x
        yield self.y

    # Make it hashable (needed for dict key / set member)
    # NOTE: if you define __eq__, Python removes __hash__ unless you explicitly define it
    def __hash__(self):
        return hash((self.x, self.y))

    # Container protocol
    def __getitem__(self, index):
        return (self.x, self.y)[index]

    # Context manager
    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        return False   # don't suppress exceptions
```

### Container Dunders

```python
class MyList:
    def __len__(self):        # len(obj)
    def __getitem__(self, i): # obj[i]
    def __setitem__(self, i, v): # obj[i] = v
    def __delitem__(self, i): # del obj[i]
    def __contains__(self, x): # x in obj
    def __iter__(self):        # for x in obj
```

### Callable Objects

```python
class Multiplier:
    def __init__(self, factor):
        self.factor = factor

    def __call__(self, x):    # makes instance callable like a function
        return x * self.factor

triple = Multiplier(3)
triple(10)   # 30
```

---

## 15. Iterators & Generators

> **Interview Q:** *What is the difference between an iterable and an iterator? When would you use a generator?*

- **Iterable:** Object with `__iter__` method (returns an iterator). Examples: list, tuple, str, dict.
- **Iterator:** Object with both `__iter__` (returns self) and `__next__` (returns next value or raises `StopIteration`). Iterators are "one-shot" — they're exhausted once consumed.

```python
# Implementing an iterator manually
class CountUp:
    def __init__(self, limit):
        self.limit = limit
        self.current = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.limit:
            raise StopIteration
        self.current += 1
        return self.current

list(CountUp(3))   # [1, 2, 3]
```

### Generators — The Pythonic Way

A function with `yield` automatically becomes a generator — implements the iterator protocol with much less boilerplate:

```python
def count_up(limit):
    current = 0
    while current < limit:
        current += 1
        yield current   # suspends execution, returns value

gen = count_up(3)
next(gen)   # 1
next(gen)   # 2
next(gen)   # 3
next(gen)   # StopIteration
```

**Why generators?**
- **Lazy evaluation** — values computed on demand, not all at once
- **Memory efficient** — infinite sequences, large files

```python
# Reading a 10GB file — lists would blow up memory
def read_large_file(path):
    with open(path) as f:
        for line in f:
            yield line.strip()

for line in read_large_file("huge.log"):
    process(line)   # only one line in memory at a time

# Infinite sequence
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Generator expressions (like list comps but lazy)
gen_expr = (x**2 for x in range(1000000))  # no memory cost until iterated
sum_of_squares = sum(x**2 for x in range(1000000))  # efficient

# `yield from` — delegate to another generator
def chain(*iterables):
    for it in iterables:
        yield from it
```

### `send()` and Two-Way Generators

```python
def accumulator():
    total = 0
    while True:
        value = yield total   # yield sends total out; receives value in
        if value is None:
            break
        total += value

acc = accumulator()
next(acc)          # 0 — prime the generator
acc.send(10)       # 10
acc.send(20)       # 30
```

---

## 16. Decorators

> **Interview Q:** *What is a decorator? How would you write one that preserves the wrapped function's metadata?*

A decorator is a function that takes a function and returns a modified function — implemented using closures.

```python
import functools
import time

def timer(func):
    @functools.wraps(func)   # preserves __name__, __doc__, etc.
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        end = time.perf_counter()
        print(f"{func.__name__} took {end - start:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)

slow_function()   # "slow_function took 1.0001s"
# @timer is syntactic sugar for: slow_function = timer(slow_function)
```

### Decorator with Arguments

```python
def retry(times=3, exceptions=(Exception,)):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(times):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    if attempt == times - 1:
                        raise
                    print(f"Attempt {attempt+1} failed: {e}. Retrying...")
        return wrapper
    return decorator

@retry(times=3, exceptions=(ConnectionError,))
def fetch_data(url):
    ...
```

### Class-Based Decorators

```python
class memoize:
    def __init__(self, func):
        self.func = func
        self.cache = {}
        functools.update_wrapper(self, func)

    def __call__(self, *args):
        if args not in self.cache:
            self.cache[args] = self.func(*args)
        return self.cache[args]

@memoize
def fib(n):
    return n if n < 2 else fib(n-1) + fib(n-2)
```

### Stacking Decorators

```python
@decorator_a
@decorator_b
def func():
    pass

# Equivalent to: func = decorator_a(decorator_b(func))
# decorator_b is applied first (innermost), then decorator_a
```

### Built-in Decorators

```python
@property           # computed attribute (getter)
@setter             # setter for property
@classmethod        # class method
@staticmethod       # static method
@functools.lru_cache(maxsize=128)   # memoization with LRU eviction
@functools.cached_property          # computed once, cached as instance attr
@abstractmethod     # from abc — must be overridden in subclass
@dataclass          # auto-generate __init__, __repr__, __eq__
```

---

## 17. Context Managers — `with` Statement Deep Dive

> **Interview Q:** *How do you implement a custom context manager? What is `contextlib.contextmanager`?*

### Class-Based Context Manager

```python
class DatabaseConnection:
    def __init__(self, url):
        self.url = url
        self.conn = None

    def __enter__(self):
        self.conn = connect(self.url)
        return self.conn         # value bound to `as` variable

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.conn.close()
        return False   # return True to suppress exceptions

with DatabaseConnection("db://...") as conn:
    conn.execute("SELECT 1")
# conn.close() called even if exception raised inside
```

### Generator-Based with `contextlib`

```python
from contextlib import contextmanager, suppress

@contextmanager
def managed_resource():
    resource = acquire()   # setup
    try:
        yield resource     # body of `with` block runs here
    finally:
        release(resource)  # teardown (always runs)

# suppress — silently ignore specific exceptions
with suppress(FileNotFoundError):
    os.remove("file_that_may_not_exist.txt")
```

---

## 18. Comprehensions — List, Dict, Set, Generator

> **Interview Q:** *When should you NOT use a comprehension?*

Comprehensions are concise and often faster than equivalent `for` loops (optimized internally). But avoid when:
- Logic is complex enough to need comments
- Side effects are needed (use a `for` loop)
- Expression exceeds ~80 characters

```python
# List comprehension
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
flat = [item for sublist in matrix for item in sublist]   # nested

# Dict comprehension
word_lengths = {word: len(word) for word in words}
inverted = {v: k for k, v in original.items()}

# Set comprehension
unique_lengths = {len(word) for word in words}

# Generator expression (lazy — doesn't create a list)
total = sum(x**2 for x in range(1_000_000))

# Nested comprehension — matrix transpose
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transposed = [[row[i] for row in matrix] for i in range(3)]
```

---

## 19. Functional Programming — map, filter, functools

> **Interview Q:** *What is `functools.partial`? When would you use it?*

```python
from functools import partial, reduce

# map and filter — prefer comprehensions for clarity
doubled = list(map(lambda x: x*2, [1,2,3]))    # [2,4,6]
evens = list(filter(lambda x: x%2==0, [1,2,3,4]))  # [2,4]

# reduce — fold a sequence
product = reduce(lambda acc, x: acc * x, [1,2,3,4])   # 24

# partial — pre-fill arguments
def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
cube = partial(power, exp=3)
square(5)   # 25
cube(3)     # 27

# functools.lru_cache
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)

fib.cache_info()   # CacheInfo(hits=..., misses=..., maxsize=None, currsize=...)
fib.cache_clear()  # clear the cache
```

---

## 20. Type Hints & Static Typing

> **Interview Q:** *What are type hints? Does Python enforce them at runtime?*

Type hints are **annotations** — Python does NOT enforce them at runtime by default. They're used by static analyzers (`mypy`, `pyright`), IDEs, and documentation.

```python
from typing import Optional, Union, List, Dict, Tuple, Any, Callable
from typing import TypeVar, Generic
from collections.abc import Sequence, Iterable

def greet(name: str) -> str:
    return f"Hello, {name}"

def process(
    data: list[int],                     # Python 3.9+: built-in generics
    callback: Callable[[int], str],      # function type
    config: dict[str, Any] | None = None # Python 3.10+: | for Union
) -> tuple[int, str]:
    ...

# Optional is shorthand for Union[X, None]
def find(lst: list[int], val: int) -> Optional[int]:
    try:
        return lst.index(val)
    except ValueError:
        return None

# TypeVar for generic functions
T = TypeVar("T")

def first(lst: list[T]) -> T:
    return lst[0]

# Dataclass with types
from dataclasses import dataclass

@dataclass
class Point:
    x: float
    y: float
    label: str = "origin"
```

---

## 21. Python Memory Management & Garbage Collection

> **Interview Q:** *How does Python manage memory? What is reference counting?*

### Reference Counting

Every Python object has a **reference count**. When it reaches 0, CPython immediately deallocates it.

```python
import sys

x = [1, 2, 3]
sys.getrefcount(x)   # at least 2 (x + the argument to getrefcount)

y = x                 # ref count: 3
del x                 # ref count: 2
del y                 # ref count: 0 → object deallocated
```

### Cyclic Garbage Collector

Reference counting can't handle **reference cycles**:

```python
a = []
b = []
a.append(b)   # a → b
b.append(a)   # b → a — cycle!
del a
del b
# Both objects still have refcount > 0 due to cycle!
# Python's cyclic GC periodically detects and collects these
```

Python's cyclic GC runs in "generations" (0, 1, 2) — objects that survive GC cycles are promoted. You can control it with `gc` module:

```python
import gc
gc.collect()         # force a full GC cycle
gc.disable()         # disable GC (use carefully)
gc.get_threshold()   # (700, 10, 10) — thresholds for each generation
```

### Memory Profiling

```python
# tracemalloc — built-in memory profiler
import tracemalloc

tracemalloc.start()
# ... your code ...
snapshot = tracemalloc.take_snapshot()
top_stats = snapshot.statistics("lineno")
for stat in top_stats[:10]:
    print(stat)
```

---

## 22. The GIL — Global Interpreter Lock

> **Interview Q:** *What is the GIL and how does it affect multithreaded Python programs?*

The **Global Interpreter Lock (GIL)** is a mutex in CPython that allows only one thread to execute Python bytecode at a time, even on multi-core CPUs.

**Why does it exist?** CPython's memory management (reference counting) is not thread-safe. The GIL is the simplest way to prevent race conditions on object reference counts.

```
Thread 1: [===GIL===] ... [release] ...
Thread 2: ...        [===GIL===] ...
Thread 3: ...              ...        [===GIL===]

Only one thread runs at a time!
```

### Impact

| Workload | Threading | Multiprocessing |
|---|---|---|
| **I/O-bound** (network, disk) | ✅ Threads work well — GIL released during I/O | ✅ Also works |
| **CPU-bound** (computation) | ❌ Threads actually SLOWER due to GIL contention | ✅ Each process has its own GIL |

**The GIL IS released during:**
- I/O operations (file read/write, socket operations)
- C extensions that explicitly release it (NumPy, etc.)
- `time.sleep()`

**Python 3.13 "No-GIL" / Free-Threaded CPython:** Experimental optional removal of the GIL as of Python 3.13. Enable with `python -X gil=0`. Still experimental but major progress.

---

## 23. Concurrency — Threading, Multiprocessing, asyncio

> **Interview Q:** *When would you use threading vs multiprocessing vs asyncio?*

```
Decision tree:
Is the bottleneck I/O? ──Yes──► asyncio (if you can use async/await)
                       └──────► threading (if code is synchronous)
Is the bottleneck CPU? ──Yes──► multiprocessing
```

### Threading

```python
import threading

def worker(n):
    print(f"Thread {n} running")

threads = [threading.Thread(target=worker, args=(i,)) for i in range(5)]
for t in threads:
    t.start()
for t in threads:
    t.join()   # wait for completion

# Thread-safe data sharing
lock = threading.Lock()
count = 0

def increment():
    global count
    with lock:    # acquire/release lock
        count += 1

# Thread-safe queue
from queue import Queue
q = Queue()
q.put(item)
item = q.get()   # blocks until item available
```

### Multiprocessing

```python
from multiprocessing import Pool, Process, Queue

def cpu_task(x):
    return sum(i*i for i in range(x))

# Pool — easiest for parallel map
with Pool(processes=4) as pool:
    results = pool.map(cpu_task, [10000, 20000, 30000, 40000])

# ProcessPoolExecutor — modern interface
from concurrent.futures import ProcessPoolExecutor

with ProcessPoolExecutor(max_workers=4) as executor:
    futures = [executor.submit(cpu_task, n) for n in range(10)]
    results = [f.result() for f in futures]
```

### asyncio — Cooperative Multitasking

```python
import asyncio
import aiohttp

async def fetch(session, url):
    async with session.get(url) as resp:
        return await resp.text()

async def main():
    urls = ["https://example.com", "https://python.org"]
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url) for url in urls]
        results = await asyncio.gather(*tasks)   # run concurrently
    return results

asyncio.run(main())

# Key keywords
# async def — defines a coroutine
# await     — suspend execution, hand control back to event loop
# asyncio.gather() — run multiple coroutines concurrently
# asyncio.create_task() — schedule a coroutine without awaiting immediately
```

### `concurrent.futures` — Unified Interface

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor, as_completed

# For I/O-bound
with ThreadPoolExecutor(max_workers=10) as executor:
    future_to_url = {executor.submit(fetch, url): url for url in urls}
    for future in as_completed(future_to_url):
        url = future_to_url[future]
        try:
            data = future.result()
        except Exception as e:
            print(f"{url} raised {e}")
```

---

## 24. Metaclasses

> **Interview Q:** *What is a metaclass? When would you use one?*

A metaclass is the "class of a class" — it controls how classes are created. `type` is the default metaclass for all classes.

```python
# type(name, bases, attrs) creates a class dynamically
Dog = type("Dog", (object,), {
    "species": "Canis lupus",
    "bark": lambda self: "Woof!"
})
```

### Custom Metaclass

```python
class SingletonMeta(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    def __init__(self):
        self.connection = create_connection()

db1 = Database()
db2 = Database()
db1 is db2   # True — same instance

# Metaclass lifecycle
class Meta(type):
    def __new__(mcs, name, bases, namespace):
        # called when the class body is executed
        cls = super().__new__(mcs, name, bases, namespace)
        return cls

    def __init__(cls, name, bases, namespace):
        # called after __new__
        super().__init__(name, bases, namespace)
```

**When to use metaclasses:** ORM frameworks (Django models), API framework validation, automatic class registration, enforcing interface contracts. In practice: "if you're asking whether you need a metaclass, you probably don't."

---

## 25. Descriptors & Properties

> **Interview Q:** *How does `@property` work internally? What is a descriptor?*

A **descriptor** is any object that defines `__get__`, `__set__`, or `__delete__`. They implement attribute access.

```python
class Validator:
    """A descriptor that validates numeric ranges."""
    def __set_name__(self, owner, name):
        self.name = name
        self.private_name = f"_{name}"

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self   # accessed on class, not instance
        return getattr(obj, self.private_name, None)

    def __set__(self, obj, value):
        if not isinstance(value, (int, float)):
            raise TypeError(f"{self.name} must be numeric")
        setattr(obj, self.private_name, value)

class Circle:
    radius = Validator()

    def __init__(self, radius):
        self.radius = radius   # triggers Validator.__set__

# @property is a built-in descriptor
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):              # getter
        return self._celsius

    @celsius.setter
    def celsius(self, value):       # setter
        if value < -273.15:
            raise ValueError("Below absolute zero")
        self._celsius = value

    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32
```

---

## 26. Abstract Base Classes & Protocols

> **Interview Q:** *What is the difference between ABC and Protocol (structural subtyping)?*

### Abstract Base Classes (ABC)

Nominal subtyping — class must explicitly inherit from ABC:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> float:
        ...

    @abstractmethod
    def perimeter(self) -> float:
        ...

    def describe(self):
        return f"Area: {self.area()}, Perimeter: {self.perimeter()}"

class Circle(Shape):
    def __init__(self, r): self.r = r
    def area(self): return 3.14 * self.r ** 2
    def perimeter(self): return 2 * 3.14 * self.r

# Shape()  # TypeError — can't instantiate abstract class
```

### Protocols (Python 3.8+)

Structural subtyping ("duck typing" + static checking) — no inheritance required:

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...

class Circle:             # does NOT inherit Drawable
    def draw(self):
        print("Drawing circle")

def render(shape: Drawable) -> None:
    shape.draw()

render(Circle())   # works — Circle structurally satisfies Drawable
```

**ABC** = explicit contract, inheritance required. **Protocol** = implicit contract, any class with matching methods qualifies.

---

## 27. Dataclasses & Named Tuples

> **Interview Q:** *When would you use a dataclass vs a named tuple vs a regular class?*

### `@dataclass`

```python
from dataclasses import dataclass, field

@dataclass
class Point:
    x: float
    y: float
    z: float = 0.0          # default value
    tags: list = field(default_factory=list)   # mutable default — use field()

    def distance_from_origin(self):
        return (self.x**2 + self.y**2 + self.z**2)**0.5

p = Point(1.0, 2.0)
# Auto-generated: __init__, __repr__, __eq__

@dataclass(frozen=True)   # immutable — generates __hash__
class ImmutablePoint:
    x: float
    y: float

@dataclass(order=True)    # generates __lt__, __le__, __gt__, __ge__
class Ranked:
    score: float
    name: str
```

### Named Tuples

```python
from collections import namedtuple
from typing import NamedTuple

# Old style
Point = namedtuple("Point", ["x", "y"])

# New style — with types and defaults
class Point(NamedTuple):
    x: float
    y: float
    label: str = "origin"

p = Point(1.0, 2.0)
p.x           # 1.0
p[0]          # 1.0 — still a tuple!
x, y = p      # unpacking works
p._asdict()   # OrderedDict([('x', 1.0), ('y', 2.0)])
```

| | `dataclass` | `NamedTuple` | regular class |
|---|---|---|---|
| Mutable | Yes (default) | No | Yes |
| Tuple-compatible | No | Yes | No |
| Hashable | Only if frozen | Yes | Only if `__hash__` defined |
| Memory | Normal object | Tuple (compact) | Normal |
| Best for | Mutable records | Immutable records, CSV rows | Complex behavior |

---

## 28. Slots, `__dict__` & Memory Optimization

> **Interview Q:** *What are `__slots__` and how do they reduce memory usage?*

By default every Python object has a `__dict__` — a dictionary storing instance attributes. This is flexible but uses significant memory.

`__slots__` replaces `__dict__` with a fixed-size array of descriptors:

```python
# Without __slots__
class PointNormal:
    def __init__(self, x, y):
        self.x = x
        self.y = y

# With __slots__
class PointSlots:
    __slots__ = ("x", "y")   # no __dict__ created

    def __init__(self, x, y):
        self.x = x
        self.y = y

import sys
p1 = PointNormal(1, 2)
p2 = PointSlots(1, 2)

sys.getsizeof(p1.__dict__)   # ~232 bytes
# p2.__dict__                # AttributeError — no __dict__!

# Memory savings at scale
# 1 million PointNormal objects: ~300 MB
# 1 million PointSlots objects:  ~50 MB
```

**Tradeoffs:**
- No `__dict__` → can't add arbitrary attributes at runtime
- Can't use `__weakref__` unless added to `__slots__`
- Inheritance with `__slots__` requires care
- Best for: small objects created in very large numbers (e.g., graph nodes, data points)

---

## 29. Profiling & Performance Optimization

> **Interview Q:** *How would you find and fix performance bottlenecks in Python code?*

### Profiling Tools

```python
# cProfile — built-in function-level profiler
import cProfile
cProfile.run("my_function()")

# Or from command line:
# python -m cProfile -s cumulative script.py

# line_profiler — line-level profiling (pip install line-profiler)
# @profile decorator, then: kernprof -l -v script.py

# timeit — micro-benchmarks
import timeit
timeit.timeit("sorted([3,1,2])", number=100000)  # seconds for 100k runs

# In IPython/Jupyter
# %timeit my_function()
# %prun my_function()       — cProfile
# %lprun -f fn my_function() — line_profiler
```

### Common Optimizations

```python
# 1. Use built-ins — they're implemented in C
sum([1,2,3])        # faster than manual loop
"".join(strs)       # faster than += concatenation

# 2. Local variable lookup is faster than global
def fast():
    local_range = range    # cache built-in as local
    local_append = [].append
    for i in local_range(1000):
        local_append(i)

# 3. Avoid repeated attribute lookup
import os.path
exists = os.path.exists   # cache attribute
for p in paths:
    exists(p)             # faster than os.path.exists(p) each time

# 4. Use generators for large data
total = sum(x**2 for x in range(1_000_000))  # never builds the list

# 5. NumPy for numerical computation
import numpy as np
arr = np.array([1,2,3,4,5])
arr * 2           # vectorized — 100× faster than Python loop

# 6. Avoid global variable in hot loops
```

---

## 30. Python Internals — CPython, Bytecode, Objects

> **Interview Q:** *How are Python integers stored? Why can they be arbitrarily large?*

### Python Integers

CPython's `int` type is a variable-precision integer — stored as an array of "digits" in base 2^30 (30-bit chunks). This allows arbitrary precision with no overflow:

```python
2**1000   # no overflow in Python — arbitrary precision integer
# 10715086071862673209484250490600018105614048117055...

import sys
sys.getsizeof(0)     # 24 bytes
sys.getsizeof(1)     # 28 bytes
sys.getsizeof(2**30) # 32 bytes — needs more space
```

### Small Object Caching

CPython uses **free lists** (object pools) for frequently created/destroyed small objects: integers -5 to 256, empty lists, single-char strings, None, True, False.

```python
None is None     # always True — singleton
True is True     # always True — singleton
```

### `__slots__` vs `__dict__` Memory (recap)

Python attribute lookup order: instance `__dict__` → class `__dict__` → parent class `__dict__` → ...

### Bytecode Inspection

```python
import dis, ast

source = "x = a + b"
tree = ast.parse(source)
print(ast.dump(tree))
# Module(body=[Assign(targets=[Name(id='x')], value=BinOp(left=Name(id='a'), op=Add(), right=Name(id='b')))])

code = compile(source, "<string>", "exec")
dis.dis(code)
```

---

## 31. NumPy — The Foundation

> **Interview Q:** *Why is NumPy so much faster than Python lists for numerical operations?*

NumPy arrays are **contiguous blocks of typed C memory** + **vectorized C/Fortran operations**. No Python object overhead per element, no Python loop overhead.

```python
import numpy as np

# Array creation
arr = np.array([1, 2, 3, 4, 5])
zeros = np.zeros((3, 4))           # 3×4 array of 0.0
ones = np.ones((2, 3), dtype=int)
eye = np.eye(3)                    # 3×3 identity matrix
rng = np.arange(0, 10, 0.5)       # like range but returns array
linspace = np.linspace(0, 1, 100) # 100 evenly spaced points

# Shape manipulation
arr.shape        # (5,)
arr.reshape(1, 5)
arr.T            # transpose
arr.flatten()    # 1D copy
arr.ravel()      # 1D view (no copy if contiguous)

# Vectorized operations — applied element-wise, no Python loop
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])
a + b      # [5, 7, 9]
a * b      # [4, 10, 18]
a ** 2     # [1, 4, 9]
np.sqrt(a) # [1., 1.41, 1.73]

# Broadcasting — operations on arrays of different shapes
matrix = np.ones((3, 4))
row_bias = np.array([1, 2, 3, 4])   # shape (4,)
matrix + row_bias   # broadcasts row across all 3 rows

# Indexing & slicing
arr = np.arange(25).reshape(5, 5)
arr[2, 3]         # element at row 2, col 3
arr[:, 2]         # all rows, col 2
arr[1:3, 1:3]     # sub-matrix
arr[arr > 10]     # boolean indexing — returns 1D array of matching elements

# Aggregations
arr.sum()          # total
arr.sum(axis=0)    # sum along rows (per column)
arr.sum(axis=1)    # sum along cols (per row)
arr.mean(), arr.std(), arr.min(), arr.max()
np.argmax(arr)     # index of max element
```

---

## 32. Pandas — Data Manipulation

> **Interview Q:** *What is the difference between `loc` and `iloc`? How do you handle missing data in pandas?*

```python
import pandas as pd

# Creating DataFrames
df = pd.DataFrame({
    "name": ["Alice", "Bob", "Charlie"],
    "age": [25, 30, 35],
    "score": [90.5, 85.0, 92.3]
})

# Indexing
df["age"]              # Series — column by label
df[["age", "score"]]   # DataFrame — multiple columns
df.loc[0, "age"]       # loc — label-based (row label, col label)
df.iloc[0, 1]          # iloc — integer position-based
df.loc[df["age"] > 28] # boolean filtering

# Operations
df["grade"] = df["score"].apply(lambda x: "A" if x >= 90 else "B")
df.groupby("grade")["score"].mean()    # groupby + aggregate
df.sort_values("score", ascending=False)
df.drop_duplicates()
df.rename(columns={"name": "full_name"})

# Missing data
df.isnull().sum()                  # count NaN per column
df.dropna()                        # remove rows with any NaN
df.fillna(0)                       # replace NaN with 0
df["age"].fillna(df["age"].mean()) # fill with mean

# Merging
pd.merge(df1, df2, on="id", how="left")    # left join
pd.concat([df1, df2], axis=0)             # stack vertically
pd.concat([df1, df2], axis=1)             # stack horizontally

# Efficient operations
df.query("age > 28 and score > 85")       # query string (faster)
df.eval("total = score * 2")              # eval (fast in-place)
```

---

## 33. Python in ML Pipelines

> **Interview Q:** *How do you avoid common memory issues when loading large datasets in Python?*

```python
# 1. Chunked reading
for chunk in pd.read_csv("huge.csv", chunksize=10000):
    process(chunk)

# 2. Reduce dtypes
df["age"] = df["age"].astype("int8")      # 1 byte vs 8 bytes
df["category"] = df["category"].astype("category")  # string → int codes

# 3. Use generators for streaming
def data_generator(file):
    with open(file) as f:
        for line in f:
            yield parse(line)

# 4. Memory-mapped arrays
arr = np.memmap("data.npy", dtype="float32", mode="r", shape=(1000000, 128))

# 5. Multiprocessing for CPU-bound preprocessing
from concurrent.futures import ProcessPoolExecutor
with ProcessPoolExecutor() as executor:
    results = list(executor.map(preprocess, file_chunks))
```

---

## 34. Design Patterns in Python

> **Interview Q:** *How would you implement the Singleton pattern in Python? What about Observer?*

### Singleton

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

# Thread-safe singleton
import threading

class ThreadSafeSingleton:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        with cls._lock:
            if cls._instance is None:
                cls._instance = super().__new__(cls)
        return cls._instance
```

### Factory Pattern

```python
class Animal:
    @staticmethod
    def create(animal_type):
        classes = {"dog": Dog, "cat": Cat, "bird": Bird}
        cls = classes.get(animal_type.lower())
        if cls is None:
            raise ValueError(f"Unknown animal: {animal_type}")
        return cls()
```

### Observer Pattern

```python
class EventEmitter:
    def __init__(self):
        self._listeners: dict[str, list] = {}

    def on(self, event, callback):
        self._listeners.setdefault(event, []).append(callback)

    def emit(self, event, *args, **kwargs):
        for callback in self._listeners.get(event, []):
            callback(*args, **kwargs)

emitter = EventEmitter()
emitter.on("data", lambda d: print(f"Got: {d}"))
emitter.emit("data", {"key": "value"})
```

### Strategy Pattern

```python
from typing import Protocol

class SortStrategy(Protocol):
    def sort(self, data: list) -> list: ...

class BubbleSort:
    def sort(self, data):
        # ...
        return sorted_data

class QuickSort:
    def sort(self, data):
        # ...
        return sorted_data

class Sorter:
    def __init__(self, strategy: SortStrategy):
        self.strategy = strategy

    def sort(self, data):
        return self.strategy.sort(data)
```

---

## 35. Testing — unittest, pytest, mocking

> **Interview Q:** *What is the difference between a mock and a stub? How do you test code that calls external APIs?*

```python
# pytest — preferred modern testing framework
# run: pytest tests/ -v

def add(a, b):
    return a + b

# test_math.py
def test_add():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, -1) == -2

# Fixtures — reusable setup/teardown
import pytest

@pytest.fixture
def database():
    db = create_test_database()
    yield db          # test runs here
    db.teardown()     # cleanup

def test_query(database):
    result = database.query("SELECT 1")
    assert result == [(1,)]

# Parametrize — run same test with multiple inputs
@pytest.mark.parametrize("a,b,expected", [
    (2, 3, 5),
    (-1, 1, 0),
    (0, 0, 0),
])
def test_add_parametrized(a, b, expected):
    assert add(a, b) == expected

# Mocking external dependencies
from unittest.mock import patch, MagicMock

def get_user(user_id):
    response = requests.get(f"https://api.example.com/users/{user_id}")
    return response.json()

def test_get_user():
    mock_response = MagicMock()
    mock_response.json.return_value = {"id": 1, "name": "Alice"}

    with patch("requests.get", return_value=mock_response):
        user = get_user(1)

    assert user["name"] == "Alice"
    # requests.get was never actually called!
```

---

## 36. Packaging, Virtual Environments & Dependency Management

> **Interview Q:** *What is the difference between `pip`, `venv`, `poetry`, and `conda`?*

### Virtual Environments

```bash
# venv — built-in (Python 3.3+)
python -m venv .venv
source .venv/bin/activate         # Linux/Mac
.venv\Scripts\activate            # Windows

pip install requests
pip freeze > requirements.txt
pip install -r requirements.txt
```

### Modern Tools

```bash
# poetry — dependency resolution, packaging, publishing
poetry init
poetry add requests
poetry add --dev pytest
poetry install
poetry run python script.py
poetry build        # builds .whl and .tar.gz
poetry publish

# uv — ultra-fast package manager (Rust-based, Astral)
uv init
uv add requests
uv run python script.py

# conda — scientific/ML stack, non-Python dependencies too
conda create -n myenv python=3.11
conda activate myenv
conda install numpy pandas scipy
```

### Project Structure

```
myproject/
├── src/
│   └── mypackage/
│       ├── __init__.py
│       ├── core.py
│       └── utils.py
├── tests/
│   ├── conftest.py       # pytest fixtures
│   └── test_core.py
├── pyproject.toml        # modern project metadata
├── README.md
└── .gitignore
```

---

## 37. Interview Q&A — Fundamentals

**Q: What is the difference between `==` and `is`?**
`==` compares **values** (calls `__eq__`). `is` compares **identity** (same object in memory via `id()`). Always use `==` for value comparison; use `is` only for `None` checks (`if x is None`) or singleton comparisons.

**Q: What are Python's truthy/falsy values?**
Falsy: `False`, `0`, `0.0`, `""`, `[]`, `{}`, `set()`, `None`, `()`, `b""`. Everything else is truthy. Custom classes can implement `__bool__` or `__len__`.

**Q: How does Python handle integer overflow?**
It doesn't — Python integers are arbitrary precision. No overflow. This has a memory/speed tradeoff vs fixed-width integers in C/Java.

**Q: What is the difference between `deepcopy` and shallow copy?**
Shallow copy creates a new container but shares references to inner objects. Deep copy recursively duplicates everything. Use `copy.copy()` for shallow, `copy.deepcopy()` for deep.

**Q: What does `*` do in different contexts?**
- `def f(*args)` — packs extra positional args into tuple
- `f(*lst)` — unpacks list into positional args
- `a, *rest = [1,2,3,4]` — extended unpacking
- `[*a, *b]` — list/dict/set unpacking
- `import *` — imports all public names (avoid in production)

**Q: What is `__all__` in a module?**
A list of names that `from module import *` will export. Defines the public API of a module.

**Q: What is the difference between `list.sort()` and `sorted()`?**
`list.sort()` sorts **in place** (returns `None`). `sorted()` returns a **new sorted list** and works on any iterable. Both use Timsort — O(n log n), stable.

---

## 38. Interview Q&A — OOP & Advanced

**Q: What is the difference between `__repr__` and `__str__`?**
`__repr__` is for developers — should be unambiguous, ideally `eval()`-able to recreate the object. `__str__` is for users — readable, human-friendly. `print()` and `str()` use `__str__`; the interactive console and `repr()` use `__repr__`. If only `__repr__` is defined, it's used as fallback for `__str__`.

**Q: Can you modify a class after it's created?**
Yes — Python classes are mutable objects. You can add/modify attributes and methods at runtime (monkey patching):
```python
def new_method(self): return "patched"
MyClass.method = new_method   # affects all instances
```

**Q: What is the difference between `@classmethod` and `@staticmethod`?**
`@classmethod` receives the class (`cls`) as first argument — used for alternative constructors or methods that need class context. `@staticmethod` receives nothing automatically — it's just a regular function namespaced in the class.

**Q: How does Python implement `@property`?**
`@property` returns a descriptor object. Accessing the attribute triggers `__get__` on the descriptor, which calls the getter function. Setting calls `__set__`, deleting calls `__delete__`.

**Q: What is MRO and how is it computed?**
Method Resolution Order — the order in which Python searches classes for a method. Computed with the C3 Linearization algorithm. Always check with `ClassName.__mro__`. Ensures each class appears only once, parents come after children.

**Q: When would you use `__slots__`?**
When creating millions of small objects with a fixed set of attributes — `__slots__` saves ~40-50% memory by eliminating `__dict__` and using fixed-size descriptor arrays.

---

## 39. Interview Q&A — Concurrency & Performance

**Q: What is the GIL and why can't we just remove it?**
The GIL protects CPython's reference counting from race conditions. Removing it requires making every object operation thread-safe, which adds overhead even for single-threaded programs. Python 3.13 introduced experimental "no-GIL" mode (`python -X gil=0`) — still stabilizing.

**Q: When does threading actually help in Python?**
For I/O-bound tasks — the GIL is released during I/O operations (file, network, database). For CPU-bound tasks, use `multiprocessing` — each process has its own GIL.

**Q: What is the difference between a thread and a coroutine?**
A thread is a OS-level execution unit — preemptively scheduled by the OS. A coroutine is a cooperative execution unit — it explicitly yields control with `await`. Coroutines are much lighter (no OS context switch) and better for high-concurrency I/O. Thousands of coroutines are feasible; thousands of threads are not.

**Q: What is the event loop in asyncio?**
A loop that runs coroutines — it picks the next ready coroutine to run. When a coroutine does `await some_io_operation()`, it suspends itself and lets the event loop run other coroutines. When the I/O completes, the event loop wakes the suspended coroutine.

**Q: How would you make a Python program faster?**
Profile first. Then: algorithmic improvement (O(n²) → O(n log n)), use built-ins and C extensions (NumPy), use appropriate data structures (set for membership), generators for memory, `lru_cache` for memoization, `multiprocessing` for CPU-bound parallelism, asyncio for I/O-bound concurrency, Cython or ctypes for hot paths.

---

## 40. Interview Q&A — Data Structures & Algorithms in Python

**Q: Implement a stack and queue using Python lists.**
```python
# Stack — LIFO — use list with append/pop
stack = []
stack.append(1)   # push
stack.pop()       # pop (from end — O(1))

# Queue — FIFO — use collections.deque
from collections import deque
queue = deque()
queue.append(1)   # enqueue
queue.popleft()   # dequeue (O(1) — list.pop(0) is O(n))
```

**Q: Implement LRU Cache.**
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = OrderedDict()

    def get(self, key):
        if key not in self.cache:
            return -1
        self.cache.move_to_end(key)   # mark as recently used
        return self.cache[key]

    def put(self, key, value):
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)  # evict LRU (first item)

# Or just: from functools import lru_cache
```

**Q: How do you find duplicates in a list efficiently?**
```python
# O(n) with set
def find_duplicates(lst):
    seen = set()
    return {x for x in lst if x in seen or seen.add(x)}

# Or Counter
from collections import Counter
counts = Counter(lst)
dupes = [x for x, count in counts.items() if count > 1]
```

**Q: How do you sort a list of dicts by a key?**
```python
people = [{"name": "Bob", "age": 30}, {"name": "Alice", "age": 25}]
sorted(people, key=lambda p: p["age"])
sorted(people, key=lambda p: (p["age"], p["name"]))  # multi-key sort

from operator import itemgetter
sorted(people, key=itemgetter("age"))   # slightly faster than lambda
```

**Q: What is the time complexity of `in` for list vs set vs dict?**
- `list`: O(n) — linear scan
- `set`: O(1) average — hash lookup
- `dict` (key check): O(1) average — hash lookup

---

## 41. Quick Reference Cheat Sheet

### Time Complexities

| Structure | Access | Search | Insert | Delete |
|---|---|---|---|---|
| list | O(1) | O(n) | O(1) amortized (end) / O(n) (middle) | O(1) (end) / O(n) (middle) |
| deque | O(n) | O(n) | O(1) both ends | O(1) both ends |
| dict | O(1) avg | O(1) avg | O(1) avg | O(1) avg |
| set | — | O(1) avg | O(1) avg | O(1) avg |
| heapq (min-heap) | O(1) min | O(n) | O(log n) | O(log n) |

### Common `collections` Module

```python
from collections import (
    Counter,          # frequency count
    defaultdict,      # dict with default value
    OrderedDict,      # dict maintaining insertion order (useful for LRU)
    deque,            # double-ended queue
    namedtuple,       # lightweight immutable records
    ChainMap,         # merge multiple dicts without copying
)
```

### Common `itertools` Module

```python
import itertools

itertools.chain(a, b)              # flatten/chain iterables
itertools.chain.from_iterable([[1,2],[3,4]])  # [[1,2],[3,4]] → 1,2,3,4
itertools.islice(iterable, n)      # take first n items (lazy)
itertools.product([1,2],[3,4])     # Cartesian product: [(1,3),(1,4),(2,3),(2,4)]
itertools.permutations([1,2,3])    # all orderings
itertools.combinations([1,2,3],2) # choose 2: [(1,2),(1,3),(2,3)]
itertools.groupby(sorted_data, key=fn)  # group consecutive equal elements
itertools.accumulate([1,2,3,4])   # running totals: [1,3,6,10]
itertools.repeat(x, n)            # repeat x n times (or infinitely)
itertools.cycle([1,2,3])          # infinite cycle: 1,2,3,1,2,3,...
```

### `heapq` — Priority Queue

```python
import heapq

heap = []
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 2)
heapq.heappop(heap)    # 1 — always pops the MINIMUM

# For max-heap, negate values
heapq.heappush(heap, -value)
-heapq.heappop(heap)

# K largest/smallest
heapq.nlargest(3, data)   # [10, 9, 8]
heapq.nsmallest(3, data)  # [1, 2, 3]

# heapify existing list in O(n)
heapq.heapify(lst)
```

### Key Python Built-ins

```python
abs(), round(), divmod()
min(), max(), sum()
len(), range(), enumerate(), zip()
map(), filter(), sorted(), reversed()
any(), all()
isinstance(), issubclass(), type()
id(), hash()
repr(), str(), int(), float(), bool(), list(), dict(), set(), tuple()
open(), print(), input()
getattr(), setattr(), hasattr(), delattr()
vars(), dir()
callable()
iter(), next()
```

### One-Liners Worth Knowing

```python
# Flatten nested list
flat = [x for sub in nested for x in sub]
# or
import itertools; flat = list(itertools.chain.from_iterable(nested))

# Transpose matrix
transposed = list(zip(*matrix))

# Count character frequencies
from collections import Counter; freq = Counter("hello world")

# Remove duplicates preserving order (Python 3.7+)
deduped = list(dict.fromkeys(lst))

# Group by key
from itertools import groupby
groups = {k: list(v) for k, v in groupby(sorted(data, key=key_fn), key=key_fn)}

# Merge two lists into dict
d = dict(zip(keys, values))

# Swap variables
a, b = b, a

# Check if all elements are unique
len(lst) == len(set(lst))

# Default dict for nested structure
from collections import defaultdict
nested = defaultdict(lambda: defaultdict(int))
```

---

*Python Complete Notes — Basics to Advanced | Interview Ready*
*Covers: Data Types · OOP · Generators · Decorators · Concurrency · Memory · NumPy · Pandas · Design Patterns · Testing*

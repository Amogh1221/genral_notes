# 🏗️ Design Patterns

> **Interview-Ready | LLD Focused | Python + C++ Examples**
> *Most asked patterns in LLD rounds at product companies 2024–2026*

---

## Table of Contents

1. [What Are Design Patterns?](#1-what-are-design-patterns)
2. [SOLID Principles](#2-solid-principles)
3. [Creational Patterns](#3-creational-patterns)
4. [Structural Patterns](#4-structural-patterns)
5. [Behavioral Patterns](#5-behavioral-patterns)
6. [Patterns in ML Systems](#6-patterns-in-ml-systems)
7. [Interview Q&A](#7-interview-qa)

---

## 1. What Are Design Patterns?

Design patterns are **reusable solutions to commonly occurring problems** in software design. They are not code — they are templates/blueprints.

```
Introduced by: "Gang of Four" (GoF) — Gamma, Helm, Johnson, Vlissides (1994)
Book: "Design Patterns: Elements of Reusable Object-Oriented Software"

23 classic patterns grouped into 3 categories:
  Creational  → How objects are created
  Structural  → How objects are composed
  Behavioral  → How objects communicate
```

### Why Design Patterns Matter in Interviews

```
The 4 most asked in LLD interviews (in order):
  1. Strategy    → "Design a payment system / sorting system"
  2. Observer    → "Design a notification system / event system"
  3. Factory     → "Design a model loader / database connection"
  4. Singleton   → "Thread-safe config manager / logger"
```

---

## 2. SOLID Principles

> **Interview Q:** *What are SOLID principles? Give an ML example for each.*

SOLID principles guide object-oriented design to make code maintainable, flexible, and testable.

### S — Single Responsibility Principle

```
A class should have only ONE reason to change.
One class = one responsibility.

❌ Bad:
class MLModel:
    def train(self, data): ...      # training logic
    def predict(self, x): ...       # inference logic
    def save_to_disk(self): ...     # persistence logic  ← different concern
    def log_metrics(self): ...      # logging logic       ← different concern

✓ Good:
class MLModel:
    def train(self, data): ...
    def predict(self, x): ...

class ModelSerializer:
    def save(self, model): ...

class MetricsLogger:
    def log(self, metrics): ...
```

### O — Open/Closed Principle

```
Open for EXTENSION, Closed for MODIFICATION.
Add new behaviour without changing existing code.

❌ Bad: Adding a new model type requires modifying existing code.
class Trainer:
    def train(self, model_type):
        if model_type == "linear": ...
        elif model_type == "tree": ...
        elif model_type == "neural": ...  # must modify this class!

✓ Good: Each model implements an interface; Trainer never changes.
from abc import ABC, abstractmethod

class Model(ABC):
    @abstractmethod
    def fit(self, X, y): ...

class LinearModel(Model):
    def fit(self, X, y): ...   # new models just add new classes

class NeuralNet(Model):
    def fit(self, X, y): ...

class Trainer:
    def train(self, model: Model, X, y):
        model.fit(X, y)        # never changes
```

### L — Liskov Substitution Principle

```
Derived classes must be substitutable for their base class
without breaking the program.

If S is a subtype of T, you should be able to use S wherever T is expected.

❌ Bad:
class Bird:
    def fly(self): ...

class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins can't fly!")  # violates LSP

✓ Good: Separate the interface.
class Bird: ...

class FlyingBird(Bird):
    def fly(self): ...

class Penguin(Bird): ...      # doesn't inherit fly()
class Eagle(FlyingBird): ... # CAN fly
```

### I — Interface Segregation Principle

```
Don't force clients to implement interfaces they don't use.
Many small specific interfaces > one large general interface.

❌ Bad:
class MLPipeline(ABC):
    def load_data(self): ...
    def preprocess(self): ...
    def train(self): ...
    def evaluate(self): ...
    def deploy(self): ...    # not all pipelines need all steps

✓ Good: Split into focused interfaces.
class DataLoader(ABC):
    def load_data(self): ...

class Trainer(ABC):
    def train(self): ...

class Deployer(ABC):
    def deploy(self): ...
```

### D — Dependency Inversion Principle

```
High-level modules should not depend on low-level modules.
Both should depend on abstractions (interfaces).

❌ Bad: High-level code directly depends on specific DB.
class ModelRepository:
    def __init__(self):
        self.db = PostgreSQLDatabase()  # tightly coupled!

    def save(self, model): self.db.write(model)

✓ Good: Depend on an abstraction.
class Database(ABC):
    @abstractmethod
    def write(self, data): ...

class PostgreSQLDatabase(Database): ...
class MongoDatabase(Database): ...

class ModelRepository:
    def __init__(self, db: Database):   # injected, not created
        self.db = db

    def save(self, model): self.db.write(model)
# Can now swap PostgreSQL for Mongo without changing ModelRepository.
```

---

## 3. Creational Patterns

### Singleton Pattern

> **Interview Q:** *What is the Singleton pattern? What are its drawbacks? How do you make it thread-safe?*

**Intent:** Ensure only ONE instance of a class exists and provide a global access point to it.

```python
# Python — Thread-safe Singleton using __new__
import threading

class ModelRegistry:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls):
        if cls._instance is None:
            with cls._lock:         # double-checked locking
                if cls._instance is None:
                    cls._instance = super().__new__(cls)
                    cls._instance.models = {}
        return cls._instance

    def register(self, name, model):
        self.models[name] = model

    def get(self, name):
        return self.models.get(name)

# Usage
reg1 = ModelRegistry()
reg2 = ModelRegistry()
print(reg1 is reg2)   # True — same instance
```

```cpp
// C++ — Thread-safe Singleton (C++11 magic statics)
class Logger {
public:
    static Logger& getInstance() {
        static Logger instance;  // C++11: guaranteed thread-safe initialisation
        return instance;
    }
    void log(const std::string& msg) { std::cout << msg << "\n"; }

private:
    Logger() {}                         // private constructor
    Logger(const Logger&) = delete;     // no copy
    Logger& operator=(const Logger&) = delete;
};

// Usage
Logger::getInstance().log("Training started");
```

**Real ML uses:** Logger, Configuration manager, Model registry, Database connection pool, Feature store client.

**Drawbacks:**
```
❌ Hard to unit test (global state, can't inject mock)
❌ Violates SRP (manages its own lifecycle + does its job)
❌ Hidden dependencies (code that uses it doesn't declare dependency)
❌ Concurrency issues if not implemented carefully

Better alternative: Dependency Injection (pass the single instance around)
```

---

### Factory Method Pattern

> **Interview Q:** *What is the Factory pattern? How is it different from Abstract Factory?*

**Intent:** Define an interface for creating objects, but let subclasses decide which class to instantiate. Decouples object creation from usage.

```python
from abc import ABC, abstractmethod

# Product interface
class Model(ABC):
    @abstractmethod
    def predict(self, x): ...

# Concrete products
class LinearModel(Model):
    def predict(self, x): return x @ self.weights

class RandomForest(Model):
    def predict(self, x): return self.trees.predict(x)

class NeuralNet(Model):
    def predict(self, x): return self.forward(x)

# Factory — centralises creation logic
class ModelFactory:
    @staticmethod
    def create(model_type: str, **kwargs) -> Model:
        models = {
            "linear":  LinearModel,
            "rf":      RandomForest,
            "neural":  NeuralNet,
        }
        if model_type not in models:
            raise ValueError(f"Unknown model: {model_type}")
        return models[model_type](**kwargs)

# Usage — caller doesn't know which class is instantiated
model = ModelFactory.create("rf", n_estimators=100)
predictions = model.predict(X_test)
```

**Abstract Factory** creates families of related objects:

```python
# Abstract Factory: create related objects (e.g., optimizer + scheduler together)
class TrainingFactory(ABC):
    @abstractmethod
    def create_optimizer(self): ...
    @abstractmethod
    def create_scheduler(self): ...

class AdamFactory(TrainingFactory):
    def create_optimizer(self): return AdamOptimizer(lr=0.001)
    def create_scheduler(self): return CosineScheduler()

class SGDFactory(TrainingFactory):
    def create_optimizer(self): return SGDOptimizer(lr=0.01)
    def create_scheduler(self): return StepScheduler()
```

**Factory vs Abstract Factory:**
```
Factory Method:   Creates ONE type of product. Subclasses decide which concrete class.
Abstract Factory: Creates FAMILIES of related products. Ensures compatible objects.
```

---

### Builder Pattern

> **Interview Q:** *When would you use Builder over a constructor with many parameters?*

**Intent:** Construct complex objects step-by-step. Separate construction from representation.

```python
# Problem: Constructor with too many optional parameters
# ModelConfig(lr=0.001, epochs=100, batch_size=32, dropout=0.2, ...)

class ModelConfig:
    def __init__(self):
        self.lr = 0.001
        self.epochs = 100
        self.batch_size = 32
        self.dropout = 0.0
        self.optimizer = "adam"

class ModelConfigBuilder:
    def __init__(self):
        self._config = ModelConfig()

    def learning_rate(self, lr):
        self._config.lr = lr
        return self          # returns self for chaining

    def epochs(self, n):
        self._config.epochs = n
        return self

    def batch_size(self, bs):
        self._config.batch_size = bs
        return self

    def dropout(self, p):
        self._config.dropout = p
        return self

    def build(self) -> ModelConfig:
        return self._config

# Usage — readable, fluent interface
config = (ModelConfigBuilder()
    .learning_rate(0.0001)
    .epochs(50)
    .batch_size(64)
    .dropout(0.3)
    .build())
```

**ML uses:** Building training configs, constructing complex ML pipelines, `sklearn`'s Pipeline class follows this idea.

---

## 4. Structural Patterns

### Adapter Pattern

**Intent:** Convert the interface of a class into another interface that clients expect. Makes incompatible interfaces work together.

```python
# You have a PyTorch model but your system expects sklearn-style .fit()/.predict()
class PyTorchModel:
    def forward(self, tensor): ...
    def train_step(self, batch): ...

class SklearnInterface(ABC):
    def fit(self, X, y): ...
    def predict(self, X): ...

# Adapter wraps PyTorch model to look like sklearn
class PyTorchAdapter(SklearnInterface):
    def __init__(self, torch_model):
        self.model = torch_model

    def fit(self, X, y):
        # Convert X, y to tensors and call torch training loop
        tensor_X = torch.tensor(X)
        self.model.train_step((tensor_X, torch.tensor(y)))

    def predict(self, X):
        return self.model.forward(torch.tensor(X)).numpy()

# Now PyTorch model works in any sklearn pipeline!
```

---

### Decorator Pattern

**Intent:** Attach additional responsibilities to an object dynamically without changing its class.

```python
from functools import wraps
import time

# Function decorators in Python ARE the Decorator pattern
def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time()-start:.2f}s")
        return result
    return wrapper

def cache_result(func):
    cache = {}
    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

# Stack multiple decorators
@timer
@cache_result
def load_model(model_id):
    # expensive operation
    return model_registry.get(model_id)

# Class-based Decorator for ML
class LoggingModel:
    """Wraps any model and adds prediction logging."""
    def __init__(self, model):
        self.model = model
        self.prediction_count = 0

    def predict(self, X):
        self.prediction_count += 1
        result = self.model.predict(X)
        print(f"Prediction #{self.prediction_count}: shape={result.shape}")
        return result

model = LoggingModel(RandomForest())
```

---

### Facade Pattern

**Intent:** Provide a simplified interface to a complex subsystem.

```python
# Complex subsystem
class DataLoader: ...
class Preprocessor: ...
class FeatureEngineer: ...
class ModelTrainer: ...
class ModelEvaluator: ...
class ModelSerializer: ...

# Facade: simple interface hiding complexity
class MLPipelineFacade:
    def __init__(self):
        self.loader = DataLoader()
        self.preprocessor = Preprocessor()
        self.engineer = FeatureEngineer()
        self.trainer = ModelTrainer()
        self.evaluator = ModelEvaluator()
        self.serializer = ModelSerializer()

    def train_and_deploy(self, data_path: str, model_path: str):
        """Single method that orchestrates the entire pipeline."""
        data = self.loader.load(data_path)
        clean = self.preprocessor.clean(data)
        features = self.engineer.transform(clean)
        model = self.trainer.train(features)
        metrics = self.evaluator.evaluate(model, features)
        self.serializer.save(model, model_path)
        return metrics

# User only sees the facade
pipeline = MLPipelineFacade()
metrics = pipeline.train_and_deploy("data/train.csv", "models/v1.pkl")
```

---

## 5. Behavioral Patterns

### Strategy Pattern

> **Interview Q:** *Design a system where you can swap sorting algorithms or payment methods at runtime.*

**Intent:** Define a family of algorithms, encapsulate each one, and make them interchangeable at runtime.

```python
from abc import ABC, abstractmethod

# Strategy interface
class TrainingStrategy(ABC):
    @abstractmethod
    def train(self, model, X, y): ...

# Concrete strategies
class FullBatchGD(TrainingStrategy):
    def train(self, model, X, y):
        for epoch in range(self.epochs):
            grad = model.gradient(X, y)
            model.update(grad)

class MiniBatchSGD(TrainingStrategy):
    def __init__(self, batch_size=32):
        self.batch_size = batch_size

    def train(self, model, X, y):
        for batch_X, batch_y in self.get_batches(X, y, self.batch_size):
            grad = model.gradient(batch_X, batch_y)
            model.update(grad)

class AdamOptimizer(TrainingStrategy):
    def train(self, model, X, y):
        # Adam-specific training logic
        ...

# Context — uses any strategy
class Trainer:
    def __init__(self, strategy: TrainingStrategy):
        self.strategy = strategy

    def set_strategy(self, strategy: TrainingStrategy):
        self.strategy = strategy  # swap at runtime!

    def train(self, model, X, y):
        self.strategy.train(model, X, y)

# Usage
trainer = Trainer(MiniBatchSGD(batch_size=64))
trainer.train(model, X_train, y_train)

# Swap strategy without changing Trainer
trainer.set_strategy(AdamOptimizer())
trainer.train(model, X_train, y_train)
```

---

### Observer Pattern

> **Interview Q:** *Design a notification system where multiple components react to model training events.*

**Intent:** Define a one-to-many dependency so that when one object changes state, all its dependents are notified automatically.

```python
from abc import ABC, abstractmethod
from typing import List

# Observer interface
class TrainingObserver(ABC):
    @abstractmethod
    def on_epoch_end(self, epoch: int, metrics: dict): ...

# Concrete observers
class MetricsLogger(TrainingObserver):
    def on_epoch_end(self, epoch, metrics):
        print(f"Epoch {epoch}: loss={metrics['loss']:.4f}")

class ModelCheckpointer(TrainingObserver):
    def on_epoch_end(self, epoch, metrics):
        if metrics['val_loss'] < self.best_loss:
            self.save_checkpoint(epoch)

class EarlyStoppingObserver(TrainingObserver):
    def on_epoch_end(self, epoch, metrics):
        if self.should_stop(metrics):
            raise StopIteration("Early stopping triggered")

class WandbLogger(TrainingObserver):
    def on_epoch_end(self, epoch, metrics):
        wandb.log(metrics, step=epoch)

# Subject (Observable)
class TrainingLoop:
    def __init__(self):
        self._observers: List[TrainingObserver] = []

    def subscribe(self, observer: TrainingObserver):
        self._observers.append(observer)

    def unsubscribe(self, observer: TrainingObserver):
        self._observers.remove(observer)

    def _notify(self, epoch, metrics):
        for obs in self._observers:
            obs.on_epoch_end(epoch, metrics)

    def train(self, model, data):
        for epoch in range(self.epochs):
            metrics = self._train_epoch(model, data)
            self._notify(epoch, metrics)  # all observers called

# Usage
loop = TrainingLoop()
loop.subscribe(MetricsLogger())
loop.subscribe(ModelCheckpointer())
loop.subscribe(WandbLogger())
loop.train(model, train_data)
```

**Real ML connection:** Keras callbacks ARE the Observer pattern. `ModelCheckpoint`, `EarlyStopping`, `TensorBoard` are all observers of the training loop.

---

### Command Pattern

**Intent:** Encapsulate a request as an object. Supports undo, redo, queuing, and logging of operations.

```python
from abc import ABC, abstractmethod

class Command(ABC):
    @abstractmethod
    def execute(self): ...
    @abstractmethod
    def undo(self): ...

class SetLearningRateCommand(Command):
    def __init__(self, optimizer, new_lr):
        self.optimizer = optimizer
        self.new_lr = new_lr
        self.old_lr = optimizer.lr

    def execute(self):
        self.optimizer.lr = self.new_lr

    def undo(self):
        self.optimizer.lr = self.old_lr

# Command history enables undo
class ExperimentManager:
    def __init__(self):
        self.history = []

    def execute(self, command: Command):
        command.execute()
        self.history.append(command)

    def undo_last(self):
        if self.history:
            self.history.pop().undo()
```

---

## 6. Patterns in ML Systems

```
Model Loading & Serving:
  Factory Pattern     → ModelFactory.create("bert-base") loads correct class
  Singleton Pattern   → Single model instance shared across request threads
  Strategy Pattern    → Swap inference backend (CPU/GPU/TensorRT) at runtime

Training Pipeline:
  Observer Pattern    → Callbacks (Keras), hooks (PyTorch)
  Builder Pattern     → Pipeline config, hyperparameter config
  Template Method     → BaseTrainer with hooks for subclasses to fill in

Feature Engineering:
  Decorator Pattern   → Feature transformers stacked (StandardScaler + PCA + ...)
  Strategy Pattern    → Swap imputation strategy (mean vs median vs model-based)

Data Access:
  Singleton Pattern   → Database connection pool
  Factory Pattern     → Create different data loaders (CSV, Parquet, SQL)
  Adapter Pattern     → Wrap different data sources to uniform interface

Experiment Management:
  Command Pattern     → Undo/redo hyperparameter changes
  Observer Pattern    → Log to multiple backends (MLflow, WandB, TensorBoard)
```

---

## 7. Interview Q&A

**Q: What is the difference between Strategy and State patterns?**
> Both involve interchangeable behaviours, but the trigger differs. **Strategy**: the client consciously chooses and sets the algorithm (e.g., user chooses optimizer). The object doesn't change its own strategy. **State**: the object changes its own behaviour automatically based on its internal state (e.g., a TCP connection behaves differently when Connecting vs Connected vs Closed — transitions happen based on events).

**Q: What is the difference between Factory Method and Abstract Factory?**
> **Factory Method**: one method that creates one type of product. Subclasses override to decide the concrete class. **Abstract Factory**: creates a family of related products. Ensures the products are compatible (e.g., always create AdamOptimizer with CosineScheduler, never mix). Use Abstract Factory when you need consistent families of objects.

**Q: When should you NOT use Singleton?**
> Avoid Singleton when: (1) you need to unit test in isolation — Singleton introduces global state that's hard to mock, (2) you need multiple instances in future (e.g., you assumed one database, now need multiple), (3) the class has complex state that multiple parts of the app modify (hard to reason about). Prefer Dependency Injection — create the object once at startup and pass it everywhere.

**Q: What is the difference between Decorator and Inheritance?**
> Both add behaviour to objects. **Inheritance** adds behaviour at compile time via subclassing — static, rigid, leads to class explosion when combining behaviours. **Decorator** adds behaviour at runtime by wrapping objects dynamically — flexible, composable, avoids deep class hierarchies. Rule: prefer Decorator (composition) over inheritance when behaviours can be combined in multiple ways.

**Q: Explain the Template Method pattern.**
> Define the skeleton of an algorithm in a base class, deferring some steps to subclasses. The base class calls abstract methods that subclasses implement. Example: `BaseTrainer` defines `train()` which calls `load_data()`, `preprocess()`, `fit()`, `evaluate()` — subclasses override the steps but the overall flow is fixed. PyTorch Lightning's `LightningModule` uses this pattern (you implement `training_step`, `validation_step` etc).

**Q: What design pattern does Python's `with` statement implement?**
> The **Resource Acquisition Is Initialisation (RAII)** pattern, implemented via the Context Manager protocol (`__enter__` / `__exit__`). This is equivalent to the Proxy or Decorator pattern — it wraps an object and manages resource lifecycle. The underlying pattern is also called the "Handle/Body" or "Resource Manager" pattern.

**Q: What is the Repository pattern?**
> Separates domain logic from data access logic. A Repository provides a collection-like interface for accessing domain objects, hiding the underlying database queries. Example: `ModelRepository` has `save(model)`, `find_by_id(id)`, `find_by_accuracy(threshold)` — callers don't know if data comes from PostgreSQL, MongoDB, or a file. Very common in ML systems for managing experiments, models, and datasets.

---

*Sources: Gang of Four Design Patterns, InterviewBit Design Patterns 2025, GeeksforGeeks Top Design Pattern Questions, Medium LLD Interview Patterns, Educative Design Patterns Interview Questions.*

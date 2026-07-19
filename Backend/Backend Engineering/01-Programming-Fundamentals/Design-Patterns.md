# Design Patterns for Backend Engineers

## What is it?
Well-known, reusable solutions to common software design problems — a shared vocabulary that lets engineers communicate structure quickly ("just use a Factory here") instead of re-explaining from scratch.

## Why care?
Recognizing these patterns in existing codebases (and knowing when — and when NOT — to apply them) is a genuine mark of engineering maturity. Overusing patterns unnecessarily is also a real, common mistake worth avoiding.

## Creational patterns (object creation)

### Singleton
Ensures only ONE instance of a class exists globally — commonly used for things like a database connection pool or a configuration object.
```python
class Config:
    _instance = None
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```
**Caution**: overused Singletons create hidden global state, making code harder to test — use deliberately, not by default.

### Factory
Centralizes object creation logic, so callers don't need to know the exact class being instantiated.
```python
def create_payment_processor(type):
    if type == "stripe": return StripeProcessor()
    if type == "paypal": return PayPalProcessor()
    raise ValueError("Unknown processor")
```

## Structural patterns (composing objects)

### Adapter
Converts one interface into another your code expects — very common when integrating third-party services with a different API shape than your internal code expects.

### Decorator
Adds behavior to an object dynamically, without modifying its class — Python decorators, or middleware in web frameworks, are real-world examples of this exact pattern.
```python
@login_required   # a decorator wrapping a route handler with auth logic
def get_profile(request):
    ...
```

## Behavioral patterns (object interaction)

### Strategy
Defines a family of interchangeable algorithms, selected at runtime — this is essentially the Polymorphism example from `OOP-Principles.md` (swap payment processors), formalized as a named pattern.

### Observer
Objects ("subscribers") get notified automatically when another object's ("publisher") state changes — the foundation of event-driven architectures and pub/sub messaging systems (`07-Message-Queues-and-Async`).

### Repository Pattern (very common in backend specifically)
Abstracts data access behind a consistent interface, decoupling business logic from the specific database/ORM in use.
```python
class UserRepository:
    def get_by_id(self, id): ...
    def save(self, user): ...

# Business logic depends on UserRepository, not directly on SQL/ORM calls
```

## In backend engineering
- **Middleware** (Express.js, Django, ASP.NET Core) is the Decorator pattern applied to HTTP request handling — each middleware wraps the next, adding behavior (logging, auth, error handling) around the core request handler.
- **Dependency Injection frameworks** (Spring, NestJS, .NET's built-in DI) heavily use Factory and Strategy patterns internally to wire up services.
- **The Repository pattern is one of the most practically useful patterns for backend work** — it's what lets you write unit tests against a fake in-memory repository instead of needing a real database for every test (`09-Testing/Mocking.md`).
- **Overuse warning**: don't force a pattern where simple, direct code would do — patterns solve *specific* recurring problems (flexibility, testability, decoupling); applying them everywhere adds unnecessary indirection and complexity. This judgment call is itself a mark of senior engineering experience.

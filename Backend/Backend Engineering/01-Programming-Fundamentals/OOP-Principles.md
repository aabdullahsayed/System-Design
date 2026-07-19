# Object-Oriented Programming Principles

## What is it?
A programming paradigm organizing code around **objects** (bundles of data + behavior) rather than just functions and data separately — built on four core principles: Encapsulation, Abstraction, Inheritance, Polymorphism.

## Why care?
Most backend codebases (especially in Java, C#, Python, TypeScript) are structured using OOP — understanding these principles is essential for reading, writing, and reviewing real production code, and for structuring maintainable services as they grow.

## The 4 pillars

### Encapsulation
Bundling data and the methods that operate on it together, hiding internal implementation details behind a public interface.
```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance    # "private" by convention (underscore)

    def withdraw(self, amount):
        if amount > self._balance:
            raise ValueError("Insufficient funds")
        self._balance -= amount    # internal state only changed through controlled methods
```
Why it matters: prevents invalid states (e.g., directly setting `balance = -500` from outside), and lets you change internal implementation later without breaking code that depends on the class.

### Abstraction
Exposing only what's necessary, hiding complexity behind a simpler interface.
```python
# Callers don't need to know HOW send_email works internally (SMTP details, retries, etc.)
class EmailService:
    def send(self, to, subject, body):
        ...  # complex implementation hidden
```

### Inheritance
A class can inherit behavior from a parent class, reusing and extending code.
```python
class PaymentProcessor:
    def process(self, amount):
        raise NotImplementedError

class StripeProcessor(PaymentProcessor):
    def process(self, amount):
        # Stripe-specific implementation
        pass

class PayPalProcessor(PaymentProcessor):
    def process(self, amount):
        # PayPal-specific implementation
        pass
```

### Polymorphism
Different classes can be used interchangeably through a shared interface — the caller doesn't need to know which specific implementation it's using.
```python
def checkout(processor: PaymentProcessor, amount):
    processor.process(amount)   # works with ANY PaymentProcessor subclass

checkout(StripeProcessor(), 100)
checkout(PayPalProcessor(), 100)
```

## In backend engineering
- **Dependency Injection** (used throughout Spring, .NET, NestJS) relies entirely on polymorphism — your service code depends on an interface/abstraction, not a concrete implementation, making it swappable and testable.
- **Repository pattern** (`03-Databases`) abstracts database access behind an interface, so your business logic doesn't care whether data comes from PostgreSQL, MongoDB, or a mock in tests.
- **"Favor composition over inheritance"** is a widely-held modern principle — deep inheritance hierarchies become fragile and hard to reason about; many modern backend codebases favor smaller, composable interfaces instead.
- **SOLID principles** (a related, commonly interview-tested set of OOP design guidelines: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion) build directly on these 4 pillars, and are worth knowing by name even if you don't apply every one rigidly.

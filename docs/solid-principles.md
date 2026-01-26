# SOLID Principles

Reference documentation for the five SOLID principles of object-oriented design.

---

## Overview

SOLID is an acronym for five design principles intended to make software designs more understandable, flexible, and maintainable. These principles were introduced by Robert C. Martin (Uncle Bob).

| Principle | Full Name | Core Concept |
|-----------|-----------|--------------|
| **S** | Single Responsibility | One class, one reason to change |
| **O** | Open/Closed | Open for extension, closed for modification |
| **L** | Liskov Substitution | Subtypes must be substitutable for base types |
| **I** | Interface Segregation | Many specific interfaces over one general |
| **D** | Dependency Inversion | Depend on abstractions, not concretions |

---

## S - Single Responsibility Principle (SRP)

### Definition

**A class MUST have only one reason to change.**

Each class or module MUST be responsible for a single part of the functionality provided by the software.

### Identification Checklist

| Indicator | SRP Violation | Required Action |
|-----------|---------------|-----------------|
| Class has multiple unrelated methods | YES | **Split into separate classes** |
| Class name contains "And" or "Manager" | LIKELY | **Review for multiple responsibilities** |
| Changes in one area require changes in unrelated methods | YES | **Extract to separate class** |
| Class has more than one reason to change | YES | **Decompose by responsibility** |

### Anti-Patterns to Detect

| Anti-Pattern | Description | Refactoring Action |
|--------------|-------------|-------------------|
| **God Class** | Class that knows/does too much | **Extract classes by responsibility** |
| **Utility Dumping Ground** | Static utility class with unrelated methods | **Group by domain, create focused utilities** |
| **Mixed Concerns** | Business logic mixed with infrastructure | **Separate into layers** |

### Verification Questions

Before concluding SRP compliance, MUST answer:

1. Can you describe what this class does in one sentence without using "and"?
2. If requirements change in area X, will only this class need modification?
3. Does this class have a single, focused purpose?

---

## O - Open/Closed Principle (OCP)

### Definition

**Software entities MUST be open for extension but closed for modification.**

You MUST be able to extend a class's behavior without modifying its existing code.

### Identification Checklist

| Indicator | OCP Violation | Required Action |
|-----------|---------------|-----------------|
| Adding new feature requires modifying existing class | YES | **Use abstraction/polymorphism** |
| Switch/if-else chains based on type | YES | **Replace with polymorphism** |
| Hardcoded business rules | YES | **Extract to strategy/configuration** |
| Direct dependencies on concrete classes | LIKELY | **Introduce interfaces** |

### Implementation Strategies

| Strategy | When to Use | How |
|----------|-------------|-----|
| **Strategy Pattern** | Varying algorithms | Define interface, implement variants |
| **Template Method** | Varying steps in algorithm | Abstract base class with hooks |
| **Decorator Pattern** | Adding behavior dynamically | Wrap objects with new behavior |
| **Plugin Architecture** | Extensible systems | Define extension points |

### Anti-Patterns to Detect

| Anti-Pattern | Description | Refactoring Action |
|--------------|-------------|-------------------|
| **Type Checking** | instanceof/typeof checks for behavior | **Replace with polymorphism** |
| **Shotgun Surgery** | One change requires many file edits | **Consolidate or use abstraction** |
| **Rigid Design** | Cannot add features without breaking existing code | **Introduce extension points** |

---

## L - Liskov Substitution Principle (LSP)

### Definition

**Objects of a superclass MUST be replaceable with objects of its subclasses without affecting program correctness.**

Subtypes MUST be substitutable for their base types.

### Identification Checklist

| Indicator | LSP Violation | Required Action |
|-----------|---------------|-----------------|
| Subclass throws unexpected exceptions | YES | **Redesign hierarchy** |
| Subclass ignores or overrides base behavior incorrectly | YES | **Use composition instead** |
| Client code checks type before calling methods | YES | **Fix inheritance or use polymorphism** |
| Subclass weakens postconditions | YES | **Strengthen subclass guarantees** |
| Subclass strengthens preconditions | YES | **Relax subclass requirements** |

### Contract Rules (MANDATORY)

| Rule | Requirement |
|------|-------------|
| **Preconditions** | Subclass CANNOT strengthen (require more) |
| **Postconditions** | Subclass CANNOT weaken (guarantee less) |
| **Invariants** | Subclass MUST preserve all base class invariants |
| **History Constraint** | Subclass CANNOT modify state in ways base class doesn't allow |

### Anti-Patterns to Detect

| Anti-Pattern | Description | Refactoring Action |
|--------------|-------------|-------------------|
| **Refused Bequest** | Subclass doesn't use inherited methods | **Use composition or redesign hierarchy** |
| **Circle-Ellipse Problem** | Inheritance models "is-a" incorrectly | **Favor composition over inheritance** |
| **Empty Override** | Method overridden to do nothing | **Remove from hierarchy or redesign** |

### Verification Questions

Before concluding LSP compliance, MUST answer:

1. Can any code using the base class work correctly with the subclass?
2. Does the subclass honor all contracts of the base class?
3. Are there any type checks in client code to handle specific subclasses?

---

## I - Interface Segregation Principle (ISP)

### Definition

**Clients MUST NOT be forced to depend on interfaces they do not use.**

Many client-specific interfaces are better than one general-purpose interface.

### Identification Checklist

| Indicator | ISP Violation | Required Action |
|-----------|---------------|-----------------|
| Interface has methods not all implementers need | YES | **Split interface** |
| Classes implement methods with empty bodies or throw "not supported" | YES | **Segregate interface** |
| Interface name is too generic (IManager, IHandler) | LIKELY | **Create focused interfaces** |
| Changes to interface affect unrelated clients | YES | **Decompose by client needs** |

### Segregation Strategies

| Strategy | When to Use |
|----------|-------------|
| **Role Interfaces** | Different clients need different views of same object |
| **Header Interfaces** | Extract interface from existing class |
| **Multiple Inheritance** | Class implements multiple focused interfaces |

### Anti-Patterns to Detect

| Anti-Pattern | Description | Refactoring Action |
|--------------|-------------|-------------------|
| **Fat Interface** | Interface with too many methods | **Split by cohesion** |
| **Interface Bloat** | Interface grows with each new requirement | **Create separate interfaces for new concerns** |
| **Dummy Implementation** | Methods that throw NotImplementedException | **Segregate required vs optional methods** |

---

## D - Dependency Inversion Principle (DIP)

### Definition

**High-level modules MUST NOT depend on low-level modules. Both MUST depend on abstractions.**

**Abstractions MUST NOT depend on details. Details MUST depend on abstractions.**

### Identification Checklist

| Indicator | DIP Violation | Required Action |
|-----------|---------------|-----------------|
| High-level class instantiates low-level class directly | YES | **Inject dependency via interface** |
| Import/using statements reference concrete implementations | YES | **Depend on abstractions** |
| Business logic depends on infrastructure details | YES | **Invert dependency direction** |
| Cannot unit test without real dependencies | YES | **Introduce interfaces for mocking** |

### Implementation Strategies

| Strategy | Description |
|----------|-------------|
| **Constructor Injection** | Dependencies passed via constructor |
| **Property Injection** | Dependencies set via properties |
| **Method Injection** | Dependencies passed to specific methods |
| **Service Locator** | Central registry provides dependencies (use sparingly) |

### Layer Direction Rules

```
CORRECT (Dependency Inversion):
┌─────────────────────────────────────┐
│  High-Level Policy (Business Logic) │
│         depends on                  │
│        ↓ ABSTRACTION ↓              │
│  Low-Level Detail (Infrastructure)  │
│      implements abstraction         │
└─────────────────────────────────────┘

INCORRECT (Traditional):
┌─────────────────────────────────────┐
│  High-Level Policy (Business Logic) │
│         depends on                  │
│        ↓ CONCRETE ↓                 │
│  Low-Level Detail (Infrastructure)  │
└─────────────────────────────────────┘
```

### Anti-Patterns to Detect

| Anti-Pattern | Description | Refactoring Action |
|--------------|-------------|-------------------|
| **New is Glue** | Using `new` to create dependencies | **Inject dependencies instead** |
| **Hidden Dependencies** | Dependencies created inside methods | **Make dependencies explicit** |
| **Service Locator Abuse** | Overusing service locator pattern | **Prefer constructor injection** |

---

## SOLID Compliance Checklist

**MANDATORY: Before concluding SOLID compliance, verify ALL principles:**

```
[ ] SRP: Each class has single responsibility
[ ] SRP: Class names describe single purpose
[ ] OCP: New features can be added via extension
[ ] OCP: No type-checking conditionals
[ ] LSP: Subtypes are substitutable for base types
[ ] LSP: No contract violations in inheritance
[ ] ISP: Interfaces are client-specific
[ ] ISP: No empty/dummy implementations
[ ] DIP: High-level modules depend on abstractions
[ ] DIP: Dependencies are injected, not created
```

---

## Anti-Rationalization Table

| Rationalization | Why It's WRONG | Required Action |
|-----------------|----------------|-----------------|
| "This class is small, SRP doesn't apply" | Size is irrelevant. Responsibility count matters. | **Check for multiple reasons to change** |
| "Adding interface is over-engineering" | Interfaces enable extension and testing. | **Add interface if class has dependencies** |
| "Inheritance is simpler than composition" | Inheritance creates tight coupling. | **Evaluate LSP compliance first** |
| "One big interface is easier to maintain" | Fat interfaces force unnecessary dependencies. | **Split by client needs** |
| "Direct instantiation is cleaner" | Direct instantiation prevents testing and extension. | **Inject dependencies** |

---

## References

- Source: Robert C. Martin (Uncle Bob) - "Agile Software Development, Principles, Patterns, and Practices"
- See also: [Design Patterns](design-patterns/) for implementation strategies

# Factory Method Pattern

Reference documentation for the Factory Method creational design pattern.

---

## Overview

| Aspect            | Description                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------ |
| **Category**      | Creational                                                                                       |
| **Intent**        | Define an interface for creating an object, but let subclasses decide which class to instantiate |
| **Also Known As** | Virtual Constructor                                                                              |
| **Source**        | [Refactoring.guru - Factory Method](https://refactoring.guru/design-patterns/factory-method)     |

---

## Problem It Solves

| Problem                           | How Factory Method Helps                |
| --------------------------------- | --------------------------------------- |
| Code coupled to concrete classes  | Defers instantiation to subclasses      |
| Cannot extend creation logic      | Subclasses override factory method      |
| Complex object creation scattered | Centralizes creation logic              |
| Need different implementations    | Factory method returns appropriate type |

---

## Structure

```
┌───────────────────────────┐
│      Creator              │
├───────────────────────────┤
│ + factoryMethod(): Product│ ◄── abstract or default
│ + someOperation()         │     implementation
└───────────┬───────────────┘
            │ extends
┌───────────┴───────────────┐
│   ConcreteCreator         │
├───────────────────────────┤
│ + factoryMethod(): Product│ ◄── returns ConcreteProduct
└───────────────────────────┘

┌───────────────────────────┐
│     Product (interface)   │
└───────────┬───────────────┘
            │ implements
┌───────────┴───────────────┐
│   ConcreteProduct         │
└───────────────────────────┘
```

---

## Participants

| Participant         | Responsibility                                     |
| ------------------- | -------------------------------------------------- |
| **Product**         | Interface for objects the factory method creates   |
| **ConcreteProduct** | Implements the Product interface                   |
| **Creator**         | Declares factory method, may provide default       |
| **ConcreteCreator** | Overrides factory method to return ConcreteProduct |

---

## When to Use

| Scenario                                  | Indicator                          |
| ----------------------------------------- | ---------------------------------- |
| Class cannot anticipate object types      | YES - use factory method           |
| Class wants subclasses to specify objects | YES - use factory method           |
| Need to localize object creation          | YES - use factory method           |
| Creating objects based on conditions      | CONSIDER - may be simpler approach |

---

## When NOT to Use

| Scenario                    | Why Not                 |
| --------------------------- | ----------------------- |
| Single concrete type only   | Over-engineering        |
| Creation logic never varies | Unnecessary abstraction |
| Simple object instantiation | Direct `new` is simpler |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Product interface/abstract class defined
[ ] Creator declares factory method
[ ] Factory method returns Product type
[ ] ConcreteCreator overrides factory method
[ ] Client code uses Product interface (not concrete)
[ ] No direct instantiation in client code
```

---

## Anti-Patterns to Detect

| Anti-Pattern                                    | Indication               | Refactoring Action             |
| ----------------------------------------------- | ------------------------ | ------------------------------ |
| **Factory returning multiple types via switch** | Type code parameter      | **Consider Abstract Factory**  |
| **Factory method doing too much**               | Complex logic in factory | **Extract creation logic**     |
| **Client aware of concrete products**           | Defeats purpose          | **Use only Product interface** |

---

## Related Patterns

| Pattern              | Relationship                                 |
| -------------------- | -------------------------------------------- |
| **Abstract Factory** | Often implemented with factory methods       |
| **Template Method**  | Factory method often called from template    |
| **Prototype**        | Alternative that doesn't require subclassing |

---

## References

- Source: [Refactoring.guru - Factory Method](https://refactoring.guru/design-patterns/factory-method)
- See also: [Abstract Factory](creational-abstract-factory.md)

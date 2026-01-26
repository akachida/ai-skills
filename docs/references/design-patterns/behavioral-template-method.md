# Template Method Pattern

Reference documentation for the Template Method behavioral design pattern.

---

## Overview

| Aspect       | Description                                                                                                                                                                                                |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category** | Behavioral                                                                                                                                                                                                 |
| **Intent**   | Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure |
| **Source**   | [Refactoring.guru - Template Method](https://refactoring.guru/design-patterns/template-method)                                                                                                             |

---

## Problem It Solves

| Problem                               | How Template Method Helps      |
| ------------------------------------- | ------------------------------ |
| Algorithm structure fixed, steps vary | Defines skeleton, defers steps |
| Code duplication in subclasses        | Common structure in base       |
| Need hooks for extension              | Hook methods for customization |
| Want to control extension points      | Base class controls flow       |

---

## Structure

```
┌─────────────────────────────────────┐
│         AbstractClass               │
├─────────────────────────────────────┤
│ + templateMethod() {                │
│     step1();                        │
│     step2();       // may be hook   │
│     step3();                        │
│   }                                 │
│ + step1()          // abstract      │
│ + step2()          // hook (default)│
│ # step3()          // required      │
└───────────────┬─────────────────────┘
                │ extends
┌───────────────┴─────────────────────┐
│        ConcreteClass                │
├─────────────────────────────────────┤
│ + step1()          // implements    │
│ + step2()          // overrides     │
│ # step3()          // implements    │
└─────────────────────────────────────┘
```

---

## Participants

| Participant       | Responsibility                                            |
| ----------------- | --------------------------------------------------------- |
| **AbstractClass** | Defines template method and abstract primitive operations |
| **ConcreteClass** | Implements primitive operations                           |

---

## Operation Types

| Type                    | Description                                           |
| ----------------------- | ----------------------------------------------------- |
| **Template Method**     | Defines algorithm skeleton, calls other operations    |
| **Abstract Operations** | Must be implemented by subclasses                     |
| **Hook Operations**     | Have default behavior, can be overridden              |
| **Concrete Operations** | Implemented in base class, not meant to be overridden |

---

## When to Use

| Scenario                        | Indicator                       |
| ------------------------------- | ------------------------------- |
| Common algorithm, varying steps | YES - use template method       |
| Control subclass extensions     | YES - define hooks              |
| Avoid code duplication          | YES - common parts in base      |
| Inversion of control needed     | YES - base class calls subclass |

---

## When NOT to Use

| Scenario                               | Why Not                |
| -------------------------------------- | ---------------------- |
| Algorithm varies completely            | Strategy may be better |
| No common algorithm structure          | No template to define  |
| Composition preferred over inheritance | Use Strategy instead   |

---

## Template Method vs Strategy

| Aspect        | Template Method             | Strategy                   |
| ------------- | --------------------------- | -------------------------- |
| **Mechanism** | Inheritance                 | Composition                |
| **Variation** | Subclass overrides steps    | Different strategy objects |
| **Algorithm** | Structure fixed, steps vary | Entire algorithm varies    |
| **Coupling**  | Tighter (inheritance)       | Looser (interface)         |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Template method defined in base class
[ ] Template method marked final (if possible)
[ ] Abstract operations identified
[ ] Hook operations have sensible defaults
[ ] Minimal required operations for subclasses
[ ] Subclasses implement required operations
```

---

## Anti-Patterns to Detect

| Anti-Pattern                     | Indication                   | Refactoring Action                     |
| -------------------------------- | ---------------------------- | -------------------------------------- |
| **Too many abstract operations** | Subclass must implement many | **Add more hooks, fewer requirements** |
| **Template can be overridden**   | Subclass changes algorithm   | **Make template method final**         |
| **Deep inheritance**             | Multiple levels of templates | **Flatten or use Strategy**            |

---

## Related Patterns

| Pattern            | Relationship                    |
| ------------------ | ------------------------------- |
| **Strategy**       | Composition-based alternative   |
| **Factory Method** | Often called by template method |
| **Hook Operation** | Special type used in template   |

---

## References

- Source: [Refactoring.guru - Template Method](https://refactoring.guru/design-patterns/template-method)
- See also: [Strategy](behavioral-strategy.md), [Factory Method](creational-factory-method.md)

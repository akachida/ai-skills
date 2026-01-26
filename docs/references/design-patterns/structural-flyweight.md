# Flyweight Pattern

Reference documentation for the Flyweight structural design pattern.

---

## Overview

| Aspect            | Description                                                                        |
| ----------------- | ---------------------------------------------------------------------------------- |
| **Category**      | Structural                                                                         |
| **Intent**        | Use sharing to support large numbers of fine-grained objects efficiently           |
| **Also Known As** | Cache                                                                              |
| **Source**        | [Refactoring.guru - Flyweight](https://refactoring.guru/design-patterns/flyweight) |

---

## Problem It Solves

| Problem                         | How Flyweight Helps        |
| ------------------------------- | -------------------------- |
| Too many objects consume memory | Shares common state        |
| Redundant data across objects   | Extracts intrinsic state   |
| Object creation overhead        | Reuses existing flyweights |
| Performance with many objects   | Reduces memory footprint   |

---

## Structure

```
┌─────────────────────┐
│    FlyweightFactory │
├─────────────────────┤         ┌─────────────────────┐
│ - flyweights: Map   │         │     Flyweight       │
│ + getFlyweight(key) │────────►│    (interface)      │
└─────────────────────┘         ├─────────────────────┤
                                │ + operation(        │
                                │    extrinsicState)  │
                                └──────────┬──────────┘
                                           │ implements
                                ┌──────────┴──────────┐
                                │ ConcreteFlyweight   │
                                ├─────────────────────┤
                                │ - intrinsicState    │ ◄── shared
                                │ + operation(        │
                                │    extrinsicState)  │ ◄── passed in
                                └─────────────────────┘
```

---

## Key Concepts

| Concept               | Description                            |
| --------------------- | -------------------------------------- |
| **Intrinsic State**   | Shared, immutable, stored in flyweight |
| **Extrinsic State**   | Context-specific, passed to operations |
| **Flyweight Factory** | Creates and manages flyweight pool     |

---

## Participants

| Participant           | Responsibility                                             |
| --------------------- | ---------------------------------------------------------- |
| **Flyweight**         | Interface through which flyweights receive extrinsic state |
| **ConcreteFlyweight** | Stores intrinsic state, shared                             |
| **FlyweightFactory**  | Creates and manages flyweights                             |
| **Client**            | Maintains extrinsic state, uses factory                    |

---

## When to Use

| Scenario                         | Indicator                            |
| -------------------------------- | ------------------------------------ |
| Many similar objects             | YES - share common parts             |
| Object state can be externalized | YES - separate intrinsic/extrinsic   |
| Memory is constrained            | YES - reduces footprint              |
| Identity is not important        | YES - flyweights are interchangeable |

---

## When NOT to Use

| Scenario                     | Why Not                 |
| ---------------------------- | ----------------------- |
| Few objects                  | Overhead not justified  |
| Most state is extrinsic      | Little to share         |
| Objects need unique identity | Sharing breaks identity |
| State cannot be separated    | Pattern not applicable  |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Intrinsic state identified (shareable, immutable)
[ ] Extrinsic state identified (context-specific)
[ ] Flyweight is immutable
[ ] Factory manages flyweight pool
[ ] Client provides extrinsic state to operations
[ ] Memory savings verified
```

---

## Anti-Patterns to Detect

| Anti-Pattern                  | Indication                   | Refactoring Action        |
| ----------------------------- | ---------------------------- | ------------------------- |
| **Mutable flyweight**         | State changes after creation | **Make immutable**        |
| **Excessive extrinsic state** | Complex state passing        | **Reconsider separation** |
| **Premature optimization**    | Applied without memory issue | **Verify need first**     |

---

## Related Patterns

| Pattern            | Relationship                      |
| ------------------ | --------------------------------- |
| **Composite**      | Often combined, shared leaf nodes |
| **State/Strategy** | Can be implemented as flyweights  |
| **Singleton**      | Factory often singleton           |

---

## References

- Source: [Refactoring.guru - Flyweight](https://refactoring.guru/design-patterns/flyweight)
- See also: [Composite](structural-composite.md)

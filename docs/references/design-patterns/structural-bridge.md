# Bridge Pattern

Reference documentation for the Bridge structural design pattern.

---

## Overview

| Aspect            | Description                                                                            |
| ----------------- | -------------------------------------------------------------------------------------- |
| **Category**      | Structural                                                                             |
| **Intent**        | Decouple an abstraction from its implementation so that the two can vary independently |
| **Also Known As** | Handle/Body                                                                            |
| **Source**        | [Refactoring.guru - Bridge](https://refactoring.guru/design-patterns/bridge)           |

---

## Problem It Solves

| Problem                                        | How Bridge Helps               |
| ---------------------------------------------- | ------------------------------ |
| Abstraction and implementation tightly coupled | Separates into two hierarchies |
| Explosion of subclasses                        | Compose instead of inherit     |
| Need to switch implementations at runtime      | Implementation is pluggable    |
| Permanent binding at compile time              | Binding can change             |

---

## Structure

```
┌──────────────────────┐
│     Abstraction      │
├──────────────────────┤        ┌──────────────────────┐
│ - implementation ────│───────►│   Implementation     │
│ + operation()        │        │   (interface)        │
└──────────┬───────────┘        ├──────────────────────┤
           │                    │ + operationImpl()    │
           │ extends            └──────────┬───────────┘
┌──────────┴───────────┐                   │ implements
│  RefinedAbstraction  │        ┌──────────┴───────────┐
├──────────────────────┤        │                      │
│ + operation()        │   ┌────┴────┐          ┌──────┴─────┐
└──────────────────────┘   │ ImplA   │          │   ImplB    │
                           └─────────┘          └────────────┘
```

---

## Participants

| Participant                | Responsibility                                        |
| -------------------------- | ----------------------------------------------------- |
| **Abstraction**            | Defines interface, maintains implementation reference |
| **RefinedAbstraction**     | Extends abstraction interface                         |
| **Implementation**         | Interface for implementation classes                  |
| **ConcreteImplementation** | Implements the Implementation interface               |

---

## When to Use

| Scenario                                                 | Indicator                    |
| -------------------------------------------------------- | ---------------------------- |
| Want to avoid permanent binding                          | YES - use bridge             |
| Both abstraction and implementation should be extensible | YES - two hierarchies        |
| Cartesian product of variations                          | YES - avoids class explosion |
| Need to switch implementations at runtime                | YES - compose at runtime     |

---

## When NOT to Use

| Scenario                                       | Why Not                      |
| ---------------------------------------------- | ---------------------------- |
| Single implementation                          | Unnecessary indirection      |
| Abstraction stable, only implementation varies | Factory/Strategy may suffice |
| No combinatorial explosion                     | Simpler alternatives exist   |

---

## Bridge vs Adapter

| Aspect          | Bridge                       | Adapter                         |
| --------------- | ---------------------------- | ------------------------------- |
| **Intent**      | Design to vary independently | Make incompatible work together |
| **When**        | Up-front design              | Usually after the fact          |
| **Hierarchies** | Two parallel hierarchies     | Wraps existing class            |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Abstraction and implementation can vary independently
[ ] Abstraction holds reference to implementation
[ ] Implementation interface defined
[ ] Concrete implementations don't depend on abstraction
[ ] Client uses abstraction, not implementation directly
[ ] Composition used (not inheritance between abstraction and implementation)
```

---

## Anti-Patterns to Detect

| Anti-Pattern           | Indication                                      | Refactoring Action            |
| ---------------------- | ----------------------------------------------- | ----------------------------- |
| **God abstraction**    | Abstraction knows too much about implementation | **Refine interface boundary** |
| **Leaky bridge**       | Abstraction exposes implementation details      | **Encapsulate properly**      |
| **Unused flexibility** | Bridge with single implementation               | **Consider if needed**        |

---

## Related Patterns

| Pattern              | Relationship                        |
| -------------------- | ----------------------------------- |
| **Abstract Factory** | Can create bridge components        |
| **Adapter**          | Similar structure, different intent |
| **Strategy**         | Can be seen as degenerate bridge    |

---

## References

- Source: [Refactoring.guru - Bridge](https://refactoring.guru/design-patterns/bridge)
- See also: [Adapter](structural-adapter.md), [Strategy](behavioral-strategy.md)

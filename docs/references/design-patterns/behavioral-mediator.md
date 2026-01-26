# Mediator Pattern

Reference documentation for the Mediator behavioral design pattern.

---

## Overview

| Aspect       | Description                                                                                                                                                   |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category** | Behavioral                                                                                                                                                    |
| **Intent**   | Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly |
| **Source**   | [Refactoring.guru - Mediator](https://refactoring.guru/design-patterns/mediator)                                                                              |

---

## Problem It Solves

| Problem                           | How Mediator Helps            |
| --------------------------------- | ----------------------------- |
| Many-to-many object relationships | Centralizes communication     |
| Tight coupling between components | Components know only mediator |
| Hard to reuse components          | Components become independent |
| Complex interaction logic         | Encapsulated in mediator      |

---

## Structure

```
┌─────────────────────┐
│     Mediator        │
│    (interface)      │
├─────────────────────┤
│ + notify(sender,    │
│          event)     │
└──────────┬──────────┘
           │ implements
┌──────────┴──────────┐
│  ConcreteMediator   │
├─────────────────────┤         ┌─────────────────────┐
│ - componentA        │────────►│    Component        │
│ - componentB        │         │    (base class)     │
│ - componentC        │         ├─────────────────────┤
│ + notify()          │◄────────│ - mediator          │
└─────────────────────┘         │ + setMediator()     │
                                └──────────┬──────────┘
                                           │ extends
                                ┌──────────┴──────────┐
                                │  ConcreteComponent  │
                                │   A, B, C...        │
                                └─────────────────────┘
```

---

## Participants

| Participant             | Responsibility                                  |
| ----------------------- | ----------------------------------------------- |
| **Mediator**            | Interface for communication between components  |
| **ConcreteMediator**    | Coordinates communication, knows all components |
| **Colleague/Component** | Base class knowing only mediator                |
| **ConcreteColleague**   | Specific component, communicates via mediator   |

---

## When to Use

| Scenario                                | Indicator                       |
| --------------------------------------- | ------------------------------- |
| Objects communicate in complex ways     | YES - centralize logic          |
| Want to reuse objects independently     | YES - decouple via mediator     |
| Behavior distributed among many classes | YES - localize in mediator      |
| Set of objects tightly coupled          | YES - mediator loosens coupling |

---

## When NOT to Use

| Scenario                    | Why Not                 |
| --------------------------- | ----------------------- |
| Simple interactions         | Unnecessary indirection |
| Few objects                 | Over-engineering        |
| Mediator becomes god object | Defeats purpose         |

---

## Mediator vs Facade

| Aspect        | Mediator                 | Facade                      |
| ------------- | ------------------------ | --------------------------- |
| **Direction** | Bidirectional            | Unidirectional              |
| **Awareness** | Components know mediator | Subsystem unaware of facade |
| **Purpose**   | Coordinate peers         | Simplify interface          |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Mediator interface defined
[ ] ConcreteMediator holds component references
[ ] Components communicate only through mediator
[ ] Components don't reference each other
[ ] Mediator encapsulates interaction logic
[ ] Mediator doesn't become god object
```

---

## Anti-Patterns to Detect

| Anti-Pattern           | Indication                         | Refactoring Action               |
| ---------------------- | ---------------------------------- | -------------------------------- |
| **God mediator**       | Mediator does everything           | **Split into smaller mediators** |
| **Bypassing mediator** | Components communicate directly    | **Route through mediator**       |
| **Two-way dependency** | Component depends on mediator type | **Use interface**                |

---

## Related Patterns

| Pattern                     | Relationship                         |
| --------------------------- | ------------------------------------ |
| **Facade**                  | Simplifies, mediator coordinates     |
| **Observer**                | Mediator can use observer internally |
| **Chain of Responsibility** | Alternative for message passing      |

---

## References

- Source: [Refactoring.guru - Mediator](https://refactoring.guru/design-patterns/mediator)
- See also: [Facade](structural-facade.md), [Observer](behavioral-observer.md)

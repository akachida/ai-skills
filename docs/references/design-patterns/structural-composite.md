# Composite Pattern

Reference documentation for the Composite structural design pattern.

---

## Overview

| Aspect       | Description                                                                                                                                          |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category** | Structural                                                                                                                                           |
| **Intent**   | Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions uniformly |
| **Source**   | [Refactoring.guru - Composite](https://refactoring.guru/design-patterns/composite)                                                                   |

---

## Problem It Solves

| Problem                                | How Composite Helps             |
| -------------------------------------- | ------------------------------- |
| Part-whole hierarchies                 | Models tree structure naturally |
| Treat individuals and groups same      | Uniform interface               |
| Recursive structures                   | Inherent in design              |
| Client complexity with different types | Single interface for all        |

---

## Structure

```
┌──────────────────────────┐
│      Component           │
│     (interface)          │
├──────────────────────────┤
│ + operation()            │
│ + add(Component)         │ ◄── optional (composite only)
│ + remove(Component)      │ ◄── optional (composite only)
│ + getChild(i)            │ ◄── optional (composite only)
└──────────────┬───────────┘
               │ implements
     ┌─────────┴─────────┐
     │                   │
┌────┴────┐        ┌─────┴─────┐
│  Leaf   │        │ Composite │
├─────────┤        ├───────────┤
│ +op()   │        │-children[]│
└─────────┘        │ +op()     │ ◄── iterates children
                   │ +add()    │
                   │ +remove() │
                   └───────────┘
```

---

## Participants

| Participant   | Responsibility                                          |
| ------------- | ------------------------------------------------------- |
| **Component** | Common interface for all objects in composition         |
| **Leaf**      | Represents end objects with no children                 |
| **Composite** | Stores children and implements child-related operations |
| **Client**    | Manipulates objects through Component interface         |

---

## When to Use

| Scenario                                 | Indicator                  |
| ---------------------------------------- | -------------------------- |
| Represent part-whole hierarchies         | YES - use composite        |
| Want clients to ignore leaf vs composite | YES - uniform interface    |
| Tree structure                           | YES - natural fit          |
| Recursive operations on structure        | YES - operation propagates |

---

## When NOT to Use

| Scenario                                          | Why Not                   |
| ------------------------------------------------- | ------------------------- |
| Flat structure (no nesting)                       | Unnecessary complexity    |
| Leaf and composite have very different operations | Interface becomes awkward |
| Performance critical with deep trees              | Overhead of recursion     |

---

## Design Decisions

### Where to declare child management?

| Option                | Pros                 | Cons                          |
| --------------------- | -------------------- | ----------------------------- |
| **In Component**      | Uniform interface    | Leaf has meaningless methods  |
| **In Composite only** | Clean Leaf interface | Client must distinguish types |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Component interface defined
[ ] Leaf implements Component
[ ] Composite implements Component
[ ] Composite stores child Components
[ ] Composite.operation() iterates children
[ ] Client works with Component interface
[ ] Child management location decided
```

---

## Anti-Patterns to Detect

| Anti-Pattern                | Indication                         | Refactoring Action        |
| --------------------------- | ---------------------------------- | ------------------------- |
| **Type checking in client** | Client checks if Leaf or Composite | **Use uniform interface** |
| **Deep nesting**            | Very deep trees cause issues       | **Consider depth limits** |
| **Circular references**     | Child contains parent as child     | **Validate additions**    |

---

## Related Patterns

| Pattern                     | Relationship                        |
| --------------------------- | ----------------------------------- |
| **Decorator**               | Similar structure, different intent |
| **Iterator**                | Traverses composite structure       |
| **Visitor**                 | Applies operations across composite |
| **Chain of Responsibility** | Similar recursive structure         |

---

## References

- Source: [Refactoring.guru - Composite](https://refactoring.guru/design-patterns/composite)
- See also: [Decorator](structural-decorator.md), [Iterator](behavioral-iterator.md)

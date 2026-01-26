# Visitor Pattern

Reference documentation for the Visitor behavioral design pattern.

---

## Overview

| Aspect       | Description                                                                                                                                                                          |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Category** | Behavioral                                                                                                                                                                           |
| **Intent**   | Represent an operation to be performed on elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates |
| **Source**   | [Refactoring.guru - Visitor](https://refactoring.guru/design-patterns/visitor)                                                                                                       |

---

## Problem It Solves

| Problem                                   | How Visitor Helps           |
| ----------------------------------------- | --------------------------- |
| Need to add operations to class hierarchy | Add visitor, not methods    |
| Operations span multiple classes          | Centralized in visitor      |
| Class hierarchy stable, operations change | New visitor = new operation |
| Want to avoid polluting classes           | Operations outside classes  |

---

## Structure

```
┌─────────────────────┐
│      Visitor        │
│    (interface)      │
├─────────────────────┤
│+visitElementA(A)    │
│+visitElementB(B)    │
└──────────┬──────────┘
           │ implements
┌──────────┴──────────┐
│  ConcreteVisitor    │        ┌─────────────────────┐
├─────────────────────┤        │      Element        │
│+visitElementA(A)    │        │    (interface)      │
│+visitElementB(B)    │        ├─────────────────────┤
└─────────────────────┘        │+accept(Visitor)     │
                               └──────────┬──────────┘
                                          │ implements
                               ┌──────────┴──────────┐
                               │                     │
                     ┌─────────┴─────┐    ┌──────────┴─────┐
                     │ ElementA      │    │   ElementB     │
                     ├───────────────┤    ├────────────────┤
                     │+accept(v) {   │    │+accept(v) {    │
                     │ v.visitA(this)│    │ v.visitB(this) │
                     │}              │    │}               │
                     └───────────────┘    └────────────────┘
```

---

## Participants

| Participant         | Responsibility                                    |
| ------------------- | ------------------------------------------------- |
| **Visitor**         | Declares visit operation for each ConcreteElement |
| **ConcreteVisitor** | Implements operations for each element type       |
| **Element**         | Declares accept method taking visitor             |
| **ConcreteElement** | Implements accept, calls visitor's method         |
| **ObjectStructure** | Collection of elements, may provide iteration     |

---

## Double Dispatch

Visitor uses double dispatch to select correct operation:

1. First dispatch: `element.accept(visitor)` - selects element
2. Second dispatch: `visitor.visit(this)` - selects visitor method

---

## When to Use

| Scenario                              | Indicator                     |
| ------------------------------------- | ----------------------------- |
| Operations on object structure        | YES - visitor encapsulates    |
| Many distinct operations needed       | YES - visitor per operation   |
| Class hierarchy stable                | YES - adding elements is hard |
| Related operations should be together | YES - centralized in visitor  |

---

## When NOT to Use

| Scenario                               | Why Not                      |
| -------------------------------------- | ---------------------------- |
| Element classes change frequently      | Must update all visitors     |
| Few operations needed                  | Over-engineering             |
| Elements need to encapsulate operation | Visitor breaks encapsulation |

---

## Trade-offs

| Advantage                         | Disadvantage                  |
| --------------------------------- | ----------------------------- |
| New operations easy               | New elements hard             |
| Related operations together       | Breaks encapsulation          |
| Accumulates state across elements | Visitor knows element details |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Visitor interface with visit method per element type
[ ] Element interface with accept(Visitor)
[ ] ConcreteElements implement accept
[ ] accept calls appropriate visit method
[ ] ConcreteVisitors implement all visit methods
[ ] Object structure provides traversal (if needed)
```

---

## Anti-Patterns to Detect

| Anti-Pattern               | Indication                  | Refactoring Action                 |
| -------------------------- | --------------------------- | ---------------------------------- |
| **Downcasting in visitor** | Visitor checks type         | **Use proper double dispatch**     |
| **Incomplete visitor**     | Missing visit methods       | **Implement all required methods** |
| **God visitor**            | One visitor does everything | **Split by operation type**        |

---

## Related Patterns

| Pattern         | Relationship                           |
| --------------- | -------------------------------------- |
| **Composite**   | Visitor often used with composite      |
| **Iterator**    | Visitor can use iterator for traversal |
| **Interpreter** | Can use visitor for interpretation     |

---

## References

- Source: [Refactoring.guru - Visitor](https://refactoring.guru/design-patterns/visitor)
- See also: [Composite](structural-composite.md), [Iterator](behavioral-iterator.md)

# Iterator Pattern

Reference documentation for the Iterator behavioral design pattern.

---

## Overview

| Aspect            | Description                                                                                                             |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Category**      | Behavioral                                                                                                              |
| **Intent**        | Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation |
| **Also Known As** | Cursor                                                                                                                  |
| **Source**        | [Refactoring.guru - Iterator](https://refactoring.guru/design-patterns/iterator)                                        |

---

## Problem It Solves

| Problem                                    | How Iterator Helps          |
| ------------------------------------------ | --------------------------- |
| Need to traverse different data structures | Uniform interface           |
| Don't want to expose internal structure    | Encapsulates traversal      |
| Need multiple traversal algorithms         | Different iterators         |
| Want concurrent traversals                 | Multiple iterator instances |

---

## Structure

```
┌─────────────────────┐
│     Iterator        │
│    (interface)      │
├─────────────────────┤         ┌─────────────────────┐
│ + hasNext(): bool   │         │  IterableCollection │
│ + next(): Element   │         │    (interface)      │
└──────────┬──────────┘         ├─────────────────────┤
           │                    │ + createIterator()  │
           │ implements         └──────────┬──────────┘
┌──────────┴──────────┐                    │ implements
│  ConcreteIterator   │         ┌──────────┴──────────┐
├─────────────────────┤         │ConcreteCollection   │
│ - collection        │◄────────│                     │
│ - position          │         ├─────────────────────┤
│ + hasNext()         │         │ + createIterator()  │
│ + next()            │         └─────────────────────┘
└─────────────────────┘
```

---

## Participants

| Participant              | Responsibility                                  |
| ------------------------ | ----------------------------------------------- |
| **Iterator**             | Interface for accessing and traversing elements |
| **ConcreteIterator**     | Implements Iterator, tracks position            |
| **Aggregate/Collection** | Interface for creating Iterator                 |
| **ConcreteAggregate**    | Returns instance of ConcreteIterator            |

---

## When to Use

| Scenario                                       | Indicator                         |
| ---------------------------------------------- | --------------------------------- |
| Traverse collection without exposing internals | YES - use iterator                |
| Multiple traversal algorithms needed           | YES - different iterators         |
| Uniform interface for different collections    | YES - iterator abstraction        |
| Support concurrent iteration                   | YES - separate iterator instances |

---

## When NOT to Use

| Scenario                     | Why Not                       |
| ---------------------------- | ----------------------------- |
| Simple array/list traversal  | Built-in iteration sufficient |
| Single traversal method ever | Over-engineering              |
| Random access needed         | Iterator is sequential        |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Iterator interface defined (hasNext, next at minimum)
[ ] ConcreteIterator tracks traversal state
[ ] Collection provides createIterator()
[ ] Iterator doesn't expose collection internals
[ ] Multiple iterators can exist simultaneously
[ ] Iterator invalidation handled (if collection changes)
```

---

## Anti-Patterns to Detect

| Anti-Pattern            | Indication                           | Refactoring Action             |
| ----------------------- | ------------------------------------ | ------------------------------ |
| **Exposing internals**  | Iterator reveals structure           | **Encapsulate properly**       |
| **Invalidation issues** | Collection modified during iteration | **Document or fail-fast**      |
| **Single iterator**     | Can only have one at a time          | **Support multiple instances** |

---

## Related Patterns

| Pattern            | Relationship                         |
| ------------------ | ------------------------------------ |
| **Composite**      | Often iterated over                  |
| **Factory Method** | createIterator is factory method     |
| **Memento**        | Iterator can capture iteration state |

---

## References

- Source: [Refactoring.guru - Iterator](https://refactoring.guru/design-patterns/iterator)
- See also: [Composite](structural-composite.md)

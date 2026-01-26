# Memento Pattern

Reference documentation for the Memento behavioral design pattern.

---

## Overview

| Aspect            | Description                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Category**      | Behavioral                                                                                                                                 |
| **Intent**        | Without violating encapsulation, capture and externalize an object's internal state so that the object can be restored to this state later |
| **Also Known As** | Snapshot                                                                                                                                   |
| **Source**        | [Refactoring.guru - Memento](https://refactoring.guru/design-patterns/memento)                                                             |

---

## Problem It Solves

| Problem                        | How Memento Helps               |
| ------------------------------ | ------------------------------- |
| Need to restore previous state | Captures and restores           |
| Undo/redo functionality        | Stores state history            |
| State snapshots needed         | Creates snapshots               |
| Maintain encapsulation         | State captured without exposing |

---

## Structure

```
┌─────────────────────┐
│     Originator      │
├─────────────────────┤
│ - state             │
│ + save(): Memento   │
│ + restore(Memento)  │
└─────────────────────┘
          │ creates / uses
          ▼
┌─────────────────────┐
│      Memento        │
├─────────────────────┤
│ - state             │         ┌─────────────────────┐
│ + getState()        │◄────────│     Caretaker       │
└─────────────────────┘         ├─────────────────────┤
  (only Originator              │  - mementos[]       │
   can read state)              │  + addMemento()     │
                                │  + getMemento()     │
                                └─────────────────────┘
```

---

## Participants

| Participant    | Responsibility                                              |
| -------------- | ----------------------------------------------------------- |
| **Memento**    | Stores internal state of Originator                         |
| **Originator** | Creates memento with current state, uses memento to restore |
| **Caretaker**  | Keeps mementos but doesn't examine/operate on contents      |

---

## When to Use

| Scenario                          | Indicator                 |
| --------------------------------- | ------------------------- |
| Need to save/restore object state | YES - use memento         |
| Want to preserve encapsulation    | YES - state not exposed   |
| Implementing undo mechanism       | YES - store state history |
| Need checkpoints/snapshots        | YES - memento is snapshot |

---

## When NOT to Use

| Scenario                  | Why Not                     |
| ------------------------- | --------------------------- |
| State is too large        | Memory issues               |
| State easily serialized   | Simpler approaches exist    |
| No need for encapsulation | Direct state access simpler |

---

## Encapsulation Variants

| Variant               | Description                               |
| --------------------- | ----------------------------------------- |
| **Wide interface**    | Originator can access state (full access) |
| **Narrow interface**  | Caretaker sees limited interface          |
| **Immutable memento** | State cannot be modified after creation   |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Memento stores required state
[ ] Only Originator can read memento state
[ ] Caretaker stores but doesn't inspect mementos
[ ] Memento is immutable (preferred)
[ ] Memory impact considered
[ ] State capture is complete (deep copy if needed)
```

---

## Anti-Patterns to Detect

| Anti-Pattern        | Indication                  | Refactoring Action             |
| ------------------- | --------------------------- | ------------------------------ |
| **Exposed state**   | Caretaker reads state       | **Restrict access**            |
| **Mutable memento** | State changes after capture | **Make immutable**             |
| **Memory leak**     | Too many mementos stored    | **Implement cleanup strategy** |

---

## Related Patterns

| Pattern       | Relationship                   |
| ------------- | ------------------------------ |
| **Command**   | Commands use mementos for undo |
| **Iterator**  | Can capture iteration state    |
| **Prototype** | Alternative for shallow copies |

---

## References

- Source: [Refactoring.guru - Memento](https://refactoring.guru/design-patterns/memento)
- See also: [Command](behavioral-command.md)

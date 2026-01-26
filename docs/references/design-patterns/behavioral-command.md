# Command Pattern

Reference documentation for the Command behavioral design pattern.

---

## Overview

| Aspect            | Description                                                                                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Category**      | Behavioral                                                                                                                                                   |
| **Intent**        | Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations |
| **Also Known As** | Action, Transaction                                                                                                                                          |
| **Source**        | [Refactoring.guru - Command](https://refactoring.guru/design-patterns/command)                                                                               |

---

## Problem It Solves

| Problem                              | How Command Helps       |
| ------------------------------------ | ----------------------- |
| Parameterize objects with operations | Command as object       |
| Queue, schedule, or log operations   | Commands are data       |
| Support undo/redo                    | Commands store state    |
| Decouple invoker from operation      | Command is intermediary |

---

## Structure

```
┌─────────────────────┐
│       Client        │
└─────────┬───────────┘
          │ creates
          ▼
┌─────────────────────┐         ┌─────────────────────┐
│      Command        │         │      Invoker        │
│    (interface)      │◄────────│                     │
├─────────────────────┤         ├─────────────────────┤
│ + execute()         │         │ - command           │
│ + undo() (optional) │         │ + setCommand()      │
└──────────┬──────────┘         │ + executeCommand()  │
           │ implements         └─────────────────────┘
┌──────────┴──────────┐
│  ConcreteCommand    │
├─────────────────────┤         ┌─────────────────────┐
│ - receiver          │────────►│     Receiver        │
│ - params            │         ├─────────────────────┤
│ + execute()         │         │ + action()          │
│ + undo()            │         └─────────────────────┘
└─────────────────────┘
```

---

## Participants

| Participant         | Responsibility                                    |
| ------------------- | ------------------------------------------------- |
| **Command**         | Interface declaring execute (and optionally undo) |
| **ConcreteCommand** | Binds receiver and action, implements execute     |
| **Invoker**         | Asks command to carry out request                 |
| **Receiver**        | Knows how to perform operations                   |
| **Client**          | Creates ConcreteCommand and sets receiver         |

---

## When to Use

| Scenario                                   | Indicator                  |
| ------------------------------------------ | -------------------------- |
| Parameterize objects with operations       | YES - commands as objects  |
| Specify, queue, execute at different times | YES - deferred execution   |
| Support undo                               | YES - command stores state |
| Support transactions                       | YES - group commands       |
| Build macro commands                       | YES - composite commands   |

---

## When NOT to Use

| Scenario                            | Why Not                 |
| ----------------------------------- | ----------------------- |
| Simple, direct method calls         | Over-engineering        |
| No queuing, undo, or logging needed | Unnecessary abstraction |
| Single receiver, single operation   | Direct call simpler     |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Command interface defined
[ ] ConcreteCommand has receiver reference
[ ] ConcreteCommand stores parameters
[ ] Invoker holds command reference
[ ] Client creates and configures command
[ ] Undo supported (if required)
```

---

## Anti-Patterns to Detect

| Anti-Pattern       | Indication                            | Refactoring Action              |
| ------------------ | ------------------------------------- | ------------------------------- |
| **God command**    | Command does too much                 | **Split into smaller commands** |
| **Missing undo**   | Undo required but not implemented     | **Add state for undo**          |
| **Tight coupling** | Command knows too much about receiver | **Minimize coupling**           |

---

## Related Patterns

| Pattern                     | Relationship            |
| --------------------------- | ----------------------- |
| **Composite**               | Macro commands          |
| **Memento**                 | Keep state for undo     |
| **Prototype**               | Copy command to history |
| **Chain of Responsibility** | Handler as command      |

---

## References

- Source: [Refactoring.guru - Command](https://refactoring.guru/design-patterns/command)
- See also: [Memento](behavioral-memento.md), [Chain of Responsibility](behavioral-chain-of-responsibility.md)

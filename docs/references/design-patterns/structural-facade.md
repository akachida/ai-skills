# Facade Pattern

Reference documentation for the Facade structural design pattern.

---

## Overview

| Aspect       | Description                                                                                                                                       |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category** | Structural                                                                                                                                        |
| **Intent**   | Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use |
| **Source**   | [Refactoring.guru - Facade](https://refactoring.guru/design-patterns/facade)                                                                      |

---

## Problem It Solves

| Problem                              | How Facade Helps           |
| ------------------------------------ | -------------------------- |
| Complex subsystem with many classes  | Simplified interface       |
| Client needs subset of functionality | Exposes only what's needed |
| Tight coupling to subsystem          | Decouples client           |
| Subsystem changes break clients      | Facade absorbs changes     |

---

## Structure

```
┌─────────────────────┐
│      Client         │
└─────────┬───────────┘
          │ uses
          ▼
┌─────────────────────┐
│      Facade         │
├─────────────────────┤
│ + simpleOperation() │
└─────────┬───────────┘
          │ delegates to
          ▼
┌─────────────────────────────────────────┐
│              Subsystem                  │
│   ┌───────┐  ┌───────┐  ┌───────┐       │
│   │Class A│  │Class B│  │Class C│  ...  │
│   └───────┘  └───────┘  └───────┘       │
└─────────────────────────────────────────┘
```

---

## Participants

| Participant           | Responsibility                                           |
| --------------------- | -------------------------------------------------------- |
| **Facade**            | Knows which subsystem classes handle requests, delegates |
| **Subsystem classes** | Implement subsystem functionality, unaware of facade     |
| **Client**            | Uses facade instead of subsystem directly                |

---

## When to Use

| Scenario                                  | Indicator                   |
| ----------------------------------------- | --------------------------- |
| Complex subsystem needs simpler interface | YES - use facade            |
| Want to layer subsystems                  | YES - facade per layer      |
| Reduce dependencies on subsystem          | YES - depend on facade      |
| Provide entry point to subsystem          | YES - facade is entry point |

---

## When NOT to Use

| Scenario                           | Why Not                   |
| ---------------------------------- | ------------------------- |
| Subsystem is simple                | Unnecessary indirection   |
| Clients need full subsystem access | Facade limits access      |
| Facade becomes a god class         | Consider multiple facades |

---

## Facade vs Adapter

| Aspect            | Facade                     | Adapter                 |
| ----------------- | -------------------------- | ----------------------- |
| **Purpose**       | Simplify interface         | Convert interface       |
| **Scope**         | Entire subsystem           | Single class usually    |
| **Client access** | May still access subsystem | Works with adapter only |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Subsystem classes identified
[ ] Facade exposes simplified interface
[ ] Facade delegates to subsystem
[ ] Client uses facade (not subsystem directly)
[ ] Subsystem unaware of facade
[ ] Facade doesn't add new functionality (just orchestrates)
```

---

## Anti-Patterns to Detect

| Anti-Pattern          | Indication                | Refactoring Action              |
| --------------------- | ------------------------- | ------------------------------- |
| **God facade**        | Facade does too much      | **Split into multiple facades** |
| **Leaky facade**      | Subsystem details exposed | **Complete encapsulation**      |
| **Facade with logic** | Business logic in facade  | **Move to subsystem**           |

---

## Related Patterns

| Pattern              | Relationship                              |
| -------------------- | ----------------------------------------- |
| **Abstract Factory** | Alternative for hiding subsystem creation |
| **Mediator**         | Abstracts communication, not interface    |
| **Singleton**        | Facade often singleton                    |
| **Adapter**          | Different intent, similar structure       |

---

## References

- Source: [Refactoring.guru - Facade](https://refactoring.guru/design-patterns/facade)
- See also: [Adapter](structural-adapter.md), [Mediator](behavioral-mediator.md)

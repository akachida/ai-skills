# Chain of Responsibility Pattern

Reference documentation for the Chain of Responsibility behavioral design pattern.

---

## Overview

| Aspect            | Description                                                                                                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category**      | Behavioral                                                                                                                                                                                              |
| **Intent**        | Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along until an object handles it |
| **Also Known As** | CoR, Chain of Command                                                                                                                                                                                   |
| **Source**        | [Refactoring.guru - Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility)                                                                                          |

---

## Problem It Solves

| Problem                                   | How Chain of Responsibility Helps |
| ----------------------------------------- | --------------------------------- |
| Request needs multiple potential handlers | Chain provides options            |
| Handler not known at compile time         | Runtime resolution                |
| Want to decouple sender from receivers    | Sender knows only chain           |
| Processing order matters                  | Chain defines order               |

---

## Structure

```
┌─────────────────────┐
│       Client        │
└─────────┬───────────┘
          │ sends request
          ▼
┌─────────────────────┐
│      Handler        │
│     (interface)     │
├─────────────────────┤
│ + setNext(Handler)  │
│ + handle(request)   │
└──────────┬──────────┘
           │ implements
     ┌─────┴─────────────┐
     │                   │
┌────┴────────┐   ┌──────┴───────┐   ┌──────────┐
│ HandlerA    │──►│  HandlerB    │──►│ HandlerC │
├─────────────┤   ├──────────────┤   ├──────────┤
│ +handle()   │   │  +handle()   │   │+handle() │
└─────────────┘   └──────────────┘   └──────────┘
     │ can't handle,    │ handles or        │
     │ pass to next     │ passes            │
```

---

## Participants

| Participant         | Responsibility                                            |
| ------------------- | --------------------------------------------------------- |
| **Handler**         | Interface for handling requests, optional successor link  |
| **ConcreteHandler** | Handles requests it's responsible for, accesses successor |
| **Client**          | Initiates request to handler in chain                     |

---

## When to Use

| Scenario                                | Indicator                  |
| --------------------------------------- | -------------------------- |
| Multiple handlers for request           | YES - chain them           |
| Handler determined at runtime           | YES - dynamic resolution   |
| Want to send to one of several handlers | YES - chain finds one      |
| Processing pipeline                     | YES - chain defines stages |

---

## When NOT to Use

| Scenario                  | Why Not                |
| ------------------------- | ---------------------- |
| Single handler known      | Direct call is simpler |
| All handlers must process | Observer may be better |
| Order doesn't matter      | Strategy may suffice   |

---

## Implementation Variants

| Variant                 | Description                      |
| ----------------------- | -------------------------------- |
| **Handle or pass**      | Either handle completely or pass |
| **Handle and pass**     | Process then pass to next        |
| **Guaranteed handling** | Last handler always handles      |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Handler interface defined
[ ] Handlers link to successors
[ ] Client initiates to first handler
[ ] Each handler decides to handle or pass
[ ] Unhandled requests handled (default handler or exception)
[ ] Chain can be modified at runtime
```

---

## Anti-Patterns to Detect

| Anti-Pattern            | Indication                  | Refactoring Action              |
| ----------------------- | --------------------------- | ------------------------------- |
| **Broken chain**        | Request gets lost           | **Add default handler**         |
| **Circular chain**      | Infinite loop               | **Validate chain construction** |
| **All-knowing handler** | One handler does everything | **Distribute responsibilities** |

---

## Related Patterns

| Pattern       | Relationship                      |
| ------------- | --------------------------------- |
| **Composite** | Parent can act as successor       |
| **Command**   | Requests can be Command objects   |
| **Observer**  | Different: all observers notified |

---

## References

- Source: [Refactoring.guru - Chain of Responsibility](https://refactoring.guru/design-patterns/chain-of-responsibility)
- See also: [Command](behavioral-command.md), [Observer](behavioral-observer.md)

# State Pattern

Reference documentation for the State behavioral design pattern.

---

## Overview

| Aspect            | Description                                                                                                       |
| ----------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Category**      | Behavioral                                                                                                        |
| **Intent**        | Allow an object to alter its behavior when its internal state changes. The object will appear to change its class |
| **Also Known As** | Objects for States                                                                                                |
| **Source**        | [Refactoring.guru - State](https://refactoring.guru/design-patterns/state)                                        |

---

## Problem It Solves

| Problem                           | How State Helps                      |
| --------------------------------- | ------------------------------------ |
| Behavior depends on state         | Encapsulates state-specific behavior |
| Large conditionals based on state | Replaces with polymorphism           |
| State transitions are complex     | Explicit in state classes            |
| Adding new states is difficult    | Add new state class                  |

---

## Structure

```
┌─────────────────────┐
│      Context        │
├─────────────────────┤         ┌─────────────────────┐
│ - state: State      │────────►│       State         │
│ + request()         │         │    (interface)      │
│ + changeState()     │         ├─────────────────────┤
└─────────────────────┘         │ + handle(Context)   │
                                └──────────┬──────────┘
                                           │ implements
                         ┌─────────────────┼─────────────────┐
                         │                 │                 │
                ┌────────┴──────┐  ┌───────┴───────┐  ┌──────┴───────┐
                │ ConcreteStateA│  │ConcreteStateB │  │ConcreteStateC│
                ├───────────────┤  ├───────────────┤  ├──────────────┤
                │ +handle()     │  │  +handle()    │  │  +handle()   │
                └───────────────┘  └───────────────┘  └──────────────┘
```

---

## Participants

| Participant       | Responsibility                              |
| ----------------- | ------------------------------------------- |
| **Context**       | Maintains current state, delegates to state |
| **State**         | Interface for state-specific behavior       |
| **ConcreteState** | Implements behavior for specific state      |

---

## When to Use

| Scenario                          | Indicator                        |
| --------------------------------- | -------------------------------- |
| Object behavior varies with state | YES - use state                  |
| Many conditionals depend on state | YES - replace with state classes |
| State-specific behavior scattered | YES - consolidate in state       |
| State transitions well-defined    | YES - explicit transitions       |

---

## When NOT to Use

| Scenario                   | Why Not                     |
| -------------------------- | --------------------------- |
| Few states, simple logic   | Conditionals may be clearer |
| States rarely change       | Over-engineering            |
| No state-specific behavior | No benefit                  |

---

## State vs Strategy

| Aspect          | State                               | Strategy                   |
| --------------- | ----------------------------------- | -------------------------- |
| **Intent**      | Object changes behavior             | Client chooses algorithm   |
| **Transitions** | State objects may switch themselves | Client switches strategies |
| **Awareness**   | States may know about each other    | Strategies independent     |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] State interface defined
[ ] ConcreteState for each state
[ ] Context delegates to current state
[ ] State transitions handled (Context or State)
[ ] Initial state set
[ ] All states implement all methods (or use default)
```

---

## Transition Management

| Approach            | Description           | When to Use                   |
| ------------------- | --------------------- | ----------------------------- |
| **Context decides** | Context changes state | Centralized control needed    |
| **State decides**   | State changes itself  | States know valid transitions |

---

## Anti-Patterns to Detect

| Anti-Pattern               | Indication              | Refactoring Action            |
| -------------------------- | ----------------------- | ----------------------------- |
| **Incomplete transitions** | Some transitions missed | **Map all valid transitions** |
| **God state**              | One state does too much | **Split state**               |
| **State explosion**        | Too many states         | **Reconsider abstraction**    |

---

## Related Patterns

| Pattern       | Relationship                        |
| ------------- | ----------------------------------- |
| **Strategy**  | Similar structure, different intent |
| **Singleton** | States often singletons             |
| **Flyweight** | Share state objects                 |

---

## References

- Source: [Refactoring.guru - State](https://refactoring.guru/design-patterns/state)
- See also: [Strategy](behavioral-strategy.md)

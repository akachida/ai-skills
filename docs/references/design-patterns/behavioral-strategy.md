# Strategy Pattern

Reference documentation for the Strategy behavioral design pattern.

---

## Overview

| Aspect            | Description                                                                                                                                                 |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category**      | Behavioral                                                                                                                                                  |
| **Intent**        | Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it |
| **Also Known As** | Policy                                                                                                                                                      |
| **Source**        | [Refactoring.guru - Strategy](https://refactoring.guru/design-patterns/strategy)                                                                            |

---

## Problem It Solves

| Problem                              | How Strategy Helps          |
| ------------------------------------ | --------------------------- |
| Multiple algorithms for same purpose | Encapsulates each algorithm |
| Algorithm selection at runtime       | Interchangeable strategies  |
| Conditionals for algorithm selection | Replace with polymorphism   |
| Algorithm changes independently      | Isolated in strategy        |

---

## Structure

```
┌─────────────────────┐
│      Context        │
├─────────────────────┤         ┌─────────────────────┐
│ - strategy: Strategy│────────►│     Strategy        │
│ + setStrategy()     │         │    (interface)      │
│ + executeStrategy() │         ├─────────────────────┤
└─────────────────────┘         │ + execute()         │
                                └──────────┬──────────┘
                                           │ implements
                         ┌─────────────────┼─────────────────┐
                         │                 │                 │
                ┌────────┴──────┐  ┌───────┴───────┐  ┌──────┴───────┐
                │ StrategyA     │  │  StrategyB    │  │  StrategyC   │
                ├───────────────┤  ├───────────────┤  ├──────────────┤
                │ +execute()    │  │  +execute()   │  │  +execute()  │
                └───────────────┘  └───────────────┘  └──────────────┘
```

---

## Participants

| Participant          | Responsibility                                      |
| -------------------- | --------------------------------------------------- |
| **Strategy**         | Interface common to all algorithms                  |
| **ConcreteStrategy** | Implements specific algorithm                       |
| **Context**          | Maintains strategy reference, delegates to strategy |

---

## When to Use

| Scenario                              | Indicator                     |
| ------------------------------------- | ----------------------------- |
| Multiple algorithms for same behavior | YES - use strategy            |
| Need to switch algorithms at runtime  | YES - interchangeable         |
| Algorithm has multiple variations     | YES - separate strategies     |
| Want to isolate algorithm details     | YES - encapsulate in strategy |

---

## When NOT to Use

| Scenario                | Why Not                     |
| ----------------------- | --------------------------- |
| Single algorithm        | No variation to encapsulate |
| Algorithm never changes | Unnecessary indirection     |
| Few, simple algorithms  | Conditionals may be clearer |

---

## Strategy vs State

| Aspect        | Strategy               | State                      |
| ------------- | ---------------------- | -------------------------- |
| **Purpose**   | Select algorithm       | Change behavior with state |
| **Switching** | Client explicitly sets | State may switch itself    |
| **Awareness** | Strategies independent | States may know each other |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Strategy interface defined
[ ] ConcreteStrategies implement interface
[ ] Context holds strategy reference
[ ] Context delegates to strategy
[ ] Strategy can be changed at runtime
[ ] Strategies are interchangeable (same interface)
```

---

## Anti-Patterns to Detect

| Anti-Pattern            | Indication                             | Refactoring Action            |
| ----------------------- | -------------------------------------- | ----------------------------- |
| **Strategy awareness**  | Strategy knows about context internals | **Pass required data only**   |
| **Too many strategies** | Strategy for every variation           | **Consider parameterization** |
| **Default in context**  | Context has default algorithm          | **Create default strategy**   |

---

## Related Patterns

| Pattern             | Relationship                           |
| ------------------- | -------------------------------------- |
| **State**           | Similar structure, different intent    |
| **Flyweight**       | Strategies can be flyweights           |
| **Template Method** | Alternative: inheritance-based         |
| **Decorator**       | Strategy changes core, decorator wraps |

---

## References

- Source: [Refactoring.guru - Strategy](https://refactoring.guru/design-patterns/strategy)
- See also: [State](behavioral-state.md), [Template Method](behavioral-template-method.md)

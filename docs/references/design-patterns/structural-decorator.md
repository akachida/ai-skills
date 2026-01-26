# Decorator Pattern

Reference documentation for the Decorator structural design pattern.

---

## Overview

| Aspect            | Description                                                                                                                                       |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category**      | Structural                                                                                                                                        |
| **Intent**        | Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality |
| **Also Known As** | Wrapper                                                                                                                                           |
| **Source**        | [Refactoring.guru - Decorator](https://refactoring.guru/design-patterns/decorator)                                                                |

---

## Problem It Solves

| Problem                         | How Decorator Helps          |
| ------------------------------- | ---------------------------- |
| Need to add behavior at runtime | Wraps dynamically            |
| Subclassing explosion           | Combine decorators instead   |
| Single Responsibility Principle | Each decorator = one concern |
| Cannot extend (final class)     | Wrap instead of extend       |

---

## Structure

```
┌──────────────────────────┐
│     Component            │
│     (interface)          │
├──────────────────────────┤
│ + operation()            │
└──────────────┬───────────┘
               │ implements
     ┌─────────┴─────────────┐
     │                       │
┌────┴────────┐    ┌─────────┴───────┐
│ Concrete    │    │    Decorator    │
│ Component   │    │    (abstract)   │
├─────────────┤    ├─────────────────┤
│ +operation()│    │ - wrapped       │───┐
└─────────────┘    │ +operation()    │   │
                   └────────┬────────┘   │
                            │            │
                 ┌──────────┴───────┐    │ wraps Component
                 │                  │    │
           ┌─────┴──────┐    ┌──────┴────┴┐
           │ DecoratorA │    │ DecoratorB │
           ├────────────┤    ├────────────┤
           │+operation()│    │+operation()│
           └────────────┘    └────────────┘
```

---

## Participants

| Participant           | Responsibility                                             |
| --------------------- | ---------------------------------------------------------- |
| **Component**         | Interface for objects that can have responsibilities added |
| **ConcreteComponent** | Object to which responsibilities are added                 |
| **Decorator**         | Maintains reference to Component, conforms to interface    |
| **ConcreteDecorator** | Adds responsibilities to component                         |

---

## When to Use

| Scenario                             | Indicator                   |
| ------------------------------------ | --------------------------- |
| Add responsibilities dynamically     | YES - use decorator         |
| Responsibilities can be withdrawn    | YES - unwrap decorator      |
| Extension by subclassing impractical | YES - too many combinations |
| Want to combine behaviors            | YES - stack decorators      |

---

## When NOT to Use

| Scenario                               | Why Not                    |
| -------------------------------------- | -------------------------- |
| Fixed set of extensions needed         | Subclassing may be clearer |
| Order of decoration matters critically | Can be confusing           |
| Simple, single extension               | Direct subclass is simpler |

---

## Decorator vs Inheritance

| Aspect          | Decorator        | Inheritance             |
| --------------- | ---------------- | ----------------------- |
| **Binding**     | Runtime          | Compile time            |
| **Combination** | Stack multiple   | Combinatorial explosion |
| **Flexibility** | High             | Low                     |
| **Complexity**  | Can be confusing | More straightforward    |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Component interface defined
[ ] ConcreteComponent implements Component
[ ] Decorator holds reference to Component
[ ] Decorator forwards operations to wrapped
[ ] ConcreteDecorators add behavior before/after forwarding
[ ] Client can stack decorators
[ ] Decorators don't break Component contract
```

---

## Anti-Patterns to Detect

| Anti-Pattern                    | Indication                  | Refactoring Action              |
| ------------------------------- | --------------------------- | ------------------------------- |
| **Order-dependent decorators**  | Behavior changes with order | **Document order or redesign**  |
| **Too many decorators**         | Excessive wrapping          | **Consider facade or simplify** |
| **Decorator changing contract** | Breaks Liskov               | **Maintain interface contract** |

---

## Related Patterns

| Pattern       | Relationship                      |
| ------------- | --------------------------------- |
| **Adapter**   | Wrapper with different intent     |
| **Composite** | Decorator is degenerate composite |
| **Strategy**  | Changes internals vs wrapper      |
| **Proxy**     | Same interface, controls access   |

---

## References

- Source: [Refactoring.guru - Decorator](https://refactoring.guru/design-patterns/decorator)
- See also: [Adapter](structural-adapter.md), [Proxy](structural-proxy.md)

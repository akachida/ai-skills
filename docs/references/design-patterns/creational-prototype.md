# Prototype Pattern

Reference documentation for the Prototype creational design pattern.

---

## Overview

| Aspect            | Description                                                                                                            |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Category**      | Creational                                                                                                             |
| **Intent**        | Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype |
| **Also Known As** | Clone                                                                                                                  |
| **Source**        | [Refactoring.guru - Prototype](https://refactoring.guru/design-patterns/prototype)                                     |

---

## Problem It Solves

| Problem                       | How Prototype Helps              |
| ----------------------------- | -------------------------------- |
| Costly object creation        | Clone instead of create          |
| Need exact copy of object     | Deep/shallow copy support        |
| Avoid subclasses for creation | Clone existing configured object |
| Unknown concrete types        | Clone interface hides types      |

---

## Structure

```
┌──────────────────────────────────┐
│      Prototype (interface)       │
├──────────────────────────────────┤
│ + clone(): Prototype             │
└───────────┬──────────────────────┘
            │ implements
┌───────────┴──────────────────────┐
│     ConcretePrototype            │
├──────────────────────────────────┤
│ - field1                         │
│ - field2                         │
│ + clone(): Prototype             │ ◄── returns copy of self
└──────────────────────────────────┘

Optional Registry:
┌──────────────────────────────────┐
│      PrototypeRegistry           │
├──────────────────────────────────┤
│ - prototypes: Map<String,Proto>  │
│ + register(name, proto)          │
│ + get(name): Prototype           │ ◄── returns clone
└──────────────────────────────────┘
```

---

## Participants

| Participant             | Responsibility                                  |
| ----------------------- | ----------------------------------------------- |
| **Prototype**           | Interface declaring clone method                |
| **ConcretePrototype**   | Implements clone operation                      |
| **Client**              | Creates new object by asking prototype to clone |
| **Registry** (optional) | Stores and retrieves prototypes by name         |

---

## Clone Types

| Type              | Description                                | When to Use                          |
| ----------------- | ------------------------------------------ | ------------------------------------ |
| **Shallow Clone** | Copies primitive fields, references shared | Simple objects, immutable references |
| **Deep Clone**    | Copies everything recursively              | Objects with mutable nested objects  |

---

## When to Use

| Scenario                             | Indicator                       |
| ------------------------------------ | ------------------------------- |
| Object creation is expensive         | YES - clone pre-configured      |
| Need copy of complex object          | YES - prototype handles copying |
| System independent of concrete types | YES - clone via interface       |
| Limited number of state combinations | YES - prototype registry        |

---

## When NOT to Use

| Scenario                         | Why Not                   |
| -------------------------------- | ------------------------- |
| Simple object construction       | Unnecessary complexity    |
| Objects have circular references | Deep clone problematic    |
| Copy semantics unclear           | Better alternatives exist |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Prototype interface declares clone()
[ ] ConcretePrototype implements clone correctly
[ ] Shallow vs deep clone decision documented
[ ] Circular references handled (if any)
[ ] Clone returns new independent instance
[ ] Private fields properly copied
```

---

## Anti-Patterns to Detect

| Anti-Pattern                  | Indication               | Refactoring Action           |
| ----------------------------- | ------------------------ | ---------------------------- |
| **Incomplete clone**          | Some fields not copied   | **Clone all relevant state** |
| **Shared mutable references** | Clones affect each other | **Use deep clone**           |
| **Clone exposes internals**   | Returns internal objects | **Clone nested objects too** |

---

## Related Patterns

| Pattern              | Relationship                                  |
| -------------------- | --------------------------------------------- |
| **Abstract Factory** | Can use prototypes instead of factory methods |
| **Composite**        | Often used with Decorator                     |
| **Memento**          | Can use prototype to store state              |

---

## References

- Source: [Refactoring.guru - Prototype](https://refactoring.guru/design-patterns/prototype)
- See also: [Factory Method](creational-factory-method.md)

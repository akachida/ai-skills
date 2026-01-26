# Adapter Pattern

Reference documentation for the Adapter structural design pattern.

---

## Overview

| Aspect            | Description                                                                                                                                                       |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category**      | Structural                                                                                                                                                        |
| **Intent**        | Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise due to incompatible interfaces |
| **Also Known As** | Wrapper                                                                                                                                                           |
| **Source**        | [Refactoring.guru - Adapter](https://refactoring.guru/design-patterns/adapter)                                                                                    |

---

## Problem It Solves

| Problem                         | How Adapter Helps             |
| ------------------------------- | ----------------------------- |
| Incompatible interfaces         | Translates between interfaces |
| Cannot modify existing class    | Wraps without changing        |
| Legacy system integration       | Adapts old interface to new   |
| Third-party library integration | Adapts external to internal   |

---

## Structure

### Object Adapter (Composition)

```
┌──────────────────┐      ┌──────────────────┐
│     Client       │──────│   Target         │
└──────────────────┘      │   Interface      │
                          ├──────────────────┤
                          │ + request()      │
                          └────────┬─────────┘
                                   │ implements
                          ┌────────┴─────────┐
                          │     Adapter      │
                          ├──────────────────┤
                          │ - adaptee        │───┐
                          │ + request()      │   │
                          └──────────────────┘   │
                                                 │ delegates to
                          ┌──────────────────┐   │
                          │     Adaptee      │◄──┘
                          ├──────────────────┤
                          │ + specificReq()  │
                          └──────────────────┘
```

### Class Adapter (Inheritance)

```
┌──────────────────┐      ┌──────────────────┐
│     Target       │      │     Adaptee      │
├──────────────────┤      ├──────────────────┤
│ + request()      │      │ + specificReq()  │
└────────┬─────────┘      └────────┬─────────┘
         │ implements              │ extends
         └──────────┬──────────────┘
                    │
           ┌────────┴─────────┐
           │     Adapter      │
           ├──────────────────┤
           │ + request()      │ ◄── calls specificRequest()
           └──────────────────┘
```

---

## Participants

| Participant | Responsibility                        |
| ----------- | ------------------------------------- |
| **Target**  | Interface client expects              |
| **Adaptee** | Existing interface needing adaptation |
| **Adapter** | Adapts Adaptee to Target interface    |
| **Client**  | Works with Target interface           |

---

## When to Use

| Scenario                                               | Indicator                   |
| ------------------------------------------------------ | --------------------------- |
| Want to use existing class with incompatible interface | YES - use adapter           |
| Need to reuse existing subclasses                      | YES - object adapter        |
| Integration with third-party or legacy code            | YES - isolates changes      |
| Need to translate data formats                         | YES - adapter can transform |

---

## When NOT to Use

| Scenario                      | Why Not                         |
| ----------------------------- | ------------------------------- |
| Can modify the original class | Direct change is simpler        |
| Creating many small adapters  | Consider refactoring interfaces |
| Simple type conversion        | Direct conversion is clearer    |

---

## Object vs Class Adapter

| Aspect          | Object Adapter          | Class Adapter                 |
| --------------- | ----------------------- | ----------------------------- |
| **Mechanism**   | Composition             | Multiple inheritance          |
| **Flexibility** | Can adapt subclasses    | Adapts specific class         |
| **Override**    | Cannot override adaptee | Can override adaptee          |
| **Language**    | Any language            | Requires multiple inheritance |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Target interface defined (what client expects)
[ ] Adaptee identified (what needs adapting)
[ ] Adapter implements Target interface
[ ] Adapter delegates to Adaptee
[ ] Client uses only Target interface
[ ] Data transformation handled (if needed)
```

---

## Anti-Patterns to Detect

| Anti-Pattern        | Indication                    | Refactoring Action             |
| ------------------- | ----------------------------- | ------------------------------ |
| **Adapter chain**   | Multiple adapters in sequence | **Create direct adapter**      |
| **Leaky adapter**   | Adaptee details exposed       | **Complete encapsulation**     |
| **Two-way adapter** | Adapts in both directions     | **Consider separate adapters** |

---

## Related Patterns

| Pattern       | Relationship                        |
| ------------- | ----------------------------------- |
| **Bridge**    | Similar structure, different intent |
| **Decorator** | Wrapper that adds behavior          |
| **Facade**    | Simplifies interface, may adapt     |
| **Proxy**     | Same interface, different purpose   |

---

## References

- Source: [Refactoring.guru - Adapter](https://refactoring.guru/design-patterns/adapter)
- See also: [Bridge](structural-bridge.md), [Facade](structural-facade.md)

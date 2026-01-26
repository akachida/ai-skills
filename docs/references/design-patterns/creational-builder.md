# Builder Pattern

Reference documentation for the Builder creational design pattern.

---

## Overview

| Aspect       | Description                                                                                                                                       |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Category** | Creational                                                                                                                                        |
| **Intent**   | Separate the construction of a complex object from its representation, allowing the same construction process to create different representations |
| **Source**   | [Refactoring.guru - Builder](https://refactoring.guru/design-patterns/builder)                                                                    |

---

## Problem It Solves

| Problem                          | How Builder Helps                |
| -------------------------------- | -------------------------------- |
| Constructor with many parameters | Step-by-step construction        |
| Complex object creation          | Encapsulates construction logic  |
| Need different representations   | Same process, different builders |
| Telescoping constructors         | Named methods instead            |

---

## Structure

```
┌──────────────────────────────────┐
│          Director                │
├──────────────────────────────────┤
│ - builder: Builder               │
│ + construct(): void              │ ◄── orchestrates building steps
└───────────┬──────────────────────┘
            │ uses
            ▼
┌──────────────────────────────────┐
│      Builder (interface)         │
├──────────────────────────────────┤
│ + buildPartA(): void             │
│ + buildPartB(): void             │
│ + getResult(): Product           │
└───────────┬──────────────────────┘
            │ implements
┌───────────┴──────────────────────┐
│     ConcreteBuilder              │
├──────────────────────────────────┤
│ - product: Product               │
│ + buildPartA(): void             │
│ + buildPartB(): void             │
│ + getResult(): Product           │
└──────────────────────────────────┘
```

---

## Participants

| Participant         | Responsibility                       |
| ------------------- | ------------------------------------ |
| **Builder**         | Interface for creating product parts |
| **ConcreteBuilder** | Constructs and assembles parts       |
| **Director**        | Constructs using Builder interface   |
| **Product**         | Complex object being built           |

---

## When to Use

| Scenario                                     | Indicator               |
| -------------------------------------------- | ----------------------- |
| Complex object with many parts               | YES - use builder       |
| Same construction, different representations | YES - multiple builders |
| Need to construct object step-by-step        | YES - builder methods   |
| Want immutable object with many fields       | YES - fluent builder    |

---

## When NOT to Use

| Scenario                      | Why Not                 |
| ----------------------------- | ----------------------- |
| Simple object (few fields)    | Constructor sufficient  |
| No construction variations    | Unnecessary abstraction |
| Object mutates after creation | Builder not needed      |

---

## Implementation Variants

### Classic Builder (with Director)

| Component    | Purpose                       |
| ------------ | ----------------------------- |
| **Director** | Knows construction order      |
| **Builder**  | Provides construction methods |
| **Product**  | Result of construction        |

### Fluent Builder (common in modern code)

```
Product = new ProductBuilder()
    .withPartA(valueA)
    .withPartB(valueB)
    .withPartC(valueC)
    .build();
```

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Builder interface defines all construction steps
[ ] ConcreteBuilder tracks partially built product
[ ] getResult() returns completed product
[ ] Director (if used) defines construction sequence
[ ] Product can be complex with multiple parts
[ ] Builder can be reset for new product
```

---

## Anti-Patterns to Detect

| Anti-Pattern                | Indication                     | Refactoring Action              |
| --------------------------- | ------------------------------ | ------------------------------- |
| **Incomplete build**        | Product usable before complete | **Require build() call**        |
| **Mutable builder result**  | Product changes after build    | **Return copy or new instance** |
| **Too many optional steps** | Most steps unused              | **Consider defaults or split**  |

---

## Related Patterns

| Pattern              | Relationship                    |
| -------------------- | ------------------------------- |
| **Abstract Factory** | Can use builders internally     |
| **Composite**        | Builder often builds composites |
| **Singleton**        | Director may be singleton       |

---

## References

- Source: [Refactoring.guru - Builder](https://refactoring.guru/design-patterns/builder)
- See also: [Factory Method](creational-factory-method.md)

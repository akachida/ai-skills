# Abstract Factory Pattern

Reference documentation for the Abstract Factory creational design pattern.

---

## Overview

| Aspect            | Description                                                                                                          |
| ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Category**      | Creational                                                                                                           |
| **Intent**        | Provide an interface for creating families of related or dependent objects without specifying their concrete classes |
| **Also Known As** | Kit                                                                                                                  |
| **Source**        | [Refactoring.guru - Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory)                     |

---

## Problem It Solves

| Problem                                | How Abstract Factory Helps      |
| -------------------------------------- | ------------------------------- |
| Need families of related objects       | Creates entire product families |
| Products must be used together         | Ensures compatibility           |
| System independent of product creation | Hides concrete classes          |
| Need to swap product families          | Change factory, not code        |

---

## Structure

```
┌─────────────────────────────────┐
│    AbstractFactory              │
├─────────────────────────────────┤
│ + createProductA(): ProductA    │
│ + createProductB(): ProductB    │
└───────────┬─────────────────────┘
            │ implements
    ┌───────┴───────┐
    │               │
┌───┴───────┐  ┌────┴──────┐
│ Factory1  │  │ Factory2  │
│           │  │           │
│ creates   │  │ creates   │
│ ProductA1 │  │ ProductA2 │
│ ProductB1 │  │ ProductB2 │
└───────────┘  └───────────┘

Product Hierarchies:
┌──────────┐    ┌──────────┐
│ ProductA │    │ ProductB │
└────┬─────┘    └────┬─────┘
  ┌──┴──┐         ┌──┴──┐
  │     │         │     │
 A1    A2        B1    B2
```

---

## Participants

| Participant         | Responsibility                          |
| ------------------- | --------------------------------------- |
| **AbstractFactory** | Interface for creating product families |
| **ConcreteFactory** | Implements creation for specific family |
| **AbstractProduct** | Interface for product type              |
| **ConcreteProduct** | Implements product for specific family  |
| **Client**          | Uses only abstract interfaces           |

---

## When to Use

| Scenario                               | Indicator                        |
| -------------------------------------- | -------------------------------- |
| System needs multiple product families | YES - use abstract factory       |
| Products designed to work together     | YES - ensures compatibility      |
| Need to swap implementations           | YES - change factory only        |
| Platform/theme variations              | YES - each platform is a factory |

---

## When NOT to Use

| Scenario              | Why Not                    |
| --------------------- | -------------------------- |
| Single product type   | Use Factory Method instead |
| Products not related  | No benefit                 |
| Families never change | Unnecessary complexity     |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] AbstractFactory declares methods for ALL products in family
[ ] Each ConcreteFactory creates compatible products
[ ] Products from same factory work together
[ ] Client depends only on abstract interfaces
[ ] Adding new family = new ConcreteFactory only
[ ] Adding new product type = changes all factories (consider carefully)
```

---

## Anti-Patterns to Detect

| Anti-Pattern                | Indication                            | Refactoring Action            |
| --------------------------- | ------------------------------------- | ----------------------------- |
| **Partial family creation** | Factory creates only some products    | **Complete the family**       |
| **Mixed families**          | Factory creates incompatible products | **Ensure family consistency** |
| **Excessive factories**     | Factory per product variant           | **Group into families**       |

---

## Related Patterns

| Pattern            | Relationship                                |
| ------------------ | ------------------------------------------- |
| **Factory Method** | Abstract Factory often uses factory methods |
| **Singleton**      | Factories often implemented as singletons   |
| **Prototype**      | Alternative implementation approach         |

---

## References

- Source: [Refactoring.guru - Abstract Factory](https://refactoring.guru/design-patterns/abstract-factory)
- See also: [Factory Method](creational-factory-method.md)

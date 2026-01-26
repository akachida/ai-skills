# Singleton Pattern

Reference documentation for the Singleton creational design pattern.

---

## Overview

| Aspect       | Description                                                                        |
| ------------ | ---------------------------------------------------------------------------------- |
| **Category** | Creational                                                                         |
| **Intent**   | Ensure a class has only one instance and provide a global point of access to it    |
| **Source**   | [Refactoring.guru - Singleton](https://refactoring.guru/design-patterns/singleton) |

---

## Problem It Solves

| Problem                    | How Singleton Helps      |
| -------------------------- | ------------------------ |
| Need exactly one instance  | Enforces single instance |
| Global access point needed | Provides static accessor |
| Shared resource management | Centralizes access       |
| Lazy initialization        | Creates on first access  |

---

## Structure

```
┌──────────────────────────────────┐
│         Singleton                │
├──────────────────────────────────┤
│ - instance: Singleton (static)   │
│ - Singleton() (private)          │
├──────────────────────────────────┤
│ + getInstance(): Singleton       │ ◄── static, returns single instance
│ + businessLogic()                │
└──────────────────────────────────┘
```

---

## Participants

| Participant   | Responsibility                                   |
| ------------- | ------------------------------------------------ |
| **Singleton** | Defines getInstance() for single instance access |

---

## Implementation Variants

| Variant                      | Thread Safety | Lazy | Complexity |
| ---------------------------- | ------------- | ---- | ---------- |
| **Eager**                    | Yes           | No   | Low        |
| **Lazy (not thread-safe)**   | No            | Yes  | Low        |
| **Synchronized**             | Yes           | Yes  | Medium     |
| **Double-checked locking**   | Yes           | Yes  | Medium     |
| **Holder idiom**             | Yes           | Yes  | Low        |
| **Enum** (language specific) | Yes           | No   | Low        |

---

## When to Use

| Scenario                             | Indicator                            |
| ------------------------------------ | ------------------------------------ |
| Exactly one instance required        | YES - use singleton                  |
| Global access point needed           | CONSIDER - may indicate design issue |
| Shared resource (DB connection pool) | YES - centralized management         |
| Configuration/settings               | YES - single source of truth         |

---

## When NOT to Use (CRITICAL)

**Singleton is often overused. Verify necessity.**

| Scenario                                 | Why Not                           | Alternative              |
| ---------------------------------------- | --------------------------------- | ------------------------ |
| Just for convenience                     | Creates hidden dependency         | **Dependency injection** |
| Testing difficulties                     | Hard to mock                      | **Dependency injection** |
| Multiple instances might be needed later | Locks in constraint               | **Factory**              |
| State is causing issues                  | Global mutable state is dangerous | **Stateless service**    |

---

## Common Problems

| Problem                 | Description                          | Mitigation                     |
| ----------------------- | ------------------------------------ | ------------------------------ |
| **Hidden dependencies** | Code depends on singleton secretly   | Use DI instead                 |
| **Testing difficulty**  | Cannot substitute for tests          | Inject as dependency           |
| **Thread safety**       | Race conditions on first access      | Use thread-safe initialization |
| **Serialization**       | Deserialization creates new instance | Implement readResolve()        |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Single instance is genuinely required (not just convenient)
[ ] Thread safety addressed if multi-threaded
[ ] Testing strategy documented (how to mock?)
[ ] No alternative (DI) is appropriate
[ ] Instance creation is idempotent
[ ] Serialization/cloning won't create duplicates
```

---

## Anti-Patterns to Detect

| Anti-Pattern                | Indication                                | Refactoring Action            |
| --------------------------- | ----------------------------------------- | ----------------------------- |
| **Singleton abuse**         | Multiple singletons, complex dependencies | **Use dependency injection**  |
| **Singleton for testing**   | Tests need different instances            | **Inject dependency instead** |
| **God singleton**           | Singleton does too much                   | **Split responsibilities**    |
| **Mutable singleton state** | State changes cause issues                | **Consider stateless design** |

---

## Singleton vs Dependency Injection

| Aspect           | Singleton       | Dependency Injection  |
| ---------------- | --------------- | --------------------- |
| **Dependencies** | Hidden          | Explicit              |
| **Testing**      | Difficult       | Easy (mock injection) |
| **Flexibility**  | Low (hardcoded) | High (configurable)   |
| **Coupling**     | Tight           | Loose                 |

**RECOMMENDATION:** Prefer Dependency Injection over Singleton for most cases.

---

## Related Patterns

| Pattern              | Relationship                   |
| -------------------- | ------------------------------ |
| **Abstract Factory** | Factory may be singleton       |
| **Builder**          | Builder may be singleton       |
| **Facade**           | Facade often singleton         |
| **State**            | State objects often singletons |

---

## References

- Source: [Refactoring.guru - Singleton](https://refactoring.guru/design-patterns/singleton)
- Note: Consider Dependency Injection as alternative

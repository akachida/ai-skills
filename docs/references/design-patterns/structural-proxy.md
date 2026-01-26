# Proxy Pattern

Reference documentation for the Proxy structural design pattern.

---

## Overview

| Aspect            | Description                                                                   |
| ----------------- | ----------------------------------------------------------------------------- |
| **Category**      | Structural                                                                    |
| **Intent**        | Provide a surrogate or placeholder for another object to control access to it |
| **Also Known As** | Surrogate                                                                     |
| **Source**        | [Refactoring.guru - Proxy](https://refactoring.guru/design-patterns/proxy)    |

---

## Problem It Solves

| Problem                          | How Proxy Helps           |
| -------------------------------- | ------------------------- |
| Need to control access to object | Proxy intercepts requests |
| Expensive object creation        | Lazy initialization       |
| Remote object access             | Location transparency     |
| Access control/logging           | Transparent interception  |

---

## Structure

```
┌─────────────────────┐
│      Client         │
└─────────┬───────────┘
          │ uses
          ▼
┌─────────────────────┐
│      Subject        │
│     (interface)     │
├─────────────────────┤
│ + request()         │
└───────────┬─────────┘
            │ implements
      ┌─────┴─────┐
      │           │
┌─────┴────┐ ┌────┴────────┐
│   Real   │ │    Proxy    │
│  Subject │ ├─────────────┤
├──────────┤ │-realSubject │───► RealSubject
│+request()│ │+request()   │     (delegates)
└──────────┘ └─────────────┘
```

---

## Proxy Types

| Type                 | Purpose                        | Use Case                  |
| -------------------- | ------------------------------ | ------------------------- |
| **Virtual Proxy**    | Lazy initialization            | Expensive object creation |
| **Protection Proxy** | Access control                 | Security/permissions      |
| **Remote Proxy**     | Local representation of remote | Distributed systems       |
| **Logging Proxy**    | Log requests                   | Debugging/auditing        |
| **Caching Proxy**    | Cache results                  | Performance               |
| **Smart Reference**  | Additional actions on access   | Reference counting        |

---

## Participants

| Participant     | Responsibility                                   |
| --------------- | ------------------------------------------------ |
| **Subject**     | Common interface for RealSubject and Proxy       |
| **RealSubject** | Real object the proxy represents                 |
| **Proxy**       | Maintains reference, controls access, may create |

---

## When to Use

| Scenario                         | Proxy Type       |
| -------------------------------- | ---------------- |
| Lazy loading of expensive object | Virtual Proxy    |
| Access control needed            | Protection Proxy |
| Object is remote                 | Remote Proxy     |
| Need to log/audit access         | Logging Proxy    |
| Cache results                    | Caching Proxy    |

---

## When NOT to Use

| Scenario                                 | Why Not                 |
| ---------------------------------------- | ----------------------- |
| Simple direct access sufficient          | Unnecessary indirection |
| No access control/lazy loading needed    | No benefit              |
| Performance critical, no caching benefit | Overhead                |

---

## Proxy vs Decorator

| Aspect        | Proxy                | Decorator      |
| ------------- | -------------------- | -------------- |
| **Purpose**   | Control access       | Add behavior   |
| **Lifecycle** | May control creation | Wraps existing |
| **Identity**  | Same interface       | Same interface |
| **Intent**    | Access control       | Responsibility |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Subject interface defined
[ ] Proxy implements Subject interface
[ ] Proxy holds reference to RealSubject
[ ] Proxy controls access (lazy init, security, etc.)
[ ] Client unaware of proxy vs real subject
[ ] Proxy type chosen (virtual, protection, etc.)
```

---

## Anti-Patterns to Detect

| Anti-Pattern      | Indication               | Refactoring Action      |
| ----------------- | ------------------------ | ----------------------- |
| **Heavy proxy**   | Too much logic in proxy  | **Keep proxy thin**     |
| **Proxy chain**   | Multiple proxies layered | **Combine or simplify** |
| **Visible proxy** | Client knows about proxy | **Make transparent**    |

---

## Related Patterns

| Pattern       | Relationship                      |
| ------------- | --------------------------------- |
| **Adapter**   | Different interface               |
| **Decorator** | Adds behavior, not access control |
| **Facade**    | Simplifies, not controls access   |

---

## References

- Source: [Refactoring.guru - Proxy](https://refactoring.guru/design-patterns/proxy)
- See also: [Decorator](structural-decorator.md), [Adapter](structural-adapter.md)

# Observer Pattern

Reference documentation for the Observer behavioral design pattern.

---

## Overview

| Aspect            | Description                                                                                                                                      |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Category**      | Behavioral                                                                                                                                       |
| **Intent**        | Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically |
| **Also Known As** | Dependents, Publish-Subscribe, Event-Subscriber                                                                                                  |
| **Source**        | [Refactoring.guru - Observer](https://refactoring.guru/design-patterns/observer)                                                                 |

---

## Problem It Solves

| Problem                            | How Observer Helps              |
| ---------------------------------- | ------------------------------- |
| Objects need to know about changes | Automatic notification          |
| Tight coupling between objects     | Loose coupling via subscription |
| Unknown number of dependents       | Dynamic subscription            |
| Inconsistent state between objects | Synchronized updates            |

---

## Structure

```
┌─────────────────────┐
│     Subject         │
│   (Publisher)       │
├─────────────────────┤
│ - observers[]       │         ┌─────────────────────┐
│ + attach(Observer)  │         │     Observer        │
│ + detach(Observer)  │         │   (Subscriber)      │
│ + notify()          │────────►│    (interface)      │
└─────────────────────┘         ├─────────────────────┤
          │                     │ + update()          │
          │                     └──────────┬──────────┘
┌─────────┴───────────┐                    │ implements
│  ConcreteSubject    │         ┌──────────┴──────────┐
├─────────────────────┤         │ ConcreteObserver    │
│ - state             │         ├─────────────────────┤
│ + getState()        │◄────────│ - subject           │
│ + setState()        │ observes│ + update()          │
└─────────────────────┘         └─────────────────────┘
```

---

## Participants

| Participant             | Responsibility                                  |
| ----------------------- | ----------------------------------------------- |
| **Subject/Publisher**   | Maintains observers, sends notifications        |
| **Observer/Subscriber** | Interface for objects that should be notified   |
| **ConcreteSubject**     | Stores state, notifies on change                |
| **ConcreteObserver**    | Reacts to notifications, keeps state consistent |

---

## When to Use

| Scenario                                | Indicator                    |
| --------------------------------------- | ---------------------------- |
| Changes need to notify multiple objects | YES - use observer           |
| Number of observers varies              | YES - dynamic subscription   |
| Objects should be loosely coupled       | YES - via subscription       |
| Broadcast communication                 | YES - all observers notified |

---

## When NOT to Use

| Scenario                       | Why Not                          |
| ------------------------------ | -------------------------------- |
| Single observer always         | Direct reference simpler         |
| Order of notification critical | Observer doesn't guarantee order |
| Complex update dependencies    | May cause cascade issues         |

---

## Push vs Pull

| Model    | Description                          | When to Use               |
| -------- | ------------------------------------ | ------------------------- |
| **Push** | Subject sends state to observers     | Small, uniform updates    |
| **Pull** | Observer requests state from subject | Large state, varied needs |

---

## Implementation Checklist

**MANDATORY: Verify before implementing:**

```
[ ] Observer interface defined
[ ] Subject maintains observer list
[ ] attach/detach methods provided
[ ] notify calls update on all observers
[ ] Observers can unsubscribe
[ ] Prevent notification during update (if needed)
[ ] Handle observer failures gracefully
```

---

## Anti-Patterns to Detect

| Anti-Pattern            | Indication                           | Refactoring Action            |
| ----------------------- | ------------------------------------ | ----------------------------- |
| **Memory leak**         | Observers not detached               | **Unsubscribe properly**      |
| **Cascade updates**     | Observer triggers more notifications | **Consider event coalescing** |
| **Hidden dependencies** | Unclear what observes what           | **Document relationships**    |
| **Notification storm**  | Too many updates                     | **Batch notifications**       |

---

## Related Patterns

| Pattern                     | Relationship                           |
| --------------------------- | -------------------------------------- |
| **Mediator**                | Can encapsulate observer subscriptions |
| **Singleton**               | Subject often singleton                |
| **Command**                 | Commands as notifications              |
| **Chain of Responsibility** | Alternative for event handling         |

---

## References

- Source: [Refactoring.guru - Observer](https://refactoring.guru/design-patterns/observer)
- See also: [Mediator](behavioral-mediator.md)

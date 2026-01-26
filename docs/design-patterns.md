# Design Patterns Reference

Index of all 22 GoF design patterns from [Refactoring.guru](https://refactoring.guru/design-patterns).

---

## Creational Patterns (5)

Creational patterns provide object creation mechanisms that increase flexibility and reuse of existing code.

| Pattern              | Intent                                           | File                                                                                        |
| -------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| **Factory Method**   | Creates objects without specifying exact classes | [creational-factory-method.md](references/design-patterns/creational-factory-method.md)     |
| **Abstract Factory** | Creates families of related objects              | [creational-abstract-factory.md](references/design-patterns/creational-abstract-factory.md) |
| **Builder**          | Constructs complex objects step-by-step          | [creational-builder.md](references/design-patterns/creational-builder.md)                   |
| **Prototype**        | Clones existing objects                          | [creational-prototype.md](references/design-patterns/creational-prototype.md)               |
| **Singleton**        | Ensures only one instance exists                 | [creational-singleton.md](references/design-patterns/creational-singleton.md)               |

---

## Structural Patterns (7)

Structural patterns explain how to assemble objects and classes into larger structures while keeping them flexible and efficient.

| Pattern       | Intent                                               | File                                                                          |
| ------------- | ---------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Adapter**   | Makes incompatible interfaces work together          | [structural-adapter.md](references/design-patterns/structural-adapter.md)     |
| **Bridge**    | Separates abstraction from implementation            | [structural-bridge.md](references/design-patterns/structural-bridge.md)       |
| **Composite** | Treats individual objects and compositions uniformly | [structural-composite.md](references/design-patterns/structural-composite.md) |
| **Decorator** | Adds functionality dynamically                       | [structural-decorator.md](references/design-patterns/structural-decorator.md) |
| **Facade**    | Provides simplified interface to subsystem           | [structural-facade.md](references/design-patterns/structural-facade.md)       |
| **Flyweight** | Shares common data efficiently                       | [structural-flyweight.md](references/design-patterns/structural-flyweight.md) |
| **Proxy**     | Controls access to another object                    | [structural-proxy.md](references/design-patterns/structural-proxy.md)         |

---

## Behavioral Patterns (10)

Behavioral patterns take care of effective communication and assignment of responsibilities between objects.

| Pattern                     | Intent                                        | File                                                                                                      |
| --------------------------- | --------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Chain of Responsibility** | Passes requests along a handler chain         | [behavioral-chain-of-responsibility.md](references/design-patterns/behavioral-chain-of-responsibility.md) |
| **Command**                 | Encapsulates requests as objects              | [behavioral-command.md](references/design-patterns/behavioral-command.md)                                 |
| **Iterator**                | Accesses elements sequentially                | [behavioral-iterator.md](references/design-patterns/behavioral-iterator.md)                               |
| **Mediator**                | Centralizes communication between objects     | [behavioral-mediator.md](references/design-patterns/behavioral-mediator.md)                               |
| **Memento**                 | Captures and restores object state            | [behavioral-memento.md](references/design-patterns/behavioral-memento.md)                                 |
| **Observer**                | Notifies multiple objects about state changes | [behavioral-observer.md](references/design-patterns/behavioral-observer.md)                               |
| **State**                   | Alters behavior when internal state changes   | [behavioral-state.md](references/design-patterns/behavioral-state.md)                                     |
| **Strategy**                | Selects algorithms at runtime                 | [behavioral-strategy.md](references/design-patterns/behavioral-strategy.md)                               |
| **Template Method**         | Defines algorithm skeleton in base class      | [behavioral-template-method.md](references/design-patterns/behavioral-template-method.md)                 |
| **Visitor**                 | Adds operations to object structures          | [behavioral-visitor.md](references/design-patterns/behavioral-visitor.md)                                 |

---

## Document Structure

Each pattern document follows a consistent structure:

| Section                      | Purpose                                     |
| ---------------------------- | ------------------------------------------- |
| **Overview**                 | Category, intent, source                    |
| **Problem It Solves**        | What issues the pattern addresses           |
| **Structure**                | ASCII diagram of pattern structure          |
| **Participants**             | Key classes/interfaces and responsibilities |
| **When to Use**              | Scenarios where pattern applies             |
| **When NOT to Use**          | Scenarios where pattern is inappropriate    |
| **Implementation Checklist** | Verification before implementing            |
| **Anti-Patterns to Detect**  | Common mistakes and refactoring actions     |
| **Related Patterns**         | Connections to other patterns               |
| **References**               | Source links                                |

---

## Source

All patterns verified against [Refactoring.guru - Design Patterns](https://refactoring.guru/design-patterns)

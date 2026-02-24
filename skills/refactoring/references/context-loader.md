# Context Loader Reference

Quick lookup for which reference files to load based on detected issues.

---

## Principle References

| Reference             | Load When You Detect                                                                               |
| --------------------- | -------------------------------------------------------------------------------------------------- |
| `solid-principles.md` | Class doing multiple things, hard to extend, inheritance issues, fat interfaces, hard dependencies |
| `dry-principle.md`    | Duplicated code, copy-paste logic, same concept in multiple places                                 |
| `kiss-principle.md`   | Over-engineered solutions, premature abstractions, unnecessary complexity                          |

---

## Architecture References

| Reference                         | Load When You Detect                                                              |
| --------------------------------- | --------------------------------------------------------------------------------- |
| `vertical-slice-architecture.md`  | Feature logic scattered across layers, difficult to understand feature flow       |
| `cqs-command-query-separation.md` | Methods that both read and modify state, unclear side effects                     |
| `domain-driven-design.md`         | Business logic in controllers/services, anemic domain models, primitive obsession |

---

## Design Pattern References by Category

### Creational (Object Creation Problems)

| Pattern                          | Load When                                                |
| -------------------------------- | -------------------------------------------------------- |
| `creational-factory-method.md`   | Subclasses need to decide what objects to create         |
| `creational-abstract-factory.md` | Need families of related objects                         |
| `creational-builder.md`          | Complex object construction, many constructor parameters |
| `creational-prototype.md`        | Need to clone objects, costly initialization             |
| `creational-singleton.md`        | Need exactly one instance (use sparingly)                |

### Structural (Composition Problems)

| Pattern                   | Load When                                                |
| ------------------------- | -------------------------------------------------------- |
| `structural-adapter.md`   | Incompatible interfaces need to work together            |
| `structural-bridge.md`    | Abstraction and implementation should vary independently |
| `structural-composite.md` | Tree structures, part-whole hierarchies                  |
| `structural-decorator.md` | Need to add behavior dynamically without subclassing     |
| `structural-facade.md`    | Complex subsystem needs simpler interface                |
| `structural-flyweight.md` | Many similar objects consuming too much memory           |
| `structural-proxy.md`     | Need to control access, lazy loading, logging            |

### Behavioral (Communication Problems)

| Pattern                                 | Load When                                                    |
| --------------------------------------- | ------------------------------------------------------------ |
| `behavioral-chain-of-responsibility.md` | Multiple handlers for request, handler not known upfront     |
| `behavioral-command.md`                 | Need to parameterize operations, queue/log requests, undo    |
| `behavioral-iterator.md`                | Need to traverse collection without exposing internals       |
| `behavioral-mediator.md`                | Objects communicate complexly, too many connections          |
| `behavioral-memento.md`                 | Need to save/restore object state (undo)                     |
| `behavioral-observer.md`                | Objects need to be notified of changes                       |
| `behavioral-state.md`                   | Object behavior depends on state, many conditionals on state |
| `behavioral-strategy.md`                | Need interchangeable algorithms                              |
| `behavioral-template-method.md`         | Algorithm skeleton with varying steps                        |
| `behavioral-visitor.md`                 | Need to add operations to object structure                   |

---

## Decision Flowchart

```text
START: Analyze code without loading references
  │
  ├─► Is it a SIZE problem?
  │     ├─► Long method → No pattern, just extract (SRP)
  │     ├─► Large class → Load: solid-principles.md (SRP)
  │     └─► Long params → Load: creational-builder.md
  │
  ├─► Is it a DUPLICATION problem?
  │     ├─► Same code blocks → Load: dry-principle.md
  │     ├─► Similar algorithms → Load: behavioral-template-method.md
  │     └─► Similar conditionals → Load: behavioral-strategy.md
  │
  ├─► Is it a COUPLING problem?
  │     ├─► Hard dependencies → Load: solid-principles.md (DIP)
  │     ├─► Feature envy → No pattern, move method
  │     └─► Message chains → Load: structural-facade.md
  │
  ├─► Is it a COMPLEXITY problem?
  │     ├─► Switch on type → Load: behavioral-strategy.md or behavioral-state.md
  │     ├─► Over-engineered → Load: kiss-principle.md
  │     └─► Speculative code → Load: kiss-principle.md
  │
  ├─► Is it an ARCHITECTURE problem?
  │     ├─► Scattered features → Load: vertical-slice-architecture.md
  │     ├─► Mixed commands/queries → Load: cqs-command-query-separation.md
  │     └─► Misplaced logic → Load: domain-driven-design.md
  │
  └─► Is it an OBJECT CREATION problem?
        ├─► Complex construction → Load: creational-builder.md
        ├─► Family of objects → Load: creational-abstract-factory.md
        └─► Subclass decides → Load: creational-factory-method.md
```

---

## Maximum Efficient Loading

**Best case:** Load 1 reference file
**Typical case:** Load 2-3 reference files
**Worst case:** Load 5+ reference files (complex refactoring)

**Never load:** All 28 references at once

---

## File Paths

All references relative to skill location:

```text
../../docs/solid-principles.md
../../docs/dry-principle.md
../../docs/kiss-principle.md
../../docs/vertical-slice-architecture.md
../../docs/cqs-command-query-separation.md
../../docs/domain-driven-design.md
../../docs/design-patterns.md (index)
../../docs/references/design-patterns/*.md (individual patterns)
```

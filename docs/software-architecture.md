# Reference Documentation

Comprehensive reference documentation for software architecture patterns, design patterns, and coding principles.

---

## Principles

| Principle | Description                                         | File                                       |
| --------- | --------------------------------------------------- | ------------------------------------------ |
| **SOLID** | Five principles of object-oriented design           | [solid-principles.md](solid-principles.md) |
| **DRY**   | Don't Repeat Yourself - single source of truth      | [dry-principle.md](dry-principle.md)       |
| **KISS**  | Keep It Simple, Stupid - simplicity over complexity | [kiss-principle.md](kiss-principle.md)     |

---

## Architecture Patterns

| Pattern | Description                                           | File                                                               |
| ------- | ----------------------------------------------------- | ------------------------------------------------------------------ |
| **VSA** | Vertical Slice Architecture - organize by feature     | [vertical-slice-architecture.md](vertical-slice-architecture.md)   |
| **CQS** | Command Query Separation - separate reads from writes | [cqs-command-query-separation.md](cqs-command-query-separation.md) |
| **DDD** | Domain-Driven Design - focus on core domain           | [domain-driven-design.md](domain-driven-design.md)                 |

---

## Design Patterns (GoF)

22 classic design patterns organized by category.

| Category       | Count | Index                                                           |
| -------------- | ----- | --------------------------------------------------------------- |
| **Creational** | 5     | [design-patterns.md](design-patterns.md#creational-patterns-5)  |
| **Structural** | 7     | [design-patterns.md](design-patterns.md#structural-patterns-7)  |
| **Behavioral** | 10    | [design-patterns.md](design-patterns.md#behavioral-patterns-10) |

See [design-patterns.md](design-patterns.md) for complete index.

---

## Document Standards

All reference documents follow consistent structure:

### Principles & Architecture Patterns

| Section                        | Purpose                         |
| ------------------------------ | ------------------------------- |
| **Overview**                   | Definition, scope, origin       |
| **Core Concepts**              | Key ideas and definitions       |
| **Identification Checklist**   | How to detect violations        |
| **Implementation Strategies**  | How to apply                    |
| **Anti-Patterns to Detect**    | Common mistakes                 |
| **Compliance Checklist**       | Verification before concluding  |
| **Verification Questions**     | Questions to confirm compliance |
| **Anti-Rationalization Table** | Prevent shortcuts               |
| **References**                 | Sources and related patterns    |

### Design Patterns

| Section                      | Purpose                           |
| ---------------------------- | --------------------------------- |
| **Overview**                 | Category, intent, source          |
| **Problem It Solves**        | What issues the pattern addresses |
| **Structure**                | ASCII diagram                     |
| **Participants**             | Classes and responsibilities      |
| **When to Use / NOT to Use** | Applicability                     |
| **Implementation Checklist** | Verification before implementing  |
| **Anti-Patterns to Detect**  | Common mistakes                   |
| **Related Patterns**         | Connections to other patterns     |

---

## Usage

These references are used by the refactoring skill to:

1. **Identify** code smells and violations
2. **Apply** appropriate patterns and principles
3. **Verify** compliance after refactoring
4. **Prevent** common anti-patterns

---

## Total: 28 Reference Documents

- 3 Principles (SOLID, DRY, KISS)
- 3 Architecture Patterns (VSA, CQS, DDD)
- 22 Design Patterns (5 Creational + 7 Structural + 10 Behavioral)

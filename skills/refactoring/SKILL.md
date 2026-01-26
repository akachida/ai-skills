---
name: refactoring
description: Identify code smells and apply appropriate design patterns and principles with token-efficient context loading
---

# Refactoring Skill

You are a Senior Software Architect specializing in code refactoring.

## Core Principle: Token-Efficient Context Loading

**CRITICAL:** Do NOT load all reference documentation upfront. Follow the phased approach below to load ONLY what is necessary for the specific refactoring task.

1.  Always load [domain-driven-design.md](../../docs/domain-driven-design.md) for better coding context.
2.  Load [context-loader.md](references/context-loader.md) for better understanding of context loading scenarios.

---

## Phase 1: Code Analysis (No References Needed)

**Before loading ANY reference files, analyze the code:**

### 1.1 Gather Information

```bash
# If specific files mentioned, read them
# If directory mentioned, understand structure first
```

### 1.2 Identify Code Smells

Examine the code for these smell categories:

| Category          | Smells to Detect                                                |
| ----------------- | --------------------------------------------------------------- |
| **Size**          | Long method, Large class, Long parameter list                   |
| **Coupling**      | Feature envy, Inappropriate intimacy, Message chains            |
| **Duplication**   | Duplicated code, Similar conditionals                           |
| **Abstraction**   | Primitive obsession, Data clumps, Refused bequest               |
| **Complexity**    | Switch statements, Parallel inheritance, Speculative generality |
| **Encapsulation** | Data class, Exposed internals, Incomplete library class         |

### 1.3 Categorize the Problem

After analysis, determine which category applies:

| Problem Type            | Indicators                            |
| ----------------------- | ------------------------------------- |
| **PRINCIPLE_VIOLATION** | Violates SOLID, DRY, or KISS          |
| **PATTERN_OPPORTUNITY** | Could benefit from design pattern     |
| **ARCHITECTURE_ISSUE**  | Structural/organizational problem     |
| **SIMPLE_CLEANUP**      | Minor improvements, no pattern needed |

---

## Phase 2: Selective Reference Loading

**Load ONLY the references needed based on Phase 1 findings.**

### Reference Loading Decision Tree

```
┌─────────────────────────────────────────────────────────────────┐
│ What did you identify in Phase 1?                               │
└─────────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
   PRINCIPLE             PATTERN              ARCHITECTURE
   VIOLATION           OPPORTUNITY              ISSUE
        │                     │                     │
        ▼                     ▼                     ▼
  Load specific        Load pattern           Load architecture
  principle doc        category doc             pattern doc
```

### 2.1 Principle Violations → Load Specific Principle

| Detected Issue               | Load This Reference                            |
| ---------------------------- | ---------------------------------------------- |
| Single responsibility breach | `../../docs/solid-principles.md` (SRP section) |
| Extension difficulties       | `../../docs/solid-principles.md` (OCP section) |
| Inheritance abuse            | `../../docs/solid-principles.md` (LSP section) |
| Fat interfaces               | `../../docs/solid-principles.md` (ISP section) |
| Hard dependencies            | `../../docs/solid-principles.md` (DIP section) |
| Code duplication             | `../../docs/dry-principle.md`                  |
| Over-engineering             | `../../docs/kiss-principle.md`                 |

### 2.2 Pattern Opportunities → Load Pattern Category First

**Step 1: Load the index to confirm pattern choice**

```
Load: ../../docs/design-patterns.md
```

**Step 2: Load specific pattern only after confirming need**

| Problem Pattern                  | Likely Design Pattern   | Reference                                                       |
| -------------------------------- | ----------------------- | --------------------------------------------------------------- |
| Object creation complexity       | Factory Method, Builder | `../../docs/references/design-patterns/creational-*.md`         |
| Need to switch algorithms        | Strategy                | `../../docs/references/design-patterns/behavioral-strategy.md`  |
| Need to add behavior dynamically | Decorator               | `../../docs/references/design-patterns/structural-decorator.md` |
| Complex subsystem access         | Facade                  | `../../docs/references/design-patterns/structural-facade.md`    |
| State-dependent behavior         | State                   | `../../docs/references/design-patterns/behavioral-state.md`     |
| Event/notification system        | Observer                | `../../docs/references/design-patterns/behavioral-observer.md`  |
| Undo/history needed              | Memento, Command        | `../../docs/references/design-patterns/behavioral-*.md`         |
| Tree/hierarchy structure         | Composite               | `../../docs/references/design-patterns/structural-composite.md` |

### 2.3 Architecture Issues → Load Architecture Pattern

| Detected Issue                  | Load This Reference                          |
| ------------------------------- | -------------------------------------------- |
| Feature scattered across layers | `../../docs/vertical-slice-architecture.md`  |
| Mixed read/write concerns       | `../../docs/cqs-command-query-separation.md` |
| Domain logic in wrong places    | `../../docs/domain-driven-design.md`         |

---

## Phase 3: Apply Refactoring

**Only after loading relevant references, proceed with refactoring.**

### 3.1 Planning (MANDATORY)

Before making changes, create a plan:

```markdown
## Refactoring Plan

### Problem Identified

[Specific code smell or violation found in Phase 1]

### Solution Approach

[Pattern or principle to apply from Phase 2]

### Files to Modify

- [ ] file1.ts - [what changes]
- [ ] file2.ts - [what changes]

### Verification

- [ ] Original behavior preserved
- [ ] Tests still pass
- [ ] No new violations introduced
```

### 3.2 Execution

1. **Small steps** - One refactoring at a time
2. **Verify** - Run tests after each change
3. **Document** - Comment non-obvious decisions

### 3.3 Post-Refactoring Checklist

```
[ ] Original functionality preserved
[ ] Tests pass
[ ] No new code smells introduced
[ ] Follows loaded principle/pattern guidelines
[ ] Code is simpler, not more complex
```

---

## Quick Reference: Smell → Solution Mapping

**Use this to determine what references to load:**

| Code Smell                   | Principle           | Pattern Alternative             |
| ---------------------------- | ------------------- | ------------------------------- |
| **Duplicated code**          | DRY                 | Template Method, Strategy       |
| **Long method**              | SRP                 | Extract Method (no pattern)     |
| **Large class**              | SRP                 | Facade, Extract Class           |
| **Long parameter list**      | -                   | Builder, Parameter Object       |
| **Divergent change**         | SRP                 | -                               |
| **Shotgun surgery**          | -                   | Move Method (no pattern)        |
| **Feature envy**             | -                   | Move Method (no pattern)        |
| **Data clumps**              | DRY                 | Extract Class, Parameter Object |
| **Primitive obsession**      | DDD (Value Objects) | -                               |
| **Switch statements**        | OCP                 | Strategy, State, Polymorphism   |
| **Parallel inheritance**     | -                   | -                               |
| **Lazy class**               | KISS                | Inline Class (no pattern)       |
| **Speculative generality**   | KISS, YAGNI         | Remove unused abstraction       |
| **Temporary field**          | -                   | Extract Class, Null Object      |
| **Message chains**           | -                   | Facade, Hide Delegate           |
| **Middle man**               | -                   | Remove Middle Man               |
| **Inappropriate intimacy**   | -                   | Move Method, Extract Class      |
| **Alternative classes**      | -                   | Extract Interface               |
| **Incomplete library class** | -                   | Adapter, Decorator              |
| **Data class**               | DDD                 | Add behavior to class           |
| **Refused bequest**          | LSP                 | Replace inheritance             |

---

## Example: Token-Efficient Flow

### Scenario: User asks to refactor a service class

**Phase 1 (No references loaded):**

```
1. Read the service class
2. Identify: Large class (500+ lines), multiple responsibilities
3. Categorize: PRINCIPLE_VIOLATION (SRP)
```

**Phase 2 (Load only what's needed):**

```
Load: ../../docs/solid-principles.md
(Focus on SRP section only)
```

**Phase 3 (Apply):**

```
1. Plan extraction of responsibilities
2. Create focused classes
3. Verify behavior preserved
```

**NOT loaded (saves tokens):**

- All 22 design pattern files
- DRY, KISS principles (not relevant)
- Architecture patterns (not relevant)

---

## Anti-Patterns: What NOT to Do

| Anti-Pattern                            | Why It's Wrong           | Correct Approach               |
| --------------------------------------- | ------------------------ | ------------------------------ |
| Load all 28 reference docs upfront      | Wastes tokens            | Load based on Phase 1 findings |
| Apply pattern without identifying smell | Solution without problem | Always analyze first           |
| Skip the planning phase                 | Uncontrolled changes     | Always plan before coding      |
| Refactor multiple things at once        | Hard to verify           | One change at a time           |
| Over-apply patterns                     | KISS violation           | Only what's necessary          |

---

## When NOT to Refactor

| Situation                    | Action                  |
| ---------------------------- | ----------------------- |
| Code works, no clear smell   | Leave it alone          |
| Tight deadline, risky change | Document for later      |
| No tests to verify behavior  | Add tests first         |
| Unclear requirements         | Clarify before changing |

---

## Output Format

After refactoring, provide:

```markdown
## Refactoring Complete

### What Was Changed

- [File]: [Change description]

### Pattern/Principle Applied

[Name and brief explanation]

### Before/After Comparison

[Key structural change shown]

### Verification

- [ ] Tests pass
- [ ] Behavior preserved
- [ ] No new smells

### References Used

[List only the references that were actually loaded]
```

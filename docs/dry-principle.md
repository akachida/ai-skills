# DRY - Don't Repeat Yourself

Reference documentation for the DRY principle in software development.

---

## Overview

**DRY (Don't Repeat Yourself)** states that every piece of knowledge MUST have a single, unambiguous, authoritative representation within a system.

| Aspect | Requirement |
|--------|-------------|
| **Core Principle** | Single source of truth for every piece of knowledge |
| **Scope** | Code, data, documentation, configuration |
| **Goal** | Reduce duplication, increase maintainability |
| **Origin** | Andy Hunt and Dave Thomas - "The Pragmatic Programmer" |

---

## Definition

### The DRY Principle

> "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

**CRITICAL:** DRY is NOT just about code duplication. It applies to:
- Business logic
- Database schemas
- Documentation
- Configuration
- Build processes
- Test data

---

## Types of Duplication

### 1. Code Duplication (Most Obvious)

| Type | Description | Detection |
|------|-------------|-----------|
| **Exact Clones** | Identical code blocks | **Search for repeated lines** |
| **Structural Clones** | Same structure, different values | **Look for similar patterns** |
| **Semantic Clones** | Different code, same behavior | **Analyze functionality overlap** |

### 2. Knowledge Duplication (Most Dangerous)

| Type | Description | Example |
|------|-------------|---------|
| **Business Rule Duplication** | Same rule in multiple places | Validation in UI AND backend |
| **Data Schema Duplication** | Same structure defined multiple times | DTO matches entity matches API response |
| **Configuration Duplication** | Same settings in multiple files | Connection strings in multiple configs |

### 3. Documentation Duplication

| Type | Risk | Solution |
|------|------|----------|
| **Code Comments Repeating Code** | Comments become stale | **Remove redundant comments** |
| **Multiple Docs Describing Same Thing** | Docs diverge over time | **Single source, link to it** |
| **README Duplicating Wiki** | Maintenance burden | **Choose one authoritative source** |

---

## Identification Checklist

**MANDATORY: Check for these duplication indicators:**

| Indicator | DRY Violation | Required Action |
|-----------|---------------|-----------------|
| Copy-paste detected during development | YES | **Extract to shared function/module** |
| Same bug fixed in multiple places | YES | **Consolidate duplicated code** |
| Change requires updating multiple files | LIKELY | **Identify and eliminate duplication** |
| Same validation logic in multiple layers | YES | **Centralize validation rules** |
| Magic numbers/strings repeated | YES | **Extract to constants** |
| Same data transformation in multiple places | YES | **Create shared transformation function** |

---

## DRY vs WET

| DRY | WET (Write Everything Twice) |
|-----|------------------------------|
| Single source of truth | Multiple sources of truth |
| Change once, reflect everywhere | Change everywhere, risk inconsistency |
| Higher initial abstraction cost | Lower initial cost, higher maintenance |
| Easier to maintain long-term | Technical debt accumulates |

---

## Implementation Strategies

### 1. Extract Method/Function

**When:** Same code block appears multiple times

```
BEFORE (WET):
- Function A contains lines 10-25
- Function B contains same lines 10-25

AFTER (DRY):
- Extract lines 10-25 to shared function
- Function A calls shared function
- Function B calls shared function
```

### 2. Extract Class/Module

**When:** Related functions are duplicated across files

| Signal | Action |
|--------|--------|
| Multiple files import same set of utilities | **Create dedicated module** |
| Copy-paste of multiple related functions | **Extract to class** |
| Same helper code in multiple projects | **Create shared library** |

### 3. Configuration Centralization

**When:** Same values appear in multiple configuration files

| Approach | Use Case |
|----------|----------|
| **Environment Variables** | Deployment-specific values |
| **Central Config File** | Application-wide settings |
| **Config Server** | Distributed systems |

### 4. Code Generation

**When:** Boilerplate code must exist but varies slightly

| Scenario | Solution |
|----------|----------|
| DTOs from API schema | **Generate from OpenAPI/Swagger** |
| Database entities | **Generate from schema** |
| Repetitive CRUD operations | **Use code scaffolding** |

---

## Anti-Patterns to Detect

| Anti-Pattern | Description | Refactoring Action |
|--------------|-------------|-------------------|
| **Copy-Paste Programming** | Duplicating code instead of extracting | **Extract and parameterize** |
| **Shotgun Surgery** | Same change needed in many places | **Consolidate to single location** |
| **Parallel Inheritance** | Matching class hierarchies | **Merge or use composition** |
| **Magic Numbers/Strings** | Hardcoded values repeated | **Extract to named constants** |
| **Multiple Truths** | Same data defined differently in multiple places | **Establish single source of truth** |

---

## When DRY Can Be Harmful (Rule of Three)

**CRITICAL: Premature abstraction can be worse than duplication.**

### Rule of Three

| Occurrence | Action |
|------------|--------|
| **First time** | Write the code |
| **Second time** | Note the duplication, consider context |
| **Third time** | Extract and abstract |

### When Duplication May Be Acceptable

| Scenario | Reason |
|----------|--------|
| **Different domains** | Same structure, different meaning |
| **Different rates of change** | Coupling would cause unnecessary changes |
| **Accidental similarity** | Looks similar now, will diverge |
| **Testing** | Test isolation may require duplication |

### Decision Matrix

| Question | If YES | If NO |
|----------|--------|-------|
| Will these always change together? | **Extract** | Consider keeping separate |
| Do they represent the same concept? | **Extract** | Keep separate |
| Is the duplication in the same bounded context? | **Extract** | Evaluate carefully |

---

## DRY Compliance Checklist

**MANDATORY: Before concluding DRY compliance, verify:**

```
[ ] No exact code duplicates (use static analysis tools)
[ ] Business rules defined in single location
[ ] Configuration values not hardcoded in multiple places
[ ] Magic numbers/strings extracted to constants
[ ] Validation logic centralized
[ ] Data transformations not duplicated
[ ] Documentation has single source of truth
[ ] No copy-paste detected during code review
```

---

## Verification Questions

Before concluding DRY compliance, MUST answer:

1. If this business rule changes, how many files need modification?
2. Is there a single authoritative source for each piece of knowledge?
3. Could a search for common strings reveal hidden duplication?
4. Are there parallel structures that must be kept in sync manually?

---

## Anti-Rationalization Table

| Rationalization | Why It's WRONG | Required Action |
|-----------------|----------------|-----------------|
| "It's only duplicated twice" | Rule of three exists, but track it | **Document and monitor** |
| "Abstracting is over-engineering" | Duplication is under-engineering | **Evaluate maintenance cost** |
| "They're in different modules" | Location doesn't eliminate duplication | **Check if same knowledge** |
| "Copy-paste is faster" | Short-term gain, long-term pain | **Invest in proper abstraction** |
| "Tests need their own copy" | Test isolation has limits | **Share test utilities appropriately** |

---

## Tools for Detection

| Language | Tool |
|----------|------|
| **Multi-language** | SonarQube, PMD CPD |
| **JavaScript/TypeScript** | jscpd, ESLint |
| **Python** | Pylint, Clone Digger |
| **Java** | PMD CPD, IntelliJ built-in |
| **Go** | dupl, goclone |

---

## References

- Source: Andy Hunt & Dave Thomas - "The Pragmatic Programmer"
- Related: [SOLID Principles](solid-principles.md) - SRP addresses responsibility duplication
- Related: [KISS Principle](kiss-principle.md) - Balance DRY with simplicity

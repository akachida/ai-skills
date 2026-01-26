# VSA - Vertical Slice Architecture

Reference documentation for Vertical Slice Architecture pattern.

---

## Overview

**Vertical Slice Architecture (VSA)** organizes code by feature/use case rather than by technical layer. Each "slice" contains all the code needed to handle a specific feature, from UI to database.

| Aspect | Description |
|--------|-------------|
| **Core Principle** | Organize by feature, not by layer |
| **Structure** | Each feature is self-contained |
| **Goal** | High cohesion within features, low coupling between features |
| **Origin** | Jimmy Bogard - popularized through MediatR and CQRS patterns |

---

## Definition

### Traditional Layered Architecture vs Vertical Slices

```
LAYERED (Horizontal):
┌─────────────────────────────────────────┐
│            Presentation Layer           │
├─────────────────────────────────────────┤
│            Application Layer            │
├─────────────────────────────────────────┤
│             Domain Layer                │
├─────────────────────────────────────────┤
│          Infrastructure Layer           │
└─────────────────────────────────────────┘

VERTICAL SLICES:
┌──────────┬──────────┬──────────┬────────┐
│ Feature  │ Feature  │ Feature  │Feature │
│    A     │    B     │    C     │   D    │
│          │          │          │        │
│ UI       │ UI       │ UI       │ UI     │
│ Logic    │ Logic    │ Logic    │ Logic  │
│ Data     │ Data     │ Data     │ Data   │
└──────────┴──────────┴──────────┴────────┘
```

---

## Core Principles

### 1. Feature-First Organization

**MANDATORY: Code MUST be organized by what it does, not by technical concern.**

| Layered (AVOID) | Vertical Slice (PREFER) |
|-----------------|-------------------------|
| `/Controllers/OrderController.cs` | `/Features/Orders/CreateOrder/` |
| `/Services/OrderService.cs` | `/Features/Orders/CreateOrder/Handler.cs` |
| `/Repositories/OrderRepository.cs` | `/Features/Orders/CreateOrder/Query.cs` |
| `/Models/Order.cs` | `/Features/Orders/CreateOrder/Model.cs` |

### 2. Slice Independence

Each slice MUST be:
- **Self-contained**: All code for the feature in one place
- **Independent**: Changes to one slice don't affect others
- **Cohesive**: All code in slice serves same purpose
- **Complete**: Handles entire request/response cycle

### 3. Coupling Direction

| Type | Rule |
|------|------|
| **Within Slice** | High cohesion allowed |
| **Between Slices** | Minimal to no coupling |
| **Shared Code** | Only truly common infrastructure |

---

## Structure Template

### Feature Folder Structure

```
/Features
  /Orders
    /CreateOrder
      - CreateOrderCommand.cs      # Request/Input
      - CreateOrderHandler.cs      # Business logic
      - CreateOrderValidator.cs    # Validation rules
      - CreateOrderResponse.cs     # Response/Output
      - CreateOrderEndpoint.cs     # API endpoint (optional)
    /GetOrder
      - GetOrderQuery.cs
      - GetOrderHandler.cs
      - GetOrderResponse.cs
    /Shared                        # Order-specific shared code
      - OrderEntity.cs
      - OrderRepository.cs
  /Products
    /CreateProduct
      ...
    /GetProduct
      ...
/Common                            # Cross-cutting concerns only
  /Infrastructure
  /Middleware
```

### Slice Components

| Component | Purpose | Location |
|-----------|---------|----------|
| **Request/Command/Query** | Input contract | `{Feature}/{Action}/{Action}Command.cs` |
| **Handler** | Business logic | `{Feature}/{Action}/{Action}Handler.cs` |
| **Validator** | Input validation | `{Feature}/{Action}/{Action}Validator.cs` |
| **Response** | Output contract | `{Feature}/{Action}/{Action}Response.cs` |
| **Endpoint** | HTTP binding | `{Feature}/{Action}/{Action}Endpoint.cs` |

---

## Implementation Patterns

### 1. Request/Response Pattern

**Each slice MUST have explicit input and output contracts.**

```
Request → Handler → Response

- Request: What the slice needs
- Handler: What the slice does
- Response: What the slice returns
```

### 2. Handler Pattern

**Each slice MUST have a dedicated handler.**

| Handler Rule | Requirement |
|--------------|-------------|
| **Single responsibility** | One handler per slice |
| **No shared handlers** | Handlers are slice-specific |
| **Dependencies injected** | Via constructor |
| **Testable in isolation** | No hidden dependencies |

### 3. Mediator Pattern (Optional but Common)

Using MediatR or similar:

```
Endpoint → Mediator → Handler → Response
                  ↓
            Pipeline Behaviors
            (Validation, Logging, etc.)
```

---

## Identification Checklist

**MANDATORY: Check for these VSA indicators:**

| Indicator | VSA Compliant | Required Action |
|-----------|---------------|-----------------|
| Feature code in single folder | YES | Continue |
| Feature code spread across layers | NO | **Reorganize by feature** |
| Can delete feature folder and feature is gone | YES | Continue |
| Deleting feature requires changes across layers | NO | **Consolidate to slice** |
| New feature only adds new folder | YES | Continue |
| New feature requires changes to existing code | NO | **Review coupling** |

---

## When to Use VSA

### Good Fit

| Scenario | Why VSA Works |
|----------|---------------|
| **CRUD-heavy applications** | Clear feature boundaries |
| **Microservices** | Each service is essentially vertical slices |
| **Feature teams** | Teams own entire slice |
| **Rapid development** | Add features without touching existing code |

### Poor Fit

| Scenario | Why VSA May Not Work |
|----------|---------------------|
| **Highly shared domain logic** | Too much duplication across slices |
| **Complex domain with many relationships** | DDD may be better fit |
| **Small team, small application** | Overhead may not be justified |

---

## Common Mistakes

### 1. Creating Artificial Layers Within Slices

```
WRONG:
/Features/Orders/CreateOrder
  /Application
    - CreateOrderHandler.cs
  /Domain
    - Order.cs
  /Infrastructure
    - OrderRepository.cs

RIGHT:
/Features/Orders/CreateOrder
  - CreateOrderCommand.cs
  - CreateOrderHandler.cs
  - CreateOrderResponse.cs
```

### 2. Sharing Too Much

| Over-Sharing | Better Approach |
|--------------|-----------------|
| Shared DTOs between slices | **Each slice owns its contracts** |
| Shared validation across slices | **Duplicate if different contexts** |
| Shared handlers | **Never share handlers** |

### 3. Cross-Slice Dependencies

```
WRONG:
CreateOrderHandler calls GetProductHandler directly

RIGHT:
CreateOrderHandler queries product via repository/service
OR
CreateOrderHandler uses shared domain service
```

---

## VSA + CQRS Integration

**VSA pairs naturally with CQRS (Command Query Responsibility Segregation):**

```
/Features/Orders
  /Commands
    /CreateOrder
      - CreateOrderCommand.cs
      - CreateOrderHandler.cs
    /UpdateOrder
      - UpdateOrderCommand.cs
      - UpdateOrderHandler.cs
  /Queries
    /GetOrder
      - GetOrderQuery.cs
      - GetOrderHandler.cs
    /ListOrders
      - ListOrdersQuery.cs
      - ListOrdersHandler.cs
```

---

## Anti-Patterns to Detect

| Anti-Pattern | Description | Refactoring Action |
|--------------|-------------|-------------------|
| **Slice Dependency** | One slice calls another directly | **Use shared service or events** |
| **God Slice** | Slice does too many things | **Split into multiple slices** |
| **Thin Slice** | Slice is just a pass-through | **Evaluate if slice is needed** |
| **Hidden Layers** | Recreating layers inside slice | **Flatten slice structure** |
| **Shared Handler** | Handler used by multiple slices | **Duplicate or extract to service** |

---

## VSA Compliance Checklist

**MANDATORY: Before concluding VSA compliance, verify:**

```
[ ] Features organized in dedicated folders
[ ] Each feature folder contains all related code
[ ] No cross-feature dependencies (except shared infrastructure)
[ ] Adding new feature only requires new folder
[ ] Removing feature only requires deleting folder
[ ] Handlers are feature-specific (not shared)
[ ] Request/Response contracts are feature-specific
[ ] Shared code is truly cross-cutting (auth, logging, etc.)
```

---

## Verification Questions

Before concluding VSA compliance, MUST answer:

1. Can a developer find all code for feature X in one place?
2. Can feature X be deleted by removing one folder?
3. Do changes to feature X require changes to other features?
4. Are handlers specific to their slice?

---

## Anti-Rationalization Table

| Rationalization | Why It's WRONG | Required Action |
|-----------------|----------------|-----------------|
| "We need shared services for reuse" | Reuse can create coupling | **Evaluate if truly cross-cutting** |
| "Our domain requires shared entities" | Slices can share via explicit boundaries | **Define clear sharing contracts** |
| "It's easier to find code by layer" | Feature-first is more maintainable | **Train team on VSA navigation** |
| "We already have layered architecture" | Technical debt doesn't justify continuation | **Migrate incrementally** |
| "Validation should be shared" | Context-specific validation is clearer | **Duplicate if contexts differ** |

---

## Migration Strategy

### From Layered to VSA

| Phase | Action |
|-------|--------|
| **1. New Features** | Implement new features as vertical slices |
| **2. Identify Boundaries** | Map existing features to potential slices |
| **3. Extract Incrementally** | Move one feature at a time |
| **4. Refactor Shared** | Move truly shared code to common |
| **5. Delete Layers** | Remove empty layer folders |

---

## References

- Source: Jimmy Bogard - "Vertical Slice Architecture"
- Related: [CQS](cqs-command-query-separation.md) - Often used together
- Related: [DDD](domain-driven-design.md) - For complex domains
- Tool: MediatR - Common implementation library

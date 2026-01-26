# DDD - Domain-Driven Design

Reference documentation for Domain-Driven Design principles and patterns.

---

## Overview

**Domain-Driven Design (DDD)** is an approach to software development that centers the design on the core business domain and domain logic, establishing a common language between developers and domain experts.

| Aspect             | Description                                                                              |
| ------------------ | ---------------------------------------------------------------------------------------- |
| **Core Principle** | Focus on the core domain and domain logic                                                |
| **Collaboration**  | Close collaboration between technical and domain experts                                 |
| **Language**       | Ubiquitous language shared by all team members                                           |
| **Origin**         | Eric Evans - "Domain-Driven Design: Tackling Complexity in the Heart of Software" (2003) |

---

## Strategic Design

### Bounded Contexts

**A Bounded Context is a explicit boundary within which a domain model exists.**

| Concept                 | Definition                                                        |
| ----------------------- | ----------------------------------------------------------------- |
| **Bounded Context**     | Linguistic boundary where terms have specific meaning             |
| **Context Map**         | Visual representation of bounded contexts and their relationships |
| **Ubiquitous Language** | Shared vocabulary within a bounded context                        |

#### Context Relationships

| Relationship              | Description                                    | When to Use                      |
| ------------------------- | ---------------------------------------------- | -------------------------------- |
| **Shared Kernel**         | Two contexts share a subset of the model       | Tight collaboration possible     |
| **Customer-Supplier**     | Upstream context provides, downstream consumes | Clear dependency direction       |
| **Conformist**            | Downstream adopts upstream model as-is         | No influence over upstream       |
| **Anti-Corruption Layer** | Translation layer protects downstream          | Different models, need isolation |
| **Open Host Service**     | Upstream provides well-defined protocol        | Multiple downstream consumers    |
| **Published Language**    | Shared interchange format                      | Integration needs                |
| **Separate Ways**         | No integration                                 | Independent contexts             |

#### Context Map Template

```
┌─────────────────┐         ┌─────────────────┐
│   Orders        │   ACL   │   Inventory     │
│   Context       │◄───────►│   Context       │
│                 │         │                 │
│  Order          │         │  Product        │
│  OrderLine      │         │  Stock          │
│  Customer (ID)  │         │  Warehouse      │
└─────────────────┘         └─────────────────┘
        │                           │
        │ Customer-Supplier         │ Conformist
        ▼                           ▼
┌─────────────────┐         ┌─────────────────┐
│   Billing       │         │   Shipping      │
│   Context       │         │   Context       │
└─────────────────┘         └─────────────────┘
```

---

### Ubiquitous Language

**MANDATORY: Every bounded context MUST have a ubiquitous language.**

| Rule              | Requirement                                      |
| ----------------- | ------------------------------------------------ |
| **Consistency**   | Same term means same thing everywhere in context |
| **Precision**     | Terms MUST be precisely defined                  |
| **Evolution**     | Language evolves with understanding              |
| **Documentation** | Glossary MUST be maintained                      |

#### Language Enforcement

| Violation                                | Indication                             | Action                       |
| ---------------------------------------- | -------------------------------------- | ---------------------------- |
| Same concept, different names            | "Customer" vs "Client" vs "User"       | **Agree on single term**     |
| Same name, different meanings            | "Account" in billing vs auth           | **Disambiguate per context** |
| Technical terms in domain                | "UserDTO", "OrderEntity"               | **Use pure domain terms**    |
| Domain terms in code don't match experts | Code says "Basket", experts say "Cart" | **Align code with domain**   |

---

## Tactical Design

### Building Blocks

| Building Block          | Purpose                              | Characteristics                     |
| ----------------------- | ------------------------------------ | ----------------------------------- |
| **Entity**              | Has identity, lifecycle              | Unique ID, mutable                  |
| **Value Object**        | Describes characteristic             | No ID, immutable, equality by value |
| **Aggregate**           | Consistency boundary                 | Root entity + related objects       |
| **Domain Event**        | Something that happened              | Immutable, past tense               |
| **Repository**          | Access aggregates                    | Collection-like interface           |
| **Factory**             | Create complex objects               | Encapsulates creation logic         |
| **Domain Service**      | Domain logic not belonging to entity | Stateless operations                |
| **Application Service** | Orchestrates use cases               | Thin, delegates to domain           |

---

### Entities

**An Entity is an object defined by its identity rather than its attributes.**

| Characteristic | Requirement                                       |
| -------------- | ------------------------------------------------- |
| **Identity**   | MUST have unique identifier                       |
| **Continuity** | Same entity across time despite attribute changes |
| **Mutability** | State can change, identity remains                |

#### Entity Rules

```
ENTITY MUST:
- Have unique identifier
- Implement identity-based equality
- Maintain valid state (invariants)
- Encapsulate behavior with state

ENTITY MUST NOT:
- Be created in invalid state
- Expose internal collections directly
- Allow invariant violations
```

---

### Value Objects

**A Value Object is an object that describes a characteristic and has no identity.**

| Characteristic       | Requirement                                 |
| -------------------- | ------------------------------------------- |
| **Immutability**     | MUST be immutable once created              |
| **Equality**         | Equality based on attribute values          |
| **Replaceability**   | Can be replaced by another with same values |
| **Side-effect-free** | Methods return new instances                |

#### Value Object Examples

| Domain         | Value Objects                       |
| -------------- | ----------------------------------- |
| **E-commerce** | Money, Address, DateRange, Quantity |
| **Healthcare** | BloodPressure, Temperature, Dosage  |
| **Finance**    | Currency, AccountNumber, TaxRate    |

#### Value Object Rules

```
VALUE OBJECT MUST:
- Be immutable
- Implement value-based equality
- Be self-validating
- Return new instances from operations

VALUE OBJECT MUST NOT:
- Have setters
- Have identity/ID
- Depend on entities
```

---

### Aggregates

**An Aggregate is a cluster of domain objects treated as a single unit for data changes.**

| Concept                | Definition                          |
| ---------------------- | ----------------------------------- |
| **Aggregate Root**     | Entry point entity, controls access |
| **Aggregate Boundary** | Defines consistency boundary        |
| **Invariants**         | Rules that MUST always be true      |

#### Aggregate Rules (MANDATORY)

| Rule                   | Requirement                                   |
| ---------------------- | --------------------------------------------- |
| **Single Root**        | Only root entity can be referenced externally |
| **Reference by ID**    | Aggregates reference others by ID only        |
| **Single Transaction** | Changes to aggregate in single transaction    |
| **Consistency**        | Root enforces all invariants                  |
| **Cascade Delete**     | Deleting root deletes entire aggregate        |

#### Aggregate Design Guidelines

```
SMALL AGGREGATES:
- Prefer smaller aggregates
- Reference other aggregates by ID
- Use eventual consistency between aggregates

CONSISTENCY BOUNDARY:
- True invariants → Same aggregate
- Eventual consistency acceptable → Separate aggregates
```

#### Example: Order Aggregate

```
Order Aggregate:
┌─────────────────────────────────────┐
│  Order (Root)                       │
│  ├── OrderId (Identity)             │
│  ├── CustomerId (Reference by ID)   │
│  ├── Status                         │
│  ├── OrderLines []                  │
│  │   ├── ProductId (Reference)      │
│  │   ├── Quantity                   │
│  │   └── Price                      │
│  └── ShippingAddress (Value Object) │
│                                     │
│  Invariants:                        │
│  - Order must have at least 1 line  │
│  - Total must be positive           │
│  - Cannot modify after shipped      │
└─────────────────────────────────────┘
```

---

### Domain Events

**A Domain Event captures something that happened in the domain.**

| Characteristic    | Requirement                                             |
| ----------------- | ------------------------------------------------------- |
| **Past Tense**    | Named as past tense verb (OrderPlaced, PaymentReceived) |
| **Immutable**     | MUST NOT change after creation                          |
| **Contains Data** | Includes relevant data at time of occurrence            |
| **Timestamp**     | When the event occurred                                 |

#### Domain Event Uses

| Use Case                 | Description                          |
| ------------------------ | ------------------------------------ |
| **Integration**          | Communicate between bounded contexts |
| **Audit**                | Record what happened and when        |
| **Eventual Consistency** | Update related aggregates            |
| **Event Sourcing**       | Rebuild state from events            |

---

### Repositories

**A Repository mediates between domain and data mapping layers.**

| Characteristic           | Requirement                           |
| ------------------------ | ------------------------------------- |
| **Collection-like**      | Interface mimics in-memory collection |
| **Aggregate-focused**    | One repository per aggregate root     |
| **Persistence Ignorant** | Domain doesn't know storage details   |

#### Repository Interface Pattern

```
interface OrderRepository:
    Order findById(OrderId id)
    List<Order> findByCustomer(CustomerId id)
    void save(Order order)
    void delete(Order order)

    // NOT: void saveOrderLine(OrderLine line)
    // OrderLines accessed via Order aggregate
```

---

### Domain Services

**A Domain Service contains domain logic that doesn't belong to any entity or value object.**

| Indicator                              | Use Domain Service |
| -------------------------------------- | ------------------ |
| Operation involves multiple aggregates | YES                |
| Logic doesn't fit naturally in entity  | YES                |
| Operation is stateless                 | YES                |
| Business concept without lifecycle     | YES                |

#### Domain Service vs Application Service

| Aspect           | Domain Service         | Application Service     |
| ---------------- | ---------------------- | ----------------------- |
| **Contains**     | Domain logic           | Orchestration           |
| **Dependencies** | Domain objects         | Domain services, repos  |
| **Transaction**  | No transaction control | Controls transactions   |
| **Example**      | PricingService         | OrderApplicationService |

---

## Identification Checklist

**MANDATORY: Check for DDD implementation:**

| Indicator                             | DDD Compliant | Required Action                 |
| ------------------------------------- | ------------- | ------------------------------- |
| Bounded contexts identified           | YES           | Continue                        |
| Single large model for everything     | NO            | **Identify context boundaries** |
| Ubiquitous language documented        | YES           | Continue                        |
| Technical terms in domain code        | NO            | **Use domain language**         |
| Aggregates have clear boundaries      | YES           | Continue                        |
| Entities referenced across aggregates | NO            | **Use ID references**           |
| Business rules in entities            | YES           | Continue                        |
| Business rules in services only       | NO            | **Move to domain objects**      |

---

## Anti-Patterns to Detect

| Anti-Pattern               | Description                             | Refactoring Action              |
| -------------------------- | --------------------------------------- | ------------------------------- |
| **Anemic Domain Model**    | Entities with only getters/setters      | **Move behavior into entities** |
| **Big Ball of Mud**        | No clear boundaries                     | **Identify bounded contexts**   |
| **Smart UI**               | Business logic in UI layer              | **Extract to domain layer**     |
| **Database-Driven Design** | Tables dictate model                    | **Model from domain, not data** |
| **CRUD Mentality**         | Everything is create/read/update/delete | **Think in domain operations**  |
| **Leaky Abstraction**      | Infrastructure in domain                | **Use anti-corruption layer**   |

---

## DDD Compliance Checklist

**MANDATORY: Before concluding DDD compliance, verify:**

```
[ ] Bounded contexts identified and documented
[ ] Context map shows relationships
[ ] Ubiquitous language glossary exists
[ ] Code uses domain language (not technical)
[ ] Aggregates have clear boundaries
[ ] Aggregate roots control access
[ ] Entities have identity and behavior
[ ] Value objects are immutable
[ ] Repositories are aggregate-focused
[ ] Domain events capture important occurrences
[ ] Application services are thin orchestrators
[ ] Domain services contain cross-aggregate logic
[ ] No anemic domain model
```

---

## Verification Questions

Before concluding DDD compliance, MUST answer:

1. Can domain experts read the code and understand it?
2. Are aggregate boundaries based on invariants?
3. Do entities encapsulate behavior, not just data?
4. Is the ubiquitous language consistent across code and documentation?

---

## Anti-Rationalization Table

| Rationalization                         | Why It's WRONG                        | Required Action                           |
| --------------------------------------- | ------------------------------------- | ----------------------------------------- |
| "We're too small for bounded contexts"  | Complexity, not size, determines need | **Evaluate domain complexity**            |
| "Anemic model is simpler"               | It scatters logic, increases coupling | **Encapsulate behavior in entities**      |
| "Database model works fine as domain"   | Data model ≠ domain model             | **Model from domain concepts**            |
| "Domain events add complexity"          | They make integration explicit        | **Use for cross-aggregate communication** |
| "One big model is easier to understand" | It becomes harder as it grows         | **Find natural boundaries**               |

---

## When to Use DDD

### Good Fit

| Indicator                    | DDD Appropriate |
| ---------------------------- | --------------- |
| Complex business domain      | YES             |
| Domain experts available     | YES             |
| Long-lived project           | YES             |
| Core business differentiator | YES             |

### Poor Fit

| Indicator                     | Consider Alternative       |
| ----------------------------- | -------------------------- |
| Simple CRUD application       | Use simpler architecture   |
| No domain complexity          | Overhead not justified     |
| No access to domain experts   | Hard to get language right |
| Short-lived/throwaway project | Simpler approach faster    |

---

## References

- Source: Eric Evans - "Domain-Driven Design: Tackling Complexity in the Heart of Software"
- Source: Vaughn Vernon - "Implementing Domain-Driven Design"
- Related: [VSA](vertical-slice-architecture.md) - Can be combined with DDD
- Related: [CQS](cqs-command-query-separation.md) - Complements aggregate design
- Related: [SOLID](solid-principles.md) - Applied within bounded contexts

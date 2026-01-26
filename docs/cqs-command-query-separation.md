# CQS - Command Query Separation

Reference documentation for Command Query Separation principle.

---

## Overview

**Command Query Separation (CQS)** states that every method MUST either be a command that performs an action OR a query that returns data, but NOT both.

| Aspect             | Description                                                      |
| ------------------ | ---------------------------------------------------------------- |
| **Core Principle** | Separate state-changing operations from state-reading operations |
| **Scope**          | Method/function design                                           |
| **Goal**           | Predictable, side-effect-free queries; explicit state changes    |
| **Origin**         | Bertrand Meyer - "Object-Oriented Software Construction"         |

---

## Definition

### The CQS Principle

> "Asking a question should not change the answer."

Every operation MUST be one of:

| Type        | Purpose      | Returns          | Side Effects |
| ----------- | ------------ | ---------------- | ------------ |
| **Command** | Change state | Void (or status) | YES          |
| **Query**   | Read state   | Data             | NO           |

---

## Core Rules

### Rule 1: Queries MUST NOT Mutate State

**A query MUST return a result and MUST NOT change observable state.**

| Query Behavior        | Allowed |
| --------------------- | ------- |
| Return data           | YES     |
| Return computed value | YES     |
| Read from database    | YES     |
| Modify database       | **NO**  |
| Modify object state   | **NO**  |
| Trigger side effects  | **NO**  |

### Rule 2: Commands MAY Change State

**A command performs an action and typically returns void (or status/error).**

| Command Behavior     | Allowed           |
| -------------------- | ----------------- |
| Modify database      | YES               |
| Modify object state  | YES               |
| Trigger side effects | YES               |
| Return void          | YES (preferred)   |
| Return status/ID     | YES (acceptable)  |
| Return full entity   | **AVOID** (mixed) |

### Rule 3: Separation MUST Be Explicit

| Method Naming                                              | Type    | Example                          |
| ---------------------------------------------------------- | ------- | -------------------------------- |
| `Get*`, `Find*`, `List*`, `Is*`, `Has*`, `Count*`          | Query   | `GetUser()`, `IsValid()`         |
| `Create*`, `Update*`, `Delete*`, `Set*`, `Add*`, `Remove*` | Command | `CreateUser()`, `UpdateStatus()` |

---

## Identification Checklist

**MANDATORY: Check for CQS violations:**

| Indicator                                     | CQS Violation | Required Action                          |
| --------------------------------------------- | ------------- | ---------------------------------------- |
| Method named `Get*` but modifies state        | YES           | **Split into query + command**           |
| Method returns data AND modifies state        | YES           | **Separate responsibilities**            |
| Query method has void return but side effects | YES           | **Rename or redesign**                   |
| "Getter" increments counter or logs access    | YES           | **Move side effect to explicit command** |
| Method like `GetOrCreate`                     | YES           | **Split into `Get` and `Create`**        |

---

## Common Violations

### 1. GetOrCreate Pattern

```
VIOLATION:
GetOrCreateUser(email)  // Returns user, but may create one

SOLUTION:
user = GetUser(email)
if user is null:
    user = CreateUser(email)  // Explicit command
    user = GetUser(email)     // Then query
```

### 2. Pop Operation

```
VIOLATION:
item = stack.Pop()  // Returns AND removes

SOLUTION (if CQS needed):
item = stack.Peek()    // Query: returns top
stack.Remove()         // Command: removes top
```

### 3. Increment and Return

```
VIOLATION:
newValue = counter.IncrementAndGet()

SOLUTION:
counter.Increment()        // Command
newValue = counter.Get()   // Query
```

### 4. Save and Return ID

```
OFTEN ACCEPTABLE:
id = repository.Save(entity)  // Returns generated ID

STRICT CQS:
repository.Save(entity)       // Command
id = entity.Id                // ID assigned during save
```

---

## CQS vs CQRS

| Aspect         | CQS                | CQRS                        |
| -------------- | ------------------ | --------------------------- |
| **Scope**      | Method level       | Architecture level          |
| **Separation** | Within same object | Separate models/stores      |
| **Complexity** | Low                | Higher                      |
| **Use Case**   | General design     | Scalability, event sourcing |

### CQS (Method Level)

```
class UserService:
    // Query
    def get_user(id) -> User

    // Command
    def update_user(id, data) -> void
```

### CQRS (Architecture Level)

```
// Separate services/databases
WriteService → WriteDatabase
ReadService  → ReadDatabase (optimized for queries)
```

---

## Benefits of CQS

| Benefit             | Explanation                                      |
| ------------------- | ------------------------------------------------ |
| **Predictability**  | Queries never cause unexpected changes           |
| **Testability**     | Queries are pure functions, easy to test         |
| **Caching**         | Queries can be freely cached                     |
| **Parallelization** | Queries can run concurrently without locks       |
| **Debugging**       | Clear separation aids troubleshooting            |
| **Optimization**    | Queries and commands can be optimized separately |

---

## When to Relax CQS

### Acceptable Exceptions

| Scenario                           | Why Acceptable                  |
| ---------------------------------- | ------------------------------- |
| **Generated IDs on insert**        | Practical necessity             |
| **Optimistic concurrency version** | Returns version for next update |
| **Stack/Queue pop**                | Well-understood semantic        |
| **Transaction result**             | Success/failure status          |

### Decision Matrix

| Question                                 | If YES             | If NO              |
| ---------------------------------------- | ------------------ | ------------------ |
| Does relaxing CQS add significant value? | Consider exception | **Strict CQS**     |
| Is the behavior well-understood by all?  | Consider exception | **Strict CQS**     |
| Does it simplify API significantly?      | Consider exception | **Strict CQS**     |
| Can it cause subtle bugs?                | **Strict CQS**     | Consider exception |

---

## Implementation Strategies

### 1. Naming Convention

| Prefix                         | Type    | Expectation                             |
| ------------------------------ | ------- | --------------------------------------- |
| `Get`, `Find`, `Fetch`, `Load` | Query   | Returns data, no side effects           |
| `Is`, `Has`, `Can`, `Should`   | Query   | Returns boolean, no side effects        |
| `Count`, `Sum`, `Calculate`    | Query   | Returns computed value, no side effects |
| `Create`, `Add`, `Insert`      | Command | Creates something, may return ID        |
| `Update`, `Modify`, `Change`   | Command | Modifies state                          |
| `Delete`, `Remove`, `Clear`    | Command | Removes something                       |
| `Send`, `Publish`, `Trigger`   | Command | Causes side effects                     |
| `Set`, `Assign`                | Command | Sets value                              |

### 2. Return Type Convention

| Return Type   | Expected Behavior           |
| ------------- | --------------------------- |
| `void`        | Command (modifies state)    |
| Data/Entity   | Query (no modification)     |
| `boolean`     | Query (predicate check)     |
| Status/Result | Command result (acceptable) |

### 3. Interface Segregation

```
// Separate interfaces for queries and commands
interface UserQueries:
    User GetUser(id)
    List<User> FindUsers(criteria)
    bool UserExists(id)

interface UserCommands:
    void CreateUser(data)
    void UpdateUser(id, data)
    void DeleteUser(id)
```

---

## Anti-Patterns to Detect

| Anti-Pattern                      | Description                         | Refactoring Action                   |
| --------------------------------- | ----------------------------------- | ------------------------------------ |
| **Getter with Side Effects**      | `GetUser()` also logs access        | **Move logging to explicit command** |
| **Lazy Initialization in Getter** | `GetConfig()` creates default       | **Initialize explicitly**            |
| **Query Modifying Cache**         | `GetData()` updates cache timestamp | **Use separate cache management**    |
| **Get-or-Create**                 | `GetOrCreateSession()`              | **Split into separate methods**      |
| **Increment-and-Return**          | `GetNextId()` that increments       | **Split into Increment + Get**       |

---

## CQS Compliance Checklist

**MANDATORY: Before concluding CQS compliance, verify:**

```
[ ] All query methods have return values (not void)
[ ] All query methods have no observable side effects
[ ] All command methods have void return (or status/ID only)
[ ] All command methods are explicitly named for mutation
[ ] No "GetOrCreate" type methods
[ ] No getters that modify state
[ ] Method names clearly indicate query vs command
[ ] No hidden side effects in query paths
```

---

## Verification Questions

Before concluding CQS compliance, MUST answer:

1. Can this query be called multiple times with same result?
2. Does this query modify any observable state?
3. Is this command's purpose clear from its name?
4. Could calling this query in a loop cause unexpected behavior?

---

## Anti-Rationalization Table

| Rationalization                                 | Why It's WRONG                            | Required Action                       |
| ----------------------------------------------- | ----------------------------------------- | ------------------------------------- |
| "Returning ID from save is convenient"          | It is. Document as intentional exception. | **Document or split**                 |
| "GetOrCreate is a common pattern"               | Common doesn't mean CQS-compliant         | **Split into Get + Create**           |
| "Logging in getter doesn't really change state" | Observable behavior changes               | **Move to explicit audit command**    |
| "It's just one small side effect"               | Side effects compound                     | **Separate command from query**       |
| "Users expect pop to return and remove"         | Expectation doesn't change principle      | **Document as exception or redesign** |

---

## Testing Implications

### Query Testing

| Property                      | Test Approach                    |
| ----------------------------- | -------------------------------- |
| **Idempotent**                | Call multiple times, same result |
| **Pure**                      | Same input → same output         |
| **No setup for side effects** | No need to verify state changes  |
| **Cacheable**                 | Can test with cached values      |

### Command Testing

| Property                        | Test Approach                  |
| ------------------------------- | ------------------------------ |
| **State change**                | Verify state before and after  |
| **Side effects**                | Mock/verify external calls     |
| **Idempotency (if applicable)** | Verify multiple calls behavior |
| **Error handling**              | Verify rollback/cleanup        |

---

## References

- Source: Bertrand Meyer - "Object-Oriented Software Construction"
- Related: [CQRS](vertical-slice-architecture.md) - Architecture-level extension
- Related: [SOLID](solid-principles.md) - SRP complements CQS
- Related: Functional Programming - Pure functions are queries

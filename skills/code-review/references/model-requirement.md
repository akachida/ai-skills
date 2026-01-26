# Reviewer Model Requirement

## Model Requirement

All reviewer agents require Claude Sonnet 4.5, Claude Opus 4.5, Gemini 3.0 Pro or higher.

### Self-Verification

If you are not Claude Sonnet 4.5, Claude Opus 4.5, Gemini 3.0 Pro or higher, stop immediately and report:

```
ERROR: Model requirement not met
Required: Claude Sonnet 4.5, Claude Opus 4.5, Gemini 3.0 Pro or higher
Current: [your model]
Action: Cannot proceed. Orchestrator must reinvoke with model="sonnet" or "opus"
```

### Why Opus is Required for Reviewers

| Review Capability               | Why It Requires Opus                                                                        |
| ------------------------------- | ------------------------------------------------------------------------------------------- |
| **Complex code tracing**        | Tracing data flows across components, following function calls, understanding state changes |
| **Pattern recognition**         | Identifying subtle design patterns, anti-patterns, and inconsistencies                      |
| **Mental execution**            | Walking through code with concrete scenarios to verify correctness                          |
| **Context integration**         | Understanding full file context, adjacent functions, ripple effects                         |
| **Security analysis**           | Identifying attack vectors, OWASP vulnerabilities, cryptographic weaknesses                 |
| **Business logic verification** | Tracing business rules, edge cases, state machine transitions                               |

**Domain-Specific Rationale:**

- **Code Reviewer:** Requires tracing algorithmic flow, context propagation, and codebase consistency patterns
- **Business Logic Reviewer:** Requires mental execution analysis with concrete scenarios and full file context
- **Security Reviewer:** Requires deep vulnerability detection, OWASP Top 10 coverage, and cryptographic evaluation
- **Test Reviewer:** Requires analyzing test quality, coverage gaps, and test anti-patterns

---

## Enforcement

No reviewer can proceed without model verification.

If invoked with wrong model, the agent must:

1. Not perform any review
2. Output the error message above
3. Return immediately

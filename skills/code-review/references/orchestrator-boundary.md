# Reviewer-Orchestrator Boundary

**Principle:** Reviewers report, agents fix.

This document defines the mandatory separation of responsibilities between reviewer agents and implementation agents.

---

## Reviewer Responsibilities

| Responsibility | Description                                | Action                                    |
| -------------- | ------------------------------------------ | ----------------------------------------- |
| **IDENTIFY**   | Find issues, vulnerabilities, logic errors | Document with file:line references        |
| **CLASSIFY**   | Assign severity (CRITICAL/HIGH/MEDIUM/LOW) | Use calibration table                     |
| **EXPLAIN**    | Describe problem and business impact       | Include attack vectors, failure scenarios |
| **RECOMMEND**  | Provide remediation guidance               | Show corrected code as EXAMPLE            |
| **REPORT**     | Generate structured verdict                | PASS/FAIL/NEEDS_DISCUSSION                |

---

## Reviewer Prohibitions

| Forbidden Action                   | Why It's Wrong                                         | Correct Action                               |
| ---------------------------------- | ------------------------------------------------------ | -------------------------------------------- |
| **Using Edit tool**                | Reviewers are inspectors, not mechanics                | Report issue → Orchestrator dispatches agent |
| **Using Create tool**              | File creation is implementation work                   | Recommend file structure in report           |
| **Running fix commands**           | Execution is orchestrator's job                        | Include commands as recommendations          |
| **Modifying code directly**        | Breaks separation of concerns                          | Document required changes in report          |
| **"I'll just fix this quickly"**   | Scope creep destroys review integrity                  | Report it. Let agent fix it.                 |
| **"Small fix, faster if I do it"** | Efficiency ≠ correctness. Process exists for a reason. | Report it. Re-run after fix.                 |

---

## Why This Separation Matters

### 1. Review Integrity

- Reviewer who fixes code cannot objectively verify the fix
- "I fixed it, so it's correct" is self-confirmation bias
- Fresh eyes on fixes catch issues the fixer missed

### 2. Specialization

- Reviewers optimize for FINDING issues (broad coverage, checklist discipline)
- Implementers optimize for FIXING issues (deep context, correct patterns)
- Mixed roles = diluted effectiveness

### 3. Audit Trail

- Clear separation creates traceable workflow
- Who found it (reviewer) ≠ Who fixed it (agent)
- Enables proper accountability and learning

### 4. Parallel Execution

- Reviewers run in parallel (code, business-logic, security, test, nil-safety)
- If reviewers also fixed, they'd need sequential execution
- Separation enables 5x faster review cycles

---

## Orchestrator Workflow After Review

When reviewers report issues, orchestrator must:

```
1. Aggregate all reviewer findings
2. Prioritize by severity (CRITICAL first)
3. Dispatch appropriate agent:
   - Security issues → security specialist or backend engineer
   - Logic issues → backend engineer
   - Code quality → refactoring specialist or original implementer
4. Verify fix by re-running all reviewers
5. Repeat until all reviewers PASS
```

---

## Anti-Rationalization Table for Reviewers

| Rationalization                                              | Why It's WRONG                                                                       | Required Action                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------ | -------------------------------------------------------- |
| "I'll fix this one-liner, it's trivial"                      | Trivial changes can introduce bugs. You're a reviewer, not an implementer.           | Report the issue. Do not edit.                           |
| "Faster if I just fix it now"                                | Speed ≠ correctness. The agent doing the fix should verify it compiles/passes tests. | Report the issue. Let agent fix + verify.                |
| "I know exactly how to fix this"                             | Knowing how ≠ having authority to do it. Your role is review.                        | Report with recommendation. Do not implement.            |
| "It's my code anyway, I can fix my own issues"               | Self-review of self-fixes is invalid. Fresh eyes required.                           | Report the issue. Different agent or re-review required. |
| "The orchestrator will take too long"                        | Process overhead is cheaper than bugs. Trust the workflow.                           | Report and wait. Orchestrator handles dispatch.          |
| "This security issue is urgent, I should fix it immediately" | Urgency doesn't change your role. Report CRITICAL, orchestrator prioritizes.         | Report as CRITICAL. Orchestrator fast-tracks fix.        |

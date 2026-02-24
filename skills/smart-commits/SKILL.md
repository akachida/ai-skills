---
name: smart-commits
description: Organize and create atomic git commits with intelligent change grouping. Invoke when users ask to commit changes, create commits, organize working directory, or clean up git history. Handles single and multi-commit workflows with conventional commit formatting.
disable-model-invocation: true
argument-hint: "[optional commit message]"
---

# Smart Commits Skill

## Your Role

You are a Senior Software Engineer specializing in version control best practices.

- **Position:** Expert in creating clean, atomic, bisectable commit histories
- **Purpose:** Transform messy working directories into logical, well-organized commit sequences
- **Method:** Analyze → Group → Order → Confirm → Execute → Verify
- **CRITICAL:** This is an **action skill** — you create commits directly. You MUST get user confirmation before executing.

---

## Shared Patterns

Reference these canonical sources. Do not duplicate their content.

| Pattern              | Location                                                                    | Purpose                   |
| -------------------- | --------------------------------------------------------------------------- | ------------------------- |
| Model requirements   | [model-requirement.md](../code-review/references/model-requirement.md)      | Self-verification         |
| Anti-rationalization | [anti-rationalization.md](../code-review/references/anti-rationalization.md) | Prevent shortcuts         |
| Blocker criteria     | [blocker-criteria.md](../code-review/references/blocker-criteria.md)        | When to stop and escalate |
| Pressure resistance  | [pressure-resistance.md](../code-review/references/pressure-resistance.md)  | Handle user pressure      |

**Excluded shared references (with reasons):**

| Reference                  | Reason for exclusion                                     |
| -------------------------- | -------------------------------------------------------- |
| `orchestrator-boundary.md` | Action skill — creates commits, does not report findings |
| `output-schema-core.md`   | Uses its own output format for commit results            |
| `severity-calibration.md` | Uses commit-specific quality criteria below              |
| `when-not-needed.md`      | Not a review skill — invoked explicitly by user          |
| `ai-slop-detection.md`    | Commit messages are user-facing, not AI review output    |

---

## Model Requirements

See [model-requirement.md](../code-review/references/model-requirement.md) for full details.

### Self-Verification

If you are not Claude Sonnet 4.5, Claude Opus 4.5, Gemini 3.0 Pro or higher, stop immediately and report:

```
ERROR: Model requirement not met
Required: Claude Sonnet 4.5, Claude Opus 4.5, Gemini 3.0 Pro or higher
Current: [your model]
Action: Cannot proceed. Reinvoke with model="sonnet" or "opus"
```

---

## Mandatory Rules

**These rules MUST be followed for every commit:**

1. **MUST use Conventional Commits v1.0.0:**
   - Reference: [conventional-commits.md](../../docs/conventional-commits.md)
2. **Commit message body MUST be clean and professional:**
   - Only the actual commit description
   - No metadata, signatures, hashtags, or internal references in the body
3. **MUST NOT use `--no-verify`** — run pre-commit hooks unless user explicitly requests otherwise
4. **MUST get user confirmation** before executing any commit

---

## Grouping Principles

| Principle           | Description                                     |
| ------------------- | ----------------------------------------------- |
| **Feature + Tests** | Implementation and its tests go together        |
| **Config Changes**  | package.json, tsconfig, etc. grouped separately |
| **Documentation**   | README, docs/ changes grouped together          |
| **Refactoring**     | Pure refactors (no behavior change) separate    |
| **Bug Fixes**       | Each fix is atomic with its test                |

---

## Single vs Multiple Commits

**Single commit when:**

- All changes are for one coherent feature/fix
- User provides a specific message via `$ARGUMENTS`
- Changes are minimal and related

**Multiple commits when:**

- Changes span different concerns (feature + docs + deps)
- Mix of features, fixes, and chores
- Better git history benefits future archaeology

---

## Commit Process

### Step 1: Gather Context

Run these commands in parallel to understand the current state:

```bash
# Check staged and unstaged changes
git status

# View ALL changes (staged and unstaged)
git diff
git diff --cached

# View recent commits for style reference
git log --oneline -10
```

### Step 2: Analyze and Group Changes

**For each changed file, determine:**

1. **Type**: feat, fix, chore, docs, refactor, test, style, perf, ci, build
2. **Scope**: Component or area affected (auth, api, ui, etc.)
3. **Logical group**: What other files belong with this change?

**Grouping heuristics:**

**NOTE:** This example is for illustration only. Actual grouping MUST be based on the actual changes and the project's programming language conventions.

| File Pattern                  | Likely Group                   |
| ----------------------------- | ------------------------------ |
| `*.test.ts`, `*.spec.ts`      | Group with implementation file |
| `package.json`, `*-lock.json` | Dependency changes             |
| `*.md`, `docs/*`              | Documentation                  |
| `*.config.*`, `tsconfig.*`    | Configuration                  |
| Same directory/module         | Often related                  |

**Create a mental (or actual) grouping:**

```
Group 1 (feat): auth changes
  - src/auth/oauth.ts
  - src/auth/oauth.test.ts

Group 2 (chore): dependencies
  - package.json
  - package-lock.json

Group 3 (docs): documentation
  - README.md
```

### Step 3: Determine Commit Order

**Order matters for bisectability:**

1. **Dependencies first** - So subsequent commits can use them
2. **Core changes** - Implementation before consumers
3. **Tests with implementation** - Keep them atomic
4. **Documentation last** - Documents the final state

### Step 4: Present Plan and Confirm

**MANDATORY: Get user confirmation before executing.**

Present the plan:

```
Proposed Commit Plan:
─────────────────────
1. feat(auth): add OAuth2 refresh token support
   - src/auth/oauth.ts (modified)
   - src/auth/oauth.test.ts (modified)

2. chore(deps): update authentication dependencies
   - package.json (modified)
   - package-lock.json (modified)

3. docs: update OAuth2 setup guide
   - docs/auth/oauth-setup.md (modified)

How should I proceed?
  1. Execute plan
  2. Single commit
  3. Let me review it
```

If user selects "Let me review it", show the full plan with files per commit.

### Step 5: Execute Commits

**For each commit group, in order:**

Stage only the files for this commit:

```bash
git add <file1> <file2> ...
```

### Step 6: Verify Commits

After all commits, verify the result:

```bash
# Show all new commits
git log --oneline -<number_of_commits>

# Confirm clean state
git status
```

### Step 7: Offer Push (Optional)

After successful commits, ask if the user wants to push:

- If branch already tracks a remote branch:

```bash
git push
```

- If branch has no upstream:

```bash
git push -u origin <current-branch>
```

---

## When User Provides Message

If the user provides a commit message via `$ARGUMENTS`:

1. **Single commit mode** — skip grouping analysis, use provided message
2. Use their message as the subject/body
3. Ensure proper formatting (50 char subject line, wrapped body at 72 chars)
4. Create the commit

---

## Blocker Criteria

See [blocker-criteria.md](../code-review/references/blocker-criteria.md) for general escalation protocol.

### STOP and Report When

| Blocker                                 | Action                                                                       |
| --------------------------------------- | ---------------------------------------------------------------------------- |
| **No changes detected**                 | STOP — report: "No staged or unstaged changes found."                        |
| **Merge conflicts present**             | STOP — report: "Resolve merge conflicts before committing."                  |
| **Unclear grouping**                    | STOP — ask: "How should I group these changes?"                              |
| **User rejects plan**                   | STOP — ask for revised instructions                                          |
| **Pre-commit hooks fail**               | STOP — report hook errors, do not bypass with `--no-verify`                  |
| **Mixed staged/unstaged for same file** | STOP — ask: "File X has both staged and unstaged changes. Which to include?" |

### Can Decide Independently

| Area                | What You Can Decide                        |
| ------------------- | ------------------------------------------ |
| **Commit type**     | Classify as feat, fix, chore, docs, etc.   |
| **Scope detection** | Determine scope from file paths            |
| **Grouping**        | Cluster related files into logical commits |
| **Commit order**    | Sequence commits for bisectability         |
| **Message wording** | Draft descriptive commit messages          |

---

## Anti-Rationalization Table

See [anti-rationalization.md](../code-review/references/anti-rationalization.md) for universal anti-rationalizations.

| Rationalization                          | Why It's Wrong                              | Required Action                               |
| ---------------------------------------- | ------------------------------------------- | --------------------------------------------- |
| "All changes are related, single commit" | Grouping requires analysis, not assumption  | **Analyze each file before deciding**         |
| "Skip confirmation, changes are obvious" | User confirmation is mandatory              | **Present plan and wait for approval**        |
| "Use --no-verify, hooks are slow"        | Hooks catch real issues                     | **Run hooks unless user explicitly requests** |
| "Message is good enough"                 | Commit messages are permanent documentation | **Follow Conventional Commits format**        |
| "No need to verify after committing"     | Verification catches staging errors         | **Run git log and git status after commits**  |
| "User said 'just commit it'"             | Process is non-negotiable                   | **Analyze, confirm type/scope, then commit**  |
| "Unstaged changes can be ignored"        | User may have forgotten to stage            | **Report all changes, ask what to include**   |
| "Order doesn't matter"                   | Order affects bisectability                 | **Follow dependency-first ordering**          |

---

## Severity Calibration

Commit quality issues classified by impact:

| Severity     | Examples                                                                                 |
| ------------ | ---------------------------------------------------------------------------------------- |
| **CRITICAL** | Wrong files committed, sensitive data committed, commits to wrong branch                 |
| **HIGH**     | Incorrect commit type, mixed concerns in single commit, missing files from logical group |
| **MEDIUM**   | Suboptimal commit order, vague commit message, scope inconsistency                       |
| **LOW**      | Minor message wording improvements, cosmetic grouping preferences                        |

---

## Output Format

After completing commits, provide:

```markdown
## Commit Summary

### Commits Created

| # | Hash    | Message                                      |
| - | ------- | -------------------------------------------- |
| 1 | abc1234 | feat(auth): add OAuth2 refresh token support |
| 2 | def5678 | chore(deps): update auth dependencies        |
| 3 | ghi9012 | docs: update OAuth2 setup guide              |

### Working Directory Status

[Output of `git status` — confirm clean state or report remaining changes]

### Push Status

[Pushed / Not pushed — awaiting user decision]
```

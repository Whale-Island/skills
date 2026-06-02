# Entry B: Case File Resume Workflow

## Objective
Fast-track entry for resuming work on an existing case file. Skip Phase 0 (Complexity Assessment) entirely — the case file already exists, which means the problem was previously assessed as complex.

## Trigger Model: Intent-Based, Not Keyword-Based

This entry uses a **three-layer trigger model** — from concrete signals to abstract intent. Do NOT rely on keyword matching; rely on **understanding the user's intent**.

### Layer 1: Hard Signals (Deterministic — always trigger)

| Signal | Detection Method |
|--------|-----------------|
| User references `Documents/99_CaseFiles/` path | File path in user message |
| User opens a file under `Documents/99_CaseFiles/` in IDE | IDE file-open event |
| User mentions a case file by its exact name | Name matches a folder in CaseFiles/ |

### Layer 2: Continuation Intent (Semantic — model judges intent)

The user's intent is to **continue, resume, or follow up on prior work** — regardless of exact wording. This covers ANY expression of:

- Continuing unfinished work ("继续修复", "接着搞", "还没修完", "上次搞到一半")
- Referencing a prior problem ("那个地图问题", "之前那个bug", "上次的问题")
- Asking about progress ("修到哪了？", "现在什么状态？")
- Requesting to resume a tracked problem ("恢复专题", "继续专题案", "接着上次的")
- Any implied continuation ("再试试", "换个方案", "上次那个不行")

**Key principle**: If the user's words imply they've discussed this problem before and want to continue, this is a continuation intent. Do NOT require the user to say "专题案" or "案例文件" — those are internal terminology, not user-facing language.

### Layer 3: Problem Feature Signals (New problem — may lead to case file creation)

When a NEW problem exhibits complex features (cascading failures, multi-system, fix causes new issues), Phase 0 may create a case file. This is Entry A's responsibility.

## Automatic Case File Detection (MANDATORY)

**When ANY continuation intent is detected (Layer 2), proactively check for matching case files:**

1. List all case files: `LS Documents/99_CaseFiles/`
2. For each case file, read `README.md` Problem Summary
3. Match user's description against case file summaries
4. If a match is found → proceed to Resume Protocol below
5. If no match → the problem may still be complex; fall back to Entry A (Phase 0)

This detection step is what makes the trigger flexible — the user doesn't need to know about case files. The system discovers them automatically based on intent.

## Resume Protocol

### Step 1: Identify the Case File

1. If the user specified a case file name or path, locate it directly:
   - Path pattern: `Documents/99_CaseFiles/[case-name]/README.md`
2. If the user's request is ambiguous (e.g., "继续修复那个地图问题"), use Automatic Case File Detection above
3. If multiple case files could match, ask user which one
4. If no case file exists for the mentioned problem, fall back to Entry A (Phase 0)

### Step 2: Read Case File State (MANDATORY)

Read the following files IN ORDER before any other action:

| Order | File | Purpose |
|-------|------|---------|
| 1 | `README.md` | Current status, what works, what's broken, next steps |
| 2 | `05_timeline.md` | What happened in previous sessions (read last 2-3 sessions) |
| 3 | `01_background.md` | Root cause chain, symptoms, domain classification |
| 4 | Latest in `04_solutions/` | Most recent approach and its result |

### Step 3: Confirm Understanding with User

Use `AskUserQuestion` to confirm:

```
AskUserQuestion:
  header: "Case Resume"
  question: "已读取 [case-name] 案例文件。当前状态：[status from README]。上次进展：[last session summary]。是否有新的信息或变更？"
  options:
    - "无新信息，按计划继续下一步"
    - "有新信息（我会描述）"
    - "问题已解决，关闭案例"
```

### Step 4: Determine Resume Point

Based on the case file state and user input, determine where to resume:

| Case Status | Resume Point | Action |
|-------------|-------------|--------|
| Investigating | Phase 1 | Continue problem decomposition |
| In Progress (hypothesis pending) | Phase 2 | Generate next hypothesis |
| In Progress (experiment pending) | Phase 3 | Execute planned experiment |
| In Progress (verification pending) | Phase 4 | Verify latest fix |
| Partially Resolved | Phase 5 | Iterate on remaining issues |
| New information provided | Phase 1 | Re-decompose with new info |

### Step 5: Resume Work

1. Load the workflow file for the determined resume point
2. Continue the multi-round verification protocol from where the last session left off
3. Update the case file as work progresses (per `workflows/case-file-management.md`)

## Decision Flow

```
User expresses ANY continuation intent (Layer 2) or provides hard signal (Layer 1)
  │
  ├── Hard signal (file path / case name)?
  │     ├── YES → Locate case file directly → Resume Protocol
  │     └── NO ↓
  │
  ├── Automatic Case File Detection
  │     │
  │     ├── Matching case file found?
  │     │     ├── YES → Ask user: "找到案例文件 [name]（[summary]），是否恢复？"
  │     │     │         ├── YES → Resume Protocol
  │     │     │         └── NO → Entry A (Phase 0)
  │     │     └── NO → Entry A (Phase 0) — problem may still be complex
  │     │
  │     └── No case files exist at all → Entry A (Phase 0)
```

## Anti-Patterns

| Anti-Pattern | Why It's Wrong | Correct Approach |
|-------------|---------------|-----------------|
| Requiring user to say "专题案" or "案例文件" | Users don't know internal terminology | Detect intent, not keywords |
| Re-reading Phase 0 for a resumed case | The problem was already assessed as complex | Skip directly to Step 2 |
| Ignoring previous landmines | Repeating failed approaches wastes time | Always read 04_solutions/ for reverted fixes |
| Starting from scratch | Loses all accumulated context | Build on previous sessions' findings |
| Skipping README | Missing current state leads to wrong resume point | README is MANDATORY first read |
| Not checking CaseFiles/ on continuation intent | Misses opportunity to resume tracked work | Always run Automatic Case File Detection |

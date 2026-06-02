---
name: "complex-problem-solving"
description: "Game dev complex problem solving with iterative verification, multi-approach comparison, and case file tracking. Invoke when: (1) facing cascading failures, unclear root causes, multi-system interactions, fixes that cause new issues, or problems requiring long-term tracking; (2) user's intent is to continue, resume, or follow up on a previously discussed complex problem — any wording expressing continuation of prior work (e.g., 继续修复, 接着搞, 上次没修完, 那个问题, 继续专题案); (3) user references or opens files under Documents/99_CaseFiles/, or mentions a case file by name. Also use when diagnose skill's single-fix approach is insufficient. On any continuation intent, proactively check Documents/99_CaseFiles/ for matching case files. 【互斥触发】当用户报告具体单点bug/崩溃/报错时使用 diagnose 技能；当问题涉及多维度分析、跨领域知识整合、需要长期跟踪时使用本技能。Do NOT use when a single-point bug can be diagnosed with a quick feedback loop (use diagnose)."
---

# Game Development Complex Problem Solving Framework

A disciplined, iterative methodology for solving complex, multi-variable problems in game development. Applicable to rendering, physics, gameplay, UI, networking, performance, and any domain where simple fixes cascade into new failures.

## 职责边界

| 技能 | 适用场景 | 关键区别 |
|------|---------|---------|
| **complex-problem-solving（本技能）** | 多系统交互问题、级联失败、修复引发新问题、需长期跟踪 | 多维度分析、多方案对比、专题案跟踪 |
| diagnose | 运行时bug、崩溃、性能回归 | 需要运行时证据，单点问题，快速反馈循环 |

当 diagnose 的单点修复引发新问题时，应转交本技能。当本技能定位到具体单点根因后，可使用 diagnose 的反馈循环快速验证。

## 触发规格

### 关键词信号（Recall层 — 保证不遗漏）
- 显式关键词: [级联故障, 多系统交互, 根因不明, 修复引发新问题, 继续修复, 接着搞, 上次没修完, 那个问题, CaseFiles, 专题案]
- 文件模式: [Documents/99_CaseFiles/**]
- 技术栈标记: [troubleshooting, root-cause-analysis]

### 意图信号（Precision层 — 保证不误触）
- 核心动词: [investigate, trace, correlate, track]
- 意图模式: [多维度根因分析, 跨系统问题追踪, 长期问题跟踪, 继续之前未完成的修复]

### 负向条件（互斥层 — 否决权）
- 单点bug可快速诊断 → diagnose

## Input / Output

Input:
- problemDescription: string # 问题描述
- caseFileId?: string # 专题案ID，续接时提供

Output:
- caseFilePath: string # 专题案文件路径
- rootCause?: string # 根因
- resolution?: string # 解决方案

## Core Principles

### 1. Diagnose Before Acting
- **DO**: Read debug logs, inspect runtime data, trace code paths before any change
- **DO**: Use `mcp_Sequential_Thinking_sequentialthinking` to reason before coding
- **DON'T**: Change code based on assumptions without verifying (Assumption-Driven Development)
- If you cannot explain the bug in one sentence, you do not understand it well enough to fix it

### 2. One Variable at a Time
- **DO**: Change ONE thing, test, observe, then change the next
- **DO**: State a clear hypothesis: "If I change X, then Y should happen because Z"
- **DON'T**: Make multiple changes at once hoping something works (Shotgun Debugging)
- If the hypothesis is wrong, revert immediately

### 3. Distinguish Symptoms from Root Causes
- **DO**: Apply "5 Whys" — ask "Why does this happen?" at least 3 times
- **DO**: Prioritize root causes that explain multiple symptoms
- **DON'T**: Add workarounds for downstream effects instead of fixing the upstream issue
- Fixing symptoms without addressing root causes guarantees recurrence

### 4. Check Assumptions Against Reality
- **DO**: Read config data, verify previous fixes in logs, ask user to describe specifically
- **DON'T**: Assume "small change = small impact" — a 1-line change can break everything
- Do not assume the user sees what you see — confirm observations

### 5. Revert Quickly When Things Get Worse
- **DO**: REVERT IMMEDIATELY if a fix makes things worse
- **DON'T**: Try to fix the fix (Cascading Fixes: Fix A breaks B → fix C for B → breaks D)
- Sunk cost fallacy: time spent on approach X does not justify continuing a failed approach

### 6. Fix at the Source, Not at the Sink
- **DO**: Fix the generation/creation logic to produce correct data
- **DON'T**: Apply post-hoc filters/trims/cleanup to data that was generated incorrectly
- Post-hoc trimming is a SYMPTOM of insufficient generation control, not a valid fix
- Example: If nodes have too many connections, fix the connection GENERATION logic (add limits at each addition point), don't trim the adjacency list after the fact
- Rationale: Post-hoc trimming destroys data integrity — it cannot know which connections are structurally essential vs. which are redundant. Only the generation logic has this context.

### 7. Understand the Domain Constraints Before Coding
- **DO**: Ask the user to clarify domain rules before implementing (e.g., "can a node have more than 4 connections?")
- **DO**: Document domain constraints as explicit rules before designing a solution
- **DO**: Read the design document FIRST before making any changes — the design doc defines what "correct" looks like
- **DON'T**: Assume you understand the constraints from observing the current behavior
- **DON'T**: Implement a "fix" that violates domain constraints (e.g., trimming connections below the minimum needed for connectivity)
- **CRITICAL LESSON**: Visual adjacency = data adjacency. If two nodes aren't visually adjacent (physically next to each other), they shouldn't be connected in the data layer. Connection lines must be straight — if a line needs to bend, the nodes shouldn't be directly connected.
- **CRITICAL LESSON**: Derive data from physical layout, not the other way around. Don't pre-define an adjacency graph and then try to place nodes to satisfy it. Place nodes first, then derive adjacencies from physical proximity.

### 8. Iterate Until Fully Resolved (Multi-Round Verification)
- **DO**: Continue iterating until ALL identified issues are resolved, not just the primary symptom
- **DO**: After each fix, re-verify the ENTIRE problem scope (not just the part you fixed)
- **DO**: Use `AskUserQuestion` at each iteration checkpoint to confirm progress and get user feedback
- **DON'T**: Declare "fixed" after resolving one symptom while others remain
- **DON'T**: Skip verification because "it should work" — always run actual tests
- **MANDATORY**: After each fix round, ask user to test and report results before proceeding

## Multi-Round Verification Protocol

### Definition of "Fully Resolved"
A problem is **fully resolved** only when:
1. **All original symptoms are eliminated** — not just the primary one
2. **No new symptoms introduced** — the fix didn't create new issues
3. **User confirms the fix** — actual testing by user, not just code review
4. **Verification commands pass** — build, tests, lint all succeed

### Iteration Cycle (MANDATORY)

```
┌─────────────────────────────────────────────────────────────────┐
│                    MULTI-ROUND VERIFICATION                     │
├─────────────────────────────────────────────────────────────────┤
│  Round N:                                                       │
│  1. Identify remaining issues (from user feedback or logs)     │
│  2. Generate hypothesis for next fix                           │
│  3. Implement fix (ONE variable at a time)                     │
│  4. Run verification (build + runtime test)                    │
│  5. AskUserQuestion: "Test results? Still broken?"             │
│     ├── User: "Fixed" → Verify all symptoms → Done if all OK   │
│     ├── User: "Still broken (detail)" → Loop to step 1         │
│     ├── User: "New issue (detail)" → Add to symptom list       │
│     └── User: "Worse" → REVERT → Loop to step 2 (new approach) │
│  6. Update case file with iteration results                    │
└─────────────────────────────────────────────────────────────────┘
```

### Mandatory User Interaction Points

| Point | Required Action |
|-------|-----------------|
| After each fix implementation | `AskUserQuestion` to get test results |
| After reverting a failed fix | `AskUserQuestion` to confirm revert was successful |
| When new symptoms appear | `AskUserQuestion` to get detailed description |
| Before declaring "resolved" | `AskUserQuestion` for final confirmation |
| When stuck (3+ failed approaches) | `AskUserQuestion` for user guidance |

### Iteration Guards

| Condition | Action |
|-----------|--------|
| User confirms "all fixed" | Mark resolved; update case file; DONE |
| User says "stop iterating" | Accept current best state; document remaining issues |
| 5+ iterations without progress | `AskUserQuestion`: escalate or reconsider scope |
| Same approach failed twice | Do NOT retry; mark as definitively failed |
| Fix makes things worse | REVERT IMMEDIATELY; document as landmine |

### Example Interaction Flow

```
[After Fix Round 1]
Agent: Implemented Fix A. Please test and report results.
User: "Still broken — now 8-direction instead of 4"
Agent: [Revert Fix A] That approach was wrong. Trying Fix B...
       AskUserQuestion: "What specifically is 8-direction? Hub internal or hub-to-hub?"

[After Fix Round 2]
Agent: Implemented Fix B (search from core only). Please test.
User: "4-direction OK now, but hubs too far apart"
Agent: [Keep Fix B] Partial success. Next: Fix C for hub spacing.
       AskUserQuestion: "How far is 'too far'? Specific distances?"

[After Fix Round 3]
Agent: Implemented Fix C (removed cascade push). Please test.
User: "Spacing OK, but connections are diagonal now"
Agent: [Keep Fix C] New symptom. Add to list. Next: Fix D for connections.

[After Fix Round 4]
Agent: Implemented Fix D. Please test.
User: "All fixed — 4-direction, proper spacing, cardinal connections"
Agent: [Verify all symptoms] Build pass? Runtime OK? 
       AskUserQuestion: "Final confirmation: all issues resolved?"
User: "Yes"
Agent: [Mark resolved] Update case file. DONE.
```

## Workflow Overview

The problem-solving process follows an iterative cycle with TWO entry points:

```
┌─────────────────────────────────────────────────────────────────┐
│  Entry A: New Problem                                           │
│  Phase 0: Complexity Assessment & Case File Decision            │
│  → Read: workflows/complexity-assessment.md                     │
├─────────────────────────────────────────────────────────────────┤
│  Entry B: Resume Existing Case File ⚡                          │
│  → Read: workflows/case-file-resume.md                          │
│  (When user references Documents/99_CaseFiles/ or says          │
│   继续专题案/恢复专题案/案例文件, skip Phase 0, go directly      │
│   to Session Start Protocol)                                    │
├─────────────────────────────────────────────────────────────────┤
│  Phase 1: Problem Decomposition                                 │
│  → Read: workflows/problem-decomposition.md                     │
├─────────────────────────────────────────────────────────────────┤
│  Phase 2: Hypothesis Generation                                 │
│  → Read: workflows/hypothesis-generation.md                     │
├─────────────────────────────────────────────────────────────────┤
│  Phase 3: Controlled Experiment                                 │
│  → Read: workflows/controlled-experiment.md                     │
├─────────────────────────────────────────────────────────────────┤
│  Phase 4: Verification                                          │
│  → Read: workflows/verification.md                              │
├─────────────────────────────────────────────────────────────────┤
│  Phase 5: Iteration & Approach Comparison                       │
│  → Read: workflows/iteration-loop.md                            │
└─────────────────────────────────────────────────────────────────┘
         ↑                                         │
         └──────── Loop Back ──────────────────────┘
```

**Critical**: Entry A (Phase 0) is the default entry for new problems. Entry B is the fast-track entry when a user references an existing case file — skip Phase 0 entirely and go directly to the Session Start Protocol defined in `workflows/case-file-resume.md`. At each phase transition, evaluate whether to proceed forward or loop back.

## Progressive Exposure — File Index

This skill uses a multi-file layered structure. Load files on demand to manage context:

### Level 0 — Always Loaded (This File)
| File | Purpose |
|------|---------|
| `SKILL.md` | Core principles, workflow overview, file index |

### Level 1 — Phase-Loaded (Read when entering that phase)
| File | When to Load |
|------|-------------|
| `workflows/complexity-assessment.md` | Phase 0: Receiving a problem report |
| `workflows/case-file-resume.md` | Entry B: Resuming an existing case file |
| `workflows/problem-decomposition.md` | Phase 1: Starting problem analysis |
| `workflows/hypothesis-generation.md` | Phase 2: Generating solution approaches |
| `workflows/controlled-experiment.md` | Phase 3: Executing experiments |
| `workflows/verification.md` | Phase 4: Verifying results |
| `workflows/iteration-loop.md` | Phase 5: Iterating or comparing approaches |
| `workflows/case-file-management.md` | When a case file is active |

### Level 2 — On-Demand (Read when needed)
| File | When to Load |
|------|-------------|
| `templates/problem-state.md` | When tracking problem state across iterations |
| `templates/experiment-log.md` | When recording experiment results |
| `templates/approach-comparison.md` | When comparing multiple approaches |
| `templates/case-file-structure.md` | When creating or updating a case file |
| `domain-knowledge/game-dev-patterns.md` | When needing domain-specific decomposition hints |

## Integration Points

| Related Skill | Relationship | When to Hand Off |
|---------------|-------------|-----------------|
| **diagnose** | 互补 | 单点bug用diagnose；级联/多系统问题用本技能；diagnose修复引发新问题→转本技能 |
| **verification-before-completion** | 下游 | Phase 4验证通过后，用该技能做最终确认再提交 |
| **godot-csharp-patterns** | 知识源 | Phase 1分解时参考C#模式知识，避免常见陷阱 |
| **code-quality-checker** | 并行 | 性能/死代码问题可并行扫描，结果输入Phase 1分解 |

## Session Handoff Protocol

When ending a session on an unsolved problem:

1. Update the case file (if active) or project document with ALL current state
2. List every code change and its status (working / reverted / needs-revert)
3. Document the root cause chain as understood
4. List next steps with clear rationale
5. Note any "landmines" — changes that look right but cause subtle issues
6. Record which approaches were tried and failed (prevents repeating mistakes)

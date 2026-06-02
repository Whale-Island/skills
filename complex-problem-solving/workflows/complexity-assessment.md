# Phase 0: Complexity Assessment & Case File Decision

## Objective
Evaluate the complexity of an incoming problem report and decide whether to establish a formal Case File (专题案) for systematic tracking.

## When to Execute
This phase runs **immediately** when a problem is reported, before any analysis or code changes.

## Pre-Check: Existing Case File Detection

**Before starting complexity assessment**, check whether a case file already exists for this problem. This applies when the user's intent suggests they've discussed this problem before (any continuation intent — "继续修复", "接着搞", "上次那个问题", etc.):

1. List all case files: `LS Documents/99_CaseFiles/`
2. Compare the user's problem description against each case file's README.md Problem Summary
3. If a matching case file is found:
   - **Do NOT proceed with Phase 0** — the problem was already assessed as complex
   - Switch to Entry B: Read `workflows/case-file-resume.md` and follow the Resume Protocol
   - Ask user: "找到案例文件 [name]（[summary]），是否恢复？"
4. If no matching case file exists, proceed with the complexity assessment below

This pre-check prevents re-assessing a problem that already has a case file, saving time and preserving accumulated context. The detection is **intent-based, not keyword-based** — if the user implies they've worked on this problem before, check for case files proactively.

## Complexity Indicators

Assess the problem against these indicators. If **2 or more** apply, the problem qualifies as complex:

| # | Indicator | Description |
|---|-----------|-------------|
| 1 | **Multi-Domain** | Problem spans 2+ game dev domains (e.g., rendering + gameplay, data + lifecycle) |
| 2 | **Unclear Root Cause** | Root cause is not immediately obvious after initial inspection |
| 3 | **Previous Fix Failure** | One or more attempted fixes have failed or caused new issues |
| 4 | **Cross-System Interaction** | Problem involves interaction between multiple independent systems |
| 5 | **High Technical Difficulty** | Involves performance optimization, concurrency, platform-specific behavior, or algorithmic complexity |
| 6 | **Long-Term Tracking Needed** | Problem is expected to require multiple sessions to resolve |
| 7 | **Cascading Impact** | Fixing this problem may affect other systems; high regression risk |
| 8 | **Data-Dependent** | Problem only manifests with specific data configurations or edge cases |

## Assessment Steps

### 0.1 Quick Triage
Based on the user's problem description, check each complexity indicator:
- Use `mcp_Sequential_Thinking_sequentialthinking` to evaluate systematically
- Count how many indicators apply
- If 0-1 indicators: simple problem, proceed directly to Phase 1 without a case file
- If 2+ indicators: complex problem, go to Step 0.2

### 0.2 Confirm Case File Decision with User
When the problem qualifies as complex, **MUST** use `AskUserQuestion` to confirm:

```
AskUserQuestion:
  header: "Case File"
  question: "This problem shows [N] complexity indicators: [list them]. I recommend creating a Case File (专题案) for systematic tracking across sessions. Should I proceed?"
  options:
    - "Yes, create a Case File (Recommended)"
    - "No, treat as a regular problem"
```

### 0.3 Create Case File (If Confirmed)
If the user confirms, immediately:
1. Read `templates/case-file-structure.md` for the folder structure and templates
2. Create the case file folder in the project: `Documents/99_CaseFiles/[problem-keyword]/`
3. Populate the initial files based on the templates
4. Record the case file path for future reference

### 0.4 Proceed to Phase 1
Whether or not a case file is created, proceed to Phase 1 (Problem Decomposition).

## Case File Activation Rules

A case file is **active** from creation until the problem is resolved. When a case file is active:

- **Before each session**: Read the case file's `README.md` to understand current state
- **During the session**: Update relevant case file sections as work progresses
- **After each session**: Update the timeline and progress sections
- **On resolution**: Compile the final case report

Read `workflows/case-file-management.md` for the complete case file lifecycle protocol.

## Simple Problem Fast Track

For problems with 0-1 complexity indicators:
- Skip case file creation
- Use `templates/problem-state.md` for lightweight tracking if needed
- Proceed directly to Phase 1

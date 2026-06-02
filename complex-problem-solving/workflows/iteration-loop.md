# Phase 5: Iteration Loop & Approach Comparison

## Objective
Evaluate the overall progress, decide whether to iterate, compare approaches, and determine the next action. **MANDATORY**: Continue iterating until ALL issues are fully resolved.

## Multi-Round Verification (Primary Protocol)

**This is the PRIMARY protocol for Phase 5. The decision framework below is a sub-process.**

### Definition of "Fully Resolved"
A problem is **fully resolved** only when:
1. **All original symptoms are eliminated** — not just the primary one
2. **No new symptoms introduced** — the fix didn't create new issues
3. **User confirms the fix** — actual testing by user, not just code review
4. **Verification commands pass** — build, tests, lint all succeed

### Mandatory Iteration Cycle

After Phase 4 verification, **ALWAYS** follow this cycle:

```
Round N (Current):
├── Step 1: Review user feedback from AskUserQuestion
├── Step 2: Check: Are ALL symptoms resolved?
│   ├── YES → AskUserQuestion: "Final confirmation?" → If YES → DONE
│   └── NO → Continue to Step 3
├── Step 3: Identify remaining/new symptoms
├── Step 4: Generate hypothesis for next fix
├── Step 5: Return to Phase 3 (Controlled Experiment)
└── Step 6: After Phase 4, loop back to Step 1
```

### Mandatory User Interaction

**After EVERY fix round**, use `AskUserQuestion` to get test results:

```
AskUserQuestion:
  header: "Test Results"
  question: "Please test the fix and report results. Are all issues resolved?"
  options:
    - "All fixed — no remaining issues"
    - "Partially fixed — [describe remaining issues]"
    - "New issue appeared — [describe new symptoms]"
    - "Worse than before — [describe regression]"
```

**CRITICAL**: Do NOT proceed to next fix without user testing feedback.

## Decision Framework (Sub-Process)

After receiving user feedback, evaluate:

```
Is the problem fully resolved?
├── YES → Mark as resolved. Update case file. Done.
├── PARTIALLY → Go to "Partial Resolution Protocol"
└── NO → Go to "Iteration Decision Tree"
```

## Partial Resolution Protocol

When the fix partially resolves the problem:

1. Document exactly what is fixed and what remains
2. Assess whether the remaining issues are:
   - **Same root cause, incomplete fix** → Adjust the current approach (stay in Phase 3)
   - **Different root cause** → Return to Phase 1 for new decomposition
   - **Side effects of the fix** → Evaluate severity; if minor, may accept; if major, revert
3. Use `AskUserQuestion` to confirm whether to continue iterating:
   ```
   AskUserQuestion:
     header: "Next Step"
     question: "The fix resolved [X] but [Y] remains. How should we proceed?"
     options:
       - "Continue iterating to fix remaining issues"
       - "Accept current state; remaining issues are acceptable"
       - "Revert and try a different approach"
   ```

## Iteration Decision Tree

When the fix did not resolve the problem:

```
Did the fix have ANY positive effect?
├── YES (partial improvement)
│   ├── Is the improvement in the right direction?
│   │   ├── YES → Refine the current approach (return to Phase 3)
│   │   └── NO → Revert and try next approach (return to Phase 2)
│   └── Are there new negative side effects?
│       ├── YES → Revert immediately; try next approach (return to Phase 2)
│       └── NO → Consider combining partial fix with another approach
├── NO (no effect)
│   ├── Was the hypothesis correct?
│   │   ├── UNCERTAIN → Return to Phase 1; re-examine root cause
│   │   └── YES → The implementation may be wrong; try next approach (Phase 2)
│   └── Have we tried all approaches?
│       ├── NO → Try next approach (return to Phase 2)
│       └── YES → Return to Phase 1; fundamental decomposition may be wrong
└── WORSE (negative effect)
    ├── REVERT IMMEDIATELY
    └── Return to Phase 2; document why this approach made things worse
```

## Multi-Approach Comparison Protocol

When two or more approaches have been tried, compare them systematically:

### Comparison Dimensions

| Dimension | Description |
|-----------|-------------|
| **Effectiveness** | How much of the target problem does it resolve? (0-100%) |
| **Side Effects** | What new issues does it introduce? (none / minor / major) |
| **Code Quality** | Does the change improve or degrade code quality? |
| **Reversibility** | How easy is it to undo? |
| **Composability** | Can it be combined with other approaches? |

### Comparison Process

1. Read `templates/approach-comparison.md` for the recording format
2. Fill in results for each approach tried
3. Score each approach on each dimension
4. Determine the winner, or whether a hybrid approach is viable

### Hybrid Approach Consideration

When no single approach fully resolves the problem:
- Consider whether Approach A's partial fix + Approach B's partial fix = complete fix
- This is ONLY valid if the two approaches do not conflict
- Test the hybrid as a new, single experiment in Phase 3

## Maximum Iteration Guard

To prevent infinite loops:

| Condition | Action |
|-----------|--------|
| 3+ approaches tried, all failed | Return to Phase 1; fundamental re-decomposition needed |
| Same approach tried twice with same result | Do NOT try again; document as definitively failed |
| 5+ total iterations without progress | Escalate to user; may need to reconsider problem scope |
| User says "stop iterating" | Accept current best state; document remaining issues |

## Update Case File (If Active)

If a case file exists for this problem, update after each iteration decision:

- Update `README.md`:
  - Current status (Investigating / In Progress / Partially Resolved / Resolved)
  - Current best state (what works, what doesn't)
  - Next steps with rationale
  - Assumptions needing validation
- Update `05_timeline.md`:
  - Add iteration decision entry
  - Record rationale for the decision
- If resolved, proceed to case closure (see `workflows/case-file-management.md`)

## Session Continuation

When a session ends with an unresolved problem:

1. If a case file is active, follow the Session End Protocol in `workflows/case-file-management.md`
2. Otherwise, update `templates/problem-state.md` with:
   - Current symptoms (original + any new ones)
   - All approaches tried and their results
   - Current best state (if any partial fix is applied)
   - Next steps with rationale
3. Record all "landmines" — changes that looked right but caused subtle issues
4. List any assumptions that need validation in the next session

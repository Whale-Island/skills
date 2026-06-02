# Phase 3: Controlled Experiment

## Objective
Execute a single-variable change, observe the result, and compare against the expected outcome documented in Phase 2.

## Pre-Experiment Checklist
Before making any code change:
- [ ] The hypothesis is clearly stated: "If I change X, then Y should happen because Z"
- [ ] Only ONE variable will be changed
- [ ] Expected outcome is documented (from Phase 2)
- [ ] Failure indicators are documented
- [ ] Revert criteria are documented
- [ ] Current code state is recorded (for potential revert)

## Steps

### 3.1 Implement the Change
- Make ONLY the change specified in the selected approach
- Do NOT add logging, debugging aids, or supplementary fixes
- Do NOT refactor nearby code "while you are here"
- Record the exact change made (file, line range, before/after)

### 3.2 Build and Run
Execute the project build and runtime verification:

**For Godot C# projects:**
1. Run `dotnet build` — must succeed with zero errors
2. If build fails, the experiment is invalid — fix the build error (this is a separate issue) or revert
3. Launch the game and navigate to the affected scenario
4. Observe the specific metrics defined in Phase 2

### 3.3 Collect Observations
Record what actually happened:
- Did the target symptom change? How?
- Did any new symptoms appear?
- Did any previously working functionality break?
- What do the logs show?

Use `templates/experiment-log.md` to record results.

### 3.4 Compare Against Expected Outcome

| Outcome | Action |
|---------|--------|
| **Matches expected** | Proceed to Phase 4 (Verification) |
| **Partially matches** | Analyze the gap; may need minor adjustment or new hypothesis |
| **Does not match** | REVERT the change; return to Phase 2 for next approach |
| **Made things worse** | REVERT IMMEDIATELY; return to Phase 2; document failure |

### 3.5 Revert Protocol
If revert criteria are met:
1. Undo the code change immediately
2. Re-run `dotnet build` to confirm revert is clean
3. Document what went wrong in the experiment log
4. Do NOT attempt to "fix the fix" — return to Phase 2

### 3.6 Update Case File (If Active)
If a case file exists for this problem:
- Update `04_solutions/[approach-name]_[YYYYMMDD].md` with:
  - Change details (what was actually changed)
  - Actual outcome
  - Side effects observed
  - Decision (success / partial / failed / worse / reverted)
- If new data was collected, add to `03_data-collection/[description]_[YYYYMMDD].md`
- Update `05_timeline.md` with experiment result

## Multi-Approach Trial Protocol
When multiple approaches exist (documented in Phase 2), and the first approach fails:

1. Revert the failed approach completely
2. Verify the codebase is back to baseline (build succeeds, original symptoms present)
3. Select the next approach from the prioritized list
4. Execute as a fresh experiment — do NOT carry over changes from the failed approach
5. Record results for comparison using `templates/approach-comparison.md`

## Loop-Back Triggers
- Build failure after change → fix build or revert, then retry
- Unexpected new symptoms → return to Phase 1 (may be a new root cause)
- Change has no effect → return to Phase 2 (hypothesis was wrong)
- Change makes things worse → REVERT, return to Phase 2

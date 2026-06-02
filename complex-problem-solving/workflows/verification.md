# Phase 4: Verification

## Objective
Confirm that the fix resolves the target problem without introducing regressions, using automated verification where possible and standardized user verification where automation is insufficient.

## Verification Pipeline

Verification proceeds through three tiers. Each tier must pass before proceeding to the next:

### Tier 1: Automated Build Verification
**Always execute. No exceptions.**

```
Command: dotnet build
Pass Criteria: Zero errors, zero warnings related to changed code
Fail Action: Fix build issues before any further verification
```

### Tier 2: Automated Runtime Verification
**Execute when the fix can be verified by code inspection or log analysis.**

| Verification Type | Method | When Applicable |
|-------------------|--------|-----------------|
| Log Analysis | Check Godot console output for errors/warnings | All fixes |
| Code Path Tracing | Use SearchCodebase/Grep to verify execution reaches the fix | Logic fixes |
| Data Validation | Read config files to verify values are correct | Config/data fixes |
| State Inspection | Check in-memory state via debug output | State management fixes |
| Regression Check | Run existing test suites if available | All fixes |

**Automated verification checklist:**
- [ ] `dotnet build` succeeds with zero errors
- [ ] No new errors or warnings in Godot console output
- [ ] The changed code path is reachable (verified by code search)
- [ ] No other code paths are broken by the change (verified by cross-reference search)
- [ ] Related configuration values are consistent (verified by reading config files)

### Tier 3: User Verification
**Required when automated verification cannot fully confirm the fix.**

This includes but is not limited to:
- Visual/UX changes (rendering, animation, layout)
- Gameplay feel changes (controls, timing, difficulty)
- Complex multi-system interactions
- Performance characteristics (frame rate, load time)

## Standardized User Verification Request Protocol

When user verification is needed, use `AskUserQuestion` with the following structure:

```
AskUserQuestion:
  header: "[Verification Type]"
  question: "[Specific, verifiable question about the expected behavior]"
  options:
    - "Confirmed: [expected behavior observed]"
    - "Partially: [some aspects work, details below]"
    - "Not working: [expected behavior not observed]"
```

**Rules for user verification requests:**
1. **Be specific**: NOT "Does it work?" BUT "Is the enemy now spawning at the correct position (near the portal)?"
2. **One question per verification**: Do not bundle multiple checks into one question
3. **Provide context**: Briefly state what was changed and what should be different
4. **Offer clear options**: Always include "Confirmed", "Partially", and "Not working" options
5. **Respect user feedback**: If user says "Not working", do NOT argue — accept and iterate

## Verification Confidence Scoring

After completing all applicable verification tiers, assign a confidence score:

| Score | Meaning | Action |
|-------|---------|--------|
| **High** | All automated checks pass + user confirms | Mark fix as verified |
| **Medium** | Automated checks pass + user partially confirms | Investigate partial issues; may need minor adjustment |
| **Low** | Automated checks pass but user cannot verify | Request more specific verification; consider adding debug output |
| **Failed** | Any automated check fails or user reports regression | REVERT and return to Phase 2 |

## Update Case File (If Active)
If a case file exists for this problem:
- Update `04_solutions/[approach-name]_[YYYYMMDD].md` with:
  - Verification results (build, automated, user)
  - Confidence score
  - Final decision on this approach
- Update `05_timeline.md` with verification outcome

## Handling Verification Failures

| Failure Type | Response |
|-------------|----------|
| Build failure | Fix build issue; this is a separate problem from the original |
| New runtime error | REVERT immediately; this is a regression |
| User reports regression | REVERT immediately; return to Phase 2 |
| User reports partial fix | Document what works and what doesn't; may need Phase 2 for remaining issues |
| Cannot reproduce | Add more logging/diagnostics; ask user for specific reproduction steps |

## Loop-Back Triggers
- Any tier fails → address the failure, then re-verify from the failed tier
- User reports new issues → return to Phase 1 (new symptoms may indicate new root cause)
- Partial fix confirmed → proceed to Phase 5 to decide whether to iterate or accept

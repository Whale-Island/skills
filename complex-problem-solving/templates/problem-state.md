# Problem State Tracking Template

Use this template to maintain a living document for the current problem. Update after every iteration.

## Problem Summary
- **Problem ID**: [short identifier, e.g., MAP-LAYOUT-001]
- **Date Opened**: [YYYY-MM-DD]
- **Domain**: [Rendering / Physics / Gameplay / UI / Data / Performance / Lifecycle / Networking]
- **Status**: [Investigating / In Progress / Partially Resolved / Resolved / Escalated]

## Symptoms
| # | Symptom | Severity | Observed | Root Cause (if identified) |
|---|---------|----------|----------|---------------------------|
| 1 | [description] | [Critical/High/Medium/Low] | [Always/Conditional/Random] | [linked root cause ID] |
| 2 | | | | |

## Root Causes
| # | Root Cause | Confidence | Unblocks Symptoms | Status |
|---|-----------|------------|-------------------|--------|
| 1 | [description] | [High/Medium/Low] | [S1, S2] | [Identified / Fixing / Fixed / Rejected] |
| 2 | | | | |

## Root Cause Chain
```
[Root Cause 1] → [Intermediate Effect] → [Symptom A]
                              ↓
                    [Intermediate Effect] → [Symptom B]
```

## Code Changes
| # | File | Change Description | Status | Approach ID |
|---|------|-------------------|--------|-------------|
| 1 | [path:lines] | [what was changed] | [Applied / Reverted / Needs-Revert] | [A1] |
| 2 | | | | |

## Approaches Tried
| # | Approach | Result | Iteration | Notes |
|---|----------|--------|-----------|-------|
| 1 | [brief description] | [Success / Partial / Failed / Worse] | [#] | [why it failed/succeeded] |
| 2 | | | | |

## Failed Approaches (Do NOT Retry)
| # | Approach | Why It Failed | Why It Looked Right |
|---|----------|---------------|-------------------|
| 1 | | | |

## Landmines
| # | Change | Why It Looks Right | Why It Breaks Things |
|---|--------|-------------------|---------------------|
| 1 | | | |

## Current Best State
- [ ] Original problem fully resolved
- [ ] Partial fix applied: [describe what works]
- [ ] No partial fix applied; codebase at baseline

## Next Steps
1. [specific next action with rationale]
2. [specific next action with rationale]

## Assumptions Needing Validation
- [ ] [assumption 1]
- [ ] [assumption 2]

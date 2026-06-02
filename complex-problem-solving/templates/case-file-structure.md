# Case File Structure & Templates

This document defines the folder structure and file content templates for Case Files (专题案). Case files are created in the project at `Documents/99_CaseFiles/[problem-keyword]/`.

## Folder Structure

```
Documents/99_CaseFiles/[problem-keyword]/
├── README.md                          ← Directory overview & current status
├── 01_background.md                   ← Problem background & symptoms
├── 02_technical-analysis/             ← Technical investigation artifacts
│   └── [analysis-topic]_[YYYYMMDD].md
├── 03_data-collection/                ← Data, logs, screenshots
│   └── [data-description]_[YYYYMMDD].md
├── 04_solutions/                      ← Solution attempts & results
│   └── [approach-name]_[YYYYMMDD].md
├── 05_timeline.md                     ← Timeline & progress tracking
└── 06_final-report.md                 ← Final case report (after resolution)
```

## File Templates

### README.md — Directory Overview

```markdown
# Case File: [Problem Keyword]

## Status
- **Status**: [Investigating / In Progress / Partially Resolved / Resolved]
- **Opened**: [YYYY-MM-DD]
- **Last Updated**: [YYYY-MM-DD]
- **Complexity Indicators**: [list from Phase 0]
- **Domains**: [Rendering / Physics / Gameplay / UI / Data / Performance / Lifecycle / Networking]

## Problem Summary
[1-2 sentence description of the core problem]

## Current State
- **What works**: [list of aspects that are functioning correctly]
- **What's broken**: [list of current symptoms]
- **Current best fix**: [description of any partially applied fix, or "None"]

## Next Steps
1. [specific next action with rationale]
2. [specific next action with rationale]

## Assumptions Needing Validation
- [ ] [assumption 1]
- [ ] [assumption 2]

## Related Case Files
- [None / link to related case files]

## Directory Contents
| File/Folder | Description |
|-------------|-------------|
| 01_background.md | Problem background & symptom details |
| 02_technical-analysis/ | Technical investigation artifacts |
| 03_data-collection/ | Collected data, logs, screenshots |
| 04_solutions/ | Solution attempts & results |
| 05_timeline.md | Chronological progress log |
| 06_final-report.md | Final case report (created on resolution) |
```

### 01_background.md — Problem Background & Symptoms

```markdown
# Problem Background: [Problem Keyword]

## Problem Description
[Detailed description of the problem as reported by the user]

## Symptoms
| # | Symptom | Severity | Observed | Since When |
|---|---------|----------|----------|------------|
| 1 | [description] | [Critical/High/Medium/Low] | [Always/Conditional/Random] | [date or version] |
| 2 | | | | |

## Root Causes Identified
| # | Root Cause | Confidence | Unblocks Symptoms | Status |
|---|-----------|------------|-------------------|--------|
| 1 | [description] | [High/Medium/Low] | [S1, S2] | [Identified/Fixing/Fixed/Rejected] |

## Root Cause Chain
```
[Root Cause] → [Intermediate Effect] → [Symptom]
                        ↓
              [Intermediate Effect] → [Symptom]
```

## Domain Classification
[Which game dev domains are involved and why]

## Previous Attempts (Before Case File)
| # | What Was Tried | Result | Why It Failed |
|---|---------------|--------|---------------|
| 1 | | | |

## Key Technical Difficulties
- [difficulty 1]
- [difficulty 2]

## Related Data & Logs
[References to files in 03_data-collection/]
```

### 02_technical-analysis/ — Technical Analysis Files

File naming: `[analysis-topic]_[YYYYMMDD].md`

```markdown
# Technical Analysis: [Topic]

**Date**: [YYYY-MM-DD]
**Related Root Cause**: [which root cause this analysis addresses]

## Analysis Objective
[What this analysis aims to determine]

## Code Paths Traced
| Entry Point | Path | Key Findings |
|-------------|------|-------------|
| [entry] | [path description] | [finding] |

## Data Examined
| Data Source | What Was Checked | Result |
|-------------|-----------------|--------|
| [file/table] | [check description] | [result] |

## Key Findings
1. [finding 1]
2. [finding 2]

## Implications for Solution
[How these findings affect approach selection]
```

### 03_data-collection/ — Data & Log Files

File naming: `[data-description]_[YYYYMMDD].md`

```markdown
# Data Collection: [Description]

**Date**: [YYYY-MM-DD]
**Collection Method**: [Runtime log / Config dump / Screenshot / Profiler output]

## Data
[Paste or reference the collected data here]

## Observations
[What this data tells us]

## Relevance
[How this data relates to the problem]
```

### 04_solutions/ — Solution Attempt Files

File naming: `[approach-name]_[YYYYMMDD].md`

```markdown
# Solution Attempt: [Approach Name]

**Date**: [YYYY-MM-DD]
**Approach Type**: [Conservative / Moderate / Aggressive]
**Target Root Cause**: [which root cause this addresses]

## Hypothesis
"If I change [X], then [Y] should happen because [Z]."

## Risk Assessment
| Dimension | Score (1-5) |
|-----------|-------------|
| Success Probability | |
| Risk of New Issues | |
| Reversibility | |
| Risk/Reward Ratio | |

## Change Details
- **File**: [path]
- **Lines**: [start-end]
- **Change**: [description of what was changed]

## Expected Outcome
[What should be different after the change]

## Actual Outcome
[What actually happened]

## Verification Results
- **Build**: [Pass/Fail]
- **Automated Checks**: [Pass/Fail with details]
- **User Verification**: [Confirmed/Partially/Not Working]

## Side Effects
| Side Effect | Severity | Action Needed |
|-------------|----------|---------------|
| [description] | [Critical/Minor/None] | [action] |

## Decision
- **Status**: [Success / Partial / Failed / Made Worse / Reverted]
- **Rationale**: [why this decision was made]
- **Next Step**: [what to try next]
```

### 05_timeline.md — Timeline & Progress

```markdown
# Timeline: [Problem Keyword]

## [YYYY-MM-DD] — Session Start
- **Action**: [Case file created / Session resumed]
- **Status at Start**: [status]
- **Planned Work**: [what was planned]

## [YYYY-MM-DD HH:MM] — [Event Description]
- **Phase**: [0/1/2/3/4/5]
- **What Happened**: [description]
- **Code Changes**: [file:lines, status]
- **Decision**: [what was decided and why]

## [YYYY-MM-DD] — Session End
- **Status at End**: [status]
- **What Was Accomplished**: [summary]
- **What Remains**: [remaining issues]
- **Next Steps**: [planned actions for next session]
- **Landmines Found**: [any dangerous findings]

---

[Repeat for each session]
```

### 06_final-report.md — Final Case Report

```markdown
# Final Case Report: [Problem Keyword]

**Opened**: [YYYY-MM-DD]
**Resolved**: [YYYY-MM-DD]
**Total Sessions**: [N]
**Total Approaches Tried**: [N]

## Problem Description
[Concise description of the original problem]

## Solution Process Summary
[Chronological summary of the investigation and fix attempts]

## Final Solution
[Description of the solution that actually worked]

### Change Details
| File | Change | Status |
|------|--------|--------|
| [path:lines] | [description] | [Applied] |

## Failed Approaches
| # | Approach | Why It Failed |
|---|----------|---------------|
| 1 | | |
| 2 | | |

## Lessons Learned
1. [lesson 1 — what we learned about the codebase/system]
2. [lesson 2 — what we learned about the debugging process]
3. [lesson 3 — what we would do differently]

## Prevention Recommendations
- [how to prevent this problem from recurring]
- [what safeguards or tests could catch this earlier]

## Key Takeaways for Similar Problems
[Generalizable insights that could help with future problems of this type]
```

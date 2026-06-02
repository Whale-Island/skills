# Experiment Log Template

Use this template to record each controlled experiment. One log entry per experiment.

## Experiment Record

### Identity
- **Experiment ID**: [E-001]
- **Date**: [YYYY-MM-DD]
- **Problem ID**: [linked problem ID]
- **Approach ID**: [linked approach ID]
- **Iteration**: [#]

### Hypothesis
"If I change [X], then [Y] should happen because [Z]."

### Change Made
- **File**: [path]
- **Lines**: [start-end]
- **Before**: [code or description of original logic]
- **After**: [code or description of changed logic]
- **Variables Changed**: [list the single variable changed]

### Expected Outcome
[What should be different after the change]

### Observable Metrics
| Metric | Expected Value | Actual Value | Match? |
|--------|---------------|-------------|--------|
| [metric 1] | [expected] | [actual] | [Yes/No] |
| [metric 2] | [expected] | [actual] | [Yes/No] |

### Actual Outcome
[What actually happened]

### Comparison
- [ ] Matches expected → Proceed to verification
- [ ] Partially matches → Gap analysis needed
- [ ] Does not match → Revert and try next approach
- [ ] Made things worse → Revert immediately

### Side Effects Observed
| Side Effect | Severity | Related to Change? |
|-------------|----------|-------------------|
| [description] | [Critical/Minor/None] | [Yes/No/Uncertain] |

### Decision
- **Action**: [Proceed / Adjust / Revert / Escalate]
- **Rationale**: [why this decision]
- **Next Step**: [specific next action]

### Build Result
- `dotnet build`: [Success / Failed]
- Errors: [count, if any]
- Warnings: [count, if any]

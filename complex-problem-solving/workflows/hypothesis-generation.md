# Phase 2: Hypothesis Generation

## Objective
Generate multiple viable solution approaches for each root cause, evaluate their risk/reward, and select the best starting approach.

## Steps

### 2.1 Generate Multiple Approaches
For each root cause identified in Phase 1, generate **at least 2** and preferably **3** distinct approaches:

- **Approach A (Conservative)**: Minimal change, lowest risk, may be partial fix
- **Approach B (Moderate)**: Balanced change, moderate risk, likely complete fix
- **Approach C (Aggressive)**: Significant refactor, higher risk, definitive fix

Each approach must include:
- What specifically to change (file, function, logic)
- Why this should fix the root cause (causal chain)
- What side effects might occur
- Whether it is reversible (can we easily undo it?)

### 2.2 Rate Each Approach
Score each approach on three dimensions (1-5 scale):

| Dimension | 1 (Low) | 5 (High) |
|-----------|---------|----------|
| **Success Probability** | Unlikely to fix | Almost certainly fixes |
| **Risk of New Issues** | Very safe | Likely to break something else |
| **Reversibility** | Cannot undo | Trivial to revert |

Calculate the **Risk/Reward Ratio**:
```
RRR = Success Probability / (Risk of New Issues × (6 - Reversibility))
```
Lower RRR = better. Prefer approaches with high success, low risk, high reversibility.

### 2.3 Document Expected Outcomes
For the selected approach, write down BEFORE implementing:
- **Expected outcome**: What should change after the fix
- **Observable metrics**: How to measure success (specific values, behaviors, log outputs)
- **Failure indicators**: What would indicate the fix failed or made things worse
- **Revert criteria**: Under what conditions to immediately revert

### 2.4 Handle Uncertainty
If you are uncertain about any aspect of the approach:

**MUST use `AskUserQuestion` to confirm:**
- Ambiguous root cause interpretation
- Multiple valid implementation strategies
- Uncertain side effect predictions
- User preferences between trade-offs

**Required confirmation format:**
```
AskUserQuestion:
  header: "Approach"
  question: "For root cause [X], I have [N] approaches. Approach [A] is [description]. Which should I try first?"
  options: ["Approach A (Recommended): [brief reason]", "Approach B: [brief reason]", "Approach C: [brief reason]"]
```

### 2.5 Record All Approaches
Use `templates/approach-comparison.md` to record all approaches and their ratings. Even approaches not selected now may be needed if the first approach fails.

### 2.6 Update Case File (If Active)
If a case file exists for this problem:
- Create `04_solutions/[approach-name]_[YYYYMMDD].md` for each approach generated
- Fill in hypothesis, risk assessment, and expected outcome sections
- Update `05_timeline.md` with hypothesis generation completion

## Output
- A prioritized list of approaches with ratings
- A selected approach with documented expected outcomes
- All alternative approaches preserved for potential fallback

## Loop-Back Triggers
- If Phase 3 shows the selected approach is fundamentally flawed → return to Phase 2, select next approach
- If Phase 3 reveals new root causes → return to Phase 1, re-decompose

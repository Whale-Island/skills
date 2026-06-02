# Phase 1: Problem Decomposition

## Objective
Transform a vague problem report into a structured, prioritized list of symptoms and root causes.

## Steps

### 1.1 Collect All Symptoms
- List EVERY observed symptom, not just the one the user mentioned
- Use `mcp_Sequential_Thinking_sequentialthinking` to organize findings
- For each symptom, record:
  - What the user observes
  - When it occurs (always / conditional / random)
  - What changed recently (code, config, data)

### 1.2 Trace Code Paths
For each symptom, trace the code path that produces it:
- Identify the entry point (user action, system event, timer callback)
- Follow the execution flow through all relevant systems
- Note where the flow deviates from expected behavior
- Use Grep, SearchCodebase, and Read tools to trace paths

### 1.3 Identify Shared Root Causes
- Group symptoms that share code paths or data dependencies
- Apply the "5 Whys" technique to each group:
  - Why does symptom A occur? → Because of B
  - Why does B occur? → Because of C
  - Continue until you reach a cause that is self-evident or external
- A single root cause often explains multiple symptoms — prioritize these

### 1.4 Classify Problem Domain
Determine which game development domain(s) the problem spans:

| Domain | Typical Indicators |
|--------|-------------------|
| Rendering | Visual artifacts, draw order issues, shader errors |
| Physics | Collision misses, tunneling, jitter, incorrect forces |
| Gameplay | Logic errors, state desync, rule violations |
| UI | Layout breaks, input not registering, stale display |
| Data/Config | Missing values, wrong types, cross-reference errors |
| Performance | Frame drops, memory growth, loading stalls |
| Lifecycle | Null references, double-init, cleanup failures |
| Networking | Desync, latency spikes, state inconsistency |

If uncertain about domain classification, read `domain-knowledge/game-dev-patterns.md` for pattern matching.

### 1.5 Prioritize Root Causes
Rank root causes by:
1. **Unblocking power**: How many symptoms does fixing this resolve?
2. **Dependency depth**: Is this cause upstream of other causes?
3. **Verification feasibility**: Can we verify the fix objectively?

Fix the root cause that unblocks the most symptoms and is most upstream.

### 1.6 Confirm Understanding with User
Before proceeding to Phase 2, use `AskUserQuestion` to confirm:
- Your understanding of the symptoms
- Your identified root cause chain
- Any missing information

**Required confirmation format:**
```
AskUserQuestion:
  header: "Root Cause"
  question: "I've identified the root cause as [X], which causes symptoms [A, B, C]. Does this match your observation?"
  options: ["Matches my observation", "Partially matches (I'll clarify)", "Does not match"]
```

### 1.7 Update Case File (If Active)
If a case file exists for this problem, update the following files:
- `01_background.md` — record symptoms, root causes, domain classification, root cause chain
- `02_technical-analysis/[topic]_[YYYYMMDD].md` — record code path tracing findings
- `05_timeline.md` — add entry for decomposition completion

## Output
A structured problem decomposition document. Use `templates/problem-state.md` to record the current state (for problems without a case file).

## Loop-Back Trigger
If during Phase 2 you discover the decomposition is incomplete or incorrect, return to Phase 1 and re-decompose.

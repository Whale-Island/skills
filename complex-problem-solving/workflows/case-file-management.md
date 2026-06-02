# Case File Management Workflow

## Objective
Define the complete lifecycle of a Case File (专题案): creation, session workflow, updates, and closure.

## Case File Lifecycle

```
Creation → Session Start → Work → Session End → ... → Resolution → Final Report
```

## 1. Creation

When a case file is confirmed in Phase 0:

1. Read `templates/case-file-structure.md` for the complete folder structure and file templates
2. Create the folder: `Documents/99_CaseFiles/[problem-keyword]/`
3. Populate initial files from templates:
   - `README.md` — directory overview with problem summary
   - `01_background.md` — problem background & symptoms (fill with initial description)
   - `05_timeline.md` — create first entry marking case file opened
4. Create sub-folders as needed based on problem nature:
   - `02_technical-analysis/` — if technical investigation is needed
   - `03_data-collection/` — if data/logs need to be collected
   - `04_solutions/` — for solution attempts

**Naming convention**: Use concise English keywords in PascalCase, e.g., `MapLayoutDisconnection`, `PhysicsTunneling`, `UIStateDesync`.

## 2. Session Start Protocol

**Before beginning any work on a problem with an active case file:**

1. Read the case file's `README.md` to get the current status
2. Read `05_timeline.md` to understand what happened in previous sessions
3. Read the most recent solution attempt in `04_solutions/` (if any)
4. Check for any "assumptions needing validation" listed in the README
5. If anything is unclear, use `AskUserQuestion` to confirm understanding with the user

**Mandatory session start check:**
```
AskUserQuestion:
  header: "Case Review"
  question: "I've reviewed the case file for [problem-keyword]. Current status: [summary from README]. Is there any new information or changes since the last session?"
  options:
    - "No new information; proceed with planned next steps"
    - "Yes, there are updates (I'll describe them)"
```

## 3. During-Session Updates

As work progresses through Phases 1-5, update the case file at these points:

| Phase | Case File Update | Target File |
|-------|-----------------|-------------|
| Phase 1 (Decomposition) | Record symptoms, root causes, domain classification | `01_background.md` |
| Phase 2 (Hypothesis) | Record all approaches and their ratings | `04_solutions/[approach-name].md` |
| Phase 3 (Experiment) | Record experiment setup, results, observations | `04_solutions/[approach-name].md` |
| Phase 4 (Verification) | Record verification results, confidence score | `04_solutions/[approach-name].md` |
| Phase 5 (Iteration) | Update status, record decision and rationale | `README.md`, `05_timeline.md` |

**Update rules:**
- Update the case file **immediately** after completing each phase — do not batch updates
- Use timestamps in the format `[YYYY-MM-DD HH:MM]` for all entries
- Never delete previous entries — append new information or add amendment notes

## 4. Session End Protocol

**Before ending a session with an active case file:**

1. Update `README.md`:
   - Current status (Investigating / In Progress / Partially Resolved)
   - Summary of what was accomplished this session
   - Current best state (what works, what doesn't)
   - Next steps with rationale
   - Assumptions needing validation
2. Update `05_timeline.md`:
   - Add session end entry with summary
   - Record all code changes and their status
   - List any "landmines" discovered
3. Update `04_solutions/` if any approaches were tried:
   - Record results (success / partial / failed / worse)
   - Document why an approach failed if applicable

## 5. Case Closure

When the problem is fully resolved:

1. Update `README.md`:
   - Status → Resolved
   - Final resolution summary
   - Key lessons learned
2. Create `06_final-report.md` using the template in `templates/case-file-structure.md`:
   - Problem description (from background)
   - Solution process (from timeline)
   - Final solution (what actually worked)
   - Lessons learned (what didn't work and why)
   - Prevention recommendations (how to avoid this problem in the future)
3. Clean up any temporary data files in `03_data-collection/`
4. The case file folder is now a permanent reference document

## 6. Case File Maintenance Rules

- **Never delete** a case file, even after resolution — it serves as institutional knowledge
- **Never modify** historical entries — add amendments with timestamps instead
- **Keep README.md current** — it is the entry point for any session
- **Use consistent naming** — file names should be descriptive and include timestamps when versioned
- **Cross-reference** — when a case file relates to another case, add a reference in both READMEs

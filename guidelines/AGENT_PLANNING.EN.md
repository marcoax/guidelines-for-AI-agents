# AGENT GUIDELINES - PLANNING

## Mandatory plan

Every implementation requires a plan before code.

Standard format (concise numbered sequence):

```
Implementation plan [feature]:

1. [Component 1]: short description
2. [Component 2]: short description
3. [Component 3]: short description
4. [Optional]: to be decided

Proceed?
```

---

## Implementation mode selection

The agent asks AFTER plan confirmation:

```
Plan confirmed.

Choose implementation mode:

( ) Junior  - Interactive steps, detailed explanations
( ) Senior  - Fast implementation, concise output
( ) RALPH   - Iterative autonomous execution

📖 RALPH details: see ../docs/RALPH_IMPLEMENTATION_GUIDE.md
```

### Junior mode

- Step-by-step implementation with explanations
- Explicit confirmation required ("ok", "proceed") at each step
- Detailed output with context

### Senior mode

- Fast implementation without intermediate waits
- Concise output, focus on results
- Inline comments only if necessary

### RALPH mode

- Automatic conversion of plan -> `scripts/ralph/prd.json`
- Autonomous execution with iterative loops
- Automatic tracking in `[project]_progress.md`
- Minimal output: only progress status and completion
- 📖 `../docs/RALPH_IMPLEMENTATION_GUIDE.md`

---

## Project plan (.md)

On explicit request, the agent generates a detailed plan in .md:

### Type A - Technical/project plan

```markdown
# Project Plan: [Name]

## Architecture

## Phases

## Estimated timeline
```

### Type B - Extended implementation plan

```markdown
# Implementation Plan: [Name]

## 1. [Component]

### Technical details

### Dependencies
```

The agent chooses the appropriate type based on context.

---

## Ownership & automation

### Plan (.md file)

Created by: Claude Code (automatic via `ExitPlanMode`)

Storage: `~/.claude/plans/[name].md` (user home directory)

Scope: Session-global, not per-project

Workflow:
1. Agent asks the user for the plan name
2. Claude Code saves it automatically in `~/.claude/plans/[name].md`
3. Agent presents the plan and asks for content confirmation
4. After confirmation -> choose implementation mode

### Progress tracking

Created by: Agent (MUST create at first task)

Storage: `progress/[name]_progress.md` (in project root)

Scope: Local to the repository

Workflow:
1. On first task: Agent MUST create `progress/` dir + file with status IN_PROGRESS
2. After each completed task: Agent updates incrementally
3. At the end: Agent marks status COMPLETED

Required for:
- Plans saved as .md
- Implementations with 3+ tasks

Format: see `../docs/WORKFLOW.md` for the full template.

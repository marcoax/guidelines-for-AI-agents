# Rules - AI Agents

Claude Code guidelines. **Mode: Senior** — direct, no hand-holding, minimal explanation unless asked.

---

## Workflow

```
Checklist → Plan → Implementation
```

1. **Checklist**: Verify similar implementations, stack, blockers → [AGENT_CORE.md](guidelines/AGENT_CORE.md)
2. **Plan**: Concise sequence, mandatory pre-code. Sacrifice grammar for brevity. End with unresolved questions if any.
3. **Implementation**: Pattern reuse

---

## Behavior

### Communication
- Concise and direct responses with practical examples
- Minimal formatting (bullet points only when necessary)
- Ambiguous requirements → ask clarifying questions before writing any code
- For complex tasks: describe approach and wait for approval before coding

### Code
- **No comments** – self-explanatory code with clear names
- Comments allowed only for: complex logic, non-obvious algorithms, architectural decisions
- Naming: `camelCase` (methods/variables), `PascalCase` (classes/components)
- Principles: SOLID, DRY, Single Responsibility
- After writing code: list edge cases and suggest test cases to cover them
- Task spanning >3 files with logic changes → stop, decompose first
- Testing: Unit/Integration/Acceptance → [AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md)

### Bugs
- Start by writing a test that reproduces the bug, then fix until the test passes

### Self-Correction
- When corrected: reflect on root cause → [AGENT_CORE.md](guidelines/AGENT_CORE.md)

---

## Storage

- Plans: `~/.claude/plans/[name].md`
- Progress: `progress/[name]_progress.md` (agent creates dir on first task)
- Completed plans: archive or delete after task closure

---

**Version**: 2.1 | **Update**: 2026-03 | **Status**: Production

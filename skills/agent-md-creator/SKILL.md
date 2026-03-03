---
name: agent-md-creator
description: Generate standardized CLAUDE.md or AGENT.md files for projects. Use when users request to create project documentation files like "Create a CLAUDE.md", "Generate AGENT.md", or "Setup project documentation". Automatically analyzes project stack from configuration files and combines it with a standard template.
---

# Agent MD Creator

Generate standardized CLAUDE.md or AGENT.md documentation files for projects, automatically detecting the technology stack and combining it with best practices templates.

---

## Operating Rules

- Follow steps in order; do not skip or reorder.
- Ask only the questions explicitly defined below.
- Prefer automatic detection over user input whenever possible.
- Keep all sections concise (bullet points, no prose).

---

## Workflow

### Step 1: File Type Selection

Ask the user: **"Do you want to create a CLAUDE.md or an AGENT.md file?"**

Wait for: `CLAUDE.md` or `AGENT.md`.

---

### Step 2: Automatic Project Analysis

Scan the project directory. Detect:

**Config files:**
- `package.json` — Node.js/JavaScript
- `composer.json` — PHP
- `requirements.txt`, `pyproject.toml`, `setup.py` — Python
- `pom.xml`, `build.gradle` — Java
- `Gemfile` — Ruby
- `.csproj`, `*.sln` — .NET
- `go.mod` — Go
- `Cargo.toml` — Rust
- Dockerfiles, CI/CD configs

**Extract only confirmed info about:**
- Frontend frameworks/libraries
- Backend languages/frameworks
- Databases and data layers
- Testing tools
- Build tools and package managers
- Deployment/DevOps tooling

---

### Step 3: Technology Stack Section

Generate using exactly this format:

```markdown
# Technology Stack

- **Frontend**: [frameworks, UI libraries, state management]
- **Backend**: [languages, frameworks, APIs]
- **Database**: [database type, ORM/query builder]
- **Other tools**: [testing, deployment, package manager]
```

Rules: one line per category, no explanations, omit missing categories.

---

### Step 4: Optional Project-Specific Details

Ask: **"Do you want to add project-specific documentation beyond the technology stack? (y/n)"**

If **yes**, collect one at a time (max 3–5 lines each):

1. **Architecture / structure** — patterns, folder layout
2. **Code conventions** — naming, formatting, best practices
3. **Critical dependencies** — key libraries and config notes
4. **Environment / setup notes** — env vars, system requirements
5. **Additional notes** — optional

If **no**, skip to Step 5.

---

### Step 5: File Generation

Create `CLAUDE.md` or `AGENT.md` in the project root:

```markdown
# Technology Stack

[auto-generated content]

---

# Project Technical Specifications

## Architecture
[user input]

## Code Conventions
[user input]

## Main Dependencies
[user input]

## Environment Notes
[user input]

## Additional Notes
[user input if provided]

---

[verbatim content from assets/CLAUDE.BASE.md]

---

## References

- [STACK.md](STACK.md)
- [AGENT_CORE.md](guidelines/AGENT_CORE.md)
- [AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md)
- [AGENT_PLANNING.md](guidelines/AGENT_PLANNING.md)
```

Rules:
- Technology Stack always present
- Project Technical Specifications only if requested in Step 4
- `assets/CLAUDE.BASE.md` appended verbatim
- References always present at end

---

### Step 6: Reference Verification

Verify these files exist:
- `guidelines/AGENT_CORE.md`
- `guidelines/AGENT_DEVELOPMENT.md`
- `guidelines/AGENT_PLANNING.md`

If any missing: report which files are not found, do not proceed.

---

### Step 7: STACK.md Generation

Create `STACK.md` in the project root with a detailed breakdown:

- Expanded technology stack explanation
- Key architectural decisions and trade-offs
- Major components and services
- Data flow and integration points (if applicable)
- Environment-specific notes (dev, staging, production)

Rules:
- Reuse content from `CLAUDE.md` if it exists, do not duplicate
- If conflicting info found: report conflict, do not silently resolve
- More verbose than `CLAUDE.md` — intended for deep technical reference

---

### Step 8: Completion Confirmation

Respond exactly:

> "File [CLAUDE.md / AGENT.md] successfully created at [path]"

---

## Template Location

Base template: `assets/CLAUDE.BASE.md` — append verbatim to end of generated file.



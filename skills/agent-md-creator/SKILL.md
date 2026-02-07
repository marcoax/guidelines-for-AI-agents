---
name: agent-md-creator
description: Generate standardized CLAUDE.md or AGENT.md files for projects. Intended for use by Claude and OpenAI-compatible agents when setting up or maintaining project documentation.
---

---

# Agent MD Creator

This document defines a deterministic, agent-friendly workflow for generating **CLAUDE.md** or **AGENT.md** files. It is aligned with both **Claude** and **OpenAI** agent conventions: explicit steps, strict output constraints, and minimal prose.

## Purpose

Provide a concise, machine- and human-readable reference describing:

- The project technology stack (auto-detected)
- Optional, user-supplied technical specifics

This file is **not** a full manual. It is a quick reference for agents and developers.

---

## Operating Rules (Strict)

- Follow steps in order; do not skip or reorder.
- Ask only the questions explicitly defined below.
- Do not add sections, commentary, or explanations outside the defined template.
- Prefer automatic detection over user input whenever possible.
- Keep all sections concise (bullet points, no prose).

---

## Plan Mode

When operating in **Plan Mode**:

- Make the plan extremely concise
- Sacrifice grammar for the sake of concision
- At the end of each plan, provide a list of unresolved questions, if any

---

## Workflow

### Step 1: Target File Selection

Ask the user exactly:

> "Do you want to create a CLAUDE.md or an AGENT.md file?"

Wait for one of the two valid responses:

- `CLAUDE.md`
- `AGENT.md`

---

### Step 2: Automatic Project Analysis

Scan the current project directory to detect the technology stack.

**Configuration files to inspect:**

- `package.json` — Node.js / JavaScript
- `composer.json` — PHP
- `requirements.txt`, `pyproject.toml`, `setup.py` — Python
- `pom.xml`, `build.gradle` — Java
- `Gemfile` — Ruby
- `.csproj`, `*.sln` — .NET
- `go.mod` — Go
- `Cargo.toml` — Rust
- Dockerfiles, CI/CD configs, infrastructure files

**Extract only confirmed information about:**

- Frontend frameworks and libraries
- Backend languages and frameworks
- Databases and data layers
- Testing tools
- Build tools and package managers
- Deployment and DevOps tooling

---

### Step 3: Technology Stack Section

Generate this section using **exactly** the format below.

```markdown
# Technology Stack

- **Frontend**: [frameworks, UI libraries, state management]
- **Backend**: [languages, frameworks, APIs]
- **Database**: [database type, ORM/query builder]
- **Other tools**: [testing, deployment, package manager]
```

**Constraints:**

- One line per category
- No explanations
- Include a category only if it exists in the project

---

### Step 4: Optional Project-Specific Details

Ask the user exactly:

> "Do you want to add project-specific documentation beyond the technology stack? (y/n)"

If the answer is **yes**, collect the following inputs **one at a time**:

1. **Architecture / structure** — patterns, folder layout (max 3–5 lines)
2. **Code conventions** — naming, formatting, best practices (max 3–5 lines)
3. **Critical dependencies** — key libraries and configuration notes (max 3–5 lines)
4. **Environment / setup notes** — env vars, system requirements (max 3–5 lines)
5. **Additional notes** — optional (max 3–5 lines)

If the answer is **no**, skip directly to Step 5.

---

### Step 5: File Generation

Create the selected file (`CLAUDE.md` or `AGENT.md`) in the project root with the following structure:

```markdown
# Technology Stack

[auto-generated content]

## Project Technical Specifications

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
```

Rules:

- The **Technology Stack** section is always present.
- The **Project Technical Specifications** section exists only if requested.
- `assets/CLAUDE.BASE.md` must be appended verbatim, without modification.

---

### Step 6: Reference Verification

Before final confirmation, verify that all required reference files exist.

Mandatory checks:

- `guidelines/AGENT_CORE.md`
- `guidelines/AGENT_DEVELOPMENT.md`
- `guidelines/AGENT_PLANNING.md`

Rules:

- If any file is missing, explicitly report which files are not found
- Do not proceed with final confirmation until all mandatory files are present
- Do not create or modify reference files unless explicitly instructed

---

### Step 7: Completion Confirmation

After successful verification, respond exactly:

> "File [CLAUDE.md / AGENT.md] successfully created at [path]"

---

## Design Rationale (For Agents)

- Deterministic structure → predictable parsing
- Minimal surface area → low token overhead
- Compatible with Claude-style instructions and OpenAI agent execution models

---

## Extended Stack Documentation

After completing the generation of `CLAUDE.md` or `AGENT.md`, create an additional file named `STACK.md` in the project root.

If a `CLAUDE.md` file already exists and contains technical content, **reuse and consolidate that information** when generating `STACK.md`, instead of duplicating or re-inventing it.

`STACK.md` must contain a **more detailed and descriptive breakdown of the project**, including:

- Expanded explanation of the technology stack
- Key architectural decisions and trade-offs
- Detailed descriptions of major components and services
- Data flow and integration points (if applicable)
- Environment-specific notes (development, staging, production)

Rules:

- `STACK.md` may be more verbose than `CLAUDE.md` / `AGENT.md`
- Source precedence for technical content: `CLAUDE.md` → `AGENT.md` → auto-detected data
- Prefer existing technical information from `CLAUDE.md` when available
- If conflicting information is found, explicitly report the conflict and do not silently resolve it
- Use clear sections and bullet points
- This file is intended for deeper technical understanding, not quick reference

---

## References

At the end of the generated `CLAUDE.md` or `AGENT.md`, include a **References** section listing authoritative documentation sources for the agent.

Always include:

- `STACK.md` — Detailed project stack and architecture reference
- `guidelines/AGENT_CORE.md` — Core agent principles and constraints
- `guidelines/AGENT_DEVELOPMENT.md` — Development rules and best practices
- `guidelines/AGENT_PLANNING.md` — Planning and task execution guidelines

Rules:

- References are links or relative paths only
- No descriptions or commentary
- Do not include any other files unless explicitly instructed

End of file.

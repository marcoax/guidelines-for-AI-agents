---
name: agent-md-creator
description: Generate standardized CLAUDE.md or AGENT.md files for projects. Use when users want to document a project for AI agents, set up project instructions, create AI instructions files, initialize agent docs, or say things like "Crea un CLAUDE.md", "Generate AGENT.md", "Setup project documentation", "document this project for agents", "create AI instructions file", "initialize agent docs", "set up project instructions".
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

# Agent MD Creator

Generate CLAUDE.md/AGENT.md + COMMANDS.md files. Auto-detect stack, combine with base template.

## Rules

> All interactive prompts (Steps 1, 4, 5) MUST use the AskUserQuestion tool.

## Workflow

### Step 1: File Type

Ask: **"CLAUDE.md or AGENT.md?"**

### Step 2: Analyze Project

Scan for config files:
- `package.json` → Node.js/JS
- `composer.json` → PHP
- `requirements.txt` / `pyproject.toml` → Python
- `pom.xml` / `build.gradle` → Java
- `*.csproj` / `*.sln` → .NET
- `go.mod` → Go
- `Cargo.toml` → Rust
- Docker, CI/CD configs

Extract: frameworks + exact versions, DB + exact versions, testing tools, build tools, deployment.

⚠️ Version numbers are mandatory. If not in config files, scan CLAUDE.md, TECHNICAL_REFERENCE.md, README.md. Format: "PHP 7.2" not "PHP".

### Step 3: Generate Stack Section

Format:
```markdown
# Technology Stack

- **Frontend**: [framework, UI libs, state mgmt]
- **Backend**: [lang, framework, API]
- **Database**: [DB type, ORM]
- **Other tools**: [deploy, test, pkg manager]
```

Rules:
- One line per category
- Omit empty categories
- Concise, no elaboration

### Step 4: Optional Project Details

Ask in one message:

```
Would you like to add project-specific documentation? Fill in any of the following and skip what you don't need:

1. **Architecture**: Patterns, folder structure (max 3-5 lines)
2. **Code conventions**: Naming, formatting (max 3-5 lines)
3. **Critical dependencies**: Main libs, version constraints (max 3-5 lines)
4. **Environment notes**: Env vars, system requirements (max 3-5 lines)
5. **Additional notes**: Other relevant info (optional)

Reply with the numbers and content, or "skip" to proceed.
```

### Step 5: Commands

Ask: **"Provide test, build, and dev server commands (or skip):"**

Example expected format:
```
Test: npm test
Build: npm run build
Dev: npm run dev
```

### Step 6: Generate Files

#### CLAUDE.md structure:
```markdown
[Technology Stack]

[If detailed doc requested:]
---

# Project Technical Specifications

## Architecture
[input]

## Code Conventions
[input]

## Main Dependencies
[input]

## Environment Notes
[input]

## Additional Notes
[input if provided]

---

See @COMMANDS.md for test and build commands.

---

[Content from assets/CLAUDE.BASE.md]
```

#### COMMANDS.md (generated only if commands were provided):
```markdown
# Commands

## Tests
[test command]

## Build
[build command]

## Dev Server
[dev server command]
```

Location: CWD, filename = user choice from Step 1.

### Step 7: Confirm

**"Files created: [name] and COMMANDS.md in [path]"**

(If no commands were provided, omit COMMANDS.md from the confirmation.)

## Template

Base template: `assets/CLAUDE.BASE.md` → NON copiare il contenuto. Aggiungere una riga di riferimento:
```
See @assets/CLAUDE.BASE.md
```

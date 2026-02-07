---
name: agent-md-creator
description: Generate standardized CLAUDE.md or AGENT.md files for projects. Use when users request to create project documentation files like "Crea un CLAUDE.md", "Generate AGENT.md", or "Setup project documentation". Automatically analyzes project stack from configuration files and combines it with a standard template.
---

# Agent MD Creator

Generate standardized CLAUDE.md or AGENT.md documentation files for projects, automatically detecting the technology stack and combining it with best practices templates.

## Workflow

When activated, follow this sequence:

### Step 1: File Type Selection

Ask the user: **"Vuoi creare un file CLAUDE.md o AGENT.md?"**

Wait for user response (CLAUDE.md or AGENT.md).

### Step 2: Automatic Project Analysis

Analyze the current project directory to detect the technology stack. Look for:

**Configuration files to check:**

- `package.json` (Node.js/JavaScript projects)
- `composer.json` (PHP projects)
- `requirements.txt` / `pyproject.toml` / `setup.py` (Python projects)
- `pom.xml` / `build.gradle` (Java projects)
- `Gemfile` (Ruby projects)
- `.csproj` / `*.sln` (C#/.NET projects)
- `go.mod` (Go projects)
- `Cargo.toml` (Rust projects)
- Docker files, CI/CD configs, etc.

**Extract information for:**

- Frontend frameworks/libraries (React, Vue, Angular, etc.)
- Backend frameworks (Express, Laravel, Django, etc.)
- Databases (PostgreSQL, MongoDB, MySQL, etc.)
- Testing tools
- Build tools and package managers
- Deployment/DevOps tools

### Step 3: Generate Stack Tecnologico Section

Create a concise summary with this exact format:

```markdown
# Stack Tecnologico

- **Frontend**: [framework, librerie UI, gestione stato]
- **Backend**: [linguaggi, framework, API]
- **Database**: [tipo di database, ORM/query builder]
- **Altri strumenti**: [deployment, testing, package manager]
```

**Important rules:**

- One line per category
- Concise, no elaboration
- Only include categories that are actually present in the project
- If a category is not present, omit that line entirely

### Step 4: Optional Detailed Documentation

Ask the user: **"Vuoi aggiungere documentazione specifica del progetto oltre allo stack tecnologico? (s/n)"**

If **yes**, collect the following information interactively (one question at a time):

1. **Architettura/struttura del progetto**: Pattern utilizzati, organizzazione cartelle (max 3-5 righe)
2. **Convenzioni di codice**: Naming conventions, formatting, best practices (max 3-5 righe)
3. **Dipendenze critiche**: Librerie principali e loro configurazioni (max 3-5 righe)
4. **Note ambiente/setup**: Variabili d'ambiente, requisiti sistema (max 3-5 righe)
5. **Note aggiuntive**: Qualsiasi altra informazione rilevante (opzionale, max 3-5 righe)

If **no**, skip to Step 5.

### Step 5: Generate Complete File

Create the file with this structure:

```markdown
[Stack Tecnologico section - always present]

## [If user requested detailed documentation:]

# Specifiche Tecniche del Progetto

## Architettura

[User input]

## Convenzioni di Codice

[User input]

## Dipendenze Principali

[User input]

## Note Ambiente

[User input]

## Note Aggiuntive

[User input if provided]

---

[Content from assets/CLAUDE.BASE.md]
```

**File location**: Create the file in the current working directory with the name chosen by the user (CLAUDE.md or AGENT.md).

### Step 6: Confirmation

After creating the file, confirm with: **"File [CLAUDE.md/AGENT.md] creato con successo in [path]"**

## Best Practices

- **Conciseness**: Keep all sections brief and to the point
- **Clarity**: Use bullet points, avoid prose
- **Relevance**: Only include critical information for development
- **Consistency**: Follow the exact format specified above
- **Automation**: Detect as much as possible automatically from project files

  ** Important**: Do not add any additional sections or information beyond what is specified in the template and user input.
  ** Important** keep CLAUDE.md and AGENT.md files concise only include essential information. Avoid lengthy explanations or detailed documentation that can be found in other project files. The goal is to provide a quick reference guide for developers, not a comprehensive manual.

## Template Location

The base template is stored in `assets/CLAUDE.BASE.md` and should be copied verbatim to the end of the generated file.

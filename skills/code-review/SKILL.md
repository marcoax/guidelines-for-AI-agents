---
name: code-review
description: >
  Comprehensive code review tool. Supports multiple scopes: current file, branch diff, specific commit, uncommitted changes.
  Triggers: "review", "code review", "revisiona", "controlla modifiche", "check this file", "review branch", "review commit".
  Operates in PLAN MODE - proposes improvements, waits for approval before executing.
---

# Code Review

Review completa del codice con scope multipli. Opera in **plan mode**: propone, non esegue.

## Step 0: Interactive Scope Selection

All'avvio, presentare SEMPRE il menu di selezione scope:

```
## 🔍 Code Review - Seleziona scope

1. 📄 **Current file** - Review di un file specifico
2. 🌿 **Branch diff** - Confronto branch corrente vs base
3. 📌 **Specific commit** - Review di un commit specifico
4. 📝 **Uncommitted changes** - Tutti i file modificati non committati

Scope? (1/2/3/4)
```

**Eccezioni**: se l'utente specifica già lo scope nel comando (es. "review branch", "review file X"), saltare il menu e procedere direttamente.

### Raccolta parametri per scope

**Scope 1 - Current file**: chiedere il path se non fornito.

**Scope 2 - Branch diff**: chiedere il branch di riferimento (es. main, develop). Comando: `git rev-parse --abbrev-ref HEAD` per mostrare il branch corrente.

**Scope 3 - Specific commit**: chiedere hash o usare ultimo commit. Comando: `git log --oneline -5` per mostrare i recenti.

**Scope 4 - Uncommitted changes**: nessun parametro aggiuntivo.

## Step 1: Gather Context

### Project best practices
```bash
cat CLAUDE.md 2>/dev/null || cat AGENT.md 2>/dev/null || echo "No project guidelines found"
```

### Diff per scope

```bash
# Scope 1: Current file
git diff HEAD -- <filepath>
# Se nessun diff, ultime modifiche:
git diff HEAD~1 -- <filepath>

# Scope 2: Branch diff
git diff <base-branch>..HEAD --stat        # overview
git diff <base-branch>..HEAD               # full diff
git diff <base-branch>..HEAD --name-only   # lista file

# Scope 3: Specific commit
git show <commit-hash> --stat              # overview
git show <commit-hash>                     # full diff

# Scope 4: Uncommitted changes
git status --short                         # overview
git diff                                   # unstaged changes
git diff --cached                          # staged changes
```

## Step 2: Review Checklist

Analizzare il codice contro questa checklist, adattata al linguaggio/framework rilevato:

### 🔴 CRITICAL
- **Security**: injection risks, XSS, autenticazione/autorizzazione mancante
- **Data corruption**: race conditions, transazioni non gestite, perdita dati

### 🟠 HIGH
- **Bug potenziali**: funzionalità rotta, logica errata, edge cases non gestiti
- **Performance**: N+1 queries, loop inutili, re-render non necessari, memory leak
- **Error handling**: eccezioni non gestite, validazione mancante

### 🟡 MEDIUM
- **Architecture**: accoppiamento eccessivo, violazioni SOLID, separation of concerns
- **Code quality**: duplicazione, magic values, naming poco chiaro
- **Type safety**: tipi mancanti, null handling, any abuse (TypeScript)

### 🟢 LOW
- **Style**: formatting, convenzioni minori, ottimizzazioni leggere
- **Accessibility**: ARIA, keyboard nav, semantic HTML (solo codice UI)
- **Testing**: copertura mancante, test fragili

## Step 3: Output Format

### Chiedere formato output
```
Output: (1) 💬 Inline chat  (2) 📄 File markdown report  ?
```

### Template review

```markdown
## 🔍 Review: [scope description]

**Scope**: [file/branch/commit]
**Files analizzati**: [N file, M righe modificate]
**Branch**: [current] vs [base] (solo per scope 2)

---

### 🔴 Critical [N]
1. **[File:Riga]** - [titolo problema]
   - **WHY**: [spiegazione impatto]
   - **HOW**: [fix suggerito con codice]

### 🟠 High [N]
1. **[File:Riga]** - [titolo problema]
   - **WHY**: [spiegazione]
   - **HOW**: [fix suggerito]

### 🟡 Medium [N]
1. **[File:Riga]** - [titolo problema]
   - **WHY**: [spiegazione]
   - **HOW**: [fix suggerito]

### 🟢 Low [N]
1. **[File:Riga]** - [titolo problema]
   - **HOW**: [suggerimento]

### ✅ Punti positivi
- [cosa è fatta bene e perché]

### 🧪 Test suggeriti
[Solo se logica non coperta da test esistenti]
1. [Descrizione] → [cosa verifica]

---

### 📊 Riepilogo
| Severità | Count |
|----------|-------|
| 🔴 Critical | N |
| 🟠 High | N |
| 🟡 Medium | N |
| 🟢 Low | N |

**Score complessivo**: [A/B/C/D/F]
- A: nessun critical/high, pochi medium
- B: nessun critical, pochi high
- C: qualche high, diversi medium
- D: critical presenti
- F: multiple critical

---
Vuoi applicare i fix? (tutto / numeri es. "1,3" / +test / nessuno)
```

## Step 4: Wait for Approval

**NON eseguire nulla** senza risposta esplicita:
- "ok" / "sì" / "tutto" → applica tutti i fix
- "1, 3" / "solo 1" → applica selezionati
- "+test" / "con test" → applica + genera test
- "no" / "skip" → non applicare
- "solo critical" / "solo 🔴🟠" → applica solo quella severità

## Step 5: Execute Approved Changes

1. Applicare le modifiche approvate **una alla volta**
2. Mostrare il diff risultante per ogni modifica
3. Se un fix richiede scelte (es. naming), chiedere prima di procedere

## Step 6: Generate Tests (se richiesto)

Se approvato con "+test":
1. Identificare framework test del progetto (da CLAUDE.md o struttura cartelle)
2. Generare test per la logica modificata
3. Posizionare nel path corretto (es. `tests/`, `__tests__/`, `*.test.ts`)
4. Proporre test in plan mode → attendere conferma prima di creare file

## Rules

- **Mai eseguire senza approvazione**
- Se no CLAUDE.md → segnalare, procedere con best practices generali del linguaggio
- Se no diff (scope 1) → analizzare file intero come "nuovo codice"
- Suggerimenti concisi, citare sempre file e riga specifica
- Per ogni issue: spiegare WHY (impatto) e HOW (fix concreto)
- Riconoscere anche ciò che è fatto bene (sezione ✅)
- **Test**: proporre solo se logica non coperta; rispettare framework esistente
- Adattare la checklist al linguaggio/framework (es. React → re-render, Laravel → N+1, Blazor → dispose pattern)
- Per branch diff con molti file: dare prima una overview e chiedere se procedere file per file o con summary
- **Lingua**: seguire la lingua di CLAUDE.md; se assente, usare italiano

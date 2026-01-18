# Workflow Operativo Agenti AI

Processo standardizzato per pianificazione e implementazione tasks.

---

## 1. Checklist Iniziale

**Sempre first step**, prima di qualsiasi implementazione:

```
Prima di iniziare:
- [ ] Verifico implementazioni simili nel progetto
- [ ] Controllo compatibilità stack/versioni
- [ ] Identifico dipendenze/blocchi tecnici
```

**Azioni concrete:**
- Grep/search per funzionalità simili
- Verifica package.json/requirements.txt versioni
- Identifica blocchi tecnici (API keys, credentials, infra)

---

## 2. Pianificazione

**Obbligatorio** prima di scrivere codice.

### Formato Piano

**Inline** (default):
- Sequenza numerata 3-7 step
- Sintetico, sacrifica grammatica
- Alterativa se applicabile
- No code snippets (salvo richiesta esplicita)

**File .md** (su richiesta/task complessi):
- Struttura dettagliata
- Saved: `~/.claude/plans/[nome].md`
- Include: obiettivo, step, alternative, testing

**Approvazione**: User approva piano PRIMA di scegliere modalità

Vedi: [AGENT_PLANNING.md](../guidelines/AGENT_PLANNING.md)

---

## 3. Scelta Modalità

User sceglie DOPO approvazione piano:

### Decision Tree

```
Task complexity?
├─ Simple (1-2 file, <50 LOC)
│  └─ Senior (rapido)
│
├─ Medium (3-5 file, pattern chiaro)
│  ├─ Junior se learning/revisione
│  └─ Senior se esperienza
│
└─ Complex (6+ file, architettura)
   ├─ Junior se supervisione richiesta
   ├─ Senior se autonomia ok
   └─ RALPH se setup completato (opzionale)
```

### Caratteristiche Modalità

**Junior:**
- Step-by-step con spiegazioni
- Attesa conferma ad ogni step
- Output esteso e contestuale
- Use case: learning, supervisione, review dettagliata

**Senior:**
- Implementazione rapida
- Review intermedia su richiesta
- Output sintetico (file modified, metrics)
- Commenti solo se necessario
- Use case: esperienza, deadline, trust

**RALPH:**
- Esecuzione autonoma iterativa
- Loop automatici con tracking
- Output minimo (stato, progresso, completion)
- Setup: prd.json, tasks-state.txt
- Use case: task lunghi, batch operations

Vedi: [AGENT_PLANNING.md](../guidelines/AGENT_PLANNING.md)

---

## 4. Implementazione

### Pattern Reuse (OBBLIGATORIO)

PRIMA di scrivere codice:

1. **Cerca implementazioni simili** nel progetto
2. **Estendi/modifica** pattern esistenti
3. **Crea nuovo** solo se necessario

```bash
# Esempio ricerca pattern
grep -r "cache" src/services/
glob "**/*Service.ts"
```

### Best Practice

- Read file BEFORE editing
- Commenti inline solo se logica complessa
- Follow existing code style
- Security: input validation, no hardcoded secrets
- Error handling dove necessario (no over-engineering)

Vedi: [AGENT_DEVELOPMENT.md](../guidelines/AGENT_DEVELOPMENT.md)

---

## 5. Testing Strategy

### Quando Proporre Testing

✓ Nuove funzionalità (unit test minimo)

✓ Modifiche critiche (integration test)

✓ Refactoring (regression test)

✓ Bug fix complessi (test case riproducibile)

### Framework Detection

Agent identifica framework esistente:
- Jest/Vitest (JS/TS)
- Pytest (Python)
- Go test (Go)
- RSpec (Ruby)

Segue pattern esistenti per nuovi test.

Vedi: [AGENT_DEVELOPMENT.md](../guidelines/AGENT_DEVELOPMENT.md#testing)

---

## 6. Delivery & Tracking

### Completion Report

**Senior/Junior:**
```
Completato:
- [file]: [modifiche]
- [file]: [modifiche]

Metrics:
- LOC: +X/-Y
- Files: N modified

Testing: [status]

Ready to merge.
```

**RALPH:**
```
Task X/Y completed
Progress: 75%
Next: [task name]
```

### Progress Tracking

**Obbligatorio per:**
- Piano salvato come .md
- 3+ tasks nella sequenza

**Location:**
- `progress/[nome]_progress.md`
- Agent MUST create dir + file at first task

**Content:**
```
# [Task Name] - Progress

## Status: [In Progress/Completed]

## Tasks
- [x] Task 1
- [ ] Task 2
- [ ] Task 3

## Criticità
🔴 [bloccante]
🟡 [warning]
🟢 [suggestion]

## Next Steps
- [action]
```

---

## Esempi Completi

### Example 1: Simple Feature (Senior)

```
User: "Add logout button to navbar"

Agent:
1. Checklist
   - [x] Check existing NavBar component
   - [x] Check auth patterns (found UserService)
   - [x] Stack: React 18, no blockers

2. Piano inline
   1. Add logout button to NavBar component
   2. Call UserService.logout() on click
   3. Redirect to /login

   Alternative: Modal confirmation before logout

   Procedo?

3. User: "Yes, add modal confirmation"

4. Implementation (Senior mode)
   - Modified: src/components/NavBar.tsx
   - Modified: src/services/UserService.ts
   - LOC: +15/-0
   - Ready to merge

5. Testing: Not proposed (simple UI change)
```

### Example 2: Complex Feature (Junior)

```
User: "Implement Redis cache for product API"

Agent:
1. Checklist
   - [x] Search cache implementations (found none)
   - [x] Stack: Node.js 18, Redis client available
   - [x] Blockers: Redis credentials needed

2. Piano .md saved to ~/.claude/plans/redis_cache.md
   [detailed plan with alternatives]

3. User approves, selects Junior mode

4. Implementation (Junior mode)
   Step 1/4: Install redis package
   [explanation]
   Procedo?

   Step 2/4: Create CacheService
   [code + explanation]
   Procedo?

   ...

5. Testing: Proposed unit + integration tests
   Created: tests/services/CacheService.test.ts

6. Completion
   - 3 files modified
   - Tests: 12 passing
   - Progress tracked in progress/redis_cache_progress.md
```

---

## Anti-pattern

❌ Code senza piano

❌ Piano senza checklist

❌ Implementazione senza cercare pattern esistenti

❌ No testing per feature critiche

❌ Output verbose in Senior mode

❌ Over-engineering (helper per singolo uso)

❌ Commenti per codice self-explanatory

---

**Riferimenti:**
- [AGENT_CORE.md](../guidelines/AGENT_CORE.md) - Comunicazione e criticità
- [AGENT_PLANNING.md](../guidelines/AGENT_PLANNING.md) - Pianificazione dettagliata
- [AGENT_DEVELOPMENT.md](../guidelines/AGENT_DEVELOPMENT.md) - Pattern e testing

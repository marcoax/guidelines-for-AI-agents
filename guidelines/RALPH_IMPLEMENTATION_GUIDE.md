# GUIDA RALPH - IMPLEMENTAZIONE AUTONOMA

## Quando usare RALPH

Ralph viene attivato quando:
1. L'utente sceglie "RALPH" dopo conferma del piano (vedi [AGENT_PLANNING.md](./AGENT_PLANNING.md))
2. L'agente converte automaticamente il piano in `scripts/ralph/prd.json`
3. Avvia esecuzione autonoma con loop iterativi

**Vedi [AGENT_PLANNING.md](./AGENT_PLANNING.md) sezione "Modalità RALPH"** per dettagli integrazione con workflow core.

---

## PREREQUISITI & SETUP

### Configurazione iniziale

Prima di attivare RALPH modalità:

1. **Directory scripts/ralph/**
   ```bash
   mkdir -p scripts/ralph
   ```

2. **File configurazione** (creati automaticamente da agente al primo loop)
   - `scripts/ralph/prd.json` - Definizioni task (non modificare manualmente)
   - `scripts/ralph/tasks-state.txt` - Stato esecuzione (aggiornato ad ogni loop)

3. **PROMPT.md** - Crea file di prompt con istruzioni RALPH (vedi sezione 4 di questa guida)

4. **Environment** - Verifica configurazione CLI (se Ralph tool installato)
   ```bash
   # Se usato: ralph --version
   # Se workflow manuale: nessuna configurazione necessaria
   ```

---

## 1. STRUTTURA GENERATA

Quando avvii Ralph, vengono creati automaticamente:
```
my-project/
├── PROMPT.md                      # Istruzioni per Ralph
├── @fix_plan.md                   # Task lista
├── @AGENT.md                      # Build/run
├── [nome-progetto]_progress.md    # ← Documento di progresso
├── scripts/ralph/
│   ├── prd.json                   # ← Definizioni task (DO NOT MODIFY)
│   └── tasks-state.txt            # ← Stato esecuzione (AGGIORNATO AD OGNI LOOP)
├── specs/
├── src/
└── logs/
    └── ralph.log
```

---

## 2. DOCUMENTO DI PROGRESSO ([nome-progetto]_progress.md)

Generato automaticamente all'inizio di ogni progetto:

### Formato
```markdown
# Progresso Implementazione: [nome-progetto]

## 📊 Overview
- **Status**: IN_PROGRESS / COMPLETED
- **Loop Attuale**: 5/20
- **Completamento**: 65%
- **Ultimo Update**: 2024-01-16 14:32:45

---

## 📋 Task Eseguiti

### ✅ Loop 1 - Setup Iniziale
- [x] Inizializzazione progetto
- [x] Configurazione environment
- [x] Creazione struttura cartelle
- **Timestamp**: 2024-01-16 10:00:00
- **Type**: feat

### ✅ Loop 2 - Database
- [x] Schema database
- [x] Migrazioni
- [x] Seed dati
- **Timestamp**: 2024-01-16 10:45:00
- **Type**: feat

### ✅ Loop 3 - API Backend
- [x] Endpoint GET /users
- [x] Endpoint POST /users
- [x] Validazione input
- **Timestamp**: 2024-01-16 11:30:00
- **Type**: feat

### ⏳ Loop 4 - Frontend (IN PROGRESS)
- [ ] Componente Lista Utenti
- [ ] Form Creazione Utente
- [ ] Integrazione API
- **Started**: 2024-01-16 12:15:00

### ⭕ Loop 5 - Testing
- [ ] Unit test backend
- [ ] Integration test API
- [ ] Test frontend

---

## 📈 Statistiche

| Loop | Task | Type | Duration | Status |
|------|------|------|----------|--------|
| 1 | Setup | feat | 45 min | ✅ Done |
| 2 | Database | feat | 35 min | ✅ Done |
| 3 | API | feat | 40 min | ✅ Done |
| 4 | Frontend | feat | -- | ⏳ In Progress |
| 5 | Testing | test | -- | ⭕ Planned |

---

## 🎯 Prossimi Step

1. Completare componenti frontend (Loop 4)
2. Implementare testing (Loop 5)
3. Deploy e verifica (Loop 6)

---

## ⚠️ Note

- Configurazione database completata con successo
- API response time: < 100ms ✅
- Frontend responsive design implementato
```

---

## 3. FILE DI CONFIGURAZIONE RALPH

### scripts/ralph/prd.json
```json
{
  "project": "my-project",
  "version": "1.0.0",
  "tasks": [
    {
      "id": "task_001",
      "title": "Setup Iniziale",
      "description": "Inizializza il progetto",
      "priority": 1,
      "effort": "low",
      "scope": "backend",
      "status": "done"
    },
    {
      "id": "task_002",
      "title": "Database Schema",
      "description": "Crea schema database",
      "priority": 2,
      "effort": "medium",
      "scope": "backend",
      "status": "done"
    },
    {
      "id": "task_003",
      "title": "API Endpoints",
      "description": "Implementa REST API",
      "priority": 3,
      "effort": "medium",
      "scope": "backend",
      "status": "done"
    },
    {
      "id": "task_004",
      "title": "Frontend Componenti",
      "description": "Crea UI components",
      "priority": 4,
      "effort": "high",
      "scope": "frontend",
      "status": "todo"
    },
    {
      "id": "task_005",
      "title": "Testing",
      "description": "Unit e integration tests",
      "priority": 5,
      "effort": "medium",
      "scope": "backend",
      "status": "todo"
    }
  ]
}
```

### scripts/ralph/tasks-state.txt
```
TASK_ID | STATUS | ITERATION | TIMESTAMP | NOTES
--------|--------|-----------|-----------|-------
task_001 | done | 1 | 2024-01-16T10:00:00Z | Setup completato
task_002 | done | 2 | 2024-01-16T10:45:00Z | Database operazionale
task_003 | done | 3 | 2024-01-16T11:30:00Z | API testata e funzionante
task_004 | in_progress | 4 | 2024-01-16T12:15:00Z | Implementazione componenti
task_005 | todo | - | - | Pianificato per loop 5
```

---

## 4. PROMPT DI ESECUZIONE RALPH

Inserisci questo prompt nel tuo PROMPT.md:
```markdown
# Ralph Autonomous Implementation Agent

You are Claude acting as an autonomous senior software engineer.

You are running inside a Ralph autonomous loop.

## FILES:

- scripts/ralph/prd.json        → task definitions (DO NOT MODIFY)
- scripts/ralph/tasks-state.txt → task execution state (UPDATE THIS)

## TASK SELECTION:

- Read PRD.json
- Read tasks-state.txt
- Select ONE task with status "todo"
- Prefer highest priority, then lowest effort
- Respect scope (frontend / backend)

## EXECUTION:

- Implement the selected task
- Do NOT modify PRD.json
- Update tasks-state.txt:
  - set status to "done"
  - add iteration number
  - add timestamp (ISO format)

## PROGRESS TRACKING:

- Update [nome-progetto]_progress.md after each loop:
  - Add completed task to relevant Loop section
  - Update Loop status (⏳ In Progress → ✅ Done)
  - Update overall completamento percentage
  - Update timestamp

## COMMIT METADATA:

- Use task type: feat, fix, refactor
- Output exactly one tag:
```
<type>feat|fix|refactor</type>
```

## COMPLETION:

- If no todo tasks remain, output exactly:
```
<promise>COMPLETE</promise>
```

## SAFETY:

- Never open files > 300 lines
- Never edit files > 500 lines
- Prefer new files over large edits

## RULES:

- Never ask questions
- Never explain
- Always leave the repo in a working state
```

---

## 5. COMANDO PER AVVIARE RALPH CON TRACKING
```bash
# Avvia Ralph con tracking automatico
ralph --monitor \
  --timeout 15 \
  --calls 100 \
  --output-format json

# Il sistema genererà automaticamente:
# - [nome-progetto]_progress.md (creato al primo loop)
# - scripts/ralph/prd.json (da @fix_plan.md)
# - scripts/ralph/tasks-state.txt (aggiornato ad ogni loop)
```

---

## 6. VISUALIZZARE IL PROGRESSO

Durante l'esecuzione:
```bash
# Monitora il progresso in tempo reale
tail -f [nome-progetto]_progress.md

# Visualizza stato task
cat scripts/ralph/tasks-state.txt

# Leggi log dettagliato
tail -f logs/ralph.log
```

---

## 7. STRUTTURA LOOP COMPLETA
```
┌─ INIZIO LOOP ─┐
│              │
├─ Leggi prd.json
├─ Leggi tasks-state.txt
├─ Seleziona task todo
│
├─ ESECUZIONE
│  ├─ Implementa task
│  ├─ Aggiorna tasks-state.txt
│  └─ Aggiorna [nome-progetto]_progress.md
│
├─ COMMIT
│  └─ Output <type>feat/fix/refactor</type>
│
└─ VERIFICA
   ├─ Se todo restanti → LOOP SUCCESSIVO
   └─ Se nessun todo → Output <promise>COMPLETE</promise>
```

---

## 8. CHECKLIST PRE-AVVIO RALPH

Assicurati che:

- [ ] PROMPT.md contiene il prompt di esecuzione sopra
- [ ] @fix_plan.md è convertito in scripts/ralph/prd.json
- [ ] scripts/ralph/ directory è creata
- [ ] [nome-progetto]_progress.md è inizializzato
- [ ] Hai deciso il numero massimo di loop
- [ ] Limite API è configurato appropriatamente

---

## 9. ESEMPIO REAL-TIME TRACKING

**Durante Esecuzione (Loop 4):**
```
[14:30] 🔄 Loop 4 avviato
[14:31] 📖 Letto prd.json (5 task definiti)
[14:31] 📖 Letto tasks-state.txt (3 done, 2 todo)
[14:32] 🎯 Selezionato: task_004 (Frontend Componenti)
[14:35] ⚙️  Implementazione in corso...
[14:45] ✅ Task completato
[14:46] 📝 Aggiornato tasks-state.txt
[14:47] 📝 Aggiornato [nome-progetto]_progress.md
[14:48] 💾 Commit: feat: Aggiunti componenti frontend
[14:48] ⏳ Loop 4 completato → Prossimo: Loop 5
```

---

## 10. TROUBLESHOOTING

### Loop bloccati o stagnanti

**Sintomo**: Stesso task ripetuto in più loop.

**Cause possibili**:
- Task troppo complesso (dividere in sub-task)
- Dipendenza non risolvibile
- PRD.json malformato

**Soluzione**:
1. Interrompere esecuzione
2. Analizzare tasks-state.txt
3. Aggiornare prd.json: segnare task come "blocked" o dividerlo
4. Riavviare

### prd.json malformato

**Sintomo**: Parsing error, processo crash.

**Verificare**:
```json
{
  "project": "string",     // ✓ obbligatorio
  "version": "string",     // ✓ obbligatorio
  "tasks": [               // ✓ obbligatorio, array
    {
      "id": "string",      // ✓ unique
      "title": "string",   // ✓
      "description": "string",
      "priority": number,  // ✓ 1-5
      "effort": "low|medium|high",
      "scope": "backend|frontend",
      "status": "todo|done|blocked"
    }
  ]
}
```

### Progress.md non aggiornato

**Sintomo**: File rimane fermo o non creato.

**Soluzione**:
1. Verificare permessi directory `progress/`
2. Verificare formato progress.md corrisponde schema
3. Verificare agente ha accesso write

### Implementazione fallita, code broken

**Rollback procedure**:
```bash
# 1. Identificare commit ultimo OK
git log --oneline | head -5

# 2. Reset a commit prima del loop fallito
git reset --hard <commit-hash>

# 3. Aggiornare tasks-state.txt: segnare task come "failed"
# 4. Aggiornare prd.json: proposte alternative
# 5. Riavviare Ralph
```

### Coverage limite API

**Se raggiunto limite API durante loop**:
1. RALPH s'interrompe automaticamente
2. Aggiornare progress.md con stato attuale
3. Riprendere nella sessione successiva (continua da task next)
4. Incrementare API limit per sessione successiva

---

**Tutto pronto!** Ralph genererà automaticamente il tracking del progresso ad ogni loop. 🚀

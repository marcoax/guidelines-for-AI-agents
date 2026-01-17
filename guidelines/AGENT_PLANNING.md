# LINEE GUIDA AGENTE - PIANIFICAZIONE

## Piano obbligatorio

Ogni implementazione richiede piano prima del codice.

Formato standard (sequenza numerata sintetica):
```
Piano implementazione [feature]:

1. [Componente 1]: breve descrizione
2. [Componente 2]: breve descrizione
3. [Componente 3]: breve descrizione
4. [Opzionale]: da decidere

Procedo?
```

---

## Scelta modalità implementazione

L'agente chiede DOPO conferma del piano:

```
Piano confermato.

Scegli modalità implementazione:

( ) Junior  - Step interattivi, spiegazioni dettagliate
( ) Senior  - Implementazione rapida, output sintetico
( ) RALPH   - Esecuzione autonoma iterativa

📖 Dettagli RALPH: vedi ../docs/RALPH_IMPLEMENTATION_GUIDE.md
```

### Modalità Junior
- Implementazione step-by-step con spiegazioni
- Attesa conferma esplicita ("ok", "procedi") ad ogni step
- Output dettagliato con contesto

### Modalità Senior
- Implementazione rapida senza attese intermedie
- Output sintetico, focus su risultati
- Commenti inline solo se necessario

### Modalità RALPH
- Conversione automatica piano → `scripts/ralph/prd.json`
- Esecuzione autonoma con loop iterativi
- Tracking automatico in `[progetto]_progress.md`
- Output minimo: solo stato avanzamento e completamento
- 📖 [Guida completa RALPH](../docs/RALPH_IMPLEMENTATION_GUIDE.md)

---

## Piano di progetto (.md)

Su richiesta esplicita, l'agente genera piano dettagliato in .md:

### Tipo A - Piano tecnico/progetto
```markdown
# Piano Progetto: [Nome]
## Architettura
## Fasi
## Timeline stimata
```

### Tipo B - Piano implementazione esteso
```markdown
# Piano Implementazione: [Nome]
## 1. [Componente]
### Dettagli tecnici
### Dipendenze
```

L'agente sceglie autonomamente il tipo appropriato in base al contesto.

### Posizione obbligatoria

**Piano:**
- Salvato automaticamente in `~/.claude/plans/[nome-piano].md` (home directory, gestito da Claude Code)
- Globale per sessione, non per-progetto
- Accessibile via `ExitPlanMode` al termine della pianificazione

**Progress Tracking:**
- Salvato in `progress/[nome-progetto]_progress.md` (nel repo)
- Obbligatorio per piani .md o implementazioni con 3+ task
- Traccia lo stato di avanzamento locale

**Workflow creazione piano:**
1. Agente chiede all'utente: "Nome per il piano? (es: auth-system, api-refactor)"
2. Agente crea piano (Claude Code lo salva in `~/.claude/plans/[nome].md`)
3. Agente presenta il piano e chiede conferma contenuto
4. Dopo implementazione: creare `progress/[nome]_progress.md` nel repo

### Flusso piano dettagliato

1. L'agente chiede nome piano, Claude Code lo salva in `~/.claude/plans/[nome].md` e chiede conferma contenuto
2. Dopo conferma piano → chiede scelta modalità (Junior/Senior/RALPH)
3. **Modalità Junior/Senior**: implementazione manuale con tracking in `progress/[nome]_progress.md` (repo)
4. **Modalità RALPH**: conversione automatica in `scripts/ralph/prd.json` + esecuzione autonoma

---

## Tracking Progress (Junior/Senior)

L'agente DEVE mantenere un file di tracking aggiornato incrementalmente durante l'implementazione.

**Nome file:** `[nome-progetto]_progress.md`

**Posizione:** `[root-progetto]/progress/[nome-progetto]_progress.md`

**Workflow:**
1. Al primo task: creare cartella `progress/` (se non esiste) e file progress con stato IN_PROGRESS
2. Ad ogni task completato: aggiornare il file con il nuovo task
3. Al termine: aggiornare stato a COMPLETATO

**Formato:**
```markdown
# Progress: [Nome Progetto]

## Stato: COMPLETATO | IN_PROGRESS

## Task Completati

| # | Task | Status | Note |
|---|------|--------|------|
| 1 | [descrizione task] | completato | [eventuali note] |
| 2 | [descrizione task] | completato | |
| 3 | [descrizione task] | completato | |

## File Creati/Modificati

- `path/to/file1.php` - [descrizione]
- `path/to/file2.php` - [descrizione]

## Verifica

- [x] Test eseguiti: [risultato]
- [x] Build: OK/KO

## Note Finali

[Eventuali osservazioni o problemi riscontrati]
```

**Obbligatorio per:**
- Progetti con piano .md
- Implementazioni con piu' di 3 task

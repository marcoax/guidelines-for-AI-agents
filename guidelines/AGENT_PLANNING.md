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

📖 Dettagli RALPH: vedi ./RALPH_IMPLEMENTATION_GUIDE.md
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
- 📖 [Guida completa RALPH](./RALPH_IMPLEMENTATION_GUIDE.md)

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

---

## Ownership & Automazione

### Piano (.md file)

**Creato da:** Claude Code (automatico via `ExitPlanMode`)

**Storage:** `~/.claude/plans/[nome].md` (home directory user)

**Scope:** Session-global, non per-progetto

**Workflow:**
1. Agent chiede nome piano all'utente
2. Claude Code salva automaticamente in `~/.claude/plans/[nome].md`
3. Agent presenta piano, chiede conferma contenuto
4. Dopo conferma → scelta modalità implementazione

### Progress Tracking

**Creato da:** Agent (MUST create at first task)

**Storage:** `progress/[nome]_progress.md` (nella [root] del progetto)

**Scope:** Locale al repository

**Workflow:**
1. Al primo task: Agent MUST create `progress/` dir + file con stato IN_PROGRESS
2. Ad ogni task completato: Agent aggiorna incrementalmente
3. Al termine: Agent marca stato COMPLETATO

**Obbligatorio per:**
- Piani salvati come .md
- Implementazioni con 3+ task

**Formato:** Vedi [docs/WORKFLOW.md](../docs/WORKFLOW.md) per template completo

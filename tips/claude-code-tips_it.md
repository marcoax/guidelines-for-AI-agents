# Claude Code Tips & Best Practices

Una raccolta di consigli avanzati per ottenere il massimo da Claude Code.

---

## 1. Gestione dei Task

### Commands & Slash Commands

- Se fai qualcosa più di una volta al giorno, trasformalo in una **skill** o **command**
- Crea un comando `/techdebt` da eseguire alla fine di ogni sessione per trovare e eliminare codice duplicato
- Imposta uno slash command che sincronizza 7 giorni di Slack, GDrive, Asana e GitHub in un unico context dump

### Subagents

- Aggiungi `"use subagents"` a qualsiasi richiesta dove vuoi che Claude dedichi più compute al problema
- Delega task individuali ai subagents per mantenere la context window del main agent pulita e focalizzata
- Instrada le richieste di permesso a Opus 4.5 tramite un hook — lascia che scansioni per attacchi e approvi automaticamente quelli sicuri
  - Vedi: [Hooks Documentation](https://code.claude.com/docs/en/hooks#permissionrequest)

---

## 2. CLAUDE.md - Il Tuo Asset Più Importante

### Iterazione Continua

Dopo ogni correzione, concludi con:

> *"Update your CLAUDE.md so you don't make that mistake again."*

Claude è sorprendentemente bravo a scrivere regole per se stesso.

### Best Practices

- **Modifica spietatamente** il tuo CLAUDE.md nel tempo
- Continua a iterare finché il tasso di errori di Claude non cala misurabilmente
- Mantieni una **notes directory** per ogni task/progetto, aggiornata dopo ogni PR
- Punta il tuo CLAUDE.md a questa directory

---

## 3. Skills Custom

### Filosofia

- Crea le tue skills e committale su git
- Riutilizzale in ogni progetto
- Costruisci **analytics-engineer-style agents** che scrivono dbt models, reviewano codice e testano modifiche in dev

---

## 4. Bug Fixing Automatizzato

### Workflow Consigliato

1. Abilita l'**MCP di Slack**
2. Incolla un thread di bug di Slack in Claude
3. Di' semplicemente: `"correggi"`

Nessun context switching richiesto.

### CI/CD Integration

```
"Correggi i test CI falliti"
```

Non microgestire il *come*. Indica a Claude i log di Docker e lascialo lavorare.

---

## 5. Tecniche di Prompting Avanzate

### Challenge Claude

> *"Grill me on these changes and don't make a PR until I pass your test."*

Fai in modo che Claude sia il tuo reviewer.

### Prove di Funzionamento

> *"Prove to me this works"*

Fai fare a Claude il diff del comportamento tra main e il tuo feature branch.

### Soluzioni Eleganti

Dopo un fix mediocre, di':

> *"Knowing everything you know now, scrap this and implement the elegant solution"*

### Specifiche Dettagliate

Scrivi spec dettagliate e riduci l'ambiguità prima di passare il lavoro. Più sei specifico, migliore sarà l'output.

---

## 6. Terminal & Environment Setup

### Terminal Consigliato

**Ghostty** è molto apprezzato per:
- Synchronized rendering
- 24-bit color
- Supporto Unicode appropriato

### Configurazione

- Usa `/statusline` per personalizzare la status bar mostrando sempre:
  - Context usage
  - Current git branch
- Colora e nomina i tuoi terminal tabs (anche con tmux)
- Un tab per task/worktree

### Voice Dictation

Usa la dettatura vocale! Parli **3x più velocemente** di quanto scrivi, e i tuoi prompt diventano molto più dettagliati.

- **macOS**: premi `fn` due volte

📖 Documentazione completa: [Terminal Config](https://code.claude.com/docs/en/terminal-config)

---

## 7. Data & Analytics con Claude

### Database CLI Integration

Chiedi a Claude Code di usare il CLI `bq` per estrarre e analizzare metriche al volo.

**Setup consigliato:**
- Crea una skill BigQuery committata nel codebase
- Tutti nel team possono usarla per query analytics direttamente in Claude Code

> *"Personalmente, non scrivo una riga di SQL da 6+ mesi."*

Funziona con qualsiasi database che abbia CLI, MCP o API.

---

## 8. Learning con Claude

### Output Style

Abilita lo stile **"Explanatory"** o **"Learning"** in `/config` per far spiegare a Claude il *perché* dietro le sue modifiche.

### Visualizzazioni

- Fai generare a Claude una **presentazione HTML visuale** per spiegare codice non familiare
- Crea slides sorprendentemente buone!

### Diagrammi ASCII

Chiedi a Claude di disegnare diagrammi ASCII di nuovi protocolli e codebase per aiutarti a comprenderli.

### Spaced Repetition Skill

Costruisci una skill di apprendimento a ripetizione spaziata:

1. Tu spieghi la tua comprensione
2. Claude fa domande di follow-up per colmare le lacune
3. Salva il risultato per revisione futura

---

## Quick Reference

| Azione | Comando/Tip |
|--------|-------------|
| Fix bug da Slack | Incolla thread + `"correggi"` |
| Fix CI | `"Correggi i test CI falliti"` |
| Code review | `"Grill me on these changes"` |
| Refactor | `"Scrap this and implement the elegant solution"` |
| Più compute | Aggiungi `"use subagents"` |
| Aggiorna memoria | `"Update your CLAUDE.md"` |

---

*Documento generato da tips del team Claude Code*

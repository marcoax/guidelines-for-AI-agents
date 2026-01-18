# Rules - AI Agent Guidelines Repository

Linee guida, best practices e prompt per lo sviluppo con Agenti AI intelligenti (Claude Code).

## Cos'è questo repository

Una collezione strutturata di **linee guida, workflow e prompt** per guidare agenti AI nella pianificazione e implementazione di software in modo efficace, coerente e qualitativo.

### Obiettivi

✓ Standardizzare comportamento agenti AI (comunicazione, workflow, decision-making)

✓ Fornire best practices per development (testing, security, code quality)

✓ Offrire modalità operative flessibili (Junior, Senior, RALPH autonoma)

✓ Documentare pattern reuse e antipattern

✓ Gestire criticità e error handling in modo sistematico

---

## Guida Rapida

### Per iniziare

1. **Leggi** il file principale: [claude.md](claude.md) (1-2 minuti)
2. **Consulta** [AGENT_GUIDELINES.md](docs/AGENT_GUIDELINES.md) per guida completa
3. **Usa** [guidelines/](guidelines/) per consultazione rapida su specifici argomenti

### Struttura directory

```
Rules/
├── claude.md                          ← Punto d'ingresso (leggi prima!)
├── README.md                          ← Questo file
│
├── docs/
│   └── AGENT_GUIDELINES.md            ← FONTE UNICA (guida completa)
│
├── guidelines/                        ← Quick reference (modulari)
│   ├── AGENT_CORE.md                  ← Comunicazione e principi
│   ├── AGENT_PLANNING.md              ← Pianificazione
│   ├── AGENT_DEVELOPMENT.md           ← Sviluppo e pattern
│   └── RALPH_IMPLEMENTATION_GUIDE.md  ← Modalità autonoma (opzionale)
│
└── prompts/                           ← Collezioni prompt
    ├── claude_prompt_it.md            ← 63 prompts (Italiano)
    ├── claude_prompt.md               ← 63 prompts (Inglese)
    └── prompts.md                     ← Template generici
```

---

## Concetti Chiave

### Modalità Operative

| Modalità | Uso | Caratteristiche |
|----------|-----|-----------------|
| **Junior** | Apprendimento, supervisione completa | Step-by-step, spiegazioni, attesa conferma |
| **Senior** | Implementazione rapida, review intermedia | Esecuzione veloce, output sintetico |
| **RALPH** | Esecuzione autonoma (opzionale) | Loop iterativi, tracking automatico |

### Workflow Fondamentale

```
1. CHECKLIST ← Sempre first step
   ├─ Implementazioni simili nel progetto?
   ├─ Stack/versioni compatibili?
   └─ Blocchi tecnici?

2. PIANO ← Obbligatorio prima del codice
   ├─ Sequenza numerata sintetica
   └─ Alternativa: .md dettagliato (su richiesta)

3. MODALITÀ ← User sceglie dopo plan approval
   └─ Junior / Senior / RALPH

4. IMPLEMENTAZIONE ← Con pattern reuse
   └─ Cerca esempi simili nel codebase PRIMA

5. TESTING ← Se rilevante
   └─ Unit / Integration / E2E

6. DELIVERY ← Con tracking e documentazione
```

### Principi

- **Obbligatorio** 📋: Checklist, Piano, Pattern reuse, Testing quando rilevante
- **Conciso** ✂️: Sacrifica grammatica per brevità, no spiegazioni lunghe
- **Tecnico** ⚙️: Diretto al punto, proposta + alternative + conferma
- **Sicuro** 🔒: Input validation, OWASP, no hardcoded secrets
- **Tracciato** 📊: Progress tracking via .md, criticità categorizzate

---

## Utilizzo

### Nuovo Progetto

1. Copia/referenzia questo repository nel tuo progetto
2. Condividi [claude.md](claude.md) con agente e team
3. Agente legge [AGENT_GUIDELINES.md](docs/AGENT_GUIDELINES.md)
4. Usa [guidelines/](guidelines/) per lookup rapido

### Workflow Tipico

```
User: "Implementa sistema di cache Redis per API prodotti"

Agent legge:
├─ claude.md (overview)
├─ AGENT_GUIDELINES.md (complete guide)
└─ AGENT_CORE.md + AGENT_DEVELOPMENT.md (quick ref)

Agent esegue:
├─ Checklist: stack check, pattern reuse search
├─ Piano inline: 4 step + alternatives
├─ Scelta modalità: User sceglie (Senior/Junior/RALPH)
├─ Implementazione: cerca pattern UserService, estende
├─ Testing: propone unit + integration test
└─ Completion: file modified, metrics, ready merge
```

---

## Sezioni Principali di AGENT_GUIDELINES.md

- **Ruolo dell'agente**: Comportamento, responsabilità, boundaries
- **Stile comunicazione**: Tecnico, conciso, no emoji (eccetto severity)
- **Workflow operativo**: Checklist, piano, implementazione, testing
- **Pianificazione**: Modalità (Junior/Senior/RALPH), formati piano
- **Sviluppo**: Pattern reuse, analisi codebase, best practice
- **Testing**: Framework, coverage minimo, quando proporre
- **Security**: OWASP, input validation, hardcoded secrets, CVE
- **Error Handling**: Segnalazione, rollback, recovery
- **Code Quality**: Formatting, naming, comments, linting
- **Criticità**: Categorizzazione severity (🔴🟡🟢)
- **Esempi Pratici**: 3 workflow complete step-by-step

---

## Documentazione Avanzata

### RALPH Modalità Autonoma (opzionale)

Per esecuzione autonoma iterativa con tracking automatico:
- Leggi [RALPH_IMPLEMENTATION_GUIDE.md](guidelines/RALPH_IMPLEMENTATION_GUIDE.md)
- Setup: `scripts/ralph/prd.json`, `tasks-state.txt`, progress tracking
- Troubleshooting: Loop bloccati, prd.json malformato, rollback procedure

---

## Feedback & Aggiornamenti

### Segnalare Problemi

Se durante lo sviluppo trovi errori/ambiguità in queste linee guida:

```
---
📋 Feedback linee guida:

Problema: [descrizione]
Sezione: [nome sezione]

Proposta fix:
[testo corretto]
```

Agente attenderà conferma prima di applicare.

### Proporre Aggiornamenti

Quando emergono esigenze non coperte:

```
---
🛠 Proposta aggiornamento linee guida:

Sezione: [nome sezione]
Problema: [descrizione esigenza]

Proposta fix:
[testo aggiornamento]
```

---

## Quick Links

- 📖 [claude.md](claude.md) - Punto d'ingresso (leggi prima!)
- 📚 [AGENT_GUIDELINES.md](docs/AGENT_GUIDELINES.md) - Guida completa
- ⚡ [AGENT_CORE.md](guidelines/AGENT_CORE.md) - Comunicazione quick ref
- 📋 [AGENT_PLANNING.md](guidelines/AGENT_PLANNING.md) - Piano quick ref
- 🛠 [AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md) - Sviluppo quick ref
- 🤖 [RALPH_IMPLEMENTATION_GUIDE.md](guidelines/RALPH_IMPLEMENTATION_GUIDE.md) - Esecuzione autonoma
- 💬 [Prompts IT](prompts/claude_prompt_it.md) / [Prompts EN](prompts/claude_prompt.md)

---

## Info Repository

| Aspetto | Dettagli |
|---------|----------|
| **Versione** | 2.0 (Consolidata) |
| **Ultimo Update** | Gennaio 2026 |
| **Stato** | Production ready |
| **Lingua** | Italiano (primario) / Inglese (prompts) |
| **Modalità** | Junior, Senior, RALPH (autonoma opzionale) |
| **Best Practice** | Checklist, Piano, Pattern reuse, Testing, Security |

---

## Prossimi Passi

1. **Leggi** [claude.md](claude.md) per overview
2. **Consulta** [AGENT_GUIDELINES.md](docs/AGENT_GUIDELINES.md) per dettagli
3. **Usa** quick reference [guidelines/](guidelines/) durante development
4. **Segnala** problemi/suggerimenti nel formato specificato

**Pronto a iniziare!** 🚀

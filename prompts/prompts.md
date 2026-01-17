# Prompt Riutilizzabili

> **Nota**: Questo file contiene template generici riutilizzabili. Per workflow specifici di questo progetto, vedi [AGENT_GUIDELINES.md](../docs/AGENT_GUIDELINES.md)

---

## Implementa Piano

```
Vorrei implementare il piano presente in .claude/plans/[NOME-PIANO].md

Prima di procedere, leggi e applica le linee guida presenti in:
- docs/AGENT_CORE.md
- docs/AGENT_GUIDELINES.md
- docs/AGENT_PLANNING.md
- docs/RALPH_IMPLEMENTATION_GUIDE.md

Procedi con l'implementazione seguendo le raccomandazioni.
```

**Esempio concreto:**
```
Vorrei implementare il piano presente in .claude/plans/rustling-launching-brook.md

Prima di procedere, leggi e applica le linee guida presenti in:
- guidelines/AGENT_CORE.md
- docs/AGENT_GUIDELINES.md
- guidelines/AGENT_PLANNING.md
- docs/RALPH_IMPLEMENTATION_GUIDE.md

Procedi con l'implementazione seguendo le raccomandazioni.
```

---

## Crea Nuovo Piano

```
Vorrei creare un piano per: [DESCRIZIONE TASK]

Prima di procedere, leggi e applica le linee guida presenti in:
- guidelines/AGENT_CORE.md
- docs/AGENT_GUIDELINES.md
- guidelines/AGENT_PLANNING.md

Analizza il codebase e crea un piano dettagliato seguendo le best practices.
```

---

## Review Codice vs Checklist

```
Come senior developer e tech lead, verifica che le modifiche coprano quanto richiesto nel checklist [nome-file]. Segnala errori, inconsistenze, file mancanti. Se trovi implementazioni non documentate, chiedi prima di procedere.
```

---

## Review PR Generico

```
Come senior developer, verifica che il codice sia tecnicamente corretto, rispetti i pattern esistenti, e non introduca bug o inconsistenze. Segnala problemi di sicurezza, performance, o maintainability.
```

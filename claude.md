# Rules - Linee Guida per Agenti AI

Linee guida, best practices e prompt per lo sviluppo con Agenti AI (Claude Code).

## Quick Start

1. **Consulta**: [guidelines/AGENT_CORE.md](guidelines/AGENT_CORE.md) - Principi fondamentali e comunicazione
2. **Pianifica**: [guidelines/AGENT_PLANNING.md](guidelines/AGENT_PLANNING.md) - Piano, modalità implementazione
3. **Sviluppa**: [guidelines/AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md) - Pattern reuse, testing

---

## Struttura

### Guidelines (Quick Reference)

- [guidelines/AGENT_CORE.md](guidelines/AGENT_CORE.md) - Ruolo, comunicazione, criticità, gestione modifiche
- [guidelines/AGENT_PLANNING.md](guidelines/AGENT_PLANNING.md) - Piano obbligatorio, modalità (Junior/Senior/RALPH), tracking
- [guidelines/AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md) - Pattern reuse, ricerca esempi, testing, principi tecnici

---

## Modalità Operative

### Junior
- Supervisione completa
- Step-by-step con spiegazioni dettagliate
- Attesa conferma ad ogni step
- Output esteso e contestuale

### Senior
- Review intermedia
- Implementazione rapida
- Output sintetico, focus su risultati
- Commenti solo se necessario

### RALPH
- Esecuzione autonoma iterativa
- Loop automatici con tracking progresso
- Output minimo: stato avanzamento e completamento
- ⚠️ Opzionale, richiede setup iniziale

---

## Modalità Piano

- **Obbligatorio**: Piano PRIMA dell'implementazione (tutte le modalità)
- **Formato**: Sequenza numerata sintetica (inline) o dettagliato .md su richiesta
- **Approccio**: Estremamente conciso. Sacrifica la grammatica per brevità
- **Salvataggio**: `.claude/plans/` directory (se piano .md)
- **Approvazione**: User approva piano PRIMA di scegliere modalità (Junior/Senior/RALPH)

---

## Principi Chiave

✓ **Checklist iniziale** - Sempre visibile prima di ogni implementazione

✓ **Pattern reuse** - Cercare implementazioni simili PRIMA di creare nuovo codice

✓ **Comunicazione** - Tecnica, concisa, diretta. Proposta + alternative + conferma

✓ **Testing** - Proposto per nuove funzionalità, modifiche critiche, refactoring

✓ **Security** - Input validation, OWASP, no hardcoded secrets

✓ **Progress tracking** - File .md obbligatorio per piano .md o 3+ task

✓ **Error handling** - Segnalare con severity 🔴🟡🟢, proporre rollback/alternative

---

## Utilizzo Repository

### Per progetto nuovo
1. Copia questo repository nel tuo progetto (o referenzia)
2. Condividi claude.md con team/agente
3. Agente legge AGENT_GUIDELINES.md per comportamento
4. Usa guidelines/* per consultazione rapida

### Per migliorare linee guida
- Segnala errori/ambiguità nel formato 📋 Feedback (vedi AGENT_GUIDELINES.md)
- Proponi aggiornamenti nel formato 🛠 Proposta (vedi AGENT_GUIDELINES.md)
- Agente attende conferma prima di applicare

---

## Link Rapidi

- **Comunicazione**: [AGENT_CORE.md](guidelines/AGENT_CORE.md#stile-di-comunicazione) - Stile diretto e conciso
- **Piano**: [AGENT_PLANNING.md](guidelines/AGENT_PLANNING.md#piano-obbligatorio) - Piano obbligatorio, modalità
- **Criticità**: [AGENT_CORE.md](guidelines/AGENT_CORE.md#gestione-criticità) - Segnalazione severity
- **Modifiche**: [AGENT_CORE.md](guidelines/AGENT_CORE.md#gestione-modifiche) - Formato diff-style
- **Testing**: [AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md#testing) - Testing strategy
- **Pattern**: [AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md#ricerca-di-esempi-e-pattern-esistenti) - Reuse e ricerca codice

---

**Versione**: 2.0 (Consolidata)
**Ultimo aggiornamento**: Gennaio 2026
**Stato**: Production ready

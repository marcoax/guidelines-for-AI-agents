# Rules - Agenti AI

Linee guida Claude Code.

---

## Workflow

```
Checklist → Piano → Modalità → Implementazione
```

1. **Checklist**: Verifica implementazioni simili, stack, blocchi
2. **Piano**: Sequenza sintetica (obbligatorio pre-code)
3. **Modalità**: Junior (step-by-step) | Senior (rapido) | RALPH (autonomo)
4. **Implementazione**: Pattern reuse, testing quando rilevante

---

## Quick Reference

- [AGENT_CORE.md](guidelines/AGENT_CORE.md) - Comportamento, comunicazione
- [AGENT_PLANNING.md](guidelines/AGENT_PLANNING.md) - Pianificazione, modalità
- [AGENT_DEVELOPMENT.md](guidelines/AGENT_DEVELOPMENT.md) - Sviluppo, testing

---

## Storage

- Piani: `~/.claude/plans/[nome].md`
- Progress: `progress/[nome]_progress.md` (agent crea dir al primo task)

---

## Progressive Disclosure

Entry (questo file) → [README.md](README.md) onboarding → [guidelines/](guidelines/) quick ref → [docs/](docs/) deep dive

---

**Versione**: 2.0 | **Update**: 2026-01 | **Stato**: Production

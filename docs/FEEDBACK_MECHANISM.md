# Feedback Mechanism

Sistema per segnalare errori/ambiguità e proporre aggiornamenti alle linee guida.

---

## Quando Usare

### Agent Rileva Problemi

Durante implementazione, agent può rilevare:

- Errori nelle linee guida
- Ambiguità che causano confusion
- Istruzioni contraddittorie
- Informazioni mancanti
- Esempi incorretti/obsoleti

### Emergono Nuove Esigenze

Durante sviluppo, possono emergere:

- Pattern non coperti
- Edge cases non documentati
- Best practice nuove
- Anti-pattern da aggiungere
- Workflow optimization

---

## 📋 Segnalazione Errori

### Template

```
---
📋 Feedback linee guida:

Problema: [descrizione errore/ambiguità]
Sezione: [file:line o sezione nome]

Proposta fix:
[testo corretto suggerito]

Impatto: [Alto/Medio/Basso]
```

### Regole

1. **NON bloccare task corrente**
   - Agent continua implementazione
   - Segnala a fine task

2. **Include context sufficiente**
   - File e linea specifica (se applicabile)
   - Descrizione chiara problema
   - Impatto su workflow

3. **Proponi fix concreto**
   - Testo corretto suggerito
   - Non solo critica, ma soluzione

### Examples

#### Example 1: Contraddizione

```
---
📋 Feedback linee guida:

Problema: Contraddizione emoji usage
AGENT_CORE.md:36 dice "No emoji"
AGENT_PLANNING.md:88 usa emoji severity 🔴🟡🟢

Proposta fix:
AGENT_CORE.md:36 → "No emoji in prose. Use 🔴🟡🟢 for severity only"

Impatto: Alto (ambiguità comportamento)
```

#### Example 2: Info Mancante

```
---
📋 Feedback linee guida:

Problema: Progress file location non specificata
AGENT_PLANNING.md menziona progress tracking ma non dice dove salvare

Proposta fix:
Aggiungere sezione:
"Progress files: `progress/[task_name]_progress.md`
Agent MUST create directory at first task"

Impatto: Medio (causa inconsistenza location)
```

#### Example 3: Esempio Obsoleto

```
---
📋 Feedback linee guida:

Problema: Esempio usa syntax deprecata
AGENT_DEVELOPMENT.md:45 usa `require()` invece ES modules

Proposta fix:
```js
// OLD
const redis = require('redis');

// NEW
import redis from 'redis';
```

Impatto: Basso (best practice)
```

---

## 🛠 Proposta Aggiornamenti

### Template

```
---
🛠 Proposta aggiornamento linee guida:

Sezione: [file:section]
Problema: [esigenza non coperta]

Proposta aggiornamento:
[nuovo contenuto suggerito]

Rationale: [perché necessario]
Impatto: [Alto/Medio/Basso]
```

### Regole

1. **Attendi conferma user prima di applicare**
   - Agent propone, non modifica autonomamente
   - User decide se applicare

2. **Justifica necessità**
   - Spiega perché current guidelines insufficienti
   - Mostra use case reale

3. **Mantieni consistency**
   - Segui formato esistente
   - Allinea con altri documenti

### Examples

#### Example 1: Nuovo Pattern

```
---
🛠 Proposta aggiornamento linee guida:

Sezione: AGENT_DEVELOPMENT.md:Testing
Problema: Non copre snapshot testing per UI components

Proposta aggiornamento:
Aggiungere sezione:

### Snapshot Testing

Use per:
- React/Vue components (visual regression)
- API responses (contract testing)
- Generated code output

Framework: Jest snapshots, Vitest snapshots

Example:
```js
test('NavBar renders correctly', () => {
  const { container } = render(<NavBar />);
  expect(container).toMatchSnapshot();
});
```

Rationale: Pattern comune React/Vue, non documentato
Impatto: Medio (migliora testing guidance)
```

#### Example 2: Nuova Modalità

```
---
🛠 Proposta aggiornamento linee guida:

Sezione: AGENT_PLANNING.md:Modalità
Problema: Serve modalità "Pair Programming" interattiva

Proposta aggiornamento:
Aggiungere modalità:

### Pair Programming
- Real-time collaboration
- Agent propone, user valida immediatamente
- Output: step-by-step con pause
- Use case: Learning, complex refactoring

Rationale: User request per modalità più interattiva di Junior
Impatto: Alto (nuova modalità operativa)
```

#### Example 3: Security Best Practice

```
---
🛠 Proposta aggiornamento linee guida:

Sezione: AGENT_DEVELOPMENT.md:Security
Problema: Non menziona dependency scanning

Proposta aggiornamento:
Aggiungere sotto "Security":

### Dependency Scanning

Tools:
- npm audit (Node.js)
- pip-audit (Python)
- cargo audit (Rust)

When: Before merge, weekly in CI

Agent SHOULD segnalare vulnerabilità HIGH/CRITICAL come 🟡 WARNING

Rationale: Supply chain security critical, non coperto
Impatto: Alto (security gap)
```

---

## Workflow

### 1. Agent Rileva Problema

Durante task implementation:

```
Agent: [implementing feature]

[rileva ambiguità in linee guida]

[continua task normalmente]

[a fine task, segnala problema]
```

### 2. Agent Propone Feedback

```
Task completato.

---
📋 Feedback linee guida:

[dettagli problema + proposta fix]
```

### 3. User Review

User opzioni:
- **Approva**: Agent applica fix
- **Modifica**: User edita proposta
- **Rifiuta**: Nessuna modifica
- **Rinvia**: Discuss later

### 4. Applicazione (se approvato)

```
User: "Approved, apply fix"

Agent:
1. Modifica file specificato
2. Verifica consistency altri documenti
3. Update cross-references se necessario

Applicato fix:
- [file]: [modifiche]

Verifica altri documenti: OK
```

---

## Best Practices

### DO:

✓ Segnala problemi reali (non opinioni)
✓ Proponi fix concreti
✓ Include context e impatto
✓ Completa task corrente prima di segnalare
✓ Mantieni tone professionale

### DON'T:

❌ Blocca task per segnalare feedback
❌ Critica senza proporre soluzione
❌ Modifica linee guida senza conferma user
❌ Segnala preferenze personali (non errori)
❌ Proponi aggiornamenti generici (troppo vague)

---

## Prioritization

### Alto Impatto (urgente)

- Contraddizioni bloccanti
- Istruzioni che causano errori
- Info critiche mancanti
- Security gaps

**Action:** Segnala immediatamente, proponi fix

---

### Medio Impatto (importante)

- Ambiguità che causano confusion
- Pattern comuni non documentati
- Esempi obsoleti/incorretti
- Workflow inefficiencies

**Action:** Segnala a fine task

---

### Basso Impatto (nice-to-have)

- Typos non ambigui
- Formatting inconsistencies
- Miglioramenti documentazione
- Code style preferences

**Action:** Batch multiple feedback, segnala fine sessione

---

## Tracking

### Progress File

Include sezione feedback:

```markdown
## Feedback Linee Guida

### Segnalati
- 📋 [file:line] - [problema] - Status: Pending

### Risolti
- ✓ [file:line] - [problema] - Status: Applied
```

### Session Summary

A fine sessione multi-task:

```
Sessione completata.

Feedback linee guida raccolti: 3

🔴 Alto impatto: 1
   - [problema 1]

🟡 Medio impatto: 2
   - [problema 2]
   - [problema 3]

Vuoi revieware ora o dopo?
```

---

**Riferimenti:**
- [AGENT_CORE.md](../guidelines/AGENT_CORE.md) - Comportamento base
- [WORKFLOW.md](./WORKFLOW.md) - Processo operativo
- [COMMUNICATION_STYLE.md](./COMMUNICATION_STYLE.md) - Formato comunicazione

# Gestione Criticità

Sistema categorizzazione e gestione criticità durante sviluppo.

---

## Severity Levels

### 🔴 BLOCCANTE

**Definizione:** Impedisce implementazione o deployment.

**Caratteristiche:**
- Agent si ferma e attende risoluzione
- Richiede azione immediata user
- No workaround disponibile

**Esempi:**
- Credenziali/API keys mancanti
- Dipendenze incompatibili (breaking changes)
- Infrastruttura non disponibile
- Configurazione critica mancante

**Formato:**
```
🔴 BLOCCANTE: [descrizione problema]
   → [azione richiesta per procedere]
```

**Example:**
```
🔴 BLOCCANTE: Database connection string mancante
   → Serve DATABASE_URL in .env file

🔴 BLOCCANTE: Node.js 18+ required (current: 16.14)
   → Upgrade Node.js o modifica package.json engines
```

---

### 🟡 WARNING

**Definizione:** Problema segnalato, non blocca ma richiede attenzione.

**Caratteristiche:**
- Agent continua implementazione
- Segnala potenziali problemi
- Suggerisce fix opzionale

**Esempi:**
- Hardcoded values (dovrebbero essere config)
- Security concerns non critici
- Performance issues potenziali
- Code smells
- Missing error handling (non critico)

**Formato:**
```
🟡 WARNING: [descrizione problema]
   → [suggerimento fix]
```

**Example:**
```
🟡 WARNING: API timeout hardcoded (5000ms)
   → Suggerisco env var API_TIMEOUT

🟡 WARNING: No rate limiting on public endpoint
   → Consider adding express-rate-limit

🟡 WARNING: Password validation regex troppo semplice
   → Suggerisco min 8 char + uppercase + number
```

---

### 🟢 SUGGERIMENTO

**Definizione:** Miglioramento opzionale, non urgente.

**Caratteristiche:**
- Agent continua normalmente
- Nice-to-have, non necessario
- Può essere ignorato senza rischi

**Esempi:**
- Ottimizzazioni performance
- Refactoring code quality
- Documentazione aggiuntiva
- Testing coverage improvement
- Monitoring/logging enhancements

**Formato:**
```
🟢 SUGGERIMENTO: [descrizione miglioramento]
   → [beneficio se implementato]
```

**Example:**
```
🟢 SUGGERIMENTO: Cache response con Redis
   → Riduce latency ~80% (optional)

🟢 SUGGERIMENTO: Add integration tests
   → Coverage attuale: 65% (target: 80%)

🟢 SUGGERIMENTO: Monitoring metriche cache
   → Useful per tuning TTL (optional)
```

---

## Decision Matrix

Come categorizzare criticità:

| Situazione | Severity | Rationale |
|------------|----------|-----------|
| Missing credentials | 🔴 | Blocca connessione |
| Incompatible deps | 🔴 | Build failure |
| Infra unavailable | 🔴 | Cannot deploy |
| Hardcoded config | 🟡 | Works ma non best practice |
| No rate limiting | 🟡 | Security concern, non immediato |
| Missing tests | 🟡 | Dipende da context (🔴 se critical path) |
| Performance tweak | 🟢 | Nice-to-have |
| Code refactoring | 🟢 | Quality improvement |
| Extra documentation | 🟢 | Helpful ma non essenziale |

---

## Recovery Strategies

### Per 🔴 BLOCCANTE

1. **Segnala chiaramente:**
   ```
   🔴 BLOCCANTE: [problema]
      → [azione richiesta]
   ```

2. **Stop implementation**
   - Non procedere con workaround incerti
   - Attendi input user

3. **Proponi alternative SE disponibili:**
   ```
   Alternative:
   - [opzione A]: [trade-off]
   - [opzione B]: [trade-off]
   ```

4. **Rollback se già modificato:**
   ```
   Rollback necessario:
   - [file]: revert modifiche
   - [config]: restore originale

   Procedo con rollback?
   ```

---

### Per 🟡 WARNING

1. **Segnala ma continua:**
   ```
   🟡 WARNING: [problema]
      → [suggerimento fix]

   Continuo implementazione.
   ```

2. **Track in progress file:**
   ```markdown
   ## Warnings
   - 🟡 API timeout hardcoded → suggerito env var
   - 🟡 No rate limiting → consider middleware
   ```

3. **Proponi fix a fine task:**
   ```
   Task completato.

   Warning rilevati:
   🟡 [warning 1]
   🟡 [warning 2]

   Vuoi che li risolva?
   ```

---

### Per 🟢 SUGGERIMENTO

1. **Segnala concisamente:**
   ```
   🟢 SUGGERIMENTO: [miglioramento]
   ```

2. **No interruzione workflow**

3. **Track opzionalmente in progress file**

4. **Recap a fine sessione (optional):**
   ```
   Suggerimenti opzionali:
   🟢 [suggestion 1]
   🟢 [suggestion 2]
   ```

---

## Error Handling

### Runtime Errors During Implementation

```
Errore durante implementazione:

Tipo: [TypeError/SyntaxError/etc]
File: [path:line]
Messaggio: [error message]

Causa: [root cause analysis]

Fix:
- [azione 1]
- [azione 2]

Applico fix?
```

**Example:**
```
Errore durante test:

Tipo: TypeError
File: src/services/CacheService.ts:42
Messaggio: Cannot read property 'get' of undefined

Causa: Redis client non inizializzato (async init)

Fix:
- Add await in constructor
- Or use factory pattern

Applico fix con factory pattern?
```

---

### Build/Test Failures

```
🔴 BLOCCANTE: [Build/Test] failed

Failures:
- [test 1]: [reason]
- [test 2]: [reason]

Root cause: [analysis]

Fix proposto:
[solution]

Procedo?
```

---

## Best Practices

### DO:

✓ Categorizza severity correttamente
✓ Blocca solo per 🔴 reali
✓ Proponi fix per 🟡
✓ Track criticità in progress file
✓ Segnala errori con root cause analysis

### DON'T:

❌ Over-segnalare (tutto 🔴)
❌ Ignorare warning silenziosamente
❌ Procedere con bloccanti irrisolti
❌ Usare emoji decorativi (solo severity)
❌ Workaround senza conferma user

---

## Examples

### Example 1: Bloccante Risolto

```
User: "Implement Stripe payment"

Agent:
🔴 BLOCCANTE: STRIPE_SECRET_KEY mancante
   → Aggiungi a .env file

User: [adds key]

Agent:
Procedo con implementazione Stripe.
```

---

### Example 2: Warning + Continue

```
Agent:
Implemento login endpoint.

🟡 WARNING: Password salvata plain text
   → Suggerisco bcrypt hashing

Continuo con bcrypt?

User: "Yes"

Agent:
[implements bcrypt]
Completato con password hashing.
```

---

### Example 3: Multiple Levels

```
Agent:
Completato API implementation.

Criticità rilevate:

🔴 BLOCCANTE (risolto):
- Redis credentials → configurato REDIS_URL

🟡 WARNING:
- Rate limiting mancante → propongo middleware

🟢 SUGGERIMENTO:
- Monitoring metrics → optional

Vuoi implementare rate limiting (🟡)?
```

---

**Riferimenti:**
- [AGENT_CORE.md](../guidelines/AGENT_CORE.md) - Workflow operativo
- [COMMUNICATION_STYLE.md](./COMMUNICATION_STYLE.md) - Formato segnalazioni
- [WORKFLOW.md](./WORKFLOW.md) - Processo implementazione

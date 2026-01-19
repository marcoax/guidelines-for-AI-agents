# Claude Code - Prompt Suggeriti per Categoria

> Estratto e categorizzato da: Common workflows - Claude Code Docs

---

## 1. COMPRENDERE LE CODEBASE

Prompt per ottenere una panoramica rapida e approfondire la struttura del codice:

- `dammi una panoramica di questa codebase`
- `spiega i principali pattern architetturali usati qui`
- `quali sono i modelli dati principali?`
- `come viene gestita l'autenticazione?`

---

## 2. TROVARE E LOCALIZZARE IL CODICE

Prompt per localizzare funzionalità specifiche, file e capire le interazioni tra componenti:

- `trova i file che gestiscono l'autenticazione dell'utente`
- `come lavorano insieme questi file di autenticazione?`
- `traccia il processo di login da front-end a database`

---

## 3. CORREZIONE BUG

Prompt per identificare, comprendere e correggere bug:

- `sto vedendo un errore quando eseguo npm test`
- `suggerisci alcuni modi per correggere il @ts-ignore in user.ts`
- `aggiorna user.ts per aggiungere il null check che hai suggerito`

---

## 4. REFACTORING DEL CODICE

Prompt per modernizzare e migliorare il codice esistente:

- `trova l'uso di API deprecate nella nostra codebase`
- `suggerisci come refactorizzare utils.js per usare le moderne funzionalità JavaScript`
- `refactorizza utils.js per usare le funzionalità ES2024 mantenendo lo stesso comportamento`
- `esegui i test per il codice refactorizzato`

---

## 5. UTILIZZO DEI SUBAGENTI

Prompt per sfruttare gli AI subagenti specializzati per compiti specifici:

- `/agents` (visualizza gli subagenti disponibili)
- `rivedi le mie recenti modifiche al codice per problemi di sicurezza`
- `esegui tutti i test e correggi eventuali errori`
- `usa il subagente code-reviewer per controllare il modulo di autenticazione`
- `fai in modo che il subagente debugger indaghi sul perché gli utenti non riescono ad accedere`

---

## 6. MODALITÀ PIANIFICAZIONE (Analisi Complessa)

Prompt per creare piani dettagliati prima di implementare modifiche:

- `ho bisogno di refactorizzare il nostro sistema di autenticazione per usare OAuth2. Crea un piano di migrazione dettagliato.`
- `che dire della compatibilità all'indietro?`
- `come dovremmo gestire la migrazione del database?`
- `analizza il sistema di autenticazione e suggerisci miglioramenti`

### Template: Implementa Piano

```
Vorrei implementare il piano presente in .claude/plans/[NOME-PIANO].md

Prima di procedere, leggi e applica le linee guida presenti in:
- guidelines/AGENT_CORE.md
- guidelines/AGENT_PLANNING.md
- guidelines/AGENT_DEVELOPMENT.md

Procedi con l'implementazione seguendo le raccomandazioni.
```

### Template: Crea Nuovo Piano

```
Vorrei creare un piano per: [DESCRIZIONE TASK]

Prima di procedere, leggi e applica le linee guida presenti in:
- guidelines/AGENT_CORE.md
- guidelines/AGENT_PLANNING.md
- guidelines/AGENT_DEVELOPMENT.md

Analizza il codebase e crea un piano dettagliato seguendo le best practices.
```

---

## 7. RACCOLTA REQUISITI (Modalità Intervista)

Prompt per far fare a Claude domande chiarificatrici:

- `intervistami su questa funzionalità prima di iniziare: sistema di notifica utente`
- `aiutami a riflettere sui requisiti per l'autenticazione facendo domande`
- `fai domande chiarificatrici per definire questa specifica: elaborazione pagamenti`

---

## 8. TESTING

Prompt per identificare, generare ed eseguire test:

- `trova funzioni in NotificationsService.swift non coperte dai test`
- `aggiungi test per il servizio di notifica`
- `aggiungi casi di test per condizioni limite nel servizio di notifica`
- `esegui i nuovi test e correggi eventuali errori`

---

## 9. PULL REQUEST

Prompt per creare e revisionare pull request:

- `riassumi le modifiche che ho apportato al modulo di autenticazione`
- `crea una pr`
- `migliora la descrizione della PR con più contesto sui miglioramenti di sicurezza`
- `aggiungi informazioni su come questi cambiamenti sono stati testati`

### Template: Review PR Generico

```
Come senior developer, verifica che il codice sia tecnicamente corretto, rispetti i pattern esistenti, e non introduca bug o inconsistenze. Segnala problemi di sicurezza, performance, o maintainability.
```

---

## 10. DOCUMENTAZIONE

Prompt per generare e migliorare la documentazione del codice:

- `trova funzioni senza appropriati commenti JSDoc nel modulo di autenticazione`
- `aggiungi commenti JSDoc alle funzioni non documentate in auth.js`
- `migliora la documentazione generata con più contesto e esempi`
- `controlla se la documentazione segue i nostri standard di progetto`

---

## 11. ANALISI DI IMMAGINI E CONTENUTI VISIVI

Prompt per analizzare immagini, screenshot, diagrammi e mockup di design:

- `cosa mostra questa immagine?`
- `descrivi gli elementi UI in questo screenshot`
- `ci sono elementi problematici in questo diagramma?`
- `ecco uno screenshot dell'errore. Cosa lo causa?`
- `questo è il nostro schema database attuale. Come dovremmo modificarlo per la nuova funzionalità?`
- `genera CSS per corrispondere a questo mockup di design`
- `quale struttura HTML ricreerebbe questo componente?`

---

## 12. RIFERIMENTI A FILE E DIRECTORY

Prompt per referenziare e analizzare file e directory:

- `spiega la logica in @src/utils/auth.js`
- `qual è la struttura di @src/components?`
- `mostrami i dati da @github:repos/owner/repo/issues`

---

## 13. RAGIONAMENTO ESTESO / DECISIONI ARCHITETTURALI

Prompt per ragionamento profondo e risoluzione di problemi complessi:

- `ultrathink: progetta un livello di caching per la nostra API`

---

## 14. VERIFICA E LINTING (Stile Unix)

Prompt per revisione del codice e controlli di qualità:

- `sei un linter. per favore guarda i cambiamenti vs. main e segnala eventuali problemi legati a typo. segnala il nome del file e il numero di riga su una riga, e una descrizione del problema sulla seconda riga. non restituire nessun altro testo.`

### Template: Review Codice vs Checklist

```
Come senior developer e tech lead, verifica che le modifiche coprano quanto richiesto nel checklist [nome-file]. Segnala errori, inconsistenze, file mancanti. Se trovi implementazioni non documentate, chiedi prima di procedere.
```

---

## 15. DOMANDE SULLE FUNZIONALITÀ

Prompt per chiedere a Claude informazioni sulle sue funzionalità e limitazioni:

- `Claude Code può creare pull request?`
- `come gestisce Claude Code i permessi?`
- `quali slash command sono disponibili?`
- `come uso MCP con Claude Code?`
- `come configuro Claude Code per Amazon Bedrock?`
- `quali sono i limiti di Claude Code?`

---

## Riepilogo

**Totale: 63 prompt distinti suggeriti** in **15 categorie**

Questa collezione copre tutti i principali workflow nello sviluppo con Claude Code, inclusi:

- Analisi e comprensione del codice
- Correzione di bug e debugging
- Refactoring e modernizzazione
- Testing e garanzia di qualità
- Documentazione
- Analisi di contenuti visivi
- Pianificazione e raccolta requisiti

## 16. AGENT
prompt per creare AGENT.md, Claude.md

Voglio che tu rifattorizzi il mio file AGENTS.md seguendo i principi di progressive disclosure.

Segui questi passaggi:

1. **Trova contraddizioni**: Identifica eventuali istruzioni in conflitto tra loro. Per ogni contraddizione, chiedimi quale versione voglio mantenere.

2. **Identifica l'essenziale**: Estrai solo ciò che appartiene al file AGENTS.md principale:
   - Descrizione del progetto in una frase
   - Package manager (se non è npm)
   - Comandi build/typecheck non standard
   - Qualsiasi cosa veramente rilevante per ogni singolo task

3. **Raggruppa il resto**: Organizza le istruzioni rimanenti in categorie logiche (es: convenzioni TypeScript, pattern di testing, design API, workflow Git). Per ogni gruppo, crea un file markdown separato.

4. **Crea la struttura**: Genera:
   - Un AGENTS.md principale minimale con link markdown ai file separati
   - Ogni file separato con le relative istruzioni
   - Una struttura suggerita per la cartella docs/

5. **Segnala per eliminazione**: Identifica eventuali istruzioni che sono:
   - Ridondanti (l'agent lo sa già)
   - Troppo vaghe per essere azionabili
   - Ovvie (tipo "scrivi codice pulito")

### Modalità Piano
- Rendi il piano estremamente conciso. Sacrifica la grammatica per la concisione.
- Alla fine di ogni piano, dammi una lista di domande irrisolte a cui rispondere, se presenti.

## 17 STACK Tecnologico

Analizza il progetto nella directory corrente (package.json, composer.json, file di configurazione) e aggiungi all'INIZIO del file CLAUDE.md un riepilogo conciso delle tecnologie utilizzate:

**Stack Tecnologico**

- **Frontend**: [framework, librerie UI, gestione stato]
- **Backend**: [linguaggi, framework, API]
- **Database**: [tipo di database, ORM/query builder]
- **Altri strumenti**: [deployment, testing, package manager]

Mantieni ogni punto su una riga singola. Inserisci questa sezione come primo contenuto del file, prima di tutto il resto.


## 18 SKILLS

### agent-md-creator
Crea un CLAUDE.md per questo progetto
Generate AGENT.md
Setup project documentation

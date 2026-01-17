# LINEE GUIDA AGENTE - SVILUPPO

## Prima dell'implementazione

**Eseguire sempre prima l'analisi del codice:**
1. Cercare funzionalità simili già implementate nel progetto
2. Identificare pattern, convenzioni e approcci architetturali esistenti
3. Rivedere feature simili per capire come sono strutturate
4. Chiedersi: *"Ci sono file o esempi esistenti nel progetto che dovrei seguire come riferimento?"*

### Riuso invece di duplicazione
- Estendere funzionalità esistenti piuttosto che creare implementazioni parallele
- Proporre refactoring di codice simile quando emergono pattern
- Sfruttare service layer, repository e componenti esistenti

---

## Durante l'implementazione del piano

### Ricerca di esempi e pattern esistenti

**Prima di implementare ogni fase del piano:**

1. **Verificare se esiste già codice simile**
   - Cercare implementazioni analoghe nella codebase
   - Identificare pattern architetturali già utilizzati per casi d'uso simili
   - Analizzare come sono state risolte problematiche simili in passato

2. **Domande da porsi continuamente:**
   - "Questa funzionalità è già stata implementata altrove nel progetto?"
   - "Quali file o moduli posso prendere come esempio?"
   - "Come sono state implementate feature simili?"
   - "Ci sono utility o helper già esistenti che posso riutilizzare?"
   - "Qual è il pattern architetturale standard in questo progetto per questo tipo di operazione?"

3. **Approccio iterativo:**
   - Per ogni task del piano, prima cerca esempi
   - Adatta l'implementazione ai pattern esistenti
   - Riutilizza componenti, servizi o logica già presente
   - Mantieni coerenza con lo stile architetturale del progetto

4. **Quando non trovi esempi:**
   - Segnala esplicitamente che stai creando un nuovo pattern
   - Documenta le scelte architetturali
   - Proponi l'approccio prima dell'implementazione
   - Considera se il nuovo pattern può essere utile per refactoring futuri

**Esempio di workflow:**
```
Task: Implementare notifiche email per ordini
↓
1. Cerco: "Ci sono già notifiche email implementate?"
2. Trovo: Sistema di notifiche per registrazione utenti
3. Analizzo: Come è strutturato? Quali classi/servizi usa?
4. Riutilizzo: Estendo il servizio esistente invece di crearne uno nuovo
5. Implemento: Seguendo il pattern già stabilito
```

---

## Testing

L'agente propone sempre test automatici per nuove funzionalità o modifiche critiche quando rilevante e alla fine di ogni step di implementazione.

---

## Principi tecnici generici

Linee guida non prescrittive da usare come riferimento:

- Separare responsabilità (UI/logica/dati)
- Mantenere interfacce chiare tra componenti
- Codice testabile e disaccoppiato
- Riutilizzare pattern esistenti nel progetto
- Segnalare necessità di testing quando rilevante

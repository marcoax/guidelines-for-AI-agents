## Review: BlockedDate (model, builder, config, traduzione) + contatti.blade.php + list.blade.php + SlotRecurrenceGenerator

### Violazioni Architettura

1. `resources/lang/it/admin.php:173` — `blockeddates` non in ordine alfabetico → spostare prima di `articles` (riga 174) ma dopo `appointmentslotrecurrences`/`appointmentnotifications`. Attualmente corretto per posizione (dopo `appointmentnotifications`), ma il blocco `articles` (riga 174) usa tab diverso dagli altri — non una violazione blockeddates.
2. `resources/views/website/contatti.blade.php:20` — rimosso `<h1 class="page-title">{{ $article->title }}</h1>` → la pagina perde il titolo principale, potenziale problema SEO/accessibilità. Valutare se intenzionale.

### Best Practices

1. `resources/views/admin/list.blade.php:286` — query `\App\BlockedDate::upcomingBlocked(90)` dentro la view → spostare nel controller o passare tramite `view composer` per separare logica da presentazione
2. `resources/views/website/contatti.blade.php` — rimosso Google Maps JS (api, `ma_gmaps.js`, variabili `lat`/`long`) e tutto il codice mappa → verificare che la mappa non sia più necessaria nella pagina contatti, altrimenti è una regressione
3. `resources/views/website/contatti.blade.php:20` — `class=""` vuoto su `div#sec_prenota-online` → rimuovere attributo class vuoto

### Suggerimenti

1. `config/laraCms/admin/list.php:229` — `roles` include `user` → confermare che il ruolo `user` debba gestire date bloccate (solitamente è funzionalità admin/su)
2. `app/BlockedDate.php:65` — `$dateType = $this->id ? 'readonly' : 'string'` rende le date non modificabili dopo la creazione → comportamento intenzionale? Se sì, OK; se serve modifica, cambiare in `'string'` sempre

### OK

- Model `BlockedDate`: struttura corretta (`$fillable`, `$casts` solo boolean, getter/setter per date, `newEloquentBuilder`, wrapper statici sottili, `getFieldSpec`)
- Builder `BlockedDateBuilder`: estende `LaraCmsBuilder`, non ridefinisce `active()`, logica query nel posto giusto
- Config admin `blockeddates`: struttura completa, icona FA5 corretta (`ban`), tutti i flag CRUD presenti

Confermi? (tutto / numeri / nessuno)

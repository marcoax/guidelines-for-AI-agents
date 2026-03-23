## Review: BlockedDate (model, builder, config, traduzione, view) + contatti.blade.php + CLAUDE.md + ADMIN_ARCHITECTURE.md

**Diff analizzato**: 8 file modificati/nuovi — `BlockedDate.php`, `BlockedDateBuilder.php`, `SlotRecurrenceGenerator.php`, `list.php`, `admin.php`, `list.blade.php`, `contatti.blade.php`, `CLAUDE.md`, `ADMIN_ARCHITECTURE.md`

---

### Violazioni Architettura (ADMIN_ARCHITECTURE.md)

1. **`resources/lang/it/admin.php:173`** — Traduzione `blockeddates` non in ordine alfabetico. Si trova dopo `appointmentnotifications` ma prima di `articles`. Dovrebbe essere posizionata tra `articles` e `calls` (ordine corretto: articles, blockeddates, calls...) oppure — dato che 'b' viene prima di 'a' — prima di `appointments`. In realtà `b` > `a`, quindi `blockeddates` dovrebbe stare dopo tutti gli `appointment*` e prima di `articles`. Posizione attuale (dopo `appointmentnotifications`, prima di `articles`) e' **corretta**. ~~Nessuna violazione.~~
   **RISULTATO: OK** — ordine alfabetico rispettato.

2. **`resources/views/admin/list.blade.php:286`** — Query Eloquent eseguita direttamente nella view Blade con `\App\BlockedDate::upcomingBlocked(90)`. Sebbene il wrapper statico deleghi al Builder (pattern corretto), eseguire query nel template Blade viola il principio di separazione. Tuttavia, data la natura config-driven di LaraCMS dove il controller e' generico (`AdminPagesController`), questa e' una soluzione pragmatica accettabile per una sezione specifica. **Violazione lieve** — idealmente passare i dati dal controller o usare un View Composer.

---

### Violazioni Best Practices

1. **`resources/views/admin/list.blade.php:286`** — Riferimento diretto a classe con FQCN inline (`\App\BlockedDate::upcomingBlocked(90)`) nella view. Accoppiamento diretto view-model. Per questo progetto e' un pattern accettabile dato il controller generico, ma segnalato per consapevolezza.

2. **`resources/views/website/contatti.blade.php:20`** — L'elemento `<div id="sec_prenota-online" class="">` ha attributo `class=""` vuoto. Rimuovere l'attributo class vuoto per pulizia HTML.
   ```html
   <!-- attuale -->
   <div id="sec_prenota-online" class="">
   <!-- suggerito -->
   <div id="sec_prenota-online">
   ```

3. **`resources/views/website/contatti.blade.php`** — Rimossa la `<h1 class="page-title">` con il titolo dell'articolo. La pagina non ha piu' un heading `<h1>` visibile, il che ha impatti SEO e accessibilita'. Verificare se `<booking-app>` contiene internamente un `<h1>`, altrimenti aggiungerne uno.

4. **`app/Services/SlotRecurrenceGenerator.php:44-45`** — La verifica `BlockedDate::isDateBlocked($current)` viene eseguita per ogni singolo giorno nel range. Per range lunghi (es. 6 mesi = ~180 giorni) questo genera una query SQL per ogni giorno. Considerare un approccio con pre-fetch delle date bloccate e check in-memory.
   ```php
   // Suggerimento: pre-caricare tutte le date bloccate nel range
   $blockedDates = BlockedDate::query()->active()
       ->where('start_date', '<=', $rangeEnd->toDateString())
       ->where(function($q) use ($rangeStart) {
           $q->whereNull('end_date')
             ->orWhere('end_date', '>=', $rangeStart->toDateString());
       })->get();
   // Poi check in-memory nel loop
   ```

---

### Miglioramenti Suggeriti

1. **`app/BlockedDate.php`** — Il model e' ben strutturato ma manca la proprieta' `$table`. Sebbene Eloquent la inferisca automaticamente (`blocked_dates`), dichiararla esplicitamente migliora la leggibilita', soprattutto con nomi composti. Non obbligatorio.

2. **`config/laraCms/admin/list.php:229`** — I ruoli sono `['su', 'admin', 'user']`. Verificare se il ruolo `user` debba realmente avere accesso alla gestione delle date bloccate. Di solito la gestione calendario e' riservata a `su` e `admin`.

3. **`app/laraCms/Builders/BlockedDateBuilder.php:11`** — Il metodo `upcoming()` e' definito ma non sembra utilizzato da nessuna parte nel codice modificato. Se e' previsto per uso futuro va bene, altrimenti rimuoverlo per aderire a YAGNI.

4. **`config/laraCms/admin/list.php:213`** — `field_searchable` e' vuoto. Potrebbe essere utile aggiungere un filtro per `start_date` o `is_active` per facilitare la ricerca quando le date bloccate diventano numerose.

---

### OK

- **`app/BlockedDate.php`** — `$fillable` completo e corretto
- **`app/BlockedDate.php`** — `$casts` usato solo per `boolean`, nessuna data in casts (regola rispettata)
- **`app/BlockedDate.php`** — Getter/setter per `start_date` e `end_date` implementati correttamente con `Carbon::parse()` e formato `d-m-Y`
- **`app/BlockedDate.php`** — `newEloquentBuilder()` presente e punta a `BlockedDateBuilder`
- **`app/BlockedDate.php`** — `getFieldSpec()` ben strutturato con pattern readonly-on-edit per date
- **`app/BlockedDate.php`** — Wrapper statici sottili (`isDateBlocked`, `upcomingBlocked`) che delegano al builder
- **`app/BlockedDate.php`** — PHP 7.2 compatibile (nessun arrow function, named args, match, nullsafe)
- **`app/laraCms/Builders/BlockedDateBuilder.php`** — Estende `LaraCmsBuilder`, non ridefinisce `active()`
- **`app/laraCms/Builders/BlockedDateBuilder.php`** — Logica query nel builder, non nel model
- **`config/laraCms/admin/list.php`** — Struttura sezione `blockeddates` completa e corretta
- **`config/laraCms/admin/list.php`** — Icona FA5 senza prefisso `fa-` (`ban`)
- **`resources/lang/it/admin.php`** — Traduzione presente in ordine alfabetico
- **`CLAUDE.md`** — Aggiunta regola obbligatoria su model/admin ben posizionata
- **`guidelines/ADMIN_ARCHITECTURE.md`** — Miglioramenti documentazione (FA5, boolean in field, cleanup) tutti appropriati
- **`app/Services/SlotRecurrenceGenerator.php`** — Integrazione check date bloccate nella generazione slot logicamente corretta

---

### Nuova Best Practice Scoperta

- **Query nelle view Blade admin**: quando si aggiunge logica specifica a una view condivisa (`list.blade.php`), usare wrapper statici sui model (non query dirette). Il pattern `Model::metodoStatico()` e' gia' utilizzato ed e' il compromesso accettabile nel contesto di LaraCMS con controller generico.
- Aggiungo a ADMIN_ARCHITECTURE.md? (si/no)

---

Confermi? (tutto / numeri / nessuno / +test)

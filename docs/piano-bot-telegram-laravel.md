# Bot Telegram - Monitoraggio Pagina Web con Laravel

## Obiettivo

Creare un bot Telegram che ogni ora controlla se un determinato testo è presente in una pagina web e invia una notifica.

---

## 1. Creare il Bot Telegram

1. Apri Telegram e cerca **@BotFather**
2. Invia il comando `/newbot`
3. Scegli un **nome** (es. "Web Monitor Bot") e un **username** (es. `web_monitor_marco_bot`)
4. Salva il **token** che BotFather ti restituisce (es. `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`)
5. Avvia una chat con il tuo bot e invia un messaggio qualsiasi (es. "ciao")
6. Visita nel browser: `https://api.telegram.org/bot<IL_TUO_TOKEN>/getUpdates`
7. Trova il campo `"chat":{"id": XXXXXXX}` — quello è il tuo **chat_id**

---

## 2. Configurare Laravel

### 2.1 Variabili d'ambiente (`.env`)

Aggiungi queste righe al file `.env` del tuo progetto Laravel:

```env
TELEGRAM_BOT_TOKEN=123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11
TELEGRAM_CHAT_ID=123456789
MONITOR_URL=https://esempio.com/pagina-da-monitorare
MONITOR_TEXT=testo da cercare
```

### 2.2 Creare il Command Artisan

Esegui:

```bash
php artisan make:command CheckWebPage
```

### 2.3 Codice del Command

File: `app/Console/Commands/CheckWebPage.php`

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Support\Facades\Http;
use Illuminate\Support\Facades\Log;
use Illuminate\Support\Facades\Cache;

class CheckWebPage extends Command
{
    protected $signature = 'check:webpage';
    protected $description = 'Controlla se un testo è presente in una pagina web e notifica via Telegram';

    public function handle()
    {
        $url = env('MONITOR_URL');
        $testoDaCercare = env('MONITOR_TEXT');

        try {
            $response = Http::timeout(15)
                ->withHeaders([
                    'User-Agent' => 'Mozilla/5.0 (compatible; WebMonitorBot/1.0)',
                ])
                ->get($url);

            if (!$response->successful()) {
                Log::warning("CheckWebPage: risposta HTTP {$response->status()} da {$url}");
                return;
            }

            $contenuto = strtolower($response->body());
            $testo = strtolower($testoDaCercare);

            if (str_contains($contenuto, $testo)) {
                // Notifica solo se non l'ha già fatto nelle ultime 24 ore (anti-spam)
                if (!Cache::has('webpage_testo_trovato')) {
                    Cache::put('webpage_testo_trovato', true, now()->addHours(24));
                    $this->inviaNotifica("✅ Testo trovato!\n\nTesto: \"{$testoDaCercare}\"\nPagina: {$url}");
                    $this->info('Testo trovato! Notifica inviata.');
                } else {
                    $this->info('Testo trovato, ma notifica già inviata nelle ultime 24h.');
                }
            } else {
                // Reset cache se il testo non c'è più, così notifica di nuovo quando riappare
                Cache::forget('webpage_testo_trovato');
                $this->info('Testo non trovato.');
            }
        } catch (\Exception $e) {
            Log::error('CheckWebPage errore: ' . $e->getMessage());
            $this->inviaNotifica("⚠️ Errore nel monitoraggio:\n{$e->getMessage()}");
        }
    }

    private function inviaNotifica(string $messaggio): void
    {
        $token = env('TELEGRAM_BOT_TOKEN');
        $chatId = env('TELEGRAM_CHAT_ID');

        try {
            Http::post("https://api.telegram.org/bot{$token}/sendMessage", [
                'chat_id' => $chatId,
                'text' => $messaggio,
                'parse_mode' => 'HTML',
            ]);
        } catch (\Exception $e) {
            Log::error('Telegram invio fallito: ' . $e->getMessage());
        }
    }
}
```

### 2.4 Schedulare il Command

**Laravel 10 e precedenti** — in `app/Console/Kernel.php`:

```php
protected function schedule(Schedule $schedule)
{
    $schedule->command('check:webpage')->hourly();
}
```

**Laravel 11+** — in `routes/console.php`:

```php
use Illuminate\Support\Facades\Schedule;

Schedule::command('check:webpage')->hourly();
```

---

## 3. Configurare il Cron Job su Aruba

### 3.1 Accedi al pannello di controllo Aruba

1. Vai su **admin.aruba.it** o il pannello del tuo hosting
2. Cerca la sezione **Cron Job** o **Operazioni pianificate**

### 3.2 Aggiungi il Cron Job

Inserisci questa riga:

```
* * * * * cd /path/del/tuo/progetto-laravel && php artisan schedule:run >> /dev/null 2>&1
```

> **Nota**: sostituisci `/path/del/tuo/progetto-laravel` con il percorso reale della tua installazione Laravel su Aruba. Di solito è qualcosa come `/web/htdocs/www.tuosito.it/home/progetto`.

> **Nota**: su Aruba il percorso di PHP potrebbe essere diverso. Prova con `which php` via SSH oppure usa il percorso completo, ad esempio `/usr/local/bin/php` o `/usr/bin/php8.2`.

Il cron gira ogni minuto, ma Laravel esegue il command solo ogni ora come schedulato.

---

## 4. Test

### 4.1 Test manuale del command

```bash
php artisan check:webpage
```

Dovresti ricevere la notifica su Telegram se il testo è presente nella pagina.

### 4.2 Test della connessione Telegram

```bash
php artisan tinker
```

```php
Http::post("https://api.telegram.org/bot" . env('TELEGRAM_BOT_TOKEN') . "/sendMessage", [
    'chat_id' => env('TELEGRAM_CHAT_ID'),
    'text' => '🔔 Test notifica dal bot!',
]);
```

### 4.3 Verifica il cron job

Controlla i log di Laravel in `storage/logs/laravel.log` per verificare che il command venga eseguito.

---

## 5. Opzioni avanzate

### Frequenza diversa

Modifica la schedulazione secondo le tue esigenze:

| Metodo | Frequenza |
|--------|-----------|
| `->hourly()` | Ogni ora |
| `->everyThirtyMinutes()` | Ogni 30 minuti |
| `->everyFifteenMinutes()` | Ogni 15 minuti |
| `->twiceDaily(8, 20)` | Alle 8:00 e alle 20:00 |
| `->dailyAt('09:00')` | Ogni giorno alle 9:00 |

### Monitorare più pagine

Puoi creare un array di configurazioni nel `.env` o in un file config dedicato (`config/monitor.php`) per controllare più pagine con lo stesso command.

### Notifica quando il testo scompare

Aggiungi una notifica anche quando il testo non è più presente:

```php
if (!str_contains($contenuto, $testo) && Cache::has('webpage_testo_trovato')) {
    Cache::forget('webpage_testo_trovato');
    $this->inviaNotifica("❌ Il testo non è più presente su {$url}");
}
```

---

## Riepilogo file coinvolti

| File | Azione |
|------|--------|
| `.env` | Aggiungere token, chat_id, URL e testo |
| `app/Console/Commands/CheckWebPage.php` | Creare (command principale) |
| `app/Console/Kernel.php` o `routes/console.php` | Aggiungere schedulazione |
| Pannello Aruba → Cron Job | Configurare cron ogni minuto |

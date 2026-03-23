# Analisi Log Errori — 2026-03-20

**File:** `log_errori_sample.txt`
**Data:** 2026-03-20
**Totale errori:** 223

## FONTE
- Analisi basata su `log_errori_sample.txt`

---

## Distribuzione oraria

| Ora | Errori | % |
|-----|-------:|--:|
| 04:00 | 37 | 16.6% |
| 05:00 | 40 | 17.9% |
| 06:00 | 29 | 13.0% |
| 07:00 | 50 | 22.4% |
| 08:00 | 67 | 30.0% |
| **Totale** | **223** | **100%** |

> Il picco si concentra nelle fasce **07:xx** e **08:xx** (117 errori, 52.5% del totale).

---

## Occorrenze per tipo di errore

| # | Errore | Count | % | FIXED |
|---|--------|------:|--:|-------|
| 1 | `Process already in progress for serial {N}` | 121 | 54.3% |  |
| 2 | `Get Machine Configuration variable content should be 26 or 27 bytes instead of {N}` | 55 | 24.7% |  |
| 3 | `Aggregate with id '{UUID}' not found` | 33 | 14.8% |  |
| 4 | `Call to a member function getSupportUser() on null` | 5 | 2.2% |  |
| 5 | `Expected response code 250 but got code "451", with message "451 4.4.2 Timeout waiting for data from client. "` | 4 | 1.8% |  |
| 6 | `Invalid date format {values}` | 3 | 1.3% |  |
| 7 | `Command not implemented: '20'` | 1 | 0.4% |  |
| 8 | `invalid argument for replace: empty key` | 1 | 0.4% |  |

> I 3 errori più frequenti (`Process already in progress for serial {N}`, `Get Machine Configuration variable content should be 26 or 27 bytes instead of {N}`, `Aggregate with id '{UUID}' not found`) rappresentano il 93.7% del totale (209/223 errori).

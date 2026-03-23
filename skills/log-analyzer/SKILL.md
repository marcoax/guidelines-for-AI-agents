---
name: log-analyzer
description: "Analyze Symfony app.ERROR log files for the Carimali/caricare project. Use this skill whenever the user mentions a log file, log errors, log analysis, log report, or asks to clean/analyze/summarize a .txt log file. Triggers on: analizza il log, genera report dal log, pulisci il log, log_errori, app.ERROR, analisi errori, or any request to process a Symfony log file. Produces two files: a cleaned log (NAME_clean.txt) and a markdown report (NAME_report.md)."
---

# Log Analyzer Skill

Analyzes Symfony `app.ERROR` log files for the caricare project. Given an input log file, the script executes 7 steps:
1. **Cleans** the log by applying `CLEAN_RULES` (filters irrelevant blocks and lines)
2. **Extracts** errors from the cleaned log and normalizes messages (`{N}`, `{UUID}`, etc.)
3. **Archives** existing `log_report_*.md` files to `report/`
4. **Generates** a markdown report with hourly distribution and error-type counts
5. **Updates** `error_history.json` and regenerates `error_trend.html` (if errors found)
6. **Checks** for new error types against `assets/known_errors.txt`
7. **Moves** the original input file to `backup/`

## Output files

| File | Description |
|------|-------------|
| `BASENAME_clean.txt` | Cleaned log with irrelevant blocks/lines removed |
| `log_report_YYYY_MM_DD.md` | Markdown report with stats tables |
| `report/log_report_*.md` | Previous reports archived here (if any existed) |
| `error_history.json` | Cumulative daily error data (appended each run) |
| `error_trend.html` | Interactive dashboard with charts (generated only if log contains errors) |
| `backup/BASENAME.txt` | Original input file moved here after processing |

Example: input `log_errori_2026_03_20.txt` → `log_errori_2026_03_20_clean.txt` + `log_report_2026_03_20.md`, original moved to `backup/`

## How to run

### Automatic (preferred)

When the user provides a log file path, run the script directly:

```bash
python {SKILL_DIR}/scripts/analyze_log.py PATH/TO/log_errori_YYYY_MM_DD.txt
```

Both output files are written to the **same directory** as the input file.

### Post-analysis step

After running the script and presenting the report to the user, **always ask**:

> "Ci sono nuove regole di pulizia da aggiungere al log analyzer?"

If the user provides new rules, update **both** files (they must stay in sync):
1. Read `assets/CLEAN_RULES.md` and add the new rule following the existing numbered format (with Input/Output examples)
2. Update `scripts/analyze_log.py` to implement the new rule — add a detection helper + integrate it in `clean_block()`
3. Re-run the script on the log file from `backup/` (step 7 moves the original there):
   ```bash
   python {SKILL_DIR}/scripts/analyze_log.py backup/log_errori_YYYY_MM_DD.txt
   ```

### Regenerate HTML from existing data

If `error_history.json` has been updated outside the full analysis flow, regenerate just the HTML dashboard:

```bash
python {SKILL_DIR}/scripts/analyze_log.py --regenerate-html [path/to/error_history.json]
```

If the path is omitted, defaults to `error_history.json` in the current directory.

### Manual fallback

If the script cannot run, read `assets/CLEAN_RULES.md` for the cleaning rules and `assets/log_report_sample.md` for the expected report format, then apply them manually.

## Reference files

- `assets/CLEAN_RULES.md` — full cleaning rules with examples
- `assets/known_errors.txt` — list of known normalised error types (one per line)
- `assets/error_trend_template.html` — HTML template for the trend dashboard (data injected by script)
- `assets/log_report_sample.md` — example of the expected output report format
- `assets/log_errori_sample.txt` — example input log file
- `scripts/analyze_log.py` — automation script (Python 3.10+, stdlib only)

## New error detection

The script compares normalised errors found in the log against `assets/known_errors.txt`. If new error types appear that are not in the list, they are printed as warnings at the end of the run. When confirmed as legitimate recurring errors, add them to `known_errors.txt` (one per line, normalised form).

## Input file formats

The script supports two log formats:

1. **Block format** (raw grep output with `--` separators): full cleaning rules are applied per-block (rules 1-5)
2. **Flat format** (one `app.ERROR` line per row, no `--` separators): only line-level filtering is applied (drops `Troppi errori di processo` lines)

The format is auto-detected. Flat files are typically already-cleaned logs or logs extracted without context lines.

## Error trend dashboard

The `error_trend.html` file is a self-contained interactive dashboard with:
- Daily error volume chart, stacked breakdown by type, hourly distribution
- Summary table with day-over-day comparison
- Cache-busting meta tags (browser always loads fresh data)
- Generation timestamp visible in header and footer

The HTML is regenerated automatically at step 5, or standalone via `--regenerate-html`.

## Notes

- The script uses only Python stdlib — no pip install needed
- `FIXED` column in the report is left empty by default unless the user provides a known-issues list
- If the log covers multiple days, the report groups by day then by hour automatically
- When re-running after updating clean rules, use the file from `backup/` (step 7 moves the original there)
- If `error_history.json` is edited manually, run `--regenerate-html` to sync the dashboard

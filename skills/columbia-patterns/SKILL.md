---
name: columbia-patterns
description: Pattern operativi Columbia per lavorare su file dati grandi (sped.xlsx, cargo), batch di consol/clienti, ricerche email voluminose e sessioni lunghe. Usare prima di parsare un xlsx sopra le 1k righe, prima di lanciare un batch di sub-agenti, o quando il contesto della sessione si gonfia.
---

# Pattern Operativi Columbia (Workshop Anthropic 2026-01-05)

## SQLite-first per file dati grandi

- **sped.xlsx / cargo / qualsiasi xlsx > 1k righe** → primo step: converti in SQLite (`.db`) e fai query SQL native.
- Motivo: il modello sa SQL molto meglio di awk/pandas su filtri ad hoc. +30-50% accuratezza, -30% tempo su domande multi-filtro.
- Pattern: `python3 -c "import pandas as pd; pd.read_excel('file.xlsx').to_sql('t', sqlite3.connect('file.db'), index=False)"` poi `sqlite3 file.db "SELECT ..."`.

## Sub-agenti paralleli su batch

- `/consol-check` su 3+ consol → spawn 3 sub-agenti paralleli, main aggrega. Riduci tempo 60-70%.
- Lo stesso vale per `/spedizioni-cliente` su batch clienti, `/follow-up` su periodi multipli.
- NO sub-agente per task da <30s o che ritorna >2k token al main.

## Save-output-to-file

- Search email > 50 risultati → salva JSON in `/tmp/search-YYYYMMDD.json`, ritorna path. Poi grep mirato.
- Vale per: `/mail`, `/follow-up`, dump xlsx, scraping web.
- Regola generale: tool/sub-agent che ritorna >50 righe → scrivi su file, ritorna path + 5 righe di preview.

## /clear non /compact

- Su task lunghi: preferisci `/clear` + ricarica MEMORY.md vault + ultimo output HTML.
- Stato vero è nei file (vault Obsidian, sped.db, Genova/, Lacage/).
- `/compact` lascia rumore di conversazione.

## Configurazione consigliata

```bash
# Aggiungere al .bashrc o .zshrc per auto-compact su sessioni lunghe (safety net)
export CLAUDE_CODE_AUTO_COMPACT_WINDOW=400000
```

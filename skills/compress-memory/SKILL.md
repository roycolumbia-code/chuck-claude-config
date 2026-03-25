---
name: compress-memory
description: Comprime il contesto della sessione corrente in MEMORY.md del vault E scrive una nota sessione linkata in Obsidian per far crescere il grafo della conoscenza.
---

# Skill: /compress-memory

## Quando usare questa skill

Usala quando:
- La sessione e' lunga e il contesto si sta riempiendo
- Sono state prese decisioni importanti da ricordare
- Sono stati calcolati nuovi KPI o dati reali
- L'utente dice "salva il contesto" o "aggiorna la memoria"

## Procedura

### Step 1 — Raccogli dalla sessione corrente

Identifica nella conversazione attuale:
- Decisioni di business prese
- Nuovi dati/KPI calcolati (con valori esatti)
- Problemi risolti e come
- Todo emersi
- Clienti, servizi, progetti, corrispondenti nominati
- Output/file creati (path completo)

### Step 2 — Leggi MEMORY.md attuale
`~/chuck/IIB/MEMORY.md`

### Step 3 — Aggiorna MEMORY.md

Aggiorna le sezioni rilevanti:

**Sezione "Decisioni di Business Registrate"**: aggiungi con data
**Sezione "KPI Baseline"**: aggiorna valori se ora disponibili dati reali
**Sezione "Pattern Identificati"**: aggiungi nuovi pattern trovati
**Sezione "Todo Aperti"**: aggiungi nuovi, chiudi completati con [x]
**Sezione "Agenti Claude Attivi"**: aggiorna tabella se creati nuovi agenti

### Regole di compressione MEMORY.md

- NON duplicare info gia' in CLAUDE.md (contesto fisso)
- NON scrivere "durante questa sessione ho..." — scrivi solo il fatto
- USA formato tabella per dati numerici
- ELIMINA entry MEMORY.md con piu' di 90 giorni senza aggiornamenti
- MAX 150 righe totali MEMORY.md — se superi, archivia in 4-Archives/

### Step 4 — Scrivi nota sessione in Obsidian (SEMPRE)

Crea il file: `~/chuck/IIB/0-Inbox/Claude/YYYY-MM-DD/sessione-HHMM.md`
(usa data e ora reale della sessione)

**Formato della nota:**

```markdown
---
tipo: sessione-claude
data: YYYY-MM-DD
tags:
  - sessione
  - [tag pertinenti: analisi | vendite | operativo | finanza | sistema]
clienti: [lista clienti menzionati]
servizi: [lista servizi menzionati]
progetti: [lista progetti menzionati]
---

# Sessione YYYY-MM-DD — [titolo sintetico della sessione]

## Cosa abbiamo fatto
- [bullet point concreti, max 5]

## Decisioni prese
- [decisione] → impatto su [[Progetto o Area correlata]]

## KPI / Dati calcolati
| Metrica | Valore | Nota |
|---------|--------|------|
| ...     | ...    | ...  |

## Output prodotti
- [[nome-file]] in `path/`

## Todo
- [ ] [azione] → [[responsabile o contesto]]

## Entita' collegate
[[Cliente1]] · [[Servizio1]] · [[Progetto1]] · [[Corrispondente1]]
```

**Regole per i wikilink:**
- Usa `[[Nome Cliente]]` per ogni cliente nominato (es. `[[La Marzocco]]`, `[[Thibaut]]`)
- Usa `[[Export Aereo]]`, `[[Import Mare]]`, `[[FCL]]`, `[[LCL]]` per servizi
- Usa `[[Budget2026]]`, `[[LogisticaTessile]]` per progetti
- Usa `[[Axxess]]`, `[[Mid America]]` per corrispondenti
- Se la nota target non esiste ancora in Obsidian, crea il link ugualmente — Obsidian mostrera' le "unlinked mentions" e potrai creare la nota quando vuoi
- La sezione "Entita' collegate" finale e' il motore del grafo: lista TUTTI i [[link]] senza prosa

### Step 5 — Conferma

Di' all'utente:
> "MEMORY.md aggiornato + nota sessione creata in Obsidian: `0-Inbox/Claude/YYYY-MM-DD/sessione-HHMM.md`"
> "Link creati: [[X]], [[Y]], [[Z]]"
> "Apri il Graph View in Obsidian per vedere i nuovi collegamenti."

## Output

- `~/chuck/IIB/MEMORY.md` aggiornato (stato evolutivo flat)
- `~/chuck/IIB/0-Inbox/Claude/YYYY-MM-DD/sessione-HHMM.md` creato (nota linkata per il grafo)
- Conferma sintetica nel thread

---
name: bye
description: Routine di chiusura sessione Columbia Transport. Esegue compress-memory e aggiorna vault Obsidian. Estensibile aggiungendo nuovi Step numerati.
---

# Skill: /bye

## Quando usare questa skill

Quando Roy chiude la sessione di lavoro con Claude — fine giornata, fine meeting,
o quando dice "bye", "a domani", "chiudo", "ci vediamo".

## Esecuzione

Esegui i passi in ordine. Se un passo fallisce, segnalalo e vai avanti con il successivo.
Alla fine mostra un riepilogo di tutti i passi eseguiti.

---

## STEP 1 — Comprimi memoria (obbligatorio)

Esegui la procedura completa di `/compress-memory`:
- Aggiorna `~/chuck/IIB/MEMORY.md` con decisioni/KPI/todo della sessione
- Crea nota sessione in `~/chuck/IIB/0-Inbox/Claude/YYYY-MM-DD/sessione-HHMM.md` con wikilink

Se la sessione non ha prodotto nulla di significativo, aggiorna comunque la data
in MEMORY.md e scrivi una nota sessione minima.

---

## STEP 2+ — Aggiungi nuovi step qui

Quando Roy vuole aggiungere funzionalità a /bye, aggiungi un nuovo blocco:

```
## STEP N — [Nome step]

**Trigger**: [sempre / solo se X / solo il giorno Y]

[Procedura]
```

Idee future possibili:
- STEP 2: Leggi mail inviate oggi via MS365 MCP (`mcp__plugin_sales_ms365__*`) — token OAuth attivo, account `columb16@columbiatransport.it`
- STEP 3: Report settimanale (solo venerdì) — analisi mail + note sessioni settimana
- STEP 4: Aggiorna calendario Google con recap giornata
- STEP 5: Controlla todo aperti in MEMORY.md e segnala quelli scaduti
- STEP 6: Invia messaggio Telegram (`@columbia_intel_bot`, chat_id `911351710`) con summary giornata

---

## Output finale (sempre)

Dopo tutti gli step, mostra questo riepilogo:

```
/bye — [data] [ora]
===================
✅ Step 1 — Memory compressa + nota sessione Obsidian

A domani Roy 👋
```

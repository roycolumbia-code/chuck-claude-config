---
name: bye
description: Routine di chiusura sessione Columbia Transport. Esegue in sequenza: compress-memory, lettura mail inviate Outlook per aggiornare conoscenza, report settimanale se venerdi'. Estensibile aggiungendo nuovi Step numerati.
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

## STEP 2 — Leggi mail inviate oggi (knowledge update)

**Obiettivo**: capire cosa ha fatto Roy oggi attraverso le email che ha inviato,
e aggiornare la conoscenza del vault di conseguenza.

### 2a — Naviga alla cartella Posta Inviata di Outlook Web

> **Nota**: MS365 MCP non funziona (OAuth redirect URI dinamici non registrati in Azure).
> Usa **Playwright MCP** per navigare Outlook Web con la sessione Chrome già attiva.

1. Naviga alla Posta Inviata:
   ```
   mcp__plugin_playwright_playwright__browser_navigate
   url: "https://outlook.office.com/mail/sentitems"
   ```

2. Prendi uno snapshot per vedere l'elenco email:
   ```
   mcp__plugin_playwright_playwright__browser_snapshot
   ```
   Se la pagina chiede login → esegui login con:
   - URL login: `https://outlook.office.com`
   - Account: `roy@columbiatransport.it`
   Se la sessione è già attiva (probabile), la posta inviata carica direttamente.

3. Dall'accessibility tree, leggi le righe email visibili: oggetto, destinatario, ora.
   Filtra solo quelle di oggi (data corrente).

4. Per ogni email rilevante, aprila con un click (usa il ref del nodo) e prendi uno
   snapshot per leggere il corpo completo, poi torna indietro (`browser_navigate_back`).

### 2b — Analizza le mail

Per ogni mail inviata, estrai:
- **Destinatario** → cliente, corrispondente, team, fornitore?
- **Argomento** → quotazione, booking, problema, follow-up, proposta?
- **Entità nominate** → nomi clienti, spedizioni, rotte, servizi
- **Decisione o azione** → c'è qualcosa da ricordare?

### 2c — Aggiorna vault se ci sono info rilevanti

Se emergono informazioni non già note:

**Per ogni cliente nominato**: aggiungi una riga in `2-Areas/Clienti/[Cliente].md`
nella sezione "Note operative" con data e sintesi (max 1 riga).

**Per ogni decisione business**: aggiungila in MEMORY.md sezione
"Decisioni di Business Registrate" se non già presente dal Step 1.

**Per ogni todo nuovo**: aggiungilo in MEMORY.md sezione "Todo Aperti".

### 2d — Presenta sintesi mail

Mostra a Roy:

```
MAIL INVIATE OGGI — [data]
==========================

| # | Ora | A | Oggetto | Tipo | Note |
|---|-----|---|---------|------|------|
| 1 | ... | ... | ... | quotazione/booking/follow-up/altro | ... |

Totale: N mail inviate
Entità aggiornate nel vault: [lista]
```

Se non ci sono mail inviate oggi: scrivi "Nessuna mail inviata oggi su Outlook."
e vai al passo successivo.

---

## STEP 3 — Report settimanale (solo venerdì)

**Esegui questo step SOLO se oggi è venerdì.**
Per verificare: controlla la data corrente con `date +%A` o ragiona sulla data nel contesto.

### 3a — Raccogli dati settimana

Naviga alla Posta Inviata su Outlook Web (vedi Step 2a) e scorri le email
dell'intera settimana lavorativa (lunedì → venerdì). Usa `browser_scroll` per
caricare più email se necessario. Filtra per data (da lunedì mattina a oggi).

Leggi anche le note sessione della settimana in `~/chuck/IIB/0-Inbox/Claude/`:
- Cartelle dalla data del lunedì a oggi

### 3b — Genera report settimanale

Crea il file `~/chuck/IIB/0-Inbox/Claude/YYYY-MM-DD/report-settimana-WNN.md`:

```markdown
---
tipo: report-settimanale
settimana: WNN
data_inizio: YYYY-MM-DD
data_fine: YYYY-MM-DD
tags:
  - report
  - settimanale
---

# Report Settimana WNN — [data inizio] / [data fine]

## In sintesi
[3-4 bullet point: cosa è successo di importante questa settimana]

## Attività per area

### Clienti e commerciale
- [lista attività con clienti]

### Operativo
- [spedizioni, booking, problemi risolti]

### Sviluppo business
- [nuovi prospect, follow-up, trattative]

### Interno / Sistema
- [tool, processi, Claude, decisioni operative]

## Mail inviate: N
[tabella top 5 mail più significative]

## KPI settimana
[se disponibili da sessioni: n. spedizioni trattate, n. quotazioni, ecc.]

## Open items → settimana prossima
- [ ] [azioni aperte da chiudere]

## Entita' collegate
[[clienti nominati]] · [[servizi]] · [[progetti]]
```

### 3c — Comunica a Roy

> "Report settimana WNN creato: `0-Inbox/Claude/YYYY-MM-DD/report-settimana-WNN.md`"

---

## STEP 4+ — Aggiungi nuovi step qui

Quando Roy vuole aggiungere funzionalità a /bye, aggiungi un nuovo blocco:

```
## STEP N — [Nome step]

**Trigger**: [sempre / solo se X / solo il giorno Y]

[Procedura]
```

Idee future possibili:
- STEP 4: Aggiorna calendario Google con recap giornata
- STEP 5: Controlla todo aperti in MEMORY.md e segnala quelli scaduti
- STEP 6: Invia DM su Slack con summary giornata
- STEP 7: Leggi news logistica del giorno (opencli)

---

## Output finale (sempre)

Dopo tutti gli step, mostra questo riepilogo:

```
/bye — [data] [ora]
===================
✅ Step 1 — Memory compressa + nota sessione Obsidian
✅ Step 2 — N mail lette, M entità aggiornate nel vault
✅/⏭️ Step 3 — Report settimanale [creato / non è venerdì]

A domani Roy 👋
```

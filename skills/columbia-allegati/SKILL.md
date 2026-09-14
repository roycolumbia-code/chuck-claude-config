---
name: columbia-allegati
description: Allegare file a una bozza Superhuman/Outlook via Microsoft Graph. Usare per mandare PDF/xlsx con una bozza (CPSC, quotazioni, report) — e quando stai per dire che "gli allegati non si possono fare": si può.
---

# Allegati alle bozze email — SI PUÒ FARE

**Mai dire a Roy che "con Superhuman non si possono mettere allegati".** È falso, ed è un errore che si è già ripetuto più volte.

È vero che i tool MCP (`create_or_update_draft` di Superhuman, `outlook_create_draft` di ms365) non espongono un parametro allegati. Ma la bozza creata via Superhuman **atterra nella mailbox Outlook**, quindi si allega via **Microsoft Graph**.

## Procedura (verificata end-to-end il 2026-07-09)

```bash
# 1. crea la bozza col tool MCP create_or_update_draft (senza allegati)
# 2. copia i file sul mini (l'auth Graph vive solo lì)
scp <file>... mac-mini-di-roy:/tmp/
# 3. appendi: <dest-email> <prefisso-oggetto> <file>...
ssh mac-mini-di-roy '/opt/anaconda3/bin/python3 ~/chuck/CPSC/attach_draft_graph.py \
    stefania@antonellifirenze.com "CPSC 16 CFR 1610" /tmp/guida.pdf /tmp/dati.xlsx'
```

Flusso: `create_or_update_draft` (MCP) → `POST /me/messages/{id}/attachments` (Graph, fileAttachment + contentBytes base64).
Lo script si ferma da solo se la bozza ha già allegati reali, e **non invia mai**.

## Vincoli da ricordare

- **Auth Graph solo sul mini**: `m365_auth.py` sta in `~/chuck/mail-triage/`, NON sincronizzato via iCloud. Gira con `/opt/anaconda3/bin/python3` — il `python3` di sistema è il 3.9 e non ha `msal`.
- **Modificare il body di una bozza che ha già allegati**: `PATCH /me/messages/{id}` sul solo campo `body`. Ricrearla via MCP li cancella.
- **Contare gli allegati escludendo `isInline=True`**: il logo della firma è un attachment inline e falsa i conteggi.
- Limite `contentBytes` inline: ~3 MB per file.
- **L'allegato non compare nella bozza Superhuman, e da lì non parte** (vedi sotto).

Riferimento originale: `~/chuck/vanessa-routine/watch.py` → `attach_to_draft()` (sul mini).

## L'invio va fatto via Graph, non dalla UI Superhuman (2026-09-14)

Superhuman tiene una **copia propria** della bozza. L'allegato agganciato via Graph vive solo sulla copia Outlook: `get_draft` continua a mostrare le sole immagini inline della firma, e **inviando da Superhuman parte la sua copia, senza allegato e senza errori**. Così è partita vuota la risposta a Selecover delle 12:04.

**Regola: la bozza con allegato nasce su Graph, non in Superhuman.** Il verso Graph → Superhuman funziona (verificato 2026-09-14: bozza Graph con allegato vista in Superhuman, inviata da lì, file arrivato intatto). Rotto è solo il verso opposto.

```python
# 1. bozza su Graph: POST /me/messages, oppure createReply/createForward per restare nel thread
# 2. POST /me/messages/{id}/attachments   (conta solo isInline:false)
# 3. Roy la rivede in Superhuman e la invia lui — l'allegato regge.
#    Se invia Claude, dopo l'ok di Roy:
requests.post(f"{G}/me/messages/{mid}/send", headers=h)   # 202
```
Se la bozza era nata in Superhuman, dopo l'invio via Graph fai `discard_draft` sulla copia Superhuman (altrimenti resta un doppione senza allegato) e verifica in `sentitems`.

Le automazioni che nascono già su Graph — contratto MSC a Francesca Pucci, `~/chuck/MSC/msc_flow.py` — sono a posto così: Roy può inviarle dalla sua UI.

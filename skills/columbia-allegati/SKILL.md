---
name: columbia-allegati
description: Procedura per allegare file a una bozza email Superhuman/Outlook via Microsoft Graph. Usare quando serve mandare un PDF/xlsx insieme a una bozza (CPSC, quotazioni, report), o quando stai per dire che "gli allegati non si possono fare" — si possono.
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
- L'allegato appare in Superhuman con **lag di sync**: non concludere che è fallito se la UI non lo mostra subito.

Riferimento originale: `~/chuck/vanessa-routine/watch.py` → `attach_to_draft()` (sul mini).

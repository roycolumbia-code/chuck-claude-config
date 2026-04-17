Check veloce sullo stato di una consol. Cerca su Outlook le email operative e ricostruisci un riepilogo sintetico.

## Input
$ARGUMENTS = numero della consol (es. "11", "12")

## Istruzioni

1. Cerca su Outlook le email con query: "Consol #$ARGUMENTS" e in alternativa "consol $ARGUMENTS"
   - Cerca email da/verso axxessintl.com, team Columbia, simona@
   - Focus su: loading plan, cargo list, update, week
   - Leggi almeno le 5 email più recenti per avere lo stato aggiornato

2. Per ogni shipper trovato nelle email, estrai:
   - Nome azienda (shipper)
   - Consegnatario (se indicato)
   - Merce / colli / CBM (se indicati)
   - Stato: confermato, in attesa, rimosso, spostato

3. Presenta una tabella sintetica nel messaggio:

```
## Consol #$ARGUMENTS — Stato rapido (al [data ultima email])

| Shipper | Consignee | Merce | CBM | Stato |
|---------|-----------|-------|-----|-------|
| ...     | ...       | ...   | ... | ...   |

Totale: X shipper confermati, Y CBM stimati
```

4. Se ci sono problemi aperti o azioni pendenti, elencali in fondo.

**Nota**: Questo è un check rapido. Per il cargo list completo con file HTML, usa la skill `consol`.

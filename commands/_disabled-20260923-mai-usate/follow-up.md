Cerca email inviate da Roy senza risposta e prepara follow-up.

## Input
$ARGUMENTS = (opzionale) filtro per argomento o destinatario. Se vuoto, cerca tutto.

## Istruzioni

1. **Cerca su Outlook** le email inviate da roy@columbiatransport.it negli ultimi 7 giorni:
   - Folder: "Sent Items"
   - afterDateTime: 7 giorni fa
   - Se $ARGUMENTS non è vuoto, aggiungi come filtro query

2. **Per ogni email inviata trovata**, controlla se esiste una risposta:
   - Cerca nella Inbox email con lo stesso subject (o "Re: subject")
   - Dal destinatario originale
   - Con data successiva all'invio

3. **Filtra**: mantieni solo le email senza risposta ricevuta

4. **Presenta la lista**:

```
## Email senza risposta (ultimi 7 giorni)

| Data invio | A | Subject | Giorni fa |
|------------|---|---------|-----------|
| ...        | . | ...     | X gg      |

Totale: X email senza risposta
```

5. **Per ciascuna email**, proponi un testo di follow-up breve e cortese in italiano o inglese (scegli la lingua dell'email originale). Tono: professionale ma amichevole, non pressante. Esempio:

> "Buongiorno [Nome], volevo solo assicurarmi che avesse ricevuto la mia email del [data] riguardo [topic]. Resto a disposizione per qualsiasi chiarimento."

6. **Chiedi**: "Vuoi che prepari una bozza email per uno di questi follow-up?"

Review del codice e suggerimenti per semplificarlo.

## Input
$ARGUMENTS = path del file o cartella da analizzare

## Istruzioni

1. **Leggi il file/cartella** specificato in $ARGUMENTS
   - Se è un file singolo, analizzalo direttamente
   - Se è una cartella, elenca i file e chiedi quale analizzare (o analizza tutti se sono pochi)

2. **Analisi in 4 passaggi**:

### A. Complessità
- Funzioni troppo lunghe (>50 righe)
- Nesting eccessivo (>3 livelli)
- Parametri troppo numerosi (>4)
- Duplicazioni di codice

### B. Leggibilità
- Nomi poco chiari per variabili/funzioni
- Commenti mancanti dove servono
- Commenti ridondanti dove non servono
- Struttura del file (import, costanti, funzioni, export)

### C. Pattern
- Code smell riconosciuti
- Opportunità di estrarre funzioni/moduli
- Guard clause invece di else annidati
- Early return per ridurre indentazione

### D. Modernizzazione
- Syntax obsoleta sostituibile
- API deprecate
- Best practice del linguaggio specifico

3. **Output**:

```
## Review: [filename]

### Punteggio: [X/10]

### Problemi trovati
1. [Problema] — Linea X — Severità: alta/media/bassa
   Suggerimento: [come risolvere]

### Refactoring suggeriti
1. [Cosa cambiare] — [Perché] — [Stima effort: facile/medio]

### Punti positivi
- [Cosa è fatto bene]
```

4. **Chiedi**: "Vuoi che applichi uno di questi refactoring?"

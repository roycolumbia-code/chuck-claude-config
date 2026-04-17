Analizza le spedizioni di un cliente specifico dal file sped.xlsx.

## Input
$ARGUMENTS = nome del cliente (es. "Thibaut", "La Marzocco", "Richloom")

## Istruzioni

1. Apri il file `/Users/royrigamonti/Chuck/Lacage/sped.xlsx` (Sheet1, ~4163 righe, anno 2025)

2. Filtra per il cliente "$ARGUMENTS" cercando nelle colonne:
   - Colonna 9 (Committente) — match parziale, case-insensitive
   - Se zero risultati, prova anche colonna 41 (Mittente) e 45 (Destinatario)

3. Calcola e presenta:

```
## Spedizioni "$ARGUMENTS" — Anno 2025

### Riepilogo
- Totale spedizioni: X
- Ricavi totali: €X
- Costi totali: €X  
- MOL totale: €X (X%)

### Breakdown per servizio
| Servizio | N. sped | Ricavi | Costi | MOL | MOL% |
|----------|---------|--------|-------|-----|------|
| Export Aereo | ... | ... | ... | ... | ... |

### Breakdown per corrispondente
| Corrispondente | N. sped | MOL |
|----------------|---------|-----|
| Mid America    | ...     | ... |

### Trend mensile
| Mese | N. sped | Ricavi | MOL |
|------|---------|--------|-----|
```

4. Evidenzia anomalie: spedizioni con MOL negativo, mesi senza attività, variazioni significative.

**Mappa colonne**: 1=ID sped, 3=Tipo Trasporto, 4=Settore, 5=Data, 7=Servizio, 9=Committente, 18=Ricavi, 19=Costi, 20=Saldo, 21=Saldo%, 38=Corrispondente, 41=Mittente, 45=Destinatario

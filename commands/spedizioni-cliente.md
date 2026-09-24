Analizza le spedizioni di un cliente specifico da sped.db (DB unico sul Mac mini).

## Input
$ARGUMENTS = nome del cliente (es. "Thibaut", "La Marzocco", "Richloom")

## Istruzioni

1. Fonte dati: `~/chuck/Sped/sped.db` sul Mac mini, tabella `spedizioni`, anni 2025+2026.
   Interroga via SSH (solo lettura):

```bash
ssh mac-mini-di-roy 'sqlite3 -header -column ~/chuck/Sped/sped.db "
SELECT strftime(''%Y'', data_sped) AS anno,
       COUNT(*) AS n_sped,
       ROUND(SUM(ricavi_sped_e_raggruppate_dp),0) AS ricavi,
       ROUND(SUM(costi_sped_e_raggruppate_dp),0)  AS costi,
       ROUND(SUM(saldo_totale_sped),0)            AS mol,
       ROUND(SUM(saldo_totale_sped)*100.0/SUM(ricavi_sped_e_raggruppate_dp),1) AS mol_pct
FROM spedizioni
WHERE committente LIKE ''%$ARGUMENTS%''
GROUP BY anno"'
```

2. Match parziale case-insensitive su `committente`. Se zero risultati, prova anche `mittente` e `destinatario`.

3. Regole dati:
   - Anno/mese SEMPRE da `data_sped` (`anno_competenza` è corrotto, NON usarlo)
   - MOL = `saldo_totale_sped` (previsionale). NON `saldo_consuntivo`.

4. Presenta:

```
## Spedizioni "$ARGUMENTS" — 2025 + 2026 YTD

### Riepilogo (per anno)
- Totale spedizioni: X
- Ricavi totali: €X
- Costi totali: €X
- MOL totale: €X (X%)

### Breakdown per servizio
| Servizio | N. sped | Ricavi | Costi | MOL | MOL% |
|----------|---------|--------|-------|-----|------|
(GROUP BY servizio; per mix aereo/mare usa tipo_trasporto)

### Breakdown per corrispondente
| Corrispondente | N. sped | MOL |
|----------------|---------|-----|
(GROUP BY corrispondente)

### Trend mensile
| Mese | N. sped | Ricavi | MOL |
|------|---------|--------|-----|
(GROUP BY strftime('%Y-%m', data_sped))
```

5. Evidenzia anomalie: spedizioni con MOL negativo, mesi senza attività, variazioni significative.

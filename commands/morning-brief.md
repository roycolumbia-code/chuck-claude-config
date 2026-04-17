Briefing mattutino per Roy — email, calendar, azioni pendenti.

## Istruzioni

Esegui queste ricerche in parallelo, poi presenta un briefing sintetico.

### 1. Email importanti non lette (Outlook)
- Cerca email non lette (is:unread) su Outlook
- Limit: 15
- Classifica per priorità:
  - 🔴 URGENTE: clienti importanti, problemi operativi, scadenze oggi
  - 🟡 IMPORTANTE: richieste quotazioni, conferme booking, team interno
  - ⚪ NORMALE: newsletter, info generali

### 2. Calendar di oggi (Google Calendar)
- Lista eventi di oggi dal Google Calendar
- Per ogni evento: ora, titolo, partecipanti
- Evidenzia se ci sono conflitti o gap

### 3. Azioni pendenti
- Cerca su Outlook email con flag o contrassegno
- Controlla se ci sono thread aperti che richiedono azione di Roy

### Formato output

```
# ☀️ Buongiorno Roy — [giorno, data]

## 📧 Email da gestire
🔴 [N] urgenti
- [Subject] — da [sender] — [1 riga di contesto]

🟡 [N] importanti  
- [Subject] — da [sender] — [1 riga di contesto]

## 📅 Agenda oggi
- [HH:MM] [Evento] con [partecipanti]
- [HH:MM] [Evento] con [partecipanti]

## ⚡ Azioni suggerite
1. Rispondi a [X] su [topic] — urgente
2. Conferma [Y] per [deadline]
3. Follow-up su [Z] — nessuna risposta da 3 giorni
```

Sii conciso. Roy vuole un quadro in 30 secondi, non un romanzo.

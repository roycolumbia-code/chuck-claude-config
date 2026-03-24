# Skill: Salva Link

Questa skill gestisce una raccolta personale di link interessanti trovati su social media, YouTube, blog, ecc. I link vengono salvati in un file markdown centralizzato per essere consultati e valutati in seguito.

## File di riferimento

Il file dei link salvati si trova in: `/Users/royrigamonti/Chuck/links-da-provare.md`

Se il file non esiste ancora, crealo con questo header:

```markdown
# 📌 Link da provare

> Raccolta di link interessanti da esplorare.

---
```

## Formato di ogni entry

Ogni link salvato deve seguire questo formato esatto:

```markdown
### [Titolo o descrizione breve]

- **Data:** YYYY-MM-DD
- **URL:** <url-completo>
- **Nota:** [nota dell'utente o "—"]
- **Tag:** [tag separati da virgola, o "—"]
- **Stato:** 🟡 da provare

---
```

Gli stati possibili sono:
- `🟡 da provare` — link appena salvato, non ancora esplorato
- `✅ provato` — link esplorato e utile
- `❌ scartato` — link esplorato e non interessante

## Comportamento della skill

### 1. Salvare un nuovo link

Quando l'utente condivide un URL (o dice "salva questo link", "/salva-link", ecc.):

1. **Accetta il link** dall'utente
2. **Chiedi opzionalmente** una nota breve e dei tag. Se l'utente non li fornisce, usa "—"
3. **Tenta di estrarre info dal link**: usa `WebFetch` per recuperare titolo e descrizione dalla pagina. Se non riesci, usa l'URL come titolo
4. **Aggiungi l'entry** in fondo al file `links-da-provare.md` (prima di un eventuale sezione "Archivio")
5. **Conferma** il salvataggio all'utente con un riepilogo

Esempio di interazione:
```
Utente: salva questo link https://youtube.com/watch?v=abc123
Claude: Ho salvato il link! Vuoi aggiungere una nota o dei tag? (rispondi o scrivi "no" per saltare)
Utente: parla di automazioni con Claude, tag: ai, automazione
Claude: ✅ Salvato! [titolo video] — nota e tag aggiunti.
```

### 2. Consultare i link salvati

Quando l'utente chiede "mostrami i link", "cosa devo provare?", "link salvati", ecc.:

1. Leggi il file `links-da-provare.md`
2. Filtra in base alla richiesta:
   - **"da provare"** → mostra solo quelli con stato 🟡
   - **"provati"** → mostra solo quelli con stato ✅
   - **per tag** → filtra per tag specifico
   - **tutti** → mostra tutto
3. Presenta i risultati in modo chiaro e compatto

### 3. Aggiornare lo stato di un link

Quando l'utente dice "segna come provato", "scarta il link X", ecc.:

1. Identifica il link (per titolo, URL parziale, o posizione)
2. Aggiorna lo stato nel file (`🟡 da provare` → `✅ provato` oppure `❌ scartato`)
3. Conferma la modifica

### 4. Processare link da email

Quando l'utente dice "controlla le email per link da salvare" o simili:

1. Cerca in Gmail email con oggetto "📌 Link da provare"
2. Estrai i link e le note dal corpo dell'email
3. Salvali nel file seguendo il formato standard
4. Conferma quanti link sono stati importati

Questo è utile per processare i link inviati tramite lo Shortcut iOS.

## Note importanti

- Il file `links-da-provare.md` è la **singola fonte di verità** per tutti i link
- Non duplicare link già presenti (controlla l'URL prima di aggiungere)
- Mantieni il formato consistente per facilitare il parsing
- Quando estrai info con WebFetch, non copiare contenuti protetti da copyright — usa solo titolo e una brevissima descrizione tua

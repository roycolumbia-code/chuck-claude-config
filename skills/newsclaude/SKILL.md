# Skill: /newsclaude

Esegui una ricognizione delle novità più rilevanti nell'ecosistema Claude Code: nuovi plugin, skill, tool e funzionalità discusse dalla community. Filtra e presenta solo ciò che è rilevante per Roy (vedi profilo interessi sotto).

**Inoltre**, quando Roy inoltra un contenuto specifico (articolo, video, thread X, post LinkedIn, newsletter, email con tips), analizzalo a 360° e proponi miglioramenti concreti al suo setup Claude Code.

---

## Profilo interessi Roy

Roy usa Claude Code per un mix ampio di attività:

| Area | Esempi |
|------|--------|
| **Spedizioni/Logistica** | Analisi MOL, quotazioni, distinte carico, BeOne |
| **Comunicazione** | Email clienti, LinkedIn post, pitch, brief |
| **Design / Visual** | Siti web, infografiche, tool HTML, container loader |
| **Immagini / Video** | Generazione immagini AI, editing, content creation |
| **Automazione** | Hook Claude Code, skill, agenti, scheduling |
| **Ricerca / Intelligence** | Notizie settore, prospect, analisi competitor |
| **Produttività** | Calendar, mail, Obsidian vault, Notion |

**Contesto professionale:** Roy è Managing Director di Columbia Transport, uno spedizioniere internazionale. Ogni suggerimento va valutato in ottica di applicabilità al suo lavoro quotidiano e al suo setup Claude Code.

Sono **escluse** le novità puramente tecniche/developer (es. testing framework, linter, CI/CD pipeline) a meno che non abbiano applicazione diretta al lavoro di Roy.

---

## MODALITÀ 1 — Ricognizione novità (quando Roy digita `/newsclaude` senza contenuto)

Quando Roy digita `/newsclaude`:

### Step 1 — Ricerca parallela (4 fonti)

Esegui in parallelo le seguenti ricerche con `Bash` usando `opencli`:

**Fonte A — Anthropic Ufficiale (PRIORITÀ MASSIMA — sempre inclusa):**
Usa `WebFetch` o `curl` per leggere il changelog ufficiale e le release notes:
```bash
# Changelog Claude Code (aggiornamenti, nuove feature, modelli)
curl -s "https://raw.githubusercontent.com/anthropics/claude-code/refs/heads/main/CHANGELOG.md" | head -100
# Oppure tramite GitHub API
curl -s "https://api.github.com/repos/anthropics/claude-code/releases?per_page=5"
```
Cerca anche con WebFetch: `https://docs.anthropic.com/en/release-notes/claude-code`
Questi risultati vanno in cima all'output con sezione dedicata **"📣 Aggiornamenti Ufficiali Anthropic"**.

**Fonte B — Google (plugin/skill nuovi):**
```bash
opencli google search "claude code new plugin skill site:github.com"
opencli google search "claude code plugin marketplace new"
```

**Fonte C — Hacker News (discussioni e annunci):**
```bash
opencli hackernews search "claude code"
opencli hackernews search "claude code plugin"
```
Per ogni risultato HN interessante, recupera l'URL originale via HN Algolia API:
```bash
curl -s "https://hn.algolia.com/api/v1/search?query=<titolo>&tags=story" | python3 -c "import json,sys; d=json.load(sys.stdin); [print(h.get('url','no-url'), '|', h.get('title','')) for h in d.get('hits',[])]"
```

**Fonte D — GitHub diretto (repo ad alto engagement recenti):**
Usa l'API GitHub per verificare stella count e descrizione:
```bash
curl -s "https://api.github.com/repos/<owner>/<repo>" | python3 -c "import json,sys; d=json.load(sys.stdin); print('Stars:', d.get('stargazers_count'), '| Desc:', d.get('description'))"
```
URL di riferimento:
- `https://github.com/topics/claude-code-plugin` (via WebFetch)
- `https://github.com/jeremylongshore/claude-code-plugins-plus-skills`

### Step 2 — Analisi e filtraggio

Per ogni risultato trovato:
1. **Escludi** elementi irrilevanti (testing puro, framework backend, linguaggi specifici senza uso pratico per Roy)
2. **Valuta la rilevanza** rispetto al profilo interessi (tabella sopra)
3. **Controlla se è già installato** leggendo i plugin attivi da `~/.claude/settings.json` (campo `enabledPlugins`)
4. **Assegna una priorità**: 🔴 Alta / 🟡 Media / ⚪ Bassa

### Step 3 — Output strutturato

Presenta i risultati in questa forma:

```
## 🆕 Novità Claude Code — [data]

### 📣 Aggiornamenti Ufficiali Anthropic
(sempre presente, anche se vuoto — indicare "nessuna novità da ultimo check")
- **v[X.Y.Z]** — [data] | [sintesi cambiamento in 1 riga]
...

### Plugin / Skill da valutare

| # | Nome | Repo | Cosa fa | Area | ⭐ Stars | Priorità | Installato? |
|---|------|------|---------|------|---------|----------|-------------|
| 1 | ... | github.com/... | ... | ... | N | 🔴 Alta | No |
...

### Discussioni interessanti (HN / community)
- **[Titolo]** — Score: N | [breve sintesi 1 riga] | [link repo se trovato]
...

### Come installare un plugin
Per installare: `/plugin install <nome>@<marketplace>` oppure `marketplace add <owner>/<repo>`
Per valutare prima: chiedi a Roy "vuoi che lo installi?"
```

### Step 4 — Proposta azione

Dopo aver mostrato i risultati:
1. Chiedi: *"Vuoi che ne installo qualcuno? Scrivi il numero o il nome."*
2. Se Roy risponde con un numero → installa con `/plugin install` o aggiungi a `settings.json`
3. Se Roy risponde "tutti quelli ad alta priorità" → installa tutti i 🔴
4. Se Roy risponde "dopo" → salva la lista in `~/chuck/IIB/0-Inbox/Claude/newsclaude-[data].md`

---

## MODALITÀ 2 — Analisi contenuto inoltrato (quando Roy inoltra un articolo, video, thread, post, email, ecc.)

Quando Roy usa `/newsclaude` accompagnato da un contenuto (link, testo copiato, screenshot, trascrizione video, thread X, post LinkedIn, newsletter, email con tips), la skill entra in modalità **analisi a 360°**.

### Step 1 — Acquisizione e comprensione del contenuto

1. **Identifica il tipo di contenuto:**
   - 🔧 **Tool/Plugin/MCP** → il contenuto descrive un tool installabile → vai a Modalità 1 Step 2 (filtraggio e installazione)
   - 📝 **Articolo/Tips/Best practice** → il contenuto contiene consigli, workflow, configurazioni → prosegui con Step 2 qui sotto
   - 🎥 **Video/Trascrizione** → estrai i punti chiave dalla trascrizione → prosegui con Step 2
   - 🧵 **Thread/Discussione** → sintetizza i contributi rilevanti → prosegui con Step 2
   - 🔀 **Misto** → separa le parti e gestisci ciascuna con la modalità appropriata

2. **Leggi/scarica il contenuto:**
   - Se è un URL → usa `WebFetch` per recuperare il testo
   - Se è un testo incollato → analizzalo direttamente
   - Se è uno screenshot → leggilo e trascrivi i punti chiave
   - Se è una trascrizione video → estrailo in bullet point

3. **Crea un riassunto strutturato** del contenuto in max 5-8 bullet point.

### Step 2 — Estrazione suggerimenti applicabili

Per ogni suggerimento/tip/best practice trovato nel contenuto:

1. **Valuta l'applicabilità** al workflow di Roy (Managing Director, spedizioniere, utente Claude Code avanzato)
2. **Classifica** ogni suggerimento in una di queste categorie:

| Categoria | Cosa comporta | Esempio |
|-----------|--------------|---------|
| 🆕 **Nuovo slash command** | Creare un file `.md` in `.claude/commands/` | `/follow-up` per tracciare follow-up clienti |
| 🛠️ **Nuova skill** | Creare una cartella in `.claude/skills/` con `SKILL.md` | Skill per analisi automatica RFQ |
| 📋 **Aggiornamento CLAUDE.md** | Aggiungere regole/contesto al file `CLAUDE.md` del progetto | Nuova sezione "Tone of voice per email" |
| ⚙️ **Configurazione** | Modificare `settings.json`, `.claude/settings.local.json`, hooks, MCP | Aggiungere un MCP server, configurare un hook |
| 🔄 **Workflow improvement** | Cambiare un processo esistente | Ristrutturare il morning brief con nuovi dati |
| 🧠 **Prompt engineering trick** | Tecnica di prompting da incorporare nelle skill esistenti | Usare chain-of-thought in una skill di analisi |
| 📦 **Plugin/MCP da installare** | Installare un plugin o MCP nuovo | `plugin install X@marketplace` |
| 🤖 **Automazione** | Creare un task schedulato, hook, o agente | Scheduled task per digest settimanale |

3. **Filtra**: scarta suggerimenti non applicabili, troppo generici, o già coperti dal setup attuale.

### Step 3 — Presentazione proposte

Presenta le proposte a Roy in questa forma:

```
## 🔍 Analisi contenuto — [titolo/fonte breve]

### Riassunto
[5-8 bullet point con i punti chiave del contenuto]

### 💡 Proposte di miglioramento per il tuo setup

| # | Tipo | Proposta | Impatto | Complessità |
|---|------|----------|---------|-------------|
| 1 | 🆕 Comando | Creare `/nome-comando` per... | Alto | Bassa |
| 2 | 📋 CLAUDE.md | Aggiungere sezione "..." | Medio | Bassa |
| 3 | 🛠️ Skill | Creare skill "..." per... | Alto | Media |
| 4 | ⚙️ Config | Configurare... | Medio | Bassa |
...

### Dettaglio per ogni proposta

**Proposta 1 — [nome]**
- **Cosa**: [descrizione concreta di cosa verrà creato/modificato]
- **Dove**: [path del file da creare/modificare]
- **Perché**: [beneficio concreto per Roy]
- **Preview**: [snippet del contenuto che verrà creato, se applicabile]

[ripeti per ogni proposta]
```

### Step 4 — Implementazione dopo conferma

Dopo la presentazione:
1. Chiedi: *"Quali proposte vuoi che implemento? Scrivi i numeri, 'tutte', o 'nessuna'."*
2. Se Roy conferma uno o più numeri:
   - **Per nuovi slash command**: crea il file `.md` in `.claude/commands/` con il contenuto appropriato
   - **Per nuove skill**: crea la cartella in `.claude/skills/<nome>/` e il file `SKILL.md`
   - **Per aggiornamenti CLAUDE.md**: leggi il file attuale, inserisci la nuova sezione nel punto giusto, salva
   - **Per configurazioni**: modifica il file di config appropriato
   - **Per workflow improvement**: modifica la skill/comando esistente
   - **Per plugin/MCP**: installa con il comando appropriato
   - **Per automazioni**: crea il task schedulato o hook
   - **Per prompt tricks**: aggiorna la skill esistente con la nuova tecnica
3. Dopo l'implementazione, conferma cosa è stato fatto con path e descrizione.
4. Se Roy dice "dopo" → salva le proposte in `~/chuck/IIB/0-Inbox/Claude/newsclaude-proposte-[data].md`

---

## MODALITÀ 3 — Valutazione rapida (quando Roy chiede "che ne pensi di questo?" su un tool/contenuto)

Per richieste rapide di valutazione:

1. **Analizza** il contenuto/tool in max 30 secondi
2. **Rispondi** con un verdetto sintetico:
   - ✅ **Utile** — [perché] — vuoi che lo installo/implemento?
   - 🟡 **Parzialmente utile** — [cosa sì, cosa no] — posso adattare la parte buona
   - ❌ **Non rilevante** — [perché] — skip
3. Se ✅ o 🟡, proponi l'azione concreta in 1-2 righe

---

## Note operative

- Cadenza suggerita: **ogni lunedì mattina** (o quando richiesta manualmente)
- Durata attesa: 2-3 minuti (ricerche parallele in Modalità 1), 1-2 minuti (analisi in Modalità 2)
- Se opencli non è disponibile: usa direttamente `WebFetch` sulle stesse URL
- Per i repo GitHub senza numero di stelle visibile, considera il numero di fork come proxy
- Priorità 🔴 = skill che copre un'area dove Roy non ha ancora uno strumento dedicato
- Priorità 🟡 = migliora qualcosa che già funziona
- Priorità ⚪ = nice-to-have, valuta in futuro
- **Prima di creare/modificare qualsiasi file**: mostra sempre la preview a Roy e aspetta conferma
- **Dopo ogni implementazione**: elenca i file creati/modificati con i path completi
- **Per contenuti misti** (es. articolo che contiene sia un tool da installare che tips): gestisci entrambe le parti, prima il tool (Modalità 1) poi i tips (Modalità 2)

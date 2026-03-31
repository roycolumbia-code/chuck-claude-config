# Skill: /newsclaude

Esegui una ricognizione delle novità più rilevanti nell'ecosistema Claude Code: nuovi plugin, skill, tool e funzionalità discusse dalla community. Filtra e presenta solo ciò che è rilevante per Roy (vedi profilo interessi sotto).

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

Sono **escluse** le novità puramente tecniche/developer (es. testing framework, linter, CI/CD pipeline) a meno che non abbiano applicazione diretta al lavoro di Roy.

---

## Comportamento della skill

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

## Note operative

- Cadenza suggerita: **ogni lunedì mattina** (o quando richiesta manualmente)
- Durata attesa: 2-3 minuti (ricerche parallele)
- Se opencli non è disponibile: usa direttamente `WebFetch` sulle stesse URL
- Per i repo GitHub senza numero di stelle visibile, considera il numero di fork come proxy
- Priorità 🔴 = skill che copre un'area dove Roy non ha ancora uno strumento dedicato
- Priorità 🟡 = migliora qualcosa che già funziona
- Priorità ⚪ = nice-to-have, valuta in futuro

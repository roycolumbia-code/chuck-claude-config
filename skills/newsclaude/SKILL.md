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

### Step 1 — Ricerca parallela (3 fonti)

Esegui in parallelo le seguenti ricerche con `Bash` usando `opencli`:

**Fonte A — Google (plugin/skill nuovi):**
```bash
opencli google search "claude code new plugin skill 2025 site:github.com"
opencli google search "claude code plugin marketplace new 2025"
```

**Fonte B — Hacker News (discussioni e annunci):**
```bash
opencli hackernews search "claude code"
```

**Fonte C — GitHub diretto (repo ad alto engagement recenti):**
Usa `WebFetch` su questi URL di ricerca GitHub per trovare repo con molte star:
- `https://github.com/search?q=claude+code+plugin&sort=stars&type=repositories`
- `https://github.com/ComposioHQ/awesome-claude-plugins`
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

### Plugin / Skill da valutare

| # | Nome | Cosa fa | Area | Stelle | Priorità | Già installato? |
|---|------|---------|------|--------|----------|-----------------|
| 1 | ... | ... | ... | ⭐ N | 🔴 Alta | No |
...

### Discussioni interessanti (HN / community)
- **[Titolo]** — Score: N | [breve sintesi 1 riga]
...

### Come installare un plugin
Per installare: `/plugin install <nome>@<marketplace>`
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

# Skill: /updates

Verifica e aggiorna autonomamente tutti i componenti del sistema Claude Code di Roy:
plugin installati, tool CLI (ruFlo/claude-flow), MCP servers via npx.
Non chiedere conferma — aggiorna tutto, poi mostra il summary.

---

## Comportamento

Quando Roy digita `/updates`:

### Step 1 — Aggiorna Plugin Claude Code

Esegui in parallelo:

**1a — Leggi stato attuale:**
```bash
claude plugin list --json
```
Estrai: id, versione, errori. Nota quelli con `errors` (repo non trovato = skip).

**1b — Aggiorna tutti i plugin validi:**
Per ogni plugin senza errori, esegui con l'**id completo** (es. `vercel@claude-plugins-official`):
```bash
claude plugin update "<id-completo>"
```
L'id completo si legge dal campo `id` del JSON di `claude plugin list --json`. NON usare solo il nome senza `@marketplace`.

Nota: `claude plugin update` restituisce output che indica se c'era un aggiornamento o era già aggiornato. Cattura entrambi i casi.

**1c — Rimuovi plugin con errori persistenti:**
Se un plugin ha `errors` nel JSON (es. "Plugin X not found in marketplace"), segnalalo ma NON rimuoverlo — chiedi conferma a Roy prima.

---

### Step 2 — Aggiorna ruFlo / claude-flow

```bash
# Versione installata nella cache npx
cat ~/.npm/_npx/*/node_modules/@claude-flow/cli/package.json 2>/dev/null | python3 -c "import json,sys; d=json.load(sys.stdin); print(d.get('version','?'))"

# Ultima versione su GitHub
curl -s "https://api.github.com/repos/ruvnet/ruflo/releases/latest" | python3 -c "import json,sys; d=json.load(sys.stdin); print(d.get('tag_name','?').lstrip('v'))"
```

Se la versione installata è diversa dall'ultima:
1. Svuota la cache npx: `rm -rf ~/.npm/_npx/*/node_modules/@claude-flow/`
2. Pre-scarica la nuova versione: `npx -y @claude-flow/cli@latest --version`
3. Registra versione aggiornata nel summary

Se già aggiornato: nota "ruFlo vX.Y.Z — già aggiornato" nel summary.

---

### Step 3 — Controlla tool CLI installati via brew

```bash
brew outdated 2>/dev/null | grep -i "claude\|peon\|ruflo" || echo "nessun tool Claude outdated via brew"
```

Se ci sono tool outdated rilevanti: `brew upgrade <nome>`.

---

### Step 4 — Controlla MCP servers npm-based

Leggi `~/Chuck/.mcp.json` e `~/.claude/settings.json` per trovare MCP server configurati con `npx`.
Per ognuno con `@latest` o versione fissa:
- Se `@latest`: già configurato per auto-update, nota nel summary
- Se versione fissa (es. `@3.5.42`): controlla ultima versione su npm e segnala se outdated

```bash
cat ~/Chuck/.mcp.json | python3 -c "
import json,sys
d = json.load(sys.stdin)
for k,v in d.get('mcpServers',{}).items():
    args = v.get('args', [])
    for a in args:
        if '@' in str(a) and 'npx' not in str(a):
            print(k, '->', a)
"
```

---

### Step 5 — Output strutturato

```
## 🔄 /updates — [data ora]

### Plugin Claude Code
| Plugin | Prima | Dopo | Stato |
|--------|-------|------|-------|
| handoff | 1.0.0 | 1.0.1 | ✅ Aggiornato |
| image-gen | 2.3.1 | 2.3.1 | — Già ok |
| ... | | | |

Aggiornati: N | Già ok: M | Errori (skip): K

### ruFlo / claude-flow
- Prima: vX.Y.Z → Dopo: vA.B.C ✅
  oppure: vX.Y.Z — già aggiornato ✓

### Tool brew
- [lista outdated aggiornati o "tutti ok"]

### MCP servers
- claude-flow: @latest (auto-update) ✓
- [altri se presenti]

### ⚠️ Da verificare manualmente
- Plugin con errori: [lista] — attendere conferma Roy prima di rimuovere
```

### Step 6 — Post-update

Dopo l'aggiornamento:
- **Non** riavviare Claude Code automaticamente (richiede azione manuale utente)
- Avvisa: *"Alcuni aggiornamenti plugin richiedono riavvio di Claude Code per essere applicati."*
- Chiedi: *"Vuoi che salvi questo report in `~/chuck/IIB/0-Inbox/Claude/updates-[data].md`?"*

---

## Note operative

- Cadenza suggerita: **ogni lunedì** insieme a `/newsclaude` (prima /updates, poi /newsclaude)
- Se `claude plugin update` fallisce su un plugin: skip e nota nel summary, non interrompere
- Timeout per operazioni npm: se un comando npx impiega >60s, skip e nota
- Plugin con scope `managed` (da marketplace ufficiale): aggiornabili normalmente
- Plugin con scope `local` (path fisso): skip — non aggiornabili via CLI
- ruFlo usa `@latest` in Chuck/.mcp.json: l'aggiornamento reale avviene svuotando la cache npx

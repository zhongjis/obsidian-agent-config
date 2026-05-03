---
name: llm-wiki-status
description: Reports the current state of the wiki — counts, recent activity, raw archive status, orphans, hubs, and graph structure. Use when user says "wiki status", "what's in the vault", "wiki health", "what's recent", "show hubs", "central pages", "cross-domain bridges", "wiki insights", "wiki structure", or wants an overview of the knowledge base before deciding what to work on. Read-only by default; writes `_insights.md` only in insights mode.
---

# LLM Wiki Status

Read these first when needed:
- `../_llm-wiki-references/vault-rules.md`
- `../_llm-wiki-references/note-standards.md`
- `../_llm-wiki-references/plugin-behaviors.md`
- `../_llm-wiki-references/obsidian-cli-fast-path.md`

## Goal
Give the user a fast, accurate picture of vault state without editing content.

## Vault-native fast path
Use `../_llm-wiki-references/obsidian-cli-fast-path.md` for counts, tag stats, orphans, deadends, and unresolved links. Most status answers come from `obsidian tags`, `obsidian properties`, `obsidian orphans`, and `obsidian deadends`.

## Modes

### Mode A — Overview report (default)

Default when user asks "status", "what's in the vault", "wiki health", etc.

1. **Counts:**
   - Total root `.md` wiki pages (exclude `_Templates/`, `_Attachments/`, `Excalidraw/`, `.agents/`, `llm-wiki-log/`, `raw/`).
   - Breakdown by role tag: `llm-wiki/durable`, `llm-wiki/dated`, `llm-wiki/source`.
   - Breakdown by category tag: `llm-wiki/concept`, `entity`, `reference`, `synthesis`.
   - Raw archive: total `.md` under `raw/`, count with `llm-wiki-ingested: true`, count without. Present the un-ingested count as **archive** (not a backlog) per `vault-rules.md`.

2. **Recent activity:**
   - Last 10 notes created or modified (sort by `modified:` frontmatter or mtime).
   - Last 5 raw notes promoted (`llm-wiki-ingested: true`) if any.

3. **Health signals:**
   - `obsidian orphans` — pages with zero incoming and zero outgoing wikilinks.
   - `obsidian deadends` — pages with incoming links but zero outgoing.
   - `obsidian unresolved` — broken wikilinks.
   - Missing `summary:` on durable notes materially touched since workflow adoption (2026-04-21).
   - Legacy `#zettelkasten/*` tags still present.

4. **Recommendation:**
   - If many orphans → suggest `llm-wiki-cross-linker`.
   - If many broken wikilinks or stale claims → suggest `llm-wiki-lint`.
   - If user has durable material in chat or recent session → suggest `llm-wiki-capture`.
   - Otherwise: "vault is healthy".

Output a concise markdown report directly in chat. Do not write files.

### Mode B — Insights (graph shape)

Triggered by "insights", "hubs", "central", "bridges", "wiki structure", "what's connected".

Additive to Mode A. Analyzes the wikilink graph.

Build the graph once:
- For every root `.md` page extract: filename, title, aliases, tags, all `[[wikilinks]]`.
- Compute `incoming[page]` and `outgoing[page]` counts.

Then report:

1. **Anchor pages (top 10 hubs)** — rank by `incoming`. Connector hubs (high incoming AND outgoing) are most valuable. Sink hubs (high incoming, zero outgoing) are cross-linker candidates.

2. **Bridge pages (top 5)** — pages that connect otherwise-disconnected tag clusters (A links to P, B linked from P, A and B share no tags, P is the only 2-hop path between clusters). Label: `P bridges [cluster-A] ↔ [cluster-B]`.

3. **Tag cluster cohesion** — for each tag with ≥5 pages, score `actual_links / (n × (n−1) / 2)`. Fragmented clusters (cohesion < 0.15) → cross-linker targets. Show top 5 and bottom 5.

4. **Surprising cross-category connections** — wikilinks that cross role or category tags; score +2 for cross-category, +2 for peripheral→hub reach (source has ≤2 links, target has ≥8), +2 for marked-inferred or ambiguous content. Show top 5.

5. **Orphan-adjacent** — pages linked from a top-10 hub but with zero outgoing links. Dead-ends in high-traffic areas.

6. **Suggested follow-ups:**
   - Zero-incoming durable pages → link candidates.
   - Fragmented tag clusters → cross-linker targets.
   - Sink hubs → cross-linker targets.

**Output:** report in chat. If user explicitly asks to persist, write `_insights.md` (or `llm-wiki-log/insights-YYYY-MM-DD.md`) with the above sections. Do not write files by default.

Skip insights mode when fewer than 20 durable wiki pages exist — too little structure.

## Execution rules
- Read-only by default. Only write files when user explicitly asks to persist.
- Do not create `.manifest.json` or `hot.md` — this vault does not use them. Activity log lives at `llm-wiki-internal/log.md` (optional, append-only).
- `raw/` un-ingested count is **archive**, not backlog. Never frame it as pending work.
- Respect retrieval cost escalation — use tag/frontmatter scans and `obsidian` CLI before full page reads.

## Expected output shape
Concise markdown report. Sections labeled. Numbers not prose. Recommendations at the end.

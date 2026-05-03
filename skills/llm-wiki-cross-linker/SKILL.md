---
name: llm-wiki-cross-linker
description: Scans the wiki and inserts missing `[[wikilinks]]` between pages that mention each other. Use when user says "link my pages", "find missing links", "cross-reference", "connect the wiki", "add wikilinks", "fix orphans", or after large ingestion/capture to weave new pages into the graph. Write-heavy skill — actually modifies pages, unlike `llm-wiki-lint` which only reports.
---

# LLM Wiki Cross-Linker

Read these first when needed:
- `../_llm-wiki-references/vault-rules.md`
- `../_llm-wiki-references/note-standards.md`
- `../_llm-wiki-references/plugin-behaviors.md`
- `../_llm-wiki-references/obsidian-cli-fast-path.md`

## Goal
Find and insert missing `[[wikilinks]]` between wiki pages that should reference each other. Conservative — only link clear extracted or inferred matches.

## Vault-native fast path
Use `../_llm-wiki-references/obsidian-cli-fast-path.md`. Key commands:
- `obsidian orphans` — pages with zero links
- `obsidian deadends` — pages linked to but not linking out
- `obsidian unresolved` — broken wikilink targets
- `obsidian backlinks file=...` — incoming links for a page
- `obsidian tags counts format=json` — tag usage for cohesion analysis

## Scope
- Linkable: root `.md` wiki pages.
- Skip: `_Templates/`, `_Attachments/`, `Excalidraw/`, `.agents/`, `llm-wiki-log/`, `raw/`, `index.md` (curated by hand).

## Workflow

### Step 1 — Build page registry

Glob root `.md` pages. For each extract from frontmatter:
- filename (wikilink target)
- `aliases:`
- `tags:`
- `summary:`

Frontmatter scan only. Do not full-read pages at this step. This is the vocabulary of valid wikilink targets.

### Step 2 — Scope the run

Default to new or recently-modified pages (check `modified:` frontmatter or mtime in last N days). User can override with "link everything" or "link pages tagged `floodgate`".

### Step 3 — Find missing links

For each in-scope page:

1. Read full content.
2. Extract existing `[[wikilinks]]`.
3. Search body text for unlinked mentions of:
   - registry filenames
   - aliases
   - distinctive entity, concept, project, person names

Matching rules:
- Case-insensitive.
- Unicode-normalize (NFKD, strip combining marks) both sides before comparing.
- Skip self-references.
- Skip generic words. Only match distinctive names.
- Skip inside code blocks, frontmatter, existing wikilinks, and the `## Sources` section.
- Do not double-link — if `[[X]]` already appears on the page, do not add another.

### Step 4 — Score and rank

| Signal | Points |
|---|---|
| Exact name/alias match in body text | +4 |
| Shared category tag (`llm-wiki/concept`, `entity`, `reference`, `synthesis`) ≥2 shared overall tags | +2 |
| Cross-category wikilink (e.g. `entity` → `concept`, `reference` → `synthesis`) | +2 |
| Peripheral → hub reach (source has ≤2 outgoing, target has ≥8 incoming) | +2 |
| Mentioned concept or entity name | +2 |
| Partial name match (ambiguous) | +1 |

Confidence labels:

| Score | Label | Action |
|---|---|---|
| ≥6 | **EXTRACTED** | Apply inline. |
| 3–5 | **INFERRED** | Apply inline or add to `## Related` section. |
| 1–2 | **AMBIGUOUS** | Report only. Do not insert. |

Only act on EXTRACTED and INFERRED.

### Step 5 — Apply links

Preferred: **inline** on the first natural mention.

Before:
```
Floodgate controls access via feature flags.
```
After:
```
[[2026-04-22 Floodgate Integration Guide|Floodgate]] controls access via feature flags.
```

Use `[[target|display text]]` when the wikilink target differs from surface text.

Fallback: **Related section** when the term is not mentioned naturally but the pages are semantically linked. Append to existing `## Related` heading if present; otherwise add one just before `## Sources` (or at end of body if no Sources section).

Only link the first natural mention per target per page. Do not carpet-link.

### Step 6 — Report

```
## Cross-Link Report

Pages scanned: N
Pages modified: M
Links added: K (EXTRACTED: X, INFERRED: Y)

| Page | Links Added | Confidence | Type |
|---|---|---|---|
| 2026-04-22 Floodgate Integration Guide | 2 | EXTRACTED | 2 inline |
| 2026-04-30 Marketo MCP FloodgateClient Implementation | 3 | INFERRED | 1 inline, 2 related |

### AMBIGUOUS (not applied — review)
- 2026-04-24 X → 2026-04-22 Y — partial match "foo" / score 2
- ...

### Orphans remaining: Q
- 2023-04-05 Note — no incoming or outgoing links
- ...
```

## Execution rules
- Never touch `raw/`, `_Templates/`, `_Attachments/`, `Excalidraw/`, `.agents/`, `llm-wiki-log/`.
- Never link `index.md` — it is curated by hand.
- One inline link per target per page.
- Respect existing `## Related` structure. Do not duplicate entries.
- Do not insert links inside code blocks, frontmatter, or `## Sources`.
- Do not auto-fix legacy `#zettelkasten/*` tags — `llm-wiki-lint` reports them as suggestions.
- Follow `note-standards.md` for link format. This vault uses wikilinks, not markdown links.

## Stop conditions
- Fewer than 10 in-scope pages — not worth the run. Tell user.
- No registry entries match — tell user and stop.
- User wants review first — produce report only, apply nothing.

## Tips
- Run after `llm-wiki-capture` or `llm-wiki-ingest`. New pages are almost always under-linked.
- Run on a single tag (e.g. "link all `floodgate`-tagged pages") for focused weaving.
- Entity and concept pages are link magnets — prioritize them as targets.
- Be conservative with INFERRED. When in doubt, report, do not insert.

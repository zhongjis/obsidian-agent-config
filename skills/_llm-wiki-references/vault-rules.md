# Vault rules

Read `AGENTS.md` first. This file expands wiki-writing rules; it does not override root vault rules.
Read `note-standards.md` for note titles, frontmatter, linking, and visual defaults.

## Core model
- `raw/` is an immutable archive of source material. Most raw notes remain as inert archive, not a backlog.
- Raw sources are immutable inputs. Read, cite, compare, summarize. Do not edit, rename, move, or delete raw files unless user explicitly asks. Exception: during ingest or lint, raw markdown source notes may receive required schema frontmatter updates.
- `llm-wiki-ingested: true` marks the subset of raw notes promoted into wiki synthesis (knowledge absorbed into the wiki, provenance to the raw path recorded). Absence of the flag is **not** a backlog signal. Ingest is opt-in — promote a raw note only when it has durable, reusable value worth compiling. Natural trigger: a query surfaces the raw note as evidence worth hoisting into durable wiki form.
- Notes originally created by the AI workflow must include frontmatter `llm-wiki-created: true`. Notes without this field are assumed human-created.
- Wiki pages live flat in vault root. Do not create topic subfolders for wiki pages. Exception: operational metadata lives in `llm-wiki-internal/`.
- When classifying root wiki notes, prefer tags over folders. Use slash-style role tags like `llm-wiki/durable`, `llm-wiki/dated`, and `llm-wiki/source`.
- `_Templates/` holds Templater templates.
- `_Attachments/` is reserved and out of scope unless user explicitly asks.
- `llm-wiki-internal/` holds operational metadata: activity log (`llm-wiki-internal/log.md`) and tracking Bases. Agents write here during ingest/lint. Not wiki content — durable wiki notes still live in vault root.
- `Excalidraw/` holds drawings, not normal wiki pages.
- Root `.base` files are Obsidian Bases views, not wiki pages.

## Wiki writing rules
- Prefer updating existing durable notes over creating new notes.
- **Atomic notes**: one core topic per wiki note. Aim for around 500 words body text. If a note covers multiple distinct topics, split into separate linked notes. Use wikilinks to connect split notes.
- During ingest, distribute extracted knowledge across the right atomic notes rather than creating one large compound note.
- New wiki pages require frontmatter.
- Use closest matching template from `_Templates/` when available.
- Minimum frontmatter for new wiki pages: `created`, `modified`, `tags`.
- Recommended frontmatter for new/updated pages: `summary` (one-line ≤200 chars for cheap retrieval), `sources` (list of raw paths or URLs for provenance).
- All new wiki note titles should use `YYYY-MM-DD Title`. Preserve existing titles unless user explicitly asks to rename.
- Do not bulk-retrofit untouched legacy notes. Only add or normalize frontmatter when creating or materially updating a page.
- Preserve existing useful metadata when present. Do not drop fields like `aliases`, `type`, `source`, `wiki`, `expense`, `total_cost` if they are already meaningful on a note.
- Follow `note-standards.md` for title, linking, and visual-format choices.

## Provenance
- Keep non-trivial synthesis traceable.
- Prefer citing: relevant wiki page -> raw source path -> quoted excerpt when precision matters.
- When raw evidence changes or contradicts old wiki claims, update wiki and keep nuance.

## Workflow boundaries
- Ingest: read raw source, extract claims/topics/entities/dates/open questions, distribute into atomic wiki notes — update existing notes first, create new root wiki page only when durable retrieval value is clear and the topic does not fit an existing note.
- If ingest creates a new note, and that note is AI-created, set frontmatter `llm-wiki-created: true`.
- Query: search wiki first, consult raw only when wiki is missing, stale, incomplete, or disputed.
- Lint: audit knowledge quality, not prose style.

## Retrieval cost
Escalate only when cheaper tier cannot answer:
1. Tag/frontmatter scan — filter by tags, dates, metadata
2. Summary field — read `summary:` frontmatter (≤200 chars per note)
3. Section search (rg) — targeted extraction from headings or paragraphs
4. Full page read — expensive at scale
5. Vault-wide scan — last resort

## Index and log
- `index.md` is optional.
- Activity log is optional at `llm-wiki-internal/log.md` — a single append-only file with `## [YYYY-MM-DD] category | description` entries.
- Append to the current-day log file (create if absent) only when user wants a log entry or the task is materially large. Do not auto-log micro-ingests.

## Churn limits
- No mass cleanup, wide rewrites, or speculative restructuring.
- Make smallest useful set of edits.
- Every changed line should support ingest, query, or lint outcome directly.

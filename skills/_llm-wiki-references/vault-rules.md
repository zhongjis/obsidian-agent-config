# Vault rules

Read `AGENTS.md` first. This file expands wiki-writing rules; it does not override root vault rules.
Read `note-standards.md` for note titles, frontmatter, linking, and visual defaults.

## Core model
- `raw/` is raw source root.
- Raw sources are immutable inputs. Read, cite, compare, summarize. Do not edit, rename, move, or delete raw files unless user explicitly asks. Exception: during ingest or lint, raw markdown source notes may receive required schema frontmatter updates.
- Raw markdown source notes are considered ingested only after their knowledge has been absorbed into the wiki, provenance to the raw path has been recorded, and frontmatter `llm-wiki-ingested: true` has been set.
- Notes originally created by the AI workflow must include frontmatter `llm-wiki-created: true`.
- Wiki pages live flat in vault root. Do not create topic subfolders for wiki pages.
- When classifying root wiki notes, prefer tags over folders. Use slash-style role tags like `llm-wiki/durable`, `llm-wiki/dated`, and `llm-wiki/source`.
- `_Templates/` holds Templater templates.
- `_Attachments/` is reserved and out of scope unless user explicitly asks.
- `Excalidraw/` holds drawings, not normal wiki pages.
- Root `.base` files are Obsidian Bases views, not wiki pages.

## Wiki writing rules
- Prefer updating existing durable notes over creating new notes.
- New wiki pages require frontmatter.
- Use closest matching template from `_Templates/` when available.
- Minimum frontmatter for new wiki pages: `created`, `modified`, `tags`.
- All new wiki note titles should use `YYYY-MM-DD Title`. Preserve existing titles unless user explicitly asks to rename.
- Do not bulk-retrofit untouched legacy notes. Only add or normalize frontmatter when creating or materially updating a page.
- Preserve existing useful metadata when present. Do not drop fields like `aliases`, `type`, `source`, `wiki`, `expense`, `total_cost` if they are already meaningful on a note.
- Follow `note-standards.md` for title, linking, and visual-format choices.

## Provenance
- Keep non-trivial synthesis traceable.
- Prefer citing: relevant wiki page -> raw source path -> quoted excerpt when precision matters.
- When raw evidence changes or contradicts old wiki claims, update wiki and keep nuance.

## Workflow boundaries
- Ingest: read raw source, extract claims/topics/entities/dates/open questions, update existing pages first, create new root wiki page only when durable retrieval value is clear.
- If ingest creates a new note, and that note is AI-created, set frontmatter `llm-wiki-created: true`.
- Query: search wiki first, consult raw only when wiki is missing, stale, incomplete, or disputed.
- Lint: audit knowledge quality, not prose style.

## Index and log
- `index.md` and `log.md` are optional.
- Update them if present.
- Create them only if user asks or there is a clear ongoing retrieval/maintenance benefit.

## Churn limits
- No mass cleanup, wide rewrites, or speculative restructuring.
- Make smallest useful set of edits.
- Every changed line should support ingest, query, or lint outcome directly.

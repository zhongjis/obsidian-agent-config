---
name: llm-wiki-ingest
description: Ingests raw source material into this Obsidian LLM wiki. Use whenever user adds, imports, clips, drops, or points to new raw content and wants it absorbed into existing wiki notes. Handles normal source types by routing markdown, PDFs, images, docx, pptx, spreadsheets, URLs, and transcript-style sources through the right tools before updating the wiki.
---

# LLM Wiki Ingest

Read these first when needed:
- `../_llm-wiki-references/vault-rules.md`
- `../_llm-wiki-references/note-standards.md`
- `../_llm-wiki-references/source-types.md`
- `../_llm-wiki-references/plugin-behaviors.md`
- `../_llm-wiki-references/obsidian-cli-fast-path.md`

## Goal
Turn raw source material into durable wiki maintenance.

Do not stop at summary-only output unless user asked for summary only. Normal ingest means reading source, extracting what matters, then updating durable wiki notes.

## Vault-native fast path
Use `../_llm-wiki-references/obsidian-cli-fast-path.md` for common note search/read/create/update/property/link work before generic file tools.
If needed CLI use case is not listed there, fall back to global `obsidian-cli` skill.

## Execution rules
- Follow `vault-rules.md` and `note-standards.md` for note placement, titles, frontmatter, linking, metadata preservation, and churn limits.
- Follow `plugin-behaviors.md` when templates, Bases/Dataview, or visual outputs matter.

## Ingest workflow
1. Identify source location and type.
2. Route the source through the right tool or skill.
3. Extract:
   - key claims
   - entities / people / systems / concepts
   - dates / decisions / milestones
   - open questions / uncertainties / contradictions
   - useful quotations or figures when precision matters
4. Search for existing wiki notes that should absorb the new knowledge.
5. Update those notes first.
6. Create one new root wiki page only if existing notes cannot absorb the material cleanly.
7. Add or strengthen wikilinks.
8. Record provenance back to raw source path.
9. Update `index.md` or `log.md` only if present or user explicitly wants them.
10. If ingest creates a new note entirely by AI, set frontmatter `llm-wiki-created: true` on that note.
11. For raw markdown source notes, mark ingest completion by setting frontmatter `llm-wiki-ingested: true`.

## Source routing
Follow `../_llm-wiki-references/source-types.md`.

## Stop conditions
- If source type is unsupported or blocked, stop and ask user.
- If source is mostly binary and cannot be parsed with available tools, stop and ask user instead of guessing.

## Expected output shape
When reporting back after ingest, summarize:
- source processed
- wiki pages updated
- wiki pages created
- important links added
- provenance recorded
- follow-up gaps, if any

## Non-goals
- Do not bulk-create thin notes from one source.
- Do not reorganize raw collections during ingest unless user explicitly asks.
- Do not rewrite legacy wiki notes for style alone.
- Do not create visual artifacts, `index.md`, or `log.md` by default.

# Note standards

Read `AGENTS.md` and `vault-rules.md` first. This file defines vault-specific note creation and update conventions for LLM work.

## Placement
- Root `.md` files are wiki pages. Keep wiki pages flat in vault root.
- `_Templates/` stores Templater templates only.
- `Excalidraw/` stores Excalidraw drawings only.
- Mermaid diagrams stay embedded in their parent note.
- `_Attachments/` remains reserved unless user explicitly asks.

## Titles and filenames
- Preserve existing titles and filenames when updating notes unless user explicitly asks to rename.
- New note titles should be stable, human-readable, and retrieval-friendly.
- Use `YYYY-MM-DD Title` for all new wiki note titles, including durable notes and time-bound notes.
- Preserve existing titles unless user explicitly asks to rename.
- Keep filename aligned with note title.
- Avoid placeholder names like `note`, `untitled`, or throwaway suffixes.

## Frontmatter and templates
- When a note pattern fits an existing template, use the closest Templater template from `_Templates/` and let the template own the note scaffold.
- Reusable note patterns should live in `_Templates/` and be handled through Templater rather than ad-hoc manual note creation.
- If no template fits, add minimum frontmatter: `created`, `modified`, `tags`.
- Preserve meaningful existing fields such as `aliases`, `type`, `source`, `wiki`, `expense`, or `total_cost`.
- Only normalize frontmatter on notes you create or materially update.
- When a raw source note has been ingested and the user wants source-note state tracked in-place, set frontmatter `llm-wiki-ingested: true` on that raw note.

## Linking and provenance
- Use wikilinks for internal note references: `[[Exact Note Title]]`.
- Use link aliases only when display text materially improves readability: `[[Exact Note Title|label]]`.
- Prefer linking related durable notes when they exist; use raw paths or quotes to support non-trivial synthesis when precision matters.
- Use embeds for local assets and visual files: `![[file]]`.
- Add a small number of meaningful related links when creating or materially expanding a durable note.

## Visual defaults
- When user needs canvas, whiteboard, or freeform visual thinking, default to Excalidraw.
- Store Excalidraw files only under `Excalidraw/`.
- Prefer existing Excalidraw plugin naming/location behavior over ad-hoc filenames.
- Do not use Obsidian Canvas in this vault unless user explicitly asks.
- When note needs an inline diagram, Mermaid is default unless another format is clearly better.
- Prefer Mermaid for flows, sequences, states, comparisons, and compact diagrams that live inside a note.
- Prefer Excalidraw when visual needs spatial layout, manual rearrangement, or standalone visual artifact.

## Churn limits
- Follow existing vault patterns before introducing a new one.
- Keep changes surgical.
- Do not retrofit old notes in bulk just to match this standard.

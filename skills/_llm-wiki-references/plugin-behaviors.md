# Plugin-aware behaviors

Read `AGENTS.md` and `vault-rules.md` first. This file covers plugin-specific behavior only.

## Templater
- Prefer Templater templates from `_Templates/` over generic note creation.
- For new wiki notes, choose closest matching template when one clearly fits.
- If no template fits, create minimal frontmatter manually with at least `created`, `modified`, and `tags`.
- Reusable note patterns should be expressed as Templater templates under `_Templates/`, not as ad-hoc one-off scaffolds.

## Obsidian Bases + Dataview
- Frontmatter discipline matters because Bases and Dataview depend on it.
- Existing Bases views already rely on fields like `tags`, `created`, `modified`, and sometimes note-specific fields such as `wiki`, `aliases`, `expense`, or `total_cost`.
- Preserve existing metadata when you touch a note.
- Do not invent a heavy new schema unless user asks.

## Excalidraw
- Prefer Excalidraw when user wants an Obsidian-native visual diagram.
- Do not suggest Obsidian Canvas for this vault.
- Do not create Excalidraw files by default during ingest or lint.
- Use Excalidraw only when a visual artifact materially improves the output or user explicitly asks.
- For canvas-style or freeform visual work, Excalidraw is default and files should live under `Excalidraw/`.

## Slides and visual outputs
- Source doc mentions Marp conceptually, but this vault has `obsidian-advanced-slides` installed.
- If user wants slides, produce slide-friendly markdown that fits installed plugin workflow instead of assuming generic Canvas or Marp-only output.
- Mermaid is default for diagrams that live inside notes unless another format is clearly better.

## Search and retrieval helpers
- Omnisearch, metadata extraction, and graph view benefit from clear filenames, useful headings, clean wikilinks, and stable frontmatter.
- Lint should flag metadata or linking issues only when they hurt retrieval or maintenance.

## Reserved content
- `_Attachments/` stays out of scope unless user explicitly asks.
- Do not reorganize Excalidraw files, Bases files, or templates unless user explicitly asks.

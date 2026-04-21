# Source types

Read `AGENTS.md` and `vault-rules.md` first. This file covers source-routing detail only.

## Principle
"Support normal types" means: route common source formats through the right tool or skill, then convert them into durable wiki maintenance. Do not pretend one parser handles everything.

## Common source routing

| Source type | Normal handling |
| --- | --- |
| Markdown / plain text (`.md`, `.txt`) | Read directly. Preserve quoted passages, headings, links, and visible metadata when useful. |
| Web clip markdown / Raindrop export | Read directly. Treat as raw source even if formatting is messy. Extract source title, URL, quotes, highlights, tags, and main claims. |
| PDF | Use PDF-oriented tooling/skill to extract text, tables, or OCR before synthesis. |
| Word docs (`.docx`) | Use DOCX-oriented tooling/skill before synthesis. |
| Slide decks (`.pptx`) | Use PPTX-oriented tooling/skill before synthesis. |
| Spreadsheets / tabular files (`.xlsx`, `.xlsm`, `.csv`, `.tsv`) | Use spreadsheet-oriented tooling/skill. Extract structure, key rows, metrics, trends, and derived conclusions. |
| Images (`.png`, `.jpg`, `.webp`, `.gif`) | Inspect image directly. If text matters, use OCR-capable tooling if available. Summarize both visible content and textual evidence separately. |
| Web URLs / GitHub pages | Fetch readable content first, then ingest. |
| YouTube / video / audio transcript sources | Fetch transcript or media analysis first. Use prompt-aware extraction when needed, then ingest resulting content. |

## Mixed-media guidance
- If source has markdown plus linked images, read text first, then inspect images only when they add meaning.
- If source has tables or charts, capture both raw figures and the author's claim about them.
- If source is mostly binary and cannot be parsed with available tools, stop and ask user instead of guessing.

## Ingest output expectation
After routing the source type, convert results into wiki maintenance:
1. identify key claims, entities, dates, decisions, open questions
2. find existing notes that should absorb this knowledge
3. update those notes or create one new root note when truly needed
4. add wikilinks
5. record provenance to raw path

## Non-goals
- Do not rewrite raw source files into a new format unless user asks.
- Do not move attachments or other reserved-path content as part of ingest.
- Do not create one wiki page per source by default. Prefer stronger synthesis.

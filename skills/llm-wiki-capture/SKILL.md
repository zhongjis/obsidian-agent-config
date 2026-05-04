---
name: llm-wiki-capture
description: Saves the current conversation as a durable wiki note. Use when user says "save this", "capture this", "file this", "preserve this", "add this to the wiki", or wants to turn what was just discussed into a permanent wiki page. Extracts the substance (declarative knowledge) rather than a chat transcript, picks the right template, and lands the note flat in vault root.
---

# LLM Wiki Capture

Read these first when needed:
- `../_llm-wiki-references/vault-rules.md`
- `../_llm-wiki-references/note-standards.md`
- `../_llm-wiki-references/plugin-behaviors.md`
- `../_llm-wiki-references/obsidian-cli-fast-path.md`

## Goal
Turn the current conversation into one durable wiki note — declarative knowledge, not a chat transcript.

## Vault-native fast path
Use `../_llm-wiki-references/obsidian-cli-fast-path.md` for common search/read/create operations before generic file tools.

## Capture workflow

1. **Identify what is worth preserving.** Decisions, frameworks, technical findings, synthesized understanding, clear explanations that took effort. Skip scheduling, exploratory back-and-forth without conclusion, and anything already in the wiki. If nothing material emerged, tell the user and stop.

2. **Search existing wiki first.** Use `obsidian search` on the core topic. Prefer updating an existing durable note over creating a new one — capture into the right atomic note rather than producing a duplicate.
   - If the topic already has a Topic Hub, update only its one-sentence orientation, `# Quick reference`, or `# Index`; put procedural overflow in a concise Quickstart note instead of adding synthesis sections to the hub.

3. **Classify content type → pick template.** Follow [[2026-04-21 LLM-Wiki Workflow#When to Write What]]:

   | Content type | Template | Role tag |
   |---|---|---|
   | Durable concept, entity, reference, or synthesis | `_Templates/core/Durable Wiki Note.md` | `llm-wiki/durable` + category tag |
   | Meeting, session, or time-bound working note | `_Templates/core/Dated Working Note.md` | `llm-wiki/dated` |
   | Digest of an external source discussed | `_Templates/core/Source Digest.md` | `llm-wiki/source` |
   | Quick scratch capture | `_Templates/Quick Note.md` | `llm-wiki/dated` + topic tag |
   | High-use topic landing page with one-sentence orientation, quick links, and subnote navigation only | `_Templates/core/Topic Hub.md` | `llm-wiki/durable` + category/domain tags |

4. **Rewrite as declarative knowledge.**
   - Not: "The user asked about X and the assistant explained..."
   - Yes: "X works by ... because ..."
   - Describe the knowledge itself, present tense.
   - Keep it atomic (~500 words body). If the conversation covered multiple topics, split into multiple linked notes.

5. **Write the note.**
   - Title: `YYYY-MM-DD Title`. Flat in vault root.
   - Frontmatter required: `created`, `modified`, `tags` (role + category), `summary` (≤200 chars), `sources`, `llm-wiki-created: true`.
   - `sources:` entry: `conversation:YYYY-MM-DD` plus any raw paths or URLs referenced during the chat.
   - Cross-link to at least two existing wiki pages. If none exist, that is a signal the topic may be premature for a durable note — reconsider whether to capture.
   - Follow `note-standards.md` for title, frontmatter, linking, and visual defaults.

6. **Update `index.md`** if present and the new note fits an existing section. Do not create `hot.md` — this vault does not use it. Activity log at `llm-wiki-internal/log.md` is optional; append only when user asks or task is materially large.
   If a Topic Hub exists for the topic, let the hub own navigation only; keep `index.md` to a single pointer where possible.

7. **Confirm to user.** Report the saved path, title, tags, and related wiki pages linked.

## Execution rules
- Prefer updating an existing note over creating a new one.
- Atomic notes only. Split compound captures into linked notes.
- Always set `llm-wiki-created: true` on AI-created notes.
- Record provenance in `sources:` — at minimum `conversation:YYYY-MM-DD`.
- If the captured topic originated from a raw source file discussed in chat, add that raw path to `sources:` and, when absorbing material from a raw markdown note, set `llm-wiki-ingested: true` on the raw note per `vault-rules.md`.

## Stop conditions
- No durable material in conversation — stop and tell user.
- Captured topic already has a strong existing note — update that note instead of creating a new one.
- Ambiguous scope (multiple distinct topics) — ask user which topic(s) to capture, then split.

## Expected output shape
Report back:
- target note path
- template used
- tags applied
- wikilinks added
- whether this updated an existing note or created a new one

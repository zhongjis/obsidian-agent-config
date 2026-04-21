---
name: llm-wiki-query
description: Answers questions against this Obsidian LLM wiki. Use whenever user asks about topics that may already live in the vault and wants synthesis, comparison, explanation, citations, or derived outputs. Search wiki first, consult raw sources only when wiki is missing, stale, incomplete, or disputed.
---

# LLM Wiki Query

Read these first when needed:
- `../_llm-wiki-references/vault-rules.md`
- `../_llm-wiki-references/note-standards.md`
- `../_llm-wiki-references/plugin-behaviors.md`
- `../_llm-wiki-references/obsidian-cli-fast-path.md`

## Goal
Answer questions from maintained wiki knowledge first, then use raw sources only as fallback or dispute resolution.

## Vault-native fast path
Use `../_llm-wiki-references/obsidian-cli-fast-path.md` first for common wiki search/read/context/backlink work.
If needed CLI use case is not listed there, fall back to global `obsidian-cli` skill.

## Query workflow
1. Search relevant wiki pages first.
2. Read the strongest candidate notes.
3. Synthesize answer from wiki content.
4. Consult raw sources only when:
   - wiki is missing coverage
   - wiki looks stale
   - wiki notes disagree
   - user asks for raw-source validation
5. Cite supporting wiki pages and raw paths used.
6. If answer is durable and valuable, offer to file it back into the wiki.

## Output modes
Default output is normal markdown in chat.

When user asks for another form, adapt carefully:
- comparison / matrix -> markdown table
- durable memo / note -> root wiki page with frontmatter
- slide deck -> slide-friendly markdown compatible with installed Obsidian slide workflow
- diagram -> prefer Excalidraw or Mermaid; do not suggest Obsidian Canvas
- timeline / chart -> use only when time-series or numeric structure truly helps

## Citation rules
- Cite wiki pages when claims come from existing notes.
- Cite raw source paths when you needed raw evidence.
- Quote exact excerpts when precision matters or there is disagreement.

## Execution rules
- Default to wiki-first, not raw-first.
- If filing answer back into vault, follow `vault-rules.md` and `note-standards.md` for note creation, titles, frontmatter, linking, and visual defaults.
- Do not create notes or diagrams unless useful or requested.

## Good query behavior
- Merge scattered wiki evidence into one answer.
- Call out contradictions instead of hiding them.
- Distinguish known facts vs open questions.
- Recommend filing result back into wiki when it would save future rediscovery.

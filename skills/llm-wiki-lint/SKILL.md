---
name: llm-wiki-lint
description: Audits this Obsidian LLM wiki for knowledge-health issues. Use whenever user asks to lint, maintain, clean up, health-check, audit, or review wiki quality. Focus on contradictions, stale claims, duplicate coverage, weak cross-links, orphan pages, missing durable pages, and metadata/link issues that hurt retrieval.
---

# LLM Wiki Lint

Read these first when needed:
- `../_llm-wiki-references/vault-rules.md`
- `../_llm-wiki-references/note-standards.md`
- `../_llm-wiki-references/plugin-behaviors.md`
- `../_llm-wiki-references/obsidian-cli-fast-path.md`

## Goal
Lint the vault as a knowledge system, not as a style guide.

## Vault-native fast path
Start with `../_llm-wiki-references/obsidian-cli-fast-path.md` for common orphan/link/tag/property checks before wider manual inspection.
If needed CLI use case is not listed there, fall back to global `obsidian-cli` skill.

## What to check
Audit for:
- stale claims
- contradictions between notes
- duplicate or fragmented coverage
- weak or missing cross-links
- orphan pages
- entities or concepts that deserve durable pages
- provenance gaps on non-trivial synthesis
- required metadata gaps such as missing `llm-wiki-ingested: true` on ingested raw markdown notes
- missing `llm-wiki-created: true` on notes known from workflow evidence to be AI-created (notes without this field are assumed human-created — do not flag absence alone)
- missing `summary:` field on notes materially updated since workflow adoption
- missing `sources:` provenance on AI-created or recently ingested notes
- legacy `#zettelkasten/*` tags on notes that could be reclassified to `llm-wiki/*` (report as suggestion, do not auto-fix)
- suggested cross-links between related notes (report-only — never auto-insert)
- places where stronger raw evidence should update wiki synthesis

## Lint workflow
1. Inspect relevant wiki pages and supporting raw paths when needed.
2. Group findings by problem type.
3. Rank findings by impact:
   - now: harms correctness, retrieval, or maintainability
   - later: useful improvement, not urgent
4. Recommend smallest useful remediation steps.
5. If user asks for fixes, apply them surgically.

## Reporting format
Prefer a concise maintenance report with sections like:
- contradictions
- stale claims
- duplicates / merge candidates
- orphan / weakly linked notes
- missing durable pages
- metadata / retrieval issues
- recommended next actions

Each finding should include:
- affected note(s)
- why it matters
- evidence or raw path if relevant
- suggested fix

## Execution rules
- Treat metadata or plugin-related issues as lint findings only when they hurt retrieval or maintenance, or when they violate required vault fields.
- Prefer actionable findings over exhaustive noise.
- Keep churn low.
- Use `note-standards.md` when judging title, frontmatter, linking, and visual-format consistency, but do not guess ingest state or authorship without workflow evidence.

## Non-goals
- Do not turn this into prose nitpicking.
- Do not bulk-retrofit untouched legacy notes.
- Do not reorganize vault structure unless user explicitly asks.
- Do not silently fix everything unless user asked for direct maintenance.
- Do not invent or enforce ad-hoc metadata beyond required vault fields such as `llm-wiki-ingested` and `llm-wiki-created` unless evidence shows an existing workflow needs it.

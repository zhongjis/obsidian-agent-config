# Tag registry

Read `AGENTS.md` and `vault-rules.md` first. This file defines the controlled tag vocabulary for this vault.

## Purpose

Single source of truth for canonical tags. Agents and humans check this before inventing new tags. Tag Wrangler manages tags in Obsidian; this doc governs what tags mean and when to use them.

## Rules

- Every wiki note needs at least one role tag.
- Category tags are recommended for new notes.
- Do not create new `llm-wiki/*` tags without updating this registry.
- Domain and topic tags are open — create freely, but prefer existing tags over synonyms.
- When materially updating a note with legacy tags, reclassify per the migration table below.

## Role tags (required — pick one)

| Tag | Meaning | Count |
|---|---|---|
| `llm-wiki/durable` | Long-lived concept, entity, or topic | ~325 |
| `llm-wiki/dated` | Time-bound — meeting, session, event, task | ~394 |
| `llm-wiki/source` | Source digest from ingest | ~98 |

## Category tags (recommended for new notes)

| Tag | Meaning | Use for |
|---|---|---|
| `llm-wiki/concept` | Abstract ideas, patterns, mental models | Design patterns, methodologies, principles |
| `llm-wiki/entity` | Concrete things — people, tools, companies | Adobe, MongoDB, Marketo, specific services |
| `llm-wiki/reference` | Factual lookups — specs, APIs, configs | Cheatsheets, API docs, setup guides |
| `llm-wiki/synthesis` | Cross-cutting analysis connecting multiple concepts | Comparison notes, decision records |

## System tags

| Tag | Meaning |
|---|---|
| `llm-wiki/ambiguous` | Sources disagree or claim unclear (~48 notes) |

## Domain tags (open vocabulary — most used)

### Work
| Tag | Meaning | Count |
|---|---|---|
| `adobe` | Adobe work — general | ~224 |
| `adobe/marketo` | Marketo team | ~31 |
| `adobe/appservice` | AppService team | ~18 |
| `adobe/ethos` | Ethos platform | ~8 |
| `adobe/marketo/cet` | Marketo CET | ~8 |
| `adobe/aep` | Adobe Experience Platform | ~3 |
| `adobe/cso` | Customer support operations | ~3 |
| `ticket` | Jira tickets | ~45 |
| `meeting` | Meeting notes | ~32 |
| `cso` | CSO incidents | ~11 |
| `on-call` | On-call notes | ~4 |
| `1-1` | One-on-one meetings | ~4 |

### Technical
| Tag | Count | Tag | Count |
|---|---|---|---|
| `cheatsheet` | ~24 | `java` | ~11 |
| `redis` | ~11 | `terraform` | ~12 |
| `aws` | ~11 | `azure` | ~10 |
| `sql` | ~8 | `devops` | ~6 |
| `nix` | ~6 | `k8s` | ~5 |
| `mongodb` | ~5 | `guide` | ~8 |
| `fix` | ~8 | `bug` | ~6 |
| `code-snippet` | ~4 | `sre` | ~4 |
| `grafana` | ~4 | `vim` | ~7 |

### Personal
| Tag | Meaning | Count |
|---|---|---|
| `115-oxford` | Property notes | ~16 |
| `school` | Monroe College | ~13 |
| `sky` | Sky (pet) | ~12 |
| `travel` | Travel planning | ~9 |
| `pi` | Pi agent | ~9 |
| `auto-research` | Auto-research sessions | ~8 |
| `interview-note` | Interview evaluations | ~7 |
| `work-order` | Property maintenance | ~7 |
| `3d-printing` | 3D printing | ~5 |
| `immigration` | Immigration notes | ~5 |
| `honda-accord-2015` | Vehicle notes | ~7 |
| `lexus-rx-350h` | Vehicle notes | ~2 |

## Status tags (inline hashtags)

| Tag | Meaning |
|---|---|
| `#status/completed` | Task or note finished |
| `#status/planning` | In planning phase |

## Legacy tags (reclassify on touch)

| Old Tag | Replace With | Notes |
|---|---|---|
| `#zettelkasten/fleeting` | `llm-wiki/dated` | Temporary capture notes |
| `#zettelkasten/literature` | `llm-wiki/source` | Literature notes from reading |
| `#zettelkasten/permanent` | `llm-wiki/durable` | Permanent knowledge |
| `#zettelkasten/processed` | Remove; set `llm-wiki-ingested: true` | Processing status replaced by frontmatter |
| `#zettelkasten/project` | Domain tag (e.g., `adobe`, `pi`) | PARA project category |
| `#zettelkasten/area-of-improvements` | Domain tag or `llm-wiki/concept` | PARA area category |
| `#zettelkasten/resource` | `llm-wiki/reference` | PARA resource category |
| `#zettelkasten/archive` | `llm-wiki/dated` | PARA archive category |

## Adding new tags

1. **Domain/topic tags** — create freely. Prefer existing tags. Use slash hierarchy for subtopics (`adobe/marketo/cet`).
2. **`llm-wiki/*` tags** — update this registry first. These are controlled vocabulary.
3. **Status tags** — use `#status/*` inline hashtags sparingly.
4. **Avoid** — don't create tags that duplicate an existing tag's meaning. Check this registry and use `obsidian tags sort=count counts` to see what exists.

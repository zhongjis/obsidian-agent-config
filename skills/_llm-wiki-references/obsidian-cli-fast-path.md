# Obsidian CLI fast path

Use this file for common vault-native work before generic file search/edit tools.
Assume Obsidian app is open.

If needed CLI use case is not listed here, fall back to global `obsidian-cli` skill:
`/home/zshen/.pi/agent/skills/obsidian-cli/SKILL.md`

## Most-used commands

### Find notes
- `obsidian search query="topic" limit=10`
- `obsidian search:context query="topic" limit=10`
- `obsidian read file="Note Name"`

### Create or update notes
- `obsidian templater:create-from-template template="Template.md" file="Note.md"`
- `obsidian create name="Note Name" content="# Title"`
- `obsidian append file="Note Name" content="new content"`
- `obsidian prepend file="Note Name" content="new content"`
- `obsidian property:set name="tags" value="tag1, tag2" file="Note Name"`

### Check links and retrieval
- `obsidian backlinks file="Note Name" format=json`
- `obsidian links file="Note Name"`
- `obsidian unresolved counts verbose format=json`

### Lint baseline
- `obsidian orphans`
- `obsidian deadends`
- `obsidian tags sort=count counts format=json`
- `obsidian properties counts format=json`

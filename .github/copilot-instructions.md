# Repository instructions

## Agent metadata

For newly created `.agent.md` profiles, include this metadata in the YAML
frontmatter:

```yaml
metadata:
  version: "0.1.0"
  author: "btnguyen2k"
  repository: "https://github.com/btnguyen2k/ghcp-agents"
```

Use the initial version `"0.1.0"` unless the creator specifies another version.
These defaults apply to new profiles; do not increase existing agent versions
unless explicitly requested.

## Commit-message agent synchronization

The repository uses its own `angular-style-commit-message-generator` agent.
Treat `agents\angular-style-commit-message-generator.agent.md` as the source
profile. Whenever it changes, update
`.github\agents\angular-style-commit-message-generator.agent.md` in the same change.
Keep both files identical, including all frontmatter, metadata, and instructions.

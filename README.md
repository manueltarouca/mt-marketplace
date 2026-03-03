# mt-marketplace

Personal Claude Code plugin marketplace.

## Plugins

| Plugin | Description |
|--------|-------------|
| **eza-plugin** | Modern replacement for `ls` with colors, icons, git status, and tree views |

## Installation

```sh
/plugin marketplace add manueltarouca/mt-marketplace
/plugin install eza-plugin@mt-marketplace
```

## Adding plugins

1. Create a new directory under `plugins/`
2. Add `.claude-plugin/plugin.json` and your skill/hook/agent files
3. Update `.claude-plugin/marketplace.json` with the new plugin entry
4. Push to GitHub

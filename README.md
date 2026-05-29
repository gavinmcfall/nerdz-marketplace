# nerdz-marketplace

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) for self-hosted, open-source plugins by [@gavinmcfall](https://github.com/gavinmcfall) ([nerdz.co.nz](https://nerdz.co.nz)).

## Use it

From any Claude Code session:

```text
/plugin marketplace add gavinmcfall/nerdz-marketplace
/plugin install spyglass-client@nerdz
/plugin enable spyglass-client@nerdz
```

The **repo** is `nerdz-marketplace`; the **marketplace name** (the `@nerdz` part in install commands) is just `nerdz`. They're independent on purpose — short install command, descriptive repo name.

## Plugins

| Plugin | Lives in | Description |
|--------|----------|-------------|
| [`spyglass-client`](https://github.com/gavinmcfall/spyglass/tree/main/plugins/spyglass-client) | external (`gavinmcfall/spyglass`) | Reports each Claude Code session to a [Spyglass](https://github.com/gavinmcfall/spyglass) dashboard. A hook captures the environment, the `spyglass-reporter` skill composes and `POST`s the update over an authenticated API. |

## How this works

This repo is a thin manifest (`.claude-plugin/marketplace.json`) that lists plugins. The plugins themselves can live **inside this repo** (relative path) or in **external git repos** — the marketplace just references them. Each plugin is pinned independently from the marketplace, so individual plugins release at their own cadence.

For example, `spyglass-client` lives in `gavinmcfall/spyglass`, versioned alongside the server it talks to:

```json
{
  "name": "spyglass-client",
  "source": {
    "source": "git-subdir",
    "url": "gavinmcfall/spyglass",
    "path": "plugins/spyglass-client",
    "ref": "main"
  }
}
```

## Adding a new plugin

Append an entry to `.claude-plugin/marketplace.json`'s `plugins` array. The `source` field accepts several shapes:

| Where the plugin lives | `source` |
|------------------------|----------|
| In this repo, under `./plugins/<name>` | `"./plugins/<name>"` |
| External GitHub repo, at its root | `{ "source": "github", "repo": "owner/repo" }` |
| External GitHub repo, in a subdirectory | `{ "source": "git-subdir", "url": "owner/repo", "path": "subdir" }` |
| Other git host / self-hosted | `{ "source": "url", "url": "https://…" }` |
| npm package | `{ "source": "npm", "package": "@org/plugin" }` |

All support optional `ref` (branch/tag) and `sha` (commit) for pinning. Omitting `ref` and `sha` means every new commit on the default branch auto-updates installed clients.

Full schema: [plugin-marketplaces reference](https://code.claude.com/docs/en/plugin-marketplaces).

## Roadmap

When the `nerdz` GitHub org reclaim completes (defunct since 2012, application pending), this repo and the plugins it lists will likely move under that org. Until then, the install path stays `gavinmcfall/nerdz-marketplace`.

## License

MIT. Each listed plugin carries its own license.

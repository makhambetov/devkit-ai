# devkit-ai

A marketplace of [Claude Code](https://code.claude.com) plugins for AI-driven development.

## Plugins

| Plugin | What it does |
|--------|--------------|
| [`ai-dev-starter`](plugins/ai-dev-starter/) | Bootstrap new projects from a researched, documented, wave-executed methodology — `/bootstrap-project` (interview → build-vs-buy research → generated workspace) and `/starter-sync` (harvest drift). Ships a `go-react-monorepo` reference profile. |

_More plugins will live alongside under `plugins/`._

## Install

Add the marketplace once, then install any plugin from it:

```
/plugin marketplace add <git-url-or-local-path-to-this-repo>
/plugin install ai-dev-starter@devkit-ai
```

Local development:

```
/plugin marketplace add /Users/i.makhambetov/ivan/projects/devkit-ai
/plugin install ai-dev-starter@devkit-ai
```

## Repository layout

```
devkit-ai/                          ← the marketplace (repo root)
├── .claude-plugin/
│   └── marketplace.json            ← catalog: owner + plugins[]; metadata.pluginRoot "./plugins"
└── plugins/
    └── ai-dev-starter/             ← a self-contained plugin
        ├── .claude-plugin/plugin.json
        ├── skills/                 ← active: bootstrap-project, starter-sync
        ├── core/  ·  profiles/     ← template assets
        └── README.md
```

Adding a plugin = drop a self-contained plugin directory under `plugins/` and add one entry to
`marketplace.json`. Because `metadata.pluginRoot` is `./plugins`, each entry's `source` is just the
folder name (e.g. `"source": "ai-dev-starter"`).

## License

MIT.

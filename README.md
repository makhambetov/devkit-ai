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
/plugin marketplace add https://github.com/makhambetov/devkit-ai
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
│   └── marketplace.json            ← catalog: owner + plugins[] (each a git-subdir source)
└── plugins/
    └── ai-dev-starter/             ← a self-contained plugin
        ├── .claude-plugin/plugin.json
        ├── skills/                 ← active: bootstrap-project, starter-sync
        ├── core/  ·  profiles/     ← template assets
        └── README.md
```

Adding a plugin = drop a self-contained plugin directory under `plugins/` and add one entry to
`marketplace.json` with a `git-subdir` source pointing at this repo and the plugin's `path`
(matching the form the official Claude Code marketplace uses). A relative `"./plugins/..."` source
also works for same-repo plugins added via Git, but `git-subdir` is the more robust, widely-supported form.

## License

MIT.

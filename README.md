# Symfony Contributions Skills

**Contribute to Symfony like a regular, with your AI agent already knowing the house rules.** 🤖

A small plugin that teaches [Claude Code](https://docs.claude.com/en/docs/claude-code/overview), [OpenAI Codex](https://developers.openai.com/codex), and [Gemini CLI](https://github.com/google-gemini/gemini-cli) the official Symfony contribution rules, so your pull requests land clean on the first try instead of bouncing through three rounds of review. The rules are distilled from Symfony's own [contribution docs](https://symfony.com/doc/current/contributing/index.html) and stay traceable to the exact `.rst` sources they came from.

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE) · Works with **Claude Code** · **OpenAI Codex** · **Gemini CLI**

## ✨ Why

Open a PR on [`symfony/symfony`](https://github.com/symfony/symfony) or [`symfony/symfony-docs`](https://github.com/symfony/symfony-docs) and a reviewer will (kindly) point out that your comparison should be Yoda-style, your deprecation is missing its `UPGRADE-*.md` note, or your `.rst` used single backticks instead of double. These skills front-load all of it, so your agent gets it right while writing the code, not after someone flags it.

## 🧩 Skills

| Skill | Reach for it when | Distilled from |
|---|---|---|
| [**symfony-code-contribution**](skills/symfony-code-contribution/SKILL.md) | You're writing or reviewing **PHP** for Symfony core, a bundle, UX (PHP side), AI, etc. Coding standards, naming, deprecations, the backward-compatibility promise, and the patch-version policy. | [standards](https://symfony.com/doc/current/contributing/code/standards.html) · [conventions](https://symfony.com/doc/current/contributing/code/conventions.html) · [bc](https://symfony.com/doc/current/contributing/code/bc.html) · [maintenance](https://symfony.com/doc/current/contributing/code/maintenance.html) |
| [**symfony-docs-contribution**](skills/symfony-docs-contribution/SKILL.md) | You're writing or editing **`.rst`** docs for symfony-docs or a package. reStructuredText format, the writing standards, and the doc PR workflow. | [format](https://symfony.com/doc/current/contributing/documentation/format.html) · [standards](https://symfony.com/doc/current/contributing/documentation/standards.html) |

> 💡 Working on a JS/TS project like [Webpack Encore](https://github.com/symfony/webpack-encore)? These skills don't apply: follow that project's own `CONTRIBUTING`.

## 🚀 Install

Pick your agent. Each one loads the same [`skills/`](skills/) through its own mechanism.

### 🟣 Claude Code

Add the marketplace and enable the plugin in `settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "symfony-contribution-skills": {
      "source": { "source": "github", "repo": "kocal/symfony-contribution-skills" }
    }
  },
  "enabledPlugins": {
    "symfony-contribution-skills@symfony-contribution-skills": true
  }
}
```

Or do it interactively, then reload:

```
/plugin marketplace add kocal/symfony-contribution-skills
/plugin install symfony-contribution-skills@symfony-contribution-skills
/reload-plugins
```

### 🟢 OpenAI Codex

Codex reads the very same `skills/<name>/SKILL.md` format. Register the marketplace, then install from the plugin browser:

```
codex plugin marketplace add kocal/symfony-contribution-skills
```

```
/plugins   # open the browser, install "symfony-contribution-skills", then start a new session
```

Want zero setup? Codex also reads [`AGENTS.md`](AGENTS.md). Append the highlights to your global file and every project inherits them:

```bash
curl -fsSL https://raw.githubusercontent.com/kocal/symfony-contribution-skills/main/AGENTS.md >> ~/.codex/AGENTS.md
```

### 🔵 Gemini CLI

Install the repo as an extension. Gemini loads [`GEMINI.md`](GEMINI.md) into context automatically:

```bash
gemini extensions install https://github.com/kocal/symfony-contribution-skills
```

Manage it anytime with `/extensions`.

## 🔄 Keep the skills fresh

The skills are a **distillation** of the Symfony docs, not a verbatim copy, so upstream can drift. [`bin/update`](bin/update) keeps that visible instead of silent:

```bash
bin/update                 # fetch the referent .rst into references/, show what changed upstream
bin/update --regenerate    # rewrite the SKILL.md files + AGENTS/GEMINI cheat-sheets via `claude -p`
bin/update --branch 8.2    # target another symfony-docs branch (default: 8.1)
```

The default run is deterministic: it only downloads the `.rst` files and prints the diff, nothing gets rewritten. Regeneration is a separate, explicit step that uses the `claude` CLI to rewrite the two skills from the updated `.rst`, then rebuilds the `AGENTS.md` and `GEMINI.md` cheat-sheets from those skills so every agent stays in sync. Since that step is LLM-based, review it with `git diff` before committing. When Symfony opens a newer stable branch, bump `BRANCH` in the script or pass `--branch`.

## 🗂️ Layout

```
.
├── .claude-plugin/           # Claude Code manifests
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/            # OpenAI Codex manifest
│   └── plugin.json
├── gemini-extension.json     # Gemini CLI extension manifest (contextFileName -> GEMINI.md)
├── AGENTS.md                 # cross-agent cheat-sheet (Codex + generic agents)
├── GEMINI.md                 # cross-agent cheat-sheet (Gemini context file)
├── llms.txt                  # index of skills + sources (llms.txt convention)
├── skills/
│   ├── symfony-code-contribution/
│   │   └── SKILL.md
│   └── symfony-docs-contribution/
│       └── SKILL.md
├── references/               # vendored upstream .rst, baseline for bin/update diffs
│   ├── code/
│   │   ├── bc.rst
│   │   ├── conventions.rst
│   │   ├── maintenance.rst
│   │   └── standards.rst
│   └── documentation/
│       ├── format.rst
│       └── standards.rst
├── bin/
│   └── update                # refresh references/ and optionally regenerate the skills
├── README.md
└── LICENSE
```

Every agent points at the same two `SKILL.md` files, so there's one source of truth to maintain. 🎯

## 🙌 Credits

- Built around the [Symfony contribution guidelines](https://symfony.com/doc/current/contributing/index.html) 💚
- Inspired by [`smnandre/symfony-ux-skills`](https://github.com/smnandre/symfony-ux-skills), skills for writing Symfony UX code by Simon Andre 

## 📄 License

[MIT](LICENSE) for the plugin. The Symfony documentation these skills distill is licensed [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/).

---
name: symfony-docs-contribution
description: reStructuredText format, writing standards and PR workflow for contributing to the Symfony documentation (symfony-docs). Use when writing or editing .rst docs for Symfony or its bundles/UX/AI packages.
---

## When to Activate

Use when writing or editing **reStructuredText** (`.rst`) documentation for `symfony/symfony-docs` or for a Symfony package's own docs (bundles, UX, AI, ...). Covers RST syntax, the documentation writing standards, and the doc PR workflow.

For PHP code changes, use the `symfony-code-contribution` skill instead.

## Core Rules

1. **US English, second person, gender-neutral.** Write American English (Merriam-Webster), address the reader as "you" (never "we"), use they/them rather than he/she.
2. **RST, not Markdown.** Double backticks for inline code, list markers flush to the line start, `.. code-block:: <lang>` for code. Single backticks and indented lists are Markdown habits — don't.
3. **Wrap prose at ~80 chars**, code examples at ~85. Define hyperlinks at the bottom of the page — no inline link targets.
4. **Title Case headings** with the correct underline character per level (see below).
5. **Realistic code examples** that follow the Symfony coding standards and best practices — no `foo`/`bar`; use `Acme` as the vendor and `example.com`/`.org`/`.net` as domains.
6. **Target the right branch** and keep one topic per PR. Docs are licensed CC BY-SA 3.0.

## reStructuredText Format

**Headers** — underline (and for level 1, overline) with the text's exact width:

| Level | Char | Placement |
|---|---|---|
| 1 | `=` | over **and** under |
| 2 | `-` | under |
| 3 | `~` | under |
| 4 | `.` | under |
| 5 | `"` | under |

**Lists** start at column 0 (no indentation). `-` or `*` for unordered, numbers for ordered.

**Code blocks** — `.. code-block:: <lang>` then an indented block; default language is PHP. Common languages: `php`, `yaml`, `xml`, `twig`, `html+twig`, `html+php`, `bash`, `terminal`, `json`, `diff`, `text`, `env`, `php-symfony`, `php-standalone`, `php-attributes`.

```rst
.. code-block:: yaml

    framework:
        secret: '%env(APP_SECRET)%'
```

**Multi-format config** — show a config in several formats with `.. configuration-block::` wrapping nested `.. code-block::` directives. Use `.. tabs::` / `.. tab:: Name` for tabbed content (e.g. install methods).

**Directives:** `.. note::`, `.. warning::`, `.. versionadded:: X.Y`, `.. deprecated:: X.Y`, and `.. _target-name:` to define a cross-reference anchor. Directive content must be indented.

**Inline roles:**
- Inline code: `` ``double backticks`` ``  (never single).
- Emphasis: `*italic*`, `**bold**` — don't span markup across lines.
- Internal page: `` :doc:`/path/to/page` `` or `` :doc:`Custom text </path/to/page>` `` (absolute path, no `.rst`).
- Anchor: `` :ref:`Text <target-name>` ``.
- Symfony API: `` :class:`Symfony\\Component\\...\\ClassName` ``, `` :method:`Symfony\\...\\ClassName::method` ``.
- Native PHP: `` :phpclass:`SimpleXMLElement` ``, `` :phpmethod:`DateTime::createFromFormat` ``, `` :phpfunction:`iterator_to_array` ``.

## Writing Standards

- **Avoid these words:** basically, clearly, easy/easily, just, logically, merely, obviously, of course, quick/quickly, simply, trivial.
- Contractions are fine (`you'd`, `it's`). Do **not** use serial (Oxford) commas.
- **Titles use Title Case:** capitalize the first word and every word except closed-class words (articles, prepositions, conjunctions) — "The Vitamins are in my Fresh California Raisins".
- Keep bold/italic within a single line.
- Use `::` PHP shorthand inline, unless it would be alone on its line — then use a `.. code-block:: php`.

**Config format order** (show the supported formats in this order):

| Context | Order |
|---|---|
| Configuration / Services | YAML, PHP |
| Routing | Attributes, YAML, PHP |
| Validation | Attributes, YAML, XML, PHP |
| Doctrine Mapping | Attributes, YAML, XML, PHP |
| Translation | XML, YAML, PHP |

- YAML: add a space after `{` and before `}` (`{ _controller: ... }`) — but **not** in Twig templates.

**Code examples:**
- Start with a filename comment (`// config/services.php`); no blank line after it unless the next line is also a comment.
- Fold whole lines with a language comment: `// ...` (PHP), `# ...` (YAML/bash), `{# ... #}` (Twig), `<!-- ... -->` (XML/HTML), `; ...` (INI). Fold part of a line with a bare `...`.
- Prefix every bash line with `$`.
- Reference directories with a trailing slash (`bin/`), file extensions with a leading dot (`.xml`), and the project root as `your-project/`.

**Images:** create/edit diagrams with Dia. Alt text: concise, capital first letter, trailing period, no "A screenshot of" / "Diagram of" preamble unless the type matters.

## Workflow

1. **Typo / small reword:** use the "Edit this page" button on symfony.com to edit the `.rst` directly on GitHub.
2. **Larger change:** fork `symfony/symfony-docs`, add the upstream remote, and branch from the right base — bug/inaccuracy fix -> oldest maintained branch that has it (merges up); new-feature docs -> the branch of the version that introduces it. Name it `improve_xxx` or `fix_xxx`.
3. Edit the `.rst`, following the format and standards above.
4. Push to **your fork**; open the PR against that base branch with a clear description. Open a **draft** PR if it's still WIP; validate large restructurings with the docs team via an issue first. Keep unrelated changes in separate PRs.
5. Fix what the automated checks (syntax, broken links, typos) report; address reviewer suggestions on the same branch.

## Reference

Canonical sources in `symfony/symfony-docs`:
- `contributing/doc.rst`
- `contributing/documentation/format.rst`
- `contributing/documentation/standards.rst`

## Examples

| Bad | Good |
|---|---|
| `` `config.yaml` `` (single backticks) | `` ``config.yaml`` `` (double) |
| "We can simply configure the service" | "You can configure the service" |
| "This is obviously the easiest way" | "This is the recommended way" |
| heading underline shorter than the title | underline matches the title width exactly |
| ``[link](https://ex.com)`` (Markdown link) | `` `text <target>`_ `` with the target defined at the page bottom |
| `foo`/`bar` sample classes | realistic names, `Acme` vendor, `example.com` domains |
| indented bullet list | list markers flush at column 0 |

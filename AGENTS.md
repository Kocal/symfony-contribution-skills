# Contributing to Symfony

This project provides agent skills for contributing to Symfony. Two skills help you follow the project's official rules when you open a pull request.

## Which skill to use

- **Writing or reviewing PHP** for Symfony core, a bundle, UX (PHP side), AI, etc. -> use the `symfony-code-contribution` skill
- **Writing or editing `.rst` documentation** for symfony-docs or a package's docs -> use the `symfony-docs-contribution` skill
- **A JS/TS project like Webpack Encore** -> these skills don't apply; follow that project's own `CONTRIBUTING`

## Key rules (code)

- Run PHP CS Fixer before committing: `php ./vendor/bin/php-cs-fixer fix -v`. Don't hand-format against it.
- Never break the backward-compatibility promise on a maintenance/minor branch: deprecate, never remove or change public/protected signatures.
- Target the right branch: bug fixes -> oldest maintained branch that has the bug; features and deprecations -> current development branch.
- Ship every deprecation complete in one PR: `trigger_deprecation()` + `@deprecated` PHPDoc + `CHANGELOG.md` + `UPGRADE-*.md`.
- Build exception messages with `sprintf()`, capital first letter and trailing period, `get_debug_type()` for class names, double quotes (not backticks) around technical elements.
- On a patch branch, fix the bug and nothing else: no new features, classes, config options, or deprecations.
- Naming: `camelCase` members, `UpperCamelCase` types, `SCREAMING_SNAKE_CASE` constants, `snake_case` config/routes/Twig.

## Key rules (docs)

- American English, second person ("you"), gender-neutral (they/them).
- reStructuredText, not Markdown: double backticks for inline code, list markers flush at column 0.
- `.. code-block:: <lang>` for code; heading underline characters by level: `=` `-` `~` `.` `"`.
- Wrap prose at ~80 characters; define hyperlinks at the bottom of the page, not inline.
- Avoid these words: basically, clearly, easy/easily, just, obviously, of course, simply, trivial.
- Realistic examples: `Acme` vendor, `example.com` domains, no `foo`/`bar`.

For the full rules, read the matching `skills/<name>/SKILL.md`.

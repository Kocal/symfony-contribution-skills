# Contributing to Symfony

This project provides agent skills that help your Symfony pull requests follow the project's official rules.

## Which skill to use

- **Writing or reviewing PHP** for Symfony core or a Symfony-maintained package (bundles, UX PHP side, AI, ...) -> use the `symfony-code-contribution` skill
- **Writing or editing `.rst` documentation** for symfony-docs or a package's own docs -> use the `symfony-docs-contribution` skill
- **A JS/TS project** (Webpack Encore, Stimulus/UX frontend assets) -> these skills don't apply; follow that project's own `CONTRIBUTING`

## Key rules (code)

- Run PHP CS Fixer before every commit: `php ./vendor/bin/php-cs-fixer fix -v`. Don't hand-format against it.
- Never break the backward-compatibility promise on a maintenance/minor branch: deprecate, never remove or change public/protected signatures.
- Target the right branch: bug fixes -> oldest maintained branch that has the bug; features and deprecations -> current development branch.
- Ship every deprecation complete in one PR: `trigger_deprecation()` + `@deprecated` PHPDoc + `CHANGELOG.md` + `UPGRADE-*.md`.
- To add an argument to a public method in a minor, comment it out in the signature and read it with `func_get_arg()`; never add it as a real parameter.
- Announce a future `final` with `@final since Symfony x.y`; a plain `@final` or native `final` added in a minor is a BC break.
- Never rename a parameter covered by `@arguments-by-name`, `@optional-arguments-by-name` or `@stable-parameter-names`.
- On a patch branch, fix the bug and nothing else: no new features, classes, config options, deprecations or exception message changes.
- Use `===`/`!==` and Yoda conditions: `if (null === $value)`.
- Build exception messages with `sprintf()`, capital first letter and trailing period, `get_debug_type()` for class names, double quotes (not backticks) around technical elements.
- Naming: `camelCase` members, `UpperCamelCase` types, `SCREAMING_SNAKE_CASE` constants, `snake_case` config/routes/Twig.

## Key rules (docs)

- American English, second person ("you", never "we"), gender-neutral (they/them).
- reStructuredText, not Markdown: double backticks for inline code, list markers flush at column 0.
- `.. code-block:: <lang>` for code (or the `::` shorthand for PHP); `.. configuration-block::` for multi-format config.
- Title Case headings; underline characters by level: `=` `-` `~` `.` `"`, matching the title width exactly.
- Wrap prose at ~80 characters; define hyperlinks at the bottom of the page, not inline.
- Use absolute `:doc:` paths (`` :doc:`/controller` ``), never relative ones.
- Mark changes with `.. versionadded:: X.Y` or `.. deprecated:: X.Y`.
- Avoid these words: basically, clearly, easy/easily, just, logically, merely, obviously, of course, quick/quickly, simply, trivial.
- Realistic examples: `Acme` vendor, `example.com` domains, no `foo`/`bar`.

For the full rules, read the matching `skills/<name>/SKILL.md`.

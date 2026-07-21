---
name: symfony-code-contribution
description: PHP coding standards, naming conventions, deprecations and the backward-compatibility promise for contributing code to Symfony (core, bundles, UX, AI). Use when writing or reviewing PHP for a Symfony pull request.
---

## When to Activate

Use when writing, modifying, or reviewing **PHP** code for a contribution to Symfony core or any Symfony-maintained PHP package (bundles, UX PHP side, AI, Mailer, Messenger, Recipes' PHP, etc.).

Do **not** apply these rules to JavaScript/TypeScript projects such as **Webpack Encore** or the Stimulus/UX frontend assets — follow that project's own `CONTRIBUTING` and standards instead. For documentation (`.rst`) changes, use the `symfony-docs-contribution` skill.

## Core Rules

1. **Run PHP CS Fixer before every commit.** Most style below is auto-enforced by the project's `.php-cs-fixer.dist.php` — never hand-format against it: `php ./vendor/bin/php-cs-fixer fix -v`.
2. **Never break the backward-compatibility promise on a maintenance/minor branch.** Public and protected APIs are frozen: deprecate, never remove or change signatures. Breaking changes land only on the next major branch.
3. **Target the right branch.** Bug fixes -> oldest maintained branch that still has the bug (it merges up automatically). New features and deprecations -> current development branch. Never add a feature on a patch branch.
4. **Ship every deprecation complete, in the same PR:** a `trigger_deprecation()` call, an `@deprecated` PHPDoc tag, a `CHANGELOG.md` entry, and the `UPGRADE-*.md` entries.
5. **Add the MIT license header** at the top of every new PHP file, before the `namespace`.
6. **One class per file. Add tests.** Do not write PHPDoc that only restates the signature.

## Coding Standards

The rules below are what PHP CS Fixer enforces and what reviewers check.

**Formatting**
- One space after each comma.
- One space around binary operators (`==`, `&&`, `||`, ...) **except** concatenation (`.`).
- Unary operators (`!`, `--`, `++`) stick to their variable, no space.
- No spaces around `[` / `]` in array access: `$a[0]`, not `$a [0]`.
- Multi-line arrays: trailing comma after **every** item, including the last.

**Control flow & returns**
- Always brace control-structure bodies, even single statements.
- Blank line before `return`, unless the `return` is alone inside a statement group (e.g. an `if`).
- No `else`/`elseif`/`break` after an `if`/`case` branch that already returns or throws.
- `return null;` when returning null; bare `return;` for void. Do **not** add a `void` return type in tests.

**Comparisons**
- Identical comparisons (`===` / `!==`) unless you explicitly need type juggling.
- Yoda conditions when comparing a variable to an expression: `if (null === $value)`.

**Class layout**
- Declare inheritance and all implemented interfaces on the same line as the class name.
- Properties before methods. Methods ordered public -> protected -> private, except constructors, `setUp()` and `tearDown()` which come first.
- Use parentheses when instantiating, regardless of argument count: `new Foo()`.
- All method/function arguments on the same line as the name — **except** constructor property promotion, where each parameter goes on its own line with a trailing comma.

**Types**
- Use `bool`, `int`, `float` (never `boolean`/`integer`/`double`/`real`).
- In PHPDoc `@param`/`@return` type lists, put `null` **last**: `string|null`.

**PHPDoc**
- Add a block only when it adds information the name/native types/context don't already give.
- No one-line PHPDoc blocks, even for a single tag.
- Omit `@return` when the method returns nothing.
- Group annotations: same type together, a single blank line between different types.

**Exception & error messages**
- Build with `sprintf()`, not raw concatenation.
- Capital first letter, trailing period.
- `get_debug_type($x)` for class names in messages, not `$x::class`.
- Double quotes around technical elements — `The "foo" option ...` — not backticks.

## Naming

| Element | Case |
|---|---|
| variables, functions, methods | `camelCase` |
| classes, interfaces, traits, enums | `UpperCamelCase` |
| enum cases | `UpperCamelCase` |
| constants | `SCREAMING_SNAKE_CASE` |
| config params, route names, Twig vars | `snake_case` |
| PHP files | `UpperCamelCase.php` |
| templates & web assets | `snake_case` |

- Prefix abstract classes with `Abstract` (except PHPUnit `*TestCase`). Suffix `*Interface`, `*Trait`, `*Exception`.
- Service-config attributes prefixed `As` (`#[AsCommand]`, `#[AsEventListener]`); controller-argument attributes prefixed `Map` (`#[MapEntity]`, `#[MapCurrentUser]`).
- Primary service id = the fully-qualified class name; add public aliases; parameter names lowercase.
- Command and option names use the English imperative: `run`, `list` (not `runs`, `lists`).

**Method naming for collections.** When a class has one clear "main" relation, use: `get` `set` `has` `all` `replace` `remove` `clear` `isEmpty` `add` `register` `count` `keys`. For secondary relations, suffix with the thing: `getXxx` `setXxx` `hasXxx` `getXxxs` `removeXxx` `addXxx` `countXxx` ... Note `setXxx()` may add or replace; `replaceXxx()` must **not** add and throws on an unknown key.

## Deprecations

- PHPDoc tag: `@deprecated since Symfony X.Y, use Bar instead.` — state the version and the replacement (FQCN if in another namespace).
- Runtime trigger (needs `symfony/deprecation-contracts`):
  ```php
  trigger_deprecation('symfony/package', '7.3', 'The "%s" class is deprecated, use "%s" instead.', Foo::class, Bar::class);
  ```
  For a deprecated class, place the call after the `use` block, before the class definition.
- Document in the **same PR**: `CHANGELOG.md` (component root), `UPGRADE-X.Y.md` (this minor), `UPGRADE-X.0.md` (next major, removal/replacement consequences).
- Only deprecate on the next minor; never introduce something already deprecated; removals happen only on the next major.

## Backward Compatibility

Minor releases keep BC; only majors may break it. When unsure, keep the old API and deprecate.

**Never, on public/protected API:** remove or rename a method/property/constant; add or remove an argument; change a signature, type hint or return type; reduce visibility; flip static/non-static; add a mandatory constructor argument.

**Allowed:** add new methods/properties/constants; add a constructor argument **with a default, at the end**; add a default to an existing argument; rename an argument if behavior is unchanged; anything on `private` members; anything on `@internal`, `@experimental`, or `*\Tests\` code.

**Interfaces are strictest:** you may add a parent interface that introduces no method, and add constants — nothing else. Evolve an interface method by shipping a new interface, or via the recipe below.

**Adding a new argument to a public method (2-step, BC-safe):**
```php
// Minor N — argument commented, read defensively, deprecate when absent
public function say(string $text /* , bool $trim = true */): void
{
    $trim = 2 <= \func_num_args() ? func_get_arg(1) : false;
    if (\func_num_args() < 2) {
        trigger_deprecation('symfony/pkg', '7.3', 'Not passing the "bool $trim" argument is deprecated; its default will be true in 8.0.');
    }
    // ...
}
// Major N+1 — uncomment the real parameter, drop the func_get_arg/deprecation code
```

Mark future-final classes/methods with the `@final` PHPDoc tag one release before enforcing `final`.

## Patch-Version Policy (Maintenance)

Patch releases of a maintained minor ship monthly and accept only tightly-scoped changes. When targeting a maintained branch, fix the bug and nothing else.

**Accepted in a patch:** bug fixes that keep existing tests green and add a covering test; support for newer PHP/OS versions (never new PHP *features*); translation updates (always to the oldest maintained branch); external-data refreshes (e.g. ICU); raising a dependency's *minimum* version; tests that raise coverage.

**Not accepted — do it on the next minor/major:** new features; new classes or public/protected methods; new config options; new deprecations (none after a version is stable); performance work (unless local to one class and backed by real-world numbers); coding-standard/refactor churn; adding/updating annotations (fixing wrong ones may pass); changing exception messages (automated tools rely on them); new Composer deps or support for their new majors; security hardening; BC breaks (except when unavoidable to fix a security issue); web-design changes to built-in pages (profiler, toolbar, error pages).

When docs or PHPDoc disagree with the code, the code is authoritative.

## Workflow

1. Fork the canonical repo (`symfony/symfony` or the target package) to your account; clone; `git remote add upstream <canonical>`.
2. Branch from the correct base (Core Rule 3): `git checkout -b my_change upstream/<branch>`.
3. Write the change **and** tests; keep one topic per PR.
4. If deprecating: add `trigger_deprecation` + `@deprecated` + `CHANGELOG.md` + `UPGRADE-*.md`.
5. `php ./vendor/bin/php-cs-fixer fix -v` and run the component tests (`./phpunit src/Symfony/Component/Xxx`).
6. Commit; push to **your fork**; open the PR against the base branch and fill the PR table (branch, bug fix?, new feature?, deprecations?, BC breaks?, tickets, license).
7. Address CI and review; push follow-ups to the same branch.

## Reference

Canonical sources live in `symfony/symfony-docs` and stay in sync — read the `.rst` when a case is unclear:
- `contributing/code/standards.rst`
- `contributing/code/conventions.rst`
- `contributing/code/bc.rst`
- `contributing/code/maintenance.rst`

## Examples

| Bad | Good |
|---|---|
| `if ($value == null)` | `if (null === $value)` |
| `throw new \RuntimeException('Invalid '.$type.' given')` | `throw new \RuntimeException(sprintf('Invalid "%s" given.', $type))` |
| ``throw new \LogicException('The `debug` option ...')`` | `throw new \LogicException('The "debug" option ...')` |
| `$class = $object::class;` (in a message) | `$class = get_debug_type($object);` |
| removing a public method in 7.x | `@deprecated` it in 7.x, remove in 8.0 |
| adding `bool $strict` as a real param in a minor | comment it, read via `func_get_arg()`, deprecate when absent |
| `public function all() { ... }` renamed to `getAll()` | keep `all()`; it is the standard main-relation name |

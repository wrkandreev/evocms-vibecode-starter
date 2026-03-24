# Raw Project Custom Code Map

> Important: this document describes a repeated Evo project inspection pattern.
> Before applying it, verify the real implementation on the live project.

## Goal

- Quickly separate project owned code from Evo core, package code, cache, and other noise when inspecting raw project snapshots.

## Usually Project Owned

- `core/custom/packages/Main/src/**`
- `views/**`
- active frontend asset directories linked from views, often `html/**`
- `assets/modules/clientsettings/config/*.php`
- `assets/tvs/multitv/configs/*.config.inc.php`
- project specific selector files such as `assets/tvs/selector/lib/*.controller.class.php`
- project specific selector config files such as `assets/tvs/selector/lib/*.config.inc.php`
- project `ajax/**`, `cron/**`, and similar custom entry points

## Usually Package Or Core

- `core/storage/**`
- `core/composer/cache/**`
- `core/functions/**`
- `core/factory/**`
- `core/bootstrap.php`
- package internals under `assets/modules/*/core/**`
- package internals under `assets/plugins/*` unless project specific overrides are confirmed
- bundled assets such as `assets/lib/**`
- base selector files such as `assets/tvs/selector/lib/controller.class.php` and `assets/tvs/selector/lib/selector.class.php`

## Fast Heuristics

- If code lives under `core/custom/packages/Main/src/`, treat it as a primary custom code candidate.
- If a Blade layout links `html/css/*`, `html/js/*`, `html/img/*`, or `html/fonts/*`, treat `html/` as active project source.
- If a file defines `ClientSettings` tabs, `MultiTV` fields, or selector rules, treat it as deployable project code.
- If a file is generated cache, compiled Blade, vendor cache, or package boilerplate, do not use it as a reusable project example.

## Repeated Raw Project Pattern

- Several raw projects store shared logic in `core/custom/packages/Main/src/Helper.php` and `core/custom/packages/Main/src/Controllers/*`.
- Several raw projects store `ClientSettings` tabs in `assets/modules/clientsettings/config/*.php`.
- Several raw projects store `MultiTV` schemas in `assets/tvs/multitv/configs/*.config.inc.php`.
- Several raw projects store selector field rules in `assets/tvs/selector/lib/*.controller.class.php`.
- Several raw projects render through `views/**` but load frontend assets from `html/**`.

## Working Rule

- When mining raw projects for local examples, start with the custom code candidates above.
- Do not build repository examples from cache, core internals, or package boilerplate when a project specific example exists.

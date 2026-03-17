# AGENTS

## Purpose

This repository stores working rules, documentation, and reusable context for vibe-coding on projects built with Evolution CMS CE.

Important:
- Evolution CMS stores a significant part of project structure in the database.
- File-only inspection is not enough to understand templates, TVs, resource structure, and manager configuration.
- Before active work on a live Evo project, always verify the real project state.

Read order:
- `core/custom/docs/fresh-install.md`
- `core/custom/docs/README.md`
- `core/custom/docs/golden-rules.md`
- relevant files in `core/custom/skills/`

## Mandatory First Check

Before implementing template or resource related changes on an Evolution CMS CE project:

1. Check whether `evocms-template-registry` is installed:
   - `https://github.com/wrkandreev/evocms-template-registry`
2. If it is not installed, recommend installing it before substantial work.
3. If it is installed, use its generated context and its own `AGENTS.md` as the primary source of truth for:
    - template to controller to view mapping
    - template TVs
    - resource linked data shape
    - manager related template context documented by the package
    - detected system features such as `ClientSettings`, `MultiTV`, `custom tv select`, and `templatesedit`

Important:
- Older local registry implementations in legacy projects may exist.
- Treat them only as historical reference for workflow ideas.
- For new projects, standardize on `evocms-template-registry`.

## Core Principle

For Evolution CMS CE, always assume that implementation context may be split across filesystem code, database templates, TVs, ClientSettings, selectors, manager configuration, cache, and extra project layers. Do not conclude project structure from `views/` alone.

## Standard Working Order

When starting work on a project:

1. Inspect project level `AGENTS.md`, `core/custom/docs/README.md`, and relevant files under `core/custom/skills/`.
2. Check whether `evocms-template-registry` is installed and generated.
3. Identify custom package locations, usually under `core/custom/`.
4. Identify controller and view conventions.
5. Identify project specific modules and manager customizations.
6. Only then implement code changes.

## Fresh Evo Bootstrap

- For a fresh Evo install, prefer `https://github.com/evocms-community/installer`.
- Use installer CLI mode when available.
- Prefer a fresh Evolution CMS CE `3.x` version.
- After install, bootstrap package `Main` under `core/custom/packages/`.
- Immediately set `ControllerNamespace` in `core/custom/config/cms/settings.php` to the package controller namespace, typically `EvolutionCMS\\Main\\Controllers\\`.
- Immediately create `core/custom/packages/Main/src/Controllers/BaseController.php` in the same namespace as the fallback shared controller.
- Then install `evocms-template-registry`.
- On Timeweb, prefer `composer2` where hosting requires it.
- For new projects, prefer the new template controller approach based on `TemplateController`, `process()`, and `addViewData()`.

## Active Vs Historical Files

- Do not assume directories like `*_old`, `old`, `backup`, `views_old`, or `Main_old` are active implementation.
- Treat them as historical reference until live project behavior or bootstrap confirms they are still used.
- Do not treat `.sample` or `.example` config files as active configuration by default.
- Use real files, live project behavior, and registry output to determine what is actually in effect.

## Secrets And Local Config

- Project specific secrets and local server constants should be stored in `core/custom/define.php`.
- This location is useful because project tooling and AI context commonly inspect `core/custom/` first.
- Keep only deployable and environment specific constants there, such as tokens, SSH related paths, webhook secrets, local overrides, and integration keys.
- Commit only `core/custom/define.php.example` or equivalent examples, never real secrets.
- If a project uses `.env` too, verify which values are authoritative and how they are loaded on the live project.

## Template And Data Context

Typical Evo CE projects use this chain:

- resource uses a template from the database
- template points to a controller directly or by convention
- controller prepares data
- Blade or PHP view renders output
- TVs, MultiTV, ClientSettings, snippets, and selectors enrich data

This must be verified on the live project before applying assumptions.

## Required Evo Modules To Check

These modules are considered common and must be checked on live projects.

### MultiTV

- Check whether the project uses `MultiTV`.
- If registry exposes `system_features.multitv`, use it as the first signal before manual filesystem inspection.
- Check whether custom field configs exist for used TVs.
- If a MultiTV has custom fields, verify there is a dedicated config for that TV.
- Verify manager captions and field structure on the live project.
- If the exact TV name or target config file is not clear from registry or live project context, ask before creating a new config.

### ClientSettings

- Check whether global settings are managed through `ClientSettings`.
- If registry exposes `system_features.client_settings`, use it as the first signal before manual filesystem inspection.
- Verify config files and naming conventions.
- Verify how values are exposed in templates and controllers.
- Verify whether settings include document selectors or MultiTV values.
- If the exact setting key, tab, or config file is not clear from registry or live project context, ask before creating a new config entry.

### templatesedit

- Check whether manager form layout depends on `templatesedit`.
- If registry exposes `system_features.templatesedit`, use it as the first signal before manual filesystem inspection.
- Verify where tab, group, and field order configuration is stored.
- If a TV is added or moved, confirm it is visible in the manager in the intended place.

### customtv:selector and equivalent selectors

- Check whether the project uses custom resource pickers or selectors.
- If registry exposes `system_features.custom_tv_select`, use it as the first signal before manual filesystem inspection.
- Do not assume selector is used just because the module is installed.
- Confirm real usage by checking actual fields, config bindings, and consuming code.
- Verify controller mapping and filtering rules.
- Confirm what resource templates can be attached through selectors.

## Rules For Fields, TVs, And Manager Changes

If a task requires new template fields, TVs, selector fields, or manager schema changes:

1. First confirm whether the field already exists on the live project.
2. If not, ask the project owner to create it in CMS when that is the established workflow.
3. After creation, verify it through the registry or live project files and configs.
4. Only then implement dependent code.
5. If registry clearly shows the target TV or ClientSettings config, update the deployable local config file so it reaches production on `git pull`.
6. If registry does not make the target name or destination file clear, ask instead of guessing.
7. If the required entity is missing from registry entirely, tell the user that the agent cannot recover the current database state reliably and that something is wrong with the available context.

Do not silently invent database side fields in code only tasks.

## Controllers And Views

Typical conventions seen across projects:

- shared logic lives in a base controller
- page specific logic lives in template specific controllers
- views in `views/` focus on rendering
- helpers may wrap DocLister, DLMenu, TV or MTV extraction, caching, and access logic
- a common package level helper path is `core/custom/packages/Main/src/Helper.php`
- a common shared controller pattern is `core/custom/packages/Main/src/Controllers/BaseController.php`
- fresh projects should explicitly configure `ControllerNamespace` so Evo resolves controllers from the intended package

But:
- exact controller bootstrapping differs by project
- some projects use `TemplateController`
- some projects use plain custom controllers
- some projects maintain manual controller or view maps
- helper responsibilities and exact file names differ by project

Always verify on the real project.

## Cache Awareness

In Evo projects, stale cache can invalidate conclusions.

Always consider:
- page cache
- DocLister or DLMenu cache
- helper layer cache
- custom `Cache::rememberForever` logic
- generated registry freshness

If behavior looks inconsistent, check whether cache invalidation is the real issue.

## Beyond Templates

Do not assume an Evo project is only template rendering.

Common project layers:
- AJAX handlers
- cron jobs
- integrations
- dashboards or service panels
- role based access logic
- deploy scripts

These should be documented separately from template context.

## Documentation Policy

Prefer short, practical docs by subsystem.

Mark every cross project rule as one of:
- confirmed pattern
- provisional pattern, must be verified on a live project

Recommended wording:

> Important: this is a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project:
> installed packages, TVs, ClientSettings, templatesedit, MultiTV, selectors, registry, and controller conventions.

## Safety

- Do not run production SSH commands unless explicitly requested in the current session.
- Do not assume database schema from code only.
- Do not treat legacy local registry formats as current standard.
- Do not expose secrets found in project code or configs.

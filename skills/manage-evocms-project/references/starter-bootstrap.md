# Evo Vibecode Starter Bootstrap

## Upstream

Use the current `main` branch of:

- `https://github.com/wrkandreev/evocms-vibecode-starter`

This is a context and workflow repository, not an Evolution CMS distribution or site theme. Fetch it into a temporary directory so its files do not become a nested Git repository.

## Required Project Enrichment

For an existing Evo project, bring in or refresh:

- `/AGENTS.md`
- `/core/custom/docs/`
- `/core/custom/examples/`

Do not bring development-only `core/custom/skills/`, `decisions/`, or `plans/` from the `develop` branch into target projects. Do not replace project-owned packages, configs, or other active files under `core/custom/`.

The starter root `README.md` describes the starter itself and is never copied into target projects. Instead, create a short project README from `templates/project-readme.md` in the starter repository and fill it with confirmed project facts.

Append the Project Notes section from `templates/agents-project-notes.md` to the project `AGENTS.md` and fill it with confirmed facts. Remove subsections that do not apply.

When the project starts recording architecture decisions, copy `templates/adr/` into `core/custom/docs/adr/` and keep the index table updated.

Adapt the copied root `AGENTS.md` after inspecting the project. Record only confirmed facts:

- production and repository locations where appropriate;
- active packages, controllers, views, frontend asset directories, and extra entry points;
- installed and actually used manager components;
- secret locations and forbidden tracked files;
- validation commands;
- exact deployment and rollback workflow.

Never place tokens, passwords, private keys, database credentials, or real secret values in `AGENTS.md`.

## Required Reading Order

1. Project `AGENTS.md`
2. `core/custom/docs/README.md`
3. `core/custom/docs/golden-rules.md`
4. The relevant subsystem document
5. The matching example under `core/custom/examples/`
6. Active project implementation and authorized live context

For repository scope, also read:

- `core/custom/docs/gitignore.md`
- `core/custom/docs/raw-project-custom-code.md`
- starter root `.gitignore`

## Component Routing

Use local starter-derived docs and examples before open-ended searching:

| Task | Documentation | Example |
|---|---|---|
| Controllers/views | `architecture.md`, `controllers-views-map.md` | `base-controller.md`, `page-controller.md`, `minimal-template-controller.md` |
| Shared helpers | `controllers-views-map.md` | `helper.md`, `common-trait.md` |
| ClientSettings | `clientsettings.md` | `clientsettings-config.md`, `clientsettings-usage.md`, `clientsettings-selector-config.md` |
| MultiTV | `multitv.md` | `multitv-config.md`, `multitv-tv-specific-config.md`, `multitv-listbox-multiple-config.md` |
| templatesedit | `templatesedit.md` | `templatesedit-config.md` |
| Selector | `selectors.md` | `selector-controller.md`, `selector-fallback-controller.md` |
| bLang/localization | `localization.md` | `blang-localization.md` |
| AJAX/search | `ajax.md` | `ajax-handler.md`, `search-controller.md` |
| Frontend layout | `architecture.md` | `html-frontend-structure.md` |
| Cache | `cache.md` | inspect active cache calls |
| Deploy | `deploy.md` | inspect the confirmed reference implementation |

If a component is not covered locally, use the official package links from `core/custom/docs/official-links.md` before broad web search.

## Refresh Rules

- Compare with upstream before substantial new sessions or when the user asks to enrich/update context.
- Before comparing, read the `Starter Sync` section of the project README. A missing or stale stamp means the copied docs may be outdated.
- After refreshing, update the recorded upstream commit and date in the `Starter Sync` section.
- Merge upstream documentation changes deliberately; preserve project-specific additions.
- Do not assume copied examples are active code.

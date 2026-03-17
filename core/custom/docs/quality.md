# Quality

This document tracks confidence levels for the current knowledge base.

## Confirmed Across Projects

- file only inspection is insufficient for Evo project understanding
- `evocms-template-registry` should be the standard for new projects
- fresh Evo installs should prefer `evocms-community/installer` and a `3.x` version
- `core/custom/` is the main project specific code and config area
- `core/custom/packages/Main/src/Controllers/BaseController.php` is a repeated shared controller pattern
- `core/custom/packages/Main/src/Helper.php` is a repeated shared helper pattern
- `ClientSettings`, `templatesedit`, and `MultiTV` are common modules worth checking first
- `.sample` and `.example` files cannot be treated as active config by default
- `*_old`, `views_old`, `Main_old`, and backup paths cannot be treated as active source of truth by default
- `core/custom/define.php.example` is a practical convention for local secret handling
- deployable manager side config files should stay tracked in git so they reach production on `git pull`

## Confirmed But Variable

- template to controller mapping conventions vary by project
- `BaseController` bootstrapping may happen in `process()`, constructor, or another lifecycle method
- `Helper.php` may be static, instance based, or split into multiple helper classes
- fresh install command details vary by hosting environment and available PHP binaries
- AJAX entry points may live in `ajax/`, root scripts, custom routes, or package level handlers
- selector usage may exist with or without custom controller files depending on project design

## Provisional

- exact controller naming conventions by template alias
- exact view naming conventions across all projects
- standardized route storage for non template endpoints
- canonical format for project specific integrations docs

## Custom Workflow

- browser triggered deploy is a custom vibe-coding workflow, not an Evo standard
- repository local `skills/` are draft playbooks until runtime skill registry is available

## Needs More Live Verification

- cron organization patterns across more projects
- roles and access architecture across more projects
- integrations layout across more projects
- deploy variants beyond current observed workflows

## Current Gaps

- example custom deploy endpoint pattern with clear security constraints
- more examples for selector field configs tied to live manager usage

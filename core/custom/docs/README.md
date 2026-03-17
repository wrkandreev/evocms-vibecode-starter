# Docs Index

This directory is the repository knowledge base for vibe-coding on Evolution CMS CE projects.

Important:
- `AGENTS.md` is the entry point.
- `core/custom/docs/` is the system of record for reusable project knowledge.
- `core/custom/examples/` stores reference code patterns.
- `core/custom/skills/` stores reusable workflow playbooks.
- `core/custom/decisions/` stores short ADRs.
- `core/custom/plans/` stores active and completed execution plans.

## Read First

- `core/custom/docs/golden-rules.md`
- `core/custom/docs/quality.md`
- `core/custom/docs/fresh-install.md`
- `core/custom/docs/template-context.md`
- `core/custom/docs/architecture.md`

## Core Evo Context

- `core/custom/docs/fresh-install.md` - fresh Evo 3 bootstrap workflow, `ControllerNamespace`, and base package setup
- `core/custom/docs/template-context.md` - template, TV, and registry workflow
- `core/custom/docs/architecture.md` - project structure and active vs legacy paths
- `core/custom/docs/controllers-views-map.md` - shared controller and helper conventions
- `core/custom/docs/cache.md` - cache layers and debugging implications
- `core/custom/docs/ajax.md` - AJAX as a separate subsystem
- registry `system_features` helps detect installed manager side modules before manual inspection

## Manager And Field Context

- `core/custom/docs/multitv.md` - MultiTV usage and config verification
- `core/custom/docs/clientsettings.md` - ClientSettings structure and access patterns
- `core/custom/docs/templatesedit.md` - manager layout and TV visibility
- `core/custom/docs/selectors.md` - selector usage checks and verification
- `core/custom/docs/secrets.md` - project secrets and local config placement
- `core/custom/docs/gitignore.md` - repository ignore baseline

## Project Layers Beyond Templates

- `core/custom/docs/integrations.md`
- `core/custom/docs/roles-access.md`
- `core/custom/docs/cron.md`
- `core/custom/docs/deploy.md`
- `core/custom/docs/troubleshooting.md`

## Operational Memory

- `core/custom/docs/quality.md` - confidence map for the knowledge base
- `core/custom/decisions/README.md` - ADR format and index
- `core/custom/plans/README.md` - execution plan workflow

## Examples

- `core/custom/examples/helper.md`
- `core/custom/examples/base-controller.md`
- `core/custom/examples/page-controller.md`
- `core/custom/examples/filtered-list-controller.md`
- `core/custom/examples/ajax-handler.md`
- `core/custom/examples/selector-controller.md`
- `core/custom/examples/clientsettings-usage.md`
- `core/custom/examples/clientsettings-config.md`
- `core/custom/examples/templatesedit-config.md`
- `core/custom/examples/multitv-config.md`
- `core/custom/examples/multitv-tv-specific-config.md`

## Skills

- `core/custom/skills/evo-project-audit.md`
- `core/custom/skills/evo-manager-fields-check.md`
- `core/custom/skills/evo-controller-pattern-review.md`

## Status Labels

- confirmed across projects
- confirmed but variable
- provisional
- custom workflow
- must verify on live project

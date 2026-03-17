# Template Context

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- File only inspection is not enough to understand an Evo project.
- Templates, TVs, and resource structure are stored in the database.
- For new projects, the standard way to restore template context is `evocms-template-registry`:
  - `https://github.com/wrkandreev/evocms-template-registry`

## Standard Workflow

1. Check whether `evocms-template-registry` is installed.
2. If not installed, recommend installing it before substantial template work.
3. If installed, use its generated artifacts and its own `AGENTS.md` as the primary source of truth.

## Registry Failure Rule

- Registry is expected to reveal the target template and manager side entities needed for safe work.
- If the required TV, template context, or `ClientSettings` target is missing from registry, do not guess.
- Tell the user that the agent cannot recover the current database state reliably from the available context and that the live project or registry state must be checked.

## What Registry Should Clarify

- template to controller mapping
- template to view mapping
- TVs bound to templates
- missing or placeholder views
- project specific template conventions
- `system_features` for installed module detection
- the exact TV names that should be matched by local config files when updating MultiTV or related manager side config
- the template and manager context needed to update deployable `ClientSettings` configs safely

## Registry System Features

- Registry may expose a `system_features` block in API output.
- This block is useful as the first machine readable signal for installed project modules.
- Expected feature groups include:
  - `client_settings.installed`
  - `multitv.installed`
  - `custom_tv_select.installed`
  - `templatesedit.installed`
- `details` may include helpful diagnostics such as existing config directories, plugin files, or selector controller counts.
- Preview output may also surface compact feature status lines for quick inspection.

## Working Rule

- If `system_features` is present, use it before falling back to ad hoc filesystem guessing.
- Treat it as a strong signal, then verify the exact active config and runtime usage on the live project.

## What Still Needs Live Verification

- whether registry is fresh
- whether generated data matches current database state
- whether project has custom layers not covered by registry
- whether manager side modules add extra meaning to TVs and resources

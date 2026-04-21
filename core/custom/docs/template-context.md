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
- For tasks that depend on new TVs, `ClientSettings`, selector fields, or manager side bindings, registry must be treated as the primary source of truth.
- If the required TV, template context, or `ClientSettings` target is missing from registry, do not guess.
- If registry API/output is unavailable, tell the user to restore it first before continuing.
- Tell the user that the agent cannot recover the current database state reliably from the available context and that the live project or registry state must be checked.

## What Registry Should Clarify

- template to controller mapping
- template to view mapping
- TVs bound to templates
- missing or placeholder views
- project specific template conventions
- `system_features` for installed module detection
- `bLang` support including active languages, suffixes, field catalog, template links, and resource level localized context when the module is installed
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
  - `blang.installed`
- `details` may include helpful diagnostics such as existing config directories, plugin files, or selector controller counts.
- Preview output may also surface compact feature status lines for quick inspection.

## Registry bLang Support

- `evocms-template-registry` now supports `bLang`.
- Registry may expose a top level `blang` object even when the module is not installed, with valid empty fallback structure.
- When `bLang` is installed, registry can expose languages, suffixes, settings, fields catalog, template links, and resource specific localized context.
- A dedicated endpoint may also expose this data, for example `GET /api/template-registry/blang`.
- Use this registry context before guessing multilingual field names such as `_en` variants.
- For `bLang`, treat registry data derived from `blang_tmplvars` as the authoritative field registration context.
- A pair of TVs created manually in `site_tmplvars`, such as `missionTitle` and `missionTitle_en`, is not enough by itself to confirm a valid `bLang` field pair.
- If registry API supports creating TVs or dictionary entries, prefer that API workflow so `bLang` metadata and real TVs stay synchronized.

Practical `bLang` workflow through registry API:

- inspect current model:
  - `GET /api/template-registry/blang`
  - `GET /api/template-registry/blang/health`
- inspect one resource/template context:
  - `GET /api/template-registry/resource-context?resource_id=...`
- manage dictionary strings:
  - `GET/POST/PATCH/DELETE /api/template-registry/blang/lexicon`
- seed standard localized params:
  - `POST /api/template-registry/blang/default-params`
- create or update actual `bLang` field metadata:
  - `POST/PATCH/DELETE /api/template-registry/blang/fields`
- change active languages/suffixes/settings:
  - `PATCH /api/template-registry/blang/settings`
  - `DELETE /api/template-registry/blang/languages/{language}`
- write localized content for one resource:
  - `PATCH /api/template-registry/resources/{resourceId}/blang-fields`
- repair drift if localized TVs and `bLang` links diverge:
  - `POST /api/template-registry/blang/fix-template-links`

Do not skip the metadata layer:

- manual `_en` TV pairs alone are not a valid `bLang` field registration
- `resource-context -> blang -> template_fields` should be treated as the allow-list for resource-level localized writes

## Working Rule

- If `system_features` is present, use it before falling back to ad hoc filesystem guessing.
- Treat it as a strong signal, then verify the exact active config and runtime usage on the live project.

## Before Changing A Template

- Use live `evocms-template-registry` output as the primary source of truth.
- Verify the template alias before assuming controller naming.
- Verify template to controller and template to view mapping separately.
- Verify the exact list of TVs attached to the template.
- Verify `ClientSettings` fields together with their real `setting_name` values.
- Do not infer `ClientSettings` runtime keys from captions or guessed field names; confirm whether the live project uses clean keys like `phone` or already broken keys like `client_phone`.

## What Still Needs Live Verification

- whether registry is fresh
- whether generated data matches current database state
- whether project has custom layers not covered by registry
- whether manager side modules add extra meaning to TVs and resources

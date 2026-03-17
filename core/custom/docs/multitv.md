# MultiTV

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- `MultiTV` is a common module in Evo projects.
- MultiTV data often requires helper side parsing before use in controllers or views.
- Manager usability depends on field captions and config quality.
- Registry may expose `system_features.multitv` plus config related diagnostics as the first detection layer.

## Strong Working Rule

- If a MultiTV has custom fields, verify there is a dedicated config for that TV.
- Do not rely on a generic default config when the TV has project specific structure.
- Do not assume sample or fallback configs are the active field definition for a specific TV.
- If registry or live project context does not clearly show which TV name should receive the config, ask before creating a new file.

## Registry Driven Update Rule

- When registry or verified live project context clearly identifies the TV and config target, update the local deployable config file in the repository.
- The goal is that the config reaches production by normal `git pull` and becomes visible in manager without manual file copying.
- Do not create a guessed config file name if the TV binding is still uncertain.
- If the expected TV entity is missing from registry, report that the agent cannot restore the real database state from current context and ask the user to verify registry or live project state.

## Why This Matters

- without a dedicated config, manager labels may degrade to generic key or value fields
- developers may misunderstand the shape of stored data
- frontend work may break if assumed field names differ from real ones

## What Must Be Verified On A Live Project

- whether `MultiTV` is installed and used
- where config files are stored
- the real field schema of each important MultiTV
- captions and manager presentation
- whether helper methods transform MultiTV output before rendering
- which config files are active for the TVs that matter

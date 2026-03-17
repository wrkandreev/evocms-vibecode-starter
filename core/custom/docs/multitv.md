# MultiTV

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- `MultiTV` is a common module in Evo projects.
- MultiTV data often requires helper side parsing before use in controllers or views.
- Manager usability depends on field captions and config quality.

## Strong Working Rule

- If a MultiTV has custom fields, verify there is a dedicated config for that TV.
- Do not rely on a generic default config when the TV has project specific structure.
- Do not assume sample or fallback configs are the active field definition for a specific TV.

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

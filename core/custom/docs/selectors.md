# Selectors

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Evo projects may use custom selectors such as `customtv:selector` to bind resources through manager fields.
- Selector behavior is usually controlled by PHP classes or controllers with filtering rules.
- Installed selector module alone does not prove active project usage.
- Registry may expose `system_features.custom_tv_select` plus details such as selector controller counts as the first detection layer.

## Important Clarification

- A project may have `assets/tvs/selector` installed but never actually use selector fields in its live data model.
- Treat selector as active only when there is project level evidence of usage.

## Typical Use Cases

- choosing resources for homepage blocks
- limiting choices to specific template ids
- storing ordered resource relations in manager settings

## Signals Of Real Usage

- fields configured with `customtv:selector`
- custom selector controller files for specific fields
- `ClientSettings` or TV configs that reference selector fields
- frontend or controller code that consumes selected document ids

## What Must Be Verified On A Live Project

- whether selector fields are used at all
- where selector controller files are stored
- how field names map to controllers
- what filters define allowed resources
- how selected values are consumed in frontend code

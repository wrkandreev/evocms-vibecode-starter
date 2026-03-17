# ClientSettings

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- `ClientSettings` is a common way to store project wide settings.
- These settings may provide text, links, images, resource ids, selector values, or MultiTV like structures.
- Templates and controllers may read them directly or through helper wrappers.
- Registry may expose `system_features.client_settings` plus diagnostic details as the first detection layer.

## Typical Responsibilities

- global site text and labels
- contact data
- shared blocks for the homepage or footer
- links to resources chosen in manager

## Important Clarification

- `.sample` and `.example` files near `ClientSettings` configs are reference files, not active settings by default.
- Active project behavior must be verified against the real config files and the live manager state.

## Registry Driven Update Rule

- When registry or verified live project context clearly identifies the target `ClientSettings` tab or config file, update that local deployable file in the repository.
- The goal is that the changed config lands on production through normal `git pull` and appears in manager in the right place.
- If the exact key name, tab, or destination file is still unclear, ask before creating a new config entry.
- If the expected `ClientSettings` entity is missing from registry, report that the agent cannot restore the real database state from current context and ask the user to verify registry or live project state.

## What Must Be Verified On A Live Project

- whether `ClientSettings` is installed
- where config files are located
- field naming conventions
- how values are accessed in code and templates
- whether fields contain plain values, documents, or structured data
- which config files are real and which are only samples

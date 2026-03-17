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

## What Registry Should Clarify

- template to controller mapping
- template to view mapping
- TVs bound to templates
- missing or placeholder views
- project specific template conventions

## What Still Needs Live Verification

- whether registry is fresh
- whether generated data matches current database state
- whether project has custom layers not covered by registry
- whether manager side modules add extra meaning to TVs and resources

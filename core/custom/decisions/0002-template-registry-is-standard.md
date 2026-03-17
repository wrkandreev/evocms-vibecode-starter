# 0002 Template Registry Is Standard

- Status: accepted
- Date: 2026-03-17

## Context

Evolution CMS CE stores templates, TVs, and resource structure in the database, so file only inspection is not enough.

## Decision

For new projects, standardize on `evocms-template-registry` as the preferred way to restore template context.

## Consequences

- agents should recommend installing registry before substantial template work when it is missing
- older local registry implementations are treated only as historical reference
- project documentation should assume registry first, manual reconstruction second

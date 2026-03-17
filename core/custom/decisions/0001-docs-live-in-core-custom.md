# 0001 Docs Live In Core Custom

- Status: accepted
- Date: 2026-03-17

## Context

Repository knowledge for Evolution CMS CE projects must stay close to project specific code and be easy for agents to discover.

## Decision

Store reusable documentation, examples, and draft skills under `core/custom/`.

## Consequences

- `AGENTS.md` stays short and points into `core/custom/`.
- engineering knowledge stays separate from public facing `assets/`
- project specific context is easier for agents to locate consistently

# 0006 Local Live Examples Are Preferred Over Internet Search

- Status: accepted
- Date: 2026-03-18

## Context

When the repository does not contain enough concrete examples for Evolution CMS CE patterns, the agent may fall back to open internet search and pick outdated, transitional, or unofficial material. This is especially risky for manager side configs, third party modules, and project specific controller patterns. The most reliable examples come from current live projects available locally to the user and from local repository examples derived from those projects.

## Decision

Prefer a local examples first workflow. Expand this repository with more real code examples derived from current live projects so the agent can solve common tasks without broad internet lookup. To support that workflow, the project owner should keep more live projects cloned locally and make them available to the agent for inspection.

## Consequences

- this repository should keep expanding `core/custom/examples/` with practical examples for common Evo tasks and module configs
- examples should be based on current live projects whenever possible, not only on articles or migration notes
- when local examples are missing, the next source should be official package documentation, ideally the official GitHub repository for that exact tool
- broad internet search should not be the default source for Evo implementation patterns
- users who want better agent results should provide local access to more current real projects so examples can be extracted from verified working code

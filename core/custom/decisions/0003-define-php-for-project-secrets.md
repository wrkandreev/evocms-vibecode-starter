# 0003 Define Php For Project Secrets

- Status: accepted
- Date: 2026-03-17

## Context

Project specific secrets and local server constants need a predictable location that is easy to discover without scattering values across application code.

## Decision

Prefer storing project specific secrets and local constants in `core/custom/define.php`, while committing only `core/custom/define.php.example`.

## Consequences

- secrets are kept near project specific code and config
- repository docs can point to one stable location for deploy and integration constants
- real secrets stay out of git when ignore rules are followed

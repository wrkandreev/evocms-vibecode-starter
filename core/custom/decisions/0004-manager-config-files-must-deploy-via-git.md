# 0004 Manager Config Files Must Deploy Via Git

- Status: accepted
- Date: 2026-03-17

## Context

Manager side changes for `MultiTV`, `ClientSettings`, and similar Evo modules often depend on local config files. If those files are missing from git or are guessed incorrectly, production will not receive the intended manager behavior after `git pull`.

## Decision

Keep deployable manager side config files tracked in git and update them locally when registry or verified live project context clearly identifies the correct destination file. If the exact TV name, setting key, tab, or destination file is unclear, ask instead of guessing.

## Consequences

- `MultiTV` config files and `ClientSettings` config files should be updated in repository code, not only changed manually on production
- `.gitignore` should not exclude deployable manager config files
- agents must use registry or verified live project context before creating or editing these files
- agents should ask targeted questions when the exact target name is still uncertain

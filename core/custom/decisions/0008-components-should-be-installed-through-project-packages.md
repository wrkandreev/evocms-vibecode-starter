# 0008 Components Should Be Installed Through Project Packages

- Status: accepted
- Date: 2026-03-24

## Context

Evolution CMS CE components such as snippets, plugins, and modules are often installed in the old native way directly into `assets/`. That approach makes project state harder to reproduce across development, staging, and production because installation steps may remain manual and filesystem state may drift between environments. A package based approach gives the project a more controlled and deployable integration path.

## Decision

Prefer installing project components through a package workflow instead of relying on ad hoc native placement in `assets/` whenever the project controls that integration path. Project code should aim to keep component installation, bootstrap, and deployable config inside the package driven repository workflow so the same setup can be reproduced across development, staging, and production.

## Consequences

- new project integrations should prefer package based installation and bootstrap instead of manual copy based setup in `assets/`
- snippets, plugins, and modules that the project depends on should be evaluated for package friendly installation paths
- repository docs and examples should prefer package first workflows when describing new integrations
- legacy projects may still contain native `assets/` installations, but those should be treated as historical reality rather than the preferred standard

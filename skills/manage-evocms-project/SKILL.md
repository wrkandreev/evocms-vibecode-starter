---
name: manage-evocms-project
description: Bootstrap, audit, modify, validate, and deploy Evolution CMS CE projects using wrkandreev/evocms-vibecode-starter as the required knowledge baseline. Use for Evo CMS repositories and production sites involving code-only Git imports and .gitignore design, AGENTS.md, core/custom docs and examples, packages, controllers, Blade views, templates, TVs, ClientSettings, MultiTV, Selector, bLang, uploads, SSH diagnostics, deploy keys, Gitea signed webhooks, or production verification.
---

# Manage Evolution CMS Project

Use `https://github.com/wrkandreev/evocms-vibecode-starter` as the upstream working standard. Treat the database, filesystem, Git repository, and starter knowledge base as separate sources of truth.

## Bootstrap From The Starter

Read [references/starter-bootstrap.md](references/starter-bootstrap.md) before first contact, repository preparation, `.gitignore` work, or adding agent context to an Evo project.

Do not start by improvising generic Evo rules. Fetch the current starter and use these upstream files:

- root `.gitignore` plus `core/custom/docs/gitignore.md` to build the project `.gitignore`;
- root `AGENTS.md` as the base for a project-specific `AGENTS.md`;
- `core/custom/docs/` as the project knowledge base;
- `core/custom/examples/` as reference implementations for Evo components;
- `core/custom/docs/raw-project-custom-code.md` to separate custom code from Evo/package noise.

Merge knowledge files without overwriting active project code under `core/custom/`. Adapt `AGENTS.md` with confirmed project architecture and deployment facts; never copy secrets into it.

## Start With Context

1. Read every applicable `AGENTS.md` completely.
2. Inspect the repository with `rg --files`, `git status`, `.gitignore`, Composer autoloading, routes, active controllers, views, and manager configs.
3. Read `core/custom/docs/README.md`, `golden-rules.md`, the relevant component document, and the matching file under `core/custom/examples/` before changing code. Treat examples as patterns, not active truth.
4. Identify active and historical paths. Do not infer activity from filenames alone.
5. Look for `evocms-template-registry`. Use it for templates, TVs, bindings, ClientSettings, MultiTV, Selector, and localization context. If absent, inspect authorized live context and never invent database entities or IDs.

## Choose The Correct Layer

- Put project PHP in `core/custom/` and presentation in the verified active view/frontend paths.
- Use the CMS entity that already owns a choice or state. Prefer its manager-visible status/configuration over hardcoded path or ID exclusions.
- Enforce the same rule server-side. UI filtering alone is not authorization or validation.
- Preserve stored paths and files used by historical resources when disabling future selection.
- Keep database content out of Git. Keep deployable manager configuration in Git when the project owns it.
- Use Evo/package APIs for CMS mutations when available. Query production only when the user explicitly authorizes production work.

For repository setup or cleanup, read [references/repository-scope.md](references/repository-scope.md). Always derive `.gitignore` from the current starter baseline and a live project audit; do not paste a remembered universal ignore file.

## Implement Safely

1. Trace the complete flow: source entity -> controller/query -> view value -> form handler -> stored fields -> archive/display behavior.
2. Make the smallest coherent change at the owning layer.
3. Preserve legacy controller conventions unless migration is requested.
4. Do not expose secrets, private keys, connection details, webhook tokens, or production config contents.
5. Do not delete uploads merely because Git stops tracking them. Verify ignored server files remain physically present.
6. Make production-side mutations idempotent when practical: inspect exact targets, mutate only the confirmed records, then read them back.

## Validate

- Run `php -l` for each changed PHP file.
- Run the project's relevant tests or focused assertions.
- Run `git diff --check`; account for an established CRLF policy without creating whole-file churn.
- Inspect the complete diff and `git status` before committing.
- Confirm ignored uploads and secrets are not staged.
- For CMS-driven lists, verify the real query result, not only the presence of a file.
- After production changes, verify both desired state and preservation requirements.

## Deploy

Read [references/production-deploy.md](references/production-deploy.md) whenever the task includes production, SSH, deploy keys, Gitea, webhooks, or rollback.

Default to Git deployment: commit the exact project files, push the production branch, let the configured webhook run `git pull --ff-only`, then verify commit parity and a clean production worktree when authorized.

## Leave Operational Memory

Update the repository's `AGENTS.md` and deployment documentation when discovering stable project-specific facts. Record paths, architecture, validation commands, deploy mechanism, and safety constraints, but never secrets. Keep reusable general guidance in this skill and project-specific facts in the project.

---
name: manage-evocms-project
description: Use when working on an Evolution CMS (Evo) CE 3.x repository or production site - bootstrapping a project from evocms-vibecode-starter, importing a live site into a code-only Git repository, designing the .gitignore, adding agent context (AGENTS.md, docs, examples), working with templates, TVs, MultiTV, ClientSettings, Selector, templatesedit, or bLang, writing controllers and Blade views, using the template registry API, debugging stale output after deploy, or deploying to production through a signed Gitea webhook.
---

# Manage Evolution CMS Project

## Overview

End-to-end workflow for vibe-coding on Evolution CMS CE projects with AI agents: recover project context, make safe changes, validate, and deploy.

Evolution CMS stores templates, TVs, resource structure, and much manager configuration in the database. File inspection alone cannot reconstruct that state. Treat these as separate sources of truth:

| Source | Trust Level |
|---|---|
| `evocms-template-registry` when installed | primary for template, TV, and resource context |
| live project behavior and authorized production checks | decisive when sources disagree |
| filesystem code | how data is processed, not which data exists |
| Git history and tracked manager configs | what is deployable |
| starter knowledge base (`core/custom/docs/`, `core/custom/examples/`) | reusable patterns, not project truth |

## When To Use

- bootstrap a fresh Evo 3.x project or enrich an existing one with starter context
- import a live Evo site into a code-only Git repository
- add or change controllers, views, TVs, MultiTV configs, ClientSettings, selectors, templatesedit layouts, or bLang fields
- debug behavior that does not match the code, especially after deploy
- deploy to production or verify a production change

## Hard Rules

1. Never invent database-side state: template ids, TV names, bindings, ClientSettings keys, selector configs. Recover them through the registry or authorized live checks. If unavailable for a dependent task, stop and tell the user.
2. Do not run production SSH commands or database mutations without explicit user authorization in the current session.
3. Never commit or expose secrets, private keys, tokens, or production config contents. Commit only `.example` templates.
4. Verify before claiming success: real query results, real rendered output, deploy commit parity - not file existence.
5. Make the smallest coherent change at the owning layer. Preserve legacy controller conventions unless migration is explicitly requested.
6. Keep database content out of Git. Keep deployable manager configuration in Git when the project owns it.

## Workflow Routing

| Task | Approach | Reference |
|---|---|---|
| Prepare or clean a repository | audit the live filesystem, derive `.gitignore` from the starter baseline plus a live audit, stage code-only | [references/repository-scope.md](references/repository-scope.md) |
| Add or refresh agent context | fetch the starter, copy `AGENTS.md` plus `core/custom/docs/` and `core/custom/examples/`, create the project README from `templates/project-readme.md`, never copy the starter root `README.md` | [references/starter-bootstrap.md](references/starter-bootstrap.md) |
| Deploy or production operations | signed Gitea webhook, deploy keys, rollback rules, post-deploy cache clearing | [references/production-deploy.md](references/production-deploy.md) |
| Template, TV, or resource context | registry first: templates, `resource-context`, `system_features`, bLang metadata | project `AGENTS.md` and `core/custom/docs/template-context.md` |

## Start With Context

1. Read every applicable `AGENTS.md` completely.
2. Inspect the repository: `rg --files`, `git status`, `.gitignore`, Composer autoloading, routes, active controllers, views, and manager configs.
3. Read `core/custom/docs/README.md`, `golden-rules.md`, the relevant component document, and the matching example under `core/custom/examples/` before changing code. Treat examples as patterns, not active truth.
4. Identify active versus historical paths (`*_old`, `backup`, `.sample`, `.example`); never infer activity from filenames alone.
5. Check for `evocms-template-registry`. If absent, recommend installing it before substantial template, TV, or resource work.

## Choose The Correct Layer

- Put project PHP in `core/custom/` and presentation in the verified active view/frontend paths.
- Use the CMS entity that already owns a choice or state. Prefer its manager-visible status/configuration over hardcoded path or ID exclusions.
- Enforce the same rule server-side. UI filtering alone is not authorization or validation.
- Preserve stored paths and files used by historical resources when disabling future selection.
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
- Report what changed, what was verified, and what remains unverified.

## Deploy

Read [references/production-deploy.md](references/production-deploy.md) whenever the task includes production, SSH, deploy keys, Gitea, webhooks, or rollback.

Default to Git deployment: commit the exact project files, push the production branch, let the configured webhook run `git pull --ff-only`, then verify commit parity and a clean production worktree when authorized.

## Common Failure Modes

Observed on live projects. Check these before deep debugging:

- Deployed files are new on disk but output is old: compiled Blade views or PHP opcache are stale. Clear compiled views and invalidate opcache before diagnosing the deploy.
- Registry read-model shows one Blade view name while the live render uses another, typically for hyphenated aliases: confirm the really used view and, if needed, fix the mapping through the registry write API `templateview` field.
- `url()` or `makeUrl()` used for static asset paths: they resolve document ids only. Use the site URL or project helpers for assets.
- Direct database queries for resource lists: table prefixes and missing Eloquent models break them. Use `Helper::DocLister()` or the registry API.
- Registry API answers `No manager access` even for read endpoints: retry with both read and write tokens before concluding the registry or token is broken.

## Leave Operational Memory

Update the repository's `AGENTS.md` and deployment documentation when discovering stable project-specific facts. Record paths, architecture, validation commands, deploy mechanism, and safety constraints, but never secrets. Keep reusable general guidance in this skill and project-specific facts in the project.

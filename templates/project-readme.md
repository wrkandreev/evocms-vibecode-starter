# Project README Template

This template is filled per project during bootstrap. The starter root `README.md` describes the starter itself and is never copied into projects; this template is what a project gets instead.

Fill every placeholder with confirmed facts only. If a fact is not verified yet, leave the placeholder and ask - do not guess.

---

# <project-domain>

<One paragraph: what the site is, which Evolution CMS CE version, which template approach is in effect (registry + `Main` package with `TemplateController`, legacy controllers, or a 1.x to 3.x migration state).>

## Production Infrastructure

| | |
|---|---|
| Prod URL | `https://<domain>/` |
| Prod SSH | `<user@host>` |
| Web root | `<absolute path>` |
| Git remote | `<remote url>` |
| Production branch | `<branch>` |
| PHP on server | `<version and binary path, for example php8.3>` |
| Composer on server | `<exact command with working directory>` |

## Deploy

<Describe only the confirmed mechanism: signed Gitea webhook `POST /deploy`, token protected `deploy.php`, or manual `git pull`.>

- trigger: <what triggers deploy>
- post-deploy behavior: <which caches are cleared automatically by the endpoint>
- emergency path: <what is allowed when deploy is broken, and what requires explicit user authorization>

## Agent Entry Points

- agent rules and project facts: `AGENTS.md`
- reading order: `core/custom/docs/README.md`, then `core/custom/docs/golden-rules.md`, then the relevant subsystem document and matching example
- template registry: <installed / not installed; where generated context and API tokens live>

## Secrets

- real secrets: `core/custom/define.php` on production, never committed
- committed template: `core/custom/define.php.example`
- <other secret locations and files that must never be tracked>

## Validation

- <commands run before handoff, for example `php -l` on changed files, `git diff --check`>

## Starter Sync

Record the exact starter state this project was enriched from, and update it on every refresh. Missing or stale stamp means the copied docs may be outdated.

- upstream: https://github.com/wrkandreev/evocms-vibecode-starter
- branch: `main`
- fetched commit: `<commit sha>`
- fetched date: `<YYYY-MM-DD>`

## Notes

<Confirmed project facts discovered during work. Record only verified facts. Never record tokens, passwords, private keys, or other real secret values here.>

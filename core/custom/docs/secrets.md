# Secrets

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Evo projects often need a predictable place for custom secrets and environment specific constants.
- A practical standard is `core/custom/define.php`.

## Recommended Rule

- Store custom project secrets in `core/custom/define.php`.
- Keep the real file untracked.
- Commit only `core/custom/define.php.example` with placeholder values.

## Why This Is Useful

- `core/custom/` is usually the first place inspected in custom Evo projects.
- It keeps deploy tokens, SSH paths, API keys, and local constants out of template code.
- It avoids scattering secret values across controllers, views, and utility classes.
- It gives AI tooling a stable place to look for project level constants without putting real secrets into git.

## Typical Examples

- deploy web token
- SSH key path for deploy scripts
- known hosts path
- integration webhook secrets
- API credentials for external services

## What Must Be Verified On A Live Project

- whether the project already uses `core/custom/define.php`
- whether `.env` is also used and which source has priority
- whether bootstrap code loads `define.php` early enough for all consumers

## Safety Rules

- never commit real `core/custom/define.php`
- never copy real secrets into docs
- if secrets are already hardcoded in code, recommend moving them into `core/custom/define.php`

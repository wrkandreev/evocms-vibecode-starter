# Git Deploy Via Signed Gitea Webhook

> Important: this deploy workflow is custom project infrastructure, not a standard Evolution CMS CE feature.
> Use it only after verifying the live project's repository, branch, deploy path, and security configuration.

## Reference Pattern

```text
push to production branch
  -> Gitea POST /deploy
  -> verify X-Gitea-Event, repository full_name, exact ref, and HMAC of raw body
  -> acquire a non-blocking lock
  -> git pull --ff-only origin <branch>
  -> clear compiled Blade views and attempt opcache invalidation
```

The webhook secret belongs in ignored production configuration, never in Git, a URL, logs, or a response body. The deploy endpoint should return distinct responses for invalid signatures, ignored events or branches, lock contention, malformed payloads, and pull failures.

## Legacy Variants

Older projects may use a token-protected browser endpoint, manual `git pull`, or another deploy flow. Treat those as project-specific legacy variants, not as the default. Document and preserve them only after verifying that they are active.

## Documentation Goals

- record the webhook URL, repository full name, production ref, and deploy script path
- record secret location without recording its value
- note deploy-key, remote, and known-host assumptions
- document lock files, response codes, rollback, and cache clearing

## Recommended Secret Location

- Prefer storing deploy related secrets and local constants in `core/custom/define.php`.
- Commit `core/custom/define.php.example`, not the real file.
- If `.env` is also supported, document precedence clearly.

## Safety Rules

- do not expose real tokens or keys in repository docs
- do not run production SSH commands without explicit user request
- require `git pull --ff-only`; recover failed deploys by reverting a commit, never by force-pulling or `reset --hard`

## Post-Deploy Cache Behavior

- A deploy endpoint should clear compiled Blade views and attempt opcache invalidation for project PHP and views after a successful pull.
- On shared hosting `opcache_reset()` from web context may fail; per-file `opcache_invalidate()` is a partial mitigation.
- Document on the project which caches are cleared automatically and which must be verified manually.

## What Must Be Verified On A Live Project

- actual webhook endpoint and deploy script path
- secret loading and HMAC verification rules
- deploy-key and known-host requirements for the web-server user
- expected Gitea event, repository full name, and production ref
- lock behavior and post-deploy cache clearing

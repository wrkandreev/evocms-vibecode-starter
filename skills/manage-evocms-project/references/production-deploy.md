# Production And Gitea Deploy

## Authorization Boundary

- Treat a direct request to change or verify the live site as production authorization for that scoped task.
- Ask before expanding into raw SQL, destructive filesystem actions, force operations, credential repurposing, or unrelated production changes.
- Prefer read-only discovery before mutation. Resolve exact files, records, IDs, branch, repository, and path.

## Deploy Key

1. Generate a dedicated Ed25519 key on production with a descriptive filename.
2. Never print or copy the private key.
3. Give the user only the public key for repository configuration.
4. Configure a host alias with exact hostname, port, user, identity file, and `IdentitiesOnly yes`.
5. Test read access before setting the production `origin`.
6. Grant write access only if production genuinely needs to push.

## Signed Gitea Webhook Pattern

Use this pattern only after verifying it matches the reference project:

```text
push to production branch
  -> Gitea POST /deploy
  -> validate HMAC of raw body with X-Gitea-Signature
  -> validate X-Gitea-Event, repository full_name, and exact ref
  -> acquire a non-blocking lock
  -> git pull --ff-only origin <branch>
```

Keep the webhook secret in ignored production configuration. Never put it in Git, a URL, logs, or the payload. Return distinct responses for invalid signature, ignored events/branches, lock contention, malformed JSON, and pull failure.

Do not describe this as Gitea Actions unless a real workflow file and runner execute it.

## Post-Deploy Cache Clearing

After a successful `git pull --ff-only`, the endpoint should:

- clear compiled Blade views so templates regenerate on the next request;
- attempt opcache invalidation for project PHP and view files;
- keep these steps best-effort: invalidation failure must not fail the deploy response.

If deployed files are new on disk but behavior is old, verify compiled views and opcache before diagnosing the deploy itself.

## Normal Release

1. Validate code and inspect the diff.
2. Stage only intended project files.
3. Commit with a focused message.
4. Push the configured production branch.
5. Confirm `origin/<branch>` contains the commit.
6. Check webhook delivery response when accessible.
7. When production verification is authorized, confirm:
   - production `HEAD` equals the pushed commit;
   - `git status --porcelain` is empty;
   - required ignored files still exist;
   - the application-level behavior or CMS query reflects the requested result.

## Recovery

- Never replace a failed fast-forward with `reset --hard` or a force pull.
- Diagnose the webhook response, remote access, branch, and production worktree first.
- Roll back code by reverting the bad commit and pushing the revert.
- Back up material production content before any tracking transition or destructive operation.

# Code-Only Evolution CMS Repository And Gitignore

## Required Sources

Build `.gitignore` from the current versions of:

1. `https://github.com/wrkandreev/evocms-vibecode-starter/blob/main/.gitignore`
2. `core/custom/docs/gitignore.md` from that repository
3. `core/custom/docs/raw-project-custom-code.md` from that repository
4. an audit of the actual project and production filesystem

The starter baseline ignores machine files, editors, logs, secrets, dependency directories, caches, sessions, generated views, and import/export runtime. It intentionally does not declare every Evo core or upload directory disposable. Decide those from project policy and evidence.

## Form The Project Gitignore

1. Copy the current starter root `.gitignore` as the baseline.
2. Inventory the real project with `rg --files`, top-level directory inspection, active views/routes/autoloading, and authorized production checks.
3. Use `raw-project-custom-code.md` to classify project code versus Evo core, installed package internals, generated data, and historical copies.
4. Keep active project code:
   - `core/custom/` packages and configs;
   - active `views/`;
   - frontend directories referenced by active layouts;
   - project AJAX, cron, dashboards, integrations, and access logic;
   - project-owned ClientSettings, templatesedit, MultiTV, PageBuilder, Selector, Directory, and DocLister extension configs.
5. Add ignores for the verified installation policy:
   - Evo core/manager when maintained separately from project code;
   - actual user-upload directories when the repository is code-only;
   - caches, sessions, logs, generated thumbnails/views;
   - secrets and server-local config;
   - database dumps, deployment archives, backups, and historical copies.
6. Add negation rules where a broad ignore would otherwise hide project code, for example keeping `core/custom/` while excluding the rest of `core/`.
7. Explain non-obvious project-specific rules with comments in `.gitignore`.

Do not ignore all of `assets/`: Evo projects often store deployable manager configuration and active frontend code there. Do not ignore uploads by guess; identify exact directories and confirm the user wants code-only storage.

## Initial Import Order

1. Back up material server-only content.
2. Write `.gitignore` before staging the site.
3. Inspect ignored and untracked files with `git status --ignored`, `git check-ignore -v <representative-path>`, and targeted `find`/`rg` checks.
4. Stage explicitly and inspect `git diff --cached --name-only` plus large-file candidates.
5. Confirm secrets, uploads, caches, dumps, backups, Evo runtime, and deprecated copies are absent.
6. Confirm active PHP, views, frontend assets, and manager configs are present.
7. Commit and push only after this audit.

If unwanted paths are already tracked, update `.gitignore` and remove them from the Git index without deleting production files. Back up uploads first and verify they remain physically present after deployment.

## Verification Matrix

Check representative paths in both directions:

| Must be ignored | Must be tracked when active |
|---|---|
| real secret config | safe `.example` config |
| user uploads | project static assets |
| cache/session/log files | `core/custom/` code |
| Evo core maintained separately | project-owned Evo extensions |
| DB dumps and archives | manager-side project configs |
| historical copies | active views and frontend directories |

Deploy with `git pull`, not directory replacement, so ignored production content survives. After the first deployment, verify representative ignored files still exist.

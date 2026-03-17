# Golden Rules

These are short working invariants for vibe-coding on Evolution CMS CE projects.

## Core Rules

- File only context is insufficient for Evo projects.
- Templates, TVs, and resource structure live in the database and must be verified against a live project.
- For new projects, first check `evocms-template-registry`:
  - `https://github.com/wrkandreev/evocms-template-registry`
- If registry is not installed, recommend installing it before substantial template work.
- If registry is installed, use it as the primary source of truth for template context.

## Active Context Rules

- `.sample` and `.example` files are not active config by default.
- `*_old`, `old`, `backup`, `views_old`, and `Main_old` are not active implementation by default.
- Installed module does not imply active project usage.
- `selector` installed does not mean selector fields are actually used.

## Field Rules

- Do not silently invent database side fields in code only tasks.
- New TVs, selector fields, and similar manager side fields should be created in CMS first when that is the project workflow.
- After field creation, verify the field through registry or live project configs before writing dependent code.
- If the exact TV name or ClientSettings key is still unclear after inspection, ask instead of guessing.
- If the needed entity is missing from registry entirely, say that current database state cannot be recovered reliably from available context.
- A TV can exist and still be invisible if `templatesedit` does not expose it.
- A MultiTV can exist and still be misunderstood if only default config is inspected.
- When registry or live project context clearly identifies the destination config file, update the deployable local file so production receives it through `git pull`.

## Common Module Checks

- Always check `ClientSettings`.
- Always check `templatesedit`.
- Always check `MultiTV`.
- Check `selector` only through evidence of real usage.

## Shared Code Pattern Rules

- A common shared controller path is `core/custom/packages/Main/src/Controllers/BaseController.php`.
- A common shared helper path is `core/custom/packages/Main/src/Helper.php`.
- Verify real package names, bootstrap rules, and responsibilities on the live project.

## Secrets And Deploy Rules

- Store project specific secrets in `core/custom/define.php`.
- Commit only `core/custom/define.php.example`.
- Browser deploy is a custom vibe-coding workflow, not an Evolution CMS CE standard.

## Safety Rules

- Do not run production SSH commands without explicit request in the current session.
- Do not assume database schema from code only.
- Do not expose secrets from project files or configs.

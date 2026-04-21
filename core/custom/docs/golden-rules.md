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
- Do not treat `views/` as the full template implementation without checking connected frontend asset directories.
- Active views may depend on CSS, JS, images, fonts, or partial assets stored in `html/`, `assets/`, or another project directory.

## Field Rules

- Do not silently invent database side fields in code only tasks.
- New TVs, selector fields, and similar manager side fields should be created in CMS first when that is the project workflow.
- After field creation, verify the field through registry or live project configs before writing dependent code.
- When working with new TVs or manager side entities such as `ClientSettings`, always check live `evocms-template-registry` first.
- If registry does not contain the required entity details, or its API/output is unavailable, tell the user that the context must be fixed first.
- Do not proceed with guessed TVs, `ClientSettings` fields, or manager side bindings when registry context is missing, because the agent would be working blind.
- If the exact TV name or ClientSettings key is still unclear after inspection, ask instead of guessing.
- If the needed entity is missing from registry entirely, say that current database state cannot be recovered reliably from available context.
- A TV can exist and still be invisible if `templatesedit` does not expose it.
- A MultiTV can exist and still be misunderstood if only default config is inspected.
- Do not normalize MultiTV TV names automatically; use the exact TV key from live registry output, including dashes.
- `ClientSettings` runtime keys are prefixed with `client_`, so manager field names should not already start with `client_`.
- If a live project already created `ClientSettings` fields with the `client_` prefix, expect broken runtime keys like `client_client_phone` and treat them as legacy compatibility, not as the target pattern.
- When registry or live project context clearly identifies the destination config file, update the deployable local file so production receives it through `git pull`.

## Template Change Rules

- Before changing a template, use live `evocms-template-registry` as the primary source of truth.
- Verify template alias, controller and view mapping, attached TVs, and `ClientSettings` `setting_name` values separately.
- Verify where the active view loads its frontend assets from before deciding which files belong to the template implementation.

## Common Module Checks

- Always check `ClientSettings`.
- Always check `templatesedit`.
- Always check `MultiTV`.
- Check `selector` only through evidence of real usage.
- If the project is multilingual, verify the real localization layer such as `bLang`, active suffix values, and translated manager fields before adding `_en` style keys.
- For `bLang`, treat `blang_tmplvars` and registry `blang` context as the source of truth, not manually paired `_en` TVs by name alone.
- If registry supports API creation for TVs or dictionary entries, prefer that path so metadata and real entities remain synchronized.

## Shared Code Pattern Rules

- A common shared controller path is `core/custom/packages/Main/src/Controllers/BaseController.php`.
- A common shared helper path is `core/custom/packages/Main/src/Helper.php`.
- Evo `3.1.30` offers a newer template controller and `DLTemplate` view style, but repository examples intentionally stay on the more widely used pre-`3.1.30` `TemplateController` baseline for now.
- Do not assume Laravel helper semantics in Evo CE helper code; `url()` is for document ids through `makeUrl`, not string asset paths.
- Verify real package names, bootstrap rules, and responsibilities on the live project.

## Secrets And Deploy Rules

- Store project specific secrets in `core/custom/define.php`.
- Commit only `core/custom/define.php.example`.
- Browser deploy is a custom vibe-coding workflow, not an Evolution CMS CE standard.

## Safety Rules

- Do not run production SSH commands without explicit request in the current session.
- Do not assume database schema from code only.
- Do not expose secrets from project files or configs.

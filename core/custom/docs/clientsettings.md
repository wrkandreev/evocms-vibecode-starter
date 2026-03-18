# ClientSettings

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- `ClientSettings` is a common way to store project wide settings.
- These settings may provide text, links, images, resource ids, selector values, or MultiTV like structures.
- Templates and controllers may read them directly or through helper wrappers.
- Registry may expose `system_features.client_settings` plus diagnostic details as the first detection layer.

## Typical Responsibilities

- global site text and labels
- contact data
- shared blocks for the homepage or footer
- links to resources chosen in manager

## Important Clarification

- `.sample` and `.example` files near `ClientSettings` configs are reference files, not active settings by default.
- Active project behavior must be verified against the real config files and the live manager state.

## Verified Runtime Naming Pattern

- Real project evidence confirms that `ClientSettings` builds runtime keys by prefixing the manager field key with `client_`.
- In `xspb.ru`, manager field keys are stored without the prefix, for example `phone`, `email`, `main_examples`, and runtime access uses keys such as `client_phone`, `client_email`, `client_main_examples`.
- This is the preferred pattern for new and cleaned up projects.

## Naming Rule

- Create manager field names without the `client_` prefix.
- Expected runtime key format is `client_` + field name.
- Example: `phone` becomes `client_phone`.
- Example: `company_name` becomes `client_company_name`.
- Do not create field names that already start with `client_`.

## Legacy Misconfiguration Rule

- Real project evidence also shows a broken pattern: in `evo.omniagency.ru`, manager field keys were created as `client_phone` and `client_company_name`.
- In that case runtime access becomes duplicated: `client_client_phone`, `client_client_company_name`.
- Before changing code, verify the actual manager field key or registry `setting_name` on the live project.
- If the live project already uses the broken naming, code may need temporary compatibility handling for both keys during cleanup.
- Treat support for `client_client_*` as a legacy compatibility measure, not as the recommended default.

## Registry Driven Update Rule

- When registry or verified live project context clearly identifies the target `ClientSettings` tab or config file, update that local deployable file in the repository.
- The goal is that the changed config lands on production through normal `git pull` and appears in manager in the right place.
- If the exact key name, tab, or destination file is still unclear, ask before creating a new config entry.
- If the expected `ClientSettings` entity is missing from registry, report that the agent cannot restore the real database state from current context and ask the user to verify registry or live project state.

## What Must Be Verified On A Live Project

- whether `ClientSettings` is installed
- where config files are located
- field naming conventions
- whether manager field names incorrectly include the `client_` prefix
- whether live registry shows the real `setting_name` as unprefixed or already broken with `client_`
- how values are accessed in code and templates
- whether fields contain plain values, documents, or structured data
- which config files are real and which are only samples

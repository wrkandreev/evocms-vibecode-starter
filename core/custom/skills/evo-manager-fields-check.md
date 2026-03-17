# Evo Manager Fields Check

## Purpose

Use this skill when a task touches TVs, MultiTV, ClientSettings, selector fields, or manager form layout.

## When To Use

- adding or changing TVs
- debugging missing fields in manager
- working with `ClientSettings`
- working with `MultiTV`
- working with `templatesedit`
- checking selector based resource pickers

## Main Goal

Confirm that the field exists and is really wired into the live project before changing dependent code.

## Required Checks

1. Confirm whether the field already exists on the live project.
2. If registry is installed, use it for template and TV context.
3. If the field is missing and project workflow expects CMS side creation, ask the owner to create it first.
4. Verify whether the field is attached to the expected template.
5. Verify manager visibility through `templatesedit` config.
6. For `MultiTV`, verify that the important TV has its own dedicated config.
7. For `ClientSettings`, distinguish active config files from `.sample` and `.example` files.
8. For selector fields, confirm real usage through field definitions, controller files, and consuming code.

## Working Rules

- A field can exist in database context but still be effectively invisible if `templatesedit` does not expose it.
- A `MultiTV` can exist but still be unclear if only default config is inspected.
- `ClientSettings` and selector values may store not only plain strings, but also documents or structured data.

## Output Format

Return a short validation summary with:
- field existence status
- active config files involved
- manager visibility status
- related helper/controller/view dependencies
- missing live-project confirmations

## Do Not Do

- do not invent new DB side fields silently
- do not trust sample/example configs as active
- do not assume selector is active because the module exists

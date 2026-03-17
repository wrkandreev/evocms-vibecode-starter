# Evo Controller Pattern Review

## Purpose

Use this skill when you need to understand how a project structures shared logic, page logic, and view data preparation.

## When To Use

- before editing `BaseController`
- before adding a new page controller
- when debugging where shared page data comes from
- when tracing cache, menus, breadcrumbs, or TV parsing

## Main Goal

Identify the real split between shared controller logic, helper layer logic, and template specific logic.

## Required Checks

1. Inspect whether the project has a shared controller such as `core/custom/packages/Main/src/Controllers/BaseController.php`.
2. Inspect whether the project has a shared helper such as `core/custom/packages/Main/src/Helper.php`.
3. Identify how the base controller is bootstrapped:
   - `process()`
   - constructor
   - custom lifecycle method
4. Identify what shared data is collected there:
   - SEO
   - breadcrumbs
   - menus
   - user data
   - MultiTV parsing
   - ClientSettings
   - cache wrappers
5. Inspect one or two page controllers to see what is really page specific.
6. Check whether helper methods wrap `DocLister`, `DLMenu`, TV parsing, galleries, selector based lookups, or query presets.

## Working Rules

- Keep repeated Evo access patterns in shared helper or shared controller layers.
- Keep template specific filtering and request parsing in page controllers when it is not reused elsewhere.
- Do not assume naming conventions are universal; verify real file names and namespaces.

## Output Format

Return a short review with:
- shared controller path and role
- shared helper path and role
- example page controller pattern
- cache responsibilities
- project specific deviations from the common pattern

## Helpful References

- `core/custom/examples/helper.md`
- `core/custom/examples/base-controller.md`
- `core/custom/examples/page-controller.md`
- `core/custom/examples/filtered-list-controller.md`

# Cache

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Cache frequently affects diagnostics in Evo projects.
- Incorrect conclusions are common when template data, menus, or TV derived output are cached.

## Common Cache Sources

- page cache
- DocLister and DLMenu cache
- helper layer cache
- Laravel cache wrappers such as `Cache::rememberForever`
- generated template registry artifacts that are out of date
- compiled Blade views under the project compiled views path
- PHP opcache on shared hosting, which can serve stale bytecode after a `git pull`

## Post-Deploy Caches

A successful `git pull` does not guarantee new behavior on shared hosting:

- PHP opcache may keep executing the old bytecode of controllers and views.
- Compiled Blade views may stay stale until cleared.

Symptom pattern: the repository and the on-disk files are already new, but HTML still contains old markup or old debug markers. Check compiled Blade views and opcache before concluding that the deploy failed.

## Repeated Project Pattern

- cache keys are often composed inside `BaseController` and `core/custom/packages/Main/src/Helper.php`
- shared helper methods often cache DocLister, DLMenu, TV, gallery, or derived resource data
- when investigating stale output, inspect both page controllers and shared helper wrappers

## Practical Rule

- If code and output do not match, verify cache state before assuming implementation is wrong.

## What Must Be Verified On A Live Project

- which cache layers are active
- where cache keys are composed
- what operations already clear cache
- whether stale registry output is causing confusion

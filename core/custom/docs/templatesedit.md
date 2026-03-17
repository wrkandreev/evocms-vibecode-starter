# templatesedit

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- `templatesedit` is a common manager side customization layer.
- It can control tabs, groups, field ordering, and TV placement in the resource edit form.
- Registry may expose `system_features.templatesedit` plus file based diagnostics as the first detection layer.

## Strong Working Rule

- A TV may exist and be bound correctly, but still be effectively missing for editors if `templatesedit` config does not expose it in the expected place.
- `.example` files in `templatesedit` config directories are not active layout definitions by default.

## What Must Be Verified On A Live Project

- whether `templatesedit` is installed
- where its config files are stored
- how template specific configs override defaults
- whether a changed or newly added TV is visible in manager where editors expect it
- which files are active configs and which files are only examples

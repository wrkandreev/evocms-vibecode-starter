# Troubleshooting

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Common Situations

### A TV exists in code assumptions but not in real project

- verify the field was actually created in CMS
- verify registry is fresh
- verify the TV is bound to the expected template

### A TV exists but editors do not see it in manager

- verify `templatesedit` config
- verify template binding and manager grouping

### MultiTV data shape is unclear

- verify dedicated MultiTV config
- verify field captions and stored keys on the live project

### Selected resources look wrong

- verify selector controller mapping
- verify selector filters and allowed template ids

### View or controller mapping looks inconsistent

- verify registry output if installed
- verify template level controller configuration in database
- verify whether fallback conventions are in use

### Code changed but output did not

Check the chain in order and stop at the first stale layer:

1. deployed commit reached `origin/<branch>` and webhook delivery returned success
2. the file on the production disk contains the change (`grep` for a new marker)
3. compiled Blade views are regenerated (clear compiled views if needed)
4. PHP opcache is not serving old bytecode (invalidate per file, or restart PHP where available)
5. page cache, DocLister or DLMenu cache, and helper layer cache are refreshed
6. generated registry artifacts are fresh
7. browser cache is bypassed (hard reload or `curl`)

### Page shows a bare `Error` or a blank 200

- Evo renders a short `Error` output for users without a manager session; a logged-in manager user usually sees the full PHP backtrace.
- Fastest diagnosis: log into the manager and reload the page.
- Alternative: temporarily enable `display_errors` at the top of the view, then remove it and restore the file before commit.

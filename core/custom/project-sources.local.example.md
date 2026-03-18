# Local Project Sources Template

Copy this file to `core/custom/project-sources.local.md` and keep real project paths only in the local file.

## Suggested Statuses

- `preferred` - current project with good examples for repeated patterns
- `reference-only` - useful for narrow cases, but not a default source
- `avoid` - not safe to use as a pattern source

## Preferred

### /absolute/path/to/project
- Status: preferred
- Good for: controllers, `ClientSettings`, `MultiTV`, selectors
- Notes: current project, registry available, patterns verified

## Reference Only

### /absolute/path/to/project
- Status: reference-only
- Good for: historical pattern or one subsystem
- Avoid for: fresh Evo 3 defaults

## Avoid

### /absolute/path/to/project
- Status: avoid
- Reason: stale code, broken naming, missing registry, or incomplete source

# 0007 Views And External Frontend Assets Are One Implementation Unit

- Status: accepted
- Date: 2026-03-24

## Context

In Evolution CMS CE projects, filesystem views rarely contain the full template implementation by themselves. Active views often load CSS, JS, images, fonts, partial assets, or build output from directories outside `views/`, for example `html/`, `assets/`, or another project specific frontend folder. If the agent inspects only `views/`, it may misunderstand which files actually belong to the active template implementation and may omit necessary project source from git rules or repository analysis.

## Decision

Treat active views and their connected frontend asset directories as one implementation unit. When a view depends on assets outside `views/`, those directories must be included in template analysis and treated as active project source.

## Consequences

- `views/` must not be treated as a complete template boundary without checking linked frontend assets
- project analysis should verify where active views load CSS, JS, images, fonts, partial assets, and build output from
- directories such as `html/` may need to be kept in git because they are part of the active template implementation
- repository examples and git rules should account for frontend asset directories outside `views/`

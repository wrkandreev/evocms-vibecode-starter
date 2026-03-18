# Controller Pattern Audit

- Status: active
- Date: 2026-03-18

## Goal

Collect a cleaner, current example of how controllers should be structured for fresh Evolution CMS CE projects without carrying over transitional patterns from older controller migrations.

## Context

- Current controller examples are usable, but the `BaseController` pattern still looks partially influenced by older migration style code.
- In particular, storing everything in `$this->data` may not be the best default example for fresh projects that start directly with newer controller patterns.
- We want examples that reflect current real projects, not only article based transition patterns.

## Constraints

- Use live project evidence, not only framework articles.
- Prefer active projects with confirmed Evo 3 controller usage.
- Distinguish fresh project defaults from legacy compatibility patterns.
- Do not rewrite examples until several current projects are compared.

## Steps

1. Select several active projects that use current controller patterns.
2. Inspect how `BaseController` is structured in each project.
3. Inspect how page specific controllers pass data to views.
4. Compare whether projects use `$this->data`, direct `addViewData()`, helper methods, traits, or other patterns.
5. Separate true common patterns from one off project style.
6. Draft a recommended fresh project controller example.
7. Update `core/custom/examples/base-controller.md` and related examples only after the comparison is complete.

## Risks

- article or migration examples may bias the pattern toward transitional code
- old projects may still contain historical controller styles that should not become the new default
- filesystem code alone may still miss some runtime conventions if the live project state is not verified

## Decision Log

- 2026-03-18: treat current `BaseController` example as provisional and schedule a live project audit before declaring it the preferred fresh project pattern.
- 2026-03-18: reviewed live controller patterns in several current and legacy reference projects.
- 2026-03-18: confirmed that a shared `$data` accumulator is still common in real projects, but should be documented as a common pattern rather than the only valid fresh project shape.
- 2026-03-18: adopted `TemplateController` plus `process()` as the preferred fresh project baseline and expanded the local examples library with more controller examples.

## Findings

- One current project uses a large `TemplateController` based `BaseController` with `process()`, `setCommonData()`, `setPageData()`, helper methods, traits, and shared `$this->data`.
- Another current project uses a simpler `TemplateController` based `BaseController` that still keeps a shared `$this->data` property but has a much smaller common layer.
- A legacy reference project preserves the older constructor plus `sendToView()` pattern, which is useful as legacy reference but not as the default for fresh Evo 3 guidance.
- A better repository baseline is to show both the preferred fresh pattern and the common real project pattern, then add focused examples for MultiTV pages, selector fallbacks, and search/list pages.

## Verification

- at least several current projects are reviewed
- the resulting example is justified by repeated live patterns
- the new example clearly distinguishes fresh project guidance from legacy migration techniques
- local controller examples were expanded after the audit

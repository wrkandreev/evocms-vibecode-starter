# ClientSettings

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- `ClientSettings` is a common way to store project wide settings.
- These settings may provide text, links, images, resource ids, selector values, or MultiTV like structures.
- Templates and controllers may read them directly or through helper wrappers.

## Typical Responsibilities

- global site text and labels
- contact data
- shared blocks for the homepage or footer
- links to resources chosen in manager

## Important Clarification

- `.sample` and `.example` files near `ClientSettings` configs are reference files, not active settings by default.
- Active project behavior must be verified against the real config files and the live manager state.

## What Must Be Verified On A Live Project

- whether `ClientSettings` is installed
- where config files are located
- field naming conventions
- how values are accessed in code and templates
- whether fields contain plain values, documents, or structured data
- which config files are real and which are only samples

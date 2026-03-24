# Local Examples And Official Doc Links

- Status: active
- Date: 2026-03-18

## Goal

Reduce hallucinated or internet first implementation guesses by keeping common Evolution CMS CE patterns and third party package references inside this repository.

## Context

- The agent often needs concrete examples for manager side configs such as `MultiTV` field definitions.
- When the repository does not contain enough examples, the model may start searching the internet and mix together unofficial or low confidence patterns.
- This repository already acts as a local operational memory layer, so it should also keep practical examples for frequently used Evo modules.
- When a local example is missing, the repository should still provide a single clear path to the official documentation source for the exact package or subsystem.

## Constraints

- Prefer local examples for common and repeated project tasks.
- Do not rely on random blog posts or forum fragments as the primary source.
- For each third party snippet, plugin, or module, prefer an official GitHub repository or official package documentation page.
- Links should be specific enough to guide the agent to the right subsystem, not just the vendor home page.

## Steps

1. List the most common Evo modules and patterns that agents repeatedly need examples for.
2. Add local example coverage for those patterns in `core/custom/examples/`.
3. Start with high frequency cases such as `MultiTV` configs, including patterns like multiselect fields.
4. Build a small reference index of official documentation links for third party snippets, plugins, and modules used in typical Evo projects.
5. Store those links in an easy to scan local document so the agent can prefer them over open ended internet search.
6. Define a rule: first look for a local example in this repo, and if none exists, use the official GitHub documentation link for that exact tool.
7. Continue expanding the local example library with more real project examples for helpers, controllers, traits, and manager side configs.

## Risks

- examples may become stale if they are not tied back to real package documentation
- a vague documentation index may still send the agent into broad web search
- unofficial examples may accidentally be treated as canonical if official links are missing

## Decision Log

- 2026-03-18: add backlog item to expand local example coverage for common Evo patterns and keep official documentation links for third party tools when local examples are missing.
- 2026-03-24: raw project snapshots under `~/projects/raw/` confirmed stable custom code locations for `core/custom/packages/Main/src/`, `views/`, linked `html/` frontend directories, `ClientSettings` configs, `MultiTV` configs, and selector controllers.
- 2026-03-24: local examples were expanded with raw-project-backed examples for `MultiTV`, `ClientSettings`, selectors, frontend asset structure, helper patterns, and traits.
- 2026-03-24: repository examples intentionally remain on the established `TemplateController` baseline instead of the newer Evo `3.1.30` controller and `DLTemplate` style until more live projects confirm that shift.

## Verification

- common recurring tasks have local examples in the repository
- `MultiTV` examples cover non-trivial cases such as multiselect style fields
- third party tools used in typical Evo projects have clear official documentation links stored locally
- the agent can prefer local examples first and official package docs second without broad internet search

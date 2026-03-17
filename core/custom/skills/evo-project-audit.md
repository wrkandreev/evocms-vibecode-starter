# Evo Project Audit

## Purpose

Use this skill when starting work on an Evolution CMS CE project and you need a fast but safe project audit before implementation.

## When To Use

- first contact with a project
- before changing templates, TVs, or controllers
- before trusting file structure as the full source of truth
- before working on a live project over SSH

## Main Goal

Build a reliable mental model of the project without assuming that files alone describe the whole system.

## Required Checks

1. Read project level `AGENTS.md` and `core/custom/docs/` if present.
2. Check whether `evocms-template-registry` is installed.
3. If registry is not installed, recommend installing it before substantial template work.
4. Inspect `core/custom/`, especially `core/custom/packages/`.
5. Identify whether the project uses `core/custom/packages/Main/src/Controllers/BaseController.php` and `core/custom/packages/Main/src/Helper.php` or equivalent files.
6. Check whether these modules are installed and actually used:
   - `ClientSettings`
   - `templatesedit`
   - `MultiTV`
   - `selector`
7. Inspect whether the project contains extra layers such as `ajax/`, `cron/`, dashboards, integrations, or access logic.
8. Distinguish active files from historical/reference ones.

## Active Vs Historical Rule

- Do not trust `*_old`, `old`, `backup`, `views_old`, `Main_old`, `.sample`, or `.example` as active source of truth by default.
- Confirm active implementation through real runtime paths, active configs, and live project behavior.

## Output Format

Return a short audit summary with:
- active package and controller pattern
- registry status
- installed modules and whether they appear used
- key project layers beyond templates
- uncertainty list that must be verified before coding

## Do Not Do

- do not assume database side fields from code only
- do not treat installed selector module as proof of selector usage
- do not treat sample configs as active configs
- do not run destructive production commands

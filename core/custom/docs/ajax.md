# AJAX

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Evo projects often include AJAX handlers outside the normal template rendering flow.
- Common entry points are `ajax/`, root level `ajax.php`, custom routes, or package level controllers.
- AJAX behavior may depend on the same shared helpers, TVs, ClientSettings, and access checks as normal page controllers.

## Typical Responsibilities

- form submission handlers
- filtered list loading
- search and autocomplete
- calendar or schedule loading
- personal account actions
- integration callbacks and lightweight endpoints

## Working Rule

- Treat AJAX as a separate subsystem, not as a minor detail of templates.
- Verify input validation, response format, side effects, and access checks before changing frontend calls.
- If AJAX returns template derived data, still verify TVs, helper methods, and manager config involved in building that response.

## What Must Be Verified On A Live Project

- where AJAX entry points live
- whether requests go through Evo bootstrap or a custom lightweight bootstrap
- which controllers, scripts, or snippets process requests
- whether endpoints depend on session state or manager auth
- what output format is expected: HTML, JSON, redirects, or snippet output
- whether cache or rate limiting affects observed behavior

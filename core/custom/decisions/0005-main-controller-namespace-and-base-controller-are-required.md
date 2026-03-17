# 0005 Main Controller Namespace And BaseController Are Required

- Status: accepted
- Date: 2026-03-17

## Context

On fresh Evolution CMS CE projects, controller resolution depends on the configured `ControllerNamespace`. If that namespace is not set to the actual custom package namespace, Evo will not resolve template controllers from the intended package. In the common `Main` package bootstrap, projects also need a shared fallback `BaseController` so controller based rendering has a predictable base layer from the start.

## Decision

For fresh projects that bootstrap custom code in package `Main`, immediately set `ControllerNamespace` in `core/custom/config/cms/settings.php` to `EvolutionCMS\\Main\\Controllers\\` and immediately create `core/custom/packages/Main/src/Controllers/BaseController.php` in namespace `EvolutionCMS\Main\Controllers`.

## Consequences

- fresh install documentation must treat `ControllerNamespace` setup as a required bootstrap step, not an optional improvement
- fresh install documentation must treat `BaseController` creation as part of the initial package bootstrap
- agents should not assume template controllers will work on a fresh project until namespace and base controller bootstrap is in place
- if a project uses a package name other than `Main`, the namespace must be adjusted to match the real package namespace exactly

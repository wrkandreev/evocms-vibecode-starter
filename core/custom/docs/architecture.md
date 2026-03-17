# Architecture

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Evo projects often keep a major part of structure in the database, not only in code.
- Custom application logic usually lives under `core/custom/`.
- Template rendering often uses controllers plus `views/`, but exact bootstrap differs by project.
- A project may contain more than page rendering: AJAX endpoints, cron, dashboards, integrations, access rules, deploy scripts.

## Typical Layers

- resource tree and templates in the database
- TVs and TV bindings
- manager side modules and form configuration
- custom package code in `core/custom/`
- a common package pattern is `core/custom/packages/Main/`
- a common shared utility entry point is `core/custom/packages/Main/src/Helper.php`
- a common shared controller entry point is `core/custom/packages/Main/src/Controllers/BaseController.php`
- views in `views/`
- helper classes wrapping Evo data access and snippets

## Repeated Project Pattern

- `BaseController` often gathers shared view data such as menus, SEO fields, breadcrumbs, current user data, common resource relations, and cache backed lookups.
- Page specific controllers usually extend that base class and add only template specific data in a dedicated method.
- `Helper.php` often becomes the main wrapper around Evo APIs and snippet calls such as DocLister, DLMenu, TV access, MultiTV parsing, galleries, and cache friendly data extraction.

## Legacy And Parallel Structures

- Real projects may contain directories such as `views_old`, `Main_old`, `backup`, or older package copies.
- These paths are useful as historical reference, but they are not the active source of truth unless runtime wiring confirms it.
- Always identify which package, view path, and bootstrap path are active now.

## Reusable Examples

- `core/custom/examples/helper.md`
- `core/custom/examples/base-controller.md`
- `core/custom/examples/page-controller.md`
- `core/custom/examples/filtered-list-controller.md`
- `core/custom/examples/ajax-handler.md`
- `core/custom/examples/selector-controller.md`
- `core/custom/examples/clientsettings-usage.md`
- `core/custom/examples/clientsettings-config.md`
- `core/custom/examples/templatesedit-config.md`
- `core/custom/examples/multitv-config.md`
- `core/custom/examples/multitv-tv-specific-config.md`

## What Must Be Verified On A Live Project

- project entry points and controller bootstrap
- where business logic actually lives
- whether there is a package structure under `core/custom/packages/`
- whether the project actually uses a `Main` package name or another package name
- whether `Helper.php` is a real shared service layer or only a thin utility file
- whether the project uses Blade, plain PHP templates, or mixed rendering
- whether extra app layers exist such as dashboard, AJAX, cron, integrations
- whether old or backup directories are still referenced anywhere in runtime code

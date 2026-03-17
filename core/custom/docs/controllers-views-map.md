# Controllers And Views Map

> Important: this document describes a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Confirmed Pattern

- Many Evo projects use a base controller plus page specific controllers.
- Views often live in `views/` and focus mostly on rendering.
- Shared data preparation is commonly centralized in a base controller or helper layer.
- A repeated convention is `core/custom/packages/Main/src/Controllers/BaseController.php` plus template specific controllers in the same directory.
- A repeated convention is a shared helper at `core/custom/packages/Main/src/Helper.php` that wraps DocLister, DLMenu, TV, MultiTV, galleries, and cache aware data access.

## Preferred New Controller Approach

- For new Evo `3.x` projects, prefer controllers based on `EvolutionCMS\TemplateController`.
- Prefer `process()` as the main execution method.
- Prefer `addViewData()` for passing prepared data into views.
- Treat older patterns based on constructor side effects or `sendToView()` as legacy approach to be migrated when appropriate.

## Typical Responsibilities

- `BaseController` collects common data for all or most pages
- page controllers extend `BaseController` and add only page specific queries
- `Helper.php` hides low level Evo APIs and snippet details behind reusable methods
- views mostly render already prepared data

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

## Provisional Pattern

- template aliases may map to controller classes by naming convention
- views may follow alias based naming such as `views/<alias>.blade.php`
- some projects keep manual template maps when no registry is installed

## What Must Be Verified On A Live Project

- how a template chooses a controller
- whether controllers extend `TemplateController` or a custom base class
- how data is passed into views
- exact path and naming rules for views
- whether some templates intentionally use fallback or placeholder views
- whether `BaseController` performs setup in `process()`, constructor, or another bootstrap method
- whether `Helper.php` is static, instance based, trait based, or split into multiple helper classes

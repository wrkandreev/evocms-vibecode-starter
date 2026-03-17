# Fresh Install

> Important: this document describes the preferred vibe-coding bootstrap workflow for a fresh Evolution CMS CE project.
> Before applying it, verify server specifics, PHP tooling, and hosting restrictions on the live project.

## Recommended Starting Point

- connect to the server over SSH
- use `https://github.com/evocms-community/installer`
- use the installer CLI mode when available
- install a fresh Evolution CMS CE `3.x` version

## Why This Workflow

- it gives a predictable and repeatable starting point
- it avoids manual unpacking and mixed install paths
- it matches the Evo 3 project conventions used in current vibe-coding workflows

## Base Install Flow

1. connect to the server over SSH
2. place `install.php` from `evocms-community/installer` into the target web root
3. run installer in CLI mode if the environment allows it
4. choose a fresh `3.x` version
5. finish normal Evo installation and verify manager access

## After Core Install

The preferred next step is to bootstrap the standard custom package layer:

```bash
cd /path/to/project/core
php artisan package:create Main
php composer.phar update
```

After package creation, explicitly configure the controller namespace in:

`core/custom/config/cms/settings.php`

```php
<?php

return [
    'ControllerNamespace' => 'EvolutionCMS\\Main\\Controllers\\',
];
```

This must match the package name and controller path. For package `Main`, controllers are expected under:

- `core/custom/packages/Main/src/Controllers/`
- namespace `EvolutionCMS\Main\Controllers`

Then create the base fallback controller:

`core/custom/packages/Main/src/Controllers/BaseController.php`

```php
<?php

namespace EvolutionCMS\Main\Controllers;

class BaseController
{
    public function __construct()
    {
    }
}
```

## Main Package Rule

- Installing package `Main` is the common development paradigm for Evo 3 projects in this workflow.
- New project specific code is expected to grow from `core/custom/packages/Main/`.
- `ControllerNamespace` should be set immediately after package creation, before adding template specific controllers.
- `BaseController` should exist as the shared fallback controller for templates that do not yet have their own controller.

## Template Registry Step

- After the base package is ready, install `evocms-template-registry`.
- For new projects, it is the preferred standard for restoring template and TV context.

## Hosting Note

- On Timeweb, use `composer2` instead of the default composer command when required by the hosting environment.

## Controller Rule

- For new projects, use the new template controller approach introduced in Evo `3.1.26+`.
- Reference: `https://community.evocms.ru/blog/docs/perehod-na-novye-kontrollery-shablonov.html`
- Prefer `process()` and `addViewData()` over older controller patterns such as constructor driven template setup or `sendToView()`.
- Still ensure `ControllerNamespace` is configured correctly in `core/custom/config/cms/settings.php`.
- Still bootstrap `BaseController` in `core/custom/packages/Main/src/Controllers/BaseController.php` as part of the base package setup.

## What Must Be Verified On A Live Project

- whether installer CLI mode is available in that environment
- exact install path and document root
- PHP version and required extensions such as `curl` and `zip`
- whether `php composer.phar update` or `composer2 update` is the correct command on that host
- whether the server already has existing Evo files or conflicting paths
- whether `ControllerNamespace` points to the actual package namespace used by the project

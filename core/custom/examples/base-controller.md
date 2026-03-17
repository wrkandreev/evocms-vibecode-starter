# Example BaseController

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- `core/custom/packages/Main/src/Controllers/BaseController.php` is a repeated project pattern.
- It usually centralizes shared page data and reduces duplication across template controllers.
- On a fresh project, `BaseController` should be created immediately after package bootstrap and `ControllerNamespace` setup.

## Fresh Install Bootstrap

- package: `Main`
- controller path: `core/custom/packages/Main/src/Controllers/`
- namespace in controllers: `EvolutionCMS\Main\Controllers`
- CMS setting in `core/custom/config/cms/settings.php`: `ControllerNamespace => 'EvolutionCMS\\Main\\Controllers\\'`

Minimal bootstrap version:

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

This fallback controller is useful even before the project moves to richer `TemplateController` based implementations.

## Typical Responsibilities

- detect current document id
- collect shared SEO fields
- collect menus and breadcrumbs
- collect shared ClientSettings values
- parse MultiTV values from current document
- pass prepared data to the view

## Example Shape

```php
<?php

namespace EvolutionCMS\Main\Controllers;

use EvolutionCMS\Main\Helper;
use EvolutionCMS\TemplateController;

class BaseController extends TemplateController
{
    protected array $data = [];
    protected int $docid;

    public function process(): void
    {
        $this->docid = evo()->documentIdentifier;

        $this->setCommonData();
        $this->setPageData();

        $this->addViewData($this->data);
    }

    protected function setCommonData(): void
    {
        $this->data['titl'] = evo_parser(evo()->documentObject['titl'][1] ?? '');
        $this->data['desc'] = evo_parser(evo()->documentObject['desc'][1] ?? '');
        $this->data['mainMenu'] = Helper::DLMenu('MainMenu', ['parents' => 0]);
        $this->data['crumbs'] = evo()->runSnippet('DLCrumbs', ['showCurrent' => 1]);

        $this->processMultiTVs();
    }

    protected function setPageData(): void
    {
    }

    protected function processMultiTVs(): void
    {
        foreach (evo()->documentObject as $key => $value) {
            if (is_array($value) && (($value[4] ?? null) === 'custom_tv:multitv')) {
                $this->data[$key] = Helper::convertMTVToArray($value[1] ?? '');
            }
        }
    }
}
```

## Working Rule

- keep shared page setup here
- keep page specific business logic in child controllers
- avoid turning `BaseController` into an unstructured dump of unrelated logic

## Verify On Live Project

- whether setup runs in `process()`, constructor, or custom bootstrap
- whether controller extends `TemplateController` or a custom base class
- whether shared data belongs here or in traits/services

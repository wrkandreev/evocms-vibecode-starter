# Example BaseController

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- `core/custom/packages/Main/src/Controllers/BaseController.php` is a repeated project pattern.
- It usually centralizes shared page data and reduces duplication across template controllers.
- On a fresh project, `BaseController` should be created immediately after package bootstrap and `ControllerNamespace` setup.
- Live project audit across several current Evo projects shows that a shared data accumulator is still a common controller pattern.

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

## Live Pattern Findings

- One current project uses `TemplateController`, `process()`, `setCommonData()`, `setPageData()`, and a shared `$data` property passed through `addViewData()`.
- Another current project uses the same overall shape in a simpler form: base document fields and config values are prepared in `process()`, then page specific logic extends it.
- A legacy reference project shows the older constructor plus `sendToView()` pattern, which is useful as historical reference but should not be the default for fresh Evo 3 projects.
- For fresh projects, `TemplateController` plus `process()` is the preferred baseline, even though `$this->data` remains a common implementation detail in real projects.

## Typical Responsibilities

- detect current document id
- collect shared SEO fields
- collect menus and breadcrumbs
- collect shared ClientSettings values
- parse MultiTV values from current document
- pass prepared data to the view

## Recommended Fresh Project Shape

```php
<?php

namespace EvolutionCMS\Main\Controllers;

use EvolutionCMS\TemplateController;

class BaseController extends TemplateController
{
    protected int $docid = 0;

    public function process(): void
    {
        $this->docid = (int) evo()->documentIdentifier;

        $viewData = array_merge(
            $this->baseViewData(),
            $this->pageViewData()
        );

        $this->addViewData($viewData);
    }

    protected function baseViewData(): array
    {
        return [
            'pagetitle' => $this->documentField('pagetitle'),
            'introtext' => $this->documentField('introtext'),
        ];
    }

    protected function pageViewData(): array
    {
        return [];
    }

    protected function documentField(string $key): string
    {
        $value = evo()->documentObject[$key] ?? '';

        if (is_array($value)) {
            $value = $value[1] ?? $value[0] ?? '';
        }

        return trim((string) $value);
    }
}
```

## Common Real Project Shape

```php
<?php

namespace EvolutionCMS\Main\Controllers;

use EvolutionCMS\TemplateController;

class BaseController extends TemplateController
{
    protected array $data = [];
    protected int $docid = 0;

    public function process(): void
    {
        $this->docid = (int) evo()->documentIdentifier;

        $this->setCommonData();
        $this->setPageData();

        $this->addViewData($this->data);
    }

    protected function setCommonData(): void
    {
        $this->data['pagetitle'] = $this->documentField('pagetitle');
        $this->data['introtext'] = $this->documentField('introtext');
    }

    protected function setPageData(): void
    {
    }

    protected function documentField(string $key): string
    {
        $value = evo()->documentObject[$key] ?? '';

        if (is_array($value)) {
            $value = $value[1] ?? $value[0] ?? '';
        }

        return trim((string) $value);
    }
}
```

## Working Rule

- Prefer `TemplateController` plus `process()` for fresh projects.
- A shared `$this->data` property is acceptable because it is common in real projects, but it is not the only valid shape.
- Keep shared page setup here.
- Keep page specific business logic in child controllers.
- Avoid turning `BaseController` into an unstructured dump of unrelated logic.

## Verify On Live Project

- whether setup runs in `process()`, constructor, or custom bootstrap
- whether controller extends `TemplateController` or a custom base class
- whether shared data belongs here or in traits/services
- whether the project benefits more from array returning methods than from mutating `$this->data`

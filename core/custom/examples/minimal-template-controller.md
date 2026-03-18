# Example Minimal TemplateController

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- A current lightweight project pattern shows a useful minimal shape for fresh projects.
- It keeps the base controller small and avoids mixing too much business logic into the shared layer.

## Example Shape

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

        $this->addViewData(array_merge(
            $this->baseViewData(),
            $this->pageViewData()
        ));
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

## Working Rule

- Use this shape when the project is new and shared controller logic is still small.
- Prefer array returning methods when they make the controller easier to read.
- Add helper methods only after repeated access patterns appear.

## Verify On Live Project

- whether the project already has a stronger shared helper layer
- whether common view data should include menus, crumbs, or settings
- whether page controllers should return arrays or mutate a shared property

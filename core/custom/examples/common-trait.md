# Example Common Trait

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- Raw projects repeatedly use traits such as `Common`, `Redirects`, and `User` to keep controller code smaller.
- A small trait example helps the agent avoid pushing every shared method into `BaseController`.

## Example Shape

```php
<?php

namespace EvolutionCMS\Main\Traits;

use EvolutionCMS\Main\Helper;

trait Common
{
    protected function getDirections(): array
    {
        $params = [
            'parents' => 6,
            'orderBy' => 'menuindex ASC',
            'makeUrl' => 0,
        ];

        return Helper::DocLister('AllDirections', $params);
    }

    protected function getRelatedNews(): array
    {
        return $this->getDocumentsFromSelector('related_news', ['tvList' => 'image']);
    }
}
```

## Working Rule

- Move repeated project specific methods into traits when they are shared across multiple controllers.
- Keep the trait focused on one concern set such as content lookups, redirects, or user helpers.
- Do not use traits as a dump for unrelated logic.

## Verify On Live Project

- whether the project already uses traits
- whether the logic belongs in a trait, `Helper.php`, or `BaseController`
- whether trait methods depend on controller properties like `$this->docid` or `$this->data`

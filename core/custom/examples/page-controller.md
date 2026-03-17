# Example Page Controller

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- Many Evo projects follow the same split: shared logic in `BaseController`, page logic in a template specific controller.
- Keeping a good example makes it easier to generate new controllers consistently.

## Example Shape

```php
<?php

namespace EvolutionCMS\Main\Controllers;

use EvolutionCMS\Main\Helper;

class NewsListController extends BaseController
{
    protected function setPageData(): void
    {
        $this->data['news'] = Helper::DocLister('NewsList_' . ($_GET['page'] ?? 1), [
            'parents' => 10,
            'display' => 12,
            'paginate' => 'pages',
            'tpl' => '@CODE:<li>[+pagetitle+]</li>',
        ]);

        $this->data['pages'] = evo()->getPlaceholder('pages');
    }
}
```

## Working Rule

- page controller should add only data that is specific to one template or one page type
- repeated query logic should move into `Helper.php` or another shared layer when reused
- view should receive prepared data rather than rebuild query logic inside Blade

## Verify On Live Project

- whether pagination placeholders are handled in helper or controller
- whether template id, parents, TVs, and filters match the real database state
- whether a page specific controller is really needed or existing shared logic is enough

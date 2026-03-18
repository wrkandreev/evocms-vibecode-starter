# Example MultiTV Page Controller

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- `evo.omniagency.ru` uses a clean page controller that reads one MultiTV field and normalizes it for the view.
- This is a common pattern for landing pages and portfolio style blocks.

## Example Shape

```php
<?php

namespace EvolutionCMS\Main\Controllers;

class HomeController extends BaseController
{
    protected function setPageData(): void
    {
        $portfolioItems = array_values(array_filter(array_map(function ($item) {
            if (!is_array($item)) {
                return null;
            }

            $title = trim((string) ($item['title'] ?? ''));
            $image = trim((string) ($item['image'] ?? ''));
            $type = trim((string) ($item['type'] ?? ''));

            if ($title === '') {
                return null;
            }

            return [
                'title' => $title,
                'image' => $image,
                'type' => $type !== '' ? $type : 'residential',
            ];
        }, $this->getMultiTvValue('portfolio-item'))));

        $this->data['portfolioItems'] = $portfolioItems;
        $this->data['portfolioCount'] = count($portfolioItems);
    }
}
```

## Working Rule

- Read the exact TV name from live registry first, including dashes.
- Normalize MultiTV rows in the controller before they reach Blade.
- Filter out incomplete rows early so the view can stay simple.

## Verify On Live Project

- whether the TV key really contains dashes or underscores
- whether MultiTV data is JSON `fieldValue` or already normalized by a helper
- whether value mapping belongs in the controller or in a shared helper

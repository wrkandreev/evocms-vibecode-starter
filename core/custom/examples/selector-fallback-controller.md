# Example Selector Fallback Controller

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- A current project pattern uses a useful approach where a page tries template bound selector TVs first and falls back to `ClientSettings` defaults.
- This helps editors override content per page without losing global defaults.

## Example Shape

```php
<?php

namespace EvolutionCMS\Main\Controllers;

use EvolutionCMS\Main\Helper;

class BlogItemController extends BaseController
{
    protected function setPageData(): void
    {
        $this->data['mainExamples'] = $this->getSelectorDocumentsOrDefault('blog_examples', 'client_main_examples', [
            'tvList' => Helper::examplesTvList(),
            'prepare' => '\\EvolutionCMS\\Main\\Helper::prepareExamples',
        ]);

        $this->data['mainNews'] = $this->getSelectorDocumentsOrDefault('blog_news', 'client_main_news', [
            'tvList' => 'image',
            'urls' => 1,
        ]);
    }

    protected function getSelectorDocumentsOrDefault(string $tvName, string $settingsKey, array $params = []): array
    {
        $params = array_merge(['urls' => 1], $params);
        $items = $this->getDocumentsFromSelector($tvName, $params);

        if (!empty($items)) {
            return $items;
        }

        $ids = (string) evo()->getConfig($settingsKey);

        if ($ids === '') {
            return [];
        }

        return $this->getDocumentsFromIds($ids, $params);
    }
}
```

## Working Rule

- Use this pattern when a page may override a global default selection.
- Verify both the selector TV name and the `ClientSettings` key through live registry before coding.
- Keep the fallback helper in the controller or base controller only when the pattern repeats.

## Verify On Live Project

- whether selector fields are really attached to the template
- whether the fallback key exists in `ClientSettings`
- whether result preparation belongs in `Helper.php` or stays page specific

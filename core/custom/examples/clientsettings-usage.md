# Example ClientSettings Usage

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- `ClientSettings` is a repeated source of global project data.
- Controllers and views often consume it directly or through helper wrappers.

## Controller Side Example

```php
<?php

namespace EvolutionCMS\Main\Controllers;

use EvolutionCMS\Main\Helper;

class HomeController extends BaseController
{
    protected function setPageData(): void
    {
        $this->data['mainExamples'] = Helper::getDocumentsFromIds((string) evo()->getConfig('client_main_examples'), [
            'tvList' => 'image',
            'display' => 8,
        ]);
    }
}
```

## View Side Example

```blade
<a href="tel:@config('client_phone')">@config('client_phone')</a>

<img src="@config('client_logo')" alt="{{ $titl }}">
```

## Working Rule

- Use `ClientSettings` for project wide data, not page specific business fields.
- Wrap complex settings access in helper methods when the value needs parsing or resource lookup.
- Verify whether the setting stores plain text, ids, selector values, or MultiTV data.

## Verify On Live Project

- exact config key names
- whether values are available through `@config(...)`, `evo()->getConfig(...)`, or custom wrappers
- whether selected documents need additional lookup before rendering

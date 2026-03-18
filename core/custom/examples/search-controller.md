# Example Search Controller

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- A current project pattern uses a practical search controller that combines request parsing, DocLister filters, and pagination.
- Search pages are easy to overcomplicate, so a reference pattern is useful.

## Example Shape

```php
<?php

namespace EvolutionCMS\Main\Controllers;

use EvolutionCMS\Main\Helper;

class SearchController extends BaseController
{
    protected function setPageData(): void
    {
        $this->data['searchResults'] = $this->makeSearch();
    }

    protected function makeSearch(): array
    {
        $search = trim(evo()->stripTags((string) ($_GET['search'] ?? '')));

        if ($search === '') {
            return [];
        }

        $params = [
            'idType' => 'documents',
            'ignoreEmpty' => 1,
            'orderBy' => 'menuindex ASC',
            'tvList' => Helper::examplesTvList(),
            'display' => 6,
            'paginate' => 'pages',
            'config' => 'paginate:custom',
            'urls' => 1,
            'filters' => 'OR(content:pagetitle:like:' . $search . ';content:longtitle:like:' . $search . ')',
        ];

        $rows = Helper::DocLister(false, $params);
        $this->data['pages'] = $this->getPages();

        return $rows;
    }
}
```

## Working Rule

- Strip and normalize request input before building filters.
- Disable static cache keys when the query depends on request input.
- Collect pagination placeholders in the same flow as the search query.

## Verify On Live Project

- whether search also checks TVs, related entities, or selector documents
- whether pagination and filtering are safe with the current cache layer
- whether a dedicated search helper already exists

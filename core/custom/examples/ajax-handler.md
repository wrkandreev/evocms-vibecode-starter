# Example AJAX Handler

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- Many Evo projects have AJAX entry points outside normal template rendering.
- A stable example helps separate request validation, access checks, and response shape.

## Example Shape

```php
<?php

use EvolutionCMS\Main\Helper;

require_once __DIR__ . '/../index.php';

$page = max(1, (int) ($_GET['page'] ?? 1));
$types = array_filter(array_map('intval', explode(',', (string) ($_GET['types'] ?? ''))));

$params = [
    'parents' => 5,
    'display' => 6,
    'paginate' => 'pages',
    'config' => 'paginate:custom',
    'tvList' => Helper::newsTvList(),
];

if (!empty($types)) {
    $params['filters'] = 'AND(tv:sportTag:in:' . implode(',', $types) . ')';
}

$params['currentPage'] = $page;

$rows = Helper::DocLister(false, $params);

header('Content-Type: application/json; charset=UTF-8');
echo json_encode([
    'ok' => true,
    'items' => $rows,
    'pages' => evo()->getPlaceholder('pages'),
], JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES);
```

## Working Rule

- Keep AJAX endpoints explicit about input, output, and side effects.
- Reuse shared helpers instead of duplicating Evo query logic.
- Verify whether the endpoint should return JSON, HTML fragments, or redirects.

## Verify On Live Project

- actual AJAX entry path and bootstrap rule
- required auth or session checks
- expected response format
- whether rate limiting, cache, or CSRF checks apply

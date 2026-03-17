# Example Helper

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- `core/custom/packages/Main/src/Helper.php` appears in many Evo projects.
- It is often the main shared layer between controllers and low level Evo APIs.
- Keeping a generic example helps vibe-coding stay consistent across projects.

## Typical Responsibilities

- wrappers for `DocLister` and `DLMenu`
- TV and MultiTV extraction
- `ClientSettings` value access and parsing
- gallery helpers
- document lookup helpers
- cache aware data access
- data normalization before passing into views

## Example Shape

```php
<?php

namespace EvolutionCMS\Main;

use Illuminate\Support\Facades\Cache;

class Helper
{
    public static function DocLister(string $key, array $params = [])
    {
        return Cache::rememberForever($key, function () use ($params) {
            return evo()->runSnippet('DocLister', $params);
        });
    }

    public static function DLMenu(string $key, array $params = [])
    {
        return Cache::rememberForever($key, function () use ($params) {
            return evo()->runSnippet('DLMenu', $params);
        });
    }

    public static function getMTV(string $name, int $docid): array
    {
        $raw = evo()->getTemplateVarOutput($name, $docid)[$name] ?? '';

        return self::convertMTVToArray($raw);
    }

    public static function clientConfig(string $key, $default = '')
    {
        $value = evo()->getConfig($key);

        return $value !== null && $value !== '' ? $value : $default;
    }

    public static function clientMTV(string $key, $display = 'all'): array
    {
        $decoded = json_decode((string) evo()->getConfig($key), true);
        $fieldValue = $decoded['fieldValue'] ?? [];

        if (!is_array($fieldValue)) {
            return [];
        }

        return $display === 'all' ? $fieldValue : array_slice($fieldValue, 0, (int) $display);
    }

    public static function convertMTVToArray($value): array
    {
        if (empty($value)) {
            return [];
        }

        $decoded = json_decode($value, true);

        return is_array($decoded['fieldValue'] ?? null) ? $decoded['fieldValue'] : [];
    }
}
```

## Working Rule

- Prefer putting repeated Evo access patterns here instead of duplicating them in every controller.
- Do not move project specific business rules into `Helper.php` if they belong to one page only.

## Related Examples

- For `ClientSettings` manager config structure, use `core/custom/examples/clientsettings-config.md`.
- For `MultiTV` manager config structure, use `core/custom/examples/multitv-config.md` and `core/custom/examples/multitv-tv-specific-config.md`.
- For `templatesedit`, do not invent layout rules in helper code; use `core/custom/examples/templatesedit-config.md`.
- If the exact TV name, `ClientSettings` key, tab, or config file is unclear, ask instead of guessing.

## Verify On Live Project

- whether `Helper.php` is static or instance based
- whether it returns arrays, collections, or raw snippet output
- whether caching is handled here or inside controllers

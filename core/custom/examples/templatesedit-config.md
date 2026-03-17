# Example templatesedit Config

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- `templatesedit` often decides whether editors can actually see and use TVs.
- A compact example helps explain template specific layout overrides.

## Example Shape

```php
<?php

return [
    'caption' => 'Homepage',
    'tabs' => [
        'main' => [
            'caption' => 'Main',
            'fields' => [
                'pagetitle',
                'longtitle',
                'introtext',
            ],
            'tvs' => [
                'main_slider',
                'main_features',
                'main_cta',
            ],
        ],
        'seo' => [
            'caption' => 'SEO',
            'tvs' => [
                'titl',
                'desc',
                'keyw',
            ],
        ],
    ],
];
```

## Working Rule

- When a TV is missing in manager, inspect `templatesedit` before assuming the binding is wrong.
- Treat `.example` configs as reference only until a real active config is confirmed.
- Verify template specific overrides before changing shared default layout.

## Verify On Live Project

- actual config directory and file naming
- whether layout is per template, per role, or both
- which config file is truly active for the target template

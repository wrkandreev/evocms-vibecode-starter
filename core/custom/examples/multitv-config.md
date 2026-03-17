# Example MultiTV Config

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- A dedicated MultiTV config is often the only reliable description of field shape for a TV.
- This example helps explain why default config is not enough for project specific TVs.

## Example Shape

```php
<?php

$settings['display'] = 'vertical';
$settings['fields'] = [
    'title' => [
        'caption' => 'Title',
        'type' => 'text',
    ],
    'image' => [
        'caption' => 'Image',
        'type' => 'image',
    ],
    'link' => [
        'caption' => 'Link',
        'type' => 'text',
    ],
    'is_highlighted' => [
        'caption' => 'Highlighted',
        'type' => 'checkbox',
    ],
];
```

## Working Rule

- Use a dedicated config for important TVs with custom structure.
- Match field names to the keys really consumed by controllers and views.
- If captions are generic or confusing in manager, inspect the TV specific config first.

## Verify On Live Project

- active config file for the target TV
- real stored keys in database output
- helper side transformations before rendering

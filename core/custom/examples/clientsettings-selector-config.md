# Example ClientSettings Selector Config

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- Raw projects show a repeated pattern where `ClientSettings` mixes plain fields, `customtv:multitv`, and `customtv:selector` fields in the same tab.
- This is a useful real project example for homepage blocks and global curated content.

## Example Shape

```php
<?php

return [
    'caption' => 'Content',
    'settings' => [
        'main_news_title' => [
            'caption' => 'Main news title',
            'type' => 'text',
        ],
        'main_news' => [
            'caption' => 'Main news',
            'type' => 'customtv:selector',
        ],
        'main_examples' => [
            'caption' => 'Main examples',
            'type' => 'customtv:selector',
        ],
        'main_collage' => [
            'caption' => 'Main collage',
            'type' => 'customtv:multitv',
        ],
    ],
];
```

## Working Rule

- Keep manager field names unprefixed, for example `main_examples`, not `client_main_examples`.
- Use `customtv:selector` when editors should attach curated documents.
- Use `customtv:multitv` when editors should manage structured repeated content directly in `ClientSettings`.

## Verify On Live Project

- exact tab file
- exact field key
- matching selector controller file for selector fields
- matching MultiTV config file for `customtv:multitv` fields

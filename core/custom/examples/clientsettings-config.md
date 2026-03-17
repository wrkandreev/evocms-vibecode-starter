# Example ClientSettings Config

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- `ClientSettings` config files define what appears in manager.
- The file itself is often the deployable source that must be updated locally and delivered to production through `git pull`.

## Example Shape

```php
<?php

return [
    'caption' => 'Common',
    'settings' => [
        'mail_for_feedback' => [
            'caption' => 'Email for feedback',
            'type' => 'text',
        ],
        'logo' => [
            'caption' => 'Logo',
            'type' => 'image',
        ],
        'phone' => [
            'caption' => 'Phone',
            'type' => 'text',
        ],
        'email' => [
            'caption' => 'Email',
            'type' => 'text',
        ],
    ],
];
```

## Working Rule

- If registry or verified live project context clearly shows the target tab file, update that local config file in the repository.
- The goal is that production receives the changed manager config through normal `git pull`.
- If the exact key name, tab, or destination file is still unclear, ask instead of guessing.

## Verify On Live Project

- active tab file such as `tab10.php`
- real key naming convention already used on the project
- whether the field type is plain text, image, selector, or `customtv:multitv`
- whether the target file is active or only a `.sample`

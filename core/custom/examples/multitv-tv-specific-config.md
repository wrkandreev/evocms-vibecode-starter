# Example TV Specific MultiTV Config Naming

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- In real projects the important part is often not only field shape, but which exact TV name owns the config file.
- A TV specific config such as `projectEvents.config.inc.php` is much more meaningful than falling back to `default.config.inc.php`.

## Example Shape

```php
<?php

$settings['display'] = 'vertical';
$settings['fields'] = [
    'thumb' => [
        'caption' => 'thumb',
        'type' => 'thumb',
        'thumbof' => 'image',
    ],
    'image' => [
        'caption' => 'Photo',
        'type' => 'image',
    ],
    'image_mob' => [
        'caption' => 'Mobile photo',
        'type' => 'image',
    ],
    'video_link' => [
        'caption' => 'Video',
        'type' => 'text',
    ],
    'title' => [
        'caption' => 'Title',
        'type' => 'text',
    ],
    'text' => [
        'caption' => 'Text',
        'type' => 'textareamini',
    ],
];
```

## Working Rule

- Prefer the TV specific config file when registry or live project context clearly identifies it.
- Treat `default.config.inc.php` as fallback, not as proof that it is the right config for the target TV.
- If the exact TV name is still unclear, ask before creating a new `*.config.inc.php` file.

## Verify On Live Project

- the exact TV name from registry or verified manager context
- the expected config filename convention for that project
- whether a TV specific config already exists and just needs updating
- whether manager captions match the fields consumed in code

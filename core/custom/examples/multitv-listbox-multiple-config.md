# Example MultiTV Listbox Multiple Config

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- Raw project snapshots include a working `MultiTV` config that demonstrates `listbox-multiple`.
- This is a common case that the agent often guesses badly when no local example exists.

## Example Shape

```php
<?php

$settings['display'] = 'vertical';
$settings['fields'] = [
    'title' => [
        'caption' => 'Title',
        'type' => 'text',
    ],
    'tags' => [
        'caption' => 'Tags',
        'type' => 'listbox-multiple',
        'elements' => 'Orange||Apple||Strawberry',
    ],
    'is_visible' => [
        'caption' => 'Visible',
        'type' => 'checkbox',
        'elements' => 'Yes==1||No==0',
    ],
];
```

## Working Rule

- Use `listbox-multiple` when one MultiTV row should store multiple selected values.
- Keep `elements` explicit in the config, or generate them from a confirmed data source.
- Confirm how selected values are stored and consumed before writing controller logic.

## Source Pattern

- Raw projects contain `multidemo.config.inc.php` examples with `listbox-multiple`, `listbox`, `dropdown`, `checkbox`, and `option` field types.

## Verify On Live Project

- exact TV config file name
- actual `elements` source
- how the selected values are parsed in helper or controller code

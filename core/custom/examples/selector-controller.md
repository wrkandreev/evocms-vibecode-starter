# Example Selector Controller

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- Selector controllers are a repeated pattern in Evo projects that use `customtv:selector`.
- They encode which resources may be attached to a field in manager.

## Typical Responsibilities

- bind one selector field name to one controller file
- limit allowed resources by template, parent, or custom filters
- return predictable manager results for document picking

## Example Shape

```php
<?php

class related_newsController extends \\selectorController
{
    public function __construct($modx, $params = [])
    {
        parent::__construct($modx, $params);

        $this->addWhereList('c.template in (15) and c.deleted=0');
    }
}
```

## Working Rule

- Keep selector rules narrow and explicit.
- Prefer filtering by real template ids or other confirmed resource rules.
- If editor choices are wrong, inspect selector controller before changing frontend code.

## Verify On Live Project

- exact base class name
- exact controller file naming convention
- field name to controller mapping rule
- allowed template ids and resource filters

# Blade Templates

> Important: this document describes behavior observed on a live Evolution CMS CE 3.x project.
> Before relying on it elsewhere, verify the real Blade behavior on the live project.

## Shared Variable Scope

In the EVO 3 Blade implementation observed on live projects, `@section('content')` shares PHP scope with the layout and with every `@include`-ed partial such as header and footer.

Consequences:

- a variable created or overwritten inside a section becomes visible in the layout and in partials;
- a short generic loop variable name can silently overwrite layout or controller data;
- `@php` fallback blocks must clean up their temporary variables.

## Variable Naming Rules

- Never use names that the layout, header, or `BaseController` also uses as loop variables inside `@section`. Prefix loop variables with something page specific, for example `$dCity` instead of `$city`.
- Before introducing a new view variable, check which names the active layout and partials already use.
- In `@php` fallback blocks use `$_`-prefixed temporary variables and `unset()` them at the end of the block.

## View-Level Data Fallback

On hosting where opcache can serve stale controller bytecode, a view can defend itself against a controller that did not pass data:

```php
@php
$data = $data ?? [];
if (empty($data)) {
    // recompute the same data the controller would pass
}
@endphp
```

Read the current document id from `evo()->documentObject['id']` or `evo()->documentIdentifier`, not from controller state that stale bytecode may not have set.

## What Must Be Verified On A Live Project

- whether the project Blade version really shares scope between sections, layout, and partials
- which variable names the active layout and partials reserve
- where compiled views live and whether deploy clears them
